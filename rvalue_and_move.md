# lvalue, rvalue and std::move
Basically, std::move converts a lvalue object to a rvalue object.  
Rvalue is a temporary value.
```cpp
string a = "bbb";
vector<string> v;
v.push_back(std::move(a));
```
After this operation, the value of a is "" and "bbb" is tranformed into a rvalue and push back to the vector.  
Notice that,
```cpp
template <typename T>
class Temp {
  Temp(T&& in_);
};
```
In this example, T&& means it can be a rvalue reference or lvalue reference.