---
layout:     post
title:      java常用包，类，接口
subtitle:   studying java
date:       2018-07-14
author:     inertia1p
header-img: img/post-bg-geometry.jpg
keywords_post:  "java，接口，包，类"
catalog: true
tags:
    - java
---

## class

#### 1.java.lang.Object

>object类

![class object](https://inertia1p.github.io/img_post/java.lang.object.png)

* Private static native void registerNatives()<br>
 use ‘native’ to decorate itself. In java, the function what decorate by ‘native’need use c/c++ to complete, and be compile into  .dll.

```
private static native void registerNatives();
static{
  registerNatives();
}
```

* Protected native object clone()<br>
is also used ‘native’ to decorate. Clone() return a quote of object’s duplicate.
Firstly, we look at the example:

```
package com.corn.objectsummary;

import com.corn.Person;

public class ObjectTest{
  public static void main(String[] args){
    Object o1 = new Object();
    //The method clone() from the type Object is not visible
    Object clone = o1.clone();
  }
}
```

This example will return a exception : "The method clone() from the type Object is not visible"
Why?<br>
Because if you want to use clone() to clone a object, the object need to a implement Cloneable.

* Public final native class<?> getclass()<br>
returns the class object / run-time class object Class<? > of this Object object. The effect is the same as that of Object.class.(This relate to reflection mechanism in java and I am not really understand!)

* Public boolean equals(Object obj)<br>
The difference between ‘==’ and ‘equals’ is commonly known.
== express the value of variable is exactly the same. (basic type is the value, quote is the address)
Equals express the attribute of quote is the same.<br>
/## we always override the function equals() to make it easier to use .

* Public native int hashcode()<br>
Hashcode() have these appointment:<br>
1.During the execution of the java program. When the hashCode() is called multiple times for the same object. It will return the same hashcode. The premise is that the ruler information used in equals comparison is not changed. One execution of java program to anther, the same object return the hashcode need no consistency.<br>
2.If two object called equals() return true, then their hashcode must be consistency.<br>
3.On the contrary, if two object called hashcode() return the same hashcode, they might be not equal.<br>
The method hashCode() show superiority in collection class, when we need set a new object, we need not to use equals(), it may be slow, hashCode() return a hashcode, then just compare the hashcode can confirm the differences in collection.<br>
/##override the equals() ,always need to override hashCode().

* Public string toString()<br>
/@return getClass().getName() + "@" + Integer.toHexString(hashCode());<br>
Therefore, toString() is uniquely determined by type of the class and its hashcode. The same type but not equal object might be different because they have possibility of having same hashcode.

* Public final void wait()
* Public final native void notify()
* Public final native void notifyAll()
* Public final native void wait(long timeout)
* Public final void wait(long timeout, int nanos)<br>
These methods are always used to use thread<br>
Wait():call this method to make current thread to wait until calling notify()/notifyAll() on other threads.
Wait(long timeout):what is the difference between wait() and wait(long timeout) is that if the waiting time exceed the setting, the thread will wake up.<br>
Notify()/notifyAll():used to wake up thread.

* Protected void finalize()<br>
If we want to use this method ,we need define the method ourselves. It be set to default method. At the other time, it will be call before JVM garbage collection mechanism.

#### 2.java.lang.String

>String 类

* public boolean equals(object ob)<br>
First of all, compare whether the two object is the same object(use '=='), if true, then return true, otherwise make a further judgment.<br>
1) judge whether it belongs to String or not.<br>
2) judge the length of string is equal or not.<br>
3) compare char
```
public boolean equals(Object anObject) {
     if (this == anObject) {
         return true;
     }
     if (anObject instanceof String) {
         String anotherString = (String)anObject;
         int n = value.length;
         if (n == anotherString.value.length) {
             char v1[] = value;
             char v2[] = anotherString.value;
             int i = 0;
             while (n-- != 0) {
                 if (v1[i] != v2[i])
                      return false;
             }
             return true;
         }
      }
      return false;
}
```

* public String replace(char oldChar, char newChar)<br>
a easy function.

```
public String replace(char oldChar, char newChar) {
        if (oldChar != newChar) {
            int len = value.length;
            int i = -1;
            char[] val = value; /* avoid getfield opcode */

            while (++i < len) {
                if (val[i] == oldChar) {
                    break;
                }
            }
            if (i < len) {
                char buf[] = new char[len];
                for (int j = 0; j < i; j++) {
                    buf[j] = val[j];
                }
                while (i < len) {
                    char c = val[i];
                    buf[i] = (c == oldChar) ? newChar : c;
                    i++;
                }
                return new String(buf, true);
            }
        }
        return this;
    }
```

* public String replace(CharSequence target, CharSequence replacement)<br>
This function is more commonly used. java.lang.string a

<br>
