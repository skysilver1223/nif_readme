# 검증용 trex 트래픽 생성 (시나리오#2)

## 목적(용도)
udp 트래픽 기반 주요 데이터 규격 검증 

## 탬플릿 정보

  * flow 정보 (sc2_flow_1.pcap)

    |category|info|value|
    |--------|----|-----|
    |ipv||4|
    |protocol||17|
    |east_info|east_port|12345|
    ||pkt_count|1|
    ||pkt_size_sum|185|
    ||pkt_size_square_sum|34225|
    ||pkt_size_min|185|
    ||pkt_size_max|185|
    ||payload_pkt_count|0|
    ||payload_size_sum|0|
    ||payload_size_square_sum|0|
    ||payload_size_min|0|
    ||payload_size_max|0|
    |west_info|west_port|44863|
    ||pkt_count|0|
    ||pkt_size_sum|0|
    ||pkt_size_square_sum|0|
    ||pkt_size_min|0|
    ||pkt_size_max|0|
    ||payload_pkt_count|0|
    ||payload_size_sum|0|
    ||payload_size_square_sum|0|
    ||payload_size_min|0|
    ||payload_size_max|0|
    |syn|count|0|
    |ack|count|0|
    |syn ack|count|0|
    |psh|count|0|
    |psh ack|count|0|
    |rst|count|0|
    |rst ack|count|0|
    |fin|count|0|
    |fin ack|count|0|
    |rt|count|0|

## 규격별 주요 검증 필드

  * 플로우 원시 데이터
  
      * flow_basic_attrs
        * flow_start_time, flow_end_time, flow_status, ip_version, east_ip_v4, west_ip_v4, east_ip_v6, west_ip_v6, protocol, east_port, west_port
      * direction_attrs
        * first_packet_direction
      * transport_stats(fwd , bwd)
        * first_pkt_time, last_pkt_time, pkt_count, pkt_size_sum, pkt_size_square_sum, pkt_size_min, pkt_size_max, payload_pkt_count, payload_size_sum, payload_size_square_sum, payload_size_min, payload_size_max, effective_trans_duration
      * tcp_stats(fwd , bwd)
        * none ( 재전송 템플릿 X )
      * tcp_signaling_attrs
        * none (TCP 템플릿 X)
       
  * 플로우 단위시간 원시 데이터
 
      * data_record_attrs
        * delta_start_time
      * flow_basic_attrs
        * flow_start_time, flow_end_time, flow_status, ip_version, east_ip_v4, west_ip_v4, east_ip_v6, west_ip_v6, protocol, east_port, west_port
      * direction_attrs
        * first_packet_direction
      * transport_stats(fwd , bwd)
        * first_pkt_time, last_pkt_time, pkt_count, pkt_size_sum, pkt_size_square_sum, pkt_size_min, pkt_size_max, payload_pkt_count, payload_size_sum, payload_size_square_sum, payload_size_min, payload_size_max, effective_trans_duration
      * tcp_stats(fwd , bwd)
        * none ( 재전송 템플릿 X )
  
  * 플로우 패킷 원시 데이터
  
      * data_record_attrs
        * inter_arrival_time
      * packet_basic_attrs
        * flow_start_time, ip_version, src_ip_v4, dst_ip_v4, src_ip_v6, dst_ip_v6, protocol, src_port, dst_port
      * direction_attrs
        * packet_direction
      * ethernet_frame_attrs
        * eth_len, eth_dst, eth_src, eth_type
      * ipv4_attrs
        * ip_len, ip_hdr_len, ip_tos, ip_ttl
      * ipv6_attrs
        * ipv6_plen, ipv6_tclass, ipv6_flow, ipv6_hlim
      * tcp_attrs
        * none (TCP 템플릿 X)
      * udp_attrs
        * udp_length
      * payload_attrs
        * payload
  
## 구동 방법

  * 디렉토리 구성
    ```
    ├─scen_0x : 시나리오 관리 디렉토리
    │  ├─batch : 시나리오 구동 배치 파일 관리 디렉토리
    │  ├─py : 탬플릿 기반 트래픽 생성 스트립트 관리 디렉토리
    │  ├─_run_scen.sh : 시나리오 구동 스크립트
    │  └─template : 시나리오 플로우 탬플릿 관리 디렉토리
    └─trex_script
    ```

  * 탬플릿 기반 트래픽 생성 스트립트 설명
    * 구동 예시
      *  trex> start -f $py_dir/scenario_astf.py -m 1 -d 100 -t pcap_file=flow_1.pcap|flow_1.pcap,rexmtthresh=x,no_delay=x,no_delay_counter=x,initwnd=x,rampup_sec=x,cps=x,dport=x
        
    *  옵션(-t) 설명
       * --pcap_files : 트래픽 생성 탬플릿 리스트
       * --rexmtthresh : 재전송을 트리거하기 위한 중복 ack 수
       * --no_delay :  2 bits: LSB - Nagle Algorithm (0 - Enabled, 1 - Disabled). MSB - Force Push (FP) (0 - Disabled, 1 - Enabled)
       * --delay_ack_msec : 지연 확인 시간 초과(msec)
       * --no_delay_counter : ack이 전송될 때까지 기다리는 recv 바이트 수입니다.
       * --initwnd : init window 값(MSS 단위)
       * --rampup_sec : 스케줄러 상승 시간(초)
       * --cps : connection per second
       * --dport : dst port

  *  trex 실행
  ```
  # 참고) 새로운 터미널을 연 후 실행
  $ /opt/trex/v3.xx
  $ sudo ./t-rex-64 -i --astf
  ```
               
  *  실행
  ```
  $ cd scen_02
  $ ./_run_scen.sh
  ```

  * 결과데이터 설명
    * -m 1 -d 50 옵션을 고정으로 사용하는 배치
    * sc2_flow_1.pcap 탬플릿 플로우 기반으로 49개의 플로우를 생성
    * 트래픽 생성시간( 2023. 06. 17. (토) 19:28:57 ~ 2023. 06. 17. (토) 19:29:55) 동안의 트래픽을 캡쳐하여 검증에 수행
  ```
  ######################################################################################################
  ================================================================== ( 트래픽 생성 시나리오 시작 )
  START( 2023. 06. 17. (토) 19:28:57 KST ) >>> 
   Scenario 1 ( = /home/kbell/src_kbell/scen_02/batch/s2_batch.txt)  (시나리오 s1_batch.txt 배치파일 정보 , 배치 순서대로 구동)
   Profile ( = 200)                                                  (시나리오 프로파일 ID)
  ==================================================================
  
  Executing line 1 : 'reset'
  Executing line 2 : 'start -f /home/kbell/src_kbell/scen_02/py/scenario_astf.py -m 1 -d 50 --pid 100 -t pcap_file="/home/kbell/src_kbell/scen_02/template/sc2_flow_1.pcap"'
  Executing line 3 : 'quit'
  Batch Running... /                                                  ( 트래픽 생성 동안 로딩 )
  
  ================================================================== ( 트래픽 생성 시나리오 종료 )
  END( 2023. 06. 17. (토) 19:29:55 KST ) >>>
   Scenario 1 ( = /home/kbell/src_kbell/scen_02/batch/s2_batch.txt) scenario End
   Profile ( = 200) End
  ==================================================================  
  ######################################################################################################
  ```

