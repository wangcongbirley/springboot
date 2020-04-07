# springboot
做项目过程中，记录问题与解决方案

yml文件格式：key:+空格+value

pom文件，maven配置。

springboot 集成其他组件。

#### 上传
*前端input[file]原生上传。*

`
<div style="margin-top:5px;">
  <input type="file" id="uploadFile" style="display:none;" />
  <input type="text" id="filename" />
  <button class="btn btn_default" id="selectBtn" onclick="javascript:$('#uploadFile').click();">浏览...</button>
</div>
var f=document.getElementById("files");
	$("#uploadFile").change(function() {
				var fileList = $("#uploadFile").get(0).files;
				var filename = fileList[0].name;
				var fs = filename.split(".");
				var fix = fs[fs.length - 1];
				if(fix != "gltf" && fix != "fbx" && fix != "glb") {
					layer.msg("您上传的不是模型文件！", {
						icon: 2,
						offset: '300px'
					});
					return false;
				}
				var img = window.webkitURL.createObjectURL(fileList[0]);
				var filesize = Math.floor((fileList[0].size) / 1024 / 1025);
				if(filesize > 5) {
					alert("上传大小不能超过5M.");
					return false;
				}
				sourceFile = fileList[0];
				var obj = {
					"url": img
				}
				$("#filename").val(filename);
				loadModel(obj);

				xhr = new XMLHttpRequest();
				xhr.open("post", "FileUploadServlet", true);
				xhr.setRequestHeader("X-Requested-With", "XMLHttpRequest");

				var fd = new FormData();
				fd.append('files', sourceFile);
				xhr.onreadystatechange = function(e) {
					if(xhr.readyState == 4 && xhr.status == 200) {
						var result = eval("(" + xhr.responseText + ")");
						if(result.returnCode == 200) {
							file_id = result.fileId;
							alert(file_id);
						} else {
							alert(result.returnMessage);
							layer.msg("请上传模型资源文件！", {
								icon: 0,
								offset: '30px'
							});
						}
					}
				};
				xhr.send(fd);
			});
`
JSON 数组，各种格式。Map，Bean，字符串，数组，混用。

外键查询，
