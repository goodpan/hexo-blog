---
title: HTML表单详解
date: 2016-11-02 10:41:48
tags: form
categories: HTML
---
# 概念

通常，HTML 表单的作用的是将数据发送到服务器上。服务器会处理这些数据并将结果返回给用户。这个过程似乎很容易，但是必须时刻注意要确保这些数据不能够破坏服务器或者给用户带来麻烦。

# 表单<form>

HTML的表单元素标签为<form></form>，它定义了数据将以怎样的方式发送到服务器。当按下“发送”按钮的时候，它将产生一个到服务器请求。需要牢记它的action和method方法。

## action

这个方法定义了数据将被发送到的位置。它的值必须是一个合法的URL地址，如果没有包含该属性，数据将被发送到当前页面的地址。
``` bash
<form action="http://goodpan.github.io">
```
许多旧的编写方式如下所示，加上#来指示数据应该被发送到当前页面，这在HTML5出现之前都是必须要有的，但是现在不再需要了。
``` bash
<form action="#">
```
## method方法

这个属性定义了数据是如何发送的。HTTP协议提供了一些处理请求的方法。HTML表单数据可以通过至少两种方式来发送：GET和POST方法。
为了理解这两种方法的差别，我们回过头分析下HTTP是如何工作的。每次你想在互联网上取得一个资源，浏览器就会发送一次请求。一个HTTP请求包含了两部分：一个“请求头”：包含了浏览器所具有能力的集合；还有一个“body”：包含了希望服务器可以处理的各种数据信息。

1.GET方法
GET方法用来让浏览器向服务器请求并索取一个资源。在这种情况下，浏览器会发送一个空的body，由于body是空的，如果发送表单也是采用这种方式的话，数据将会以增加的方式追加到URL地址后面。
``` bash
<form action="http://foo.com" method="get">
  <input name="say" value="Hi">
  <input name="to" value="Mom">
  <button>Send my greetings</button>
</form>
```
本次请求的结构如下所示：
``` bash
GET /?say=Hi&to=Mom HTTP/1.1
Host: foo.com
```
2.POST请求
``` bash
<form action="http://foo.com" method="post">
  <input name="say" value="Hi">
  <input name="to" value="Mom">
  <button>Send my greetings</button>
</form>
```
我们使用了POST方式来发送数据，本次HTTP请求结构如下：
``` bash
POST / HTTP/1.1
Host: foo.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 13

say=Hi&to=Mom
```
Content-Length 头指定了body的大小，Content-Type头指定了发送到服务器资源的类型。

3.GET和POST区别
1). 如果你需要发送密码（或者其它敏感数据），请不要使用GET方法否则你会将密码展示到URL中。
2). 如果你要发送大量数据，推荐使用POST方法，因为一些浏览器限制了URL的大小以及长度。
无论你采用哪种HTTP方法，服务端都会将接收到的字符串解析成一对一对的键值对，以便获取数据。
每次你发送数据到服务端，你都需要考虑安全问题。HTML表单是攻击服务端的首选对象，这个问题并不是由HTML表单产生的，而是由服务端采用了不合理的处理方式而导致的。

# 表单元素
## input
``` bash
<input type='text' name='' placeholder='' value=''>
<input type="text" pattern="^cherry|banana$">
<input type="email" multiple>
<input type="password">
<input type="search">
<input type="tel">
<input type="url">
<input type="radio" checked>
<input type="range" min="1" max="5" step="1">
<input type="date">
<input type="datetime-local">
<input type="month">
<input type="time">
<input type="week">
<input type="color">
<input type="hidden" name="timestamp" value="1286705410">
<input type="image" alt="Click me!" src="my-img.png" width="80" height="30" />
<input id="fileItem" type="file">
```

## textarea
``` bash
<textarea cols="20" rows="10"></textarea>
```
## select
``` bash
<select>
  <option>Banana</option>
  <option>Cherry</option>
  <option>Lemon</option>
</select>
```
``` bash
<select>
  <optgroup label="fruits">
    <option>Banana</option>
    <option selected>Cherry</option>
    <option>Lemon</option>
  </optgroup>
  <optgroup label="vegetables">
    <option>Carrot</option>
    <option>Eggplant</option>
    <option>Potatoe</option>
  </optgroup>
</select>
```
## button
``` bash
<button type="button">
    This an <br><strong>anonymous button</strong>
</button>
<input type="button" value="This is an anonymous button">
```
``` bash
<button type="submit">
    This a <br><strong>submit button</strong>
</button>
<input type="submit" value="This is a submit button">
```
``` bash
<button type="reset">
    This a <br><strong>reset button</strong>
</button>
<input type="reset" value="This is a reset button">
```
## Meters and progress
``` bash
<progress max="100" value="75">75/100</progress>
<meter min="0" max="100" value="75" low="33" high="66" optimum="50">75</meter>
```

