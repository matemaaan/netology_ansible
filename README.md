    Попробуйте запустить playbook на окружении из test.yml, зафиксируйте какое значение имеет факт some_fact для указанного хоста при выполнении playbook'a.  
``` 12 ```  

    Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.  
``` group_vars/all/examp.yml ```  

    Воспользуйтесь подготовленным (используется docker) или создайте собственное окружение для проведения дальнейших испытаний.
``` docker run -d --name=ubuntu python:3.8-slim-buster sleep 600 ```  
``` docker run -d --name=centos7 python:3.8-slim-buster sleep 600 ```  

    Проведите запуск playbook на окружении из prod.yml. Зафиксируйте полученные значения some_fact для каждого из managed host.  

```  
TASK [Print OS] *******************************************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "Debian"
}
ok: [ubuntu] => {
    "msg": "Debian"
}

TASK [Print fact] *****************************************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}
``` 

    Добавьте факты в group_vars каждой из групп хостов так, чтобы для some_fact получились следующие значения: для deb - 'deb default fact', для el - 'el default fact'.
``` group_vars/deb/examp.yml ```  
``` group_vars/el/examp.yml ```  

    Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные значения для всех хостов.  

```  
TASK [Print OS] *******************************************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "Debian"
}
ok: [ubuntu] => {
    "msg": "Debian"
}

TASK [Print fact] *****************************************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
``` 

    При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.  

``` ansible-vault encrypt group_vars/deb/examp.yml ```  
``` ansible-vault encrypt group_vars/el/examp.yml ```  

    Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь в работоспособности.  

``` ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass ```  

    Посмотрите при помощи ansible-doc список плагинов для подключения. Выберите подходящий для работы на control node.  
    
``` local ```  

    В prod.yml добавьте новую группу хостов с именем local, в ней разместите localhost с необходимым типом подключения.  

```
---
  el:
    hosts:
      centos7:
        ansible_connection: docker
  deb:
    hosts:
      ubuntu:
        ansible_connection: docker
  local:
    hosts:
      localhost:
        ansible_host: localhost
        ansible_connection: local
```  

    Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь что факты some_fact для каждого из хостов определены из верных group_vars.  

```  
TASK [Print OS] *******************************************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}
ok: [centos7] => {
    "msg": "Debian"
}
ok: [ubuntu] => {
    "msg": "Debian"
}

TASK [Print fact] *****************************************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

```  

    Заполните README.md ответами на вопросы. Сделайте git push в ветку master. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым playbook и заполненным README.md.
