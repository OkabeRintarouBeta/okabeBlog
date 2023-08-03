---
author: zihui
pubDatetime: 2023-07-31T10:00:00Z
title: Cherno C++ Notes
postSlug: cherno-c++-notes
featured: true
draft: false
tags:
  - C-plus-plus
  - Youtube
ogImage: ""
description: Notes for the course on C++ provided by The Cherno
---

## Table of contents

## Static

Use it to make the variable is only linked inside the translation unit that it is declared.
For example, we have two files.
In Static.cpp, there is a line:

```C++
    int var1=10;
```

In Main.cpp, we have

```C++
int var1=10;

int main(){
    cout<<var1<<endl;
    return 0;
}
```

Then we compile these two files, there should be an error message.
That is because `var` is declared twice. To correct it, we have two options:

1. Make var1 static(only linked inside one cpp file),like `static int var1=10` inside `Static.cpp`
2. If we want to make `var1` global, then inside `Main.cpp`, we have `extern var1;`, note that we don't use `static` in `Static.cpp` in this case.

### Static inside class

1. Static variables that are inside a class are shared between different class instances, like global variables
2. Static methods have similar effects
3. Static methods can't access non-static variables

```C++
class example{
    int x;
    int y;

    static void Print(){
        cout<<x<<y<<endl; //WRONG, since x and y are non-static variables
    }
};
```

## ENUMS

- Without `enum`:

  ```c++
  #include <iostream>

  int A=0;
  int B=1;
  int C=2;

  int main(){
    int value=B;
    if(value==1){
        //Do something
    }
  };
  ```

  - Problem: value could be changed, risk of redefining A,B,C later...

- With `enum`:

  ```C++
  #include <iostream>

  enum Example{
    A,B,C
    //if we don't specify A,B,C, they will be set to 0,1,2 by default
    // we could specify like A=1,B=2,C=3
  }

  /*
  to specify type, we could use:
  enum Example:float{
    .....
  }
  */

  // to refer to B, we could use Example::B

  int main(){
    Example value=B;  //Example gives a restriction about what value could be(only A or B or C)
    if(value==1){
        //Do something
    }
  }
  ```

## Constructor & Destructor

### Constructor

- By default has an empty constructor for each class
- Automatically called when an object of class is initialized
- Name must be the same as the class, no return type

#### Member Initializer Lists(Constructor Initializer List)

```c++
class Entity{
private:
  std::string m_Name;
  int x,y,z;
public:
  Entity():m_Name("Unknown"),x(0),y(0),z(0){}  //initializer, should be in order of variables
};
```

Why use it?

- If one class has a member of another class, without using member initializer list, the default constructor will create that member twice(waste in performance)

### Destructor

- Called by default when the object get destroyed
- Begin by `~` followed by the class name

## Inheritance

Subclass could use methods and variables of base class(as long as they are accessible), don't have to redeclare

### Virtual functions

- If the subclass rewrites a methods of base class and wants to override it, then use `virtual` keyword for the base class function, and add `override` keyword to the subclass function.

  ```c++
    #include <iostream>
    #include <string>

    class Entity{
      public:
        virtual std::string GetName(){
          return "Entity";
        }
    }

    class Player:public Entity{
      private:
        std::string m_Name;
      public:
        Player(const std::string& name):m_Name(name){}
        std::string GetName() override{
          return m_Name;
        }
    }
  ```

### Interfaces(pure virtual functions)

- An interface defines a pure virtual function in C++ class without commiting to a particular implementation of that class.
  - A pure virtual function is specified by placing a "=0" in its declaration
  ```c++
    class Printable{
      public:
        virtual std::string GetClassName()=0;
    }
  ```
  - For every subclass extended from the base class, it must have that method defined.

## Const

### Const for pointer

- `const int* a` or `int* const a`cannot change the content of the pointer (the data at address a), but can change a itself
  ```c++
    const int MAX_AGE=90
    const int* a = new int;
    *a=2; //ERROR
    a=(int*)&MAX_AGE; //OK
  ```
- `int* const a`: can change the content of the pointer, but couldn't change the pointer itself
  ```c++
    const int MAX_AGE=90
    int* const a=new int; // or const int* a=new int;
    a=new int;
    *a=2; //ok
    a=(int*)&MAX_AGE; //ERROR
  ```
- what is const depends on whether `const` is placed before or after \*.

### Const for class

```c++
class Entity{
  private:
    int m_X,m_Y;
  public:
    int GetX() const{ //this const means that the function couldn't modify any class variables
      m_X=2;  //ERROR
      return m_X; // OK
    }
};
```

- `int* m_X, m_Y`: then `m_X` is a pointer but `m_Y` is an integer
- Example:

  ```C++
  class Entity{
    private:
      int m_X,m_Y;
    public:
      int GetX() const{ //this const means that the function couldn't modify any class variables
        m_X=2;  //ERROR
        return m_X; // OK
      }
  };

  void PrintEntity(const Entity& e){
    std::cout<<e.GetX()<<std::endl;
    // if we don't declare GetX() as const, then there will be error
    // Since it is not guaranteed that GetX() won't change class variables in Entity e
  };
  ```

  ### Mutable

  Question from the last section: if there is a class variable that we really want to change in a class method marked as `const`, what shall we do?

  Ans: use `mutable`

  Example:

  ```c++
  class Entity{
    private:
      int m_Name;
      mutable int m_debugCount=0;

    public:
      const std::string& GetName() const{
        m_debugCount++;
        // by adding keyword mutable, the value of m_debugCount can now be changed in a const method
        return m_Name;
      }
  };
  ```

