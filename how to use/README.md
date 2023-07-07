
# NIF 시험 시나리오 절차 매뉴얼 

## 목적(용도)
본 문서는 검증용 트래픽 생성 - 검증용 Avro 생성 - 원시 데이터 검증까지의 절차를 정리한 문서이다.

## 시나리오 플로우
데이터 검증은 아래와 같은 플로우로 절차를 진행한다.

![image](https://github.com/skysilver1223/nif_readme/assets/109940215/ae6eba67-78a0-4042-8d66-c26b8c0cc5f9)

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

   해당 시나리오는 플로우 구성상 아래의 파트 부분을 수행하며, 아래의 절차 대로 수행한다.
   ![image](https://github.com/skysilver1223/nif_readme/assets/109940215/119364a2-fb1b-450a-a420-4ec3d4317ef3)

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

   해당 시나리오는 플로우 구성상 아래의 파트 부분을 수행하며, 아래의 절차 대로 수행한다.
   ![image](https://github.com/skysilver1223/nif_readme/assets/109940215/37d8a8d8-15a4-4c11-bf65-36dfa606307b)

   * 2-1) 트래픽 덤프 결과를 검증용 Avro파일 생성 프로그램의 분석용 디렉토리로 이동
       ```
       검증용 Avro파일 생성 프로그램의 분석용 디렉토리 확인방법
       - 검증용 Avro파일 생성 프로그램의 Config 파일의 project_info.pcap_file_dir_path 경로 확인
       
       # vi $current_nif_project/nif_pcap_verificater/go_source/config/config.yaml
       -------------------------------------------------------------------------------------------------------  
       project_info :
       ...     
         pcap_file_dir_path: "$current_nif_project/nif_pcap_verificater/pcap_samples"
         # 분석대상 pcap 디렉토리
       ...
       -------------------------------------------------------------------------------------------------------

       # mv $res_pcap_file_name $current_nif_project/nif_pcap_verificater/pcap_samples
       ```
       
   * 2-2) 트래픽 파일(Pcap)에 대한 분석을 위한 Config를 설정
     ```
     # vi $current_nif_project/nif_pcap_verificater/go_source/config/config.yaml
     -------------------------------------------------------------------------------------------------------
     project_info :
       res_file_dir_path: "$current_nif_project/nif_pcap_verificater/pcap_results"
       # pcap 분석 결과 생성 디렉토리
     
       pcap_file_dir_path: "$current_nif_project/nif_pcap_verificater/pcap_samples"
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
     # cd $current_nif_project/nif_pcap_verificater/go_source/bin
     # ./1_src_run.sh
     ```


   * 2-4) 검증용 결과물 확인
     ```
     검증용 Avro파일 생성 프로그램의 결과용 디렉토리 확인방법
     - 검증용 Avro파일 생성 프로그램의 Config 파일의 project_info.res_file_dir_path 경로 확인
       
     # vi $current_nif_project/nif_pcap_verificater/go_source/config/config.yaml
     -------------------------------------------------------------------------------------------------------  
     project_info :
     ...     
       res_file_dir_path: "$current_nif_project/nif_pcap_verificater/pcap_results"
       # pcap 분석 결과 생성 디렉토리
     ...
     -------------------------------------------------------------------------------------------------------
     
     # cd $current_nif_project/nif_pcap_verificater/pcap_results
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
  
     
