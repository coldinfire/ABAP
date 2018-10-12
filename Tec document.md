# learn target
	1. ABAP语法详解
	全面掌握SAP的开发环境，ABAP语法等。
	2. 报表开发全面总结
	全面总结SAP系统中各种类型报表的实现方案和技术细节，让学习者在今后项目中可以应对各种形式报表开发的需求
	3. 表单开发全面总结 
	全面讲解SAP系统各种表单的设计、使用和配置，以及标准表单的修改等。具体包括 SCRIPT FORM、SMART FORM、ADOBE FORM等。
	4. 屏幕程序开发全面总结
	系统掌握屏幕程序的原理，各种复杂界面的实现方法，让学习者完全达到轻松实现SAP系统各种标准程序界面的目的
	5. 定制开发技术完全总结
	全面总结定制化开发的各种技术，包括定制开发的原则，标准程序修改，标准程序增强技术等
	 满足不同企业定制化的业务流程，操作界面或系统功能的定制要求。通过该章节的学习，学习者可以轻松应对项目中各种非标准功能的实现方法!
	6. 接口实现技术全面总结
	系统讲解SAP和外部系统的整合技术，让学习者能用最有效的方式实现SAP与外部系统间的数据交换，
	具体包括BAPI技术、IDOC技术、RFC技术、EDI技术，XI技术介绍等。
	7. 数据传输技术全面讲解
	全面讲解SAP系统提供的各种数据导入，更新技术及其各种应用，主要包括BDC技术、LSMW技术、CATT技术、程序方式等。
	8. 数据库更新技术
	掌握事务处理的相关知识和原理，掌握数据库更新类程序开发的要求。
	9. 面向对象程序设计
	掌握ABAP中面向对象程序开发的原理，方法和运用。 
	10. 数据分析技术总结
	作为高级开发顾问，我们常常要独立面对客户需求，这个时候我们不仅要精通各种开发技术还要了解业务，便于理解需求，
	同时我们还必须掌握从业务流程、业务操作到程序逻辑之间的转化。该章节就是详细讲解这种分析技术。
	11. 程序效能全面总结
	很多初级开发人员的程序可维护性非常差，后期维护成本很高，给咨询公司和客户带来很多麻烦，怎样让开发的程序具有高可维护性，
	让我们的程序可读性强，结构清晰，可配置性好，可移植性方便。该章节将系统介绍这方面的准则和经验，
	同时也让学习者系统掌握程序优化的各种技巧和知识
	12. WebDynpro开发技术全面讲解，演练
	Web化程序代表SAP今后的开发方向，本部分课程将系统讲解WEBDYNPRO开发的具体实现。
	理论结合案例，让学习者系统掌握WEBDYNPRO程序的实现方法
# 一、内表&报表
----------
## 1. 定义内表和工作区
	1. with header line,用itab[]和itab来区分内表和工作区
	2. 分别定义内表和工作区
	3. 定义结构时用occur 0直接定义
	4. LIKE LINE OF后面接一个内表，表示一个data参数具有和内表一样的结构，可当做Work Area使用
	5. LIKE TABLE OF后面接一个structure，表示一个data参数是一个内表，这个内表和后面接的结构一样
## 2. 报表程序大体逻辑结构
	 抬  头: 报表的主要信息(抬头信息)
	 行项目: 查询出的每行记录信息
	 明  细: 每个行项目出关键字外其他明细内容
## 3.清空内表和工作区
	CLEAR、REFRESH、FREE	
		内表：如果使用有表头行的内表，CLEAR 仅清除表格工作区域。要重置整个内表而不清除表格工作区域，使用REFRESH语句或 CLEAR 语句CLEAR <itab>[]；
		REFRESH加不加中括号都是只清内表，另外REFRESH是专为清内表的，不能清基本类型变量，但CLEAR可以
		以上都不会释放掉内表所占用的空间，如果想初始化内表的同时还要释放所占用的空间，请使用：FREE <itab>.
