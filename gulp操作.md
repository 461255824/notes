## gulp操作
### gulp是用来压缩合并代码的
1.npm 全局安装gulp
```
npm install gulp-cli -g
```
2.找到要压缩代码的文件夹

（1）首先初始化npm工具
```
npm init -y
```
会生成一个package.json的安装文件
（2）安装gulp文件
```
npm install gulp --save--dev
```
--save
表示要将文件存到dependencies 项目使用依赖的插件
--save--dev
表示要将文件存到devDependencies 开发时依赖的插件
###  文件使用gulp压缩时需要的其他插件
1.html压缩
```
npm install gulp-htmlmin --save--dev
```
2.css压缩
```
npm install gulp-cssnano --save--dev
```
3.js压缩
```
npm install gulp-uglify --save--dev
```
4.文件合并
```
npm install gulp-concat --save--dev
```
###gulp执行需要的js代码
1.新建gulpfile.js 代码如下
```
//js引入gulp对象
var gulp = require("gulp");
*注：引入的对象要加引号；
//引入其他需要的插件对象
var htmlmin = require("gulp-htmlmin");
var cssnano = require("gulp-cssnano");
var uglify = require("gulp-uglify");
var concat = require("gulp-concat");
var rename = rqurie("gulp-rename");//rename是一个插件

```
2.执行任务代码
- gulp通过执行任务进行操作 任务顺序：gulp.task --> gulp.src--> gulp.dest -->gulp.watch
```
//新建一个task任务压缩js代码
gulp.task("任务名",function(){
    gulp.src(["需要压缩的文件相对路径"])//如果是多个使用数组
    .pipe(uglify())//gulp是通过管道pipe进行流通的
    .pipe(rename('新的名字'))//重命名一般是    名字.min.js
    .pipe(gulp.dest("压缩后文件要存的位置"))
})
```
3.合并两及多个文件concat();
```
gulp.task("合并任务",function(){
    gulp.src(['文件路径文件1','文件2'])
    .pipe(concat('生成的文件路径及名称'))
    .pipe(gulp.dest("压缩后文件要存的位置"))
})
```
4.压缩html文件
html文件中肯能含有css和js所以是应该是判读最多的 需要传入参数对象
```
gulp.task('htmlmin', function () {
//如果需要去除多个参数 可以先声明变量 便于整洁
    var options = {
        removeComments: true,//清除HTML注释
        collapseWhitespace: true,//压缩HTML
        collapseBooleanAttributes: true,//省略布尔属性的值 <input checked="true"/> ==> <input />
        removeEmptyAttributes: true,//删除所有空格作属性值 <input id="" /> ==> <input />
        removeScriptTypeAttributes: true,//删除<script>的type="text/javascript"
        removeStyleLinkTypeAttributes: true,//删除<style>和<link>的type="text/css"
        minifyJS: true,//压缩页面JS
        minifyCSS: true//压缩页面CSS
    };
    gulp.src('src/html/*.html')   //找到文件下所有以.html结尾的文件
        .pipe(htmlmin(options))
        .pipe(gulp.dest('dist/html'));  //默认压缩后的文件都放到dist文件下
});
```
5.wactch任务可以配合browser-sync一起使用
- 就是当监视下文件发生改变的时候 browser也进行显示已保证压缩的文件没有问题
```
//全局安装borowsync
npm install browser-sync -g

//启动browser服务
browsync start --server --files "文件相对路径";
注：browsync启动的当前目录一定要含有index.html文件 要不然启动不了
```
browser-sync和wacth没有必然的练习
```
gulp.task("mywacht",function(){
    gulp.watch(['文件名1','文件名2'],['任务名1','任务名2']);
})
//当文件名1或者2的内容改变的时候执行任务1,2；
```
6.利用gulp合并图片成雪碧图
原理：是将css文件中用到的图片合并成一张，并改变css文件中图片的引入路径
(1)安装插件
```
npm install gulp-spriter --save--dev
```
(2)js代码
```
//引入对象
var spriter = gulp.require("gulp-spriter");
//新建任务代码
gulp.task("spriter",function(){
    gulp.src("文件名.css")
    .pipe(spriter({
        spriter:'合并后的图片名.png',
        slice:'./原来的图片所在的文件夹',
        outpath:'合并后图片保存的文件夹'
    }))
    .pipe(gulp.dest("css文件保存的路径"))
})
```
### 总结：
- API:
task (创建任务)
src (选择路径)
dest (输出路径)
watch (监视文件及执行任务)
rename (重命名文件)
require (引入插件对象)
pipe (管道)

- 插件：

|常用插件|不常用插件|
| ---------- | -----------:|
|uglify (压缩js)|imgmin|
|concat (合并文件)|jshint|
|cssnano (压缩css)|eslint|
|htmlmin (压缩html)|sourcemap|
|spriter (合并雪碧图)|gulp-less|
|gulp-rename(重命名)|gulp-if|
|                    |gulp-jsonminify|
|                    |gulp-load-plugins|
|                    |gulp-eslint|


注：跳转[gulp中文官网](http://www.gulpjs.com.cn/);
