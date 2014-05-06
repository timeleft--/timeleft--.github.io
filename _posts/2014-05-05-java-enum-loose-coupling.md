---
layout: bootstrap
title: Loose coupling of Java enums
date: May 5, 2014
---
<p>A quick search will show that a lot of discussion is going on about ways to extend Java enums, but what is mostly discussed is the ability to add values to the enum. This isn't why I thought I need to extend a Java enum when I typed in the query. The reason I think Java enums need to be extensible is for developers to be able to loosen the coupling between code they own and code of <i>others</i>, and still be able to use enum constants that <i>others</i> define (others being developers of a framework, or a service your code calls, .. etc etc). The moment you start importing the enum type all over the place in your code then your code is becoming tightly coupled to the code of <i>others</i>, and this is bad because if the <i>others</i> decide to change something (the package of their enum for example) then you will need to change many files in your code. Obviously, this is also bad if you decide you want to swap the code of the <i>others</i> for another.</p>

### The quick and dirty solution: use Strings

<p>Disrespect the ingenious of whoever decided to introduce enums into Java, and fallback to using String constants (please not literals). I don't think you will be reading this blog if you are one who would opt for such solution.</p>

### The EnumExtension class

<p>The pattern I use when I import enums into my code is that I keep direct references to enums in a Facade between my code and the code of <i>others</i>. I obviously don't use the enums in my code directly, but rather create an EnumExtension for each enum dependency and use it in my code instead. The EnumExtension is a generic abstract class that can be used like enums are used (it walks like an enum and it quacks like an enum, yet it is not an enum... this is Java not Ruby). To extend an enum, I create a concrete subclass of the EnumExtension specifying the generic argument to be the enum I am extending. </p>

Here is how I do it:

1. Create an abstract class with a generic argument that must be an enum. For now, let it contain only a final member which is the enum constant value it represents/wraps. Code: 

        public abstract class EnumExtension&lt;E extends Enum<E>> {
            protected final Enum<E> original;
        }

2. Provide only a protected constructor, and copy the valueOf method from java.lang.Enum only changing the return type. Code:

        public abstract class EnumExtension<E extends Enum<E>> {
             public static &ltE extends Enum<E>> EnumExtension<E> valueOf(Class<E> enumType, String name) {
                E resultValue = enumType.enumConstantDirectory().get(name);
        
                if (result != null)
                    return new EnumExtension<E>(resultValue);
        
                if (name == null)
                    throw new NullPointerException("Name is null");
        
                throw new IllegalArgumentException("No enum const " + enumType + "." + name);
            }
        
            protected final Enum<E> original;
        
            protected EnumExtension(Enum&ltE> original) {
                this.original = original;
            }
        }
        
3. Now we need to implement some of the methods in the Enum class delegating them to the wrapped value; ordinal(), name(), toString(), equals() and last but not least hashCode(). Code:

        public abstract class EnumExtension<E extends Enum<E>> {
             public static &ltE extends Enum<E>> EnumExtension<E> valueOf(Class<E> enumType, String name) {
                E resultValue = enumType.enumConstantDirectory().get(name);
        
                if (result != null)
                    return new EnumExtension<E>(resultValue);
        
                if (name == null)
                    throw new NullPointerException("Name is null");
        
                throw new IllegalArgumentException("No enum const " + enumType + "." + name);
            }
        
            protected final Enum<E> original;
        
            protected EnumExtension(Enum&ltE> original) {
                this.original = original;
            }
        
            public int ordinal() {
                return original.ordinal();
            }
            
            public String name() {
                return original.name();
            }
            
            @Override
            public String toString() {
                return original.toString();
            }
        
            @Override
            public boolean equals(Object other) {
                return original.equals(other);
            }
            
            @Override
            public int hashCode() {
                return original.hashCode();
            }
        
        }

4. That's it for the EnumExtension base class. Now you can create subclasses of it for each enum you want to use in your code: 

        public class LooselyCoupledEnumOfOthers extends EnumExtension<EnumOfOthers> {
            
            protected LooselyCoupledEnumOfOthers(EnumOfOthers original) {
                super(original);
            }
        }

5. If you are really keen on making your EnumExtension closely resemble the Java Enum class, then you need to make it implement Comparable&lt;E> and Serializable.
 
###Conclusion
In this post I have blogged about a method I use for wrapping enums into what would seem to be an enum, as far as coding is concerned. This method gives the ability to use enum constants defined by dependencies, while keeping own code loosely coupled from it. This might look like a glorified wrapper, specially that I call it an EnumExtension, but there is more to it than just being a wrapper. Granted, as it is right now it is just a wrapper, but in many cases only wrapping an enum to provide loose coupling is not enough. What happens when an enum defines so many constant values and you use only a subset of those values in your code? Is there a  way to make sure an enum constant value that your code doesn't support cannot be instantiated? I will write about how I do this in the next blog post. Please give me a nudge if you want me to write that post soon.

###Final remarks
I am sorry if I didn't get to providing some way to post comments yet. Your feedback is welcome through direct messages on my [LinkedIn](http://ca.linkedin.com/in/younosnaga/) or [Google+](https://plus.google.com/110367479775392810217?rel=author).

