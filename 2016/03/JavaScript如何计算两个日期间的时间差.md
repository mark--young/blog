##JavaScript如何计算两个日期间的时间差

有时候我们需要知道两个日期之间差了多少天，多少小时，甚至多少分钟多少秒。下面我们用JavaScript实现一个函数，用于计算两个日期的时间差，先来看看代码：

	<script type="text/javascript">
	
	/**
	* 时间对象的格式化;
	*/
	Date.prototype.format = function(format){
	 /*
	  * eg:format="YYYY-MM-dd hh:mm:ss";
	  */
		var o = {
	  		"M+" :  this.getMonth()+1,  //month
	  		"d+" :  this.getDate(),     //day
	  		"h+" :  this.getHours(),    //hour
	      	"m+" :  this.getMinutes(),  //minute
	      	"s+" :  this.getSeconds(), //second
	      	"q+" :  Math.floor((this.getMonth()+3)/3),  //quarter
	      	"S"  :  this.getMilliseconds() //millisecond
		}
	  
	   	if(/(y+)/.test(format)) {
	    	format = format.replace(RegExp.$1, (this.getFullYear()+"").substr(4 - RegExp.$1.length));
	   	}
	 
	   	for(var k in o) {
	    	if(new RegExp("("+ k +")").test(format)) {
	      		format = format.replace(RegExp.$1, RegExp.$1.length==1 ? o[k] : ("00"+ o[k]).substr((""+ o[k]).length));
	    	}
	   	}
	 	return format;
	}
	
	
	/* 
	* 获得时间差,时间格式为 年-月-日 小时:分钟:秒 或者 年/月/日 小时：分钟：秒 
	* 其中，年月日为全格式，例如 ： 2016-01-01 01:00:00 
	* 返回精度为：秒，分，小时，天
	*/
	
	function GetDateDiff(startTime, endTime, diffType) {
	    //将xxxx-xx-xx的时间格式，转换为 xxxx/xx/xx的格式 
	    startTime = startTime.replace(/\-/g, "/");
	    endTime = endTime.replace(/\-/g, "/");
	
	    //将计算间隔类性字符转换为小写
	    diffType = diffType.toLowerCase();
	    var sTime = new Date(startTime);      //开始时间
	    var eTime = new Date(endTime);  //结束时间
	    //作为除数的数字
	    var divNum = 1;
	    switch (diffType) {
	        case "second":
	            divNum = 1000;
	            break;
	        case "minute":
	            divNum = 1000 * 60;
	            break;
	        case "hour":
	            divNum = 1000 * 3600;
	            break;
	        case "day":
	            divNum = 1000 * 3600 * 24;
	            break;
	        default:
	            break;
	    }
	    return parseInt((eTime.getTime() - sTime.getTime()) / parseInt(divNum));
	}
	
	var testDate = new Date();
	var testStr = testDate.format("yyyy-MM-dd hh:mm:ss");
	
	var result = GetDateDiff("2016-01-01 16:00:00", testStr, "day");
	document.write("Wayearn.com 建站已有" + result + "天了。");
	//alert(result);
	</script>