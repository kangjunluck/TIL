# CI/CD Jenkins 자동배포

#### Ubuntu

- 리눅스의 배포판 중 하나로, 대부분의 리눅스 배포판이 서버용으로 만들어 졌다면, 우분투는 개인, 데스크탑에 최적화 되도록 사용자 편의를 중점으로 개발되었다.
- 누구나 무료로 사용할 수 있다.

#### EC2

- 아마존에서 제공하는 가상 서버
- 가상 서버를 구축하고 보안 및 네트워킹을 구성할 수 있다.
- AWS public 키 저장, 사용자가 개인키를 저장한다.

#### Nginx 가벼움과 높은 성능

- 동시접속 처리에 특화된 웹 서버 프로그램
- 정적 파일을 처리, HTTP 서버로 역할 (HTML, CSS 정보를 웹 브라우저에 전송한다.)
- 프록시 서버로서의 역할

#### 기본 과정

1. 배포할 서버에 jenkins와 필요한 기술들을 설치한다.( Java, Jenkins, MySQL, npm)
2. 서버와 gitlab을 연결시킨다.
3. jenkins를 통해 gitlab의 특정 branch(develop) 변경 감지 시, 특정 명령(build 및 실행)을 수행하도록 한다.



### 1. 배포할 서버에 Jenkins와 필요한 기술 설치

- apt 업데이트

  ```
  $apt-get update
  ```

- JDK 8 설치

  ```
  $sudo apt-get install openjdk-8-jdk
  ```

- Jenkins 설치

  ```
  $sudo apt-get install jenkins
  $sudo jenkins # jenkins로 들어가
  HTTP_PORT=9090 # 원하는 포트로 변경한다.
  
  $sudo systemctl status jenkins
  ```

- Jenkins로 들어가 필요한 플러그인을 설치해준다.



### 2. 서버와 gitlab을 연결시킨다.

- jenkins 플러그인 설치

  Blue Ocean 모두

  gitlab 모두 (GitLab Hook 제외)

  NodeJS

- Jenkins 관리 → 시스템 설정

  GitLab 파트

   - connection name : 아무거나

   - Gitlab host URL : gitlab 주소 (gitlab.com)

   - Credentials : Add 후, jenkins를 눌러 생성

      - Add credentials

      - Kind : GitLab API token

      - API token : **발급받은 personal token**

        :star:personal token

        gitlab

        - User Settings > Access Token 통해 발급

- 새로운 Item (PJT) 생성

  구성 → 소스코드 관리 탭

  - Git 체크

  - Repository URL : gitlab clone 주소

    :warning:Access denied가 뜨는 경우,

    https://gitlabID:Personal access token@Repository주소 로 적는다

  - Credentials : add - jenkins

    - Add credentials

    - Kind : SSH Username with private Key 선택

    - ID, Username을 입력한다. username은 gitlab email 형식이다 (id는 자유)

    - Private Key : ssh key를 입력한다.

      :star:ssh key

      1. ubuntu 서버 내에서 아래 명령어로 키를 발급받는다.

         ```
         $ssh-keygen -t rsa -C "깃랩메일주소"
         ```

      2. cd home/ubuntu/.ssh/ 경로에 cat id_rsa.pub 파일을 열고 ssh-rsa를 제외하고 복사한다.

      3. gitlab에서 ssh key 입력 후 키 생성하여 이를 jenkins에 입력한다.

  - Build 유발

    - Build when a change is pushed to GitLab. GitLab webhook  체크
    - 고급 > **key 생성 > 보관**

- GitLab 프로젝트 설정 > Integrations > Go to Webhooks

  - URL : build when ~~~ 맨 뒤 주소
  - Secret token : **보관해둔 key**
  - Push branch : 만들 branch 지정 (develop)

### jenkins를 통해 gitlab의 특정 branch(develop) 변경 감지 시, 특정 명령(build 및 실행)을 수행하도록 한다.

- 구성 > 빌드 탭

  - MobaXtern 내에서 nginx를 설치하고 nginx를 통해 프론트엔드 빌드 파일 경로를 설정해준다.

    ```
    $cd nginx/site-available # nginx로 들어가
    $vi default
    server {
    	~~~~~
    	root /var/lib/jenkins/workspace/PJT/frontend/dist; # dist 파일 경로 설정
    	~~~~~
    }
    ```

    참고, 404에러 방지

    ```
    location / {
    	try_files $rui $uri/ /index.html;
    }
    ```

  - Command

    ```
    cd frontend
    npm install
    npm run build
    cd dist
    
    cd ../../Backend
    cd init
    echo $PATH
    chmod +x gradlew
    ./gradlew clean
    ./gradlew build
    ```

  - Script

    ```
    fuser -k 8080/tcp
    nohup java -jar /var/lib/jenkins/workspace/PJT/Backend/init/build/libs/init-0.0.1-SNAPSHOT.jar &
    ```

    



