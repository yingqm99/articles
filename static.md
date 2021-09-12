# Static
## Static variables in global scope
In global scope, if a varaible is declared as static, it can not be accessed outside the file where it declares in.   
Important: Notice that for a non-const global variable, it is declared as static by default. In contrast, a const global variable is declared as non-static by default.
## Static member variables
When a member in a class is declared as static, it has only one piece of that static variable no matter how many instances of the class have been declared.
### Non-const Static variables
Also, for a non-const static member variable, it can only be initialized independently, which means that you cannot initialize it in a constructor.
### Const Static variables
Initialization is the same as other const member variables initialization.
## Static variables in local scope
In function local scope, if a variable is declared as static, it changes the duration of the variable from automatic to static and its life time is the entire duration of the program.   
Important: Notice that the value of the static variables in local scope is initialized only once prior to program start up.  
In unnamed local block, the static variables cannot be accessed outside the block, which means that its lifetime is only with in block.  
## Static functions
For static functions, similar to static variables in global cope, when is declared as static, cannot be used within a file outside of this definition.
## Static member functions
Static member functions can only access static member variables of a class.