# 서버 보안 지침에 따른 패스워드 정책

- 서버 보안 지침에 명시된 패스워드 관련 정책을 준수하기 위해서 계정을 관리해야 합니다. 
- 다음은 지침의 주요 내용입니다:

## 패스워드 정책

- **최대 사용 기간**: 90일
- **패스워드 조합 규칙**: 대문자, 소문자, 숫자, 특수문자 중 3가지 이상 조합
- **최소 길이**: 8자리 이상


## 현재 설정

현재 계정 관리에서는 최소 8자리 이상 비밀번호 정책만 적용되어 있습니다. 

나머지 조건인 최대 90일 사용 및 패스워드 조합 규칙에 대한 설정은 추가적으로 적용해야 합니다.

이러한 패스워드 정책을 준수하여 보안을 강화하고, 서버를 안전하게 운영하기 위해 노력해야 합니다.


# PAM(Pluggable Authentication Modules) 사용하여 비밀번호 8자리 이상 규제

**-> 리눅스 서버 새로 생성 후 작업**

비밀번호 강도와 품질을 보장하기 위한 `pam_pwquality` 모듈의 설정입니다.





## 리눅스 환경
**ubuntu**

<img src="https://github.com/tandpfun/skill-icons/blob/main/icons/Ubuntu-Dark.svg" alt="Ubuntu Icon" width="100" />


## NAT 네트워크 포트포워딩 규칙 추가

![NAT 포트포워딩 설정](https://github.com/user-attachments/assets/38d36bb4-9093-46e0-bd0c-30db02be74fd)


![포트포워딩 추가](https://github.com/user-attachments/assets/8b2be45f-bd5a-44bf-bec4-a01e35c6e7ad)

## 고정 IP 설정

![고정 IP 설정](https://github.com/user-attachments/assets/1d282177-96e7-42bc-bee3-4a4b1d66710e)


- 경로상 yaml 파일 있는지 확인

    ```bash
    ls /etc/netplan
    ```

- 디렉토리 구조 확인

    ```bash
    tree /etc/netplan
    ```

- yaml 파일 내용 확인

    ```bash
    cat /etc/netplan/00-installer-config.yaml
    ```

![yaml 파일 확인](https://github.com/user-attachments/assets/6ef40bce-7d8f-46f8-a0a0-8bb038001e6e)


## yaml 파일 수정

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      addresses:
        - 10.0.2.21/24  # 변경된 고정 IP 주소
      routes:
        - to: default
          via: 10.0.2.1  # 게이트웨이
      nameservers:
        addresses:
          - 8.8.8.8
      dhcp4: false
```

## yaml 파일 적용

```bash
sudo netplan apply
```

## 모듈 설치 → libpam-pwquality

![image5](https://github.com/user-attachments/assets/884a40b8-eb33-4794-a6f1-26cb01752810)

## /etc/pam.d/common-password 에 추가

```bash
password        requisite                       pam_pwquality.so  minlen=8 
```

![image6](https://github.com/user-attachments/assets/c866c6b0-b86a-4ac1-bc79-ee74a1148739)

## 8글자보다 작은 비밀번호 입력시 결과

![image7](https://github.com/user-attachments/assets/772dcf6d-811b-425b-9c24-17baa5fd88a5)
