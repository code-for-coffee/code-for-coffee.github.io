---
layout: post
title: MVC4 AJAX uploads using HTML5/Javascript
---


This is a quick walkthrough showcasing how to use ASP.NET MVC4 to upload files via AJAX. You can take a look at the code on <a href="https://github.com/code-for-coffee/mvc4-html5-ajax-upload">github</a>. This functionality requires a current version of Firefox, Chrome, or Internet Explorer 10+.

We'll begin with the controller. Your <code>controller.cs</code> file should have a new <strong>JsonResult</strong> to receive whatever you're uploading. Inside, you'll iterate through each Request.File sent to the server using the  <a href="http://msdn.microsoft.com/en-us/library/system.web.httppostedfile">HttpPostedFileBase</a> class.

<code>

[HttpPost]
public JsonResult Upload()
{
for (int i = 0; i &lt; Request.Files.Count; i++) {
// for each file being sent over...
HttpPostedFileBase file = Request.Files[i];

// Example of gathering information from each file
int fileSize = file.ContentLength;
string fileName = file.FileName;
string mimeType = file.ContentType;

// Open input stream
System.IO.Stream fileContent = file.InputStream;

// do stuff here
// do.stuff.huzzah

}
// Return JSON
return Json("Uploaded " + Request.Files.Count + " file(s)");
}

</code>

Now, the fun part: the HTML5. First, you need to create a new <a href="https://developer.mozilla.org/en-US/docs/Web/API/FormData">FormData</a> object.

<code>
var formdata = new FormData();
</code>

Next, you'll iterate through each file.

<code>for (i = 0; i < uploadedFiles.files.length; i++) {
}
</code>
Inside of your loop, you can append each file to the FormData object.

<code>

formdata.append(uploadedFiles.files[i].name, uploadedFiles.files[i]);
</code>

Finally, you can send the files over via a traditional Ajax call.

<code>
var xhr = new XMLHttpRequest();
xhr.open('POST', ajaxPostPath);
xhr.send(formdata);
xhr.onreadystatechange = function () {
if (xhr.readyState == 4 && xhr.status == 200) {
document.getElementById(idToPlaceResponseText).innerHTML = xhr.responseText;
}
</code>
That's it! Obviously, you can do some nifty stuff with the files if they're supported. The code on the github project is designed to upload only files that are an Image MIME type and display a preview inside of an &lt;code&gt;id&lt;/code&gt;.