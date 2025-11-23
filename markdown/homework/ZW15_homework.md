
1. 버튼 추가
   
    ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw15_homework/image001.png?raw=true)
```ABAP
* 수정위치1 - *_F01 #SET_FUNCTION_KEY
...
  G_FUNCTION_KEY-ICON_ID   = ICON_EXTRA.
  G_FUNCTION_KEY-ICON_TEXT = 'SPFLI SAMPLE다운'.
  G_FUNCTION_KEY-TEXT      = 'SPFLI SAMPLE다운'.
  SSCRFIELDS-FUNCTXT_03    = G_FUNCTION_KEY.

  CLEAR G_FUNCTION_KEY.
...

* 수정위치2 - *_SEL FUNCTION KEY
...
SELECTION-SCREEN:
  FUNCTION KEY 1,
  FUNCTION KEY 3.
...

```


2. 버튼 이벤트 동작 정의 및 함수 수정
```ABAP
* 수정위치1 - *_F01 #ACT_FUNCTION_KEY
...
    CASE SSCRFIELDS-UCOMM.
        WHEN 'FC01'.
            PERFORM EXCEL_DOWN_SMPL USING ''.
        WHEN 'FC03'.
            PERFORM EXCEL_DOWN_SMPL USING 'SPFLI'.
    ENDCASE. 
...
	
* 수정위치2 - *_F01 #EXCEL_DOWN_SMPL
...
    FORM excel_down_smpl USING p_tb_name.
* 다운로드 양식 선택
    IF p_tb_name = 'SPFLI'.
	    LS_KEY-OBJID = 'ZTEST15_SPFLI_EXCEL01'.
	    LS_KEY-RELID = 'MI'.
    ELSE.
	    LS_KEY-OBJID = 'ZTEST14_EXCEL01'.
	    LS_KEY-RELID = 'MI'.
    ENDIF.
...	
* 엑셀 다운
	  PERFORM DOWNLOAD_EXCEL_SMPL USING p_tb_name LS_KEY-OBJID.
...
	
* 수정위치3 - *_F01 #DOWNLOAD_EXCEL_SMPL
...
    FORM download_excel_smpl  USING    
	                            p_tb_name
	                            p_ls_key_objid . 
...
* 데이터 입력
	IF p_tb_name = 'SPFLI'.
    SET PROPERTY OF go_sheet 'Name' = 'SPFLI'.

    PERFORM fill_cell USING go_application 01: 01 'MANDT',
                                                 02 'CARRID',
                                                 03 'CONNID',
                                                 04 'COUNTRYFR',
                                                 05 'CITYFROM',
                                                 06 'AIRPFROM',
                                                 07 'COUNTRYTO',
                                                 08 'CITYTO',
                                                 09 'AIRPTO',
                                                 10 'FLTIME',
                                                 11 'DEPTIME',
                                                 12 'ARRTIME',
                                                 13 'DISTANCE',
                                                 14 'DISTID',
                                                 15 'FLTYPE',
                                                 16 'PERIOD'.
* COLUMN WIDTH
    PERFORM column_width USING 01 10.
    PERFORM column_width USING 02 10.
    PERFORM column_width USING 03 10.
    PERFORM column_width USING 04 20.
    PERFORM column_width USING 05 10.
    PERFORM column_width USING 06 10.
    PERFORM column_width USING 07 20.
    PERFORM column_width USING 08 10.
    PERFORM column_width USING 09 10.
    PERFORM column_width USING 10 20.
    PERFORM column_width USING 11 20.
    PERFORM column_width USING 12 20.
    PERFORM column_width USING 13 10.
    PERFORM column_width USING 14 10.
    PERFORM column_width USING 15 10.
    PERFORM column_width USING 16 10.

* 기존 테이블 조회 엑셀 입력
    DATA:
      rnum TYPE i,
      cnum TYPE i.

    SELECT * FROM spfli
      INTO TABLE @DATA(lt_spfli).

    LOOP AT lt_spfli INTO DATA(ls_spfli).
      rnum = sy-tabix + 1.
      PERFORM fill_cell USING go_application rnum:
                                             01 ls_spfli-mandt,
                                             02 ls_spfli-carrid,
                                             03 ls_spfli-countryfr,
                                             04 ls_spfli-cityfrom,
                                             05 ls_spfli-airpfrom,
                                             06 ls_spfli-countryto,
                                             07 ls_spfli-cityto,
                                             08 ls_spfli-airpto,
                                             09 ls_spfli-fltime,
                                             10 ls_spfli-deptime,
                                             11 ls_spfli-arrtime,
                                             12 ls_spfli-distance,
                                             13 ls_spfli-distid,
                                             14 ls_spfli-fltype,
                                             15 ls_spfli-period,
                                             16 ls_spfli-mandt.
    ENDLOOP.

* styling
    DATA : borders TYPE ole2_object.
    DATA : range TYPE ole2_object.
    DATA : cell1 TYPE ole2_object.
    DATA : cell2 TYPE ole2_object.

    CALL METHOD OF go_application 'CELLS' = cell1  " gs_cell2 에 2열 5행을 대입하고
      EXPORTING
      #1 = 1
      #2 = 1.

    CALL METHOD OF go_application 'Cells' = cell2  " gs_cell2 에 3열 15행을 대입하고
      EXPORTING
      #1 = 1
      #2 = 16.

    CALL METHOD OF go_application 'Range' = range
      EXPORTING
      #1 = cell1
      #2 = cell2.

    DATA: lo_interior TYPE ole2_object.

    CALL METHOD OF range 'Interior' = lo_interior.
    SET PROPERTY OF lo_interior 'Color' = 65535.

    CALL METHOD OF go_application 'Cells' = cell2  " gs_cell2 에 3열 15행을 대입하고
    EXPORTING
    #1 = rnum
    #2 = 16.
    CALL METHOD OF go_application 'Range' = range
    EXPORTING
    #1 = cell1
    #2 = cell2.

    CALL METHOD OF range 'borders' = borders
      EXPORTING #1 = 7.
    SET PROPERTY OF borders 'Linestyle' = 1.

    CALL METHOD OF range 'borders' = borders
      EXPORTING #1 = 8.
    SET PROPERTY OF borders 'Linestyle' = 1.

    CALL METHOD OF range 'borders' = borders
      EXPORTING #1 = 9.
    SET PROPERTY OF borders 'Linestyle' = 1.

    CALL METHOD OF range 'borders' = borders
      EXPORTING #1 = 10.
    SET PROPERTY OF borders 'Linestyle' = 1.

    CALL METHOD OF range 'borders' = borders
      EXPORTING #1 = 11.
    SET PROPERTY OF borders 'Linestyle' = 1.

    CALL METHOD OF range 'borders' = borders
      EXPORTING #1 = 12.
    SET PROPERTY OF borders 'Linestyle' = 1.

    CONSTANTS:
      c_center TYPE i VALUE -4108,
      c_left   TYPE i VALUE -4131,
      c_right  TYPE i VALUE -4152.

    CALL METHOD OF range 'select'.
    SET PROPERTY OF range 'HorizontalAlignment' = c_center.

  ELSE.
    SET PROPERTY OF go_sheet 'Name' = 'ZSCARR'.
    PERFORM fill_cell USING go_application 01: 01 'MANDT',
                                               02 'CARRID',
                                               03 'CARRNAME',
                                               04 'CURRCODE',
                                               05 'URL'.
	ENDIF. 
	
...
```

3. Debug  - FILL_CELL 호출

    ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw15_homework/image003.png?raw=true)

4. 결과물

   ![](https://github.com/user-attachments/assets/99050b5b-1d97-42ed-bd45-fcee0ead4b7c)

