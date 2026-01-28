# 과제

### ZSTRAVELAG 으로 테이블 변경하며 기존 프로그램 수정 내용

1. include TOP
    - 업로드 데이터 조회 내용 변수 타입 선언 변경
        - 기존
            
            ```abap
            DATA: BEGIN OF GS_ZSC,
                    ZSTATUS   TYPE ICON-ID,
                    MANDT      TYPE ZSCARR-MANDT,
                    CARRID      TYPE ZSCARR-CARRID,
                    CARRNAME     TYPE ZSCARR-CARRNAME,
                    CURRCODE    TYPE ZSCARR-CURRCODE,
                    URL    TYPE ZSCARR-URL,
                    ZRESULT   TYPE CHAR200,
                  END OF GS_ZSC.
            DATA: BEGIN OF GS_EXCEL,
                    MANDT      TYPE ZSCARR-MANDT,
                    CARRID      TYPE ZSCARR-CARRID,
                    CARRNAME     TYPE ZSCARR-CARRNAME,
                    CURRCODE    TYPE ZSCARR-CURRCODE,
                    URL    TYPE ZSCARR-URL,
                  END OF GS_EXCEL.
            ```
            
        - 변경 
        TYPE 을 Table 참조 정의 참조로 변경
            
            ```abap
            DATA BEGIN OF GS_ZSC.
              DATA ZSTATUS TYPE ICON-ID.
              INCLUDE TYPE zstravelag.
              DATA ZRESULT TYPE CHAR200.
            DATA END OF GS_ZSC.
            
            ```
            
    - DB 데이터 조회 결과 담는 변수명 및 형태 변경
        - 기존
            
            ```abap
            DATA: GT_ZSCARR TYPE TABLE OF ZSCARR,
                  GS_ZSCARR TYPE          ZSCARR.
            ```
            
        - 변경
            
            ```abap
            DATA: GT_TB TYPE TABLE OF zstravelag,
                  GS_TB TYPE          zstravelag.
            ```
            
2. include - C01(클래스)
    - 그리드 더블 클릭 이벤트 제어 함수 내 명칭 표기
        - 기존
            
            ```abap
            CLASS lcl_event_receiver IMPLEMENTATION.
              METHOD handle_double_click.
                IF r1 ='X'.
                  DATA: tmp_gt_zsc LIKE LINE OF gt_zsc.
            
                  READ TABLE gt_zsc INDEX e_row-index INTO tmp_gt_zsc.
                  tmp_gt_zsc-carrname = 'ZSCARR'.
                  SET PARAMETER ID 'DTB' FIELD tmp_gt_zsc-carrname.
            ```
            
        - 변경
            
            ```abap
            CLASS lcl_event_receiver IMPLEMENTATION.
              METHOD handle_double_click.
            
                IF r1 ='X'.
                  DATA: tmp_gt_zsc LIKE LINE OF gt_zsc.
            
                  READ TABLE gt_zsc INDEX e_row-index INTO tmp_gt_zsc.
                  tmp_gt_zsc-name = 'ZSTRAVELEG'.
            ```
            