## 4.字段符号：filed-symbols
	4.1 定义：
		FIELD-SYMBOLS <FS> LIKE structure.
		<1> FS必须和某个变量，结构或者内表绑定后才能使用，这点和C语言里的指针（在ABAP里最接近指针的是TYPE REF TO）是不同的，在使用FS前必须分配给某个变量，
		不然会发生FS未分配的运行时错误。注意如果LOOP内表时ASSIGNING到FS，那么之后假如有REFRESH内表的操作的话，FS也会再次回到初始未被ASSIGN的状态，
		这时如果使用FS也会发生FS未分配的RUNTIME ERROR。
		<2> ASSIGN structure TO <fs>:将某个内存区域分配给字段符号，这样字段符号就代表了该内存区域，即该内存区域别名
		字段符号可以看作仅是已经被解引用的指针（类似于C语言中带有解引用操作符 * 的指针），但更像是C++中的引用类型（int i ;&ii= i;），
		即某个变量的别名。
		ASSIGN xxxx TO <fs>：将某个内存区域分配给字段符号，这样字段符号就代表了该内存区域，即该内存区域别名。
		UNASSIGN：该语句是初始化<FS>字段符号，执行后字段符号将不再引用内存区域，<fs> is assigned返回假
		CLEAR：与UNASSIGN不同的是，只有一个作用就是初始化它所指向的内存区域，而不是解除分配
	4.2 	LOOP内表INTO结构（工作区）和LOOP内表ASSIGNING<结构>的比较
		LOOP内表INTO结构时，系统会把先把当前行的数据复制到结构，如果结构的值改了，还需要使用MODIFY语句把更改后的值传回内表。
		也就是说，结构是内表里的数据的一个副本，操作这个副本并不会影响内表里的数据。
		为了提高效率，可以使用FS，FS直接指向内表数据，省去了复制数据到结构的过程，修改FS的值也就是相当于直接修改内表里的数据，不需要再使用MODIFY语句。
	4.3 READ TABLE内表
	4.4 ASSIGN隐式强转
		TYPES: BEGIN OF t_date,
		  year(4) TYPE  n,
		  month(2) TYPE n,
		  day(2) TYPE n,
		END OF t_date.
		FIELD-SYMBOLS <fs> TYPE t_date."将<fs>定义成了具体限定类型
		ASSIGN sy-datum TO <fs> CASTING. "后面没有指定具体类型，所以使用定义时的类型进行隐式转换
	4.5 ASSIGN显示强转
		DATA txt(8) TYPE c VALUE '19980606'.
		FIELD-SYMBOLS <fs>.
		ASSIGN txt TO <fs> CASTING TYPE d."由于定义时未指定具体的类型，所以这里需要显示强转
## 5.others 
	1. 标记字段的使用有时可以使程序的逻辑更加清晰
	2. 字符串连接：&& 替代 CONCATENATE
## 6.ALV 报表的表头字段显示设置
	call function 'LVC_FIELDCATALOG_MERGE'
		exporting
	      I_BUFFER_ACTIVE        = I_BUFFER_ACTIVE
		  I_structure_name       = 'ZALV_FEED' "ALV需要显示的字段结构
		  I_CLIENT_NEVER_DISPLAY = 'X'
	      I_BYPASSING_BUFFER     = I_BYPASSING_BUFFER
	      I_INTERNAL_TABNAME     = I_INTERNAL_TABNAME
		changing
		  ct_fieldcat            = lt_fieldcat  "对应ALV显示的字段结构
		exceptions
		  inconsistent_interface = 1
		  program_error          = 2.
	可以通过loop对相应的ALV字段描述进行自定义设置
## 7.Layout 字段
	zebra	type  c  	   (斑马显示)
	edit	 type  c    	 (可编辑的)
	colwidth_optimize type c			 (自动宽高)
	info_fieldname type slis_fieldname   (设置行颜色)
	coltab_fieldname type slis_fieldname (单元格颜色)
## 8.Fieldcatalog 字段
	key type c						(关键字)
	col_pos like sy-cucol			 (列输出位置)
	fieldname  type slis_fieldname    (列显示的设置)
	cfieldname type slis_fieldname	(金额字段所参照的货币单位字段名)
	ctabname   type slis_tabname	  (金额字段所参照的货币单位表名)
	qfieldname type slis_fieldname	(数量字段所参照的货币单位字段名)
	qtabname   type slis_tabname	  (数量字段所参照的货币单位表名)
	just    type c   			     (对齐方式)
	lzero   type c   				 (为X时输出前导零)
	no_zero type c	 		   	(空值不输出)
	no_sign type c					(不显示数字符号)
	seltext_l,seltext_m,seltext_s	 (文本描述)
	convexit 					 	(设置转换规则，对应于Domain中的转换规则)	
## 9.单元格中的数据被修改后，将ALV单元格中的数据立即刷新到ABAP对应的内表中
	方法一：通过对REUSE_ALV_GRID_DISPLAY函数参数i_grid_settings-edt_cll_cb进行设置：
			i_grid_settings-edt_cll_cb  = 'X' .
			CALL FUNCTION 'REUSE_ALV_GRID_DISPLAY'
			EXPORTING i_grid_settings = i_grid_settings
	方法二：通过函数参数I_CALLBACK_USER_COMMAND指定的回调Form的参数slis_selfield进行设置：
			FORM user_command USING ucomm LIKE sy-ucommselfield selfield TYPE slis_selfield.
				selfield-refresh = 'X'.
			CASE ucomm.
				WHEN 'UPDATE'.
				PERFORM frm_update.
			ENDCASE.
			ENDFORM. 
## 10.删除重复
	(1) SORT record_tab. //首先进行排序处理
	(2) DELETE ADJACENT DUPLICATES FROM record_tab COMPARING ALL FIELDS. //进行去重复操作
