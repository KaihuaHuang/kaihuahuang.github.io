---
layout:     post
title:      Design Pattern - 1
subtitle:   Singleton
date:       2020-2-12
author:     William
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - Design Pattern
    - Singleton
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

# Overview of Singleton
## Introduction
The singleton is a pattern that's used to restrict the number of class instantiations to exactly one. If in the entire program, there is only one object of a certain type. Then we can consider applying singleton to that type. 

For example, in derivative pricing, there should be only one UK::Exchange calendar. In this case, we can make the UK::Exchange calendar as a Singleton.

## Quick Example
Below is the simplest singleton implementation just for illustration. In this case, the program doesn't even instantiate a object of UKExchangeCalendar. It uses static member to gurantee that only one copy of data members is allocated. 
``` cpp
// .h file
class UKExchangeCalendar{
    public:
        // related public API
        static bool isBusinessDay(Date d);
        static bool isWeekend(Date d);
        ...
        
    private:
        // Explicitly delete constructor, so the program cannot instantiate this class 
        // The static data member will be used and accessed by static memeber function
        UKExchangeCalendar() =delete;
        
        // static date memebers
        static Date NewYear;
        static Date Yuanxiao;
        ....
}

// .cpp file
// Define the static class member
Date NewYear = Date(2020,1,1);
...

// other .cpp file
// The way to use singleton
UKExchangeCalendar::isBusinessDay(Date(2019,05,07));
```

## Concerns about Singleton
**1. Drawback of global variable**
Actually, we can view singleton as global vairbale roughly. However, global variable could be dangerous. For example, if there are multiple threads are accessing the global variable (or say singleton), if one of the thread modify the data, other threads will read the wrong data. In conclusion, we should use singleton carefully.

**2. Lose generalizability**
Use previous UKExchange Calendar as an example. If in the future, there are domestic UKExchange Calendar and foreign UKExchange Calendar. In this case, only one object is not enough. We will need one for domestic and one for foreign. Thus, it would be painful for us to reconstruct our codes. The decision to use a singleton is a judgment decision that trades generalizability for expediency. 

> **Negative Exmaple**

> Around 30 years ago, there is only one keyboard connected to the computer. So the program wrote at that time naturally design the keyboard as a singleton. Today, we have USB keyboards, wireless keyboards etc. And several keyboards can be connected to the computer. Singleton becomes a bad design for keyboard and those programs have to be rewritten.

# Variation of Singleton Implementation
## Static Singleton
Static singleton gurantee the data member unique by making them to be static. The Quick Example above is one of the simplest implementation of static singleton. 

There is also another implementation of static singleton. In this case, the program uses the C++ feature that besides static memeber function, object can also access the static data memeber.
``` cpp
// .h file
class UKExchangeCalendar{
    public:
        // Most basic constructor
        // created object only provide a handle for user to access static data memeber
        UKExchangeCalendar(){}
    
        // related public API
        // In this implementation, they are not static memeber function
        bool isBusinessDay(Date d);
        bool isWeekend(Date d);
        ...
        
    private:
        
        // static date memebers
        static Date NewYear;
        static Date Yuanxiao;
        ....
}

// .cpp file
// Define the static class member
Date NewYear = Date(2020,1,1);
...

// other .cpp file
// The way to use singleton
UKExchangeCalendar UKCalendar;
UKCalendar.isBusinessDay(Date(2019,05,07));

// Use it on the fly by temporary object
UKExchangeCalendar().isBusinessDay(Date(2019,05,07));
```

In this version, eventhough several objects can be created which seems violate the requirement of singleton. But they are "fake" objects, the data memebers are still unique (because they are static).

The biggest drawback of static singleton is that the static data members are initialized toghther with all the static data in the program before get into the main() function. And it's impossible to know and control the order of static data initialization. This will cause problems when the static data B is dependent on static data A. If static data B initialization is before A, the initialization will be failled.

## Meyers' Singleton
To address the drawback of static singleton, Meyers' singleton is created. In this implementation, the program doesn't make the data memeber to be static. In turn, it makes the instance to be static to gurantee uniqueness of the data.

``` cpp
// .h file
class UKExchangeCalendar{
    public:
        static UKExchangeCalendar& instance(){
            // Making the instance to be static to gurantee uniqueness
            static UKExchangeCalendar instance;
            return instance;
        }
        
        // related public API
        // In this implementation, they are not static memeber function
        bool isBusinessDay(Date d);
        bool isWeekend(Date d);
        ...
        
    private:
        // To gurantee that there is only one instance, we need to
        // 1. Make consturctor private so that we cannot instantiate object outside class
        UKExchangeCalendar(){
            // Define class member
            Date NewYear = Date(2020,1,1);
            ...
        }
        
        // 2. Delete copy constructor
        UKExchangeCalendar(const UKExchangeCalendar&) = delete;
        
        // 3. Delete assign operator
        UKExchangeCalendar& operator = (const UKExchangeCalendar&) = delete;
    
        //date memebers are not static anymore
        Date NewYear;
        Date Yuanxiao;
        ....
}

// other .cpp file
// The way to use singleton
UKExchangeCalendar::instance().isBusinessDay(Date(2019,05,07));
```

The key of this implementation is instance( ) static memeber function. In this function, the instance is declared to be static. So it will be instantiated once. What's more, it's a kind of lazy initialization - the initialization is deferred until it's needed (the first time invoke instance( ) function). And this also solve the drawback of static singleton. Because, when static data B need A (by invoking A::instance( ) ), A will be instantiated.

However, this implementation will slow down the program. Because every call to UKExchangeCalendar::instance( ) must check whether the object is already initialized. By keeping a reference as the codes below can mitigate this problem.

