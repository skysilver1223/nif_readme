
# NIF 시험 시나리오 절차 매뉴얼 

## 목적(용도)
본 문서는 검증용 트래픽 생성 - 검증용 Avro 생성 - 원시 데이터 검증까지의 시나리오 절차를 정리한 문서이다.

## 시나리오 플로우
데이터 검증은 아래와 같은 플로우로 절차를 진행한다.

<img src="https://github.com/skysilver1223/nif_readme/assets/109940215/ae6eba67-78a0-4042-8d66-c26b8c0cc5f9" width="1200" height="500"/>

  * 플로우 별 절차 요약설명
    
    * 1) 트래픽 생성 및 Dump 절차요약
      
      * 검증용 트래픽 생성 프로그램의 시나리오를 구동을 통해 트래픽을 생성
      * 생성한 트래픽을 Dump하여 Pcap 파일을 생성
     
    * 2) Pcap 분석 기반 Avro(Kbell) 생성 절차요약
      
      * 저장한 Pcap 파일을 검증용 Avro 파일 생성 프로그램의 분석 디렉토리로 이동
      * 트래픽이 발생한 시간을 참고하여 검증용 Avro 파일 생성 프로그램의 Config을 수정
      * 검증용 Avro 파일 생성 프로그램을 구동하여 검증용 Avro 생성
      * 검증용 Avro 파일 생성 프로그램의 결과 디렉토리로 이동하여 생성된 검증용 Avro 파일들을 확인
     
    * 3) Avro(Kbell-ETRI) 비교분석 절차요약
         
      * 검증용 Avro 파일들(Kbell 규격)과 원시 데이터 Avro 파일들(ETRI)을 원시 데이터 정확성 검증 프로그램의 각 디렉토리로 이동
      * 원시 데이터 정확성 검증 프로그램의 Config를 검증할 데이터의 타입(플로우 원시 , 플로우 단위시간 , 플로우 패킷 원시)에 맞게 설정
      * 원시 데이터 정확성 검증 프로그램 구동하여 비교 결과 생성

