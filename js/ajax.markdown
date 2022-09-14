- [原生AJAX](#原生ajax)
- [jQurey ajax](#jqurey-ajax)
  - [定义和用法](#定义和用法)
  - [语法](#语法)
- [Fetch](#fetch)
  - [支持的请求参数](#支持的请求参数)
  - [上传 JSON 数据](#上传-json-数据)
  - [上传文件](#上传文件)
  - [上传多个文件](#上传多个文件)
  - [逐行处理文本文件](#逐行处理文本文件)
  - [检测请求是否成功](#检测请求是否成功)
- [axious](#axious)
- [表单异步提交](#表单异步提交)
- [javascript Headers对象](#javascript-headers对象)
- [Body](#body)
- [XMLHttpRequest.upload](#xmlhttprequestupload)
- [ajax文件上传进度条XHR](#ajax文件上传进度条xhr)
- [jQuery 文件上传进度条](#jquery-文件上传进度条)
- [数据缓冲](#数据缓冲)
  - [arrayBuffer对象](#arraybuffer对象)
  - [typeArray](#typearray)
  - [DataView](#dataview)
- [FileReader](#filereader)
# 原生AJAX

# jQurey ajax
## 定义和用法
ajax() 方法用于执行 AJAX（异步 HTTP）请求。

所有的 jQuery AJAX 方法都使用 ajax() 方法。该方法通常用于其他方法不能完成的请求。
## 语法
> $.ajax({name:value, name:value, ... })

|名称|	值/描述|
| -- | -- |
|async	|布尔值，表示请求是否异步处理。默认是 true。|
|beforeSend(xhr)|	发送请求前运行的函数。|
|cache	|布尔值，表示浏览器是否缓存被请求页面。默认是 true。|
|complete(xhr,status)|	请求完成时运行的函数（在请求成功或失败之后均调用，即在 success 和 error 函数之后）。|
|contentType	|发送数据到服务器时所使用的内容类型。默认是："application/x-www-form-urlencoded"。|
|context	|为所有 AJAX 相关的回调函数规定 "this" 值。|
|data	|规定要发送到服务器的数据。|
|dataFilter(data,type)|	用于处理 XMLHttpRequest 原始响应数据的函数。|
|dataType|	预期的服务器响应的数据类型。|
|error(xhr,status,error)	|如果请求失败要运行的函数。|
|global|	布尔值，规定是否为请求触发全局 AJAX 事件处理程序。默认是 true。|
|ifModified|	布尔值，规定是否仅在最后一次请求以来响应发生改变时才请求成功。默认是 false。|
|jsonp|	在一个 jsonp 中重写回调函数的字符串。|
|jsonpCallback|	在一个 jsonp 中规定回调函数的名称。|
|password	|规定在 HTTP 访问认证请求中使用的密码。|
|processData	|布尔值，规定通过请求发送的数据是否转换为查询字符串。默认是 true。|
|scriptCharset|	规定请求的字符集。|
|success(result,status,xhr)|	当请求成功时运行的函数。|
|timeout|	设置本地的请求超时时间（以毫秒计）。|
|traditional	|布尔值，规定是否使用参数序列化的传统样式。|
|type|	规定请求的类型（GET 或 POST）。|
|url	|规定发送请求的 URL。默认是当前页面。|
|username|	规定在 HTTP 访问认证请求中使用的用户名。|
|xhr|	用于创建 XMLHttpRequest 对象的函数。|
# Fetch 
`Fetch API` 提供了一个 `JavaScript` 接口，用于访问和操纵 `HTTP` 管道的一些具体部分，例如请求和响应。它还提供了一个全局 `fetch()` 方法，该方法提供了一种简单，合理的方式来跨网络异步获取资源。

这种功能以前是使用 `XMLHttpRequest` 实现的。Fetch 提供了一个更理想的替代方案，可以很容易地被其他技术使用，例如  `Service Workers`。`Fetch` 还提供了专门的逻辑空间来定义其他与 `HTTP` 相关的概念，例如 `CORS` 和 `HTTP` 的扩展。

请注意，`fetch` 规范与 `jQuery.ajax()` 主要有以下的不同：

当接收到一个代表错误的 `HTTP 状`态码时，从 `fetch()` 返回的` Promise `不会被标记为 `reject`，即使响应的 `HTTP` 状态码是 `404 `或 `500`。相反，它会将` Promise` 状态标记为 `resolve `（如果响应的 `HTTP` 状态码不在 `200 - 299` 的范围内，则设置 `resolve` 返回值的 `ok` 属性为 `false` ），仅当网络故障时或请求被阻止时，才会标记为 `reject`。
`fetch` 不会发送跨域 `cookies`，除非你使用了 `credentials` 的初始化选项。（自2018 年 8 月以后，默认的 `credentials` 政策变更为 `same-origin`。`Firefox` 也在 `61.0b13` 版本中进行了修改）

一个基本的 fetch 请求设置起来很简单。看看下面的代码：
```js
fetch('http://example.com/movies.json')
  .then(response => response.json())
  .then(data => console.log(data));
```
这里我们通过网络获取一个 JSON 文件并将其打印到控制台。最简单的用法是只提供一个参数用来指明想 `fetch()` 到的资源路径，然后返回一个包含响应结果的` promise`（一个 `Response` 对象）。

当然它只是一个` HTTP` 响应，而不是真的 `JSON`。为了获取JSON的内容，我们需要使用 `json()` 方法（该方法返回一个将响应 `body` 解析成 JSON 的 `promise`）。
## 支持的请求参数
`fetch()` 接受第二个可选参数，一个可以控制不同配置的 `init` 对象：

参考 fetch()，查看所有可选的配置和更多描述。
```js
// Example POST method implementation:
async function postData(url = '', data = {}) {
  // Default options are marked with *
  const response = await fetch(url, {
    method: 'POST', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, *cors, same-origin
    cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'same-origin', // include, *same-origin, omit
    headers: {
      'Content-Type': 'application/json'
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
    redirect: 'follow', // manual, *follow, error
    referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
    body: JSON.stringify(data) // body data type must match "Content-Type" header
  });
  return response.json(); // parses JSON response into native JavaScript objects
}

postData('https://example.com/answer', { answer: 42 })
  .then(data => {
    console.log(data); // JSON data parsed by `data.json()` call
  });
```
注意：`mode: "no-cors"` 仅允许使用一组有限的 HTTP 请求头：

- `Accept`
- `Accept-Language`
- `Content-Language`
- `Content-Type` 允许使用的值为：`application/x-www-form-urlencoded、multipart/form-data` 或 `text/plain`
## 上传 JSON 数据
使用 fetch() `POST` JSON数据
```js
const data = { username: 'example' };

fetch('https://example.com/profile', {
  method: 'POST', // or 'PUT' 'GET'
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(data),
})
.then(response => response.json())
.then(data => {
  console.log('Success:', data);
})
.catch((error) => {
  console.error('Error:', error);
});
```
## 上传文件

可以通过 HTML <input type="file" /> 元素，FormData() 和 fetch() 上传文件。
```js
const formData = new FormData();
const fileField = document.querySelector('input[type="file"]');

formData.append('username', 'abc123');
formData.append('avatar', fileField.files[0]);

fetch('https://example.com/profile/avatar', {
  method: 'PUT',
  body: formData
})
.then(response => response.json())
.then(result => {
  console.log('Success:', result);
})
.catch(error => {
  console.error('Error:', error);
});
```
## 上传多个文件
可以通过 HTML <input type="file" multiple /> 元素，FormData() 和 fetch() 上传文件。
```js
const formData = new FormData();
const photos = document.querySelector('input[type="file"][multiple]');

formData.append('title', 'My Vegas Vacation');
for (let i = 0; i < photos.files.length; i++) {
  formData.append(`photos_${i}`, photos.files[i]);
}

fetch('https://example.com/posts', {
  method: 'POST',
  body: formData,
})
.then(response => response.json())
.then(result => {
  console.log('Success:', result);
})
.catch(error => {
  console.error('Error:', error);
});
```
## 逐行处理文本文件
从响应中读取的分块不是按行分割的，并且是 `Uint8Array` 数组类型（不是字符串类型）。如果你想通过 `fetch()` 获取一个文本文件并逐行处理它，那需要自行处理这些复杂情况。以下示例展示了一种创建行迭代器来处理的方法（简单起见，假设文本是 `UTF-8` 编码的，且不处理 `fetch()` 的错误）。
```js
async function* makeTextFileLineIterator(fileURL) {
  const utf8Decoder = new TextDecoder('utf-8');
  const response = await fetch(fileURL);
  const reader = response.body.getReader();
  let { value: chunk, done: readerDone } = await reader.read();
  chunk = chunk ? utf8Decoder.decode(chunk) : '';

  const re = /\n|\r|\r\n/gm;
  let startIndex = 0;
  let result;

  for (;;) {
    let result = re.exec(chunk);
    if (!result) {
      if (readerDone) {
        break;
      }
      let remainder = chunk.substr(startIndex);
      ({ value: chunk, done: readerDone } = await reader.read());
      chunk = remainder + (chunk ? utf8Decoder.decode(chunk) : '');
      startIndex = re.lastIndex = 0;
      continue;
    }
    yield chunk.substring(startIndex, result.index);
    startIndex = re.lastIndex;
  }
  if (startIndex < chunk.length) {
    // last line didn't end in a newline char
    yield chunk.substr(startIndex);
  }
}

async function run() {
  for await (let line of makeTextFileLineIterator(urlOfFile)) {
    processLine(line);
  }
}

run();
```
## 检测请求是否成功
如果遇到网络故障或服务端的 CORS 配置错误时，fetch() promise 将会 reject，带上一个 TypeError 对象。虽然这个情况经常是遇到了权限问题或类似问题——比如 404 不是一个网络故障。想要精确的判断 fetch() 是否成功，需要包含 promise resolved 的情况，此时再判断 Response.ok 是否为 true。类似以下代码：
```js
fetch('flowers.jpg')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not OK');
    }
    return response.blob();
  })
  .then(myBlob => {
    myImage.src = URL.createObjectURL(myBlob);
  })
  .catch(error => {
    console.error('There has been a problem with your fetch operation:', error);
  });
```
# axious

# 表单异步提交

# javascript Headers对象
使用 `Headers` 的接口，你可以通过 `Headers()` 构造函数来创建一个你自己的 `headers` 对象。一个 headers 对象是一个简单的多键值对
```js
const content = 'Hello World';
const myHeaders = new Headers();
myHeaders.append('Content-Type', 'text/plain');
myHeaders.append('Content-Length', content.length.toString());
myHeaders.append('X-Custom-Header', 'ProcessThisImmediately');
```
也可以传入一个多维数组或者对象字面量：
```js
const myHeaders = new Headers({
  'Content-Type': 'text/plain',
  'Content-Length': content.length.toString(),
  'X-Custom-Header': 'ProcessThisImmediately'
});
```
它的内容可以被获取：
```js
console.log(myHeaders.has('Content-Type')); // true
console.log(myHeaders.has('Set-Cookie')); // false
myHeaders.set('Content-Type', 'text/html');
myHeaders.append('X-Custom-Header', 'AnotherValue');

console.log(myHeaders.get('Content-Length')); // 11
console.log(myHeaders.get('X-Custom-Header')); // ['ProcessThisImmediately', 'AnotherValue']

myHeaders.delete('X-Custom-Header');
console.log(myHeaders.get('X-Custom-Header')); // null
```
# Body
不管是请求还是响应都能够包含 body 对象。body 也可以是以下任意类型的实例。

- ArrayBuffer
- ArrayBufferView (Uint8Array等)
- Blob/File
- string
- URLSearchParams
- FormData

Body 类定义了以下方法（这些方法都被 Request 和 Response所实现）以获取 body 内容。这些方法都会返回一个被解析后的 Promise 对象和数据。
```js
Request.arrayBuffer() (en-US) / Response.arrayBuffer()
Request.blob() (en-US) / Response.blob()
Request.formData() (en-US) / Response.formData()
Request.json() (en-US) / Response.json()
Request.text() (en-US) / Response.text()
```
相比于XHR，这些方法让非文本化数据的使用更加简单。

请求体可以由传入 body 参数来进行设置：
```js
const form = new FormData(document.getElementById('login-form'));
fetch('/login', {
  method: 'POST',
  body: form
});
```
request 和 response（包括 fetch() 方法）都会试着自动设置 Content-Type。如果没有设置 Content-Type 值，发送的请求也会自动设值。
# XMLHttpRequest.upload
`XMLHttpRequest.upload` 属性返回一个 `XMLHttpRequestUpload`对象，用来表示上传的进度。这个对象是不透明的，但是作为一个`XMLHttpRequestEventTarget`，可以通过对其绑定事件来追踪它的进度。

可以被绑定在upload对象上的事件监听器如下：

|事件|	相应属性的信息类型|
|---|---|
|onloadstart	|获取开始|
|onprogress|	数据传输进行中|
|onabort	|获取操作终止|
|onerror	|获取失败|
|onload|	获取成功|
|ontimeout|	获取操作在用户规定的时间内未完成|
|onloadend	|获取完成（不论成功与否）|
# ajax文件上传进度条XHR
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>上传文件(显示进度与速度)</title>
    <style>
        .boxFile {
            width: 645px;
            height: 500px;
        }
        img {
            width: 100%;
            height: 100%;
        }
    </style>
</head>

<body>
    <input type="file" class="myfile">
    进度<progress max="100" value="0"></progress>
    <span class="persent">0%</span>
    速度 <span class="speed">0b/s</span>
    <button class="btn">上传文件</button>
    <button class="btn">取消文件</button>
    <div class="boxFile"></div>
    <script>
        // XMLHttpRequest 对象用于在后台与服务器交换数据。
        // 实例化 XMLHttpRequest 对象
        let xhr = new XMLHttpRequest();
        let stime; //存储时间
        let sloaded; // 存储上传的内容
        let btns = document.querySelectorAll(".btn");
        // 点击"上传文件"
        btns[0].onclick = function () {
            let file = document.querySelector(".myfile").files[0];//
            console.log(file);
            let form = new FormData(); //实例化FormData对象
            
            // 向form中添加新的属性值，如果对应的属性值存在也不会覆盖原值，而是新增一个值，
            // 如果属性不存在则新增一项属性值。
            form.append("myfiles", file); 

            xhr.open("post", "/fileUpload",true); //初始化请求

            xhr.onload = function () {
                console.log(xhr.responseText);
            }
            /* onreadystatechange：存有处理服务器响应的函数，每当 readyState 改变时，
               函数就会被执行。readyState：存有服务器响应的状态信息。status存响应状态码*/
            xhr.onreadystatechange = function () {
                if (xhr.readyState == 4 && xhr.status == 200) {
                    let resText = xhr.responseText;
                    let imgEl = document.createElement("img");
                    imgEl.src = resText;
                    imgEl.onload = function () {
                        // document.body.appendChild(this);//上传图片后，页面会出现两张一样的图片
                        document.querySelector(".boxFile").appendChild(this); //img标签外面加一个父级div标签就好了
                    }
                }
            }

            xhr.upload.onloadstart = function () {
                console.log("开始上传");
                stime = new Date().getTime();
                sloaded = 0;
            }
            xhr.upload.onprogress = function (evt) {
                console.log("上传中......");
                // 当前上传文件的文件大小：evt.loaded
                // 需要上传的总文件的大小：evt.total
                // 上传进度
                //上传文件的百分比
                //toFixed(参数)：保留几位小数  参数是整型的数值，数值是几就保留几位小数
                let persent = ((evt.loaded / evt.total) * 100).toFixed(0);   
                document.querySelector("progress").value = persent;
                document.querySelector(".persent").innerHTML = persent + "%";

                // 上传速度
                // 上传结束时间
                let endTime = new Date().getTime();

                // 1、时间差（时间段）
                let dTime = (endTime - stime) / 1000; //毫秒换算成秒

                // 2、当前时间段内上传的文件大小
                let dloaded = evt.loaded - sloaded;

                // 3、当前速度= 当前上传文件的内容/当前时间差
                let speed = dloaded / dTime;

                // 重置上传开始时间与文件大小(分时段上传的)
                stime = new Date().getTime();
                sloaded = evt.loaded;

                // 基本单位：字节b
                // 1mb=1024kb    1kb=1024b
                // b/s
                let unit = "b/s"; //不足1kb，以b/s来计算速度
                // kb/s
                if (speed / 1024 > 1) { //当大小超过1024b(即1kb时)，以kb/s来计算速度
                    unit = "kb/s";
                    speed = speed / 1024;
                }
                // mb/s  
                if (speed / 1024 > 1) { //当大小超过1024Kb(即1mb时)，以mb/s来计算速度
                    unit = "mb/s";
                    speed = speed / 1024;
                }
                document.querySelector(".speed").innerHTML = speed.toFixed(2) + unit;
            }
            xhr.upload.onload = function () {
                console.log("上传成功");
            }
            xhr.upload.onerror = function () {
                console.log("上传失败");
            }
            xhr.send(form);//发送请求
        }
        // 点击"取消上传"
        // xhr.abort()：终止请求，调用后即可取消文件上传操作
        btns[1].onclick = function () {
            xhr.abort();
        }
    </script>
</body>
</html>
```
# jQuery 文件上传进度条
```js
$(function(){
	// 图片上传. 
            $("#upload").click(function() {
                if(!validUpload()) return; // 验证图片格式。
                $("#file").upload({
                    action: hy.url+"/upload/uploadTeacherIMG.action",
                    oncomplete: function(result) {
                        var mess = result.responseText;
                        // 上传成功处理.更换页面显示图片。
                        if(result.status == 200){
                            $("#teacherImg").attr("src",hy.url+"/FileTemp/"+mess.split(":")[1]);
                             $.ligerDialog.success("图片已上传成功,请完善教师其它信息.");
                         }else 
                             $.ligerDialog.error(mess); 
                    },onprogress: function(e) {
                        $("#progress").html("上传进度:"+(e.loaded / e.total *100 ).toFixed(2) + "%");
                    }
                });
            });
      });
```
# 数据缓冲
## arrayBuffer对象
## typeArray
 |类型  |	单个元素值的范围	|大小(bytes)	|描述|	Web IDL 类型	|C 语言中的等价类型|
 | --|--|--|--|--|--|
|Int8Array	|-128 到 127	|1	|8 位二进制有符号整数|	byte|	int8_t|
|Uint8Array|	0 到 255	|1	|8 位无符号整数（超出范围后从另一边界循环）|	octet| uint8_t|
|Uint8ClampedArray|	0 到 255	|1|	8 位无符号整数（超出范围后为边界值）|	octet	|uint8_t|
|Int16Array|	-32768 到 32767|	2	|16 位二进制有符号整数	|short|	int16_t|
|Uint16Array|	0 到 65535|	2	|16 位无符号整数	|unsigned short|uint16_t|
|Int32Array|	-2147483648 到 2147483647	|4	|32 位二进制有符号整数	|long|	int32_t|
|Uint32Array|	0 到 4294967295|	4|	32 位无符号整数	|unsigned long|	uint32_t|
|Float32Array |	-3.4E38 到 3.4E38 最小正数为：1.2E-38|	4	|32 位 IEEE 浮点数（7 位有效数字，如 1.1234567）	|unrestricted float|	float|
|Float64Array	|-1.8E308 到 1.8E308 最小正数为：5E-324|	8	|64 位 IEEE 浮点数（16 有效数字，如 1.123...15)	|unrestricted double|	double|
|BigInt64Array|	-2^63 到 2^63-1|	8	|64 位二进制有符号整数|	bigint|	int64_t (signed long long)|
|BigUint64Array|	0 到 2^64 - 1	|8	|64 位无符号整数|	bigint|	uint64_t (unsigned long long)|
## DataView
# FileReader
用于读取Blob