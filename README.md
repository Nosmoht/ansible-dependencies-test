Repository to demonstarte role dependency bug in Ansible 2.0.1.0

Current result
```shell
$ ansible-playbook play.yml

PLAY ***************************************************************************

TASK [setup] *******************************************************************
ok: [127.0.0.1]

TASK [db : db task] ************************************************************
ok: [127.0.0.1] => {
    "msg": "task of db"
}

TASK [service1 : service1 task] ************************************************
ok: [127.0.0.1] => {
    "msg": "Task of service 1"
}

TASK [service2 : service2 tasks] ***********************************************
ok: [127.0.0.1] => {
    "msg": "task of service 2"
}

TASK [node-role : node role] ***************************************************
ok: [127.0.0.1] => {
    "msg": "Task of node-role"
}

PLAY RECAP *********************************************************************
127.0.0.1                  : ok=5    changed=0    unreachable=0    failed=0   
```

Expected result
```shell
$ ansible-playbook play.yml
 [WARNING]: provided hosts list is empty, only localhost is available


PLAY ***************************************************************************

TASK [setup] *******************************************************************
ok: [127.0.0.1]

TASK [db : db task] ************************************************************
ok: [127.0.0.1] => {
    "msg": "task of db"
}

TASK [service1 : service1 task] ************************************************
ok: [127.0.0.1] => {
    "msg": "Task of service 1"
}

TASK [db : db task] ************************************************************
ok: [127.0.0.1] => {
    "msg": "task of db"
}

TASK [service2 : service2 tasks] ***********************************************
ok: [127.0.0.1] => {
    "msg": "task of service 2"
}

TASK [node-role : node role] ***************************************************
ok: [127.0.0.1] => {
    "msg": "Task of node-role"
}

PLAY RECAP *********************************************************************
127.0.0.1                  : ok=5    changed=0    unreachable=0    failed=0   
```

Note: ansible-playbook --list-tasks shows the expected result
```shell
$ ansible-playbook play.yml --list-tasks

playbook: play.yml

  play #1 (127.0.0.1): 	TAGS: []
    tasks:
      db : db task	TAGS: []
      service1 : service1 task	TAGS: []
      db : db task	TAGS: []
      service2 : service2 tasks	TAGS: []
      node-role : node role	TAGS: []
```
