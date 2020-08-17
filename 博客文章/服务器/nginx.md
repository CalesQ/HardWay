1. 配置跨域/转发

		location ~ /api/ {
		            proxy_set_header Host $host;
		            proxy_pass  http://ip:port;  #请求转向 ip 定义的服务器列表
		            proxy_set_header X-Real-IP $remote_addr;
		            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;#在nginx 作为代理服务器时，设置的IP列表，会把经过的机器ip，代理机器ip都记录下来，用 【，】隔开；代码中用 echo $x-forwarded-for |awk -F, '{print $1}' 来作为源IP
		        }