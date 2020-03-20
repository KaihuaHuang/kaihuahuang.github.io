---
layout:     post
title:      Design Pattern - 2
subtitle:   Visitor
date:       2020-3-10
author:     William
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - Design Pattern
    - Visitor
---
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: { 
      inlineMath: [['$','$'], ['\\(','\\)']],
      processEscapes: true
    }
  });
  </script>
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# Overview of Visitor Pattern
## Introduction
From the view of design, visitor patter is used to make large class hierarchies more maintainable by seperating the algorithm from the object structure (data for algorithm). This pattern follows **open/closed principle** - open to extension, close to modification.

From the view of implementation, visitor patter implements **double dispatch**. Double dispatch means the actual opearation depends on 2 factors. For example, in financial world, the price of any derivatives depends on two things: characters of derivatives and underlying models (Heston / Local Vol / BS etc.).



## Quick Example
First, we need a class hierarchy to store the data (derivative info) for algorithm (models).
``` cpp
class Derivative{
    public:
        virtual ~Derivative (){}
        //Constructor
        //...
    private:
        double expire_;
        double notional_;
};

class Vanilla: public Derivative{
    private:
        bool pcFlag_;
        double strike_;
}

class Digital: public Derivative{
    private:
        bool pcFlag_;
        double strike_;
}
```

Then we need a pricie engine as a visitor to handle the pricing operation.
``` cpp
// Pure virtual class - Interface
class PriceEngine{
    public:
        virtual void visit(Vanilla* option) = 0;
        virtual void visit(Digital* option) = 0;
        double getPrice(){return price_;}
    private:
        double price_;
        
}

class BSPriceEngine: public PriceEngine{
    public:
        void visit(Vanilla* option){
            // Detailed Implementation
        }
        void visit(Digital* option){
            // Detailed Implementation
            
        }
    private:
        double spot_;
        double discountFactor_;
}
```

To let the visitor access the private data of the class, we should add an interface for visitor.
``` cpp
class Derivative{
    public:
        virtual void accept(PriceEngine& engine) = 0;
        ...
}

class Vanilla: public Derivative{
    public:
        void accept(PriceEngine& engine){ 
            // The key is this pointer which is Vanilla* type.
            // So the behavior of engine will also vary according to the parameter type
            engine.visit(this);
        }
}

class Digital: public Derivative{
    public:
        void accept(PriceEngine& engine){ 
            engine.visit(this);
        }
}
```
Below is the way how we uses this:
``` cpp
// Constructor may take some parameters, below is just for illustration
BSPriceEngine engine;
Vanilla call;
call.accept(engine);
cout << engine.getPrice();
```

In most cases, we don't know the actual type of derivative at compile time. The visitor can be used polymorphically as below:
``` cpp
// Constructor may take some parameters, below is just for illustration
BSPriceEngine engine;
std::unique_ptr<Derivative> p(new Vanilla());
p->accept(engine);
cout << engine.getPrice();
```

## Quick Summary
Above is the most bare-bones example. But we can still find the benefits of visitor patterns:

**1. We can easily add new price engine (Heston, SLV, LV etc..) without modifing the derivative class hierarchy.
2. The actual implementation is double dispatch according to derivative type and price engine type.**


But the disadvantages of this simplest visitor pattern are also obvious:

**1. if a new class is added to the hierarchy, all visitors must be updated.**

So for this reason, it is sometimes recommended to use the visitor only on relatively stable hierarchies that do not have new classes added often.




# One Step Further - Additional Paramters
## Naive Solution
In the previous example, *visit()* method do not have other parameters except the pointer to the derived class. But in many cases, we need more parameters to make it work.

For example, now we want to add local vol price engine. So we need implied vol surface as parameter.
``` cpp
class LVPriceEngine: public PriceEngine{
    public:
        void visit(Vanilla* option, ImpSurface* surface){
            // Detailed Implementation
        }
        void visit(Digital* option, ImpSurface* surface){
            // Detailed Implementation
            
        }
    private:
        double spot_;
        double discountFactor_;
}
```

