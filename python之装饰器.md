# python装饰器 [原文链接](http://www.cnblogs.com/weke/articles/5744793.html#undefined)

在python中，装饰器是一种增加函数功能的简单方法，利用装饰器功能可以很快的给不同的函数插入相同的功能。

在函数的参数中，已经知道，除了形式参数外，其实函数也可以是函数的参数，见下面的代码，来实现这样的一个效果，

	def f1():
		print('Hello')

	def f2(xxx):
		return xxx

 在如上的代码中，函数f1()输出结果是"Hello"，函数f2的参数是xxx，函数f2返回值是xxx ，那么调用函数后，得到的结果

见如下：

![](http://images2015.cnblogs.com/blog/731800/201608/731800-20160806201816450-505134278.png)

函数f2()调用后，输出结果是函数f1()的输出结果，在函数f2()调用中，可以看到参数是函数f1()。在python中，这仅仅是一种

实现的方式，函数的参数不仅仅是变量，也可以是函数。

装饰器要实现的效果就是给不同的函数插入相同的功能，在软件开发过程中，这样的事情是经常遇到的，比如一个系统，有

用户系统，业务系统，支付系统，要求每个系统加log的详细输出，直接写一个log详细输出的函数调用就可以了，但是如果又

要求全部取消了，那么就把调用的log详细输出的函数代码屏蔽，很显然，这样的实现方式并不是很完美的解决方案，另外，在

现在软件开发过程中，产品经理也是很任性的，早上可能说需要这个功能，下午可能又说取消XX功能，会把人折腾死，不管这么

多了，以实际的方式来依次深入到装饰器中，下面就实现给函数f1,f2,f3都加输出log的功能，来见实现这样的一个过程的代码：

	def f1():
    	print('函数f1')
 
	def f2():
    	print('函数f2')
     
	def f3():
    	print('函数f3')

现在要求给函数f1,f2,f3都加一个log的输出，要实现这样的一个过程的代码为：

	def log():
    	print('out log')
 
	def f1():
   		print('函数f1')
    	log()
 
	def f2():
   	 print('函数f2')
    	log()
 
	def f3():
   		print('函数f3')
    	log()

在如上的代码中，可以看到，编写了函数log()，然后函数f1,f2,f3分别调用函数log实现了这样的一个过程，但是如果有很多个函数都

要求加入log，那么就意味着很多个函数都得调用log函数，很显然，这样的方式过程很复杂，下面就使用装饰器来实现这样的一个过程，

见实现的代码：

	def outer(func):
    	def log():
        	print('out log')
        	func()
    	return log


	@outer
	def f1():
    	print('函数f1')


	@outer
	def f2():
    	print('函数f2')

	@outer
	def f3():
    	print('函数f3')
	f3()

见如上的代码，我们结合了装饰器来实现了这样的一个过程，先编写函数outer，然后分别在函数f1,f2,f3加了装饰器@outer来实现

了这样的一个过程，即使以后再加什么功能或者取消什么功能，直接在函数outer中维护，而不需要让每个函数都调用公共的函数，

同时某些函数不需要新添加的功能，可以在函数的上面取消@outer。装饰器的实现方式为：@+函数名，它的功能简单的可以总结

为：

	装饰器功能总结点：
 	 a、自动执行outer函数并且将其下面的函数名f1当作参数来传递；
  	 b、将outer函数的返回值(变量或者是函数)，重新赋值给f1；
  	 c、一旦结合装饰器后,调用f1其实执行的是log函数内部,原来的f1被覆盖；
  	 d、一旦这个函数被装饰器装饰之后，被装饰的函数重新赋值成装饰器的内层函数

依据总结的这几点，就来看函数f1，f2,f3实现的一个过程，因为函数f1,f2,f3实现的效果一直，就只看f1代码执行的过程，打断点，

调用函数f1()，来看执行的过程：

![](http://images2015.cnblogs.com/blog/731800/201608/731800-20160806204715856-1869961637.png)

执行顺序为：先执行函数outer()，在执行内部函数log()，下来是log()函数返回值，再到函数log()，下来是执行函数log()的内部，

最后是执行@outer(),，见添加注释后的代码和函数，就更加清晰：

	def outer(func):
    	#func值的是函数f1，也就是装饰器装饰的函数
    	def log():
        	print('out log')
        	#f1函数会被当作参数来传递,这里等价于调用f1()函数
        	func()
        	#返回值重新赋值给f1函数
    return log
 
 
	@outer
	def f1():
    	print('函数f1')
 
	f1()

可以获取到被装饰器装饰的函数，实际上调用函数f1实际上执行的是装饰器内部的函数log。

被装饰器装饰的函数，一般分为二种情况，一种是没有返回值，对于一个函数来说，函数中没有return，那么函数的返回值

就是None，如如下的函数，它的返回值是None，见执行的结果;

![](http://images2015.cnblogs.com/blog/731800/201608/731800-20160806210642981-747932948.png)

函数返回值为None，装饰器实现的效果为：

	def outer(func):
    	def log():
        print('out log')
        func()
    return log
 
	@outer
	def f1():
   	 	print('函数f1')

如果函数有返回值，那么装饰器就获取原函数的返回值，见实现的代码：

	def outer(func):
    	def log():
        print('out log')
        r=func()
        return r
    return log
 
 
	@outer
	def f1():
   	 return '函数f1'

如上演示的函数，都是没有参数的函数，但是在现实的实际例子中，还是都是有参数的，那么针对这种情况，

其实就是对装饰器内部函数中加入原函数的参数，就可以了，如实现二个数相加的结果，见装饰器实现的代码：

	def outer(func):
	    def log(a,b,c):
	        r=func(a,b,c)
	        return r
	    return log
	 
	 
	 
	@outer
	def add(a,b,c):
	    return a+b+c
	print(add(1,2,3))


函数的参数某些时候是一个，某些函数是N个，很有可能是列表，元组或者字典，针对这种情况，解决的办法是让

装饰器的函数参数是万能的，也就是什么参数都是可以接受的，见实现的过程：

	def outer(func):
	    def log(*args,**kwargs):
	        r=func(*args,**kwargs)
	        return r
	    return log
	 
 
 
	@outer
	def add(a,b,c):
	    return a+b+c

参数为函数的一种形式，见实现的代码：
	
	def outer(func):
	    def log(*args,**kwargs):
	        r=func(*args,**kwargs)
	        return r
	    return log
	 
	 
	def result(a,b):
	    print(a+b)
	 
	@outer
	def add(sum):
	    '''函数的参数为函数'''
	    return sum
	 
	add(result(2,3))

下面就以装饰器来实现用户登录的部分，在一个后台登录中，不管用户想做什么，前提是都必须得登录才可以的，

假设后台后台分别有订单模块，先看不使用装饰器来实现这个过程，见实现的代码：

	import  sys
	 
	 
	LOGIN_USER={"is_login":False}
	 
	 
	def admin():
	    '''后台管理系统'''
	    if LOGIN_USER['is_login']:
	        print('欢迎%s访问无涯后台管理系统'%LOGIN_USER['current_user'])
	    else:
	        print('请先登录系统，谢谢！')
	 
	 
	 
	def login(username,password):
	    if username=='admin' and password=='admin':
	        LOGIN_USER['is_login']=True
	        LOGIN_USER['current_user']=username
	        admin()
	    else:
	        print('用户名或者密码错误')
	 
	 
	def order():
	    if LOGIN_USER['is_login']:
	        print('欢迎%s访问无涯后台管理系统'%LOGIN_USER['current_user'])
	    else:
	        print('请先登录系统，谢谢！')
	 
	def main():
	    while True:
	        f=input('1、后台管理系统 2、查看订单 3、登录 4、退出系统\n')
	        if f=='1':
	            admin()
	        elif f=='2':
	            order()
	        elif f=='3':
	            username=input('请输入用户名:\n')
	            password=input('请输入密码:\n')
	            login(username,password)
	        elif f=='4':
	            sys.exit(1)
	main()

从如上的代码中可以看出，要登录到后台或者查看订单模块，都得登录，都执行的相同的代码，就是判断了用户是否登录成功，

如果么有，让用户先登录，下来就来使用装饰器来重构这部分代码，见实现的代码：

	import  sys
	 
	 
	LOGIN_USER={"is_login":False}
	 
	 
	def wuya(func):
	    def inner():
	        if LOGIN_USER['is_login']:
	            r=func()
	            return r
	        else:
	            print('请先登录系统，谢谢！')
	    return inner
	 
	def f1():
	    print('欢迎%s访问无涯后台管理系统'%LOGIN_USER['current_user'])
	 
	 
	@wuya
	def admin():
	    '''后台管理系统'''
	    f1()
	 
	 
	def login(username,password):
	    if username=='admin' and password=='admin':
	        LOGIN_USER['is_login']=True
	        LOGIN_USER['current_user']=username
	        admin()
	    else:
	        print('用户名或者密码错误')
	 
	 
	@wuya
	def order():
	    f1()
	 
	def main():
	    while True:
	        f=input('1、后台管理系统 2、查看订单 3、登录 4、退出系统\n')
	        if f=='1':
	            admin()
	        elif f=='2':
	            order()
	        elif f=='3':
	            username=input('请输入用户名:\n')
	            password=input('请输入密码:\n')
	            login(username,password)
	        elif f=='4':
	            sys.exit(1)
	main()

重构后的代精简很多，把判断用户登录的部分代码放在了装饰器中，这样就不需要写重复性的代码。


补充：

	# 装饰是一个函数，该函数需要另一个函数作为它的参数
	def my_shiny_new_decorator(a_function_to_decorate):
	
	    # 在装饰器的函数实现里面它定义了另一个函数: 他就是封装函数(wrapper)
	    # 这个函数将原来的函数封装到里面
	    # 因此你可以在原来函数的前面和后面执行一些附加代码
	    def the_wrapper_around_the_original_function():
	
	        # 在这里放置你想在原来函数执行前执行的代码
	        print "Before the function runs"
	
	        # 调用原来的函数(使用圆括号)
	        a_function_to_decorate()
	
	        # 在这里放置你想在原来函数执行后执行的代码
	        print "After the function runs"
	
	    # 这个时候，"a_function_to_decorate"并没有执行
	    # 我们返回刚才创建的封装函数
	    # 这个封装函数包含了原来的函数，和将在原来函数前面和后面执行的代码。我们就可以使用它了!
	    return the_wrapper_around_the_original_function
	
	# 想象你创建了一个你再也不想修改的函数
	def a_stand_alone_function():
	    print "I am a stand alone function, don't you dare modify me"
	
	a_stand_alone_function() 
	# 输出为: I am a stand alone function, don't you dare modify me
	
	# 现在你可以装饰这个函数来扩展它的行为
	# 只需要将这个函数传入装饰器，那它将被动态的包在任何你想执行的代码间，并且返回一个可被使用的新函数:
	a_stand_alone_function_decorated = my_shiny_new_decorator(a_stand_alone_function)
	a_stand_alone_function_decorated()
	#输出为:
	#Before the function runs
	#I am a stand alone function, don't you dare modify me
	#After the function runs

现在，你可能想在每次调用 ```a_stand_alone_function```的时候，真正被执行的函数是 ```a_stand_alone_function_decorated```。那很容易，只需要使用 ```my_shiny_new_decorator```返回的函数赋值给原来的 ```a_stand_alone_function```这个函数名(其实是个变量):

	a_stand_alone_function = my_shiny_new_decorator(a_stand_alone_function)
	a_stand_alone_function()

	#输出为:
	#Before the function runs
	#I am a stand alone function, don't you dare modify me
	#After the function runs
	
	# 你猜怎么着？这就是装饰器做的事情。