## How to create/instantiate objects

1. Stack allocation
   1. Example
      1. `Entity entity` call the default constructor
      2. `Entity entity("Cherno")` or `Entity entity=Entity("Cherno")`
   2. Advantage: fast
   3. Disadvantage:
      1. the object get destroyed out of range
      ```c++
        int main(){
          Entity* e;
          {
            Entity entity=Entity("Cherno");
            e=&entity;
            std::cout<<entity.GetName()<<std::endl;
          }
          // outside the above scope, entity get destroyed, e's name is gone
          std::cin.get();
        }
      ```
      2. not enough space
2. Heap allocation
   1. Example
      1. `Entity* entity=new Entity("Cherno");` use new keyword
      2. have to free the memory using `delete entity`
      ```c++
        int main(){
          Entity* e;
          {
            Entity* entity=new Entity("Cherno");
            e=entity;
            std::cout<<entity->GetName()<<std::endl;
          }

          std::cin.get();
          delete e;  // e does not get empty, we have to delete it
        }
      ```

## New keyword

- Use the new keyword to allocate a specified memory size(dynamically) on the heap (e.g. `int` takes 4 bytes of memory)
- operating systems must then go and find a contiguous block of memory to match the requested variable type and return a pointer to that memory address.
- With a class, it will determine the size of the class and then find a memory allocation
- with arrays it will calculate the type of array and multiply by the size of the array.
- The new keyword not only allocates memory, but also calls the constructor

  ```c++
  int a=2;
  int* b=new int[50]; //200 bytes
  Entity* entity=new Entity();
  // similar to Entity* entity=(Entity*)malloc(sizeof(Entity));
  // but the C++ way also calls the constructor, the C way only allocates the memory
  Entity* entity_list=new Entity[50];
  ```

- don't forget to use the delete keyword when we use the new keyword.
- if we use new keyword with array like new int[], use delete keyword like delete[]

## Implicit Conversion and the Explicit Keyword

### Implicit Conversion

```c++
class Entity{
private:
  std::string m_Name;
  int m_Age;
public:
  Entity(const std::string& name):m_Name(name),m_Age(-1){}
  Entity(int age):m_Name("Unknown"),m_Age(age){}
};

void PrintEntity(const Entity& entity){
  //Printing
}

int main{
  Entity a="Cherno";  // also correct
  Entity b=22;   // also correct

  PrintEntity(22); //works(!???)
  PrintEntity("Cherno") //ERROR: since "Cherno" is const char[7] instead of std::string by default
  PrintEntity(std::string("Cherno")); //works
  PrintEntity(Entity("Cherno")); //works
}
```

### Explicit

- Disables the implicit functionality
- `explicit` put before a constructor (e.g. `explicit Entity(int age):m_Name("Unknown"),m_Age(age){}`)
  - After that the implicit conversion mentioned above couldn't work
  - Need to use:
    - Entity a=Entity(22);
    - Entity a(22);

## Operator and Operator Overload

```C++
class Vector2{
public:
  float X,Y;
  Vector2(float x,float y):X(x),Y(y){}

  Vector2 operator+(const Vector2& other){
    return Vector2(X+other.X,Y+other.Y);
  }

  Vector2 operator*(const Vecto2& other){
    return Vector2(X*other.X,Y*other.Y);
  }

  //IMPORTANT: overload of std::cout
  std::ostream& operator<<(std::string&stream,const Vector2& other){
    stream<<other.X<<","<<other.Y<<endl;
    return stream;
  }
};
```

## THIS keyword

- Pointer to the current object instance that the method belongs to
- Inside member fuction of an object which has member variable age for instance, you can say `this->age`

## Object Lifetime

See the previous "Heap Allocation" section

- Don't do:

  ```c++
  int* createArray(){
    int array[50];
    return array;
  }
  ```

  - Just declared on the stack. It will be destroyed out of the scope
  - Correct the code:

    - Use `int* array=new int[50];`
    - Pass in by pointer

      ```c++
        int* createArray(int* arr){
          // fill out the array
        }

        int main(){
          int array[50];
          createArray(array);
        }
      ```

## Smart Pointer

- Don't have to call new or delete.
- Actually a wrapper around a raw pointer
- Type

  1. Unique pointer

  ```C++
  #include <memory>

  int main(){
    {
      std::unique_ptr<Entity> entity=std::make_unique<Entity>();
      entity->PrintName(); //call class methods in the same way
      // couldn't share the pointer(copy)
      std::unique_ptr<Entity> e=entity; //ERROR
    }
    std::cin.get();
  }
  ```

  2. Shared pointer

  ```C++
  #include <memory>

  int main(){
    std::shared_ptr<Entity>e;
    {
      std::shared_ptr<Entity> sharedEntity=std::make_shared<Entity>();
      e=sharedEntity; //increase ref count
    }
    //out of this scope, sharedEntity died but e does not
    std::cin.get();
  }
  ```

  3. Weak pointer

  ```C++
  #include <memory>

  int main(){
    std::shared_ptr<Entity>e;
    std::weak_ptr<Entity> e1;
    {
      std::shared_ptr<Entity> sharedEntity=std::make_shared<Entity>();
      e1=sharedEntity //don't increase ref count
      e=sharedEntity; //increase ref count
    }
    //out of this scope, sharedEntity died, e1 died but e does not
    std::cin.get();
  }
  ```