## 11.宏定义
	1. 定义：
		DEFINE operation.
	  		result = &1 &2 &3.
	  		output   &1 &2 &3 result.
		END-OF-DEFINITION.
		
		DEFINE output.
	  		write: / 'The result of &1 &2 &3 is', &4.
		END-OF-DEFINITION.
	2. 使用：
		operation 4+3.
## 12.MESSAGE
	1. 消息ID MESSAGE e001(00) WITH '可以传递8个参数'. //利用定义的参数
	2. MESSAGE 'XXXXXXXXXX' TYPE 'X'.//直接附加消息
## 13.前导零问题
	加上p_in的前导零
	 FORM add_zero  CHANGING p_in.
	  CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT'
	    EXPORTING
	      input  = p_in
	    IMPORTING
	      output = p_in.
	 ENDFORM.
	去除p_out的前导零
	 FORM del_zero CHANGING p_out.
	  CALL FUNCTION 'CONVERSION_EXIT_ALPHA_OUTPUT'
	    EXPORTING
	      input  = p_out
	    IMPORTING
	     output = p_out.
## 14.小数后面的0去除问题
	form DATA_DELETE_ZERO  using p_field z_result.
	   DATA:var1 TYPE p DECIMALS 3,
	        var2 TYPE p DECIMALS 2,
	        var3 TYPE p DECIMALS 1,
	        var4 TYPE i.
		    move p_field to var1.
		    move p_field to var2.
		    move p_field to var3.
		    move p_field to var4.
		    IF var2 = var1.
		      IF var3 = var1.
		        IF var4 = var1.
		          z_result = var4.
		        ELSE.
		          z_result = var3.
		        ENDIF.
		      ELSE.
		        z_result = var2.
		      ENDIF.
		    ELSE.
		      z_result = var1.
		    ENDIF.
	endform.   
## se91 消息管理
# 二、屏幕操作
----------
## 1.界面显示字段的设定方式
	1. Text symbols 设定值
	2. 在每一行单独添加 ex： 
	      SELECTION-SCREEN BEGIN OF LINE.
				 SELECTION-SCREEN COMMENT n(m) TEXT-001 FOR FIELD S_XX.
				 SELECTION-OPTION: S_XX FOR XX_XXX.
		  SELECTION-SCREEN END OF LINE.
## 2.[SCREEN设计](https://blog.csdn.net/Jay_1989/article/details/51611315)
		ABAP程序与Dialog屏幕进行数据交换的方式，通过在程序中定义一个与Dialog中同名的全局变量或者结构，从而实现数据的自动传递。
		“加载数据”：在PBO中实现
		“推送数据”：在PAI中控制
		. PBO:Before the screen is displayed
		. PAI: After a user action on the screen
		. POH: when F1
		. POV: When F4
		Screen Flow Logic定义了在screen上的各种事件，而Dynpro定义了这个Screen和各种事件。
		而 ABAP MODULE POOL会处理这些event, 在每次事件时调用一个ABAP program。
	语法：
		Module ...  END MODULE 
		set screen xxxx     call screen xxxx  	leave screen.          
		Call screen xxx  starting at lc ur
					     ending at rc lr.   (Modal dialog box类型)
		set cursor field <f> [offset <o>]  
		       
	Static Attributes：
		Screen-name，screen-group1，screen-group2, screen-group3, screen-group4， screen-length
		screen-input，screen-output，screen-required，screen-intensified，screen-invisible，screen-active
	
		MODULE EXIT AT EXIT-COMMAND. 
	
	字段检查与逻辑流的控制：
		相关语法：
		1、单个字段检查
			FIELD<FLD1>MODULE<MDL1>.
		
		2、单个字段多个MODULE检查，如FLD1有两个MODULE检查
			FIELD<FLD1>MODULE<MDL1>.
			           MODULE<MDL2>.
		
		3、检查多字段，使用CHAIN
			CHAIN.
			FIELD<FLD1>.
			FIELD<FLD2>,<FLD3>,<FLD4>.
			MODULE<MDL1>.
			MODULE<MDL2>.
			ENDCHAIN.
			表示FLD1,FLD2,FLD3,FLD4有MDL1与MDL2检查。
		
		4、不是初始值检查
			FIELD<FLD1>MODLE<MDL1>ON INPUT.
			ON INPUT表示是初始值改变时执行.
			有一个特殊情况：
			FIELD<FLD1>MODULE<MDL1>ON *-INPUT
			表示用户输入字段首先输入'*'，并且输入字段属性，MODULE无效。
			
		5、有改变检查
			FIELD<FLD1>MODULE<MDL1>ON REQUEST.
		
		6、CHAIN中有字段不是初始值检查
			CHAIN.
			FIELD<FLD1>.
			FIELD<FLD2>,<FLD3>,<FLD4>.
			MODULE<MDL1>ON CHAIN_INPUT.
			MODULE<MDL2>.
			ENDCHAIN.
			注意：CHAIN_INPUT表示FLD1,FLD2,FLD3,FLD4不是初始值是执行MDL1检查。

	PBO：CALL SUBSCREEN <SUBAREA> INCLUDING <PROGRAM_NAME> <DYNPRO_NUMBER>.
	PAI：CALL  SUBSCREEN <SUBAREA>.     
		<program_name> = sy-cprog : 代表本程序
	
	ON CHANGE OF : 当指定字段修改才会去读取数据

