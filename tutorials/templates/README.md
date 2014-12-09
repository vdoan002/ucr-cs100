#Templates and You:

Most C++ programmers have little to no experience with templates. This is because of their complex nature. Most C++ programmers stay away from templates because: 

  - Programmers have been succesfully programming without them
  - Templates are difficult to learn
  - The effort it takes to learn is not rewarding enough to some programmers

Templates are indeed quite difficult to learn at first, but if you put the time and effort into them, you can increase your programming effiency. The pros outweight the cons!

## But aren't functions the same thing?
Up until now, you been using functions in your programs (I hope!). 

Indeed, functions are quite useful. They reduce your code's length because you avoid re-writing the same code over and over. For example, this simple code:
```
int main(){
    string name;
    name = Mike;
    cout << "Hello " << name << endl;
    name = John;
    cout << "Hello " << name << endl;
    name = Doe;
    cout << "Hello " << name << endl;
}
```
can be shortened and more efficient by using a function:
```
void hello(string name){
    cout << "Hello " << name << endl;
}
    
int main(){
    string name;
    hello(Mike);
    hello(John);
    hello(Doe);
}
```

Man, aren't functions useful? Through using a simple function, we simplified the program into neater code. 

However, as useful as functions are, they can't do everything!

The idea behind using functions is **_writing code once._** But how would we write a function that can be used by **ints** and **doubles**? For example, in this function we will be doubling a number:
```
int double_num(int x){
    return 2*x;
}
```
Perfect! We can double any number, right? 
Well, sorry to break it to you, but you can't double every number with that function...

What would happen if we called the function like so:
```
    cout << double_num(2.7);
```
First off, you wouldn't get a compiler warning that you're trying to use a double in place of an int (this could lead to some painful debugging later on!). The output of this would actually be **4** and not **5.4**. This is because the double used, **2.7**, would be _rounded down_ to **2** because it would be casted as an **int** in the function.

So how would you fix this? Well, if you're just using functions, you're going to have to write another function but for doubles this time:
```
int double_num(int x){
    return 2*x;
}
double double_num(double x){
    return 2*x;
}
```
There, it's fixed! But that method just defeated the purpose of functions because we just wrote the same code **twice**!

But have no fear, there's a way to write the function once and have it be used by any type of value: **Templates**!

##What's a template?
Let's rewrite the **double_num** function into something more effiecient with a template:
```
template <typename TYPE>
TYPE double_num(TYPE x){
    return 2*x;
}
```
I know, I'm throwing a lot of new material at you right now, but let's break the code down into pieces to see what's going on.

The first line of code
```
template <typename TYPE>
```
tells the compiler: _Hey, this is a template!_ The word **TYPE** is just like any other variable. You can actually name it whatever you want! This will be demonstrated later. It allows the compiler and programmer to choose what the function/variable type will be. If you were to call this function, you would call it like normal:
```
cout << double_num(3) << endl;
cout << double_num(3.5) << endl;
```
The compiler will see that you are calling a function-template and would create two functions:
```
int double_num(int x){}
double double_num(double x){}
```
and use the appropriate function for the appropriate value type. When you run the program, you will get exactly what you wanted:
```
6
7
```
We have just succesfully used a template! It wasn't that hard was it? Templates are as complicated as you want to make them. If you want to use a template for a more sophisticated function, then the writing the template for the function might get a little messy.

##More Examples!
As stated before, templates are only as complicated as you make them. Templates can be used for many things ranging from simple functions (like the one we just made) to classes with member functions.

Here is a rough implementation of a `stack` data structure:
```
template < typename T >
class stack {
public:

  class Node {
  public:
    Node(T data, Node* n ) : data(data), next(n) {}
    T data;
    Node* next;
  };

  stack () : ttop(0) {}   // misspelling to prevent name collision 

  void push( T x ) {
    Node* temp = new Node(x,ttop);
    ttop = temp;
  }

  void pop() {
    Node* temp = ttop;
    ttop = ttop->next;
    delete temp;
  } 

  T top() {
    return ttop->data;
  }

private:

  Node* ttop;      

};

int main() {
  stack<int> s;
  s.push(1);
  s.push(2);
  s.push(3);
  cout << s.top() << endl;
  s.pop();
  cout << s.top() << endl;
  s.pop();
  cout << s.top() << endl;
  s.pop();
  return 0;
}
```

The template conviently allows us to use this data structure for any data type.