So as the changes to class hierarchy interfaces
``` cpp
class Derivative{
    public:
        virtual void accept(PriceEngine& engine, ImpSurface* surface) = 0;
        ...
}
```

The bad side of this is obvious, we add a new interface and the derived class have to implement it even sometimes it's meaningless (This interface is usesless for black scholre model).

## Improved Solution
To make the interface as general as possbile, we can define an aggregates to store all the parameters with uniform interface.

``` cpp
# Provide an uniform interface
class EngineParameters{};

class LVEngineParameters: public EngineParameters{
    public:
        ImpSurface* getSurface(){return surface_;}
    private:
        ImpSurface* surface_;
}
```

In this case, the class hierachy only needs to provide a general interface. 
``` cpp
class Derivative{
    public:
        virtual void accept(PriceEngine& engine, EngineParameters* parameters = nullptr) = 0;
        ...
}

class Vanilla: public Derivative{
    public:
        void accept(PriceEngine& engine,EngineParameters* parameters = nullptr){ 
            // The key is this pointer which is Vanilla* type.
            // So the behavior of engine will also vary according to the parameter type
            engine.visit(this, parameters);
        }
}

```
So as the visitor (price engine).
``` cpp
class LVPriceEngine: public PriceEngine{
    public:
        void visit(Vanilla* option, EngineParameters* parameters = nullptr){
            // daynamic cast the parameter type
            parameters = dynamic_cast<LVEngineParameters*>(parameters);
            
            // Detailed Implementation
        }
        void visit(Digital* option, EngineParameters* parameters = nullptr){
            // daynamic cast the parameter type
            parameters = dynamic_cast<LVEngineParameters*>(parameters);
            
            // Detailed Implementation
        }
    private:
        double spot_;
        double discountFactor_;
}
```

## Return Value of accept() and visit()
It's not necessary to be void. In previous case, the calculated result is stored in price_ private data memeber and accessed by public API. We can also return price directly from accept() and visit() function. 

But if the return type varies, we will encounter same problem as Naive solution. So, same logic of Improved solution can be applied to return type.


# Visiting Composite Objects
When the object is composite objects, there are several component. The correct way to handle the compnent objects is to simply visit each one and thus delegate the problem to someone else.

Suppose we have a deal contains 2 derivatives:
```cpp
class Deal{
    public:
        void accept(PriceEngine& engine,EngineParameters* parameters = nullptr){
            totalPrice_ = 0;
            // Simply visit each component
            for(auto& p : positions_){
                p->accept(engine,parameters);
                totalPrice_ += engine.getPrice();
            }
        }
        
        double getTotalPrice(){return totalPrice_;}
    private:
        std::vector<std::unique_ptr<Derivative>> positions_;
        double totalPrice_;
}; 

```

In this way, we decompose the problem into smaller sub-problems which can be handled by existing price engine.

# Acyclic Vistor
As what we have seen so far, visitor pattern does its jobs well - seperating the data and algorithms. However, it also brings additonal dependences:

**1. New visitor(algorithms) has to implement *visit()* method for all the visitable types. 
2. Every time a new object is added to the hierarchy, every visitor must be updated.** 

Traditional visitor pattern requires that every possible combination of the object type and visitor type must be handled. But often there are cases when some combinations do not make sense (BSprice engine cannot price path-dependent derivative).

Acyclic vistor is a variant of the Visitor pattern to address this problem by employing multi-inheritance.

We start by defining most general base class.
```cpp
class Derivative{
    public:
        virtual ~Derivative (){}
        # Accept most general visitor
        virtual void accept(AcyclicVistor& visitor) = 0;
        //Constructor
        //...
    private:
        double expire_;
        double notional_;
};

# Most base vistor class, but we don't define any interfaces here
class AcyclicVistor{
    public:
        ~AcyclicVistor(){}
}
```
Then we add derived class and its corresponding vistor.
```cpp
# Forward Declaration
class Vanilla;
class VanillaVistor{
    public:
        # Define the needed interface here
        virtual visit(Vanilla* option) = 0;
}


class Vanilla: public Derivative{
    public:
        void accept(AcyclicVistor& visitor){ 
            if(VanillaVistor* vanillaVistor = dynamic_cast<VanillaVistor*>(&visitor)){
                vanillaVistor -> visit(this);
            }else{
                // Error Handle
            }
        }
}
```

