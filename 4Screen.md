# 屏幕操作
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
