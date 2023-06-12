# 원시 데이터 정확성 검증 프로그램(nif_avro_verificater)


## 목적(용도)
ETRI 규격(Flow , Delta , Packet) AVRO와 KBELL 규격(Flow , Delta , Packet) AVRO를 비교하여 결과를 출력하는 프로그램


## 메뉴얼

- 개발환경

  * OS : Ubuntu 20.04
  * GO : go1.20.1.linux-amd64
  * CPU : 4
  * Memory : 4G

- 디렉토리 구성
  ```
  ├─avro_data : AVRO 파일 관리 디렉토리 ( 참고용 디렉토리 , Config에서 변경가능)
  │  ├─flow_delta : Flow 델타 데이터 AVRO 파일 디렉토리
  │  │  ├─e_data : ETRI 연동 규격의 AVRO 파일 관리 디렉토리 
  │  │  └─k_data : KBELL 연동 규격의 AVRO 파일 관리 디렉토리
  │  ├─flow_packet : Flow 패킷 원시 데이터 AVRO 파일 디렉토리
  │  │  ├─e_data : ETRI 연동 규격의 AVRO 파일 관리 디렉토리
  │  │  └─k_data : KBELL 연동 규격의 AVRO 파일 관리 디렉토리
  │  └─flow_raw : Flow 원시 데이터 AVRO 파일 디렉토리
  │      ├─e_data : ETRI 연동 규격의 AVRO 파일 관리 디렉토리
  │      └─k_data : KBELL 연동 규격의 AVRO 파일 관리 디렉토리
  ├─avro_sample_data : 연동 규격 버전 별 AVRO 파일 샘플 관리 디렉토리
  └─go_source : 프로그램 소스/바이너리 관리 디렉토리
      ├─bin : 패키지 구동 관리용 스크립트 관리 디렉토리
      ├─config : 패키지 설정 Config 관리 디렉토리
      ├─logs : 패키지 구동 로그 관리 디렉토리
      ├─pid : 빌드된 바이너리 구동 프로세스 아이디 관리 디렉토리
      ├─pkg : 실행/빌드에 필요한 패키지 관리용 디렉토리
      └─src : 프로그램 소스 관리 디렉토리
  ```     
  
 - 설치
 
    * 참고 
      * 소프트웨어 패키지 및 GO 설치경로는 아래의 경로에서 설치하는 것으로 가정하여 문서를 작성
     
        * /home/kbell/nif_avro_verificater_install_path
      
    * Go Download(1.20.1)
      ```
      $ wget https://go.dev/dl/go1.20.1.linux-amd64.tar.gz;
      $ tar -zxvf go1.20.1.linux-amd64.tar.gz;
      $ cd go/bin;
      $ ./go version;
        ######################################################################################################
        go version go1.20.1 linux/amd64
        ######################################################################################################
      ```
      
    * 작업 디렉토리 구성 
      ```
      $ pwd
      ######################################################################################################
      /home/kbell/nif_avro_verificater_install_path
      ######################################################################################################
    
      $ ls
      ######################################################################################################
      go  nif_avro_verificater
      ######################################################################################################
      ```
   
    * 프로젝트 경로 설정
      *  아래의 경로의 스크립트에서 GO 관련 경로로 맞춰주는 작업을 수행.
      ```
      $ cd nif_avro_verificater/go_source/bin/
      ```
      
        *  export PATH=$PATH:/home/kbell/nif_avro_verificater_install_path/go/bin/
        *  export GOROOT=/home/kbell/nif_avro_verificater_install_path/go
        *  export GOPATH=/home/kbell/nif_avro_verificater_install_path/go/pkg/go_path
       
      ```
      * 참고 *
      $ vi 0_create_go_mod.sh
      ######################################################################################################
      ... (중략)
      #export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/esk1223/env_default_pkg/go/pkg/go1.20.1.linux-amd64/bin/
      #export GOROOT=/home/esk1223/env_default_pkg/go/pkg/go1.20.1.linux-amd64
      #export GOPATH=/home/esk1223/env_default_pkg/go/pkg/go_path
      export PATH=$PATH:/home/kbell/nif_avro_verificater_install_path/go/bin/
      export GOROOT=/home/kbell/nif_avro_verificater_install_path/go
      export GOPATH=/home/kbell/nif_avro_verificater_install_path/go/pkg/go_path
      ... (중략)
      -->  위와 같이 0_get_gopkgs.sh , 1_src_run.sh , 2_bin_run.sh , 2_binary_build.sh 의 경로를 수정 후 저장
      ######################################################################################################
      
      $ chmod 755 0_create_go_mod.sh 0_get_gopkgs.sh 1_src_run.sh 2_bin_run.sh 2_bin_run_background.sh 2_binary_build.sh
      ```
      
    *  GO mod init 및 get pkgs
       ```
       $ ./0_create_go_mod.sh
       ######################################################################################################
       ========================================================
       Project Path >>
       : /home/kbell/nif_avro_verificater_install_path/nif_avro_verificater/go_source
       ========================================================
       Clean module >>
       file :  /home/kbell/nif_avro_verificater_install_path/nif_avro_verificater/go_source/src/go.mod
       ========================================================
       Create module init(kbell) >>
       go: creating new go.mod: module kbell
       go: to add module requirements and sums:
               go mod tidy
       ========================================================
       module show >>
       file :  /home/kbell/nif_avro_verificater_install_path/nif_avro_verificater/go_source/src/go.mod
       module kbell

       go 1.20
       ========================================================
       ========================================================
       Project Path >>
       : /home/kbell/nif_avro_verificater_install_path/nif_avro_verificater/go_source
       GOROOT >>
       : /home/kbell/nif_avro_verificater_install_path/go
       GOPATH >>
       : /home/kbell/nif_avro_verificater_install_path/nif_avro_verificater/go_source
       ========================================================
       Proejct Go Run (ver. interpreter) >>
       go: finding module for package github.com/hamba/avro/ocf
       go: finding module for package github.com/golang/snappy
       ... (생략)
       go: added github.com/rocketlaunchr/dataframe-go v0.0.0-20211025052708-a1030444159b
       ######################################################################################################
       --> 0_create_go_mod.sh 에서 mod init 완료 후 0_get_gopkgs.sh를 수행하여 관련 패키지를 가져온다.
       ```
 
  - 설정 및 실행
 
    * config 설정
      ```
      $ cd nif_avro_verificater/go_source/config
      ######################################################################################################
      ├─ config.yaml : 프로그램 실행 시 사용되는 config 파일
      ├─ config_flow_delta.yaml : Flow 델타 데이터 분석 시 사용하는 config 파일 예시
      ├─ config_flow_packet.yaml :  Flow 패킷 원시 데이터 분석 시 사용하는 config 파일 예시
      ├─ config_flow.yaml : Flow 원시 데이터 분석 시 사용하는 config 파일 예
      ######################################################################################################
      ```
    * config 옵션 상세
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

        etri_avro_dir_path: "/home/kbell/nif_avro_verificater_install_path/nif_avro_verificater/avro_data/flow_raw/e_data"
        # ETRI Avro 파일 경로 (절대경로 , 서브디렉토리 기능 지원)

        kbell_avro_dir_path: "/home/kbell/nif_avro_verificater_install_path/nif_avro_verificater/avro_data/flow_raw/k_data"
        # Kbell Avro 파일 경로 (절대경로 , 서브디렉토리 기능 지원)

        flow_cond_option :
            # Flow 공차 허용 범위 조건
            # ex) time_err_tolerance_msec : 1
            #     record_create_time = 2023-02-15 02:23:54.777228 +0000 UTC (1676427834777228)
            #     tolerance range = 2023-02-15 02:23:54.777227 +0000 UTC(1676427834777227) ~ 2023-02-15 02:23:54.777229 +0000 UTC (1676427834777229)

            time_sort_field_name: "record_create_time"
            # Flow 공차 허용 범위 조건 필드명

            time_err_tolerance_msec: 1
            # Flow 공차 허용 범위 값


        filter_option :
          # 비교 결과 중 특정 데이터 필터링 옵션 , 사용을 원하지 않는 경우 필드를 제거
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
              # IPv4 필드의 ip 범위를 필터링 , 시험용 옵션
              # * column_name : 필터링 필드명
              # * start_ip    : 시작 ipv6
              # * end_ip      : 종료 ipv6
      ```
    * 실행
      *  실행 방법은 2가지 종류를 제공
      
      *  인터프리터 방식
      ```
      $ cd nif_avro_verificater/go_source/bin/
      $ ./1_src_run.sh
      ```
      
      * 컴파일러 방식
      ```
      $ cd nif_avro_verificater/go_source/bin/
      
      # 소스 빌드
      $ ./2_binary_build.sh
      
      # 백그라운드 실행
      $ ./2_bin_run_background.sh start
      
      # 정지
      $ ./2_bin_run_background.sh stop
      ```
