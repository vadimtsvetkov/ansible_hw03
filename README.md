# Домашнее задание к занятию 3 «Использование Ansible»

## Подготовка к выполнению

1. Подготовьте в Yandex Cloud три хоста: для `clickhouse`, для `vector` и для `lighthouse`.
2. Репозиторий LightHouse находится [по ссылке](https://github.com/VKCOM/lighthouse).

## Основная часть

### 1. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает LightHouse.
#### 2. При создании tasks рекомендую использовать модули: `get_url`, `template`, `yum`, `apt`.
#### 3. Tasks должны: скачать статику LightHouse, установить Nginx или любой другой веб-сервер, настроить его конфиг для открытия LightHouse, запустить веб-сервер.
#### 4. Подготовьте свой inventory-файл `prod.yml`.
#### 5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
#### 6. Попробуйте запустить playbook на этом окружении с флагом `--check`.

<details>
<summary>Вывод консоли:</summary>

```sh
[tvadim@localhost playbook]$ ansible-playbook site.yml -i inventory/prod.yml --check

PLAY [Install Clickhouse] ******************************************************

TASK [Gathering Facts] *********************************************************
ok: [clickhouse-01]

TASK [Get clickhouse distrib] **************************************************
ok: [clickhouse-01] => (item=clickhouse-client)
ok: [clickhouse-01] => (item=clickhouse-server)
ok: [clickhouse-01] => (item=clickhouse-common-static)

TASK [Install clickhouse packages] *********************************************
ok: [clickhouse-01]

TASK [Create database] *********************************************************
skipping: [clickhouse-01]

PLAY [Install Vector] **********************************************************

TASK [Gathering Facts] *********************************************************
ok: [clickhouse-02]

TASK [Download vector packages] ************************************************
ok: [clickhouse-02]

TASK [Install vector packages] *************************************************
ok: [clickhouse-02]

TASK [Apply vector template] ***************************************************
ok: [clickhouse-02]

TASK [Change vector systemd unit] **********************************************
ok: [clickhouse-02]

PLAY [Install lighthouse] ******************************************************

TASK [Gathering Facts] *********************************************************
ok: [lighthouse-03]

TASK [Install git (Lighthouse)] ************************************************
ok: [lighthouse-03]

TASK [Install epel-release (Lighthouse)] ***************************************
ok: [lighthouse-03]

TASK [Install nginx (Lighthouse)] **********************************************
ok: [lighthouse-03]

TASK [Apply nginx config (Lighthouse)] *****************************************
ok: [lighthouse-03]

TASK [Clone repository (Lighthouse)] *******************************************
ok: [lighthouse-03]

TASK [Apply config (Lighthouse)] ***********************************************
ok: [lighthouse-03]

PLAY RECAP *********************************************************************
clickhouse-01              : ok=3    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
clickhouse-02              : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
lighthouse-03              : ok=7    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```   
</details>

#### 7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.

<details>
<summary>Вывод консоли:</summary>

```sh
[tvadim@localhost playbook]$ ansible-playbook site.yml -i inventory/prod.yml --diff

PLAY [Install Clickhouse] ******************************************************

TASK [Gathering Facts] *********************************************************
ok: [clickhouse-01]

TASK [Get clickhouse distrib] **************************************************
ok: [clickhouse-01] => (item=clickhouse-client)
ok: [clickhouse-01] => (item=clickhouse-server)
ok: [clickhouse-01] => (item=clickhouse-common-static)

TASK [Install clickhouse packages] *********************************************
ok: [clickhouse-01]

TASK [Create database] *********************************************************
ok: [clickhouse-01]

PLAY [Install Vector] **********************************************************

TASK [Gathering Facts] *********************************************************
ok: [clickhouse-02]

TASK [Download vector packages] ************************************************
ok: [clickhouse-02]

TASK [Install vector packages] *************************************************
ok: [clickhouse-02]

TASK [Apply vector template] ***************************************************
ok: [clickhouse-02]

TASK [Change vector systemd unit] **********************************************
ok: [clickhouse-02]

PLAY [Install lighthouse] ******************************************************

TASK [Gathering Facts] *********************************************************
ok: [lighthouse-03]

TASK [Install git (Lighthouse)] ************************************************
ok: [lighthouse-03]

TASK [Install epel-release (Lighthouse)] ***************************************
ok: [lighthouse-03]

TASK [Install nginx (Lighthouse)] **********************************************
ok: [lighthouse-03]

TASK [Apply nginx config (Lighthouse)] *****************************************
ok: [lighthouse-03]

TASK [Clone repository (Lighthouse)] *******************************************
ok: [lighthouse-03]

TASK [Apply config (Lighthouse)] ***********************************************
ok: [lighthouse-03]

PLAY RECAP *********************************************************************
clickhouse-01              : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
clickhouse-02              : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
lighthouse-03              : ok=7    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```   
</details>

#### 8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.

Команда выдала тоже самое.

#### 9. Подготовьте README.md-файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.
#### 10. Готовый playbook выложите в свой репозиторий, поставьте тег `08-ansible-03-yandex` на фиксирующий коммит, в ответ предоставьте ссылку на него.

---