# 三、TABLE
----------
## 1.Hash table, sort table, standy table
	对于排序内表，不要使用append，append lines附加数据，使用insert、insert lines向排序内表中插入数据
## 2.search help 的创建
	
## 3.create table
	(1) Field：定义Short Description 分配Element应注意命名，缩略
	(2) Element：定义Field Label 和分配相应的Domain
	(3) Domain：定义具体的字段属性，Data type，NO.Cahracters，Decimal Places，Output Length
	(4) SM30：表维护   Utilities ----->Table maintenance generator
			Technical Dialog Details
				Authorization Group：&NC&
				Function group：ZFG_tablename
			Maintenance Screens : 
				Maintenance type：one step
## 4. 表操作
	双层loop循环时，第二个内部表定义为SORTED TABLE会大大的提高处理速度
## 5. SQL 语句的执行顺序
	书写顺序：SELECT[DISTINCT]-->FROM-->WHERE-->GROUP BY-->HAVING-->UNION-->ORDER BY
	其执行顺序为：FROM-->WHERE-->GROUP BY-->HAVING-->SELECT-->DISTINCT-->UNION-->ORDER BY
	1、FROM 才是 SQL 语句执行的第一步，并非 SELECT 
	2、SELECT 是在大部分语句执行了之后才执行的，严格的说是在 FROM 和 GROUP BY 之后执行的。
	3、无论在语法上还是在执行顺序上， UNION 总是排在在 ORDER BY 之前。
# 四、Open SQL
----------
## Focus
	1. 多表的结合的查询，首先进行各自的关键字查询，然后进行表与表转换可以提升效率
	2. 表相应字段非关键字时，寻找有该关键字的相对应的关联表进行查询并合并表数据
	3. binary search 和 sort 进行使用时注意sort的关键字与binary search 保持统一
	4. MODIFY itab1 INDEX sy-tabix.进行修改时最好定义变量记录sy-tabix.
	5. inner join 和 left outer join 的连接条件最好都是关键字.

# 五、Debug
----------
## Focus
	1. 在进行断点设置时应注意避免循环体内断点
	2. Debug时注意前导0相对应的情况
	3. Debug 当发生有LUW时，后台数据库底层会进行Update，需要进行软断点的设置才能够跟踪debug

