## OO与面向过程的对比
	1. 可以使用的OO技术来减少错误的隐患以及增强代码的可维护性。
	ABAP OO的年代史。
		<1> SAP Basis Release 4.5发布了ABAP OO的一个版本，引入了类接口的概念，并可以通过类来创建对象（实例化类）。
		<2> SAP Basis Release 4.6发布了ABAP OO的完全版本，引入了OO方式的重要概念继承（inheritance）,可以通过多个接口来建立一个复合的接口。
		<3> SAP WEB APPLICATION SERVER 6.10/6.20 SAP basis的下一代版本，在类之间引入了friendship的概念。
		并引入了对象服务（object service）可以把对象存储在数据库中。
		<4> SAP WEB APPLICATION SERVER 6.40引入了共享对象（Shared Objects）的概念,即允许在应用服务器的共享内存中存储对象。
		这样在这个服务器中的任何一个程序都可以访问它。
	2. 几个关键点：
         <1> ABAP OO是ABAP编程语言的扩展
         <2> ABAP OO 是向下兼容的
         <3> SAP发布ABAP OO是为了进一步增强代码的可重用性
      	 <4> 随着ABAP OO的发布，ABAP运行时支持面向过程和面向对象两种模式
	对于面向过程的模式，程序的运行通常是从screen的dialog module或selection screen的start-of-selection事件开始的。
	你在这些处理模块中操作全局变量来实现需求的功能。
	你可以通过内部的form和外部的function module来实现程序的模块化。
	这些过程除了可以操作全局变量外还可以具备内部的本地变量来协助实现内部的一些特定功能。

	对于OO编程，唯一的结构单位就是类，这里类的实例对象取代了全局变量。这些对象封装了应用的状态和行为。应用的状态是用属性来代表的它取代了面向过程中的全局变量。
	应用的行为是通过方法来实现的，他们用来改变应用的属性或者调用其它对象的方法。
	ABAP OO支持OO和面向过程的两种模式，这样在传统的ABAP程序（比如报表，模块池，功能池等）中你也可以使用ABAP对象类。
	在这些程序里你也就可以使用基于面向对象的新技术了，比如一些用户界面，避免了要想使用这些新技术必须重新编写程序。

	纯粹的ABAP OO模式，所有的代码都封装在类中。你的应用中并不直接触presentation layer(SAP Gui , Business Server Pages etc.),persistent data(database table,system file)。他们是通过类库中的相应服务类来提供的。比如SAP Control Framework ,Desktop Office Integration, and Business Pages提供了与表现层的接口。对于SAP Web Application 6.10以上提供了与数据库层接口的服务对象。
	虽然纯粹的OO模式技术上是可行的，但是现实中还存在着大量的两种模式的混合体。ABAP 对象和面向过程的技术同时应用，调用常用的功能模块，调用屏幕或者直接访问数据库等在对象中都存在。混合的模式及利用了新技术又保护了已投入的成本。
	两种模式的选择

## OO的基本使用
	1. 定义
		CLASS C1 DEFINITION.
			PUBLIC SECTION.(公有)
			DATA: A1…(动态属性)      obj_name->xyz
			METHODS: M1….(动态方法)
			EVENTS: E1….(动态事件)
									class_name=>xyz/obj_name=>xyz
			CLASS-DATA:CA1….(静态属性)
			ClASS-METHODS:CM1….(静态方法)
			CLASS-EVENTS:CE1….(静态事件)
			CONSTANTS:CO1….(常量)
			PROTECTED SECTION.(保护)
			DATA:A2…
			METHODS:M2..
			EVENT:E2…
			PRIBATE SECTIONS.(私有)
			DATA:A3..
			METHODS: M3…
			EVENT:E3.
		ENDCLASS.
	2. 实现
		CLASS C1 PLEMENTATION.
			METHOD M1.
			…….
			ENDMETHODS.
			METHOD M2.
			…….
			ENDMETHODS.
			METHOD M3.
			…….
			ENDMETHODS..
		ENDCLASS.
	3. 构造器
		<1> 每个类只有一个构造器
		<2> 运行时在Create object 语句中自动调用构造器
		<3> 必须在public section 中定义和应用构造器
