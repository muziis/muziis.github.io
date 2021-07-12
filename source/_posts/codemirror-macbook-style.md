---
title: codemirror_macbook_style
date: 2021-07-12 15:44:22
tags:
---

### CSS

```css
@font-face {
    font-family: 'Hack';
    font-display: swap;
    src: url('https://cdn.jsdelivr.net/font-hack/2.020/fonts/woff2/hack-regular-webfont.woff2?v=2.020') format('woff2');
}

.CodeMirror {
    font-family: Hack;
    font-size: 12px;
    line-height: 150%;
}

.codeWindows {
    background-color: #282a36 !important;
    padding: 12px 18px 18px 16px;
    border-radius: 5px;
    box-shadow: rgb(0 0 0 / 55%) 0px 20px 25px;
    width: 60%;
    margin: 30px auto;
}

.codeBlockTitle {
    margin-top: 25px;
    text-align: center;
    font-family: Hack;
    font-weight: bold;
}
```

### JS

```js
function uuid2(len, radix) {
    // 获取 uuid
    var chars = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'.split('');
    var uuid = [],
        i;
    radix = radix || chars.length;

    if (len) {
        // Compact form
        for (i = 0; i < len; i++) uuid[i] = chars[0 | Math.random() * radix];
    } else {
        // rfc4122, version 4 form
        var r;

        // rfc4122 requires these characters
        uuid[8] = uuid[13] = uuid[18] = uuid[23] = '-';
        uuid[14] = '4';

        // Fill in random data.  At i==19 set the high bits of clock sequence as
        // per rfc4122, sec. 4.1.5
        for (i = 0; i < 36; i++) {
            if (!uuid[i]) {
                r = 0 | Math.random() * 16;
                uuid[i] = chars[(i == 19) ? (r & 0x3) | 0x8 : r];
            }
        }
    }

    return uuid.join('');
}

function makeCodeMirror(code, title) {
    let uuid = uuid2(16, 16)
    // 插入标签
    $("body").append(
        '<div class="codeBlockTitle">' +
        '   <span>' + title + '</span>' +
        '</div>' +
        '<div class="codeWindows">' +
        '    <div style="margin-bottom: 10px; margin-left: 3px;">' +
        '        <svg xmlns="http://www.w3.org/2000/svg" width="54" height="14" viewBox="0 0 54 14">' +
        '            <g fill="none" fill-rule="evenodd" transform="translate(1 1)">' +
        '                <circle cx="6" cy="6" r="6" fill="#FF5F56" stroke="#E0443E" stroke-width=".5"></circle>' +
        '                <circle cx="26" cy="6" r="6" fill="#FFBD2E" stroke="#DEA123" stroke-width=".5"></circle>' +
        '                <circle cx="46" cy="6" r="6" fill="#27C93F" stroke="#1AAB29" stroke-width=".5"></circle>' +
        '            </g>' +
        '        </svg>' +
        '    </div>' +
        '    <textarea id="' + uuid + '"></textarea>' +
        '</div>'
    )
    // 实例化代码块
    codeMirror = CodeMirror.fromTextArea(document.getElementById(uuid), {
        mode: "python", // 语言模式
        smartIndent: true, // 智能缩进
        indentUnit: 4, // 智能缩进单位为4个空格长度
        indentWithTabs: true, // 使用制表符进行智能缩进
        theme: "dracula"  // 主题
    })
    codeMirror.setSize('auto', '100%')
    codeMirror.setOption("value", code)
    codeMirror.setOption("readOnly", true)
    return codeMirror
}
```

### HTML

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.59.4/codemirror.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.59.4/mode/python/python.min.js"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.59.4/codemirror.min.css">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.59.4/theme/dracula.min.css">
```

### Usage

```js
window.onload = function () {
    makeCodeMirror(
        "$ Hello World", "Hello World"
    )
}
```