# 六、OO
	1. 首先ABAP OO具有更高层次的数据封装性，从而增强了程序的可维护性和稳定性。在面向过程的模式里，一个程序的全局变量区包含着许多不相干的变量，
	这些变量在一个区域里交织在一起，这样的话这个程序的某个功能对程序状态的改变并不为程序中另外一个功能所知。为保持整个程序的稳定性需要大量的约束和维护成本。
	而对于OO模式，状态是保存在对象上的。对象可以把内部和外部数据和方法分隔开，以保证功能（OO中的方法）只能改变与它相关的数据，对象的属性是不会被改变的，
	从而保证了其在应用程序中的稳定性。
	2. ABAP OO可以实现一个类的多个实例。（对象是由数据以及操作数据的方法组成的），每一个对象都有自己在类中定义的属性值，并且可以通过本身的方法来改变本身的状态。
	这就意味着开发者无需为每个对象建立数据和方法之间的联系也无需手工管理每个对象的生命周期。在面向过程的方法中没有多实例的概念，数据和功能是相互分离的。
	你使用的是无状态的功能，并且每次都需要通过传递参数来初始化它，并且手工将其占有的内存清除。
	3. ABAP OBJECT通过继承进一步增强了程序代码的可重用性，这正是面向对象方法的一个重要方面。
	通过这个特点你可以重复利用所继承类的部分或者所有方法，只需编写类本身所特有的方法，这样会减少为每个类编写的方法，从而增强了程序的可维护性。
	而对于面向过程的编程来说，你将会受制于all-or- nothing的状况，你要么调用所有的部分要么就建立新的。
	4. ABAP OO是你可以通过接口（interface）来调用对象的业务逻辑，而不是直接去使用对象，这样就避免了你需要详细了解每一个对象的特定功能。
	这样也避免了你在需要修改某项功能的时候无需修改接口（interface）的内容，这也为标准程序的修改提供了新的途径，即BADI。
	面向过程的编程并没有这种提 供对立的接口去联系某个对象的概念—接口只是通过form或function module的参数实现的。
	5. ABAP OO非常容易与事件驱动的模式结合在一块。不同的应用之间是通过发布与预定松散的耦合在一起的，调用者和被调用者之间并不是被动的绑定在一起的。
	这种方式与面向过程的方法相比更进一步的增强了灵活性，他们的绑定是紧密地，程序的执行流程也是预定义好的。
	
	通过使用对象的方法来代替form和function module你仍然可以增强你的ABAP程序。
	1. ABAP OO更加明确所以更易于使用。例如在使用ABAP OO你的程序的执行流程不再是由运行时隐含的控制。
	这样你就可以自己去设计程序所执行的流程了而不必像面向过程那样去了解和服从外部控制机制（即报表和 dialog screen的事件）。
	2. ABAP OO具有更加清晰的语法和语义规则，比如一些容易出错的过时的语句在ABAP OO类中已经明确不能再使用。
	而在面向过程的程序中这些语法仍然被支持，顶多就是在关键的时候给你报个警告信息。
	3. ABAP的一些新技术只能通过ABAP OO来实现。例如所有新的GUI的概念比如SAP Control Framework和BSP只有通过ABAP OO的方式才能够实现。
	而对于面向过程的ABAP你就只能使用传统的screen和list processing了。
	
	可以使用的OO技术来减少错误的隐患以及增强代码的可维护性。
	ABAP OO的年代史。
		1. SAP Basis Release 4.5发布了ABAP OO的一个版本，引入了类接口的概念，并可以通过类来创建对象（实例化类）。
		2. SAP Basis Release 4.6发布了ABAP OO的完全版本，引入了OO方式的重要概念继承（inheritance）,可以通过多个接口来建立一个复合的接口。
		3. SAP WEB APPLICATION SERVER 6.10/6.20 SAP basis的下一代版本，在类之间引入了friendship的概念。
		并引入了对象服务（object service）可以把对象存储在数据库中。
		4. SAP WEB APPLICATION SERVER 6.40引入了共享对象（Shared Objects）的概念,即允许在应用服务器的共享内存中存储对象。
		这样在这个服务器中的任何一个程序都可以访问它。
	几个关键点：
         1. ABAP OO是ABAP编程语言的扩展
         2. ABAP OO 是向下兼容的
         3. SAP发布ABAP OO是为了进一步增强代码的可重用性
      	  4. 随着ABAP OO的发布，ABAP运行时支持面向过程和面向对象两种模式
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
	
	
# 六、SMARTFORMS
----------
## Focus
	1. 输入参数和在smartform中显示的布局
	2. 对text输出格式进行相对应得编程调整和取数问题
	3. 查找程序中调用的smartform及其对应的程序段
		NACE----->选中所属模块----->output types----->选中输出类型----->processing routines----->查看对应信息

# 七、权限问题
## 1.简介
	SU01: 查看和编辑Role	
	SU20: 权限字段清单，可以增、删、改权限字段，可以浏览字段具体被哪些权限对象所调用
	SU21: 维护权限对象，可以创建和维护权限类，权限对象，权限字段在该事务码中被分配到权限对象
	SU22: 维护权限对象的分配，可以通过该事物代码为具体事务分配权限对象
	SU24: 将Authorization Object Assign 到 T-Code 上
	SU53: 当前用户权限不足问题查看
	SUIM: 用户、角色、权限对象、事务等之间的关系查看
	PFCG: 进入权限角色维护界面，创建Role 设置Role的Authorization Object
	角色(Role): 用于给用户分配具体的权限菜单，可以把相关操作的菜单分配到某个角色中，每个角色可以分配给多个用户，每个用户也可以同时分配多个角色。
		 通用角色(Common Role)、本地角色(Local Role)：本地角色较之通用角色的区别是，在同样的操作权限情况下本地角色多了具体的限制值。
	参数文件: 每个角色都会有对应的参数文件，SAP通过参数文件检查用户访问呢系统的权限。(每个角色只对应一个参数文件)
	组(Group): 权限检查用户组，与“登录数据”页面中的用户组是配合使用的，可以实现对用户管理的分散维护。
		通过事务代码SU10实现对用户组的批量维护。
	权限角色: 用户的权限菜单时通过权限角色分配实现的。角色维护又分为单一角色和符合角色，单一角色是一个独立的权限对象，复合角色可以有多个单一角色组合而成。
		菜单: 用于定义并分配该角色所能操作的事物代码。
		权限: SAP会根据对该角色的菜单项所分配的角色列出具体的权限对象，可以设置权限对象的具体的参数值。
			 为某个角色分配具体的权限数据后，会自动产生一个参数文件。
		用户: 将权限角色分配到SAP具体的用户账号。
			 需要点击用户比较，并点击完成比较即可。		 
