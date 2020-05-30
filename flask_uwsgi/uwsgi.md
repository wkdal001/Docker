# WSGI
> WSGI는 Web Server Gateway Interface의 약어로 웹서버와 웹어플리케이션이 어떤 방식으로 통신하는가에 관한 인터페이스를 의미한다. 웹서버와 웹어플리케이션 간의 소통을 정의해 어플리케이션과 서버가 독립적으로 운영될 수 있게 돕는다. 
  
$ pip install uwsgi   
$ uwsgi --ini uwsgi.ini // 실행하기  
  

```uwsgi
uwsgi.ini

[uwsgi]
# this config will be loaded if nothing specific is specified
# load base config from below
ini = :base

# %d is the dir this configuration file is in
socket = %dapp.sock
master = true
processes = 4

[dev]
ini = :base
# socket (uwsgi) is not the same as http, nor http-socket
socket = :8001


[local]
ini = :base
http = :8000
# set the virtual env to use
# home=/Users/you/envs/env
home=/root/.pyenv/versions/app


[base]
# chdir to the folder of this config file, plus app/website
chdir = %dapp/
# load the module from wsgi.py, it is a python path from 
# the directory above.
module=wsgi
# allow anyone to connect to the socket. This is very permissive
chmod-socket=666

logto = /var/log/uwsgi/app/@(exec://date +%%Y-%%m-%%d).log

```

* chdir : 프로젝트 기본 경로입니다. django의 경우 manage.py가 위치는 곳을 경로로 잡아주면 됩니다.
* module : django 프로젝트를 생성하면 해당 프로젝트 이름으로 생성되는 wsgi.py가 생성되는데 해당 파일을 모듈로 불러옵니다. 프로젝트이름.wsgi로 해주시면됩니다.
* home, virtualenv : 파이썬 가상환경이 위치한 폴더의 경로를 잡아주면 됩니다.
* master : uWSGI 프로세스를 master로 돌아가게 해줍니다.
* processes : 프로세스 수 입니다.
* socket : UNIX socket 파일의 위치입니다. socket file의 위치를 잡아줘도 되며 localhost와 port를 명시해줘도 됩니다.
* chmod-socket : UNIX socket에 대한 권한 설정입니다. 666을 해야 실행이 가능합니다.
* vacuum : uWSGI를 통해서 생성된 파일들은 삭제하는 옵션입니다.
* daemonize : 백그라운드로 돌리기 위한 설정이며 log파일을 남길 경로를 지정해주면 됩니다.
* pidfile : 생성할 pid 파일의 위치입니다.
* enable-threads : thread 사용을 앱(uWSGI) 내에서 가능하게 해줍니다.
* single-interpreter : 단일한 python interpreter를 사용하게 하는 옵션입니다.
* lazy-apps : master말고 각각의 worker에(master에서 spawn한 자식들) 앱을 로드하는 설정입니다.
