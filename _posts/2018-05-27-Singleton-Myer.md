---
layout: post
title: Thread safe singleton strings in C++.
---

## Problem
There is a string which will remain unchanged through out the program. And we want to initialise the string only once at the begining.

#### Method 1
At first glance, local static variable seems like a good way to acheive this. Static variables life time is not attached to the fuction but until the program ends.


```cpp
	string getStaticString() {
		static std::string testString;
		if(testString.empty()) {
			testString = doSomeWorkAndReturnString();
		}

		return testString;
	}
	cout<<getStaticString();	
```

But there is a catch here. If multiple threads enters the if block simultaneously the program will crash since, internally access to std::string is not thread safe.
One way to solve this is by adding mutex.

```cpp
	mutex mux;
	string getStaticString() {
		static std::string testString;
		if(testString.empty()) {
			mux.lock();
			testString = doSomeWorkAndReturnString();
			mux.unlock();
		}

		return testString;
	}
	cout<<getStaticString();	
```

#### Method 2
In C++11 local static variables are initialised only one even if multiple threads tries to access it at once([1](https://en.cppreference.com/w/cpp/language/storage_duration#Static_local_variables)). So if we create a class with initialising function as the constructor we should have a working solution.

```cpp
	class singletonString {
		public: 
			static string getStaticString() {
				static singletonString instance;
				return instance.m_testString;
			}
		private:
			string m_testString;
			singletonString() {
				m_testString = doSomeWorkAndReturnString(); 
			}
	}

	cout<<singletonString::getStaticString();
	
```
The singleton that is used here is the well known Myers singleton.

References:
1. https://en.cppreference.com/w/cpp/language/storage_duration#Static_local_variables
