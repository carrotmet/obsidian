## 配置
- yum -y install epel-release
- yum -y install ansible

- /etc/ansble/hosts/   操控的主机位置（主机清单）
## 点对点
- 配置免密登录
	- 生成密钥对  ssh-keygen -t rsa
	- ssh-copy-id -i /root/.ssh/id_rsa.pub root@192.168.2.132

- 也可以直接在host对应集群中把ip user 密码都加上


## 剧本——远端下载httpd

- ansible-playbook install_yaml
- [ansible使用-playbook剧本+roles角色模式-案例实战(一)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1oX4y1z7Kr?p=9&vd_source=dfa876815ff4fa974172466e667cfd18)

- vars、task 、handlers 定义变量、执行的任务、钩子

## 带role的playbook

- 有些yaml的配置是**一次性的**，有些我们不想重复执行
- 引入结构化、层次化组织的playbook——role角色
- 本质是对大yaml的拆分
![[Pasted image 20240702193638.png]]

 - 标准的role-playbook目录

- ansible-playbook-roles/
	- host
		- hosts   # 和/etc/ansible/hosts 一样
	- **roles**
		- role1-httpd
			- **vars**  # 定义标准变量，调用本机的各种文件等
			- **files**  # 所有在yml中配置所需要的文件，远程传输需要的
			- **handlers** # 钩子yml文件：比如触发服务重启yml可以放在这
			- **tasks**   # 角色部署的任务列表（main.yml调整任务顺序，其他yaml定义任务）
				- **main.yml**  # 控制顺序的yml
			- **templetes**   # 模版文件——html等
			- **default**  # 定义角色默认变量，比vars优先级低，一般不用
			- **meta**   # 角色定义的元数据，一般不用
	- playbook-all-roles.yml