As we can find from above codes, the input visitor has to be both the type of AcyclicVistor and VanillaVistor. So the price engine has to inherite these two base class.
```cpp
class BSPriceEngine: public VanillaVistor, AcyclicVistor{
    public:
        void visit(Vanilla* option){
            // Detailed Implementation
        }
}
```

The invocation of Acyclic visitor pattern is done same as traditional visitor pattern.
```cpp
BSPriceEngine engine;
std::unique_ptr<Derivative> call(new Vanilla());
std::unique_ptr<Derivative> digital(new Digital());

call.accept(engine);
digital.accept(engine); //Error
```

## Comparison with traditonal Visitor Pattern
The Acyclic Visitor pattern looks so good that one has to wonder, why not use it all the time instead of the regular Visitor pattern?

**The biggest issues is that *dynamic_cast* is typically more expensive than the virtual function call. So the Anacylic Vistor will slow down the program comparing to traditional visitor pattern.**

# Generic Vistor
## Simplify Visitable Class 
Generic Vistor is not a variant of visitor pattern. It uses the power of modern C++ to simplify the codes (specificlly template feature).

The traditional visitor pattern has similar codes in *accept()* method as below:
```cpp
class Derivative{
    public:
        virtual void accept(PriceEngine& engine) = 0;
        ...
}

class Vanilla: public Derivative{
    public:
        void accept(PriceEngine& engine){ 
            engine.visit(this);
        }
}

class Digital: public Derivative{
    public:
        void accept(PriceEngine& engine){ 
            engine.visit(this);
        }
}
```

The code looks trivial. **Why can't we move it to base class and just inherit it?**

It's because the this pointer is the actual type we need. And it's also the key how we make double delegate work.

In this case, we can uses the **CRTP** pattern to let the base class know the type of derived class.
```cpp
class Derivative{
    public:
        virtual void accept(PriceEngine& engine) = 0;
        ...
}

template <typename Derived>
class Visitable: public Derivative{
    // C++11 Feature
    // Inheriting Constructor
    using Derivative::Derivative;
    void accept(PriceEngine& engine){
        // From derived to base; static_cast is fine.
        v.visit(static_cast<Derived*>(this));
    }
}

class Vanilla: public Visitable<Vanilla>{
    using Visitable<Vanilla>::Visitable;
    // Don't need to write accept boring codes anymore!
}

```

## Simplify Visitor Class
As in the previous example, defining the pure virtual class (interface) is also trivial.
```cpp
class PriceEngine{
    public:
        virtual void visit(Vanilla* option) = 0;
        virtual void visit(Digital* option) = 0;
        // ...
        // This repetitive codes problem becomes non-ignored when there is a large class hierchy
    
        double getPrice(){return price_;}
    private:
        double price_;
}
```

We can use variadic templates in C++11 to simplify this work. Comparing to traditional template, variadic template can take arbitrary number of Template parameters.
```cpp
template<typename ... Types>
class Vistor;

template <typename T, typename ... Types>
class Visitor<T, Types ...> : public Visitor<Types ...> {
    public:
        using Visitor<Types ...>::visit;
        virtual void visit(T* t) = 0;
};

template <typename T>
class Visitor<T> {
    public:
        virtual void visit(T* t) = 0;
};
```
variadic templates will recursively instaiate the template.

Then, it would be quiet easy to do the job as our previous codes:

```cpp
using PriceEngine = Visitor<Vanilla, Digital>;
```

# References
[Hands-On Design Patterns with C - Fedor G. Pikus](https://www.amazon.com/Hands-Design-Patterns-reusable-maintainable/dp/1788832566)

[variadic templates](https://blog.csdn.net/tennysonsky/article/details/77389891)

[Inheriting Constructor](https://blog.csdn.net/SwordArcher/article/details/88717442)
