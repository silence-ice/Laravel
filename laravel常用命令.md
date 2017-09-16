#laravel常用命令
****

###安装项目名为Blog的laravel项目
>composer create-project laravel/laravel Blog --prefer-dist

###将根目录下的server.php替换成index.php即可使用目录名直接访问
>`http://127.0.0.1/Blog/server.php`  =>  `http://127.0.0.1/Blog/`

####Artisan创建控制器
>1.cd到项目根目录下。
>
>2.【创建Home控制器】：php artisan make:controller HomeController

>3.【创建后台目录并创建Api控制器】：php artisan make:controller Admin/ApiController

####Artisan生成中间件
> php artisan make:middleware AdminLogin

	//中间件
	//1.建立中间件：php artisan make:middleware AdminLogin，这个命令会在 app/Http/Middleware 目录下创建一个新的中间件类AdminLogin
	//2.注册中间件：
		//2.1 如果你想要中间件在每一个 HTTP 请求期间被执行，只需要将相应的中间件类设置到 app/Http/Kernel.php 的数组属性 $middleware 中即可。
		//2.2 如果你想要分配中间件到指定路由，只需要将相应的中间件类设置到 app/Http/Kernel.php 的数组属性 $routeMiddleware 中即可。 如下代码`'admin.login' => \App\Http\Middleware\AdminLogin::class,`
	//3.分配多个中间件到路由：如下面代码
	
	//4.每当请求`127.0.0.1/Blog/index.php/Home/Base/addAccountInfo?age=2`都会经过名字为'admin.login'的中间件进行过滤验证

####Artisan生成模型
>php artisan make:model Http/Models/Post

####Artisan查看路由
>php artisan route:list

####laravel生成全局公共函数
>1. 创建文件 app/Helpers/function.php
>
>2. 修改项目 composer.json，新增files选项

    "autoload": {
        "classmap": [
            "database"
        ],
        "psr-4": {
            "App\\": "app/"
        },
        "files": [
            "app/Helpers/function.php"
        ]
    },

>3.使用`composer dump-autoload`，重新加载autoload。


####laravel调用自定义类
>1.在app/Services/Human.php编写如下自定义类：
>
>2.注意要用namespace声明命名空间

	<?php
	namespace App\Services;
	
	class Human{
		static public $name = "王五";
		public $height = 180;
	
		static public function tell(){
			echo self::$name;//静态方法调用静态属性，使用self关键词
			//echo $this->height;//错。静态方法不能调用非静态属性
			//因为 $this代表实例化对象，而这里是类，不知道 $this 代表哪个对象
		}
	
		public function say(){
			echo self::$name . "我说话了";
			//普通方法调用静态属性，同样使用self关键词
			echo $this->height;
		}
	}

>3.在目标控制器中调用该类；注意要use自定义类

	<?php
	
	namespace App\Http\Controllers;
	
	use App\Http\Controllers\Controller;
	use Illuminate\Http\Request;
	use App\Http\Requests;
	
	use App\Services\Human;
	
	class UserController extends Controller{
	
		function getPublicFun(){
	
			/*公共类调用*/
			// require_once 'app/Services/Human.php';
			// \Human::tell();
	
			Human::tell();
	
		}
	}

>5.最笨的方法就是直接require_once

	/*公共类调用*/
	// require_once 'app/Services/Human.php';
	// \Human::tell();


###清除缓存
>php artisan cache:clear 用来清除各种缓存，如页面，Redis，配置文件等缓存。

###打包配置文件
>当你修改过env文件或者其他配置文件，可以使用`php artisan config:cache` 把所有的配置文件打包到一个文件达到更新配置的目的。


