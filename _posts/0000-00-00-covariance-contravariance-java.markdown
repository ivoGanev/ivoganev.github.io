---
layout: post
title:  "Covariance and contravariance in Java"
date:   2021-05-13 12:25:08 +0000
categories: OOP, Kotlin, exceptions
published: false
---

PECS (Producer extends and Consumer super)

mnemonic → Get and Put principle.

This principle states that:

- Use an extends wildcard when you only get values out of a structure.
- Use a super wildcard when you only put values into a structure.
- And don’t use a wildcard when you both get and put.

Example in Java:

``` java
class Super {
        Number testCoVariance() {
            return null;
        }
        void testContraVariance(Number parameter) {
        }
    }

    class Sub extends Super {
        @Override
        Integer testCoVariance() {
            return null;
        } //compiles successfully i.e. return type is don't care(Integer is subtype of Number)
        @Override
        void testContraVariance(Integer parameter) {
        } //doesn't support even though Integer is subtype of Number
    }
}
```
The Liskov Substitution Principle (LSP) states that “objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program”.

Within the type system of a programming language, a typing rule

- <b>covariant</b> if it preserves the ordering of types (≤), which orders types from more specific to more generic;
- <b>contravariant</b> if it reverses this ordering;
- <b>invariant or nonvariant</b> if neither of these applies.
Covariance and contravariance

- Read-only data types (sources) can be covariant;
- write-only data types (sinks) can be contravariant.
- Mutable data types which act as both sources and sinks should be invariant.

To illustrate this general phenomenon, consider the array type. For the type Animal we can make the type Animal[]

- <b>covariant</b>: a Cat[] is an Animal[];
- <b>contravariant</b>: an Animal[] is a Cat[];
- <b>invariant</b>: an Animal[] is not a Cat[] and a Cat[] is not an Animal[].

Java Examples:

``` java
Object name= new String("prem"); //works
List<Number> numbers = new ArrayList<Integer>();//gets compile time error

Integer[] myInts = {1,2,3,4};
Number[] myNumber = myInts;
myNumber[0] = 3.14; //attempt of heap pollution i.e. at
// runtime gets java.lang.ArrayStoreException:
// java.lang.Double(we can fool compiler but not run-time)

List<String> list=new ArrayList<>();
list.add("prem");
List<Object> listObject=list; //Type mismatch: cannot convert
                              //from List<String> to List<Object> at Compiletime
```


bounded(i.e. heading toward somewhere) wildcard : There are 3 different flavours of wildcards:

- In-variance/Non-variance: ? or ? extends Object - <b>Unbounded</b> Wildcard. It stands for the family of all types. Use when you both get and put.
- Co-variance: ? extends T (the family of all types that are subtypes of T) - a wildcard with an <b>upper bound</b>. T is the <b>upper</b>-most class in the inheritance hierarchy. Use an extends wildcard when you only <b>Get</b> values out of a structure.
- Contra-variance: ? super T ( the family of all types that are supertypes of T) - a wildcard with a <b>lower bound</b>. T is the <b>lower</b>-most class in the inheritance hierarchy. Use a super wildcard when you only <b>Put</b> values into a structure.
Note: wildcard ? means zero or one time, represents an unknown type. The wildcard can be used as the type of a parameter, never used as a type argument for a generic method invocation, a generic class instance creation.(i.e. when used wildcard that reference not used in elsewhere in program like we use T)


{:refdef: style="text-align: center;"}
![PECS](/assets/ki1Kf.jpg)
{: refdef}

``` java
import java.util.ArrayList;
import java.util.List;

class Shape { void draw() {}}

class Circle extends Shape {void draw() {}}

class Square extends Shape {void draw() {}}

class Rectangle extends Shape {void draw() {}}

public class Test {

    public static void main(String[] args) {
        //? extends Shape i.e. can use any sub type of Shape, here Shape is Upper Bound in inheritance hierarchy
        List<? extends Shape> intList5 = new ArrayList<Shape>();
        List<? extends Shape> intList6 = new ArrayList<Cricle>();
        List<? extends Shape> intList7 = new ArrayList<Rectangle>();
        List<? extends Shape> intList9 = new ArrayList<Object>();//ERROR.


        //? super Shape i.e. can use any super type of Shape, here Shape is Lower Bound in inheritance hierarchy
        List<? super Shape> inList5 = new ArrayList<Shape>();
        List<? super Shape> inList6 = new ArrayList<Object>();
        List<? super Shape> inList7 = new ArrayList<Circle>(); //ERROR.

        //-----------------------------------------------------------
        Circle circle = new Circle();
        Shape shape = circle; // OK. Circle IS-A Shape

        List<Circle> circles = new ArrayList<>();
        List<Shape> shapes = circles; // ERROR. List<Circle> is not subtype of List<Shape> even when Circle IS-A Shape

        List<? extends Circle> circles2 = new ArrayList<>();
        List<? extends Shape> shapes2 = circles2; // OK. List<? extends Circle> is subtype of List<? extends Shape>

        //-----------------------------------------------------------
        Shape shape2 = new Shape();
        Circle circle2= (Circle) shape2; // OK. with type casting

        List<Shape> shapes3 = new ArrayList<>();
        List<Circle> circles3 = shapes3; //ERROR. List<Circle> is not subtype of  List<Shape> even Circle is subetype of Shape

        List<? super Shape> shapes4 = new ArrayList<>();
        List<? super Circle> circles4 = shapes4; //OK.
    }

    /*
     * Example for an upper bound wildcard (Get values i.e Producer `extends`)
     *
     * */
    public void testCoVariance(List<? extends Shape> list) {
        list.add(new Object());//ERROR
        list.add(new Shape()); //ERROR
        list.add(new Circle()); // ERROR
        list.add(new Square()); // ERROR
        list.add(new Rectangle()); // ERROR
        Shape shape= list.get(0);//OK so list act as produces only
    /*
     * You can't add a Shape,Circle,Square,Rectangle to a List<? extends Shape>
     * You can get an object and know that it will be an Shape
     */
    }

    /*
     * Example for  a lower bound wildcard (Put values i.e Consumer`super`)
     * */
    public void testContraVariance(List<? super Shape> list) {
        list.add(new Object());//ERROR
        list.add(new Shape());//OK
        list.add(new Circle());//OK
        list.add(new Square());//OK
        list.add(new Rectangle());//OK
        Shape shape= list.get(0); // ERROR. Type mismatch, so list acts only as consumer
        Object object= list.get(0); //OK gets an object, but we don't know what kind of Object it is.
        /*
         * You can add a Shape,Circle,Square,Rectangle to a List<? super Shape>
         * You can't get an Shape(but can get Object) and don't know what kind of Shape it is.
         */
    }
}

```

generics and examples

Covariance and contravariance determine compatibility based on types. In either case, variance is a directed relation. Covariance can be translated as "<b>different in the same direction</b>," or with-different, whereas contravariance means "<b>different in the opposite direction,</b>" or against-different. Covariant and contravariant types are not the same, but there is a correlation between them. The names imply the direction of the

- Covariance: accept subtypes (read only i.e. Producer)
- Contravariance: accept supertypes (write only i.e. Consumer)
