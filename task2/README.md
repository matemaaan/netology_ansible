# Домашнее задание к занятию "08.02 Работа с Playbook"

## Подготовка к выполнению
1. Создайте свой собственный (или используйте старый) публичный репозиторий на github с произвольным именем.
2. Скачайте [playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.
3. Подготовьте хосты в соотвтествии с группами из предподготовленного playbook. 
4. Скачайте дистрибутив [java](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html) и положите его в директорию `playbook/files/`. 

## Основная часть
1. Приготовьте свой собственный inventory файл `prod.yml`.
2. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает kibana.
3. При создании tasks рекомендую использовать модули: `get_url`, `template`, `unarchive`, `file`.
4. Tasks должны: скачать нужной версии дистрибутив, выполнить распаковку в выбранную директорию, сгенерировать конфигурацию с параметрами.
5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
6. Попробуйте запустить playbook на этом окружении с флагом `--check`.
7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.
8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.
9. Подготовьте README.md файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.

``` 
Playbook:
 play1:
   создает fact для java home
   загружает на клиента архив с java
   создает директорию для java
   разархивирует туда java
   из template устанавливает переменные окружения
 play2:
   скачивает архив с elasticsearch (закоментировано, т.к. через впн долго загружает и в общем и целом требуется 1 раз для получения файла, а т.к. файл скачан, то можно пропускать данный пункт)
   загружает на клиента архив с elasticsearch
   создает директорию для elasticsearch
   разархивирует туда elasticsearch
   из template устанавливает переменные окружения
 play3:
   скачивает архив с kibana (закоментировано, т.к. через впн долго загружает и в общем и целом требуется 1 раз для получения файла, а т.к. файл скачан, то можно пропускать данный пункт)
   загружает на клиента архив с kibana
   создает директорию для kibana
   разархивирует туда kibana
   из template устанавливает переменные окружения

Теги которые есть: [java] [elastic] [kibana]

```  
```
PLAY [Install Java] ****************************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************
ok: [ubuntu]

TASK [Set facts for Java 11 vars] **************************************************************************************************************************************************
ok: [ubuntu]

TASK [Upload .tar.gz file containing binaries from local storage] ******************************************************************************************************************
ok: [ubuntu]

TASK [Ensure installation dir exists] **********************************************************************************************************************************************
ok: [ubuntu]

TASK [Extract java in the installation directory] **********************************************************************************************************************************
skipping: [ubuntu]

TASK [Export environment variables] ************************************************************************************************************************************************
ok: [ubuntu]

PLAY [Install Elasticsearch] *******************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************
ok: [ubuntu]

TASK [Upload .tar.gz file containing binaries from local storage] ******************************************************************************************************************
ok: [ubuntu]

TASK [Create directrory for Elasticsearch] *****************************************************************************************************************************************
ok: [ubuntu]

TASK [Extract Elasticsearch in the installation directory] *************************************************************************************************************************
skipping: [ubuntu]

TASK [Set environment Elastic] *****************************************************************************************************************************************************
ok: [ubuntu]

PLAY [Install Kibana] **************************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************
ok: [ubuntu]

TASK [Upload .tar.gz file containing binaries from local storage] ******************************************************************************************************************
ok: [ubuntu]

TASK [Create directrory for Kibana] ************************************************************************************************************************************************
ok: [ubuntu]

TASK [Extract Kibana in the installation directory] ********************************************************************************************************************************
skipping: [ubuntu]

TASK [Set environment Kibana] ******************************************************************************************************************************************************
ok: [ubuntu]

PLAY RECAP *************************************************************************************************************************************************************************
ubuntu                     : ok=13   changed=0    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0   

user@ubuntu:~/tmp/netology_ansible/task2$ ansible-playbook -i inventory/prod.yml site.yml --diff -K
BECOME password: 

PLAY [Install Java] ****************************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************
ok: [ubuntu]

TASK [Set facts for Java 11 vars] **************************************************************************************************************************************************
ok: [ubuntu]

TASK [Upload .tar.gz file containing binaries from local storage] ******************************************************************************************************************
ok: [ubuntu]

TASK [Ensure installation dir exists] **********************************************************************************************************************************************
ok: [ubuntu]

TASK [Extract java in the installation directory] **********************************************************************************************************************************
skipping: [ubuntu]

TASK [Export environment variables] ************************************************************************************************************************************************
ok: [ubuntu]

PLAY [Install Elasticsearch] *******************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************
ok: [ubuntu]

TASK [Upload .tar.gz file containing binaries from local storage] ******************************************************************************************************************
ok: [ubuntu]

TASK [Create directrory for Elasticsearch] *****************************************************************************************************************************************
ok: [ubuntu]

TASK [Extract Elasticsearch in the installation directory] *************************************************************************************************************************
skipping: [ubuntu]

TASK [Set environment Elastic] *****************************************************************************************************************************************************
ok: [ubuntu]

PLAY [Install Kibana] **************************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************
ok: [ubuntu]

TASK [Upload .tar.gz file containing binaries from local storage] ******************************************************************************************************************
ok: [ubuntu]

TASK [Create directrory for Kibana] ************************************************************************************************************************************************
ok: [ubuntu]

TASK [Extract Kibana in the installation directory] ********************************************************************************************************************************
skipping: [ubuntu]

TASK [Set environment Kibana] ******************************************************************************************************************************************************
ok: [ubuntu]

PLAY RECAP *************************************************************************************************************************************************************************
ubuntu                     : ok=13   changed=0    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0   

```  

10. Готовый playbook выложите в свой репозиторий, в ответ предоставьте ссылку на него.

## Необязательная часть

1. Приготовьте дополнительный хост для установки logstash.
2. Пропишите данный хост в `prod.yml` в новую группу `logstash`.
3. Дополните playbook ещё одним play, который будет исполнять установку logstash только на выделенный для него хост.
4. Все переменные для нового play определите в отдельный файл `group_vars/logstash/vars.yml`.
5. Logstash конфиг должен конфигурироваться в части ссылки на elasticsearch (можно взять, например его IP из facts или определить через vars).
6. Дополните README.md, протестируйте playbook, выложите новую версию в github. В ответ предоставьте ссылку на репозиторий.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---