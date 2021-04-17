---
layout: post
title:  "CosLog project dev log #1"
date:   2021-04-12 12:25:08 +0000
categories: OOP, Kotlin, exceptions
--- 

## How to execute logic only when the fragment is loaded, resisting configuration change and process death?

A cache is shared between two fragments but it needs to be cleared only when the app enters any one of those fragments. It is tempting to put the method inside `onViewCreated()` or `onResume()` but they will be called when the app rotates and I didn't want that. This one got solved by checking if there is a saved instance:

{% highlight kotlin %}
// Will get executed only when the app has entered the fragment
// resisting rotation and process death
if (savedInstanceState == null) {
    viewModel.clearPendingUriCache()
}
{% endhighlight %}

## Two activities onSupportNavigateUp() doesn't navigate to first from second

Suppose Activity A and B with two separate navigation graphs. Hooking the back button to take us from B to A is easy

{% highlight kotlin %}
class ActivityB : AppCompatActivity() {
   override fun onCreate(savedInstanceState: Bundle?) {
        ...
        supportActionBar?.setDisplayHomeAsUpEnabled(true)
    }
}
{% endhighlight %}

The problem here is that any fragment in Activity B is hooked to take us back to activity A and that's about it. We have some fragments in Activity B that need to navigate back to previously opened fragments in the same activity so we used

{% highlight kotlin %}
class ActivityB : AppCompatActivity() {
    override fun onSupportNavigateUp(): Boolean {
        return myController.navigateUp()
    }
}
{% endhighlight %}

With the addition of the code above we can now navigate flawlessly back to any fragment in Activity B but now can't move back to Activity B using the back button because myController doesn't remember where it came from. There are a few ways to overcome this problem: override `NavController.OnDestinationChangedListener` and navigate from there, or override `onOptionsItemSelected()` in your activity for every fragment.

## FragmentStateAdapter is not updating the fragments when deleted.

Sounds a bit weird right? 

Naively, I thought that when you try to remove an item from FragmentStateAdapter, it will update it properly, but sadly that is not the case. 

Imagine an image gallery with images A, B, C and you try to delete B and call `notifyItemRemoved(position)`; what do you think it would happen? The image gallery will become: A, C.

What?

Yes that's right. By default the FragmentStateAdapter will not update your fragments when you restructure it and I believe it has something to do with optimization.  I decided to play with the debugger and found that the code in `ensureFragment()` inside the `if (!mFragments.containsKey(itemId))` block would not be executed if you pass the default id which is your data position.

{% highlight kotlin %}
private void ensureFragment(int position) {
    long itemId = getItemId(position);

    if (!mFragments.containsKey(itemId)) {
        // with the default implementation of getItemId() this 
        // if block cannot be reached when deleting an item

        // TODO(133419201): check if a Fragment provided here is a new Fragment
        Fragment newFragment = createFragment(position);
        newFragment.setInitialSavedState(mSavedStates.get(itemId));
        mFragments.put(itemId, newFragment);
    }
}
{% endhighlight %}

I didn't have time to swim through 10000+ lines of RecyclerView code but I noticed this: if you pass the current position in `getItemId()` and you make a structural change, e.g. delete an item, the new item on its place would still display the deleted stuff which might mean that there must be a cache somewhere that holds the old data. To overcome this problem you should pass a <i>stable</i> id like a hash code or something else that doesn't change.

{% highlight kotlin %}

class ImagePagerAdapter(
    fragment: Fragment,
    private val _data: MutableList<Pair<String, Int>>
) : FragmentStateAdapter(fragment) {

    val data: List<Pair<String, Int>> get() = _data

    fun removeAt(position: Int) {
        _data.removeAt(position)
        notifyItemRemoved(position)
    }

    override fun getItemCount(): Int = _data.size

    // super.getItemId(position) always returns 
    // 0 and won't update your fragment
    override fun getItemId(position: Int): Long {
        return _data[position].hashCode().toLong()
    }
}
{% endhighlight %}
