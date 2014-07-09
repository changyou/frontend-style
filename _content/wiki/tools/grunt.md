# Grunt

----------------

任务自动管理工具，[详解(需要翻墙)](http://javascript.ruanyifeng.com/tool/grunt.html#toc4)

## 安装

Grunt 基于 node.js ，安装之前请先安装node.js / npm

grunt-cli 表示安装grunt的命令行界面
```
npm install grunt-cli -g
```

grunt  使用模块结构，除了命令行界面意外，还根据需要安装相应的模块。
（不同项目可能需要同一个模块的不同版本，因而建议分开安装）

* 手动创建package.json

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

* 自动生成package.json

使用以下命令，按屏幕提示回到所需模块的名称和版本即可

```
npm init
```

* 已有package.json 文件不包括grunt模块。

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

## gruntfile.js 命令脚本文件

根目录下，新建gruntfile.js || gruntfile.coffee 脚本文件，他是grunt的配置文件

gruntfile.js 就是一般的 node.js 模块的写法

JS版

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

CoffeeScript 版

```
module.exports = (grunt)->

    # 配置Grunt各种模块的参数
    grunt.initConfig(
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
