# 검증용 trex 트래픽 생성 샘플#3

## 목적(용도)
tcp 트래픽(재전송 포함 - packet index(11)) template 기반 주요 데이터 규격 검증

## 탬플릿 정보
  * flow #1

    |category|info|value|
    |--------|----|-----|
    |ipv||4|
    |protocol||6|
    |east_info|east_port|50863|
    ||pkt_count|5|
    ||pkt_size_sum|613|
    ||pkt_size_square_sum|148625|
    ||pkt_size_min|60|
    ||pkt_size_max|365|
    ||payload_pkt_count|1|
    ||payload_size_sum|311|
    ||payload_size_square_sum|96721|
    ||payload_size_min|311|
    ||payload_size_max|311|
    |west_info|west_port|80|
    ||pkt_count|7|
    ||pkt_size_sum|7024|
    ||pkt_size_square_sum|8790488|
    ||pkt_size_min|62|
    ||pkt_size_max|1314|
    ||payload_pkt_count|6|
    ||payload_size_sum|6638|
    ||payload_size_square_sum|8052244|
    ||payload_size_min|338|
    ||payload_size_max|1260|
    |syn|count|1|
    |ack|count|6|
    |syn ack|count|1|
    |psh|count|0|
    |psh ack|count|4|
    |rst|count|0|
    |rst ack|count|1|
    |fin|count|0|
    |fin ack|count|0|
    |rt|count|5|

## 규격별 주요 검증 필드

  * 플로우 원시 데이터
  
      * flow_basic_attrs
        * flow_start_time, flow_end_time, flow_status, ip_version, east_ip_v4, west_ip_v4, east_ip_v6, west_ip_v6, protocol, east_port, west_port
      * direction_attrs
        * first_packet_direction
      * transport_stats(fwd , bwd)
        * first_pkt_time, last_pkt_time, pkt_count, pkt_size_sum, pkt_size_square_sum, pkt_size_min, pkt_size_max, payload_pkt_count, payload_size_sum, payload_size_square_sum, payload_size_min, payload_size_max, effective_trans_duration
      * tcp_stats(fwd , bwd)
        * retrans_pkt_count, retrans_pkt_size_sum, retrans_pkt_size_square_sum, retrans_pkt_size_min, retrans_pkt_size_max, retrans_payload_size_sum, retrans_payload_size_square_sum, retrans_payload_size_min, retrans_payload_size_max
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
        * retrans_pkt_count, retrans_pkt_size_sum, retrans_pkt_size_square_sum, retrans_pkt_size_min, retrans_pkt_size_max, retrans_payload_size_sum, retrans_payload_size_square_sum, retrans_payload_size_min, retrans_payload_size_max
  
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
  ```
    - duration : 30.0
    # 테스트 기간(초)

    generator :
    # 템플릿 기반 트래픽 생성 정보
            distribution : "seq"
            clients_start : "16.0.0.1"
            clients_end   : "16.0.0.255"
            servers_start : "48.0.0.1"
            servers_end   : "48.0.255.255"
            clients_per_gb : 201 # Deprecated
            min_clients    : 101 # Deprecated
            dual_port_mask : "1.0.0.0"
            tcp_aging      : 0  # 흐름할당 취소 시간
            udp_aging      : 0 # 흐름할당 취소 시간

    cap_ipg    : false
    # ipg(Interpacket gap) : https://en.wikipedia.org/wiki/Interpacket_gap
    # true는 IPG가 cap 파일에서 가져온 것
    # false는 IPG가 템플릿 섹션별로 가져온 것

    cap_ipg_min    : 10
    #(if (pkt_ipg<cap_ipg_min) { pkt_ipg=cap_override_ipg} )

    cap_override_ipg    : 100
    # Value to override (microseconds)

    mac_override_by_ip : 0
    # 0, (legacy value: false) - 포트 구성/ARP 쿼리에서 MAC
    # 1, (legacy value: true) - 클라이언트에서 보낸 패킷의 소스 MAC
    # 2 - 모든 방향으로 모든 MAC를 교체

    cap_info :
       - name: /home/kbell/src_kbell/scenario_03/pcap_template/kbell_tcpretrans_sample_1.pcap
         # 템플릿 pcap 파일

         cps : 1
         #초당 연결 수

         ipg : 10000
         #cap_ipg : false가 포함된 경우 , 패킷 간 간격을 마이크로초 단위로 설정

         rtt : 10000
         # ipg와 동일한 값으로 설정 (microseconds).

         w   : 1
         # Default value: w=1.
         # IP 생성기에 흐름을 생성하는 방법.
         # 만일 w=2, 동일한 템플릿의 두 흐름이 버스트에 생성됩니다 

         keep_src_port: false
         # 원래 TCP/UDP 소스 포트 사용 여부

  ```
    
  *  config.csv 주요 옵션 상세
  ```
  csv 설정값 별 옵션 정보
  thread_num : t-rex-6 의 '-c' option
  multiplier_num : t-rex-6 의 '-m' option
  duration : t-rex-6 의 '-d' option
  active_flows: t-rex-6 의 '--active-flows' option
  learn_mode : t-rex-6 의 '--learn-mod' option
  
  -> csv 각 라인 단위 별 설정값을 기반으로 trex 구동
  
  ex)
  thread_num,multiplier_num,duration,active_flows,learn_mode
  1,1,60,10000,3
  
  ```
  
  *  실행
  ```
  $ ./run.sh
  ```
