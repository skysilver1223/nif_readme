# 검증용 trex 트래픽 생성 (시나리오#1)

## 목적(용도)
tcp 트래픽  기반 주요 데이터 규격 검증

## 탬플릿 정보

  * flow 정보 (sc1_flow_1.pcap)

    |category|info|value|
    |ipv||4|
    |protocol||6|
    |east_info|east_port|59496|
    ||pkt_count|5|
    ||pkt_size_sum|498|
    ||pkt_size_square_sum|56820|
    ||pkt_size_min|66|
    ||pkt_size_max|146|
    ||payload_pkt_count|2|
    ||payload_size_sum|160|
    ||payload_size_square_sum|12800|
    ||payload_size_min|80|
    ||payload_size_max|80|
    |west_info|west_port|8080|
    ||pkt_count|4|
    ||pkt_size_sum|432|
    ||pkt_size_square_sum|52464|
    ||pkt_size_min|66|
    ||pkt_size_max|146|
    ||payload_pkt_count|2|
    ||payload_size_sum|160|
    ||payload_size_square_sum|12800|
    ||payload_size_min|80|
    ||payload_size_max|80|
    |syn|count|1|
    |ack|count|1|
    |syn ack|count|1|
    |psh|count|0|
    |psh ack|count|4|
    |rst|count|0|
    |rst ack|count|0|
    |fin|count|0|
    |fin ack|count|2|
    |rt|count|1|

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
        * syn_first_time, syn_last_time, syn_ack_first_time, syn_ack_last_time, ack_time, syn_count, syn_ack_count, rst_first_time, rst_last_time, rst_count, fin_first_time, fin_last_time, fin_ack_time, fin_count, window_close_count, rt_count, rtt_sum, rtt_square_sum, rtt_min, rtt_max
       
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
        * tcp_len, tcp_hdr_len, tcp_seq, tcp_ack, tcp_flags_ns, tcp_flags_cwr, tcp_flags_ecn, tcp_flags_urg, tcp_flags_ack, tcp_flags_push, tcp_flags_reset, tcp_flags_syn, tcp_flags_fin, tcp_window_size
      * udp_attrs
        * none ( UDP 템플릿 X )
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
     
  *  실행
  ```
  $ cd scen_01
  $ ./_run_scen.sh
  ```

  * 결과데이터 설명
    * -m 1 -d 50 옵션을 고정으로 사용하는 배치
    * sc1_flow_1.pcap 탬플릿 플로우 기반으로 49개의 플로우를 생성
  ```
  ######################################################################################################
  ================================================================== ( 트래픽 생성 시나리오 시작 )
  START( 2023. 06. 17. (토) 19:28:57 KST ) >>> 
   Scenario 1 ( = /home/kbell/src_kbell/scen_01/batch/s1_batch.txt)  (시나리오 s1_batch.txt 배치파일 정보 , 배치 순서대로 구동)
   Profile ( = 100)                                                  (시나리오 프로파일 ID)
  ==================================================================
  
  Executing line 1 : 'reset'
  Executing line 2 : 'start -f /home/kbell/src_kbell/scen_01/py/scenario_astf.py -m 1 -d 50 --pid 100 -t pcap_file="/home/kbell/src_kbell/scen_01/template/sc1_flow_1.pcap"'
  Executing line 3 : 'quit'
  Batch Running... /                                                  ( 트래픽 생성 동안 로딩 )
  
  ================================================================== ( 트래픽 생성 시나리오 종료 )
  END( 2023. 06. 17. (토) 19:29:55 KST ) >>>
   Scenario 1 ( = /home/kbell/src_kbell/scen_01/batch/s1_batch.txt) scenario End
   Profile ( = 100) End
  ==================================================================  
  ######################################################################################################
  ```
