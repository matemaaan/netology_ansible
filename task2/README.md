# Домашнее задание к занятию "08.03 Использование Yandex Cloud"

## Подготовка к выполнению

1. (Необязательно) Познакомтесь с [lighthouse](https://youtu.be/ymlrNlaHzIY?t=929)
2. Подготовьте в Yandex Cloud три хоста: для `clickhouse`, для `vector` и для `lighthouse`.

Ссылка на репозиторий LightHouse: https://github.com/VKCOM/lighthouse

## Основная часть

1. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает lighthouse.
2. При создании tasks рекомендую использовать модули: `get_url`, `template`, `yum`, `apt`.
3. Tasks должны: скачать статику lighthouse, установить nginx или любой другой webserver, настроить его конфиг для открытия lighthouse, запустить webserver.
4. Приготовьте свой собственный inventory файл `prod.yml`.
5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
6. Попробуйте запустить playbook на этом окружении с флагом `--check`.
7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.
8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.
9. Подготовьте README.md файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.
10. Готовый playbook выложите в свой репозиторий, поставьте тег `08-ansible-03-yandex` на фиксирующий коммит, в ответ предоставьте ссылку на него.


```
playbook:
- проверяет наличие скачанных архивов
- если их нет, то качает
- создает каталоги для них на удаленном хосте
- копирует на удаленный хост
- распаковывает
- устанавливает
- запускает

да, знаю, что можно было через apt быстрее установить, но в текущих реалиях будет не лишним иметь архив с установщиком всего
```  

```
ansible-playbook -i inventory/prod.yml site.yml -K
BECOME password: 

PLAY [Upload] **********************************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************
ok: [ubuntu]

TASK [Check that the clickhouse-client exists] *************************************************************************************************************************************
ok: [ubuntu]

TASK [Check that the clickhouse-common-static exists] ******************************************************************************************************************************
ok: [ubuntu]

TASK [Check that the clickhouse-common-static-dbg exists] **************************************************************************************************************************
ok: [ubuntu]

TASK [Check that the clickhouse-server exists] *************************************************************************************************************************************
ok: [ubuntu]

TASK [Check that the vector exists] ************************************************************************************************************************************************
ok: [ubuntu]

TASK [Check that the lh exists] ****************************************************************************************************************************************************
ok: [ubuntu]

TASK [Upload tar.gz clickhouse-client from remote URL] *****************************************************************************************************************************
changed: [ubuntu]

TASK [Upload tar.gz clickhouse-common-static from remote URL] **********************************************************************************************************************
changed: [ubuntu]

TASK [Upload tar.gz clickhouse-common-static-dbg from remote URL] ******************************************************************************************************************
changed: [ubuntu]

TASK [Upload tar.gz clickhouse-server from remote URL] *****************************************************************************************************************************
changed: [ubuntu]

TASK [Upload tar.gz vector from remote URL] ****************************************************************************************************************************************
changed: [ubuntu]

TASK [Upload tar.gz vector from remote URL] ****************************************************************************************************************************************
changed: [ubuntu]

TASK [Upload client .tar.gz file containing binaries from local storage] ***********************************************************************************************************
skipping: [ubuntu]

TASK [Upload common .tar.gz file containing binaries from local storage] ***********************************************************************************************************
skipping: [ubuntu]

TASK [Upload common-dbg .tar.gz file containing binaries from local storage] *******************************************************************************************************
skipping: [ubuntu]

TASK [Upload server .tar.gz file containing binaries from local storage] ***********************************************************************************************************
skipping: [ubuntu]

TASK [Upload vector .tar.gz file containing binaries from local storage] ***********************************************************************************************************
skipping: [ubuntu]

TASK [Upload lh .zip file containing binaries from local storage] ******************************************************************************************************************
skipping: [ubuntu]

TASK [Create directrory for clickhouse-client] *************************************************************************************************************************************
ok: [ubuntu]

TASK [Create directrory for clickhouse-common-static] ******************************************************************************************************************************
ok: [ubuntu]

TASK [Create directrory for clickhouse-common-static-dbg] **************************************************************************************************************************
ok: [ubuntu]

TASK [Create directrory for clickhouse-server] *************************************************************************************************************************************
ok: [ubuntu]

TASK [Create directrory for vector] ************************************************************************************************************************************************
ok: [ubuntu]

TASK [Create directrory for lh] ****************************************************************************************************************************************************
ok: [ubuntu]

PLAY [Install Clickhouse] **********************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************
ok: [ubuntu]

TASK [Check that the clickhouse exists] ********************************************************************************************************************************************
ok: [ubuntu]

TASK [Extract common-static in the installation directory] *************************************************************************************************************************
skipping: [ubuntu]

TASK [Install common] **************************************************************************************************************************************************************
skipping: [ubuntu]

TASK [Extract common_dbg in the installation directory] ****************************************************************************************************************************
skipping: [ubuntu]

TASK [Install common_dbg] **********************************************************************************************************************************************************
skipping: [ubuntu]

TASK [Extract server in the installation directory] ********************************************************************************************************************************
skipping: [ubuntu]

TASK [Extract vector in the installation directory] ********************************************************************************************************************************
ok: [ubuntu]

TASK [Install server] **************************************************************************************************************************************************************
skipping: [ubuntu]

TASK [Extract client in the installation directory] ********************************************************************************************************************************
skipping: [ubuntu]

TASK [Install client] **************************************************************************************************************************************************************
skipping: [ubuntu]

TASK [Install Nginx] ***************************************************************************************************************************************************************
ok: [ubuntu]

TASK [Delete /var/www/html folder] *************************************************************************************************************************************************
changed: [ubuntu]

TASK [Create directrory] ***********************************************************************************************************************************************************
changed: [ubuntu]

TASK [Extract lh in the installation directory] ************************************************************************************************************************************
changed: [ubuntu]

PLAY [start] ***********************************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************
ok: [ubuntu]

TASK [start clickhouse-server] *****************************************************************************************************************************************************
changed: [ubuntu]

TASK [create link vector] **********************************************************************************************************************************************************
ok: [ubuntu]

TASK [Create directrory for vector] ************************************************************************************************************************************************
ok: [ubuntu]

TASK [copy config vector] **********************************************************************************************************************************************************
ok: [ubuntu]

TASK [add vector] ******************************************************************************************************************************************************************
changed: [ubuntu]

TASK [start vector] ****************************************************************************************************************************************************************
changed: [ubuntu]

PLAY RECAP *************************************************************************************************************************************************************************
ubuntu                     : ok=33   changed=12   unreachable=0    failed=0    skipped=14   rescued=0    ignored=0  
```  


---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---