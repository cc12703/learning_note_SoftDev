


# Ansible语法




## PlayBook

### 结构
分为三部分：
* 在什么机器上以什么身份执行
* 执行的任务是什么
* 善后的任务是什么


#### 示例
```yaml
hosts: webservers
  vars:
    http_port: 80
    max_clients: 200
  remote_user: root
  tasks:
    - name: ensure apache is at the latest version
      yum: pkg=httpd state=latest
    - name: write the apache config file
      template: src=/srv/httpd.j2 dest=/etc/httpd.conf
      notify:
          - restart apache
    - name: ensure apache is running
      service: name=httpd state=started
  handlers:
    - name: restart apache
      service: name=httpd state=restarted
```

### 概述
* `tasks` 定义任务，调用一次指定的模块
* `handlers` 定义响应，由`notify`触发，调用一次指定的模块
* `vars` 定义变量，使用`{{ }}`引用


### 主机
* `hosts` 主机信息

### 用户
* `remote_user` 远程执行时的用户
* `become` 是否切换成其他用户
* `become_method` 切换用的方法
    * `sudo`,`su`
* `become_user` 切换成的用户
    * `root`


### 任务
* 从上到下顺序执行
* 每个任务都是对模块的一次调用
* 使用`KV对`、`字典`传参数
* 参数太长可以分隔到多行上


#### 示例
```yaml
tasks:
    - name: xxx
      service: name=httpd state=running

    - name: xxx
      copy:
          src: xxx
          dest: xxx
          owner: root
          group: root

```


### 响应
* 每个响应也是对模块的一次调用
* 每个响应最多执行一次
* 响应是按照定义的顺序执行的
* 当任务的执行状态为`changed`时，才会执行响应


#### 示例
```yaml
tasks
    - name: xxx
      copy: xxx
      notify:
        - <handle-name>

handlers
    - name: xxx
      debug: msg="xxx"
```


### 变量
* `vars_files` 引用变量文件