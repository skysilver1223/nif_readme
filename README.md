# 검증용 trex 트래픽 생성 샘플#1

## 목적(용도)
tcp 트래픽 template 기반 주요 데이터 규격 검증

## 탬플릿 정보
  * flow #1

    |category|info|value|
    |--------|----|-----|
    |ipv||4|
    |protocol||6|
    |east_info|east_port|59496|
    ||pkt_count|7|
    ||pkt_size_sum|630|
    ||pkt_size_square_sum|65532|
    ||pkt_size_min|66|
    ||pkt_size_max|146|
    ||payload_pkt_count|2|
    ||payload_size_sum|160|
    ||payload_size_square_sum|12800|
    ||payload_size_min|80|
    ||payload_size_max|80|
    |west_info|west_port|8080|
    ||pkt_count|7|
    ||pkt_size_sum|630|
    ||pkt_size_square_sum|65532|
    ||pkt_size_min|66|
    ||pkt_size_max|146|
    ||payload_pkt_count|2|
    ||payload_size_sum|160|
    ||payload_size_square_sum|12800|
    ||payload_size_min|80|
    ||payload_size_max|80|
    |syn|count|1|
    |ack|count|6|
    |syn ack|count|1|
    |psh|count|0|
    |psh ack|count|4|
    |rst|count|0|
    |rst ack|count|0|
    |fin|count|0|
    |fin ack|count|2|
    |rt|count|6|

  * flow #2

    |category|info|value|
    |--------|----|-----|
    |ipv||4|
    |protocol||6|
    |east_info|east_port|59498|
    ||pkt_count|4|
    ||pkt_size_sum|352|
    ||pkt_size_square_sum|35504|
    ||pkt_size_min|66|
    ||pkt_size_max|146|
    ||payload_pkt_count|1|
    ||payload_size_sum|80|
    ||payload_size_square_sum|6400|
    ||payload_size_min|80|
    ||payload_size_max|80|
    |west_info|west_port|8080|
    ||pkt_count|3|
    ||pkt_size_sum|194|
    ||pkt_size_square_sum|12748|
    ||pkt_size_min|54|
    ||pkt_size_max|74|
    ||payload_pkt_count|0|
    ||payload_size_sum|0|
    ||payload_size_square_sum|0|
    ||payload_size_min|0|
    ||payload_size_max|0|
    |syn|count|1|
    |ack|count|2|
    |syn ack|count|1|
    |psh|count|0|
    |psh ack|count|1|
    |rst|count|1|
    |rst ack|count|0|
    |fin|count|0|
    |fin ack|count|1|
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
    ├─ cfg : 설정파일 관리 디렉토리
    │  ├─ template.yaml : 템플릿 기반 트래픽 설정 값
    │  ├─ config.csv :  trex 구동 단위별 설정 값 
    ├─ logs : 구동로그 관리
    ├─ pcap_template :  pcap 템플릿 관리 디렉토리
    ├─ run.sh : 트래픽 성성기 구동 스크립트
    ```
    
  *  template.yaml 주요 옵션 상세
    *  
  *  config.csv 주요 옵션 상세

  *  실행
  ```
  $ ./run.sh
  ```
