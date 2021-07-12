---
title: frida_demo1
date: 2021-07-12 15:50:37
tags:
---

### APK

frida/demo1/1.apk

frida/demo1/1.apk

frida/demo1/1.apk

### 1.apk

```js
/* 观察源码得出输入的字符串被限定为 5 位且字符串内容为数字类型
 * 从 10000 - 99999 进行遍历调用原函数, 直到内容返回为 true 为止 */

Java.perform(function () {
    Java.use("com.kanxue.pediy1.VVVVV").VVVV.implementation = function (listener, input) {
        for (var i = 10000; i < 99999; i++) {
            var s = i.toString()
            console.log(s)
            var result = this.VVVV(listener, s);
            if (result == true) {
                console.log('flag is ', s)
                return result
            }
        }
    }
})

```

### 2.apk

```js
/* 基于第一题新增了动态加载 dex
 * 需要先遍历 ClassLoader 选择正确的 classLoader 在对函数进行主动调用
 * 算法与规则跟第一题一样, 主动调用进行爆破即可得到
 * 在 jadx 中看到有 stringFromJNI 调用, 使用 IDA 打开发现该函数对结果进行了 int 转换并且自增 1 并返回结果
 * 所在对结果所得数字减 1 即可获得最终结果*/

Java.perform(function () {
    Java.enumerateClassLoaders({
        onMatch: function (loader) {
            if (loader.toString().indexOf("dalvik.system.DexClassLoader") > -1) {
                console.log(loader.toString())
                Java.classFactory.loader = loader;
                hookFunc()
                return
            }
        }, onComplete: function () {
            console.log("complete!")
        }
    })
})

function hookFunc() {
    Java.perform(function () {
        for (var i = 10000; i < 99999; i++) {
            var newInput = Java.use("java.lang.String").$new(i.toString())
            console.log(newInput)

            var result = Java.use("com.kanxue.pediy1.VVVVV").VVVV(newInput);
            if (result == true) {
                console.log('flag is ', s)
                return result
            }
        }
    })
}
```

### 3.apk

```js
/* 基于第二题新增了 frida-server 反调试 
 * hook pthread_create 函数后直接使用第二题的 Hook 文件即可 */

Java.perform(function () {
    hook_pthread_create()
})

function hook_pthread_create() {
    var pt_create_func = Module.findExportByName(null, 'pthread_create');
    var detect_frida_loop_addr = null;
    console.log('pt_create_func:', pt_create_func);

    Interceptor.attach(pt_create_func, {
        onEnter: function () {
            if (detect_frida_loop_addr == null) {
                var base_addr = Module.getBaseAddress('libnative-lib.so');
                if (base_addr != null) {
                    detect_frida_loop_addr = base_addr.add(0xe9c)
                    console.log('this.context.x2: ', detect_frida_loop_addr, this.context.x2);
                    if (this.context.x2.compare(detect_frida_loop_addr) == 0) {
                        hook_anti_frida_replace(this.context.x2);
                    }
                }
            }
        },
        onLeave: function (retval) {
            // console.log('retval',retval);
        }
    })
}

function hook_anti_frida_replace(addr) {
    console.log('replace anti_addr :', addr);
    Interceptor.replace(addr, new NativeCallback(function (a1) {
        console.log('replace success');
        return;
    }, 'pointer', []));
}
```

