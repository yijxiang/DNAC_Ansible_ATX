## Ansible_ATX

主要是演示Cisco DNA center 与 Ansible互操作。

### Python、Ansible、Git 准备工作

- 在部署ansible之前，请首先完成python的安装，具体方法请参考 [ python on ubuntu ](https://github.com/yijxiang/python-on-ubuntu)
- 创建、并进入Python 虚拟环境 [ Python Virtual Env. ]( https://github.com/yijxiang/python-on-ubuntu/blob/main/Python%20%E8%99%9A%E6%8B%9F%E7%8E%AF%E5%A2%83%20-%20venv.md)
- Ansible安装请参考 [ dnac_ansible 使用和安装方法 ](https://github.com/yijxiang/python-on-ubuntu/blob/main/dnac_ansible%20%E4%BD%BF%E7%94%A8%E5%92%8C%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.md)
- 本地安装好*GIT* 软件，并可以使用 git push 到 remote github server (Gitlab类似)

Git remote repo 建议使用**SSH**模式下载和更新上传，利用预先设置 SSH key 可以很方便实现远程repo的同步操作。


### ATX demo repo 使用方法

- Fork 到自己的 github 中；
- 在本地电脑的项目目录下，运行 **git clone** 复制项目到本地

  git clone *git@github.com:yijxiang/DNAC_Ansible_ATX.git* : git clone **url-of-your-fork-repo** 请修改为自己fork来的链接。因为main.yml 中将执行*git push*操作，需要对remote repo 有**读写**权限。

进入到 *git clone*过程新创建的目录下。


### 目录、文件说明

文件、目录名称 | 描述
------------ | -------------
README.md |   本说明文件
api_payload_new_site.json |  DNAC 中使用*Platform-> API-> Try it*使用到的payload，用于新创建area site
hosts |  定义DNAC主机名字，不需要修改
credential_sample.yml |  变量文件，用于定义DNAC访问方法，以其创建新文件**credentials.yml**，并修改其内容
create_site.yml |  playbook：用于新创建area 
network_device_info.yml |  playbook：用于获取设备资产信息，例子中有hostname、uuid等信息
config.yml |  playbook：用于获取设备配置信息，输入信息有 uuid
site_health.yml |  playbook：用于获取site health，Assurance 使用场景
site_info.yml |  playbook：用于获取site list，适用于后续需要用到site id等
tag_info.yml |  playbook：用于获取tag list
main.yml |  playbook：用于抓取设备配置，并保存到git repo中，可以同步到github
backup |  本地设备配置保存目录


### Ansible 配置

密码、用户名等配置请修改文件 *credential_sample_.yml* 并更改文件名为 -> *credentials.yml* ，该名称将在后续playbook中调用，建议在测试环境中测试使用 ansible playbook，之后在生产环境中实践。

如果密码敏感，则推荐使用Ansible vault。


### Ansible Playbook 命令使用方法

ansible-playbook -i hosts main.yml

其中 *main.yml* 文件替换为其他*playbook.yml*即可进行测试。

ATX DEMO 中演示的 main.yml playbook 实现几个任务：
- 从DNAC中获取所有其平台为交换机的资产信息；
- 保存交换机hostname、uuid 至变量；
- 从DNAC中获取指定交换机的配置；
- 并保存该交换机的配置到backup目录；
- 如果有变化，则通过GIT 更新同步文件至remote github；


### 参考链接


- [Ansible Modules for Cisco DNA Center ](https://galaxy.ansible.com/cisco/dnac)
- [ Github 源代码 ](https://github.com/cisco-en-programmability/dnacenter-ansible)
- [ DNA Center Ansible documentation ](https://cisco-en-programmability.github.io/dnacenter-ansible/main/index.html)
- [ Ansible Modules for DNA Center 开发者指南 ](https://developer.cisco.com/docs/dna-center/?utm_campaign=dnac-padm&utm_source=padm-ww&utm_medium=dnac-blog-docs#!ansible)
- [ Config backup with Ansible + Git ](https://nwmichl.net/2020/03/09/configbackup-with-ansible-git/)

