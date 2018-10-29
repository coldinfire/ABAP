## OO与面向过程的对比
	1. 可以使用的OO技术来减少错误的隐患以及增强代码的可维护性。
	ABAP OO的年代史。
	  <1> SAP Basis Release 4.5发布了ABAP OO的一个版本，引入了类接口的概念，并可以通过类来创建对象（实例化类）。
	  <2> SAP Basis Release 4.6发布了ABAP OO的完全版本，引入了OO方式的重要概念继承,可以通过多个接口来建立一个复合的接口。
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

	对于OO编程，唯一的结构单位就是类，这里类的实例对象取代了全局变量。这些对象封装了应用的状态和行为。
	应用的状态是用属性来代表的它取代了面向过程中的全局变量。
	应用的行为是通过方法来实现的，他们用来改变应用的属性或者调用其它对象的方法。
	ABAP OO支持OO和面向过程的两种模式，这样在传统的ABAP程序（比如报表，模块池，功能池等）中你也可以使用ABAP对象类。
	在这些程序里你也就可以使用基于面向对象的新技术了，比如一些用户界面，避免了要想使用这些新技术必须重新编写程序。

	纯粹的ABAP OO模式，所有的代码都封装在类中。你的应用中并不直接触presentation layer(SAP Gui , Business Server Pages etc.),
	persistent data(database table,system file)。他们是通过类库中的相应服务类来提供的。
	比如SAP Control Framework ,Desktop Office Integration, and Business Pages提供了与表现层的接口。
	对于SAP Web Application 6.10以上提供了与数据库层接口的服务对象。
	虽然纯粹的OO模式技术上是可行的，但是现实中还存在着大量的两种模式的混合体。
	ABAP 对象和面向过程的技术同时应用，调用常用的功能模块，调用屏幕或者直接访问数据库等在对象中都存在。
	混合的模式及利用了新技术又保护了已投入的成本。
	
## OO的基本使用
	1. 定义
	  CLASS C1 DEFINITION.
	    PUBLIC SECTION.(公有)
		DATA: A1…(动态属性)      obj_name->xyz
		METHODS: M1….(动态方法)
		EVENTS: E1….(动态事件)
								class_name=>xyz
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
	4. 对象及其引用
	  DATA: ov1 TYPE REF TO class_name.
	  CREATE OBJECT ov1.
	  WRITE:/ ov1->method().
	  CALL METHOD ov1->method().
	  自我引用:ME,包含当前对象的地址的引用
	  父类引用:super
	  指针内表:oref_tab TYPE TABLE OF REF TO class_name.(内表)
	  	  oref TYPE REF TO class_name.(结构)
	5. 方法的使用
	  参数接口:METHODS meth1
		IMPORTING VALUE(i1) / i1 TYPE type / LIKE dobj DEFAULT def1
		EXPORTING VALUE(e1) / e1 TYPE type / LIKE dobj
		CHANGING VALUE(c1)/ c1 TYPE type /LIKE dobj DEFAULT def2
		EXCEPTIONS x1
	  方法调用:CALL METHODS oref1 ->meth1
		[ EXPOTING i1 = a1…….in = an ]
		[ IMPOTING e1 = a1…….en = an ]
		[ CHANGING c1 = a1…….cn = an ]
		[ EXCEPTION x1 = r1 …. Other = rn] (a1-an为实际参数)
	6. 继承(单继承,不支持方法重载)
	  CLASS subclass DEFINITION INHERITING FROM superclass.(继承除private外的组件)
	    method(方法重写,功能扩展)
	  ENDCLASS.
	7. 抽象类
	  CLASS cl_supper DEFINITION.
	    PUBLIC SECTION.
	      METHODS:demo1 ABSTRACT,demo2.
	  ENDCLASS.
	  CLASS cl_sub DEFINITION INHERITING FROM cl_super.
	    PUBLIC SECTION.
	      METHODS demo1 REDEFINITION.
	  ENDCLASS.
	8. Final 类
	  CLASS final_class DEFINITION FINAL.
	    .....
	  ENDCLASS.
	  A.最终类不能再被继承
	  B.一个最终类的所有方法也都被视为最终的,不需要在方法中再声明FINAL.
	  C.最终方法不会被重定义(非最终类的情况下,可以在继承类出现).
	  D.在保护一个类或者避免方法被重新设定的时候可以才用FINAL关键字.
	  E.一个最终方法不能是抽象方法.
	  F.一个最终类可以是抽象类,但这种情况下只能使用它的静态组件,不能实例化.
	  
	9. 多态 依靠继承的功能和动态的引用实现
	  静态的定义
	  DATA: supr_obj type ref to super_class,
	    sub1_obj type ref to sub_class_one,
	    sub2_obj type ref to sub_class_two.
	  START-OF-SELECTION.
	    CREATE OBJECT sub1_obj.
	    CREATE OBJECT sub2_obj.

	  动态的引用方式
	  supr_obj = sub1_obj.
	  CALL METHOD supr_obj->test_method.
	  supr_obj = sub2_obj.
	  CALL METHOD supr_obj->test_method.
	10. 接口
	  INTERFACE intr1.
	    METHODS m1.
	  ENDINTERFACE.
	  INTERFACE intr2.
	    METHODS m2.
	    ALIASES meth1 FOR I1~m1.
	  ENDINTERFACE.
	  
	  CLASS c1 DEFINITION.
	    PUBLIC SECTION.
		  INTERFACES intr2.
		  ALIASES meth2 FOR intr2~m2.
	  ENDCLASS.
	  
	  CLASS c1 IMPLEMENTATION.
	  	METHOD I1~m1.(INTERFACE ~ METHOD:接口方法在类中的实现)
		  DATA:num TYPE I VALUE 10.
		  WRITE:/ num.
		ENDMETHOD.
		METHOD I2~m2.
		  ...
		ENDMETHOD.
	  ENDCLASS.
	  
	  START-OF-SELECTION.
	    DATA: oref TYPE REF TO c1.
		CREATE OBJECT oref.
		CALL METHOD: oref->I2~meth1.
		CALL METHOD: oref->meth2
	  a.接口的组件全名称能够使用intf~comp语句被添加到其它接口或类中.
	  b.别名是用来代替接口中组件名称,在类的实现部分,通过别名来代替组件名称.
	  c.在类或类的对象调用的时候,必须使用接口的组件全名称.
	11.Event
	  事件的工作机制:在ABAP中，我们使用发布和预定的机制(publish and subscribe).类声明一个事件,通过实现一个方法来发布这个事件
	  (事件挂在一个方法上,事件一触发就会调用这个方法)一个类可以实现一个方法来预定这个事件.
	  步骤:
	  	<1> 在类中定义事件
		<2> 实现句柄方法，把事件与要触发这个事件的类关联起来
		<3> 注册事件方法	