## 2.权限对象	
	角色包含了若干权限对象、在透明标AGR_1250中存储二者之间的关系。
	在SAP实际应用中，用户所直接操作的是屏幕及屏幕对应的字段，这些字段都是由权限对象进行控制，包括该字段所允许的操作及所允许的值。
	SAP权限对象(Authorization Object)： 权限对象设置后，需要绑定到事务码上，然后在ABAP程序中通过AUTHORITY-CHECK OBJECT 语句来做权限检查。
		权限对象就起作用了。
	权限对象包含了若干权限字段、允许的操作和允许的值，在透明表AGR_1251中体现了ROLE/Object/Field/Value之间的关系；
	有一个特殊的权限对象用来包含了若干事务码。这个权限对象叫“S_TCODE”，该权限对象的权限字段叫“TCD”，该字段允许的值（Field Value）存放的就是事务代码；
	在透明表USOBX中，存放了事务码与权限对 象的对应关系。 
	权限字段(Authorization Field)： 分配取值 
		ACTVT:该字段存放的就是允许操作的代码。
		TCD: 存放该权限角色所包含的事物代码。
	允许的操作(Activity):  维护具体的选项操作。
	允许的值(Field Value): 值代表每个选型的功能。
		保存并激活后使该权限参数文件生效。
## 3.在程序中调用权限对象
		AUTHORITY-CHECK OBJECT 'XXX'
				ID 'XXX1' FIELD field
				ID 'XXX2' FIELD field
				ID 'XXX3' FIELD field
				ID 'XXX4' FIELD field.
		IF SY-SUBRC <> 0.
			MESSAGE 'XXXXX'
		ENDIF.
		OBJECT: 表示具体的权限对象。
		ID： 表示权限字段，可以同时检查权限对象中的一个或多个权限字段。
		FIELD： 权限字段所检查的权限值，将该字段改为屏幕元素，可检查该字段所输入的值是否符合要求。
*类似一个大致的权限矩阵，纵向是操作人（ID），横向是某些权限对象，权限对象再细分成若干事务代码、允许动作、权限字段及其允许的值等。*
	可参考:[SAP用户权限控制设置及开发](https://blog.csdn.net/candy_mmyy/article/details/54906571)、 [用SAP Authority Object 对权限控制](https://www.cnblogs.com/long2006sky/archive/2009/06/07/1498029.html)
## 4.权限授权解决
	SU01: 输入UserID查看和编辑Role
		  Role: menu(T-Code)、Authorization(Display Auth)、Organization levels
	SU53: 当前用户权限不足的原因
	SUIM: 查找用户及Role等的具体信息，确定问题点，及可以解决的方法
	PFCG: 维护Role、Menu修改
	Role Menu: SAP_(SAP 标准)       Z_WIK_ALL(用户无关)
			   Z_U_(混合角色不适用)  Z_模块名_(对应的Role编码)

# 八、增强(Enhancement)
----------
## 1.隐式增强，需要定位增强点，然后进行相应的增强确认。
		(1)定位到合适的增强点
		(2)EDIT->ENHANCEMENT OPERATIONS->SHOW IMPLICIT ENHANCEMENT OPTIONS
		(3)点击增强按钮，光标要停留在红点上的“|”后面，然后在菜单栏选择：
		   EDIT->ENHANCEMENT OPERATIONS->CREATE->CODE，输入增强实现的名称和描述。
		(4)加入增强代码
## 2.修改增强
		(1)点击增强按钮
		(2)将光标定位到增强的名称上
		(3)EDIT->ENHANCEMENT OPERATIONS->CHANGE
## 3.SAP标准系统的修改
	对SAP标准系统的修改可分为以下的几个层次：
		1. 配置（Customizing）：配置是最基本的
		2. SAP软件更改。使用事务代码SPRO进行系统配置。
		3. 个性化（Personalization）：个性化是指通过设置用户参数等方式更改SAP功能展现形式。
				个性化的方式主要包括设置字段默认值/隐藏字段、配置事务变式等。
		4. 更改（Modification）：更改是指对SAP标准系统代码进行修改。
				更改的方式主要包括使用Modification Assistant直接修改源代码、使用用户出口（User exits）实现增强。
		5. 增强（Enhancement）：增强是指实现SAP标准系统预留的接口。
				增强的方式主要包括客户出口（Customer exits)、BTE、BADI等。
		6. 客户化开发（Customer development):客户化开发是指在在SAP平台上完全客户化开发程序。客户化开发可调用标准系统的知识库。
	若需要对标准系统进行更改，应遵守以下的原则：
		1. 应优先使用配置的方式实现
		2. 若SAP有类似的功能，只需要做一些调整，那么首先应该考虑使用由SAP提供的增强方式实现
		若SAP没有类似的功能，那么可以选择：
			<1> 使用CSP（Complementary Software Product）。可在SAP Service Marketplace中查找经过SAP认证的CSP解决方案.
			<2> 复制出标准程序进行修改
			<3> 修改标准程序，值得注意的是，修改标准程序会导致在升级时出现问题，因此应尽量不使用。
			    如果可行的话，也应该将标准程序拷贝出来进行修改，而不是直接修改。
# 九、RFC远程调用
	
