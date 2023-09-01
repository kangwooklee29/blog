GCP VM 인스턴스에서 Flask 서버를 gunicorn과 nginx로 실행 중이라면, Let's Encrypt의 certbot을 사용하여 무료로 SSL/TLS 인증서를 받아 HTTPS로 전환할 수 있습니다. 다음은 간단한 절차입니다:

1. **certbot 설치**:
    ```bash
    sudo apt update
    sudo apt install software-properties-common
    sudo add-apt-repository universe
    sudo add-apt-repository ppa:certbot/certbot
    sudo apt update
    sudo apt install certbot python3-certbot-nginx
    ```

2. **인증서 받기**: 
    ```bash
    sudo certbot --nginx
    ```
    위의 명령을 실행하면 certbot이 실행되어 도메인을 묻습니다. 이후에 도메인에 대한 검증 절차를 진행하고, 검증이 완료되면 자동으로 nginx 설정을 업데이트합니다.

3. **자동 갱신 설정**:
    Let's Encrypt의 인증서는 90일마다 갱신되어야 합니다. 자동 갱신을 설정하기 위해 다음 명령을 사용하면 됩니다:
    ```bash
    sudo certbot renew --dry-run
    ```

4. **nginx 설정 확인**:
    `/etc/nginx/sites-available` 또는 `/etc/nginx/conf.d`에 있는 설정 파일을 열어보고 SSL 관련 설정이 제대로 추가되었는지 확인합니다.

5. **nginx 재시작**:
    모든 설정 후 nginx를 재시작하여 변경사항을 적용합니다:
    ```bash
    sudo systemctl restart nginx
    ```

6. **방화벽 설정**:
    GCP의 VM 인스턴스에서 방화벽 설정을 통해 443 포트(HTTPS)가 허용되어 있는지 확인하세요. 만약 아직 설정되지 않았다면, 해당 포트를 열어 주세요.

위의 절차를 따르면, Flask 애플리케이션을 HTTPS를 사용하여 제공할 수 있게 됩니다. 
