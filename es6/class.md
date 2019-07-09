#1.es6 中新增类声明，类实际上是个“特殊的函数”，类似函数的语法糖

构造函数是可选的

        class ClassName{

            // 构造函数
            constructor(){

            }
        }

#2. 声明类的多种方式

+ 类声明

        // 默认无参构造函数
        class My{}
        // 自定义无参构造函数
        class My{
            constructor(){

            }
        }

        // 定义有参构造函数值
        class My{
            constructor(width,height){

            }
        }

+ 类表达式
  
        let My=class {

        }

        let My=class{
            constructor(){

            }
        }

        let My=class{
            constructor(h,w){

            }
        }
+ 具名类表达式

        let My=class xx{

        }
**注意**

具名类表达式，xx 只能用于类的内部，在外面只能通过My访问

#3.new 实例化类

> 使用new 实例化调用的是类的构造函数


>如果构造函数没有参数，实例化时也不用传入参数，

>如果有参数，如果不传入参数，参数值为undefined(传参规则参照函数规则)

>一个类只能拥有一个名为 “constructor”的特殊方法。

>如果没有提供constructor构造函数，则会使用默认的无参构造函数

        let My=class{
            constructor(){

            }
        }

        // 实例化
        let m=new My()

        class MyTest{
            constructor(w,h){
                this.width=w;
                this.height=h;
            }
        }

        //实例化
        let x=new MyTest(200,34)
        {width:200,height:34}
        
        let y=new MyTest(200)
        {width:200,height:undefined}

        let z=new MyTest()
        {width:undefined,height:undefined}

#4.class 和let，const 一样，不会声明提升，同一作用域不允许声明相同名称的类。

Uncaught SyntaxError: Identifier 'My' has already been declared
    at <anonymous>:1:1

#5.实例变量，在构造函数中通过this.varibleName 定义

        class My{
            constructor(){
                this.name='张三'
            }
        }

        var x=new My();
        console.log(x.name)====>张三

**注意** 

通过this 创建的变量为实例变量，this 指向实例对象

#6.静态变量

+ 使用类名

        class My(){
            
        }
        My.name='my'
        My.a=function(){}
+ 使用static

        class My(){

            static a(){

            }

            static b=132;
        }

**注意**

静态成员只能通过类名访问，不能使用实例对象使用。

#7.使用extends 继承

        class Y exends My{}

        let Y=class extends My{}

#8.super 调用父类的构造函数

+ 如果子类中没有自定义构造函数，可以不调用super 

+ 如果子类中有自定义构造函数，则必须在this 使用之前调用
  
  super 初始化this 

        class Z extends My4{

            constructor(){

            }
        }

        var z=new Z()
        // 报错
        VM1565:3 Uncaught ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        at new Z (<anonymous>:3:17)
        at <anonymous>:1:7

        class Z1 extends My4{

            constructor(){
                this.name=123;
            }
        }
        
        var z=new Z1()
        // 报错
        VM1565:3 Uncaught ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        at new Z (<anonymous>:3:17)
        at <anonymous>:1:7

        class Z2 extends My4{

            constructor(){
            
                this.name=123;
                super()
            }
        }
        
        var z=new Z2()
        // 报错
        VM1565:3 Uncaught ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        at new Z (<anonymous>:3:17)
        at <anonymous>:1:7

        // 如果有自定义构造函数，则必须先实现super
        class Z3 extends My4{

            constructor(){
                super()
                this.name=123;
                
            }
        }       
        
        var z=new Z3()
        undefined
        z.name
        123


#9. 继承：

+ 子类可以拥有父类的一切
+ 子类中如果有和父类同名的成员（变量，方法），那么会覆盖父类的成员
