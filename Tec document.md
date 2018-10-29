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