## 文件上传File

通过File API,我们可以在用户选取一个或者多个文件之后,访问到代表了所选文件的一个或多个File对象,这些对象被包含在一个FileList对象中.
如果用户只选择了一个文件,那么我们只需要访问这个FileList对象中的第一个元素.

可以使用传统的DOM选择方法来获取到用户所选择的文件：
``` bash
<input id="fileItem" type="file">
var file = document.getElementById('fileItem').files[0];
```
还可以使用jQuery选择器来选择：
``` bash
var selectedfile = $('#input').get(0).files[0];
var selectedFile = $('#input')[0].files[0];
```
以上可以返回一个FileList对象。
如果你的程序可以让用户选择多个文件,记得要在input元素上加上multiple属性。

### 获取所选文件的信息
用户所选择的文件都存储在了一个FileList对象上，其中每个文件都对应了一个File对象。你可以通过这个FileList对象的length属性知道用户一共选择了多少个文件：
``` bash
var numFiles = files.length;
```
1. name
文件名,只读字符串,不包含任何路径信息.

2. size
文件大小,单位为字节,只读的64位整数.

3. type
MIME类型,只读字符串,如果类型未知,则返回空字符串.

4. item()
``` bash
// fileInput is an HTML input element: <input type="file" id="myfileinput" multiple>
var fileInput = document.getElementById("myfileinput");

// files is a FileList object (similar to NodeList)
var files = fileInput.files;
var file;

// loop through files
for (var i = 0; i < files.length; i++) {

    // get item
    file = files.item(i);
    //or
    file = files[i];

    alert(file.name);
}
```
### 通过拖放操作选择文件
你可以让用户将本地文件拖放到你的应用程序上.
1. 首先要创建一个拖放操作的目的区域。可根据您的应用程序的设计来决定哪部分的内容接受 drop，但创建一个接收drop事件的元素是简单的：
``` bash
var dropbox;

dropbox = document.getElementById("dropbox");
dropbox.addEventListener("dragenter", dragenter, false);
dropbox.addEventListener("dragover", dragover, false);
dropbox.addEventListener("drop", drop, false);
```
在这个例子中，ID 为 dropbox 的元素所在的区域是我们的拖放目的区域。我们需要在该元素上绑定 dragenter，dragover，和drop 事件。
我们必须阻止dragenter和dragover事件的默认行为，这样才能触发 drop 事件：
``` bash
function dragenter(e) {
  e.stopPropagation();
  e.preventDefault();
}

function dragover(e) {
  e.stopPropagation();
  e.preventDefault();
}
```
下面是 drop 函数：
``` bash
function drop(e) {
  e.stopPropagation();
  e.preventDefault();

  var dt = e.dataTransfer;
  var files = dt.files;

  handleFiles(files);
}
```
在该函数中,我们从事件对象中获取到dataTransfer对象，把该对象包含的Filelist对象传入函数handleFiles()，这个函数会无区别的对待从input元素或拖放操作中来的文件列表。

假设你正在开发下一个伟大的照片分享网站，并希望使用HTML5在用户上传他们图片之前进行缩略图预览。您可以如前面所讨论的建立一个输入元素或者拖放区域,并调用一个函数，如下面的 handleFiles 函数。
``` bash
function handleFiles(files) {
  for (var i = 0; i < files.length; i++) {
    var file = files[i];
    var imageType = /^image\//;

    if ( !imageType.test(file.type) ) {
      continue;
    }

    var img = document.createElement("img");
    img.classList.add("obj");
    img.file = file;
    // 假设 "preview" 是将要展示图片的 div
    preview.appendChild(img);

    var reader = new FileReader();
    reader.onload = (function(aImg) {
      return function(e) {
        aImg.src = e.target.result;
      };
    })(img);
    reader.readAsDataURL(file);
  }
}
```

这里我们循环处理用户选择的文件，查看每个文件的类型属性，看看这是否是一个图像文件(通过一个正则表达式匹配字符串"image.*")。
对于每个图片文件，我们创建一个新的img元素。CSS可以用于建立任何漂亮的边界,阴影,和指定图像的大小，所以，甚至不需要在这里完成。

