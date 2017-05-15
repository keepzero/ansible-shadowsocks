# Ansible

分享一下我在白山使用 Ansible 的心得和技巧

- Ansible 基本概念
- inventory
- 动态 inventory
- ansible.cfg
- playbook 实战
  1. 一键部署 shadowsocks 科学上网 (with docker)
  2. 一键部署 shadowsocks -> 兼容不同的 Linux 发行版
  3. 一键部署 shadowsocks -> 修改成 roles 结构
- playbook 实用命令

## Ansible 基本概念

- Inventory: 资产
- Module: 模块，大部分具有幂等性
- Ad-Hoc Command: 执行一个模块或者命令
- Playbook: 多个 Ad-Hoc 命令组成一个 playbook 脚本
- Plugins: 插件，大部分 ansible 的功能都是插件式的

## Inventory

### 静态 inventory

```
localhost ansible_connection=local

[db]
db-master.example.com
db-slave.example.com

[cache]
redis.example.com

[loadbalancer]
front.example.com

[app]
app-1.example.com
app-2.example.com
front.example.com

[webservers:children]
app
loadbalancer
```

### 动态 inventory

- 对接 CMDB（资源平台）
- 任意编程语言
- 可以结合静态 inventory

## ansible.cfg

- forks=16
- inventory=/path/to/your/inventory
- remote_user=ubuntu
- remote_port=2222
- become=True # 不建议

## 实战：一键部署科学上网

- docker
- shadowsocks-libev

演示：

1. 运行 playbook 部署 shadowsocks
2. 修改 playbook 兼容 CentOS
3. 拆分 playbook 改成 roles
4. 将 playbook 写得更漂亮

## Playbook 实用技巧

### roles

复用 roles: docker

### vars

- host_vars
- group_vars
- vars_files
- and so on

### serial

```
serial: 
  - 1
  - 10
  - 10%
```

### Loop

```
yum: name={{ item }} state=present
with_items:
  - python-pip
  - python-requests
```

### strategy

`strategy: free`

### delegate

- `delegate_to: localhost`
- `local_action: `

### block

```
- block:
    - shell: hostname
    - shell: uname -a
  when: xxxx
```

### async

```
- shell: 'sleep $(expr $RANDOM / 100)'
  async: 100
  poll: 5
```

### tags

### Conditional

### limit

### register

### debug

## 插件

- action plugins
  - modules
- callback plugins
- filter plugins

## playbook 最佳实践

- 官网文档：[ansible docs](http://docs.ansible.com)
- 推荐阅读：[playbooks best practices](http://docs.ansible.com/ansible/playbooks_best_practices.html)
- Playbook 关键字：[playbooks keywords](http://docs.ansible.com/ansible/playbooks_keywords.html)
- 模块索引：[list of all modules](http://http://docs.ansible.com/ansible/list_of_all_modules.html)

## VPS

[购买 vultr vps](http://www.vultr.com/?ref=7154533)
