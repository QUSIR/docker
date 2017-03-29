 vim Dockerfile 


	FROM ubuntu:14.04
	MAINTAINER examples@docker.com
	RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
	
	RUN apt-get update
	RUN apt-get upgrade -y
	
	RUN apt-get install -y --force-yes perl-base
	RUN apt-get install -y apache2.2-common
	
	RUN apt-get install -y openssh-server apache2 supervisor
	RUN mkdir -p /var/run/sshd
	RUN mkdir -p /var/log/supervisor
	COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
	EXPOSE 22 80
	CMD ["/usr/bin/supervisord"]
	COPY sources.list /etc/apt/sources.list
	RUN apt-get update
	
	#设置密码
	RUN echo 'root:ubuntu' | chpasswd



docker build -t test/supervisord .


docker run -p 2222:22 -p 8080:80 -t -i test/supervisord