每张图片我们添加一个obj类，让他们更容易的在DOM树中被找到。我们也在图片上添加了一个file属性来确认每张图片的 File，这样可以帮助我们在之后真正的上传工作时获取到图片。最后我们使用 Node.appendChild() 把缩略图添加到我们先前的文档区域中。

然后，我们建立了{ { domxref(FileReader)} }来处理图片的异步加载,并把它添加到img元素上。在创建新的FileReader对象之后,我们建立了onload函数,然后调用readAsDataURL()开始在后台进行读取操作。当图像文件的所有内容加载后，他们转换成一个 data: URL，传递到onload回调函数中。之后只需要把img元素的src属性设置为这个加载过的图像，就可以让图像的缩略图出现在用户的屏幕上。

### 使用对象URL
Gecko 2.0 (Firefox 4 / Thunderbird 3.3 / SeaMonkey 2.1)开始支持window.URL.createObjectURL()和 window.URL.revokeObjectURL()两个DOM方法。这两个方法创建简单的URL字符串对象，用于指向任何 DOM File 对象数据，包括用户电脑中的本地文件。

当你想要在HTML中通过URL来引用File对象，你可以参考如下方式创建：

var objectURL = window.URL.createObjectURL(fileObj);

URL对象是 File 对象的一个字符串标识。 每次调用window.URL.createObjectURL()的时候，会创建一个唯一的URL对象，即使你已经为该文件创建了URL对象。这些对象都必须被释放。 当文档被卸载时，它们会自动释放，如果你的页面动态地使用它们，你应该明确地通过调用window.URL.revokeObjectURL()释放它们：

window.URL.revokeObjectURL(objectURL);

以下为完整示例：

负责界面呈现的HTML如下：
``` bash
<input type="file" id="fileElem" multiple accept="image/\*"
  style="display：none" onchange="handleFiles(this.files)">
<a href="#" id="fileSelect">Select some files</a>
<div id="fileList">
  <p>No files selected!</p>
</div>
```
这建立文件的<input>元素就像一个调用文件选择器的链接（因为我们要隐藏文件输入，以防止表现出不友好的UI）。这部分将在 Using hidden file input elements using the click() method里解释，因为是调用文件选择器的方法。

下面是 handleFiles()方法：
``` bash
window.URL = window.URL || window.webkitURL;

var fileSelect = document.getElementById("fileSelect"),
    fileElem = document.getElementById("fileElem"),
    fileList = document.getElementById("fileList");

fileSelect.addEventListener("click", function (e) {
  if (fileElem) {
    fileElem.click();
  }
  e.preventDefault(); // prevent navigation to "#"
}, false);

function handleFiles(files) {
  if (!files.length) {
    fileList.innerHTML = "<p>No files selected!</p>";
  } else {
    var list = document.createElement("ul");
    for (var i = 0; i < files.length; i++) {
      var li = document.createElement("li");
      list.appendChild(li);

      var img = document.createElement("img");
      img.src = window.URL.createObjectURL(files[i]);
      img.height = 60;
      img.onload = function(e) {
        window.URL.revokeObjectURL(this.src);
      }
      li.appendChild(img);

      var info = document.createElement("span");
      info.innerHTML = files[i].name + "： " + files[i].size + " bytes";
      li.appendChild(info);
    }
    fileList.appendChild(list);
  }
}
```

通过 "fileList" ID 获取 <div>。我们将向这个 div 块插入文件列表和略缩图的地方。

假如传递给 handleFiles() 函数的 FileList 对象为 null，那么就设置 div 块的 inner HTML 为 "No files selected!"。否则，我们将按下面的步骤建立文件列表：

1.创建一个无序列表 (<ul>) 元素。
2.通过调用 element.appendChild() 方法向 <div> 块插入新的列表项。
3.files 即是 FileList，对于其中的每一个 File ：
* 创建一个 <li> 元素，并将其插入 list 中。
* 创建一个 <img> 元素.
* 使用 window.URL.createObjectURL() 创建URL，并设置图片的源为表示文件的新的 URL 对象。
* 设置图片的 height 为 60 像素。
* 创建图片的 load 事件句柄以其解除 URL 对象，因为图像载入之后就不再需要 URL。这是通过调用 window.URL.revokeObjectURL() 方法来完成的，用 img.src 传入指定的 URL 对象字符串。
* 添加新的列表项到列表之中。
