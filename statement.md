# 
```C++ runnable
/*#include <cstdlib>
#include <iostream>
#include <new>

static std::size_t alloc{0};
static std::size_t dealloc{0};

void* operator new(std::size_t sz){
    alloc+= 1;
    return std::malloc(sz);
}

void operator delete(void* ptr) noexcept{
    dealloc+= 1;
    std::free(ptr);
}

void getInfo(){
    
    std::cout << std::endl;
 
    std::cout << "Number of allocations: " << alloc << std::endl;
    std::cout << "Number of deallocations: " << dealloc << std::endl;
    
    std::cout << std::endl;
} */

#include <algorithm>
#include <cstdlib>
#include <iostream>
#include <new>
#include <string>
#include <array>

int const MY_SIZE= 10;

std::array<void* ,MY_SIZE> myAlloc{nullptr,};
    
void* operator new(std::size_t sz){
    static int counter{};
    void* ptr= std::malloc(sz);
    myAlloc.at(counter++)= ptr;
    std::cerr << "Addr.: " << ptr << " size: " << sz << std::endl;
    return ptr;
}

void operator delete(void* ptr) noexcept{
    auto ind= std::distance(myAlloc.begin(),std::find(myAlloc.begin(),myAlloc.end(),ptr));
    myAlloc[ind]= nullptr;
    std::free(ptr);
}

void getInfo(){
    
    std::cout << std::endl;
     
    std::cout << "Not deallocated: " << std::endl;
    for (auto i: myAlloc){
        if (i != nullptr ) std::cout << " " << i << std::endl;
    }
    
    std::cout << std::endl;
    
}

#include <iostream>
#include <string>

class MyClass{
  float* p= new float[100];
};

class MyClass2{
  int five= 5;
  std::string s= "hello";
};


int main(){
    
    int* myInt= new int(1998);
    double* myDouble= new double(3.14);
    double* myDoubleArray= new double[2]{1.1,1.2};
    
    MyClass* myClass= new MyClass;
    MyClass2* myClass2= new MyClass2;
    
    delete myDouble;
    delete [] myDoubleArray;
    delete myClass;
    delete myClass2;
    
   getInfo();
    
}
