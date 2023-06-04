# 검증용 Avro 파일 생성 프로그램(nif_pcap_verificater)

## 목적(용도)
Pcap File 기반 Kbell 규격(Flow , Delta , Packet) AVRO 파일을 생성하는 프로그램

## 구성 메뉴얼

- 개발환경

  * OS : Ubuntu 20.04
  * GO : go1.20.1.linux-amd64
  * CPU : 4
  * Memory : 4G

- 디렉토리 구성
  ```
  ├─go_source : 프로그램 소스/바이너리 관리 디렉토리
  │  ├─bin : 패키지 구동 관리용 스크립트 관리 디렉토리
  │  ├─config : 패키지 설정 Config 관리 디렉토리
  │  ├─logs : 패키지 구동 로그 관리 디렉토리
  │  ├─pid : 빌드된 바이너리 구동 프로세스 아이디 관리 디렉토리
  │  ├─pkg : 실행/빌드에 필요한 패키지 관리용 디렉토리
  │  └─src : 프로그램 소스 관리 디렉토리
  ├─pcap_results : pcap 파일 분석 결과 생성 디렉토리 ( 참고용 디렉토리 , Config에서 변경가능)  
  └─pcap_samples : 분석대상 pcap 파일 관리 디렉토리 ( 참고용 디렉토리 , Config에서 변경가능)
  ```
  
- 설치

    * 참고 
      * 소프트웨어 패키지 및 GO 설치경로는 아래의 경로에서 설치하는 것으로 가정하여 문서를 작성
     
        * /home/kbell/nif_pcap_verificater_install_path

      
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
      /home/kbell/nif_pcap_verificater_install_path
      ######################################################################################################
    
      $ ls
      ######################################################################################################
      go  nif_pcap_verificater
      ######################################################################################################
      ```
   
    * 프로젝트 경로 설정
      *  아래의 경로의 스크립트에서 GO 관련 경로로 맞춰주는 작업을 수행.
      ```
      $ cd nif_pcap_verificater/go_source/bin/
      ```
      
        *  export PATH=$PATH:/home/kbell/nif_pcap_verificater_install_path/go/bin/
        *  export GOROOT=/home/kbell/nif_pcap_verificater_install_path/go
        *  export GOPATH=/home/kbell/nif_pcap_verificater_install_path/go/pkg/go_path
       
      ```
      * 참고 *
      $ vi 0_create_go_mod.sh
      ######################################################################################################
      ... (중략)
      #export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/esk1223/env_default_pkg/go/pkg/go1.20.1.linux-amd64/bin/
      #export GOROOT=/home/esk1223/env_default_pkg/go/pkg/go1.20.1.linux-amd64
      #export GOPATH=/home/esk1223/env_default_pkg/go/pkg/go_path
      export PATH=$PATH:/home/kbell/nif_pcap_verificater_install_path/go/bin/
      export GOROOT=/home/kbell/nif_pcap_verificater_install_path/go
      export GOPATH=/home/kbell/nif_pcap_verificater_install_path/go/pkg/go_path
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
      : /home/kbell/nif_pcap_verificater_install_path/nif_pcap_verificater/go_source
      ========================================================
      Clean module >>
      file :  /home/kbell/nif_pcap_verificater_install_path/nif_pcap_verificater/go_source/src/go.mod
      ========================================================
      Create module init(kbell) >>
      go: creating new go.mod: module kbell
      go: to add module requirements and sums:
              go mod tidy
      ========================================================
      module show >>
      file :  /home/kbell/nif_pcap_verificater_install_path/nif_pcap_verificater/go_source/src/go.mod
      module kbell

      go 1.20
      ========================================================
      ========================================================
      Project Path >>
      : /home/kbell/nif_pcap_verificater_install_path/nif_pcap_verificater/go_source
      GOROOT >>
      : /home/kbell/nif_pcap_verificater_install_path/go
      GOPATH >>
      : /home/kbell/nif_pcap_verificater_install_path/nif_pcap_verificater/go_source
      ========================================================
      Proejct Go Run (ver. interpreter) >>
      go: finding module for package github.com/golang/snappy
      go: finding module for package gopkg.in/natefinch/lumberjack.v2
      ... (생략)
      go: added github.com/elodina/go-avro v0.0.0-20160406082632-0c8185d9a3ba
      ######################################################################################################
      --> 0_create_go_mod.sh 에서 mod init 완료 후 0_get_gopkgs.sh를 수행하여 관련 패키지를 가져온다.
      ```
 
   - 실행
 
    * config 설정
      ```
      $ cd nif_pcap_verificater/go_source/config
      ```
     * config 옵션 상세
       ```
       project_info :

         res_file_dir_path: "/home/esk1223/project_mng_dir/029_NIF_DATA_VERIFICATION/current_nif_project/nif_pcap_verificater/pcap_results"
         # pcap 분석 결과 생성 디렉토리

         pcap_file_dir_path: "/home/esk1223/project_mng_dir/029_NIF_DATA_VERIFICATION/current_nif_project/nif_pcap_verificater/pcap_samples"
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

           min_flow_start_time : "2023-02-08 12:54:00"
           #flow 최소 시작 시간
           # ex) 2020-01-24 09:43:42 , 2020-01-24 09:43:42.000000000

           delta_start_time : "2023-02-08 12:54:00"
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
         - pcap_fname: "test_udp.pcap"
           use_flag: false
         - pcap_fname: "kbell_tcp_sample.pcap"
           use_flag: true
         - pcap_fname: "cap230509d.pcap"
           use_flag: false       
       ```
       
    * 실행
      *  실행 방법은 2가지 종류를 제공
      
      *  인터프리터 방식
      ```
      $ cd nif_pcap_verificater/go_source/bin/
      $ ./1_src_run.sh
      ```
      
      * 컴파일러 방식
      ```
      $ cd nif_pcap_verificater/go_source/bin/
      
      # 소스 빌드
      $ ./2_binary_build.sh
      
      # 백그라운드 실행
      $ ./2_bin_run_background.sh start
      
      # 정지
      $ ./2_bin_run_background.sh stop
      ```       