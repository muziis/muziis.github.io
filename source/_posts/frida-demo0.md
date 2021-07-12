---
title: frida_demo0
date: 2021-07-12 15:50:32
tags:
---

### APK

frida/demo0/android1.1.apk

### F0

```js
Java.perform(function(){
    var ChooseClass = Java.use("com.example.androiddemo.Activity.LoginActivity");
    console.log(ChooseClass)
    ChooseClass.a.overload('java.lang.String', 'java.lang.String').implementation = function(str1, str2){
        console.log(str1)
        console.log(str2)
        console.log(this.a(str1, str2))
        return str1
    }
})
```

### F1

```JS
Java.perform(function () {
    var ChooseClass = Java.use("com.example.androiddemo.Activity.FridaActivity1");
    console.log(ChooseClass)
    ChooseClass.a.implementation = function (a) {
        return "R4jSLLLLLLLLLLOrLE7/5B+Z6fsl65yj6BgC6YWz66gO6g2t65Pk6a+P65NK44NNROl0wNOLLLL="
    }
})
```

### F2

```js
Java.perform(function () {
    var ChooseClass = Java.use("com.example.androiddemo.Activity.FridaActivity2");
    console.log(ChooseClass)
    ChooseClass.onCheck.implementation = function () {
        this.setBool_var()
        this.setStatic_bool_var()
        this.onCheck()
    }
})
```

### F3

```js
Java.perform(function () {
    var ChooseClass = Java.use("com.example.androiddemo.Activity.FridaActivity3")
    console.log(ChooseClass)

    ChooseClass.static_bool_var.value = true;

    Java.choose("com.example.androiddemo.Activity.FridaActivity3", {
        onMatch: function (instance) {
            instance.bool_var.value = true;
            instance._same_name_bool_var.value = true;
            instance.same_name_bool_var()
            instance.onCheck()
        },
        onComplete: function () {
            console.log("complete!");
        }
    })
})
```

### F4

```js
Java.perform(function () {
    var ChooseClass = Java.use("com.example.androiddemo.Activity.FridaActivity4$InnerClasses")
    console.log(ChooseClass)

    ChooseClass.check1.implementation = function () {
        return true
    }
    ChooseClass.check2.implementation = function () {
        return true
    }
    ChooseClass.check3.implementation = function () {
        return true
    }
    ChooseClass.check4.implementation = function () {
        return true
    }
    ChooseClass.check5.implementation = function () {
        return true
    }
    ChooseClass.check6.implementation = function () {
        return true
    }
})
```

### F5

```js
Java.perform(function () {
    Java.enumerateClassLoaders({
        onMatch: function (loader) {
            try {
                // loadClass or findClass
                if (loader.loadClass("com.example.androiddemo.Dynamic.DynamicCheck")) {
                    Java.classFactory.loader = loader;
                    var hookClass = Java.use("com.example.androiddemo.Dynamic.DynamicCheck");
                    console.log("success hook it :", hookClass);
                    // something to do;

                    hookClass.check.implementation = function () {
                        console.log("hook check")
                        return true
                    }
                }
            } catch (error) {
                // pass
            }
        },
        onComplete: function () {
            console.log("complete !!! ")
        }
    })
})
```

### F6

```js
Java.perform(function () {
    var ChooseClass0 = Java.use("com.example.androiddemo.Activity.Frida6.Frida6Class0")
    console.log(ChooseClass0)
    ChooseClass0.check.implementation = function () {
        console.log(this.check())
        return true
    }

    var ChooseClass1 = Java.use("com.example.androiddemo.Activity.Frida6.Frida6Class1")
    console.log(ChooseClass1)
    ChooseClass1.check.implementation = function () {
        console.log("hook1")
        console.log(this.check())
        return true
    }

    var ChooseClass2 = Java.use("com.example.androiddemo.Activity.Frida6.Frida6Class2")
    console.log(ChooseClass2)
    ChooseClass2.check.implementation = function () {
        console.log(this.check())
        return true
    }
})
```
