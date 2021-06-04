---
layout: post
title: javascript frame upload file
tags: js
---

<style>
	.highlight-left{margin-left: 0;}
</style>



```

<html>
<head><title>Ifram Main</title>
</head>
<body>
	Main Ifram
	<iframe scr="ifa.html" width="200px" height="200">
	</iframe>
	<script>
		var seed = Math.floor(Math.random() * 1000);
		var id = "uploader-frame-"+seed;
		var callback = "uploader-cb-"+seed;
		var iframe = document.getElementById('<iframe id="'+id+'" name="'+id+'" style="display:none;">');
		var url = form.attr('action');
		form.attr('target', id).append(frame).attr('action', url+'?iframe='+callback);
		
		window[callbak] = function(data) {
			console.log('received callback:', data);
			iframe.remove(); // removing iframe
			form.removeAttr('target');
			form.attr('action', url);
			window[callback] = undefined; // removing callback
		}
		
		// FormData is file upload check
		if (window.FormData) {
			var formData = new FormData();
			formData.append('upload', document.getElementByid('upload').files[0]);
			var xhr = new XMLHttpRequest();
			xhr.open('POST', $(this).attr('action'));
			//callback
			xhr.onload = function(){
				if (xhr.status == 200) {
					console.log('Upload successfull');
				} else {
					console.log("Error!");
				}
			};
			xhr.send(formData);
			
			xhr.upload.onprogress=function(event) {
				if (event.lengthComputable) {
					var complete = (event.loaded / event.total * 100 | 0);
					var progress = document.getEelementById('uploadprogress');
					progress.value = progress.innerHTML = complete;
				}
			};
		}
	</script>
		
		<progress id='uploadprogress' min='0' max='100' value='0'>0</progress>
		
</body>
</html>

```


```
<html>
<head><title>this is iframe</title></head>
<body>
	this is body ifram content
	<script>
		window.top.window['callback'](data);
	</script>
</body>
</html>
```