```cpp
// Only call instance() once
UKExchangeCalendar& UKCalendar = UKExchangeCalendar::instance();
UKCalendar.isBusinessDay(Date(2019,05,07));
```

# Industry Practice - QuantLib
In this part, we will go through the implementation of calendar in QuantLib. QuantLib is an open source and free quantitative finance libaray which are widely used in the industry.

Below is the skeleton of QuantLib implementation. It uses the combination of **pimpl** and singleton.

``` cpp
//calendar.h
class Calendar {
    protected:
     //! abstract base class for calendar implementations
        class Impl {
          public:
            virtual ~Impl() {}
            // These are fundamental functions
            virtual std::string name() const = 0;
            virtual bool isBusinessDay(const Date&) const = 0;
            virtual bool isWeekend(Weekday) const = 0;
            std::set<Date> addedHolidays, removedHolidays;
        };
        //! partial calendar implementation
        /*! This class provides the means of determining the Easter
            Monday for a given year, as well as specifying Saturdays
            and Sundays as weekend days.
        */
        class WesternImpl : public Impl {
          public:
            bool isWeekend(Weekday) const;
            //! expressed relative to first day of year
            static Day easterMonday(Year);
        };
        ext::shared_ptr<Impl> impl_;
    public:
        // public API
        Calendar() {}
        bool isBusinessDay(const Date& d) const;
        Date::serial_type businessDaysBetween(const Date& from,
                                              const Date& to,
                                              bool includeFirst = true,
                                              bool includeLast = false) const;
        ...
}


```
The header file of calendar provides the interface of Calendar and the abstract base class of calendar implementations. The advantage of this implementation is a better separation between the
interface and the implementation.

``` cpp
//calendar.cpp
inline bool Calendar::isBusinessDay(const Date& d) const {
    // Some validation checks here
    return impl_->isBusinessDay(_d);
}
Date::serial_type Calendar::businessDaysBetween(const Date& from,
                                                    const Date& to,
                                                    bool includeFirst,
                                                    bool includeLast) const {
        Date::serial_type wd = 0;
        if (from != to) {
            if (from < to) {
                // the last one is treated separately to avoid
                // incrementing Date::maxDate()
                for (Date d = from; d < to; ++d) {
                    if (isBusinessDay(d))
                        ++wd;
                }
                if (isBusinessDay(to))
                    ++wd;
            } else if (from > to) {
                for (Date d = to; d < from; ++d) {
                    if (isBusinessDay(d))
                        ++wd;
                }
                if (isBusinessDay(from))
                    ++wd;
            }

            if (isBusinessDay(from) && !includeFirst)
                wd--;
            if (isBusinessDay(to) && !includeLast)
                wd--;

            if (from > to)
                wd = -wd;
        } else if (includeFirst && includeLast && isBusinessDay(from)) {
            wd = 1;
        }

        return wd;
    }
...

```
I provides part of the calendar.cpp file here is to illustrate that the essencials of the calendar is determine whether the day is business day or not. And **other advanced functions like businessDaysBetween is just based on the fundamental function isBusinessDay.** So we just need to implemente those fundamental functions (by finish impl abstract class) then the calendar class will work.

``` cpp
// unitedkingdom.hpp
class UnitedKingdom : public Calendar {
      private:
        class SettlementImpl : public Calendar::WesternImpl {
          public:
            std::string name() const { return "UK settlement"; }
            bool isBusinessDay(const Date&) const;
        };
        class ExchangeImpl : public Calendar::WesternImpl {
          public:
            std::string name() const { return "London stock exchange"; }
            bool isBusinessDay(const Date&) const;
        };
        class MetalsImpl : public Calendar::WesternImpl {
          public:
            std::string name() const { return "London metals exchange"; }
            bool isBusinessDay(const Date&) const;
        };
      public:
        //! UK calendars
        enum Market { Settlement,     //!< generic settlement calendar
                      Exchange,       //!< London stock-exchange calendar
                      Metals          //|< London metals-exchange calendar
        };
        UnitedKingdom(Market market = Settlement);
    };
```

In this header file of UK calendar, it declare three implementations of Calendar::WesternImpl. Since the definition of these implementations is under private access-specifier of class UnitedKingdom. So they can only be used inside the class UnitedKingdom. In another word, program cannot instantiate them outside class which can gurantee the uniqueness of 

``` cpp
UnitedKingdom::UnitedKingdom(UnitedKingdom::Market market) {
        // all calendar instances on the same market share the same
        // implementation instance
        // Singleton !
        static ext::shared_ptr<Calendar::Impl> settlementImpl(
                                           new UnitedKingdom::SettlementImpl);
        static ext::shared_ptr<Calendar::Impl> exchangeImpl(
                                           new UnitedKingdom::ExchangeImpl);
        static ext::shared_ptr<Calendar::Impl> metalsImpl(
                                           new UnitedKingdom::MetalsImpl);
        switch (market) {
          case Settlement:
            impl_ = settlementImpl;
            break;
          case Exchange:
            impl_ = exchangeImpl;
            break;
          case Metals:
            impl_ = metalsImpl;
            break;
          default:
            QL_FAIL("unknown market");
        }
    }

// detailed implementation of settlementImpl, exchangeImpl and metalsImpl
// ...
```

In this file, meyer's singleton pattern is used. The instance of implementation is unique (declared as static). For detailed codes, please refer to the [Git Repo](https://github.com/lballabio/QuantLib/blob/master/ql/time/calendars/unitedkingdom.hpp).

# References
[Hands-On Design Patterns with C - Fedor G. Pikus](https://www.amazon.com/Hands-Design-Patterns-reusable-maintainable/dp/1788832566)
[QuantLib](https://www.quantlib.org/)


