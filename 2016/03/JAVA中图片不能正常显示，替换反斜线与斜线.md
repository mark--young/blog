##JAVA中图片不能正常显示，替换反斜线与斜线

昨天在项目开发的时候，碰到一个图片输出问题，使用Firebug看了一下，是因为图片的链接格式不正确。下面介绍一下解决思路：

###1.定位问题位置。

使用Firebug看到图片的链接是：127.0.0.1/pmms/Telemeter/..\carImages\2016-3-10\1.jpg

直接在链接上改了一下，改成：127.0.0.1/pmms/carImages/2016-3-10/1.jpg，显示正常。

看了一下原码：

	List<Object> fixedenvcarflow = telemeterService.getFixedEnvCarFlowByRegMarkAndRegColor(regMark, regColor);
	// 0.图片路径 1.通过时间 2.车牌号码 3.车牌颜色 4.车道 5.图片ID 6.站点名称 7.站点地址
	
	System.out.println(fixedenvcarflow.size());
	try {
		if (fixedenvcarflow != null && fixedenvcarflow.size() > 0) {
			for (int i = 0; i < fixedenvcarflow.size(); i++) {
				String curr = "normal";
				if (i == 0) {
					curr = "current";
				}
				Object[] obj = (Object[]) (fixedenvcarflow.get(i));
				System.out.println("obj[0].toString() str is:"+sb);
				result1 += "<li><a href=\"" + sb + "\" target=\"_blank \"><img src=\"" + sb
						+ "\" title=\"时间：" + obj[1] + " \" " + "alt=\"站点名称：" + obj[6] + "  站点地址：" + obj[7]
						+ "  车牌号码：" + obj[2] + "  车牌颜色：" + obj[3] + "\" class=\"image" + i + "\"></a></li>";
				System.out.println("result1 is :"+result1);
				result2 += "$('img.image" + i + "').data('ad-link', '" + sb + "');";
			}
		} else {
			result1 += "<li><a href=\"/images/noimg.jpg\"><img src=\"/images/noimg.jpg\" title=\"暂无图片\" alt=\"暂无图片\" class=\"image0\"></a></li>";
		}
	} catch (Exception e) {
		e.printStackTrace();
		result1 += "<li><a href=\"/images/noimg.jpg\"><img src=\"/images/noimg.jpg\" title=\"暂无图片\" alt=\"暂无图片\" class=\"image0\"></a></li>";
	}

使用Debug看了一下`obj[0]`中的内容：`..\CarImages\2016-3-10\1.jpg`，问题就出在这里。


###2.解决思路

（1）修改数据库中内容，修改返回字段。测试可行。

（2）在JAVA中：

* 把`obj[0]`转换成字符串，把其中的反斜线全转换成斜线
* 因为访问目录层级为上一层目录中的carImages文件夹，所以还需要进行字符串的拼接。


###3.实现代码：

	String str = obj[0].toString();
	str = str.replaceAll("\\\\","/");
	StringBuilder sb=new StringBuilder(str);
	sb.insert(3,"carImages/");

把`sb`替换所有的`obj[0]`，一切OK。

###4.小结

（1）使用到了`String`类的`replaceAll`方法，替换所有的反斜线为斜线。

（2）使用`StringBuilder`类的`insert`方法，在指定位置插入字符串。

（3）反斜线和紧跟着它的那个字符构成转义字符，如`\n`（表示换行）、`\"`（表示字符`"`）等,所以在字符串中要表示字符`\`要用`\\`来表示。

PS: `String`的`replaceAll()`方法，实际是采用正则表达式的规则去匹配的，

`\\\\`，java解析为`\\`交给正则表达式，正则表达式再经过一次转换，把`\\`转换成为\

也就是java里面要用正则来表示一个`\`，必须写成4个`\`。

`<noscript>`元素，当浏览器不支持JavaScript时，使用`<noscript>`元素可以指定在不支持脚本的浏览器中显示的替代内容。但在启用了脚本的情况下，浏览器不会显示`<noscript>`元素中的任何内容。