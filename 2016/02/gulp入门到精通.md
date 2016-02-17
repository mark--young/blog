##gulp入门教程


### 1.安装gulp

前提：安装了nodejs，并在项目文件夹下使用了

	node init

初始化了项目，生成package.json文件。


第一步：安装命令行工具

	$ npm install -g gulp

第二步：在你的项目下把 gulp 安装为开发依赖组件

	$ cd <YOUR_PROJECT>
	$ npm install gulp --save-dev

第三步：在项目的根路径下创建 Gulpfile.js，初始内容为：

	var gulp = require('gulp');

	gulp.task('default', function () {
		// Hello World
		console.log("Hello World");
	});

第四步：运行！

	$ gulp

### 2.gulp-uglify插件

使用gulp-uglify插件压缩js：

第一步：安装命令行工具

	$ npm install --save-dev gulp-uglify

第二步：修改gulpfile.js文件，新建一个压缩js的task：

	var gulp = require('gulp'),
	uglify = require('gulp-uglify');
	
	gulp.task('default',function(){
		gulp.src('js/*.js')  // 指定文件夹下的所有js
		.pipe(uglify()) //使用uglify
		.pipe(gulp.dest('minijs')); //输出到指定的文件夹
	});

第三步：运行gulp

	$ gulp

就可以在minjs文件夹下得到压缩完后的js文件

### 3.自建Task

如下我们新建一个新的task，并给它命名为`scripts`

	var gulp = require('gulp'),
	uglify = require('gulp-uglify');

	//Scripts Task
	//Uglifies
	gulp.task('scripts',function(){
		gulp.src('js/*.js')
		.pipe(uglify())
		.pipe(gulp.dest('build/js'));
	});

之后，我们就可以在命令工具中使用：

	$ gulp scripts

来运行我们指定的task。


使用default来批量运行tasks：
	
	var gulp = require('gulp'),
		uglify = require('gulp-uglify');
	
	//Scripts Task
	//Uglifies
	gulp.task('scripts',function(){
		gulp.src('js/*.js')
		.pipe(uglify())
		.pipe(gulp.dest('build/js'));
	});
	
	//Styles Task
	//Uglifies
	gulp.task('styles',function(){
		console.log("runs styles");
	});
	
	gulp.task('default', ['scripts','styles']); 

那么当我们使用

	$ gulp

系统会按`scripts`，`styles`的顺序来执行任务。


### 4.动态运行task

在gulp中可以使用`watch`方法来动态监控并运行tasks。

下面我们来介绍下：

	var gulp = require('gulp'),
		uglify = require('gulp-uglify');
	
	//Scripts Task
	//Uglifies
	gulp.task('scripts',function(){
		gulp.src('js/*.js')
		.pipe(uglify())
		.pipe(gulp.dest('build/js'));
	});
	
	//Watch Task
	//Watches Js
	gulp.task('watch',function(){
		gulp.watch('js/*.js', ['scripts']);
	
	});


其中的`gulp.watch('js/*.js', ['scripts']);`即监视js文件夹下，所有后缀为js的文件，一旦文件有变化，则会运行`scripts`任务。

	$ gulp watch 

来启用监视。

下面，你可以任意在js文件夹下来修改，或者新建js文件，那么gulp会自动把新的js压缩到指定的文件夹`build/js`中。

如果，你是一个懒人，想在使用`$ gulp`的默认任务情况下，来实现监控，可以使用如下代码：

	var gulp = require('gulp'),
	uglify = require('gulp-uglify');

	//Scripts Task
	//Uglifies
	gulp.task('scripts',function(){
		gulp.src('js/*.js')
		.pipe(uglify())
		.pipe(gulp.dest('build/js'));
	});
	
	//Styles Task
	//Uglifies
	gulp.task('styles',function(){
		console.log("runs styles");
	});
	
	//Watch Task
	//Watches Js
	gulp.task('watch',function(){
		gulp.watch('js/*.js', ['scripts']);
	
	});
	
	gulp.task('default', ['scripts','styles','watch']);

很简单，就只需要在`default`任务中，加入`watch`任务就行了。

###5.对Sass压缩

第一步：使用npm安装`gulp-ruby-sass`插件

	$ npm install --save-dev gulp-ruby-sass

第二步：新建require引用：

	var sass = require('gulp-ruby-sass');


