# 검증용 trex 트래픽 생성 (시나리오#3)

## 목적(용도)
tcp/udp 트래픽 기반 주요 데이터 규격 검증

## 탬플릿 정보

  * flow 정보
    
    * sc3_tcp_flow_1.pcap
      |category|info|value|
      |--------|----|-----|
      |ipv||4|
      |protocol||6|
      |east_info|
      ||pkt_count|8|
      ||pkt_size_sum|3536|
      ||pkt_size_square_sum|2456968|
      ||pkt_size_min|66|
      ||pkt_size_max|996|
      ||payload_pkt_count|5|
      ||payload_size_sum|3000|
      ||payload_size_square_sum|2025000|
      ||payload_size_min|300|
      ||payload_size_max|900|
      |west_info|pkt_count|7|
      ||pkt_size_sum|3486|
      ||pkt_size_square_sum|2472796|
      ||pkt_size_min|66|
      ||pkt_size_max|970|
      ||payload_pkt_count|5|
      ||payload_size_sum|3016|
      ||payload_size_square_sum|2043072|
      ||payload_size_min|304|
      ||payload_size_max|904|
      |syn|count|1|
      |ack|count|1|
      |fin|count|2|
      |syn-ack|count|1|
      |psh-ack|count|10|

    * sc3_tcp_flow_2.pcap
      |category|info|value|
      |--------|----|-----|
      |ipv||4|
      |protocol||6|
      |east_info|
      ||pkt_count|13|
      ||pkt_size_sum|10616|
      ||pkt_size_square_sum|12707248|
      ||pkt_size_min|66|
      ||pkt_size_max|1716|
      ||payload_pkt_count|10|
      ||payload_size_sum|9750|
      ||payload_size_square_sum|11362500|
      ||payload_size_min|300|
      ||payload_size_max|1650|
      |west_info|pkt_count|14|
      ||pkt_size_sum|10716|
      ||pkt_size_square_sum|12019512|
      ||pkt_size_min|66|
      ||pkt_size_max|1514|
      ||payload_pkt_count|12|
      ||payload_size_sum|9784|
      ||payload_size_square_sum|10665920|
      ||payload_size_min|56|
      ||payload_size_max|1448|
      |syn|count|1|
      |ack|count|3|
      |fin|count|2|
      |syn-ack|count|1|
      |psh-ack|count|20|

    * sc3_tcp_flow_3.pcap
      |category|info|value|
      |--------|----|-----|
      |ipv||4|
      |protocol||6|
      |"east_info|
      |"|pkt_count|18|
      ||pkt_size_sum|21446|
      ||pkt_size_square_sum|36390028|
      ||pkt_size_min|66|
      ||pkt_size_max|2466|
      ||payload_pkt_count|15|
      ||payload_size_sum|20250|
      ||payload_size_square_sum|33637500|
      ||payload_size_min|300|
      ||payload_size_max|2400|
      |west_info|pkt_count|24|
      ||pkt_size_sum|21888|
      ||pkt_size_square_sum|26301600|
      ||pkt_size_min|66|
      ||pkt_size_max|1514|
      ||payload_pkt_count|22|
      ||payload_size_sum|20296|
      ||payload_size_square_sum|23516864|
      ||payload_size_min|56|
      ||payload_size_max|1448|
      |syn|count|1|
      |ack|count|8|
      |fin|count|2|
      |syn-ack|count|1|
      |psh-ack|count|30|

    * sc3_udp_flow_1.pcap
      |category|info|value|
      |--------|----|-----|
      |ipv||4|
      |protocol||17|
      |east_info|
      ||pkt_count|5|
      ||pkt_size_sum|1715|
      ||pkt_size_square_sum|588245|
      ||pkt_size_min|343|
      ||pkt_size_max|343|
      ||payload_pkt_count|5|
      ||payload_size_sum|1505|
      ||payload_size_square_sum|453005|
      ||payload_size_min|301|
      ||payload_size_max|301|
      |west_info|pkt_count|5|
      ||pkt_size_sum|2100|
      ||pkt_size_square_sum|891720|
      ||pkt_size_min|384|
      ||pkt_size_max|474|
      ||payload_pkt_count|5|
      ||payload_size_sum|1890|
      ||payload_size_square_sum|724140|
      ||payload_size_min|342|
      ||payload_size_max|432|
      |syn|count|0|
      |ack|count|0|
      |fin|count|0|
      |syn-ack|count|0|
      |psh-ack|count|0|

    * sc3_udp_flow_2.pcap
      |category|info|value|
      |--------|----|-----|
      |ipv||4|
      |protocol||17|
      |east_info|
      ||pkt_count|10|
      ||pkt_size_sum|2532|
      ||pkt_size_square_sum|670300|
      ||pkt_size_min|160|
      ||pkt_size_max|350|
      ||payload_pkt_count|10|
      ||payload_size_sum|2112|
      ||payload_size_square_sum|475252|
      ||payload_size_min|118|
      ||payload_size_max|308|
      |west_info|pkt_count|10|
      ||pkt_size_sum|4290|
      ||pkt_size_square_sum|1860660|
      ||pkt_size_min|384|
      ||pkt_size_max|474|
      ||payload_pkt_count|10|
      ||payload_size_sum|3870|
      ||payload_size_square_sum|1517940|
      ||payload_size_min|342|
      ||payload_size_max|432|
      |syn|count|0|
      |ack|count|0|
      |fin|count|0|
      |syn-ack|count|0|
      |psh-ack|count|0|

    * sc3_udp_flow_3.pcap
      |category|info|value|
      |--------|----|-----|
      |ipv||4|
      |protocol||17|
      |east_info|
      ||pkt_count|10|
      ||pkt_size_sum|2532|
      ||pkt_size_square_sum|670300|
      ||pkt_size_min|160|
      ||pkt_size_max|350|
      ||payload_pkt_count|10|
      ||payload_size_sum|2112|
      ||payload_size_square_sum|475252|
      ||payload_size_min|118|
      ||payload_size_max|308|
      |west_info|pkt_count|10|
      ||pkt_size_sum|4290|
      ||pkt_size_square_sum|1860660|
      ||pkt_size_min|384|
      ||pkt_size_max|474|
      ||payload_pkt_count|10|
      ||payload_size_sum|3870|
      ||payload_size_square_sum|1517940|
      ||payload_size_min|342|
      ||payload_size_max|432|
      |syn|count|0|
      |ack|count|0|
      |fin|count|0|
      |syn-ack|count|0|
      |psh-ack|count|0|    
                
  * flow 별 active time 정보
    * sc3_tcp_flow_1.pcap
    
      |s_delay_sec|flow active time(sec)|
      |-----------|---------------------|
      |10|5|
      |20|10|
      |40|20|

    * sc3_tcp_flow_2.pcap
      
      |s_delay_sec|flow active time(sec)|
      |-----------|---------------------|
      |10|10|
      |20|20|
      |40|40|
      
    * sc3_tcp_flow_3.pcap
      
      |s_delay_sec|flow active time(sec)|
      |-----------|---------------------|
      |10|15|
      |20|30|
      |40|60|
      
    * sc3_udp_flow_1.pcap
      
      |s_delay_sec|flow active time(sec)|
      |-----------|---------------------|
      |10|5|
      |20|10| 
      |40|20|

    * sc3_udp_flow_2.pcap      
      |s_delay_sec|flow active time(sec)|
      |-----------|---------------------|
      |10|10|
      |20|20|
      |40|40|     
      
    * sc3_udp_flow_3.pcap      
      |s_delay_sec|flow active time(sec)|
      |-----------|---------------------|
      |10|15|
      |20|30|
      |40|60|     
    
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
       * --s_delay_sec : 서버 딜레이 명령
       * --ipv6_mode : ip range를 ipv6로 변
     
  *  실행
  ```
  $ cd scen_01
  $ ./_run_scen.sh
  ```

  * 결과데이터 설명
    * -m 1 -d 100 옵션을 고정으로 사용하는 배치
    * 각 탬플릿 플로우 기반으로 49개의 플로우를 생성
    * 트래픽 생성시간( 2023-07-06 18:24:26 ~ 2023-07-06 18:26:28) 동안의 트래픽을 캡쳐하여 검증에 수행
  ```
  ######################################################################################################
  ==================================================================
  START( 2023-07-06 18:24:26 ) >>>
   Scenario 3 ( = /home/kbell/src_kbell/scen_03/batch/s3_batch.txt)
   Profile ( = 300)
  ==================================================================
  
  Executing line 1 : 'reset'
  Executing line 2 : 'start -f /home/kbell/src_kbell/scen_03/py/scenario_astf.py -m 1 -d 100 --pid 305 -t pcap_file="/home/kbell/src_kbell/scen_03/template/sc3_tcp_flow_1.pcap",cps=1,s_delay_sec=10,dport=8081'
  Executing line 3 : 'start -f /home/kbell/src_kbell/scen_03/py/scenario_astf.py -m 1 -d 100 --pid 304 -t pcap_file="/home/kbell/src_kbell/scen_03/template/sc3_tcp_flow_2.pcap",cps=1,s_delay_sec=10,dport=8082'
  Executing line 4 : 'start -f /home/kbell/src_kbell/scen_03/py/scenario_astf.py -m 1 -d 100 --pid 303 -t pcap_file="/home/kbell/src_kbell/scen_03/template/sc3_tcp_flow_3.pcap",cps=1,s_delay_sec=10,dport=8083'
  Executing line 5 : 'start -f /home/kbell/src_kbell/scen_03/py/scenario_astf.py -m 1 -d 100 --pid 302 -t pcap_file="/home/kbell/src_kbell/scen_03/template/sc3_udp_flow_1.pcap",cps=1,s_delay_sec=10,dport=8084'
  Executing line 6 : 'start -f /home/kbell/src_kbell/scen_03/py/scenario_astf.py -m 1 -d 100 --pid 301 -t pcap_file="/home/kbell/src_kbell/scen_03/template/sc3_udp_flow_2.pcap",cps=1,s_delay_sec=10,dport=8085'
  Executing line 7 : 'start -f /home/kbell/src_kbell/scen_03/py/scenario_astf.py -m 1 -d 100 --pid 300 -t pcap_file="/home/kbell/src_kbell/scen_03/template/sc3_udp_flow_3.pcap",cps=1,s_delay_sec=10,dport=8086'
  Executing line 8 : 'quit'
  Batch Running... |
  
  ==================================================================
  END( 2023-07-06 18:26:28 ) >>>
   Scenario 3 ( = /home/kbell/src_kbell/scen_03/batch/s3_batch.txt) scenario End
   Profile ( = 300) End
==================================================================
  ######################################################################################################
  ```