## 시나리오 절차
 
  ### 1) 트래픽 생성 및 Dump 절차

   해당 절차는 플로우 구성 상 아래의 파트 부분을 수행하며, 아래의 절차 대로 수행한다.
   
   <img src="https://github.com/skysilver1223/nif_readme/assets/109940215/119364a2-fb1b-450a-a420-4ec3d4317ef3" width="600" height="300"/>

   * 1-1) nif Router VM에 접속 후 트래픽 덤프 수행
       ```
       # sudo tcpdump -i $target_interface $protocol -w $res_pcap_file_name
       ```

   * 1-2) 검증용 트래픽 생성 프로그램이 위치한 시스템에 접속(패스워드 생략)
       ```
       #  ssh -p $port $user@$ip
       ```
       
   * 1-3) 구동할 시나리오 디렉토리로 이동
       ```
       #cd nif_trex_traffic_gen/scen_$num/
       ```
       
   * 1-4) 시나리오를 구동하여 트래픽 생성
       ```
       # ./_run_scen.sh
       ```
       
   * 1-5) 시나리오 구동 확인
      아래와 같은 결과 화면을 확인 할 수 있음
      ```
      파라미터 설명
      - $start_time : 시나리오 스크립트 구동시작 시간
      - $end_time :  시나리오 스크립트 구동종료 시간
      - $s_id : 시나리오 profile id (다중으로 트래픽을 생성하는 경우가 존제할 경우 마지막 profile id)
      - $p_id : 프로파일 id (다중으로 트래픽을 생성하는 경우가 존제할 경우 마지막 profile id)
      - $batch_file_path : 시나리오 구동 시 읽어오는 배치 파일 명(배치파일의 내용의 순서대로 시나리오 트래픽을 생성)
      
      ######################################################################################################
      ==================================================================
      START( $start_time ) >>>
       Scenario $s_id ( = $batch_file_path)
       Profile ( = $p_id)
      ==================================================================
      
      Executing line 1 : $cmd
       ...
      Batch Running... |
      
      ==================================================================
      END( $end_time ) >>>
       Scenario $s_id ( = $batch_file_path) scenario End
       Profile ( = $p_id) End
      ==================================================================      
      ```
      
   * 1-6) 1-1 절차에서 수행한 트래픽 덤프 종료하여 pcap 파일 결과 저장

  ### 2) Pcap 분석 기반 검증용 Avro(Kbell 규격) 생성 절차

   해당 절차는 플로우 구성 상 아래의 파트 부분을 수행하며, 아래의 절차 대로 수행한다.
   
   <img src="https://github.com/skysilver1223/nif_readme/assets/109940215/37d8a8d8-15a4-4c11-bf65-36dfa606307b" width="600" height="300"/>

   * 2-1) 트래픽 덤프 결과를 검증용 Avro파일 생성 프로그램의 분석용 디렉토리로 이동
       ```
       검증용 Avro파일 생성 프로그램의 분석용 디렉토리 확인방법
       - 검증용 Avro파일 생성 프로그램의 Config 파일의 project_info.pcap_file_dir_path 경로 확인
       
       # vi $current_nif_project/nif_pcap_verifier/go_source/config/config.yaml
       -------------------------------------------------------------------------------------------------------  
       project_info :
       ...     
         pcap_file_dir_path: "$current_nif_project/nif_pcap_verifier/pcap_samples"
         # 분석대상 pcap 디렉토리
       ...
       -------------------------------------------------------------------------------------------------------

       # mv $res_pcap_file_name $current_nif_project/nif_pcap_verifier/pcap_samples
       ```
       
   * 2-2) 트래픽 파일(Pcap)에 대한 분석을 위한 Config를 설정
     ```
     # vi $current_nif_project/nif_pcap_verifier/go_source/config/config.yaml
     -------------------------------------------------------------------------------------------------------
     project_info :
       res_file_dir_path: "$current_nif_project/nif_pcap_verifier/pcap_results"
       # pcap 분석 결과 생성 디렉토리
     
       pcap_file_dir_path: "$current_nif_project/nif_pcap_verifier/pcap_samples"
       # 분석대상 pcap 디렉토리
     
       enable_snappy : false
       # kbell Avro 연동 규격 압축 사용 여부 옵션
       # 종류 : true , false
       # * true : snappy로 압축된 kbell Avro 파일을 사용하는 경우
       # * false : snappy로 압축된 kbell Avro 파일을 사용하지 않는 경우
     
     flow_info :
         last_packet_time_out_sec : 10
         # 마지막 패킷 수신 수 타임아웃 시간
     
         last_flag_time_out_sec : 15
         # 마지막 플래그( fin , rst) 수신 후 타임아웃 시간
     
     
     nif_info :
         min_pkt_arrival_time : "$start_time"  <<<<<<<< 1-5) 절차의 트래픽 생성 시나리오의 $start_time으로 설정
         #flow 최소 시작 시간
         # ex) 2020-01-24 09:43:42 , 2020-01-24 09:43:42.000000000
     
         delta_start_time : "$start_time"      <<<<<<<< 1-5) 절차의 트래픽 생성 시나리오의 $start_time으로 설정
         #delta 시작 시간
         # ex) 2020-01-24 09:43:42 , 2020-01-24 09:43:42.000000000
     
         delta_sec : 10
         #delta 조건(s)
     
         enable_multi_pcap_flow : true
         #다중 pcap 사용관리 옵션
         # * true  : pcap_list_info의 pcap을 여러개의 별도 pcap으로 인식
         # * false : pcap_list_info의 pcap을 단일 pcap으로 인식
     
     pcap_list_info :
       # 분석 대상 pcap 리스트
       # * pcap_fname : pcap_file_dir_path 디렉토리 내 분석 대상 pcap 파일
       # * use_flag : 분석 여부 ( true : 분석 , false : 미분석 )
       - pcap_fname: "$pcap_file_name"         <<<<<<<< 1-1) 절차의 $res_pcap_file_name 으로 설정
         use_flag: true     
     -------------------------------------------------------------------------------------------------------
     ```
     
   * 2-3) 검증용 Avro파일 생성 프로그램을 구동하여 검증용 결과물 생성
     ```
     # cd $current_nif_project/nif_pcap_verifier/go_source/bin
     # ./1_src_run.sh
     ```


   * 2-4) 검증용 결과물 확인
     ```
     검증용 Avro파일 생성 프로그램의 결과용 디렉토리 확인방법
     - 검증용 Avro파일 생성 프로그램의 Config 파일의 project_info.res_file_dir_path 경로 확인
       
     # vi $current_nif_project/nif_pcap_verifier/go_source/config/config.yaml
     -------------------------------------------------------------------------------------------------------  
     project_info :
     ...     
       res_file_dir_path: "$current_nif_project/nif_pcap_verifier/pcap_results"
       # pcap 분석 결과 생성 디렉토리
     ...
     -------------------------------------------------------------------------------------------------------
     
     # cd $current_nif_project/nif_pcap_verifier/pcap_results
     # ls -al
     -------------------------------------------------------------------------------------------------------
     kb-d--$res_pcap_file_name--$delta_index.avro
     ...
     kb-p--$res_pcap_file_name--$delta_index.avro
     ...
     kb-r--$res_pcap_file_name--$delta_index.avro
     ...
     -------------------------------------------------------------------------------------------------------

     파일명 설명 : $데이터 타입_$분석 pcap 명-$단위시간 원시데이터 생성 조건횟수.avro
     ```

  ### 3) Avro(Kbell-ETRI) 비교분석 절차
  
   해당 절차는 플로우 구성 상 아래의 파트 부분을 수행하며, 아래의 절차 대로 수행한다.  
   
   <img src="https://github.com/skysilver1223/nif_readme/assets/109940215/862dff40-996f-4187-8509-78829df0f8fc" width="600" height="300"/>

   데이터 타입별(플로우 원시 데이터, 플로우 단위시간 원시 데이터, 플로우 패킷 원시 데이터) 절차를 구별하여 진행
   
   * 3-1)  검증용 Avro(Kbell 규격)을 데이터 정확성 검증 프로그램으로 이동
     ```
     # cd $current_nif_project/nif_pcap_verifier/pcap_results
     # mv kb_r-* $current_nif_project/nif_avro_verifier/avro_data/flow_raw/k_data/
     # mv kb_d-* $current_nif_project/nif_avro_verifier/avro_data/flow_delta/k_data/
     # mv kb_p-* $current_nif_project/nif_avro_verifier/avro_data/flow_packet/k_data/
     ```
       
   * 3-2) 분석 Avro(ETRI 규격)을 데이터 정확성 검증 프로그램으로 이동
     ```
     # mv (avro_file or avro_dir) $current_nif_project/nif_avro_verifier/avro_data/flow_raw/e_data/
     # mv (avro_file or avro_dir) $current_nif_project/nif_avro_verifier/avro_data/flow_delta/e_data/
     # mv (avro_file or avro_dir) $current_nif_project/nif_avro_verifier/avro_data/flow_packet/e_data/
     ```
     
   * 3-3) 데이터 정확성 검증 프로그램의 Config를 분석 데이터 타입에 맞게 설정
     ```
     # vi $current_nif_project/nif_avro_verifier/go_source/config/config.yaml
     ```
     * 3-3-1) 플로우 원시 데이터 설정 예시
       ```
       project_info :
         mode : "flow"
         # 분석 데이터 타입
         # 종류 : flow , flow_delta , flow_packet , test
         # * flow : 플로우 원시 데이터
         # * flow_delta : 플로우 단위시간 원시 데이터
         # * flow_packet : 플로우 패킷 원시 데이터
       
         record_view : "tolerance"
         # 분석 결과 확인 옵션
         # 종류 : all , tolerance ,err
         # * all : 모든 데이터 비교 결과확인
         # * tolerance : 비일치 , 공차허용범위 데이터 비교 결과확인
         # * err : 비일치 데이터 비교 결과확인
       
         enable_snappy : false
         # kbell Avro 연동 규격 압축 사용 여부 옵션
         # 종류 : true , false
         # * true : snappy로 압축된 kbell Avro 파일을 사용하는 경우
         # * false : snappy로 압축된 kbell Avro 파일을 사용하지 않는 경우
       
       flow_raw_info :
         # 데이터 분석 설정
         # 종류 : flow_raw_info , flow_delta_info , flow_packet_info
         # * flow_raw_info : 플로우 원시 데이터 분석 설정 옵션
         # * flow_delta_info : 플로우 단위시간 원시 데이터 분석 설정 옵션
         # * flow_packet_info : 플로우 패킷 원시 데이터 분석 설정 옵션
       
         etri_avro_dir_path: "$current_nif_project/nif_avro_verifier/avro_data/flow_raw/e_data"
         # ETRI Avro 파일 경로 (절대경로 , 서브디렉토리 기능 지원)
         
         kbell_avro_dir_path: "$current_nif_project/nif_avro_verifier/avro_data/flow_raw/k_data"
         # Kbell Avro 파일 경로 (절대경로 , 서브디렉토리 기능 지원)
       
         flow_cond_option :
             # Flow 공차 허용 범위 조건
             # ex) time_err_tolerance_usec : 1
             #     record_create_time = 2023-02-15 02:23:54.777228 +0000 UTC (1676427834777228)
             #     tolerance range = 2023-02-15 02:23:54.777227 +0000 UTC(1676427834777227) ~ 2023-02-15 02:23:54.777229 +0000 UTC (1676427834777229)
       
             time_sort_field_name: "record_create_time"
             # Flow 공차 허용 범위 조건 필드명
       
             time_err_tolerance_usec: 10
             # Flow 공차 허용 범위 값
       
         filter_option :
           # 비교 결과 중 특정 데이터 필터링 옵션
       
           match_time_range_filter:
               # 특정 시간 범위 필터링
               # 포맷 : 2020-01-24 09:43:42 , 2020-01-24 09:43:42.000000000
               # * column_name : 필터링 필드명
               # * start_time  : 시작시간
               # * end_time    : 종료시간        
               - column_name: "record_create_time"
                 start_time : "2023-02-15 11:23:08"
                 end_time : "2023-02-15 11:23:08.773"
       
           match_ipv4_range_filter:
               # IPv4 필드의 ip 범위를 필터링
               # * column_name : 필터링 필드명
               # * start_ip    : 시작 ipv4
               # * end_ip      : 종료 ipv4       
               - column_name: "src_ip_v4"
                 start_ip : "48.0.2.71"
                 end_ip : "48.0.2.72"
       
           match_data_filter:
               # 일치 필드 필터링
               # column_name : 필터링 필드
               # match_value : 일치값        
               - column_name : "protocol"
                 match_value : 6
               - column_name : "ip_version"
                 match_value : "IP_V4"
       
           match_ipv6_range_filter: 
               # IPv6 필드의 ip 범위를 필터링 , 시험용 옵션
               # * column_name : 필터링 필드명
               # * start_ip    : 시작 ipv6
               # * end_ip      : 종료 ipv6   
       ```
       
     * 3-3-2) 플로우 단위시간 원시 데이터 설정 예시
       ```
       project_info :
         mode : "flow_delta"
         # 분석 데이터 타입
         # 종류 : flow , flow_delta , flow_packet , test
         # * flow : 플로우 원시 데이터
         # * flow_delta : 플로우 단위시간 원시 데이터
         # * flow_packet : 플로우 패킷 원시 데이터
       
         record_view : "tolerance"
         # 분석 결과 확인 옵션
         # 종류 : all , tolerance ,err
         # * all : 모든 데이터 비교 결과확인
         # * tolerance : 비일치 , 공차허용범위 데이터 비교 결과확인
         # * err : 비일치 데이터 비교 결과확인
       
         enable_snappy : false
         # kbell Avro 연동 규격 압축 사용 여부 옵션
         # 종류 : true , false
         # * true : snappy로 압축된 kbell Avro 파일을 사용하는 경우
         # * false : snappy로 압축된 kbell Avro 파일을 사용하지 않는 경우
       
       flow_delta_info :
         # 데이터 분석 설정
         # 종류 : flow_raw_info , flow_delta_info , flow_packet_info
         # * flow_raw_info : 플로우 원시 데이터 분석 설정 옵션
         # * flow_delta_info : 플로우 단위시간 원시 데이터 분석 설정 옵션
         # * flow_packet_info : 플로우 패킷 원시 데이터 분석 설정 옵션
       
         etri_avro_dir_path: "$current_nif_project/nif_avro_verifier/avro_data/flow_delta/e_data"
         # ETRI Avro 파일 경로 (절대경로 , 서브디렉토리 기능 지원)
       
         kbell_avro_dir_path: "$current_nif_project/nif_avro_verifier/avro_data/flow_delta/k_data"
         # Kbell Avro 파일 경로 (절대경로 , 서브디렉토리 기능 지원)
       
         flow_cond_option :
             # Flow 공차 허용 범위 조건
             # ex) time_err_tolerance_usec : 1
             #     record_create_time = 2023-02-15 02:23:54.777228 +0000 UTC (1676427834777228)
             #     tolerance range = 2023-02-15 02:23:54.777227 +0000 UTC(1676427834777227) ~ 2023-02-15 02:23:54.777229 +0000 UTC (1676427834777229)
       
             time_sort_field_name: "record_create_time"
             # Flow 공차 허용 범위 조건 필드명
       
             time_err_tolerance_usec: 10
             # Flow 공차 허용 범위 값
       
         filter_option :
           # 비교 결과 중 특정 데이터 필터링 옵션
           
           match_time_range_filter:
               # 특정 시간 범위 필터링
               # 포맷 : 2020-01-24 09:43:42 , 2020-01-24 09:43:42.000000000
               # * column_name : 필터링 필드명
               # * start_time  : 시작시간
               # * end_time    : 종료시간        
               - column_name: "record_create_time"
                 start_time : "2023-02-15 11:23:08"
                 end_time : "2023-02-15 11:23:08.773"
       
           match_ipv4_range_filter:
               # IPv4 필드의 ip 범위를 필터링
               # * column_name : 필터링 필드명
               # * start_ip    : 시작 ipv4
               # * end_ip      : 종료 ipv4        
               - column_name: "src_ip_v4"
                 start_ip : "48.0.2.71"
                 end_ip : "48.0.2.72"
       
           match_data_filter:
               # 일치 필드 필터링
               # column_name : 필터링 필드
               # match_value : 일치값        
               - column_name : "protocol"
                 match_value : 6
               - column_name : "ip_version"
                 match_value : "IP_V4"
       
           match_ipv6_range_filter: 
               # IPv6 필드의 ip 범위를 필터링 , 시험용 옵션
               # * column_name : 필터링 필드명
               # * start_ip    : 시작 ipv6
               # * end_ip      : 종료 ipv6  
       ```       
     * 3-3-3) 플로우 패킷 원시 데이터 설정 예시
       ```
       project_info :
         mode : "flow_packet"
         # 분석 데이터 타입
         # 종류 : flow , flow_delta , flow_packet , test
         # * flow : 플로우 원시 데이터
         # * flow_delta : 플로우 단위시간 원시 데이터
         # * flow_packet : 플로우 패킷 원시 데이터
       
         record_view : "tolerance"
         # 분석 결과 확인 옵션
         # 종류 : all , tolerance ,err
         # * all : 모든 데이터 비교 결과확인
         # * tolerance : 비일치 , 공차허용범위 데이터 비교 결과확인
         # * err : 비일치 데이터 비교 결과확인  
       
         enable_snappy : false
         # kbell Avro 연동 규격 압축 사용 여부 옵션
         # 종류 : true , false
         # * true : snappy로 압축된 kbell Avro 파일을 사용하는 경우
         # * false : snappy로 압축된 kbell Avro 파일을 사용하지 않는 경우
       
       flow_packet_info :
         # 데이터 분석 설정
         # 종류 : flow_raw_info , flow_delta_info , flow_packet_info
         # * flow_raw_info : 플로우 원시 데이터 분석 설정 옵션
         # * flow_delta_info : 플로우 단위시간 원시 데이터 분석 설정 옵션
         # * flow_packet_info : 플로우 패킷 원시 데이터 분석 설정 옵션

         etri_avro_dir_path: "$current_nif_project/nif_avro_verifier/avro_data/flow_packet/e_data"
         # ETRI Avro 파일 경로 (절대경로 , 서브디렉토리 기능 지원)
         
         kbell_avro_dir_path: "$current_nif_project/nif_avro_verifier/avro_data/flow_packet/k_data"
         # Kbell Avro 파일 경로 (절대경로 , 서브디렉토리 기능 지원)  
       
         flow_cond_option :
             # Flow 공차 허용 범위 조건
             # ex) time_err_tolerance_usec : 1
             #     record_create_time = 2023-02-15 02:23:54.777228 +0000 UTC (1676427834777228)
             #     tolerance range = 2023-02-15 02:23:54.777227 +0000 UTC(1676427834777227) ~ 2023-02-15 02:23:54.777229 +0000 UTC (1676427834777229)
       
             time_sort_field_name: "packet_capture_time"
             # Flow 공차 허용 범위 조건 필드명
       
             time_err_tolerance_usec: 10
             # Flow 공차 허용 범위 값
       
         filter_option :
           # 비교 결과 중 특정 데이터 필터링 옵션 ( 원시데이터
       
           match_time_range_filter:
               # 특정 시간 범위 필터링
               # 포맷 : 2020-01-24 09:43:42 , 2020-01-24 09:43:42.000000000
               # * column_name : 필터링 필드명
               # * start_time  : 시작시간
               # * end_time    : 종료시간        
               - column_name: "record_create_time"
                 start_time : "2023-02-15 11:23:08"
                 end_time : "2023-02-15 11:23:08.773"
       
           match_ipv4_range_filter:
               # IPv4 필드의 ip 범위를 필터링
               # * column_name : 필터링 필드명
               # * start_ip    : 시작 ipv4
               # * end_ip      : 종료 ipv4        
               - column_name: "src_ip_v4"
                 start_ip : "48.0.2.71"
                 end_ip : "48.0.2.72"
       
           match_data_filter:
               # 일치 필드 필터링
               # column_name : 필터링 필드
               # match_value : 일치값        
               - column_name : "protocol"
                 match_value : 6
               - column_name : "ip_version"
                 match_value : "IP_V4"
       
           match_ipv6_range_filter: 
               # IPv6 필드의 ip 범위를 필터링 , 시험용 옵션
               # * column_name : 필터링 필드명
               # * start_ip    : 시작 ipv6
               # * end_ip      : 종료 ipv6           
       ```     
     
   * 3-4) 데이터 정확성 검증 프로그램 구동하여 분석결과 생성
     ```
     # cd $current_nif_project/nif_avro_verifier/go_source/bin
     # ./1_src_run.sh
     -------------------------------------------------------------------------------------------------------
     ...
     [    INFO :: 2023-07-06 10:09:16 ] ::: ==============================================================
     [    INFO :: 2023-07-06 10:09:16 ] ::: Save Result File : $res_file.log   <<<<<<<< 3-5) 절차의 분석결과 확인 파일명
     [    INFO :: 2023-07-06 10:09:16 ] ::: ==============================================================
     ...
     -------------------------------------------------------------------------------------------------------
     ```

   * 3-5) 분석결과 확인
     ```
     # cd $current_nif_project/nif_avro_verifier/go_source/logs
     # vi $res_file.log     <<<<<<<< 3-4) 절차의 분석결과 확인 파일명
     ```
