# Centos 6 GitLab install

> https://about.gitlab.com/downloads/#centos6


```
sudo yum install curl openssh-server openssh-clients postfix cronie
```

- 메일 서비스
```
sudo service postfix start
```

- 자동실행
```
sudo chkconfig postfix on
```

- 방화벽 오픈
```
sudo lokkit -s http -s ssh
```

- 설치
```
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
sudo apt-get install gitlab-ce
```

- 설정
```
$ /etc/gitlab/gitlab.rb
```

- 설정 적용
```
$ gitlab-ctl reconfigure
```

- 그 외 명령어 확인
```
$ gitlab-ctl
```