3. include - F01(함수정의)
    - FORM upload_from_excel 
    대상 테이블에 맞춰 내용 변경
        - 기존
            
            ```abap
            .
              lv_numberofcolumns = 5. 
            ...
                    CASE sy-index .
                      WHEN 1 .
                        gs_excel-mandt = <lv_field>.
                      WHEN 2 .
                        gs_excel-carrid = <lv_field>.
                      WHEN 3 .
                        gs_excel-carrname = <lv_field>.
                      WHEN 4 .
                        gs_excel-currcode = <lv_field>.
                      WHEN 5 .
                        gs_excel-url = <lv_field>.
            ...          
            ```
            
        - 변경
            
            ```abap
            ...
              lv_numberofcolumns = 13. 
            ...
                    CASE sy-index.
                      WHEN 1.
                        gs_excel-mandt = <lv_field>.
                      WHEN 2.
                        gs_excel-agencynum = <lv_field>.
                      WHEN 3.
                        gs_excel-name = <lv_field>.
                      WHEN 4.
                        gs_excel-street = <lv_field>.
                      WHEN 5.
                        gs_excel-postbox = <lv_field>.
                      WHEN 6.
                        gs_excel-postcode = <lv_field>.
                      WHEN 7.
                        gs_excel-city = <lv_field>.
                      WHEN 8.
                        gs_excel-country = <lv_field>.
                      WHEN 9.
                        gs_excel-region = <lv_field>.
                      WHEN 10.
                        gs_excel-telephone = <lv_field>.
                      WHEN 11.
                        gs_excel-url = <lv_field>.
                      WHEN 12.
                        gs_excel-langu = <lv_field>.
                      WHEN 13.
                        gs_excel-currency = <lv_field>.
            ...
            ```
            
    - FORM download_excel_smpl
    대상 테이블에 맞춰 내용 변경
        - 컬럼 표시형식 설정
            - 기존
                
                ```abap
                PERFORM column_format USING:
                                              1 '@',
                                              2 '@',
                                              3 '@',
                                              4 '@',
                                              5 '@'
                ```
                
            - 변경
                
                ```abap
                    PERFORM column_format USING:
                                              1 '@',
                                              2 '@',
                                              3 '@',
                                              4 '@',
                                              5 '@',
                                              6 '@',
                                              7 '@',
                                              8 '@',
                                              9 '@',
                                             10 '@',
                                             11 '@',
                                             12 '@',
                                             13 '@'.
                ```
                
        - 대상 테이블 조회 결과 클립보드에 등록 후 붙여넣기
            
            ```abap
            * 테이블 루프 및 조회결과 저장용
              DATA:
                tab1 TYPE TABLE OF stravelag WITH HEADER LINE,
                rnum TYPE i,
                cnum TYPE i.
            
              SELECT * FROM stravelag INTO TABLE tab1.
            
              DATA:
                lv_deli(1) TYPE c VALUE cl_bcs_convert=>gc_tab,
                lv_rc      TYPE i.
            
              TYPES:
                lv_data(9999) TYPE c,
                ty            TYPE TABLE OF lv_data.
            
              DATA: lt_clip TYPE ty WITH HEADER LINE.
            * 헤더 설정
              CONCATENATE
                'MANDT'
                'AGENCYNUM'
                'NAME'
                'STREET'
                'POSTBOX'
                'POSTCODE'
                'CITY'
                'COUNTRY'
                'REGION'
                'TELEPHONE'
                'URL'
                'LANGU'
                'CRRENCY'
              INTO lt_clip SEPARATED BY lv_deli.
            
              APPEND lt_clip.
              CLEAR lt_clip.
            * 데이터 설정
              LOOP AT tab1.
                rnum = sy-tabix + 1.
                CONCATENATE
                  tab1-mandt
                  tab1-agencynum
                  tab1-name
                  tab1-street
                  tab1-postbox
                  tab1-postcode
                  tab1-city
                  tab1-country
                  tab1-region
                  tab1-telephone
                  tab1-url
                  tab1-langu
                  tab1-currency
                INTO lt_clip SEPARATED BY lv_deli.
                APPEND lt_clip.
                CLEAR lt_clip.
              ENDLOOP.
            * 클립보드에 내용 전달
              CALL METHOD cl_gui_frontend_services=>clipboard_export
                IMPORTING
                  data                 = lt_clip[]
                CHANGING
                  rc                   = lv_rc
                EXCEPTIONS
                  cntl_error           = 1
                  error_no_gui         = 2
                  not_supported_by_gui = 3
                  OTHERS               = 4.
            * 시트 영역 및 스타일 설정용
              DATA:e_cell1   TYPE ole2_object,
                   e_cell2   TYPE ole2_object,
                   e_range   TYPE ole2_object,
                   e_borders TYPE ole2_object.
            
              CALL METHOD OF go_application 'Cells' = e_cell1
                EXPORTING
                  #1 = 1
                  #2 = 1.
            
              CALL METHOD OF go_application 'Cells' = e_cell2
                EXPORTING
                  #1 = rnum
                  #2 = 13.
            
              CALL METHOD OF go_application 'Range' = e_range
                EXPORTING
                  #1 = e_cell1
                  #2 = e_cell2.
            * 붙여넣기 
              CALL METHOD OF e_range 'Select'.
              CALL METHOD OF go_sheet 'Paste'.  
            
            ```
            
    - FORM get_needed_data
    키 데이터 중복 검증을 위한 조회 대상 테이블 변경 및 변경된 변수명으로 적용
        
        ```abap
          SELECT *
            FROM zstravelag
            INTO CORRESPONDING FIELDS OF TABLE gt_tb.
          SORT gt_tb BY agencynum.
        
          IF r2 = 'X'.
            it_cp[] = gt_tb[].
          ENDIF.
        ```
        
    - FORM get_zsc_data
    업로드 결과 내용 유효성 검증 동작 수정
        
        ```abap
        ...
        IF gs_zsc-agencynum IS INITIAL.
              gs_zsc-zstatus = icon_led_red.
              gs_zsc-zresult = gs_zsc-zresult && '[키값이 없습니다.]'.
            ELSE.
              SORT gt_tb BY agencynum.
              READ TABLE gt_tb INTO gs_tb
               WITH KEY agencynum = gs_zsc-agencynum
               BINARY SEARCH.
              IF sy-subrc = 0.
                gs_zsc-zstatus = icon_led_red.
                gs_zsc-zresult = gs_zsc-zresult && '[키값이 이미 들어있습니다.]'.
              ENDIF.
            ENDIF.
        ...
        ```
        
    - FORM set_fieldcat
    변경된 테이블 구조로 필드카탈로그 구성 및 키 컬럼에 앞자리 0 채우도록 속성 설정
        
        ```abap
        ...
          IF r1 = 'X'.
            _fcat: 'ZSTATUS' '상태' 'X' '' '3' '',
                   'MANDT' '클라이언트' 'X' '' '3' '',
                   'AGENCYNUM' '업체코드' 'X' '' '8' '',
                   'NAME' '업체명' '' 'X' '25' 'X',
                   'STREET' '도로명' '' 'X' '30' 'X',
                   'POSTBOX' '지번' '' 'X' '10' '',
                   'POSTCODE' '우편번호' 'X' '10' '' '',
                   'CITY' '도시' '' 'X' '25' 'X',
                   'COUNTRY' '국가' '' 'X' '3' '',
                   'REGION' '지역' '' 'X' '3' '',
                   'TELEPHONE' '연락처' '' 'X' '30' '',
                   'URL' 'URL' '' 'X' '255' 'X',
                   'LANGU' '언어' '' 'X' '1' '',
                   'CURRENCY' '통화' '' 'X' '5' '',
                   'ZRESULT' '비고' '' '' '50' ''.
          ELSEIF r2 = 'X'.
            _fcat: 'MANDT' '클라이언트' 'X' '' '3' '',
                   'AGENCYNUM' '업체코드' 'X' '' '8' '',
                   'NAME' '업체명' '' 'X' '25' 'X',
                   'STREET' '도로명' '' 'X' '30' 'X',
                   'POSTBOX' '지번' '' 'X' '10' '',
                   'POSTCODE' '우편번호' 'X' '10' '' '',
                   'CITY' '도시' '' 'X' '25' 'X',
                   'COUNTRY' '국가' '' 'X' '3' '',
                   'REGION' '지역' '' 'X' '3' '',
                   'TELEPHONE' '연락처' '' 'X' '30' '',
                   'URL' 'URL' '' 'X' '255' 'X',
                   'LANGU' '언어' '' 'X' '1' '',
                   'CURRENCY' '통화' '' 'X' '5' ''.
          ENDIF.
        
          LOOP AT gt_fcat INTO gs_fcat.
            CASE gs_fcat-fieldname.
              WHEN 'AGENCYNUM'.
                gs_fcat-lzero = 'X'.
                MODIFY gt_fcat FROM gs_fcat.
            ENDCASE.
          ENDLOOP.
        ```
        
    - FORM save_zsc_final
    변경된 변수명 적용
        
        ```abap
          IF gs_zsc-zstatus = icon_led_yellow.
            MOVE-CORRESPONDING gs_zsc TO gs_tb.
            gs_tb-mandt = sy-mandt.
            INSERT INTO zstravelag VALUES gs_tb.
        ```
        
    - FORM del_data
    기존 데이터 삭제 동작 대상 테이블 변경 및 테이블 변경 시 수정필요없어도록 메시지 수정
        
        ```abap
        * 입력 데이터 점검을 위해 사용할 DB 데이터
          SELECT *
            FROM zstravelag
            INTO CORRESPONDING FIELDS OF TABLE gt_tb.
        
          IF gt_tb IS NOT INITIAL.
            DELETE FROM zstravelag.
            IF sy-subrc = 0.
              MESSAGE '정상적으로 테이블 데이터가 전체 삭제되었습니다.' TYPE 'S'.
            ELSE.
              MESSAGE '데이터 삭제 중에 문제가 생겼습니다.' TYPE 'E'.
            ENDIF.
        ...
        ```
        
    - FORM save_modify
    변경 변수명 적용
        
        ```abap
        ...
        IF it_cp[] NE gt_
        tb[].
        ...
        ```
        
    - FORM f_save_data
    변경 테이블 및 변수명 적용과 업로드 데이터 검증 제거
        - 기존은 carrname, currcode, url 빈값 판단과 currcode가 존재하지 않는 값인지 판단하는 동작 존재
        
        ```abap
        DATA: wa_cp     TYPE zstravelag,
                wa_cp_tmp TYPE zstravelag.
        
          REFRESH it_changes[].
        
          LOOP AT gt_tb INTO gs_tb.
            READ TABLE it_cp INTO wa_cp INDEX sy-tabix.
            IF wa_cp NE gs_tb.
        
              MOVE-CORRESPONDING gs_tb TO wa_cp_tmp.
        
              MODIFY zstravelag FROM wa_cp_tmp.
              IF sy-subrc = 0.
                APPEND gs_tb TO it_changes.
              ELSE.
                MESSAGE '테이블 저장 중 에러가 발생했습니다.' TYPE 'I'.
                LEAVE SCREEN.
              ENDIF.
            ENDIF.
        
            CLEAR wa_cp.
          ENDLOOP.
        ```
        

