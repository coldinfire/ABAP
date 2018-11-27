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
	同时我们还必须掌握从业务流程、业务操作到程序逻辑之间的转化。
	11. 程序效能全面总结
	很多初级开发人员的程序可维护性非常差，后期维护成本很高，给咨询公司和客户带来很多麻烦，怎样让开发的程序具有高可维护性，
	让我们的程序可读性强，结构清晰，可配置性好，可移植性方便。
	12. WebDynpro开发技术全面讲解，演练
	Web化程序代表SAP今后的开发方向，本部分课程将系统讲解WEBDYNPRO开发的具体实现。
# 内表&报表
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
	4.6 动态引用，通过循环赋值给定义的字段符号，对其进行修改，等于直接修改原内表。
		field-symbols:<l_shortageqty> type mng01.
		loop at <dyn_table> assigning <dyn_wa>.
			assign component 'SHORTAGEQTY' of structure <dyn_wa> to <l_shortageqty>.
			 <l_shortageqty> = <l_shortageqty> - <l_fvalue>.
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