# 十、优化
----------
## 数据库
	1. 不要使用 SELECT * ...，选择需要的字段, SELECT * 既浪费CPU，又浪费网络带宽资源，还需占用大量的ABAP内存
	2. 不要使用SELECT DISTINCT ...，会绕过缓存，可使用 SORT BY + DELETE ADJACENT DUPLICATES 代替
	3. 少用相关子查询，因为子查询对外层查询结果集中的每条记录都会执行一次
	4. 少用嵌套SELECT … ENDSELECT，可以使用联合查询或FOR ALL ENTRIES来替换,减少循环次数
	5. 如果确定只查一条数据时，使用 SELECT SINGLE... 或者是 SELECT ...UP TO 1 ROWS ...
	6. 统计时，直接使用SQL聚合函数，而不是将数据读取出来后在程序里再进行统计
	7. 使用游标读取数据，这样省掉了将从数据库中的取记录放入内表的INTO语句这一过程开销
	8. 多使用inner join，必要时才使用left join
	9. inner join获取数据时，尽量不要用太多的表关联，特别是大表关联，关联顺序为：小表-大表
	10. where 条件里面多用索引、主键，顺序也要遵循小表-大表
	11. inner join条件放置的位置应该按照 On、Where、Having的顺序放，因为SQL条件的的执行一般是按这个顺序来执行的，将条件放在最开始执行，则可过滤掉大部数据；
		但要注意Left Outer Join，是否可以将ON中的条件移动到Where从句则要考虑（如果真能放在Where从句中，则应该使用Inner Join，而非Left Outer Join，
		因为Where条件会过滤掉那些包括在右表中不存在的左表数据），因为此时条件放在On后面与放在Where语句后面结果是不一样的（因为不管on中的条件是否为真，
        左表中在右边表不存在的数据也会被返回，但如放在where条件中，则会对On产生的数据再次过滤的条件，会滤掉不满足条件的记录——包括左表在右表中找不到的记录，
        这时已经没有left join的含义）
	12. 要根据主键或索引字段查找数据，且WHERE从句中的条件字段需按INDEX字段顺序书写，且将索引字段条件靠前（左边），检查条件组合字段是否是主键，
	    或者是上在上面创建了索引，避免条件组合字段即不是主键又没有索引。
	13. SELECT语句WHERE条件，应该先将主键相关条件放在前面 然后按照比较符 = 、< 、>、 <> 、LIKE IN 的顺序排列WHERE条件
	14. 使用部分索引字段问题：如果一个索引是由多个字段组成的，只使用一部分关键字段来进行查询时，也是可以使用到索引，但使用时要注意要按照索引定义的顺序且取其前面部分。
	15. 请根据索引字段进行ORDER BY，否则通过程序进行SORT BY。与其在数据库在通过非索引字段进行排序，不如在程序中使用SORT BY语句进行排序，
	    因为此情况下应用服务器上的执行速度要比数据库服务器快(应用服务器上采用的是内存排序)。
	16. 避免在索引字段上使用：not、<>、!=、IS NULL、IS NOT NULL,可以用> 与 < 来替代避免使用 LIKE，但LIKE '销售组1000'和LIKE '销售组1000%'可以用到，
		而LIKE '%销售组1000'（百分号前置）则用不到索引不要使用OR来连接多个索引字段(但同一字段多个值之间可以使用OR)；
		对于同一索引字段，可以使用IN来替代OR：带有BETWEEN 的WHERE 条件不能通过索引来搜索？也可使用IN代替。
	17. 避免使用以下语句，因为使用这些语句时，不能使用 Table Buffer：
		Aggregate expressions
		Select distinct
		Select … for update
		Order by、group by、having从句
		Joins，使用JOIN时，会绕过SAP缓存，可以使用FOR ALL ENTRIES来代替
		WHERE从句中使用Sub queries（子查询）
		WHERE从句中使用IS NULL条件
	18. 在下面情况下使用FOR ALL ENTRIES IN:
		在循环内表时Loop...at itab时
		join超过三个表会出现性能问题，当使用JOIN连接超过3个表时
		表数据非常大时，使用JOIN会很慢，此时改用FOR ALL ENTRIES IN
	19. 使用内表批量操作数据库，而不要使用工作区一条条操作,如：
		SELECT ... INTO TABLE itab
		INSERT dbtab FROM TABLE itab
		DELETE dbtab FROM TABLE itab
		UPDATE/MODIFY dbtab FROM TABLE itab			 
