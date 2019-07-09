#1. es6 新增let 和 const

+ let 用来声明变量
+ const 用来声明常量
  
#2. let 和const 声明的变量和常量时块级作用域（{}）

#3. let 和 const 声明的变量在同一个作用域中不允许有同名的变量或常量名称。

        let a;

        let a=123;

        // 报错
        Uncaught SyntaxError: Identifier 'a' has already been declared

        var b;
        let b;
        Uncaught SyntaxError: Identifier 'b' has already been declared

        和参数重名
        function s(a){
            let a=123;
        }

    VM1838:2 Uncaught SyntaxError: Identifier 'a' has already been declared

        let c=0;
        function d(){
            let c=123;
            console.log(c)
        }

        function show(a){
	
            if(a=='3'){
                let b=23
                console.log(b)
            }else{
                let b=44;
                console.log(b)
            }
        }

        show(5)=====>44

**注意** 

+ const 和 let 是块级（{}）作用域
+ 同级作用域内不允许出现同名的变量或常量

#4. let 和 const 声明的变量不会提升，要使用变量，必须要先声明变量不然会报错

        console.log(dddd)
        let dddd=123

        VM393:1 Uncaught ReferenceError: Cannot access 'dddd' before initialization
        at <anonymous>:1:13

#5. let 和 const 声明的全局变量不会覆盖window 下的变量。

        window.String
        ƒ String() { [native code] }

        let String =123;

        String======>123

        window.String
        ƒ String() { [native code] }

        而var 声明的变量会覆盖window 全局变量

        var String=123;

        String=======>123

        window.String====>123;

#6. let 和 const 存在暂时死区