### splitter container 구성

![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw20_homework/image.png?raw=true)

1. ALV GRID를 담을 컨테이너와 그 컨테이너를 담을 스플릿 컨테이너와 도킹 컨테이너 변수 선언
    
    ```abap
    DATA:
          GO_DOCKING TYPE REF TO CL_GUI_DOCKING_CONTAINER,
          GO_SPLIT TYPE REF TO CL_GUI_SPLITTER_CONTAINER,
    
          GO_CONT01 TYPE REF TO CL_GUI_CONTAINER,
          GO_CONT02 TYPE REF TO CL_GUI_CONTAINER,
    
          GO_GRID01    TYPE REF TO CL_GUI_ALV_GRID,
          GO_GRID02    TYPE REF TO CL_GUI_ALV_GRID.
    ```
    
2. 컨테이너 및 그리드 클래스 인스턴스 생성 및 parent 설정
    
    ```abap
    FORM create_object_instance .
      CREATE OBJECT go_docking
        EXPORTING
          repid     = sy-repid
          dynnr     = sy-dynnr
          side      = cl_gui_docking_container=>dock_at_left
          extension = 3000.
    
      CREATE OBJECT go_split
        EXPORTING
          parent  = go_docking
          rows    = 1
          columns = 2.
    
    * cl_gui_container는 cl_gui_split_container에서 생성
      CALL METHOD go_split->get_container
        EXPORTING
          row        = 1
          column     = 1
        RECEIVING
          container = go_cont01.
    
      CALL METHOD go_split->get_container
        EXPORTING
          row        = 1
          column     = 2
        RECEIVING
          container = go_cont02.
    
      CREATE OBJECT go_grid01
        EXPORTING
          i_parent = go_cont01.
    
      CREATE OBJECT go_grid02
        EXPORTING
          i_parent = go_cont02.
    ```
    
3. 기존 하나만 출력하던 결과를 그리드 두 개에 전달하도록 수정
- 동일 내용이나 테스트용으로 설정
    
    ```abap
    FORM display_alv_0100 .
    
      IF r1 ='X'.
        CALL METHOD go_grid01->set_table_for_first_display
          EXPORTING
            is_layout       = gs_layout
          CHANGING
            it_outtab       = gt_zsc
            it_fieldcatalog = gt_fcat.
        CALL METHOD go_grid02->set_table_for_first_display
          EXPORTING
            is_layout       = gs_layout
          CHANGING
            it_outtab       = gt_zsc
            it_fieldcatalog = gt_fcat.
    ```
	