第三步：新建一个task：

	//Styles Task
	//Uglifies
	gulp.task('styles',function(){
		gulp.src('scss/**/*.scss') // 在scss文件夹下的所有scss文件
		.pipe(sass())  // 使用sass
		.pipe(gulp.dest('css/'));//输出到`css/`下
	});

如需压缩sass文件：

	//Styles Task
	//Uglifies
	gulp.task('styles',function(){
		gulp.src('scss/**/*.scss')
		.pipe(sass({
			style:'compressed'  //使用压缩选项
		}))
		.pipe(gulp.dest('css/'));
	});

如果需要加入到监视文件中，进行动态的修改和输出：

	//Watch Task
	//Watches Js
	gulp.task('watch',function(){
		gulp.watch('js/*.js', ['scripts']);
		gulp.watch('scss/**/*.scss',['styles'])
	});

那么，就可以使用`$ gulp` 默认命令来执行我们上述的任务了。

###6.检查代码

如果在动态监视代码的时候，自己由于一些低级错误，可能会导致监视停止。那么，我们就要使用到gulp的plumber功能。

使用gulp来动态监视代码中的一些低级错误，导致的监视中止：

第一步：安装gulp-plumber插件

	$ npm install --save-dev gulp-plumber

第二步：新建require引用：

	var plumber = require('gulp-plumber');

第三步：在任务中使用plumber

	//Scripts Task
	//Uglifies
	gulp.task('scripts',function(){
		gulp.src('js/*.js')
			.pipe(plumber()) //使用plumber
			.pipe(uglify())
			.pipe(gulp.dest('build/js'));
	});
	
	//Styles Task
	//Uglifies
	gulp.task('styles',function(){
		gulp.src('scss/**/*.scss')
			.pipe(plumber()) //使用plumber
			.pipe(sass({
				style:'compressed'
			}))
			.pipe(gulp.dest('css/'));
	});


###7.gulp代码自检

同样，当我们不使用plumber时，gulp也提供了一些错误提示方式：
	
	//Styles Task
	//Uglifies
	gulp.task('styles',function(){
		gulp.src('scss/**/*.scss')
		.pipe(sass({
			style:'compressed'
		}))
		.on('error',console.error.bind(console)) // 错误提示
		.pipe(gulp.dest('css/'));
	});


或者使用函数把错误提示进行封装：

	var gulp = require('gulp'),
		uglify = require('gulp-uglify'),
		sass = require('gulp-ruby-sass');
	
	function errorLog(error){  //定义一个错误提示
		console.error.bind(error);
		this.emit('end');
	}
	
	//Scripts Task
	//Uglifies
	gulp.task('scripts',function(){
		gulp.src('js/*.js')
		.pipe(uglify())
		.on('error',errorLog)
		.pipe(gulp.dest('build/js'));
	});
	
	//Styles Task
	//Uglifies
	gulp.task('styles',function(){
		gulp.src('scss/**/*.scss')
		.pipe(sass({
			style:'compressed'
		}))
		.on('error',errorLog)
		.pipe(gulp.dest('css/'));
	});
	
	//Watch Task
	//Watches Js
	gulp.task('watch',function(){
		gulp.watch('js/*.js', ['scripts']);
		gulp.watch('scss/**/*.scss',['styles'])
	
	});
	
	gulp.task('default', ['scripts','styles','watch']);


###8.动态重载LiveReload

第一步：安装gulp-reload插件

	$ npm install --save-dev gulp-reload

第二步：新建require引用：

	var livereload = require('gulp-livereload');

第三步：在任务中使用livereload

	//Watch Task
	//Watches Js
	gulp.task('watch',function(){
		var server = livereload();

		gulp.watch('js/*.js', ['scripts']);
		gulp.watch('scss/**/*.scss',['styles'])
	
	});


### 9.压缩图片

第一步：安装gulp-imagemin插件

	npm install --save-dev gulp-imagemin


第二步：新建require引用：

	var imagemin = require('gulp-imagemin');

第三步：新建压缩任务

	//Image Task
	//Compress
	gulp.task('image',function(){
		gulp.src('img/*')
		.pipe(imagemin())
		.pipe(gulp.dest('build/img'));
	
	});


同样，可以加入`watch`和`default`任务

### 10.
