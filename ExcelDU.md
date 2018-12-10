1.上传文档
  SMW0:Binary data => Package => create object name and description
  
SELECTION-SCREEN:
  PUSHBUTTON /2(30) button1 USER-COMMAND but1."30是按钮长度
  
SELECTION-SCREEN BEGIN OF BLOCK blk1 WITH FRAME TITLE text-001.
SELECTION-SCREEN BEGIN OF LINE.
PARAMETERS p_file TYPE rlgrap-filename.
SELECTION-SCREEN POSITION 50.
SELECTION-SCREEN COMMENT (20) p_comm FOR FIELD p_file.
SELECTION-SCREEN END OF LINE.
SELECTION-SCREEN END OF BLOCK blk1.

AT SELECTION-SCREEN ON VALUE-REQUEST FOR p_file."发出上传文件时，弹出选择文件框，选择对应的文件
  PERFORM frm_get_filepath.

AT SELECTION-SCREEN. "检查界面操作并响应
  IF sscrfields EQ 'BUT1'."下载按钮时，下载文档模板
    PERFORM download_file.
  ENDIF.
  IF p_file IS NOT INITIAL.
    PERFORM frm_check_file."当文档上传框有请求时，检验路径是否存在
  ENDIF.

INITIALIZATION.
  p_comm = ''.
  button1 = 'Download template file'.

START-OF-SELECTION.
  PERFORM frm_inter_table.
*  PERFORM frm_check_xzael.
  
FORM download_file .
  DATA: lwa_wwwdata_tab LIKE wwwdatatab,
        l_filename      TYPE rlgrap-filename,
        mes             TYPE string.

  l_filename = 'C:/temp/BatchInput.xlsx'.
  SELECT SINGLE *
    FROM wwwdata
   INNER JOIN tadir
      ON wwwdata~objid = tadir~obj_name
    INTO CORRESPONDING FIELDS OF lwa_wwwdata_tab
   WHERE wwwdata~srtf2  = 0
     AND wwwdata~relid  = 'MI'             "标识二进制的对象
     AND tadir~pgmid    = 'R3TR'
     AND tadir~object   = 'W3MI'
     AND tadir~obj_name = 'ZMI05'.         "模板名字

  IF sy-subrc = 0.
    CALL FUNCTION 'DOWNLOAD_WEB_OBJECT'
      EXPORTING
        key         = lwa_wwwdata_tab
        destination = l_filename.
    IF sy-subrc = 0.
      CONCATENATE 'The File Download Directory:' l_filename INTO mes.
      MESSAGE mes TYPE 'S'.
    ENDIF.
  ENDIF.
ENDFORM.                    " DOWNLOAD_FILE

FORM frm_get_filepath .
  DATA lv_filepath TYPE ibipparms-path.

  CALL FUNCTION 'WS_FILENAME_GET'
    EXPORTING
      mask             = ',EXCEL FILE,*.XLS;*.XLSX;'
      mode             = 'O' "S为保存，O为打开
    IMPORTING
      filename         = p_file
    EXCEPTIONS
      inv_winsys       = 1
      no_batch         = 2
      selection_cancel = 3
      selection_error  = 4
      OTHERS           = 5.
ENDFORM.                    " FRM_GET_FILEPATH

FORM frm_check_file .
  DATA:lv_filename TYPE string,
       lv_result TYPE c.
  lv_filename = p_file.
  CALL METHOD cl_gui_frontend_services=>file_exist
    EXPORTING
      file                 = lv_filename
    RECEIVING
      result               = lv_result
    EXCEPTIONS
      cntl_error           = 1
      error_no_gui         = 2
      wrong_parameter      = 3
      not_supported_by_gui = 4
      OTHERS               = 5.
  IF sy-subrc NE 0.
  ENDIF.
  IF lv_result = ''.
    MESSAGE 'The File Not Found In The Direct!' TYPE 'E'.
  ENDIF.
ENDFORM.                    " FRM_CHECK_FILE

FORM frm_inter_table.
    CALL FUNCTION 'ALSM_EXCEL_TO_INTERNAL_TABLE'
    EXPORTING
      filename                = p_file   "本地文件全路径名
      i_begin_col             = '1'      "开始列
      i_begin_row             = '2'      "开始行
      i_end_col               = '7'      "结束列
      i_end_row               = '6666'   "结束行
    TABLES
      intern                  = it_file   "输出文件内容到it_file
    EXCEPTIONS
      inconsistent_parameters = 1
      upload_ole              = 2
      OTHERS                  = 3.

  IF it_file[] IS INITIAL.
    MESSAGE 'There is no data in the excel!' TYPE 'E'.
  ENDIF.
  SORT it_file BY row.
  LOOP AT it_file ASSIGNING <wa_file>.     "将上传文件的内容写到内表中
    CASE <wa_file>-col.
      WHEN '0001'.
        wa_iseg-gjahr  =  <wa_file>-value.
      WHEN '0002'.
        wa_iseg-iblnr  =  <wa_file>-value.
      WHEN '0003'.
        wa_iseg-zeili  =  <wa_file>-value.
      WHEN '0004'.
        wa_iseg-buchm  =  <wa_file>-value.
      WHEN '0005'.
        wa_iseg-meins  =  <wa_file>-value.
      WHEN '0006'.
        wa_iseg-lgort  =  <wa_file>-value.
      WHEN '0007'.
        wa_iseg-usnam  =  <wa_file>-value.
    ENDCASE.
    AT END OF row.
      APPEND wa_iseg TO it_iseg.
      CLEAR wa_iseg.
    ENDAT.
  ENDLOOP.
ENDFORM.                    " FRM_UPLOAD_FILE

