# Gradle Install

Gradle은 ```Gradle Wrapper```를 사용해서 설치 없이 사용 할 수 있지만 설치가 필요하다는 가정하에 진행합니다.

Gradle은 ```JDK(Java Development Kit) Version 8```이상에서 동작 하기 때문에 JDK 8 이상이 설치되어 있다면 주요 OS에서 동작한다.

자바 버전은 아래 명령어로 확인할 수 있다.
```
java -version
```
Gradle은 기본적으로 Groovy 라이브러리에 포함되어 배포 되기 때문에 Groovy는 Gradle을 설치 할 필요가 없다. 기존에 설치되어 있는 Groovy가 있어도 그 버전은 무시합니다.

### Windows 설치
1. [Gradle 다운로드](https://gradle.org/releases/) 에서 원하는 버전을 다운로드 받는다. (이 문서는 2021-05-05 기준 최신 버전 v7.0 진행)<br>
   ※ binary-only는 실행 파일만 있고 complete는 각종 문서를 포함하고 있다. 둘중 원하는 것을 다운 받는다.
2. [Gradle Install User Guide](https://docs.gradle.org/7.0/userguide/installation.html) 에 따라 ```C:\Gradle``` 디렉토리를 만들고 다운 받은 파일을 해당 디렉토리에 압축 해제한다.
3. 다운 받은 Gradle을 환경변수에 설정해준다.
    1. ```내컴퓨터 -> 속성 -> 고급 시스템 설정 -> 환경변수```를 클릭한다.
    2. 시스템 변수 하단에 새로만들기를 클릭하고 변수 이름에 ```GRADLE_HOME```, 변수값에 ```C:\Gradle\gradle-7.0```을 입력하고 확인을 클릭한다.
    3. 시스템 변수의 ```Path```에서 ```편집```을 클릭하여, 기존에 작성된 ```Path```는 놔두고 ```%GRADLE_HOME/bin%```을 추가하고 확인을 클릭한다.
    4. ```cmd```, ```Windows Powershell```, ```git bash```등과 같이 터미널을 이용하여 ```gradle -v```커맨드를 입력하여 제대로 설정되었는지 확인한다.

### Unix기반 시스템(Mac OS, Linux...) 설치
1. [SDKMAN](https://sdkman.io/) 을 사용하여 ```sdk install gradle```명령어로 설치하거나 Mac OS의 경우 [Homebrew](https://brew.sh/) 를 사용하여 ```brew install gradle``` 명령어로도 설치 가능하다.
2. ```gradle -v``` 커맨드로 설정을 확인하고 설정이 되어있지 않다면 설정해준다.(패키지 메니저를 사용하지 않고 설치 3번 참고)

### 패키지 메니저를 사용하지 않고 설치
1. [Gradle 다운로드](https://gradle.org/releases/) 에서 원하는 버전을 다운로드 받는다.
2. [Gradle Install User Guide](https://docs.gradle.org/7.0/userguide/installation.html) 에 따라 아래 명령어를 차례대로 입력한다.
   ```
   > mkdir /opt/gradle
   > unzip -d /opt/gradle gradle-7.0-bin.zip
   > ls /opt/gradle/gradle-7.0
   ```
3. 다운 받은 Gradle을 환경변수에 설정해준다.
    1. ```.bash_profile```또는 ```.profile```을 ```vi ~/.bash_profile``` 명령어로 열어준다.
    2. 아래 환경변수를 추가 및 저장한다.
       ```
       export GRADLE_HOME="/opt/gradle/gralde-7.0"
       export PATH=$PATH:$GRADLE_HOME/bin
       ```
    3. 추가한 환경변수를 ```source ~/.bash_profile```명령어를 통해 적용해준다.
    4. 환경변수가 잘 적용되었는지 ```gradle -v```명령어로 확인한다.
    
### ref
- [Gradle 공식 설치 페이지](https://gradle.org/releases/)