## 程序
	 1. READ TABLE ...WITH [TABLE] KEY...BINARY SEARCH读取标准内表使用二分查找
	 2. 在循环（LOOP AT ...WHERE..）或查询（READ TABLE ...）某内表时，如果未使用索引（排序表、哈希表）或二分查找，则在查询组合字段创建第二索引，
	 查询时通过USE KEY或WITH [TABLE] KEY选项使用第二索引，这样在查询时会自动 进行二分查找或哈希找查在没有用二分查找的情况下，
     可在查询组合字段上创建第二索引（哈希或排序索引），则在读取或循环内表时会自动使用二分查找或哈希查找算法
	 3. 查找时，优先考虑使用哈希表进行查找，再考虑使用排序表进行二分查找，因为哈希查找的时间复杂度为(O (1))，不会因数据的增加而受到影响；
	 而二分查找虽然比顺序搜索快很多，但随着数据的增加会慢下来，其时间复杂度为(O (log2n))；标准内表的时间复杂度为O(n)。
	 注：如果只使用到部分关键字为搜索条件，哈希表则会全表扫描，此时应该使用二分找查
	 4. FOR ALL ENTRIES：需要判断内表是否为空，否则会查询出所有数据
	 5. LOOP AT itab... ASSIGNING ...、READTABLE ...ASSIGNING ... 在循环或读取内表 时，使用字段符号来替换表工作区，将数据分配给字段符号Field Symbols，减少数据来回传递
	 6. 尽量避免嵌套循环，如必须时，将循环次数少的放在外层，次数多的放在内层，这样可以减少在不同循环层之间的频繁地切换及内部循环次数
	 7. 条件语句中多使用短路与或，“与”连接时将为假的机率大的条件放在前面，“或”连接时将为真的机率大的条件放在前面
	 8. 少使用递归算法，递归时会增加调用栈层次，降低了性能，可使用队列或栈来避免递归
	 9. 尽量不要使用通用类型（如FIELD-SYMBOLS、及形式参数），使用具体限定类型；比较时尽量使用同一数据类型：IF c = c.比IF i = c.快，原因是未发生类型转换
	 10. 不要使用混合类型进行计算与比较，除非有必须
	 11. 尽量使用静态语句，少用动态编程，动态编辑虽然灵活，但性能有所下降
	 12. 在对字符进行操作进，尽量使用String代替C固定长度类型，如：concatenate[kənˈkatɪneɪt]语句对固定长度的C连接时，会去扫描那些非空字符出来再进行连接，速度没有String快
	 13. READ/MODIFY TABLE时使用TRANSPORTING只读取或修改必要的字段
	 14. 尽量避免使用MOVE-CORRESPONDING和 SELECT...INTO CORRESPONDING FIELDS OF [TABLE] (SELECT时，查询几个字段就定义具有这几个字段的内表，
	 而不是直接使用基于数据库表类型创建的内表，否则如果直接使用INTO TABLE语法 检查时会警告，但结果是没有问题的)。
	 CORRESPONDING语句在系统内部存在隐式操作: 逐个字段的检查元素名称匹配; 检查元素类型匹配;元素类型转换； 
	 15. 最好不要向排序内表中插入（INSERT ... INTO TABLE ...）数据，因为在插入时会进行排序，速度会随着数据量的增加而慢下来，所以最好只向标准内表或哈希表中插入数据
	 16. 将某个内表中的全部记录或部分记录追加到另一内表时，使用INSERT/APPEND LINES OF … 代替循环逐条追加；如果是全新赋值，直接对内表使用“=”进行赋值操作即可
	 17. 调用类方法要快于Function：
	 	Calling Methods of global Classes：    
	 	call method CL_PERFORMANCE_TEST=>M1.
	 	Calling Function Modules：         
	 	call function 'FUNCTION1'.
	 18. 通过运行事务代码SLIN(或者直接通过SE38的菜单)，进行代码静态检查，根据SAP提供的反馈信息，优化代码
	 19. 通过老式方式定义内表时，使用OCCURS 0 而非OCCURS n ：[əˈkə:s]  重现
			lOCCURS n 代表初始化内表的空间大小为n（空间固定），当内表存储记录条数超出n时，系统将依靠页面文件存放超出部分的数据。 
					  当系统内存资源十分紧缺的时候，我们可以使用OCCURS n的初始化方法， 但是这样的效率稍微慢
			lOCCURS 0 代表初始化内表的空间大小为无限，当内表存储记录条数不断增加时， 内表所使用的内存空间不断扩大， 
					  直到系统无法分配为止。使用内存比使用页面交换更快一些， 但是要考虑系统的资源状态
	 20. 使用完成后及时清空释放内表所占用的空间：FREE <itab>.
	 21. 使用CASE…WHEN语句代替 IF…ELSEIF…；使用WHILE…ENDWHILE 代替 DO…ENDDO
	 22. LOOP循环内表时加上Where条件减少CPU负荷，而不是在循环里通过IF语句来过滤数据

# 十、其他内容
## 1.不同模块常用表
		MM/WM: MSEG,LTAP,EBAN,VBFA,EKKO,EKBE,EKET
		SD   : VBAK,VBAP,LIKP,LIPS,VBRK,VBRP
		PP   : AFRY,AFKO,CAUFV,AUFK,RESB,MDRS,ATP_RESB