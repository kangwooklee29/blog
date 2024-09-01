## Java SE(Standard Edition)와 그 구현체

- Java를 사용한 프로그램 개발에 필수적인 도구와 라이브러리 API를 정의하는 표준입니다. 오픈 소스 프로젝트인 OpenJDK 커뮤니티와 협력하여 Oracle에 의해 버전이 관리되고 발표됩니다.
- 공개된 Java SE 버전에 대한 구현체를 JDK(Java Development Kit)라고 하며, 이는 각각의 버전에 대해 별도로 개발되고 관리됩니다.

  - OpenJDK 프로젝트에서는 모든 Java SE에 대한 오픈소스 구현체인 OpenJDK를 발표하고 있습니다. 
  
  - Oracle은 OpenJDK에 기반하여 별도의 추가 기능을 구현한 Oracle JDK를 독자적으로 개발 및 관리하고 있습니다.


## 일반적인 JDK의 구성요소

- Java Compiler (javac): Java 소스 코드를 바이트 코드로 컴파일합니다.
- Java Archive (JAR) Tool: 클래스 파일 및 관련 리소스를 JAR 파일로 묶습니다.
- Java Debugger (jdb): 디버깅을 지원하는 도구입니다.
- Java Disassembler (javap): 클래스 파일을 분석하고 디스어셈블합니다.
- Java Runtime Environment(JRE): Java 애플리케이션을 실행하는 데 필요한 환경입니다. JRE는 다음과 같은 구성 요소로 이루어져 있습니다.
  - Java Virtual Machine (JVM): 바이트 코드를 실행하는 런타임 환경입니다.
  - Core Class Libraries: 기본 Java API 라이브러리.
  - Supporting Files: JVM과 라이브러리를 지원하는 구성 파일 등.
- Java Launcher(java): JRE와 결합하여 Java 애플리케이션을 실행하는 도구.