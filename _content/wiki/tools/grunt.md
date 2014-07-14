# Grunt

----------------

任务自动管理工具，[详解(需要翻墙)](http://javascript.ruanyifeng.com/tool/grunt.html#toc4)

快捷导航：

1. [安装](#安装)
2. [命令脚本文件](#命令脚本文件)
3. [执行](#执行)
4. [grunt常用方法](#grunt常用方法)
5. [常用模块设置](#常用模块设置)
6. [grunt模块通用配置](#grunt模块通用配置)


## 安装

Grunt 基于 node.js ，安装之前请先安装node.js / npm

grunt-cli 表示安装grunt的命令行界面
```
npm install grunt-cli -g
```

grunt  使用模块结构，除了命令行界面意外，还根据需要安装相应的模块。
（不同项目可能需要同一个模块的不同版本，因而建议分开安装）

> 手动创建package.json

1. 在项目根目录下，创建一个文本文件package.json,指定当前项目所需要的模块。
```
{
    "name": "my-project-name",
    "version": "0.1.0",
    "devDependencies": {
         "grunt": "0.4.*",
         "grunt-contrib-clean": "~0.5.0",
         "grunt-contrib-coffee": "~0.10.1",
         "grunt-contrib-less": "~0.9.0",
         "grunt-contrib-yuidoc": "~0.5.0"
    }
}
```
2. 在项目根目录下运行下面的命令，这些插件就会被自动安装在node_modules子目录中。
```
npm install
```

> 自动生成package.json

使用以下命令，按屏幕提示回到所需模块的名称和版本即可

```
npm init
```

> 已有package.json 文件不包括grunt模块。

可以在直接安装grunt模块的时候，加上--save-dev参数，该模块就会自动被加入package.json文件。
```
npm install <module> --save-dev
```

比如，对应上面package.json文件指定的模块，需要运行以下npm命令。
```
npm install grunt --save-dev
npm install grunt-contrib-clean --save-dev
npm install grunt-contrib-coffee --save-dev
npm install grunt-contrib-less --save-dev
npm install grunt-contrib-yuidoc --save-dev
```

## 命令脚本文件

根目录下，新建gruntfile.js || gruntfile.coffee 脚本文件，他是grunt的配置文件

gruntfile.js 就是一般的 node.js 模块的写法

> JS版

```
module.exports = function(grunt) {

    //  配置Grunt各种模块的参数
    grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
     jshint: { /* jshint的参数 */ },
     concat: { /* concat的参数 */ },
    uglify: { /* uglify的参数 */ },
     watch:  { /* watch的参数 */ }
    });

    // 从node_modules目录加载模块文件
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    grunt.loadNpmTasks('grunt-contrib-watch');

    // 每行registerTask定义一个任务
    grunt.registerTask('default', ['jshint', 'concat', 'uglify']);
    grunt.registerTask('check', ['jshint']);

};
```

> CoffeeScript 版

```
module.exports = (grunt)->

    # 配置Grunt各种模块的参数
    grunt.initConfig
        pkg: grunt.file.readJSON('package.json')

   #从node_modules目录加载模块文件
    grunt.loadNpmTasks 'grunt-contrib-clean'
    grunt.loadNpmTasks 'grunt-contrib-coffee'
    grunt.loadNpmTasks 'grunt-contrib-less'
    grunt.loadNpmTasks 'grunt-contrib-yuidoc'
    grunt.loadNpmTasks 'grunt-contrib-copy'

    grunt.registerTask 'build', [
    'clean:build',
    'coffee',
    'less:build',
    ]
    grunt.registerTask 'dist', [ 'build', 'clean:dist', 'copy:build']
```

* grunt.initConfig： 定义各种模块的参数，每一个成员项对应一个同名模块。
* grunt.loadNpmTasks：   加载完成任务所需的模块。
* grunt.registerTask：   定义具体的任务。
第一个参数为任务名。
第二个参数是一个数组，表示该任务需要依次使用的模块。
default任务名表示，如果直接输入grunt命令，后面不跟任何参数，这时所调用的模块（该例为jshint，concat和uglify）

## 执行

> 命令行执行某个模块

```
grunt jshint
```

> 命令行执行某个任务, 只键入grunt 则执行默认default任务

```
grunt check
```

## grunt常用方法

> grunt.loadNpmTasks 载入文件模块

每加载一个模块都需要引入一个 grunt.loadNpmTasks方法，如果加载模块过多，会非常冗长，而且凡是加载模块同时，也会载入到package.json 文件中，如果使用npm命令卸载模块以后，模块也会从package.json中删除。而gruntfile文件则需要手动删除，容易造成运行错误。

```
解决方案：

安装load-grunt-tasks模块，在gruntfile.js文件中用下面的语句替代所有的grunt.loadNpmTasks

require('load-grunt-tasks')(grunt);

这条语句的作用是自动分析package.json文件，加载所找到的grunt模块。

```

　
> grunt.initConfig 用于模块配置

```
grunt.initConfig({
    cssmin: {
        minify: {
            expand: true,
            cwd: 'css/',
            src: ['*.css', '!*.min.css'],
            dest: 'css/',
            ext: '.min.css'
        },
        combine: {
            files: {
                'css/out.min.css': ['css/part1.min.css', 'css/part2.min.css']
            }
        }
    }
});

```

接收一个对象作为参数，该对象的成员与使用的同名模块一一对应。

```
Eg：我们要配置的是cssmin模块，所以initConfig里面有一个cssmin成员（属性）
```

每一个属性指向一个对象，该对象有包含了多个成员。一些是系统设定的成员（options），另一些是自定义的成员（被称为目标target）。一个模块可以有多个target。

```
例子中的minify用于压缩css,另一combine,用于多个css合并一个文件
```

每一个target的具体设置需要参模板的文档。

　
> grunt.registerTask 定义如何调用具体任务

```
grunt.registerTask('default', ['cssmin:minify','cssmin:combine'])
```

"default"  任务表示如果不提供参数，直接输入grunt命令，则先运行“cssmin:minify”，后运行“cssmin:combine”，即先压缩再合并。

```
grunt   # 默认情况下，先压缩后合并
```

如果只执行压缩，或者只执行合并，则需要在grunt命令后面指明“模块名:目标名”。

```
grunt cssmin:minify   # 只压缩不合并

grunt css:combine     # 只合并不压缩
```

如果不指明目标，只是指明模块，就表示将所有目标依次运行一遍。

```
grunt cssmin
```

## 常用模块设置

```
} grunt-contrib-clean：删除文件。
} grunt-contrib-compass：使用compass编译sass文件。
} grunt-contrib-concat：合并文件。
} grunt-contrib-copy：复制文件。
} grunt-contrib-cssmin：压缩以及合并CSS文件。
} grunt-contrib-imagemin：图像压缩模块。
} grunt-contrib-jshint：检查JavaScript语法。
} grunt-contrib-uglify：压缩以及合并JavaScript文件。
} grunt-contrib-watch：监视文件变动，做出相应动作。
```

** 注：前缀
grunt-contrib- 表示该模块由grunt开发团队维护
grunt- 表示由第三方开发者维护

* grunt-contrib-jshint 检查语法错误，比如分号的使用是否错误，有无括号等。

```
jshint: {
    options: {
        eqeqeq: true,   // 是否严格检查
        trailing: true  // 结尾不得有多有空格
    },
    files: ['Gruntfile.js', 'lib/**/*.js'] // 检查目标
},
```

* grunt-contrib-concat 用来合并同类文件，css和js

```
concat: {
     js: {
        src: ['lib/module1.js', 'lib/module2.js'],
dest: 'dist/script.js'  // 合并js文件
     },
 css: {
        src: ['style/base.css', 'style/theme.css'],
dest: 'dist/screen.css' // 合并css文件
     }
},
```

* grunt-contrib-uglify 压缩代码，减小文件体积

```
uglify: {
    // 指定压缩后文件的头文件
    options: {
        banner: bannerContent,
        sourceMapRoot: '../',
        sourceMap: 'distrib/'+name+'.min.js.map',
        sourceMapUrl: name+'.min.js.map'
    },

    // target目标制定输入和输出文件
    target : {
        expand: true,
        cwd: 'js/origin',
        src : '*.js',
        dest : 'js/'
    }
},
```

* grunt-contrib-copy

```
copy: {
    main: {
        expand: true,
        cwd: 'src/',
        src: '**',
        dest: 'dest/',
        flatten: true,
        filter: 'isFile',
    },
},
```
