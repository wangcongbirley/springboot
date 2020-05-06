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

### 流下载

读取本地二进制文件流，通过浏览器下载。

`	
public void modelSourceUrl(String fileId, HttpServletRequest request, HttpServletResponse response) throws IOException {
		OutputStream os = null;
		InputStream is = null;
		ByteArrayOutputStream bos = null;
		byte[] b = null;
		String path = FILE_UPLOAD_DIR + File.separator + fileId;
		File file = new File(path);
		File[] tempList = file.listFiles();
		logger.info(tempList[0].toString());
		try {
			byte[] buffer = new byte[1024];
			int len = 0;
			is=new FileInputStream(tempList[0]);
			bos = new ByteArrayOutputStream();
			while ((len = is.read(buffer)) != -1) {
				bos.write(buffer, 0, len);
			}
			bos.flush();
			b = bos.toByteArray();
			os = response.getOutputStream();
            response.addHeader("Content-Disposition", "attachment;filename=" + new String(tempList[0].getName().getBytes()));
            response.addHeader("Content-Length", "" + tempList[0].length());
            response.setContentType("application/octet-stream");
			os.write(b, 0, b.length);
			os.flush();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if (is != null) {
				 is.close();
			}
		}
`

项目中遇到技术难点，一直调试失败。暂时放一放，做其他的，保持项目进度。

vue参数设置

vue组件控制v-if v-on @change @click :sync

### excel下载

Excel下载中，有HSSFWorkbook和XSSFWorkbook，它们的区别是：

HSSF对应97-2003版本的Excel，XSSF则对应2007版本的Excel。

### springboot启动初始化数据库

classpath:
 
1.  src 路径下的文件 在编译后都会放到 WEB-INF/classes 路径下。默认classpath 就是指这里。
 
2. 用maven构建 项目时，resources 目录就是默认的classpath
