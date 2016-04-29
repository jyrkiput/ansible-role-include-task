# ansible-role-include-task

Files aren't loaded from role/{templates, files} with Ansible 2.0 version.

With ansible 1.9.x
```
ansible-playbook demo.yml -i hosts                                                                                                                      [±master ●●▴]

PLAY [localhost] **************************************************************

GATHERING FACTS ***************************************************************
ok: [localhost]

TASK: [role | create file] ****************************************************
ok: [localhost]

TASK: [role | create file with relative path] *********************************
ok: [localhost]

TASK: [role | create file with role path and relative] ************************
ok: [localhost]

TASK: [role | create file with role path] *************************************
ok: [localhost]

TASK: [create file] ***********************************************************
ok: [localhost]

TASK: [create file with relative path] ****************************************
ok: [localhost]

TASK: [create file with role path and relative] *******************************
skipping: [localhost]

TASK: [create file with role path] ********************************************
skipping: [localhost]

PLAY [localhost] **************************************************************

GATHERING FACTS ***************************************************************
ok: [localhost]

TASK: [create file] ***********************************************************
ok: [localhost]

TASK: [create file with relative path] ****************************************
ok: [localhost]

TASK: [create file with role path and relative] *******************************
skipping: [localhost]

TASK: [create file with role path] ********************************************
skipping: [localhost]

PLAY [localhost] **************************************************************

GATHERING FACTS ***************************************************************
ok: [localhost]

TASK: [another_role | create file] ********************************************
ok: [localhost]

TASK: [another_role | create file with relative path] *************************
ok: [localhost]

TASK: [another_role | create file with role path and relative] ****************
ok: [localhost]

TASK: [another_role | create file with role path] *****************************
fatal: [localhost] => input file not found at /home/jyrki/projects/ansible-roles/role-include-issue/roles/another_role/files/file or /home/jyrki/projects/ansible-roles/role-include-issue/roles/another_role/files/file

FATAL: all hosts have already failed -- aborting

PLAY RECAP ********************************************************************
           to retry, use: --limit @/home/jyrki/demo.retry

localhost                  : ok=14   changed=0    unreachable=1    failed=0  
```

With Ansible 2.0

```
ansible-playbook demo.yml -i hosts

PLAY [localhost] ***************************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [role : include] **********************************************************
included: /home/jyrki/projects/ansible-roles/role-include-issue/roles/role/tasks/copy.yml for localhost

TASK [role : create file] ******************************************************
ok: [localhost]

TASK [role : create file with relative path] ***********************************
ok: [localhost]

TASK [role : include] **********************************************************
included: /home/jyrki/projects/ansible-roles/role-include-issue/roles/role/tasks/copy_with_role_path.yml for localhost

TASK [role : create file with role path and relative] **************************
ok: [localhost]

TASK [role : create file with role path] ***************************************
ok: [localhost]

TASK [include] *****************************************************************
included: /home/jyrki/projects/ansible-roles/role-include-issue/roles/role/tasks/copy.yml for localhost

TASK [create file] *************************************************************
fatal: [localhost]: FAILED! => {"changed": false, "failed": true, "msg": "could not find src=/home/jyrki/projects/ansible-roles/role-include-issue/file"}
...ignoring

TASK [create file with relative path] ******************************************
fatal: [localhost]: FAILED! => {"changed": false, "failed": true, "msg": "could not find src=/home/jyrki/projects/ansible-roles/files/file"}
...ignoring

TASK [include] *****************************************************************
included: /home/jyrki/projects/ansible-roles/role-include-issue/roles/role/tasks/copy_with_role_path.yml for localhost

TASK [create file with role path and relative] *********************************
fatal: [localhost]: FAILED! => {"failed": true, "msg": "'role_path' is undefined"}
...ignoring

TASK [create file with role path] **********************************************
fatal: [localhost]: FAILED! => {"failed": true, "msg": "'role_path' is undefined"}
...ignoring

PLAY [localhost] ***************************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [include] *****************************************************************
included: /home/jyrki/projects/ansible-roles/role-include-issue/roles/role/tasks/copy.yml for localhost

TASK [create file] *************************************************************
fatal: [localhost]: FAILED! => {"changed": false, "failed": true, "msg": "could not find src=/home/jyrki/projects/ansible-roles/role-include-issue/file"}
...ignoring

TASK [create file with relative path] ******************************************
fatal: [localhost]: FAILED! => {"changed": false, "failed": true, "msg": "could not find src=/home/jyrki/projects/ansible-roles/files/file"}
...ignoring

TASK [include] *****************************************************************
included: /home/jyrki/projects/ansible-roles/role-include-issue/roles/role/tasks/copy_with_role_path.yml for localhost

TASK [create file with role path and relative] *********************************
fatal: [localhost]: FAILED! => {"failed": true, "msg": "'role_path' is undefined"}
...ignoring

TASK [create file with role path] **********************************************
fatal: [localhost]: FAILED! => {"failed": true, "msg": "'role_path' is undefined"}
...ignoring

PLAY [localhost] ***************************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [another_role : include] **************************************************
included: /home/jyrki/projects/ansible-roles/role-include-issue/roles/another_role/tasks/../../role/tasks/copy.yml for localhost

TASK [another_role : create file] **********************************************
fatal: [localhost]: FAILED! => {"changed": false, "failed": true, "msg": "could not find src=/home/jyrki/projects/ansible-roles/role-include-issue/file"}
...ignoring

TASK [another_role : create file with relative path] ***************************
fatal: [localhost]: FAILED! => {"changed": false, "failed": true, "msg": "could not find src=/home/jyrki/projects/ansible-roles/files/file"}
...ignoring

TASK [another_role : include] **************************************************
included: /home/jyrki/projects/ansible-roles/role-include-issue/roles/another_role/tasks/../../role/tasks/copy_with_role_path.yml for localhost

TASK [another_role : create file with role path and relative] ******************
ok: [localhost]

TASK [another_role : create file with role path] *******************************
fatal: [localhost]: FAILED! => {"changed": false, "failed": true, "msg": "could not find src=/home/jyrki/projects/ansible-roles/role-include-issue/roles/another_role/files/file"}
...ignoring

PLAY RECAP *********************************************************************
localhost                  : ok=27   changed=0    unreachable=0    failed=0
```
