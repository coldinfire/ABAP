 ## 1.修改表维护
----------
	添加修改操作人字段
	1. SE11, go to Utilities - Table Maintenance Generator or SE55
	2. Go to Environments – Modifications – Events
	3. Create your Form routines to be called from view maintenance 
	   Event could be “Before saving the data in the database”.
	   The FORM routine must belong to the function group, 
		to which the maintenance modules for the view are assigned. 
	ZPP_MOLDTSTA：可参考
## 2.添加修改人信息
----------
	可以根据TCode进行相应的判断：
	创建人信息:
 	 l_wa_head-ernam = sy-uname.
      l_wa_head-erdat = sy-datum.
      l_wa_head-erzet = sy-uzeit.
      call function 'TERMINAL_ID_GET'
        exporting
          username             = sy-uname
        importing
          terminal             = l_wa_head-eterminal
        exceptions
          multiple_terminal_id = 1
          no_terminal_found    = 2
          others               = 3.	
	修改人信息：
	  l_wa_head-urnam = sy-uname.
      l_wa_head-urdat = sy-datum.
      l_wa_head-urzet = sy-uzeit.
      call function 'TERMINAL_ID_GET'
        exporting
          username             = sy-uname
        importing
          terminal             = l_wa_head-uterminal
        exceptions
          multiple_terminal_id = 1
          no_terminal_found    = 2
          others               = 3.

## 3.小数后面的0去除问题
----------
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
## 4.Tcode

	
	