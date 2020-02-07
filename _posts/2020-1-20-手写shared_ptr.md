---
title: 手写shared_ptr
key: 20200120
tags: C++
---

```c++
template<typename T>
class smart{
private:
   T* _ptr;
   int * _count;  //reference counting
public:
   //构造函数
   smart(T* ptr =  nullptr ):_ptr(ptr)
   {
     if (_ptr){
       _count =  new int (1);
     }
     else {
       _count =  new int (0);
     }
   }
  
   //拷贝构造
   smart( const smart& ptr)
   {
     if ( this != &ptr)
     {
       this ->_ptr = ptr._ptr;
       this ->_count = ptr._count;
      
       (* this ->_count)++;
     }
   }
  
   //重载operator=
   smart operator=( const smart& ptr)
   {
     if ( this ->_ptr == ptr._ptr)
     {
       return * this ;
     }
     if ( this ->_ptr)
     {
       (* this ->_count)--;
       if ( this ->_count == 0)
       {
         delete this ->_ptr;
         delete this ->_count;
       }
     }
     this ->_ptr = ptr._ptr;
     this ->_count = ptr._count;
     (*this ->_count)++;
     return * this ;
   }
  
   //operator*重载
   T& operator*()
   {
     if ( this ->_ptr)
     {
       return *( this ->_ptr);
     }
   }
  
   //operator->重载
   T& operator->()
   {
     if ( this ->_ptr)
     {
       return this ->_ptr;
     }
   }
   //析构函数
   ~smart()
   {
     (* this ->_count)--;
     if (* this ->_count == 0)
     {
       delete this ->_ptr;
       delete this ->_count;
     }
   }
   //return reference counting
   int use_count()
   {
     return * this ->_count;
   }
};
```

主要是注意到拷贝函数的参数是否指向同一个对象