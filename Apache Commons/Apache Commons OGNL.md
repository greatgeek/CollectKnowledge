#### Apache Commons OGNL - Object Graph Navigation Library

OGNL stands for Objec-Graph Navigation Language; it is an expression language for getting and setting properties of Java objects, plus other extras such as list projection and selection and lambda expressions. You use the same expression for both getting and setting the value of a property.

The ```Ognl``` class contains convenience methods for evaluating OGNL expressions. You can do this in two stages, parsing an expression into internal form and then using that internal form to either set or get the value of a property; or you can do it in a single stage, and get or set a property using the String form of the expression directly.

OGNL started out as a way to set up associations between UI components and controllers using property names. As the      desire for more complicated associations grew, Drew Davidson created what he called KVCL, for Key-Value Coding      Language, egged on by Luke Blanshard. Luke then reimplemented the language using ANTLR, came up with the new name,      and, egged on by Drew, filled it out to its current state. Later on Luke again reimplemented the language using      JavaCC. Further maintenance on all the code is done by Drew (with spiritual guidance from Luke).      

We pronounce OGNL as a word, like the last syllables of a drunken pronunciation of "*orthogonal*".      

#### Introduction

Many people have asked exactly what OGNL is good for. Several of the uses to which OGNL has been applied are:

* A binding language between GUI elements (textfield, combobox, etc.) to model objects. Transformations are made easier by OGNL's TypeConverter mechanism to convert values from one type to another(String to numeric types, for example).
* ​        A data source language to map between table columns and a Swing TableModel;        
* ​        A binding language between web components and the underlying model objects;        
* ​        A more expressive replacement for the property-getting language used by the Apache Commons BeanUtils package or        JSTL's EL (which only allow simple property navigation and rudimentary indexed properties).        

​      Most of what you can do in Java is possible in `OGNL`, plus other extras such as list *projection*,      *selection* and *lambda expressions*.      

#### 参考来源

[commons #OGNL](https://commons.apache.org/proper/commons-ognl/index.html)