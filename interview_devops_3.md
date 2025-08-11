
<div align="center">
<a href="https://devops-factory.com" >
  <img src="https://devops-factory.com/images/logott.png" width="250" alt="DevOps Factory">
</a>
</div>
<div align="center">
    <a href="https://devops-factory.com" style="vertical-align: middle;display: inline-block;">
      <img align="center" src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=14&duration=2000&pause=&center=true&vCenter=true&multiline=true&width=435&lines=%D0%A1%D0%B4%D0%B5%D0%BB%D0%B0%D0%BD%D0%BE+%D0%B0%D0%BA%D1%82%D0%B8%D0%B2%D0%B8%D1%81%D1%82%D0%BE%D0%BC+%D0%B8%D0%B7+%D0%BA%D0%BB%D1%83%D0%B1%D0%B0+%D0%BF%D1%80%D0%B8+%D1%84%D0%B0%D0%B1%D1%80%D0%B8%D0%BA%D0%B5;By+Dmitriy+Shmakov" alt="" />
    </a>  
  </a>
</div>
<div align="center">


<a href="https://devops-factory.com">
    <img src="https://img.shields.io/badge/DevOps%20Factory-green?style=for-the-badge" alt=""/>
</div>

<div align="center">
     <a href="=https://www.youtube.com/@devopsfactory">
    <img src="https://img.shields.io/badge/YouTube-red?style=for-the-badge" alt=""/>
   <a href="=https://t.me/@devopsfactory">
    <img src="https://img.shields.io/badge/Telegram-blue?style=for-the-badge" alt=""/>
</div>


# DevOps Interview Knowledge Base

Разбор **[мок собеса №3](https://youtu.be/z0QZ7VLyP7w?t)** в формате "Вопрос — Ответ" для подготовки к собеседованиям на позицию DevOps.

## 📌 Содержание

- [1. Ansible](#1-ansible)
- [2. Linux и Bash](#2-linux-и-bash)
- [3. Docker](#3-docker)
- [4. CI-CD](#4-ci-cd)
- [5. Postgres](#5-postgres)

---

# 1. Ansible

### ❓ 1. В чём разница между playbook и role в Ansible и как они используются?

**[📺 Видео-вопрос (05:33)](https://youtu.be/z0QZ7VLyP7w?t=333)**

<details><summary><strong>Показать ответ</strong></summary><p>

## 🔹 Playbook (плейбук)

### Определение

**Playbook** — это YAML-файл, в котором описаны сценарии (plays), содержащие список задач (tasks) для выполнения на хостах.

### Особенности

- Запускается напрямую: `ansible-playbook site.yml`
- Содержит описание:
  - **hosts** — на каких хостах выполняется
  - **tasks** — список шагов
  - **vars** — переменные
  - **handlers** — обработчики событий
- Может быть большим и комплексным
- Часто служит «точкой входа» для запуска ролей

### Пример

```yaml
---
- name: Установка и настройка Nginx
  hosts: webservers
  become: true
  tasks:
    - name: Установить Nginx
      apt:
        name: nginx
        state: present
    - name: Запустить и включить Nginx
      service:
        name: nginx
        state: started
        enabled: true
```

## 🔹 Role (роль)

### Определение

**Role** — это структурированный способ организации задач, переменных, шаблонов и файлов в Ansible.

### Особенности

- Хранится в виде директории с определённой структурой (`tasks/`, `handlers/`, `templates/`, `files/`, `vars/`, `defaults/` и т.д.)
- Позволяет **повторно использовать код**
- Легко импортировать в разные playbooks
- Поддерживает Galaxy (репозиторий готовых ролей)

### Структура директории роли

```
myrole/
  tasks/main.yml
  handlers/main.yml
  templates/
  files/
  vars/main.yml
  defaults/main.yml
```

### Пример использования роли в плейбуке

```yaml
- name: Настройка веб-сервера
  hosts: webservers
  roles:
    - nginx
    - php
```

## 📊 Сравнение Playbook и Role

| Характеристика              | Playbook                                   | Role                                         |
| --------------------------- | ------------------------------------------ | -------------------------------------------- |
| **Что это**                 | Сценарий выполнения задач                  | Модуль для переиспользования задач и файлов  |
| **Структура**               | Один или несколько YAML-файлов             | Каталог с фиксированной структурой           |
| **Повторное использование** | Ограниченное                               | Высокое                                      |
| **Запуск**                  | `ansible-playbook my.yml`                  | Подключается в playbook                      |
| **Сценарий применения**     | Запуск конкретной последовательности задач | Организация и переиспользование конфигурации |

## 📌 Вывод

- **Playbook** — это набор инструкций, который описывает, что нужно сделать и на каких хостах.
- **Role** — это способ структурировать и повторно использовать код Ansible в разных playbooks.
- На практике: **Playbook = сценарий, Role = строительный блок**.

## 🚀 Рекомендации

- Для небольших автоматизаций можно писать задачи прямо в playbook.
- Для больших проектов — выносите логику в роли.
- Используйте **Ansible Galaxy** для поиска и установки готовых ролей:

```bash
ansible-galaxy install geerlingguy.nginx
```

</p></details>

---

### ❓ 2. В чём разница между role и group в Ansible и для чего они используются?

**[📺 Видео-вопрос (06:56)](https://youtu.be/z0QZ7VLyP7w?t=416)**

<details><summary><strong>Показать ответ</strong></summary><p>

## 🔹 Role (роль)

### Определение

**Role** — это способ структурированной организации кода Ansible, включающей задачи, переменные, шаблоны, файлы, обработчики и др., объединённые для выполнения определённой функции.

### Особенности

- Роли позволяют **повторно использовать код** в разных плейбуках.
- Хранятся в виде директорий со стандартной структурой (`tasks/`, `handlers/`, `templates/`, `files/`, `vars/`, `defaults/` и т.д.)
- Пример: роль `nginx` может содержать установку, конфигурацию и запуск Nginx.
- Подключаются в плейбуках через ключевое слово `roles`.

### Пример структуры роли:

```
roles/
  nginx/
    tasks/main.yml
    handlers/main.yml
    templates/nginx.conf.j2
    files/
    vars/main.yml
    defaults/main.yml
```

### Пример использования роли:

```yaml
- hosts: webservers
  roles:
    - nginx
```

## 🔹 Group (группа)

### Определение

**Group** — это логическое объединение хостов в Ansible-инвентаре (inventory) для удобства управления.

### Особенности

- Группы определяются в **inventory-файле** (INI, YAML, динамический инвентарь).
- Позволяют выполнять задачи сразу на нескольких хостах одной категории.
- Могут содержать переменные, специфичные для всей группы (`group_vars`).
- Могут быть вложенными (группа в группе).

### Пример inventory:

```ini
[webservers]
web1.example.com
web2.example.com

[dbservers]
db1.example.com
db2.example.com
```

### Пример использования группы в плейбуке:

```yaml
- hosts: webservers
  tasks:
    - name: Установить Nginx
      apt:
        name: nginx
        state: present
```

## 📊 Сравнение Role и Group

| Характеристика              | Role (роль)                                                | Group (группа)                               |
| --------------------------- | ---------------------------------------------------------- | -------------------------------------------- |
| **Что это**                 | Набор задач и ресурсов для выполнения определённой функции | Логическое объединение хостов                |
| **Где определяется**        | В каталоге `roles/` с фиксированной структурой             | В inventory-файле                            |
| **Назначение**              | Организация и переиспользование кода                       | Управление наборами хостов                   |
| **Как используется**        | Через `roles:` в плейбуке                                  | Через `hosts:` в плейбуке                    |
| **Повторное использование** | Да, можно подключать в разные проекты                      | Да, можно переиспользовать в любых плейбуках |
| **Переменные**              | `vars`, `defaults` внутри роли                             | `group_vars` для всей группы                 |

## 📌 Вывод

- **Role** отвечает за то, _что_ и _как_ делать (набор действий, файлов, переменных).
- **Group** отвечает за то, _где_ и _на каких хостах_ выполнять эти действия.
- Обычно роли используют для автоматизации установки/настройки ПО, а группы — для управления разными наборами серверов.

## 🚀 Рекомендации

- Разделяйте логику (roles) и инфраструктуру (groups).
- Давайте понятные имена группам (`web`, `db`, `cache`) и ролям (`nginx`, `mysql`, `redis`).
- Храните переменные окружения в `group_vars` для удобного переиспользования.

</p></details>

---

### ❓ 3. Какие модули Ansible можно использовать для установки и настройки файлового менеджера?

**[📺 Видео-вопрос (07:28)](https://youtu.be/z0QZ7VLyP7w?t=448)**

<details><summary><strong>Показать ответ</strong></summary><p>

## 🔹 Универсальный абстрактный модуль

**`ansible.builtin.package`** — абстракция над менеджерами пакетов.

```yaml
- name: Установить mc (любой Linux)
  become: true
  ansible.builtin.package:
    name: mc
    state: present # present|latest|absent
```

## 🔹 Модули для конкретных пакетных менеджеров.

### 🐧 Debian/Ubuntu

**`ansible.builtin.apt`**

```yaml
- name: Установить mc (Debian/Ubuntu)
  become: true
  ansible.builtin.apt:
    name: mc
    state: present
    update_cache: yes
```

**Часто используется вместе с** `ansible.builtin.apt_repository`:

```yaml
- name: Добавить PPA
  become: true
  ansible.builtin.apt_repository:
    repo: ppa:some/ppa
```

### 🐧 RHEL/CentOS/Fedora/Rocky/Alma

**`ansible.builtin.dnf`** (современные) / **`ansible.builtin.yum`** (старые)

```yaml
- name: Установить mc (RHEL-семейство)
  become: true
  ansible.builtin.dnf:
    name: mc
    state: present
```

**Часто используется вместе с** `ansible.builtin.yum_repository`:

```yaml
- name: Подключить репозиторий
  become: true
  ansible.builtin.yum_repository:
    name: tools
    baseurl: https://repo.example/yum/
    gpgcheck: yes
    gpgkey: https://repo.example/RPM-GPG-KEY
```

## 💡 Подсказки

- Всегда ставьте `become: true` для задач установки.
- Для кросс-дистрибутивных ролей используйте `package`, а OS-специфику закрывайте через `when`.
- Для подключения внешних репозиториев используйте:
  - `apt_repository` (Debian/Ubuntu)
  - `yum_repository` (RHEL/CentOS/Fedora/Rocky/Alma)
  - `zypper_repository` (SUSE)

</p></details>

---

### ❓ 4. Какими способами можно управлять конфигурационными файлами пакета после его установки с помощью Ansible?

**[📺 Видео-вопрос (08:15)](https://youtu.be/z0QZ7VLyP7w?t=495)**

<details><summary><strong>Показать ответ</strong></summary><p>

## 🧱 Базовые модули для управления

### 1. `template` — шаблоны Jinja2 (рекомендуется)

- Используйте для параметризации конфигов переменными.
- Поддерживает `validate` и `backup`.

```yaml
- name: Render config from template
  ansible.builtin.template:
    src: templates/nginx.conf.j2
    dest: /etc/nginx/nginx.conf
    mode: '0644'
    validate: 'nginx -t -c %s'
  notify: Reload nginx
```

### 2. `copy` — копия файла как есть

- Для статичных конфигов (без переменных).
- Есть `backup`, можно `validate` через `check_cmd` (через `notify` или `changed_when`).

```yaml
- name: Drop static conf
  ansible.builtin.copy:
    src: files/app.conf
    dest: /etc/app/app.conf
    owner: root
    group: root
    mode: '0644'
    backup: true
  notify: Restart app
```

### 3. `lineinfile` — точечная правка строки

- Когда нужно изменить/вставить 1–2 строки без полной замены файла.

```yaml
- name: Ensure setting is present
  ansible.builtin.lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '^PasswordAuthentication'
    line: 'PasswordAuthentication no'
    backup: true
  notify: Reload ssh
```

### 4. `blockinfile` — вставка блока текста

- Удобно для управляемых секций в больших конфигах.

```yaml
- name: Manage managed block
  ansible.builtin.blockinfile:
    path: /etc/app/app.conf
    marker: '# {mark} ANSIBLE MANAGED BLOCK'
    block: |
      log_level = info
      workers = 4
  notify: Restart app
```

### 5. `replace` — замена по regex

- Для сложных замен в уже существующем файле.

```yaml
- name: Replace legacy directive
  ansible.builtin.replace:
    path: /etc/app/app.conf
    regexp: '(^|\s)threads\s*=\s*\d+'
    replace: 'threads = 8'
  notify: Restart app
```

### 6. `ini_file` — работа с форматом INI

- Встроенный модуль для key=value в INI-файлах.

```yaml
- name: Tune INI param
  ansible.builtin.ini_file:
    path: /etc/php/8.1/fpm/php.ini
    section: PHP
    option: memory_limit
    value: 512M
  notify: Reload php8.1-fpm
```

### 7. `assemble` — сборка файла из частей

- Полезно для каталогов `conf.d/` с фрагментами.

```yaml
- name: Assemble conf from parts
  ansible.builtin.assemble:
    src: /etc/app/conf.d.parts/
    dest: /etc/app/conf.d/all.conf
    mode: '0644'
  notify: Restart app
```

</p></details>

---

### ❓ 5. Каковы преимущества и недостатки модулей lineinfile и template в Ansible, и в каких случаях лучше использовать каждый из них?

**[📺 Видео-вопрос (11:05)](https://youtu.be/z0QZ7VLyP7w?t=661)**

<details><summary><strong>Показать ответ</strong></summary><p>

- **`lineinfile`** — точечная правка существующего файла (1–2 строки по `regexp`).
- **`template`** — формирование **всего файла** из Jinja2‑шаблона (полный контроль содержимого).

## 📊 Сравнение

| Критерий                                   | `lineinfile`                         | `template`                          |
| ------------------------------------------ | ------------------------------------ | ----------------------------------- |
| Назначение                                 | Изменить/вставить строку             | Сгенерировать целый файл            |
| Контроль содержимого                       | Частичный                            | Полный                              |
| Сложность файла                            | Простые/локальные правки             | Любая сложность, секции, списки     |
| Идемпотентность                            | Хорошая при точной `regexp`          | Отличная: файл совпадает с шаблоном |
| Конфликты с чужими правками                | Минимальны                           | Возможны (перезаписывает всё)       |
| Валидация перед применением                | Через отдельные шаги                 | Есть `validate:` в модуле           |
| Подходит для больших многострочных вставок | Неудобно (лучше `blockinfile`)       | Удобно                              |
| Версионирование в Git                      | Труднее отслеживать «точечные» diffs | Прозрачно: один шаблон              |

## ✅ Когда использовать

### `lineinfile`

Используйте, когда:

- Нужна **одна‑две правки** в «чужом» файле (который меняют пакеты/люди/скрипты).
- Формат простой (`key=value`, одиночные опции).
- Не хотите перезатирать остальной контент.

**Пример — отключить пароли в SSH:**

```yaml
- ansible.builtin.lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '^PasswordAuthentication'
    line: 'PasswordAuthentication no'
    backup: true
  notify: Reload ssh
```

> Для многострочных кусков лучше `blockinfile`.

### `template`

Используйте, когда:

- Вы **полностью владеете конфигом** и нужна воспроизводимость.
- Файл сложный, зависит от переменных/условий.
- Важно проверить конфиг до применения (`validate:`).

**Пример — сгенерировать `nginx.conf` с валидацией:**

```yaml
- ansible.builtin.template:
    src: templates/nginx.conf.j2
    dest: /etc/nginx/nginx.conf
    owner: root
    group: root
    mode: '0644'
    validate: 'nginx -t -c %s'
  notify: Reload nginx
```

</p></details>

---

### ❓ 6. Какие недостатки lineinfile

**[📺 Видео-вопрос (12:14)](https://youtu.be/z0QZ7VLyP7w?t=734)**

<details><summary><strong>Показать ответ</strong></summary><p>

## ⚠️ Недостатки модуля `lineinfile` в Ansible

1. **Нет контроля целого файла** — меняет только строки, возможен дрейф состояния.
2. **Зависимость от точности `regexp`** — ошибки приводят к дубликатам или правке не того места.
3. **Плохо масштабируется** — много задач с `lineinfile` сложно поддерживать.
4. **Сложно управлять блоками/порядком строк** — неудобно для многострочных вставок.
5. **Ограничения по форматам** — легко сломать INI/YAML/JSON, нарушить синтаксис.
6. **Дубликаты ключей** — трудно гарантировать единственность параметра.
7. **Нет встроенной валидации** — нельзя использовать `validate:` как в `template`.
8. **Трудно версионировать в Git** — набор мелких правок менее нагляден, чем шаблон.
9. **Хрупкость при пробелах/комментариях** — регексы ломаются от вариаций записи.
10. **Неприменим для бинарных/сгенерированных файлов** — может повредить содержимое.

</p></details>

---

### ❓ 7. Что такое ansible vault?

**[📺 Видео-вопрос (13:34)](https://youtu.be/z0QZ7VLyP7w?t=814)**

<details><summary><strong>Показать ответ</strong></summary><p>

**Ansible Vault** — это встроенный инструмент Ansible для шифрования конфиденциальных данных (пароли, токены, ключи) в репозитории и их прозрачного использования во время выполнения плейбуков.

## 🔧 Базовые команды

```bash
# Создать зашифрованный файл
ansible-vault create group_vars/all/vault.yml

# Зашифровать существующий файл
ansible-vault encrypt vars.yml

# Расшифровать файл (снять защиту)
ansible-vault decrypt vars.yml

# Редактировать зашифрованный файл
ansible-vault edit group_vars/all/vault.yml

# Показать содержимое (read-only)
ansible-vault view group_vars/all/vault.yml

# Перешифровать другим паролем/идентификатором
ansible-vault rekey group_vars/all/vault.yml
```

Запуск плейбуков:

```bash
ansible-playbook site.yml --ask-vault-pass
# или
ansible-playbook site.yml --vault-password-file .vault_pass.txt
```

Шифрование строки «на месте»:

```bash
ansible-vault encrypt_string 'supersecret' --name 'db_password'
```

Результат можно вставить в обычный YAML:

```yaml
db_password: !vault |
  $ANSIBLE_VAULT;1.1;AES256...
```

## 🧩 Использование в плейбуках

```yaml
# group_vars/all/vault.yml (зашифрован)
vault_db_password: supersecret

# playbook.yml
- hosts: db
  vars_files:
    - group_vars/all/vault.yml
  tasks:
    - name: Создать пользователя БД
      community.mysql.mysql_user:
        name: app
        password: "{{ vault_db_password }}"
        priv: "*.*:ALL"
```

## 🆔 Vault IDs (несколько ключей на проект)

Позволяют иметь разные ключи для prod/stage/dev.

Создание с меткой (ID):

```bash
ANSIBLE_VAULT_IDENTITY_LIST="prod@.vault_pass_prod,stage@.vault_pass_stage" ansible-vault create --vault-id prod@prompt group_vars/prod/vault.yml
```

Запуск плейбука с несколькими ID:

```bash
ansible-playbook site.yml   --vault-id prod@.vault_pass_prod   --vault-id stage@.vault_pass_stage
```

> Файлы, шифрованные с `prod`‑ID, откроются ключом `prod`, `stage` — ключом `stage`.

## ⚙️ ansible.cfg (удобство)

`.ansible.cfg` или `ansible.cfg` в репо:

```ini
[defaults]
vault_identity_list = prod@.vault_pass_prod, stage@.vault_pass_stage
# или для простого случая
# vault_password_file = .vault_pass.txt
```

> Никогда не коммитим файлы с паролями (`.vault_pass*`) — добавьте в `.gitignore` и храните в секрет‑хранилище.

## 🔒 Лучшие практики безопасности

- Не шифруйте «всё подряд» — только секреты. Структуру и ключи переменных оставляйте открытыми.
- Пароли к Vault **не храните в репозитории**. Используйте менеджеры секретов (1Password, LastPass, KeePassXC, gopass) или CI‑секреты.
- Для разных окружений — **разные Vault ID/ключи** (prod ≠ stage).
- Делайте **`rekey`** при смене команды/утечке.
- Логируйте доступ и регламентируйте, кто знает prod‑ключ.
- При код‑ревью проверяйте, что секреты не утекли в открытый текст/логи CI

</p></details>

---

### ❓ 8. Как запустить playbook Ansible в режиме проверки, чтобы увидеть планируемые изменения без их применения на удалённых серверах

**[📺 Видео-вопрос (15:04)](https://youtu.be/z0QZ7VLyP7w?t=904)**

<details><summary><strong>Показать ответ</strong></summary><p>

## 🏃 Быстрый старт

```bash
ansible-playbook site.yml --check
```

- `--check` (_check mode_) — прогон без внесения изменений (насколько модуль это поддерживает).

### Полезные флаги вместе с `--check`

```bash
ansible-playbook site.yml --check --diff -v
```

- `--diff` — показать различия для файлов/шаблонов.
- `-v` / `-vvv` — больше подробностей.
- `--limit web` — ограничить хосты.
- `--tags setup` / `--skip-tags slow` — выбрать подмножество задач.
- `--list-hosts` / `--list-tasks` — показать, кто/что будет выполняться (без запуска).

## 📌 Важные нюансы check‑mode

- **Оценка «наилучшая»**, но **не гарантия**: не все модули умеют предсказывать изменения без фактического применения.
- Некоторые модули **игнорируют check‑mode** (или частично). Для них предусмотрен параметр `check_mode: no` на задаче (используйте осторожно).
- Задачи с `command`/`shell` **обычно не выполняются** в check‑mode → статус _skipped_ или _unknown_.
- Для корректной оценки нужен **`gather_facts: true`**, если задачи зависят от фактов.
- Шаблоны/копирование (`template`, `copy`) в `--check --diff` покажут различия, **не записывая** файл.

## 🧩 Примеры

### 1. Dry‑run + diff для части хостов/задач

```bash
ansible-playbook site.yml   --check --diff   --limit "web:&prod"   --tags "nginx,php"
```

### 2. Сначала посмотреть «план» задач и хостов

```bash
ansible-playbook site.yml --list-hosts
ansible-playbook site.yml --list-tasks --tags nginx
```

## ✅ Рекомендации

- Комбинируйте `--check` с `--diff` — это даёт максимум ясности для файлов/шаблонов.
- Используйте теги (`--tags`, `--skip-tags`) для ускорения и фокусировки.
- Для модулей без поддержки check‑mode — добавляйте явную логику (`when: ansible_check_mode`, `changed_when`, `check_mode`).
- Держите **идемпотентность** задач — тогда dry‑run будет точнее и полезнее.

## 🧪 Мини‑чеклист перед prod

- [ ] `ansible-playbook site.yml --list-hosts` — убедиться в таргетинге.
- [ ] `ansible-playbook site.yml --list-tasks` — проверить порядок и объём задач.
- [ ] `ansible-playbook site.yml --check --diff` — увидеть предполагаемые изменения.
- [ ] Прогон на тестовой группе/одном хосте (`--limit host1`).

</p></details>

---

### ❓ 9. Как сделать отладку ansible кода?

**[📺 Видео-вопрос (16:34)](https://youtu.be/z0QZ7VLyP7w?t=994)**

<details><summary><strong>Показать ответ</strong></summary><p>

### ⚡ Быстрые флаги CLI

```bash
ansible-playbook play.yml -v                           # базовая подробность
ansible-playbook play.yml -vvv                         # детально: аргументы модулей, ssh
ansible-playbook play.yml --step                       # пошаговый режим (confirm перед задачей)
ansible-playbook play.yml --start-at-task "Имя задачи" # Начать выполнение с указанной задачи
ansible-playbook play.yml --check --diff               # dry-run + различия файлов
ansible-playbook play.yml --list-hosts                 # показать хосты
ansible-playbook play.yml --list-tasks                 # показать задачи
ansible-playbook play.yml --list-tags                  # показать теги
ansible-playbook play.yml --syntax-check               # проверка синтаксиса
```

Фокусировка: `--limit web`, `--tags setup`, `--skip-tags slow`.

### 🌍 Полезные переменные окружения

```bash
export ANSIBLE_DEBUG=1                  # включает «глубокий» режим логирования Ansible (стек-трейсы Python, внутренние детали).
export ANSIBLE_KEEP_REMOTE_FILES=1      # не удалять временные файлы модулей на удалённой машине
export ANSIBLE_STDOUT_CALLBACK=yaml     # меняет формат вывода на более читаемый YAML вместо стандартного.
export ANSIBLE_LOG_PATH=ansible.log     # пишет полный лог выполнения в файл ansible.log
```

> Верните значения по умолчанию после отладки, чтобы не копить мусор/логи.

**Пример**

```bash
ansible-playbook -i inventory play.yml -vvv --check --diff --limit web
```

> Не все модули умеют предсказывать изменения без применения; проверяйте критичные места отдельно.

### 🧰 Отладка внутри playbook

**Модуль `ansible.builtin.debug` (внутри плейбука)** - это задача внутри playbook, которая выводит значения переменных или произвольные сообщения:

```yaml
- name: Показать переменную
  ansible.builtin.debug:
    var: my_variable

- name: Произвольное сообщение с шаблоном
  ansible.builtin.debug:
    msg: "User={{ ansible_facts.user_id | default('n/a') }}"
```

### 📋 Мини‑чеклист перед prod

- `--list-hosts`, `--list-tasks`, `--syntax-check`
- `--check --diff` на одном хосте/в стейдже
- Включён `validate` для критичных конфигов
- Переменные и секреты определены, `assert` не срабатывает

</p></details>

---

# 2. Linux и Bash

### ❓ 1. Что такое LTS?

**[📺 Видео-вопрос (21:37)](https://youtu.be/z0QZ7VLyP7w?t=1297)**

<details><summary><strong>Показать ответ</strong></summary><p>

**LTS** (_Long-Term Support_) — это релиз программного обеспечения с **долгосрочной поддержкой**, которая включает:

- Обновления безопасности
- Исправления багов
- Поддержку критических компонентов

### 📊 Сравнение LTS и обычных релизов

| Критерий              | **LTS-релиз**                          | **Обычный релиз (non-LTS)**           |
| --------------------- | -------------------------------------- | ------------------------------------- |
| **Срок поддержки**    | 3–10 лет                               | 6–12 месяцев                          |
| **Стабильность**      | ✅ Максимально стабильный              | ❗ Возможны изменения, нестабильность |
| **Новые функции**     | ❌ Минимум новшеств, только стабильное | ✅ Новые и экспериментальные фичи     |
| **Частота релизов**   | Реже (раз в 2–3 года)                  | Чаще (раз в 6–12 месяцев)             |
| **Целевая аудитория** | Бизнес, продакшн, серверы              | Разработчики, тестовые окружения      |

### ✅ Когда использовать LTS?

- Для **продакшн-серверов**
- Когда важна **долговечность и безопасность**
- В критичных бизнес-приложениях
- Если не хочется часто обновлять платформу

### ⚠️ Когда подходит обычный релиз?

- Для **разработки и тестирования**
- Чтобы быстро получить **новые функции**
- В краткоживущих проектах

### 💡 Вывод

> **LTS** — это надёжность, стабильность и поддержка  
> **Non-LTS** — это свежесть, гибкость и новизна

Выбор зависит от целей: **продакшн → LTS**, **разработка → non-LTS**

</p></details>

---

### ❓ 2. Какие существуют способы аутентификации при подключении к серверу Linux по SSH, и как выбрать пользователя, под которым будет выполняться вход?

**[📺 Видео-вопрос (24:01)](https://youtu.be/z0QZ7VLyP7w?t=1441)**

<details><summary><strong>Показать ответ</strong></summary><p>

## 1️⃣ Выбор пользователя при подключении

- **Через команду**:
  ```bash
  ssh user@host
  ```
- **Через флаг**:
  ```bash
  ssh -l user host
  ```
- **В `~/.ssh/config`**:
  ```sshconfig
  Host myserver
      HostName 192.168.1.10
      User admin
  ```

> ⚠ **Best practice:**
>
> - Для администрирования использовать **отдельного системного пользователя** (например, `deploy` или `admin`), а не `root` напрямую.
> - `root` — только через `sudo`, чтобы логировались действия.

## 2️⃣ Основные виды аутентификации по SSH

### 🔹 Парольная

- **Пример**:
  ```bash
  ssh user@host
  # запросит пароль
  ```
- **Плюсы:** просто, без предварительной настройки.
- **Минусы:** небезопасно (уязвимо к перебору паролей).
- **Best practice:** отключить (`PasswordAuthentication no` в `/etc/ssh/sshd_config`) и использовать ключи.

---

### 🔹 По открытым ключам (RSA/Ed25519/ECDSA)

- **Суть:** на клиенте хранится приватный ключ, на сервере — публичный (`~/.ssh/authorized_keys`).
- **Пример генерации ключа**:
  ```bash
  ssh-keygen -t ed25519 -C "me@example.com"
  ssh-copy-id user@host
  ```
- **Best practice:**
  - Использовать современный тип: **Ed25519** (короче, безопаснее).
  - Приватный ключ защищать паролем (`passphrase`).
  - Ограничить команды/ресурсы в `authorized_keys` при автоматизации.

---

### 🔹 С агентом SSH (ssh-agent)

- **Суть:** ключ хранится в памяти агента, не надо каждый раз вводить пароль.
- **Пример**:
  ```bash
  eval "$(ssh-agent -s)"
  ssh-add ~/.ssh/id_ed25519
  ssh user@host
  ```
- **Best practice:**
  - Использовать агент в сочетании с менеджером ключей (`gnome-keyring`, `1Password`, `KeePassXC`).
  - Устанавливать таймаут (`ssh-add -t 3600`).

---

### 🔹 Через сертификаты (CA-signed keys)

- **Суть:** ключ подписывается доверенным CA, сервер доверяет CA, а не отдельным ключам.
- **Best practice:**
  - Использовать в крупных инфраструктурах для централизованного управления доступами.
  - Срок действия сертификатов — минимально необходимый.

---

### 🔹 Двухфакторная (MFA/2FA)

- **Суть:** SSH + код/токен (Google Authenticator, Yubikey, Duo).
- **Best practice:**
  - Для критичных серверов требовать второй фактор.
  - Для автоматизации — отдельные учетные записи без MFA, но с ограничениями по ключу/сети.

---

## 3️⃣ Безопасные настройки SSH-сервера (`/etc/ssh/sshd_config`)

```conf
Protocol 2
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AuthenticationMethods publickey
AllowUsers admin deploy
PermitEmptyPasswords no
ClientAliveInterval 300
ClientAliveCountMax 2
```

После изменений:

```bash
sudo systemctl restart sshd
```

---

## 4️⃣ Best Practices в целом

- **Не использовать парольный вход** для админов — только ключи.
- **Не логиниться напрямую под root** — использовать sudo.
- **Использовать Ed25519** для новых ключей.
- **Ограничивать доступ по IP** (`sshd_config` → `Match Address`).
- **Вести аудит** (`/var/log/auth.log` или journald).
- **Регулярно менять ключи** и удалять неиспользуемые.

## 5️⃣ Примеры подключения

**Обычное подключение по ключу:**

```bash
ssh -i ~/.ssh/id_ed25519 admin@myserver
```

</p></details>

---

### ❓ 3. Что такое fail2ban и как он работает?

**[📺 Видео-вопрос (27:04)](https://youtu.be/z0QZ7VLyP7w?t=1624)**

<details><summary><strong>Показать ответ</strong></summary><p>

**Fail2ban — это утилита для защиты Linux-сервера от перебора паролей и некоторых атак, автоматически блокирующая IP-адреса нарушителей**

## 🔹 Как работает

1. **Читает логи** сервисов и системы (например `/var/log/auth.log`, `/var/log/secure`, логи nginx, postfix).
2. **Фильтрует события** по регуляркам (`failregex`) из файлов `/etc/fail2ban/filter.d/*.conf`.
3. **Подсчитывает нарушения** — если IP превысил `maxretry` за время `findtime`, он считается нарушителем.
4. **Блокирует IP** через firewall (iptables, nftables, firewalld) на время `bantime`.
5. **Снимает блокировку** автоматически по истечении `bantime`.

## 📂 Основные термины

- **Jail** — связка фильтра и действия.
- **Filter** — определяет, что считать нарушением (regex).
- **Action** — команды, выполняемые при нарушении (обычно бан IP).
- **Backend** — средство блокировки (iptables/nftables/firewalld).

## 📦 Установка

Debian/Ubuntu:

```bash
sudo apt update && sudo apt install fail2ban
```

CentOS/RHEL:

```bash
sudo yum install epel-release
sudo yum install fail2ban
```

Запуск и автозапуск:

```bash
sudo systemctl enable --now fail2ban
```

## ⚙️ Базовая настройка

Редактируем локальный конфиг:

```bash
sudo nano /etc/fail2ban/jail.local
```

Пример для SSH:

```ini
[sshd]
enabled  = true
port     = ssh
filter   = sshd
logpath  = /var/log/auth.log
maxretry = 5
findtime = 300
bantime  = 600
```

- **maxretry = 5** → 5 неудачных попыток
- **findtime = 300** → за 5 минут
- **bantime = 600** → бан на 10 минут

## 🔍 Проверка статуса и управление

Общий статус:

```bash
sudo fail2ban-client status
```

Статус конкретного jail:

```bash
sudo fail2ban-client status sshd
```

Разбанить IP:

```bash
sudo fail2ban-client set sshd unbanip 1.2.3.4
```

Забанить IP вручную:

```bash
sudo fail2ban-client set sshd banip 1.2.3.4
```

## 🛠 Best Practices

- Для SSH:
  - `PasswordAuthentication no`
  - Только ключи SSH
  - `PermitRootLogin no`
- Для веб-сервисов:
  - Настроить `maxretry` и `bantime` с учётом нормального трафика
  - Комбинировать с rate-limit в nginx/HAProxy
- Вести журнал заблокированных IP:
  ```bash
  sudo fail2ban-client status <jail> | grep 'Banned IP list'
  ```
- Логи и фильтры держать актуальными

## 📌 Полезные команды

```bash
# Проверить конфиг
sudo fail2ban-client -d

# Перезапустить fail2ban
sudo systemctl restart fail2ban

# Тест фильтра
fail2ban-regex /var/log/auth.log /etc/fail2ban/filter.d/sshd.conf
```

## 📚 Ресурсы

- Документация: https://www.fail2ban.org/wiki/
- Примеры фильтров: `/etc/fail2ban/filter.d/`

</p></details>

---

### ❓ 4. Какие команды оычно используются для диагностики состояния сервера?

**[📺 Видео-вопрос (28:34)](https://youtu.be/z0QZ7VLyP7w?t=1696)**

<details><summary><strong>Показать ответ</strong></summary><p>

## Этап 1: Общий обзор состояния системы

### 📊 Команда `top` или `htop`

```bash
top
# или, если установлен:
htop
```

**Проверяем:**

- Загрузка CPU (`%us`, `%sy`, `%id`)
- Использование памяти (RES, SWAP)
- Процессы, потребляющие больше всего ресурсов

<details><summary><strong>Полное описание команды top</strong></summary><p>

#### 📊 Верхняя часть — общая сводка

##### 🕒 Строка 1: Время, аптайм, пользователи, нагрузка

```
top - 14:23:45 up 3 days,  4:56,  2 users,  load average: 0.52, 0.48, 0.31
```

| Поле           | Значение                            |
| -------------- | ----------------------------------- |
| `14:23:45`     | Текущее время                       |
| `up 3 days...` | Аптайм — как долго работает система |
| `2 users`      | Количество активных пользователей   |
| `load average` | Средняя нагрузка за 1, 5, 15 минут  |

##### 📋 Строка 2: Задачи (процессы)

```
Tasks: 215 total, 1 running, 211 sleeping, 3 stopped, 0 zombie
```

| Статус     | Значение                 |
| ---------- | ------------------------ |
| `total`    | Всего процессов          |
| `running`  | Активно выполняются      |
| `sleeping` | Ждут событий             |
| `stopped`  | Остановлены              |
| `zombie`   | Завершены, но не удалены |

##### 🧠 Строка 3: Загрузка CPU

```
%us  %sy  %ni  %id  %wa  %hi  %si  %st
```

| Поле  | Описание                                 |
| ----- | ---------------------------------------- |
| `%us` | Пользовательская нагрузка                |
| `%sy` | Системная нагрузка                       |
| `%ni` | Nice — процессы с изменённым приоритетом |
| `%id` | Idle — простаивающее время               |
| `%wa` | Wait — ожидание ввода/вывода             |
| `%hi` | Hardware interrupts                      |
| `%si` | Software interrupts                      |
| `%st` | Steal time (на виртуальных машинах)      |

##### 🧮 Строки 4–5: Память и swap

```
MiB Mem :  15929.0 total,  11054.7 used,  4874.3 free,  132.5 buffers
MiB Swap:  2048.0 total,   112.0 used,  1936.0 free.  3674.0 cached
```

| Поле      | Значение            |
| --------- | ------------------- |
| `total`   | Общий объём памяти  |
| `used`    | Используется сейчас |
| `free`    | Свободна            |
| `buffers` | Буферы для I/O      |
| `cached`  | Файловый кэш        |

##### 📋 Таблица процессов (нижняя часть)

```
PID USER  PR  NI VIRT  RES  SHR S %CPU %MEM TIME+ COMMAND
```

| Поле      | Значение                        |
| --------- | ------------------------------- |
| `PID`     | ID процесса                     |
| `USER`    | Владелец процесса               |
| `PR`      | Приоритет                       |
| `NI`      | Nice-значение                   |
| `VIRT`    | Виртуальная память              |
| `RES`     | Резидентная (физическая) память |
| `SHR`     | Shared memory                   |
| `S`       | Статус: R, S, Z и т.д.          |
| `%CPU`    | Использование CPU               |
| `%MEM`    | Использование памяти            |
| `TIME+`   | Общее время работы CPU          |
| `COMMAND` | Запущенная команда              |

##### ⌨️ Полезные клавиши `top`

| Клавиша | Описание              |
| ------- | --------------------- |
| `P`     | Сортировать по CPU    |
| `M`     | Сортировать по памяти |
| `c`     | Полный путь команды   |
| `k`     | Убить процесс         |
| `r`     | Изменить приоритет    |
| `q`     | Выйти                 |

#### ✅ Вывод

`top` позволяет:

- Мониторить загрузку CPU, RAM и swap
- Определять активные и "тяжёлые" процессы
- Анализировать поведение системы в реальном времени

</p></details>

## Этап 2: Загрузка процессора

### 🔍 Команда `mpstat` (из `sysstat`)

```bash
sudo apt install sysstat
mpstat -P ALL 1 5
```

**На что смотреть:**

- `%idle` — чем ниже, тем сильнее загружен CPU
- `%usr`, `%sys` — пользовательская и системная нагрузка

## Этап 3: Использование памяти

### 🔍 Команды `free`, `vmstat`

```bash
free -h
```

**Важно:**

- `available` — сколько памяти реально доступно
- Если активно используется `swap` → нехватка RAM

```bash
vmstat 1 5
```

## Этап 4: Проверка дисковой активности

### 📂 Команды `iostat` и `iotop`

```bash
iostat -xz 1
```

**Показатели:**

- `util` > 70–80% → диск загружен
- `await` > 20–30 мс → медленные операции

```bash
sudo iotop
```

## Этап 5: Проверка сетевой активности

### 🌐 Команда `iftop` или `nload`

```bash
sudo apt install iftop nload
sudo iftop -i eth0
nload
```

**Что искать:**

- Подозрительные входящие/исходящие потоки
- Высокая загрузка интерфейса

## Этап 6: Проверка процессов и нагрузок

### 🔍 `ps`, `pidstat`, `strace`, `perf`

```bash
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
```

```bash
pidstat -p <PID> 1
```

Если процесс "завис" — можно проследить системные вызовы:

```bash
strace -p <PID>
```

## 🧠 Общая тактика

| Проверка            | Инструмент                           |
| ------------------- | ------------------------------------ |
| Загрузка CPU        | `top`, `htop`, `mpstat`              |
| Память              | `free`, `vmstat`                     |
| Диск                | `iostat`, `iotop`                    |
| Сеть                | `iftop`, `nload`                     |
| Конкретные процессы | `ps`, `pidstat`, `strace`, `lsof`    |
| Приложение / БД     | Логи, профилировщики, мониторинг APM |

## 💡 Вывод

> Всегда начинай с **общей картины**: CPU, память, диск, сеть.  
> Затем переходи к **конкретным процессам или сервисам**, используя `pidstat`, `strace`, `iotop`.  
> Хорошая диагностика = минимизация слепых зон.

</p></details>

---

### ❓ 5. В каких метриках измеряется нагрузка на жёсткий диск или файловую подсистему, и что они означают?

**[📺 Видео-вопрос (31:13)](https://youtu.be/z0QZ7VLyP7w?t=1873)**

<details><summary><strong>Показать ответ</strong></summary><p>

## 📊 Ключевые метрики и где их увидеть

| Метрика        | Где смотреть            | Обозначение                 | Что показывает                                           |
| -------------- | ----------------------- | --------------------------- | -------------------------------------------------------- |
| **IOPS**       | `iostat -x`, `fio`      | `r/s`, `w/s`                | Операции чтения/записи в секунду                         |
| **Throughput** | `iostat -x`, `iotop`    | `rkB/s`, `wkB/s`            | Пропускная способность (кБ/с)                            |
| **%util**      | `iostat -x`             | `%util`                     | Доля времени, когда устройство занято (≈100% = перегруз) |
| **await**      | `iostat -x`             | `await` (мс)                | Среднее время ожидания операции (очередь + обработка)    |
| **svctm**      | `iostat -x` (не всегда) | `svctm` (мс)                | Среднее время обработки (без очереди)                    |
| **avgqu-sz**   | `iostat -x`             | `avgqu-sz`                  | Средний размер очереди запросов                          |
| **avgqu/sz**   | `iostat -x`             | `aqu-sz` (на новых версиях) | То же, иной ярлык                                        |
| **%rd/%wr**    | `pidstat -d`, `iostat`  | —                           | Долю чтения/записи в нагрузке                            |
| **Latency**    | `fio`, `ioping`         | `clat`, `slat`, `lat`       | Задержки на операцию (мкс/мс)                            |

> Примечание: названия столбцов могут немного отличаться по версиям `sysstat`/ядра.

## 🧮 Разбор колонок `iostat -xz`

Пример:

```
Device            r/s   w/s  rkB/s  wkB/s avgrq-sz avgqu-sz await r_await w_await  svctm  %util
nvme0n1         120.0 80.0  40960  20480     512.0     1.25   6.2     5.1     7.9    0.8  16.0
sda              50.0 20.0   8192   4096     300.0     2.80  22.0    18.0    30.0    4.0  28.0
```

- **r/s, w/s** — IOPS по чтению/записи
- **rkB/s, wkB/s** — скорость (кБ/с)
- **avgrq-sz** — средний размер запроса (секторы/КБ)
- **avgqu-sz (aqu-sz)** — средняя длина очереди
- **await** — средняя задержка операции (очередь + обработка), мс
- **r_await / w_await** — задержка отдельно для чтения/записи
- **svctm** — среднее время обслуживания (не всегда достоверно)
- **%util** — занятость устройства (приближение к 100% = saturation)

## 🧭 Как интерпретировать (ориентиры)

> Оценки грубые; зависят от модели диска, контроллера, FS и паттерна нагрузки.

### HDD (7200 rpm)

- **await**: 10–20 мс — норма, >30–50 мс — уже ощутимые задержки
- **%util**: стабильно >80–90% → диск «упёрся»
- **IOPS**: десятки–сотни на случайных операциях
- **avgqu-sz**: растёт при очередях, часто из‑за мелких случайных запросов

### SATA SSD

- **await**: 0.1–5 мс — норма, >5–10 мс — тревожно
- **%util**: может быть высоким даже при невысоком await (контроллер держит очередь)
- **IOPS**: тысячи–десятки тысяч
- **Throughput**: сотни МБ/с

### NVMe SSD

- **await**: 0.02–1 мс — типично
- **IOPS**: десятки–сотни тысяч
- **avgqu-sz**: допускается выше (много очередей), смотрим на **await** и ошибки

**Общие сигналы проблемы:**

- **%util ≈ 100%** + растёт **await** → saturation, диск «бутылочное горлышко»
- Высокий **avgqu-sz** длительно → очередь на устройстве
- **rkB/s/wkB/s** упирается в паспорт → лимит пропускной способности
- Большая асимметрия между **r_await** и **w_await** → нагрузка/троттлинг/GC SSD

## 🧰 По процессам и файлам

Текущие «тяжёлые» процессы:

```bash
sudo iotop
# или
pidstat -d 1
```

Какие файлы открыты процессом:

```bash
sudo lsof -p <PID> | grep -iE 'REG|DIR' | head
```

Кто держит файл/устройство:

```bash
sudo lsof /dev/sda
```

## 📝 Мини‑чеклист диагностики диска

1. `iostat -xz 1` — смотрим `%util`, `await`, очередь, r/w баланс
2. `iotop` — кто грузит диск
3. `dmesg -w` — ошибки контроллера/таймауты
4. `smartctl -a /dev/<dev>` — здоровье диска
5. Планировщик I/O и mount‑опции FS
6. Сопоставляем с метриками приложения (latency) и БД

## 📌 Короткие выводы

- Меряйте **и скорость (MB/s), и IOPS, и задержку (await/latency)** — по одной метрике выводы делать опасно.
- **%util≈100%** с растущей задержкой → устройство насыщено.
- Для SSD/NVMe **главный индикатор** — задержка, а не только %util.

</p></details>

---

### ❓ 6. У какого диска больше iops у sdd или hdd?

**[📺 Видео-вопрос (32:20)](https://youtu.be/z0QZ7VLyP7w?t=1940)**

<details><summary><strong>Показать ответ</strong></summary><p>

### У **SSD** (твердотельного накопителя) IOPS всегда на порядки выше, чем у **HDD** (жёсткого диска).

### 📊 Сравнение по порядкам величин

| Тип накопителя         | IOPS (случайное чтение 4K) | Задержка (latency) |
| ---------------------- | -------------------------- | ------------------ |
| HDD 7200 rpm           | ~80–200                    | 5–12 мс            |
| HDD 15K rpm (серверн.) | ~200–400                   | 3–5 мс             |
| SATA SSD               | ~10 000–100 000            | 0.05–0.5 мс        |
| NVMe SSD               | ~100 000–1 000 000+        | 0.02–0.1 мс        |

### 🔍 Почему разница такая большая

- **HDD** ограничен механикой: головка перемещается и ждёт, пока сектор пройдёт под ней.
- **SSD** работает без движущихся частей — доступ к данным почти мгновенный.
- **NVMe SSD** использует параллельные очереди и быстрый интерфейс PCIe, что ещё увеличивает IOPS.

</p></details>

---

### ❓ 7. Что такое LVM

**[📺 Видео-вопрос (16:34)](https://youtu.be/)**

<details><summary><strong>Показать ответ</strong></summary><p>

**LVM** — это система управления логическими томами в Linux, позволяющая гибко работать с дисковым пространством без жёстких ограничений физических разделов.

## 🔹 Ключевые понятия

**Вместо того чтобы работать напрямую с разделами диска, LVM вводит дополнительный уровень абстракции**

- **PV (Physical Volume)** — физический том (диск или раздел, подготовленный для LVM).
- **VG (Volume Group)** — группа томов; объединяет несколько PV в общий пул.
- **LV (Logical Volume)** — логический том; создаётся внутри VG и монтируется как обычный раздел.
- **FS (File System)** — Форматируется поверх LV (например, ext4, xfs).

## 🔹 Как это работает

- При инициализации PV всё его пространство делится на одинаковые блоки фиксированного размера PE (обычно 4 MB, но можно указать свой при создании VG).
- Эти PE потом объединяются в Logical Extents (LE), которые принадлежат Logical Volumes (LV).

1. Каждый PV → набор PE.
2. VG объединяет PE с разных PV в общий пул.
3. LV получает набор LE, которые соответствуют PE один к одному (1 LE = 1 PE).
4. При расширении или уменьшении LV LVM просто добавляет или убирает PE.

## 📊 Преимущества LVM

- 📏 **Динамическое изменение размеров** томов без перезагрузки.
- ➕ **Объединение нескольких дисков** в единый пул.
- 🔄 **Снимки (snapshots)** для бэкапов или тестирования.
- ⚡ **Striping/Mirroring** — распределение или зеркалирование данных.
- 📂 Простое добавление новых дисков.

## 🖼 Структура

```
[ /dev/sda1 ] [ /dev/sdb1 ]  →  PV  →  VG "data_vg"
                                        ├─ LV "home_lv" (/home)
                                        └─ LV "var_lv"  (/var)
```

## 🛠 Основные команды

### Просмотр состояния

```bash
pvs   # физические тома
vgs   # группы томов
lvs   # логические тома
```

### Создание LVM

```bash
pvcreate /dev/sdb1                     # Помечает раздел /dev/sdb1 как Physical Volume (PV) для LVM.
vgcreate data_vg /dev/sdb1             # Создаёт Volume Group (VG) с именем data_vg.
lvcreate -L 50G -n home_lv data_vg     # Создаёт Logical Volume (LV) с именем home_lv внутри VG data_vg.
mkfs.ext4 /dev/data_vg/home_lv         # Создаёт файловую систему ext4 на логическом томе.
mount /dev/data_vg/home_lv /home       # Монтирует LV в каталог /home
```

### Добавление нового диска в VG

```bash
pvcreate /dev/sdc1
vgextend data_vg /dev/sdc1
```

### Расширение тома

```bash
lvextend -L +10G /dev/data_vg/home_lv
resize2fs /dev/data_vg/home_lv
```

### Уменьшение тома _(осторожно)_

```bash
umount /home
e2fsck -f /dev/data_vg/home_lv
resize2fs /dev/data_vg/home_lv 40G
lvreduce -L 40G /dev/data_vg/home_lv
mount /dev/data_vg/home_lv /home
```

### Снимки (snapshots)

```bash
lvcreate -L 1G -s -n snap_home /dev/data_vg/home_lv
mount /dev/data_vg/snap_home /mnt
```

## ⚠ Best Practices

- Перед уменьшением **обязательно** делайте бэкап и проверяйте файловую систему.
- Для снапшотов выделяйте место с запасом.
- Используйте `lvs -a -o +devices` для диагностики физических устройств в LV.
- В продакшене полезно мониторить использование VG (`vgs`) и заполненность снапшотов.

## 📚 Полезные материалы

- [man lvm](https://linux.die.net/man/8/lvm)

</p></details>

---

### ❓ 8. Чем отличается Physical Volume от Volume Group

**[📺 Видео-вопрос (34:07)](https://youtu.be/z0QZ7VLyP7w?t=2047)**

<details><summary><strong>Показать ответ</strong></summary><p>

## Разница между **Physical Volume (PV)** и **Volume Group (VG)** в LVM — это разница между «сырьём» и «контейнером».

## 📦 Physical Volume (PV) — физический том

- Это **базовый строительный блок** LVM.
- Представляет собой реальное блочное устройство:
  - целый диск (`/dev/sdb`)
  - или его раздел (`/dev/sdb1`)
  - или даже RAID-устройство (`/dev/md0`)
- На PV хранится **метадата LVM** и **Physical Extents** (PE) — одинаковые по размеру блоки (обычно 4 MB).
- PV **не используется напрямую** для монтирования — он служит основой для VG.

📌 Пример создания:

```bash
pvcreate /dev/sdb1
```

## 🗂 Volume Group (VG) — группа томов

- Это **пул хранения**, который объединяет один или несколько PV.
- VG — это как **виртуальный диск**, состоящий из всех PE всех PV внутри него.
- Внутри VG создаются **Logical Volumes (LV)** — именно они монтируются в файловую систему.
- Добавление нового PV в VG увеличивает доступное пространство.

📌 Пример создания:

```bash
vgcreate data_vg /dev/sdb1 /dev/sdc1
```

(здесь VG состоит из двух физических томов)

## 🔍 Аналогия

- **PV** → кирпич
- **VG** → стена, построенная из этих кирпичей
- **LV** → отдельные комнаты в стене, которые мы используем под конкретные нужды

</p></details>

---

### ❓ 9. Для чего в Linux директория /proc

**[📺 Видео-вопрос (36:46)](https://youtu.be/z0QZ7VLyP7w?t=2206)**

<details><summary><strong>Показать ответ</strong></summary><p>

В Linux директория **`/proc`** — это виртуальная файловая система **procfs**, которая предоставляет доступ к информации о ядре, процессах и состоянии системы **в реальном времени**.

Она не хранится на диске — файлы в `/proc` генерируются ядром на лету при чтении.

## 📌 Назначение `/proc`

- **Мониторинг системы** — получение данных о CPU, памяти, устройствах, процессах, сетевых соединениях.
- **Управление настройками ядра** — изменение параметров через файлы `/proc/sys/*`.
- **Отладка** — просмотр статистики, состояния процессов, системных лимитов.

## 📂 Полезные файлы и директории

| Путь                | Описание                                  |
| ------------------- | ----------------------------------------- |
| `/proc/cpuinfo`     | Информация о процессоре                   |
| `/proc/meminfo`     | Состояние оперативной памяти              |
| `/proc/uptime`      | Время работы системы и время простоя      |
| `/proc/loadavg`     | Средняя нагрузка системы (1, 5, 15 минут) |
| `/proc/mounts`      | Список смонтированных файловых систем     |
| `/proc/partitions`  | Список разделов                           |
| `/proc/modules`     | Загруженные модули ядра                   |
| `/proc/sys`         | Настройки ядра (доступны для изменения)   |
| `/proc/filesystems` | Список поддерживаемых файловых систем     |
| `/proc/<PID>/`      | Информация о процессе с указанным PID     |

## 🔍 Примеры использования

**Информация о CPU:**

```bash
cat /proc/cpuinfo
```

**Состояние памяти:**

```bash
cat /proc/meminfo
```

**Средняя нагрузка системы:**

```bash
cat /proc/loadavg
```

**Изменение параметров ядра:**

```bash
# Включить IPv4 forwarding
echo 1 > /proc/sys/net/ipv4/ip_forward
# Или через sysctl
sysctl -w net.ipv4.ip_forward=1
```

## 💡 Особенности

- `/proc` обновляется в реальном времени — данные показывают текущее состояние системы.
- Многие утилиты (`top`, `ps`, `free`, `uptime`) используют `/proc` для получения данных.
- Запись в некоторые файлы `/proc` меняет параметры работы ядра — будьте осторожны.

## 📚 Дополнительно

- `man proc` — официальная документация по содержимому `/proc`.

</p></details>

---

### ❓ 10. Какими способами в Linux выполняется настройка и оптимизация (тюнинг) системы, и через какие инструменты или конфигурационные файлы это делается?

**[📺 Видео-вопрос (38:25)](https://youtu.be/z0QZ7VLyP7w?t=2305)**

<details><summary><strong>Показать ответ</strong></summary><p>

**В Linux оптимизация (тюнинг) системы выполняется разными методами: изменение параметров ядра, настройка служб, управление приоритетами, оптимизация сети, файловых систем и т.д**

## 1️⃣ Изменение параметров ядра

Через `sysctl`:

```bash
sysctl -w net.ipv4.ip_forward=1   # Включаем пересылку IPv4 пакетов (роутинг)
```

- `sysctl -w` — временно меняет параметр ядра до перезагрузки
- `net.ipv4.ip_forward=1` — разрешает пересылку пакетов между интерфейсами

Постоянно — через `/etc/sysctl.conf` или `/etc/sysctl.d/*.conf`:

```conf
net.core.somaxconn = 1024         # Размер очереди ожидающих подключений (TCP backlog)
net.ipv4.tcp_fin_timeout = 15     # Время ожидания закрытия TCP-соединения (FIN-WAIT-2)
```

Применить без перезагрузки:

```bash
sysctl -p                          # Загружает параметры из /etc/sysctl.conf
```

## 2️⃣ Настройка служб и демонов

Редактируем конфиги в `/etc/` (например, для nginx `/etc/nginx/nginx.conf`):

```nginx
worker_processes auto;             # Количество процессов воркеров = число ядер CPU
worker_connections 4096;           # Максимум одновременных соединений на процесс
keepalive_timeout 15;              # Таймаут keep-alive соединений
```

Перезапускаем сервис:

```bash
sudo systemctl restart nginx       # Применяем новые настройки nginx
```

## 3️⃣ Управление приоритетами процессов

```bash
nice -n 10 myscript.sh             # Запуск скрипта с пониженным приоритетом (nice = 10)
renice -n -5 -p 1234                # Изменение приоритета процесса PID=1234 на более высокий
ionice -c 2 -n 0 -p 1234            # Наивысший приоритет ввода/вывода для PID=1234
```

## 4️⃣ Оптимизация сети

Настройка размера очередей сетевого интерфейса:

```bash
ethtool -G eth0 rx 4096 tx 4096     # Увеличить буферы приема (rx) и передачи (tx) пакетов
```

Проверка и настройка offload-параметров:

```bash
ethtool -k eth0                     # Показать текущие offload-настройки
ethtool -K eth0 tso off gso off     # Выключить сегментацию TCP/GSO (иногда снижает задержки)
```

## 5️⃣ Оптимизация файловых систем

Монтажные опции в `/etc/fstab`:

```fstab
/dev/sdb1 /data ext4 defaults,noatime,nodiratime 0 2
# noatime     — не обновлять время доступа к файлу (ускоряет работу)
# nodiratime  — не обновлять время доступа к каталогам
```

Настройка ext4:

```bash
tune2fs -o journal_data_writeback /dev/sdb1   # Режим журналирования: writeback (быстрее, но менее надёжно)
```

## 6️⃣ Работа с cgroups и limits

Ограничение ресурсов для процесса через systemd:

```bash
systemd-run --scope -p MemoryMax=500M sleep 100
# --scope                — запуск команды в отдельной области systemd
# -p MemoryMax=500M      — ограничить потребление памяти 500 мегабайтами
# sleep 100              — тестовая команда (100 секунд сна)
```

Лимиты в `/etc/security/limits.conf`:

```conf
myuser soft nofile 4096             # Мягкий лимит открытых файлов для пользователя
myuser hard nofile 8192             # Жёсткий лимит открытых файлов
```

## 7️⃣ Профилирование и анализ

```bash
top                                  # Мониторинг процессов и загрузки CPU/RAM
iotop                                # Мониторинг ввода-вывода по процессам
pidstat -d 1                         # Статистика I/O каждые 1 сек
perf stat -p 1234                    # Сбор статистики работы процесса PID=1234
```

## 📌 Итог

В Linux тюнинг делается через:

- `/proc` и `/sys` (виртуальные файловые системы ядра)
- `sysctl` для параметров ядра
- конфиги служб в `/etc/`
- приоритеты процессов (`nice`, `ionice`)
- сетевые настройки (`ethtool`)
- опции монтирования и `tune2fs`
- ограничения через cgroups и limits

Настройку всегда делают **после анализа метрик**, чтобы избежать случайного ухудшения работы системы.

</p></details>

---

### ❓ 11. Какие бывают коды возврата (exit codes) в Linux, что они означают и в каких случаях используются?

**[📺 Видео-вопрос (39:05)](https://youtu.be/z0QZ7VLyP7w?t=2345)**

<details><summary><strong>Показать ответ</strong></summary><p>

- В Linux **код возврата** (exit code, return code) — это целое число, которое процесс или команда возвращает в момент завершения.  
  Этот код сообщает вызывающей программе или оболочке, чем завершилось выполнение: успешно или с ошибкой, и какой именно была ошибка.

## 📌 Общие принципы

- **0** → успех (команда выполнена без ошибок)
- **≠0** → ошибка (значение зависит от программы)
- Проверка кода последней команды:

```bash
echo $?   # Показывает exit code предыдущей команды
```

## 📊 Часто встречающиеся коды в Linux/Unix

| Код     | Значение                                      | Когда возникает                           |
| ------- | --------------------------------------------- | ----------------------------------------- |
| **0**   | Успешное завершение                           | Команда выполнилась без ошибок            |
| **1**   | Общая ошибка                                  | Неверный аргумент, сбой логики            |
| **2**   | Неверные аргументы (Misuse of shell builtins) | Некорректный синтаксис, неверный параметр |
| **126** | Команда найдена, но не может быть выполнена   | Нет прав на выполнение (`chmod +x`)       |
| **127** | Команда не найдена                            | Ошибка пути (`command not found`)         |
| **128** | Неверный аргумент для `exit`                  | Некорректное завершение                   |
| **130** | Прервано `Ctrl+C` (SIGINT)                    | Пользователь остановил выполнение         |
| **137** | Завершено сигналом `SIGKILL` (kill -9)        | Принудительное завершение                 |
| **139** | Segmentation fault (сигнал 11)                | Ошибка доступа к памяти                   |
| **143** | Завершено сигналом `SIGTERM`                  | Корректное завершение по запросу          |

## 🔹 Особенности

- Диапазон кодов — от **0 до 255** (всё, что выше, интерпретируется по модулю 256).
- Коды >128 обычно означают завершение по сигналу:
  ```
  код = 128 + номер_сигнала
  ```
  Например: `SIGTERM` (15) → 128+15=143.

## 🔍 Примеры

```bash
ls /tmp
echo $?    # 0 — директория есть

ls /notfound
echo $?    # 2 или другой код ошибки — директория не найдена
```

## 💡 В скриптах

В Bash можно явно задать код завершения:

```bash
#!/bin/bash
if [ "$1" == "" ]; then
  echo "Нет аргументов"
  exit 1   # Возвращаем ошибку
fi
echo "OK"
exit 0     # Успешно
```

</p></details>

---

### ❓ 12. Какие бывают сигналы?

**[📺 Видео-вопрос (39:05)](https://youtu.be/)**

<details><summary><strong>Показать ответ</strong></summary><p>

## 📋 Основные сигналы (ядро/пользователь/терминал)

> **Действие по умолчанию**: `Term` — завершить, `Core` — завершить и создать core dump, `Stop` — приостановить, `Cont` — продолжить.  
> **Перехватываемость**: большинство можно перехватить/проигнорировать, кроме `SIGKILL` и `SIGSTOP`.

| №   | Имя      | Действие по умолчанию | Типичное назначение / комментарии                                                                                                     | Можно перехватить?       | Пример отправки     |
| --- | -------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------------------------ | ------------------- |
| 1   | SIGHUP   | Term                  | Исторически «разрыв TTY»; часто = **reload конфигурации** у демонов (nginx, sshd, haproxy, prometheus)                                | Да                       | `kill -HUP <PID>`   |
| 2   | SIGINT   | Term                  | Прерывание с терминала (**Ctrl+C**)                                                                                                   | Да                       | `kill -INT <PID>`   |
| 3   | SIGQUIT  | Core                  | Прерывание с дампом (\*\*Ctrl+\*\*); отладка (core dump)                                                                              | Да                       | `kill -QUIT <PID>`  |
| 9   | SIGKILL  | Term                  | **Немедленное убийство**; не даёт обработать завершение; крайняя мера                                                                 | **Нет**                  | `kill -9 <PID>`     |
| 15  | SIGTERM  | Term                  | **Корректное завершение** (graceful shutdown). Используется `systemctl stop`, Kubernetes при удалении пода отправляет сначала SIGTERM | Да                       | `kill -TERM <PID>`  |
| 18  | SIGCONT  | Cont                  | **Возобновить** процесс после остановки (парой к SIGSTOP/SIGTSTP)                                                                     | Нет смысла перехватывать | `kill -CONT <PID>`  |
| 19  | SIGSTOP  | Stop                  | **Приостановить** процесс; не перехватывается (ядром)                                                                                 | **Нет**                  | `kill -STOP <PID>`  |
| 20  | SIGTSTP  | Stop                  | Приостановка с терминала (**Ctrl+Z**)                                                                                                 | Да                       | `kill -TSTP <PID>`  |
| 10  | SIGUSR1  | Term                  | Пользовательский сигнал №1 — свободная семантика (часто: перезапуск воркеров)                                                         | Да                       | `kill -USR1 <PID>`  |
| 12  | SIGUSR2  | Term                  | Пользовательский сигнал №2 — свободная семантика                                                                                      | Да                       | `kill -USR2 <PID>`  |
| 11  | SIGSEGV  | Core                  | Ошибка сегментации (доступ к памяти)                                                                                                  | Нет (по сути)            | `kill -SEGV <PID>`  |
| 13  | SIGPIPE  | Term                  | Запись в закрытый pipe/сокет                                                                                                          | Да                       | `kill -PIPE <PID>`  |
| 14  | SIGALRM  | Term                  | Сигнал от таймера `alarm()`                                                                                                           | Да                       | `kill -ALRM <PID>`  |
| 24  | SIGXCPU  | Term                  | Превышение лимита CPU (ulimit)                                                                                                        | Да                       | `kill -XCPU <PID>`  |
| 25  | SIGXFSZ  | Term                  | Превышение лимита размера файла                                                                                                       | Да                       | `kill -XFSZ <PID>`  |
| 17  | SIGCHLD  | Ignore                | Дочерний процесс завершился; обработка `wait()`                                                                                       | Да                       | `kill -CHLD <PID>`  |
| 31  | SIGSYS   | Core                  | Неверный системный вызов                                                                                                              | Да                       | `kill -SYS <PID>`   |
| 27  | SIGWINCH | Ignore                | Изменение размера окна терминала                                                                                                      | Да                       | `kill -WINCH <PID>` |

> Для полной таблицы см. `kill -l` и `man 7 signal` (зависят от платформы/архитектуры).

## 🔁 Reload против Restart: где применяется `SIGHUP`

- Многие демоны трактуют **`SIGHUP` как reload конфигурации**: nginx, sshd, HAProxy, Prometheus и др.  
  Пример для nginx:
  ```bash
  kill -HUP $(cat /var/run/nginx.pid)   # перечитать конфиг без разрыва активных соединений
  # то же через systemd:
  systemctl reload nginx
  ```
- Если сервис **не поддерживает** `SIGHUP`, он завершится по умолчанию (Term). Проверяйте документацию.

## 🧯 Корректное vs жёсткое завершение: SIGTERM и SIGKILL

- **SIGTERM (15)** — мягкое завершение: приложение может закрыть соединения, слить буферы, сохранить состояние.
  - В Kubernetes: при удалении пода → `SIGTERM`, ожидание `terminationGracePeriodSeconds` (по умолчанию 30s) → затем **`SIGKILL`**, если не успело завершиться.
- **SIGKILL (9)** — жёсткое: немедленный останов, **без** возможности обработки. Риск потери данных, «сиротских» процессов, мусора во временных каталогах. Использовать как **крайнюю меру**.

## ⏸️ Диагностика без убийства: SIGSTOP / SIGCONT / SIGTSTP

- `SIGSTOP` — мгновенно **замораживает** процесс (ядром, не перехватывается).
- `SIGCONT` — **возобновляет**.
- `SIGTSTP` — «пользовательская» остановка с терминала (`Ctrl+Z`), может быть перехвачена.  
  Полезно при расследовании «пожирателей» CPU/RAM: остановить → снять `strace`, `gcore`, посмотреть дескрипторы → запустить дальше.

## 🔢 Связь сигналов с кодами возврата

При завершении по сигналу оболочки обычно возвращают код:

```
exit_code = 128 + номер_сигнала
```

Примеры:

| Сигнал  | Номер | Типичный exit‑code |
| ------- | ----- | ------------------ |
| SIGTERM | 15    | 143                |
| SIGKILL | 9     | 137                |
| SIGINT  | 2     | 130                |
| SIGQUIT | 3     | 131                |
| SIGSEGV | 11    | 139                |

Проверка кода последней команды:

```bash
echo $?    # показать exit code
```

## 🛠 Быстрые команды

```bash
kill -l                       # список сигналов
kill -TERM <PID>              # корректно завершить
kill -KILL <PID>              # принудительно убить
kill -HUP  <PID>              # (часто) перечитать конфиг
kill -STOP <PID>; kill -CONT <PID>   # остановить и возобновить
```

## ✅ Рекомендации

- Начинайте с **SIGTERM**, а не с `-9`.
- Для **reload** используйте `SIGHUP` или `systemctl reload` (если поддерживается).
- Для диагностики перегруженных процессов применяйте `SIGSTOP`/`SIGCONT` вместо убийства.
- Мониторьте **exit‑коды** и логируйте причину завершения: помогает в автозапуске/оркестрации.

</p></details>

---

### ❓ 13. Зачем в начале Bash скрипта пишут set -e?

**[📺 Видео-вопрос (43:40)](https://youtu.be/z0QZ7VLyP7w?t=2620)**

<details><summary><strong>Показать ответ</strong></summary><p>

Флаг **`-e`** (или команда `set -e`) говорит Bash:

> _«Выйти из скрипта, если любая команда вернётся с ненулевым exit code»_ (т.е. если произошла ошибка).

## 🔍 Пример с `-e`

```bash
#!/bin/bash -e
echo "Start"
ls /nonexistent
echo "This will NOT run"
```

**Вывод:**

```
Start
ls: cannot access '/nonexistent': No such file or directory
```

Скрипт завершился сразу после ошибки.

## 💡 Зачем использовать

- Для **безопасности**: чтобы не продолжать выполнение, если что-то пошло не так (важно при автоматизации).
- Особенно полезно в деплоях, бэкапах, миграциях, чтобы не испортить данные из-за пропущенной ошибки.

## ⚠️ Подводные камни

- `-e` реагирует **только на ненулевые коды возврата**, поэтому команды, которые возвращают ошибку в обычных условиях, могут преждевременно завершить скрипт.
- Если ожидается, что команда может вернуть ошибку, нужно:
  ```bash
  some_command || true
  ```
  или
  ```bash
  set +e
  some_command
  set -e
  ```

</p></details>

---

### ❓ 14. Что такое переменные окружения и как их посмотреть?

**[📺 Видео-вопрос (44:36)](https://youtu.be/z0QZ7VLyP7w?t=2676)**

<details><summary><strong>Показать ответ</strong></summary><p>

- **Переменные окружения** — это пары _ключ=значение_, которые хранят конфигурационные данные, доступные процессам и используемые для их работы.

## 📌 Что определяют переменные окружения

- Пути поиска исполняемых файлов (`PATH`)
- Текущего пользователя (`USER`)
- Домашний каталог (`HOME`)
- Локаль (`LANG`)
- Специфичные для приложений параметры (`JAVA_HOME`, `DATABASE_URL` и др.)

## 🔹 Особенности

- Наследуются дочерними процессами от родительского.
- Можно задать временно для одной команды:
  ```bash
  VAR=value команда
  ```
- Можно задать глобально в текущей сессии:
  ```bash
  export VAR=value
  ```

## 🔍 Как посмотреть переменные окружения

### 1. Все переменные окружения

```bash
printenv
```

или

```bash
env
```

### 2. Конкретная переменная

```bash
echo $PATH
echo $HOME
```

### 3. Все переменные shell (включая окружение)

```bash
set
```

## 📂 Где задаются переменные окружения

- В shell-конфигурации пользователя:
  - `~/.bashrc`
  - `~/.bash_profile`
  - `~/.zshrc`
- В системных конфигурациях:
  - `/etc/environment`
  - `/etc/profile`
- В unit-файлах systemd:
  - Директива `Environment=`
  - Директива `EnvironmentFile=`
- В скриптах запуска сервисов

## 🔄 Локальные vs экспортированные переменные

- **Локальные** — видны только в текущем shell:

  ```bash
  MYVAR=123
  echo $MYVAR       # 123
  bash -c 'echo $MYVAR'  # (пусто)
  ```

- **Экспортированные** (`export`) — видны дочерним процессам:
  ```bash
  export MYVAR=123
  bash -c 'echo $MYVAR'  # 123
  ```

## 💾 Как сделать переменную постоянной

Добавить в файл конфигурации shell:

```bash
echo 'export MYVAR=123' >> ~/.bashrc
source ~/.bashrc
```

Для всех пользователей:

```bash
echo 'MYVAR=123' | sudo tee -a /etc/environment
```

## 📌 Итог

- Переменные окружения управляют поведением процессов и конфигурацией приложений.
- Просмотр — `printenv`, `env`, `echo $VAR`.
- Постоянное хранение — в `~/.bashrc`, `/etc/environment`, unit-файлах systemd.

</p></details>

---

### ❓ 15. Какие бывают альтернативы bash?

**[📺 Видео-вопрос (46:53)](https://youtu.be/z0QZ7VLyP7w?t=2813)**

<details><summary><strong>Показать ответ</strong></summary><p>

## Альтернативы Bash

Существует множество альтернатив оболочке `bash`. Ниже приведены популярные шеллы, их особенности, плюсы и минусы.

## 1. Zsh (Z Shell)

- **Популярность**: Очень высокая
- **Особенности**:
  - Расширенное автодополнение (по истории, аргументам команд).
  - Поддержка плагинов и тем (например, через [Oh My Zsh](https://ohmyz.sh)).
  - Совместим с `bash`.
  - Используется по умолчанию в macOS (начиная с Catalina).
- **Рекомендуется для**: Повседневного использования.

## 2. Fish (Friendly Interactive SHell)

- **Популярность**: Растёт, особенно среди новичков
- **Особенности**:
  - Цветное выделение синтаксиса в реальном времени.
  - Умное автодополнение без `Tab`.
  - Простой и понятный синтаксис.
  - Встроенные подсказки и справка.
- **Минус**: Не совместим с POSIX — не подходит для переносимых скриптов.
- **Рекомендуется для**: Интерактивного использования.

## 3. Ksh (KornShell)

- **Разработчик**: David Korn (Bell Labs)
- **Особенности**:
  - Совместим с `sh`, но с расширениями (массивы, улучшенные функции).
  - Используется в старых Unix-системах.
  - Более строгий, чем `bash`.
- **Варианты**: `ksh88`, `ksh93`, `pdksh`, `mksh`.

## 4. Tcsh / Csh (C Shell)

- **Особенности**:
  - Синтаксис похож на язык C.
  - Есть история команд и автодополнение (в `tcsh`).
- **Минусы**:
  - Плохо подходит для написания скриптов.
  - Не рекомендуется для автоматизации.
- **Где используется**: В основном в академических средах.

## 5. Dash (Debian Almquist Shell)

- **Особенности**:
  - Очень быстрый и лёгкий.
  - Полностью POSIX-совместим.
  - Используется как `/bin/sh` в Debian/Ubuntu.
- **Не для интерактива**: Нет автодополнения, истории.
- **Рекомендуется для**: Написания переносимых скриптов.

## 6. Ion

- **Написан на**: Rust
- **Особенности**:
  - Современная, безопасная оболочка.
  - Быстрая, с хорошей обработкой ошибок.
  - Чистый синтаксис.
- **Цель**: Альтернатива `bash` с акцентом на безопасность и производительность.

## 7. Nushell (nu)

- **Подход**: Табличная оболочка (работает с данными как с таблицами)
- **Особенности**:
  - Передача структурированных данных в пайпах.
  - Современный синтаксис.
  - Написана на Rust.
- **Минус**: Не совместим с `bash`.
- **Рекомендуется для**: Современных пользователей, любящих структурированные данные.

## 8. Elvish

- **Особенности**:
  - Интерактивная оболочка с функциональным стилем.
  - Структурированные данные в пайпах.
  - Встроенный менеджер истории, автодополнение, модули.
- **Не совместим с bash**.
- **Цель**: Удобство + мощь скриптов.

## 9. PowerShell

- **Особенности**:
  - Объектно-ориентированный (работает с объектами, а не строками).
  - Доступен на Windows, Linux, macOS.
  - Отлично подходит для администрирования.
  - Язык на основе .NET.
- **Минус**: Не POSIX-совместим.
- **Рекомендуется для**: Системного администрирования и автоматизации.

## 📊 Сравнение оболочек

| Shell                   | Для чего хорош                  | Совместимость   | Особенности/плюсы                                                   | Минусы                                           |
| ----------------------- | ------------------------------- | --------------- | ------------------------------------------------------------------- | ------------------------------------------------ |
| **Zsh**                 | Ежедневная интерактивная работа | Почти Bash      | Мощное автодоп., расширенные glob’ы, плагины (`oh-my-zsh`, `zinit`) | Для скриптов лучше писать в POSIX-стиле или bash |
| **Fish**                | Удобство «из коробки»           | Не Bash/POSIX   | Подсказки по истории, автодополнение, простая конфигурация          | Скрипты fish несовместимы с sh/bash              |
| **Dash**                | Быстрый `/bin/sh`               | POSIX sh        | Очень быстрый старт, минимализм; часто `/bin/sh` в Debian/Ubuntu    | Мало фич для интерактива                         |
| **Ksh** (ksh93, mksh)   | Скриптинг, серверы              | POSIX sh        | Богатый язык, стабильность, хорош на проде                          | Реже встречается, нюансы синтаксиса              |
| **BusyBox ash**         | Встраиваемые системы            | POSIX sh        | Лёгкий, экономит ресурсы                                            | Ограниченный функционал                          |
| **Tcsh/csh**            | Исторический, некоторые среды   | Не POSIX sh     | Удобен для некоторых legacy-скриптов                                | Непредсказуем для продакшн-скриптов              |
| **PowerShell (`pwsh`)** | Кросс-платф. админка            | Не sh           | Объектный конвейер, хорошие модули                                  | Иной менталитет, тяжелее                         |
| **Nushell**             | Современный конвейер            | Не sh           | Табличные/структурные данные, типы                                  | Молодой проект, несовм. со sh                    |
| **Elvish**              | Эксперим., интерактив           | Не sh           | Надёжный конвейер, коллекции, модули                                | Небольшая экосистема                             |
| **Oil Shell** (osh/ysh) | Переосмысление sh               | Совместим. слой | Улучшения синтаксиса, безопаснее                                    | В разработке                                     |
| **Xonsh**               | Python-ориент. шелл             | Не sh           | Python + shell в одном                                              | Тяжелее, нишевый                                 |

## ✅ Рекомендации

- **Для повседневного использования**: `Zsh` + `Oh My Zsh`
- **Для новичков**: `Fish`
- **Для скриптовой переносимости**: `Dash` или `sh`
- **Для современного подхода**: `Nushell` или `Elvish`
- **Для администрирования**: `PowerShell`

## 💡 Полезные команды

```bash
echo $SHELL            # Текущий шелл
chsh -s /bin/zsh       # Сменить логин-шелл (после relogin)
which zsh fish dash    # Проверить наличие шеллов
```

</p></details>

---

### ❓ 16. Какая разница между одинарными [ и двойными [[ в bash?

**[📺 Видео-вопрос (47:54)](https://youtu.be/z0QZ7VLyP7w?t=2874)**

<details><summary><strong>Показать ответ</strong></summary><p>

## 📌 Основные различия

### 1. **`[ ]` (одинарные квадратные скобки)** — старый синтаксис

- Это **команда** в Unix, которая является стандартом для **POSIX**.
- Внутри **`[`** все операторы должны быть **строго разделены пробелами**.
- Не поддерживает расширенные возможности Bash, такие как регулярные выражения или составные условия.
- Это **совместимая с POSIX команда**, работает в любой оболочке, поддерживающей стандарт POSIX (`sh`, `dash`, и др.).

#### Пример с `[ ]`:

```bash
#!/bin/bash

a=5
if [ $a -gt 3 ]; then
    echo "a больше 3"
else
    echo "a не больше 3"
fi
```

- **Примечание**: пробелы обязательны, например, между переменной и операторами (`$a -gt 3`).
- При ошибке в синтаксисе (например, отсутствие пробела) будет ошибка.

### 2. **`[[ ]]` (двойные квадратные скобки)** — улучшенный синтаксис для Bash

- Это **расширение** команды, специфичное для Bash, которое поддерживает более **мощный синтаксис** и **более удобную работу с условиями**.
- Поддерживает **составные условия**, **регулярные выражения**, логические операторы (например, `&&` и `||`), и **работает гибче с пробелами и кавычками**.
- Рекомендуется использовать в скриптах, написанных для **Bash**, если не требуется совместимость с другими оболочками.

#### Пример с `[[ ]]`:

```bash
#!/bin/bash

a=5
if [[ $a -gt 3 && $a -lt 10 ]]; then
    echo "a больше 3 и меньше 10"
else
    echo "a не попадает в диапазон"
fi
```

- В **`[[`** не нужно экранировать пробелы или скобки внутри условия.
- Поддерживаются **регулярные выражения**:
  ```bash
  string="hello"
  if [[ $string =~ h.*o ]]; then
    echo "Строка соответствует регулярному выражению"
  fi
  ```

## 📊 Сравнение `[ ]` и `[[ ]]`

| Характеристика                   | `[ ]` (одинарные скобки)                            | `[[ ]]` (двойные скобки)               |
| -------------------------------- | --------------------------------------------------- | -------------------------------------- | --- | --- |
| Совместимость с POSIX            | Да                                                  | Нет (специфично для Bash)              |
| Поддержка регулярных выражений   | Нет                                                 | Да                                     |
| Логические операторы             | Поддерживает только `-a`, `-o`                      | Поддерживает `&&` и `                  |     | `   |
| Экранирование пробелов           | Требуется экранировать кавычки и пробелы            | Легко работает с пробелами и кавычками |
| Преимущества                     | Простота, совместимость с другими оболочками        | Гибкость и мощные возможности          |
| Работа с переменными и кавычками | Требует особого внимания к пробелам и экранированию | Гибкость в обработке переменных        |

## 📝 Пример с экранированием и регулярными выражениями в `[[ ]]`

```bash
#!/bin/bash
# Использование регулярных выражений в Bash через [[ ]]

string="hello world"
if [[ $string =~ ^hello ]]; then
  echo "Строка начинается с 'hello'"
fi
```

В Bash это будет работать, но в других оболочках (например, в `sh` или `dash`) регулярные выражения с `[[ ]]` не будут поддерживаться.

## ✅ Рекомендации

- Используйте **`[[ ]]`** в **Bash** для гибкости и мощных возможностей, таких как регулярные выражения, составные условия, и поддержка логических операторов.
- Если нужен **POSIX-совместимый скрипт** для работы в других оболочках, используйте **`[ ]`**.

</p></details>

---

### ❓ 17. Какими команднами можно посмотреть процессы если это не top

**[📺 Видео-вопрос (01:02:44)](https://youtu.be/z0QZ7VLyP7w?t=3764)**

<details><summary><strong>Показать ответ</strong></summary><p>

- В Linux существует несколько команд, которые могут быть использованы для просмотра процессов, если **`top`** не является предпочтительным инструментом.

## 1. 🖥️ **`ps`** — Статическая информация о процессах

Команда **`ps`** выводит информацию о текущих процессах. Она предоставляет множество опций для настройки вывода.

- **Простой список процессов:**

  ```bash
  ps
  ```

  Покажет процессы, запущенные текущим пользователем.

- **Полный список всех процессов:**

  ```bash
  ps aux
  ```

  Покажет список всех процессов, включая процессы других пользователей.

- **Просмотр процессов с древовидной структурой:**
  ```bash
  ps axjf
  ```
  Покажет процессы в виде иерархии (дерева).

## 2. 🔍 **`pgrep`** — Поиск процессов по имени

Если вы хотите найти процессы, связанные с определенной программой, используйте **`pgrep`**:

```bash
pgrep <имя_процесса>
```

**Пример:** Найти все процессы, связанные с `nginx`:

```bash
pgrep nginx
```

## 3. 🌲 **`pstree`** — Древовидное отображение процессов

**`pstree`** отображает процессы в виде дерева, показывая иерархию их родительских и дочерних связей.

```bash
pstree
```

- Для отображения **PID** процессов:
  ```bash
  pstree -p
  ```

## 4. 🔄 **`watch`** — Постоянное обновление команд

С помощью **`watch`** можно обновлять вывод команд в реальном времени, например, для регулярного обновления списка процессов.

```bash
watch -n 1 'ps aux'
```

Эта команда будет обновлять список процессов каждую секунду.

## 📑 **Итог:**

- **`ps`** — для статического списка процессов.
- **`htop`** — для удобного, интерактивного просмотра.
- **`pgrep`** — для поиска процессов по имени.
- **`pstree`** — для отображения процессов в виде дерева.
- **`watch`** — для постоянного обновления выводимых данных.

Каждая из этих команд может быть полезна для мониторинга процессов в Linux без использования **`top`**.

</p></details>

---

### ❓ 18. Если другой пользователь запустит процессы в Docker, а я работаю от root и выполню ps aux, будут ли видны эти докер-процессы?

**[📺 Видео-вопрос (01:03:39)](https://youtu.be/z0QZ7VLyP7w?t=3819)**

<details><summary><strong>Показать ответ</strong></summary><p>

- Если вы запускаете команду **`ps aux`** от имени пользователя **`root`**, то вы сможете увидеть **все процессы**, запущенные на системе, включая те, которые были запущены другими пользователями, например, процессы, запущенные через Docker.

## 🔍 **Что такое `ps aux`?**

Команда **`ps aux`** выводит **все процессы** в системе, включая процессы, запущенные другими пользователями.

- **`ps`** — это команда для отображения информации о процессах.
- **`a`** — отображает процессы всех пользователей.
- **`u`** — показывает владельца процессов.
- **`x`** — отображает процессы, не привязанные к терминалу.

## 👨‍💻 **Что можно увидеть, если вы работаете от root?**

Когда вы запускаете команду **`ps aux`** с правами **root**, вы сможете увидеть **все процессы**, независимо от того, кто их запустил. Это включает:

- Процессы, запущенные другими пользователями.
- Процессы, связанные с Docker, если они запущены другим пользователем.

## 🔑 **Пример:**

Если какой-то пользователь, например **`user1`**, запустил процесс Docker, вы сможете увидеть этот процесс в выводе команды **`ps aux`**, если вы выполните её от **root**.

```bash
ps aux | grep docker
```

Эта команда покажет все процессы, связанные с Docker, включая их PID, владельца и другие параметры.

## 🎯 **Итог:**

Да, если вы работаете от **root**, вы сможете увидеть все процессы на системе, включая те, которые запущены другими пользователями, например, процессы Docker.

</p></details>

---

# 3. Docker

### ❓ 1. В container отличается от image

**[📺 Видео-вопрос (49:11)](https://youtu.be/z0QZ7VLyP7w?t=2951)**

<details><summary><strong>Показать ответ</strong></summary><p>

## 1. 🖼️ **Образ (Image)**

Образ — это статичный файл, который содержит все необходимые данные для запуска контейнера, такие как:

- Операционная система
- Программное обеспечение
- Библиотеки
- Зависимости
- Программы

📝 **Особенность**: Образ является шаблоном для создания контейнеров. Он **не изменяется** во время работы и может быть использован для создания множества идентичных контейнеров.

## 2. 🔲 **Контейнер (Container)**

Контейнер — это запущенная инстанция (экземпляр) образа. Контейнер использует образ как основу, но в отличие от образа, он может быть изменен во время работы, например:

- Изменение файлов
- Установка программного обеспечения
- Создание новых файлов или настроек

🔒 **Особенность**: Контейнеры изолированы друг от друга и от хостовой операционной системы.

## 🔹 **Основные различия:**

| Характеристика   | Образ (Image)                      | Контейнер (Container)                           |
| ---------------- | ---------------------------------- | ----------------------------------------------- |
| **Изменяемость** | ❌ Неизменен                       | ✔️ Может изменяться во время работы             |
| **Цель**         | 📦 Шаблон для создания контейнеров | 🏃 Рабочий экземпляр образа                     |
| **Хранение**     | 💾 Может быть сохранен и перенесен | ❌ Пропадает после остановки (если не сохранен) |

## ✨ **Итог:**

Образ — это шаблон для создания контейнеров, а контейнер — это активная единица, которая выполняется на основе образа.

</p></details>

---

### ❓ 2. В чем преимущества контейнеризации по сравнению с виртуализацией

**[📺 Видео-вопрос (49:26)](https://youtu.be/z0QZ7VLyP7w?t=2966)**

<details><summary><strong>Показать ответ</strong></summary><p>

- Контейнеризация и виртуализация — это две технологии для изоляции и управления приложениями, но у каждой из них есть свои особенности и преимущества.

# 🐳 Контейнеризация vs 💾 Виртуализация

Разница между **контейнеризацией** и **виртуализацией** — ключевой аспект современных IT-инфраструктур. Оба подхода позволяют изолировать приложения, но делают это по-разному.

---

## 📊 Сравнительная таблица

| Характеристика           | **Виртуализация**                         | **Контейнеризация**                    |
| ------------------------ | ----------------------------------------- | -------------------------------------- |
| **Уровень абстракции**   | Аппаратное обеспечение (Hardware)         | Операционная система (OS)              |
| **Что виртуализируется** | Весь сервер (CPU, RAM, диск, сеть)        | Только среда выполнения приложения     |
| **Гостевая ОС**          | Да, полная ОС (например, Ubuntu, Windows) | Нет — используется ядро хоста          |
| **Размер**               | Гигабайты (образ ВМ)                      | Мегабайты (образ контейнера)           |
| **Скорость запуска**     | Секунды – минуты                          | Миллисекунды – секунды                 |
| **Производительность**   | Ниже (накладные расходы гипервизора)      | Почти как на "голом железе"            |
| **Изоляция**             | Высокая (полная изоляция ВМ)              | Умеренная (через namespace/cgroups)    |
| **Безопасность**         | Выше (изоляция на уровне ОС)              | Ниже (общее ядро ОС)                   |
| **Плотность размещения** | Несколько ВМ на сервере                   | Десятки/сотни контейнеров на сервере   |
| **Примеры технологий**   | VMware, VirtualBox, Hyper-V, KVM          | Docker, Podman, containerd, Kubernetes |

## 🔍 Подробное объяснение

### 1. Виртуализация (Virtualization)

**Как работает:**

- Используется **гипервизор** (например, VMware, KVM, Hyper-V).
- Гипервизор создаёт **виртуальные машины (VM)**, каждая из которых имеет:
  - Собственную операционную систему (гостевую ОС).
  - Виртуальные CPU, RAM, диск, сеть.
- Каждая ВМ работает **независимо**, как отдельный физический сервер.

**Пример:**

> На одном сервере запущены 3 ВМ:
>
> - ВМ1: Ubuntu + веб-сервер
> - ВМ2: Windows + SQL Server
> - ВМ3: CentOS + приложение  
>   Все они используют одно и то же "железо", но полностью изолированы.

**✅ Плюсы:**

- Высокая изоляция и безопасность.
- Поддержка разных ОС (Linux, Windows и т.д.).
- Подходит для сложных, "тяжёлых" приложений.

**❌ Минусы:**

- Высокие накладные расходы (память, дисковое пространство).
- Медленный запуск.
- Меньшая плотность (меньше ВМ на одном сервере).

### 2. Контейнеризация (Containerization)

**Как работает:**

- Приложения упаковываются в **контейнеры** вместе со всеми зависимостями (библиотеки, настройки, среда выполнения).
- Все контейнеры **делят одно ядро хостовой ОС**, но изолированы с помощью:
  - `namespaces` — изоляция процессов, сети, файловой системы и т.д.
  - `cgroups` — ограничение ресурсов (CPU, память).
- Управляется через **движок контейнеров** (Docker, Podman и др.).

**Пример:**

> На одном сервере с Linux запущены:
>
> - Контейнер с Nginx
> - Контейнер с Node.js
> - Контейнер с PostgreSQL
>   Все они используют одно ядро, но "думают", что работают в изоляции.

**✅ Плюсы:**

- Очень быстро запускаются.
- Экономят ресурсы (не нужно запускать ОС для каждого приложения).
- Легко масштабируются (идеально для микросервисов).
- Поддерживают CI/CD и DevOps.

**❌ Минусы:**

- Все контейнеры должны быть совместимы с хостовой ОС (обычно Linux).
- Меньшая изоляция — уязвимость в ядре может повлиять на все контейнеры.
- Не подходит для запуска разных ОС (например, Windows-приложение на Linux-хосте).

## ✅ Когда что использовать?

| Сценарий                                  | Рекомендуется      |
| ----------------------------------------- | ------------------ |
| Запуск разных ОС (Linux + Windows)        | ✅ Виртуализация   |
| Высокая безопасность и изоляция           | ✅ Виртуализация   |
| Микросервисная архитектура                | ✅ Контейнеризация |
| Быстрое развертывание и масштабирование   | ✅ Контейнеризация |
| DevOps, CI/CD, Kubernetes                 | ✅ Контейнеризация |
| Устаревшие приложения, требующие своей ОС | ✅ Виртуализация   |
| Ограничение ресурсов и плотная упаковка   | ✅ Контейнеризация |

## 💡 Современные тренды

- **Kubernetes** управляет **контейнерами** в масштабе.
- **Гибридные подходы**: Виртуальные машины с контейнерами внутри (например, в облаках).
- **Wasm (WebAssembly)** — новая альтернатива, но пока в развитии.

## 🔚 Вывод

> - **Виртуализация** — это как арендовать **отдельные квартиры** в одном доме: изолированно, безопасно, но дорого и медленно.
> - **Контейнеризация** — как **комнаты в хостеле**, где общая кухня и душ (ОС), но у каждого своя закрытая комната: быстро, дёшево, компактно.

</p></details>

---

### ❓ 3. Что такое Docker Registry

**[📺 Видео-вопрос (51:30)](https://youtu.be/z0QZ7VLyP7w?t=3090)**

<details><summary><strong>Показать ответ</strong></summary><p>

## 🗄️ **Docker Registry** — это хранилище для Docker образов, которое позволяет:

- Сохранять, управлять и распространять Docker образы.
- Работать как центральный сервер для хранения и доступа к образам.

### 🧳 **Репозитории (Repositories):**

- **Репозиторий** — это место, где хранятся Docker образы. Каждый репозиторий может содержать несколько версий образов, отличающихся **тегами** (например, `my-app:latest` или `my-app:v1`).
- Репозитории могут быть **публичными** (доступными для всех) или **приватными** (ограниченными доступом).

### 🌐 **Docker Hub:**

- **Docker Hub** — это публичный Docker Registry, предоставляемый официальным Docker.
- В **Docker Hub** можно найти тысячи публичных образов, которые могут быть использованы для различных приложений и сервисов.
- Docker предоставляет официальный образ: registry:2
- Также пользователи могут загружать свои собственные образы или использовать образы других.

### 🔐 **Частные регистры:**

- Вы можете настроить **собственный Docker Registry**, чтобы хранить образы для своей организации или проекта.
- Частные регистры обеспечивают контроль доступа и безопасность Docker образов.

### 🏷️ **Образы и теги:**

- Docker Registry хранит **образы** с метками, которые называются **тегами**. Например, `nginx:latest` указывает на последнюю версию образа `nginx`.

### ⬆️⬇️ **Push и Pull:**

- **Push**: Отправка (загрузка) образа в реестр.
- **Pull**: Загрузка образа из реестра на локальную машину или в кластер для использования в контейнере.

### 🏆 **Преимущества Docker Registry:**

- **Централизованное хранилище** для всех образов, что облегчает их управление и распределение.
- **Безопасность и контроль доступа**: возможность настроить приватные репозитории с контролем прав доступа.
- **Обновления и версионирование**: возможность поддерживать несколько версий образов для различных целей (например, тестирование, продакшн).

**Docker Registry** — это важный элемент инфраструктуры для контейнеризации, который позволяет эффективно управлять и распространять образы в процессе разработки, тестирования и развертывания приложений.

</p></details>

---

### ❓ 4. Какой командой публикуются image в registry?

**[📺 Видео-вопрос (52:36)](https://youtu.be/z0QZ7VLyP7w?t=3156)**

<details><summary><strong>Показать ответ</strong></summary><p>

- Для того чтобы **опубликовать Docker образ в Docker Registry**, используется команда **`docker push`**.

## 📜 **Команда для публикации образа:**

```bash
docker push <имя_пользователя>/<имя_репозитория>:<тег>
```

### 📦 **Пример:**

1. **Создайте образ:**
   Сначала нужно создать образ с помощью команды **`docker build`**:

   ```bash
   docker build -t username/repository:tag .
   ```

2. **Войти в Docker Hub (или в другой реестр):**
   Перед загрузкой образа в Docker Hub или другой реестр необходимо войти с помощью команды:

   ```bash
   docker login
   ```

   Эта команда запросит ваш логин и пароль для Docker Hub (или для другого реестра, если используется).

3. **Отправка образа в реестр:**
   После того как образ создан, его можно отправить в реестр с помощью команды **`docker push`**:

   ```bash
   docker push username/repository:tag
   ```

### 📝 **Пример:**

Если ваш образ называется `myapp` и вы хотите его отправить в реестр с именем `mydockerhub`, команда будет выглядеть так:

```bash
docker push mydockerhub/myapp:v1
```

### 🗝️ **Примечания:**

- **`username`** — это ваше имя пользователя в Docker Hub или в другом реестре.
- **`repository`** — это название вашего репозитория.
- **`tag`** — это версия вашего образа (например, `latest`, `v1`).

После успешной команды **`docker push`** ваш образ будет доступен в реестре для использования другими пользователями или для дальнейшего развертывания.

</p></details>

---

### ❓ 5. Какую информацию обычно включают в тег в соответствии с лучшими практиками?

**[📺 Видео-вопрос (53:03)](https://youtu.be/z0QZ7VLyP7w?t=3183)**

<details><summary><strong>Показать ответ</strong></summary><p>

## 1. 🔥 **`latest`**

Тег **`latest`** является одним из самых популярных и часто используется для обозначения самой последней стабильной версии образа.

- **Пример:**
  ```bash
  docker pull nginx:latest
  ```

## 2. 📦 **Версия приложения (например, `v1.0`, `v2.1`)**

Тег с версией приложения или релиза помогает точно указать, какую именно версию образа использовать. Это стандартная практика для управления версиями.

- **Пример:**
  ```bash
  docker pull myapp:v1.0
  ```

## 3. 🧑‍🔧 **Git-хэши (например, `abc1234`)**

Некоторые команды или организации используют Git-хэши для тегирования образов, чтобы точно указать, какой коммит был использован для создания образа. Это особенно полезно для разработки и тестирования.

- **Пример:**
  ```bash
  docker pull myapp:abc1234
  ```

## 6. 📅 **Тег по дате (например, `2023-08-03`)**

Использование даты в теге помогает отслеживать, когда был построен образ, что полезно для автоматизации и контроля версий.

- **Пример:**
  ```bash
  docker pull myapp:2023-08-03
  ```

## 7. 🔖 **Комбинированный подход (например, `v1.2.0-a1b2c3d`)**

К примеру сочетает версию или дату и точный хэш коммита.

- **Пример:**
  ```bash
  myapp:v1.2.0-a1b2c3d
  #или
  myapp:20240315-a1b2c3d
  ```

## 🔔 Важно

- latest — это ярлык, а не замена версии.
- Всегда используй конкретный тег (не latest) в продакшене.

</p></details>

---

### ❓ 6. В чем разница между ENTRYPOINT и CMD в Dockerfile?

**[📺 Видео-вопрос (53:45)](https://youtu.be/z0QZ7VLyP7w?t=3225)**

<details><summary><strong>Показать ответ</strong></summary><p>

### Оба используются для задания команды, которую выполняет контейнер при запуске, но у них разные цели и поведение.

### 📦 CMD

- Задаёт **команду или аргументы по умолчанию**
- Легко **переопределяется** при запуске контейнера

```Dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

```bash
docker run myimage echo "Hello"
```

🔹 Контейнер выполнит: `echo Hello`, а не `nginx`

### 🚀 ENTRYPOINT

- Задаёт **основную команду**, которую контейнер всегда выполняет
- Аргументы можно передать, но сама команда остаётся

```Dockerfile
ENTRYPOINT ["nginx"]
```

```bash
docker run myimage -g "daemon off;"
```

🔹 Контейнер выполнит: `nginx -g "daemon off;"`

### 🔗 Совместное использование

Можно использовать `ENTRYPOINT` для команды и `CMD` для аргументов по умолчанию:

```Dockerfile
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
```

🔹 По умолчанию: `nginx -g "daemon off;"`  
🔹 Можно переопределить только `CMD` при запуске.

### 📊 Сравнительная таблица

| Характеристика         | ENTRYPOINT                     | CMD                                |
| ---------------------- | ------------------------------ | ---------------------------------- |
| Цель                   | Основная неизменяемая команда  | Аргументы или команда по умолчанию |
| Переопределяется?      | ❌ Только через `--entrypoint` | ✅ Через аргументы `docker run`    |
| Форматы                | `exec` и `shell`               | `exec` и `shell`                   |
| Часто используется для | Агентов, скриптов, CLI-утилит  | Простых конфигураций, тестов       |

### 📘 Заключение

- Используй `ENTRYPOINT`, если контейнер всегда должен выполнять конкретную программу
- Используй `CMD` для аргументов или как запасной вариант, который можно легко заменить
- В паре они дают наибольшую гибкость и контроль

</p></details>

---

### ❓ 7. Что такое RUN в Dockerfile?

**[📺 Видео-вопрос (55:45)](https://youtu.be/z0QZ7VLyP7w?t=3345)**

<details><summary><strong>Показать ответ</strong></summary><p>

- В **Dockerfile** команда **`RUN`** используется для выполнения команд внутри контейнера во время его сборки. Это позволяет вам настраивать и устанавливать программное обеспечение, библиотеки и выполнять другие операции, которые необходимы для создания образа.

## 🔧 **Основное назначение:**

- **`RUN`** выполняет команду в процессе сборки образа, например, установку пакетов, копирование файлов, создание директорий и т. д.
- Каждый **`RUN`** создает новый слой в Docker образе, что влияет на размер конечного образа. Поэтому важно комбинировать команды, если это возможно, чтобы уменьшить количество слоев.

## 📜 **Синтаксис:**

```dockerfile
RUN <команда>
```

## ⚙️ **Пример использования:**

```dockerfile
# Использование RUN для обновления пакетов и установки приложений
RUN apt-get update && apt-get install -y nginx
```

В этом примере:

1. Обновляется список пакетов с помощью `apt-get update`.
2. Устанавливается пакет `nginx` с помощью `apt-get install -y nginx`.

## 🛠️ **Разные виды команд `RUN`:**

1. **RUN в виде одной команды** (используется для выполнения нескольких операций в одном слое):

   ```dockerfile
   RUN apt-get update && apt-get install -y curl
   ```

2. **RUN с использованием оболочки (`sh`)**:
   Docker по умолчанию использует `/bin/sh -c` для выполнения команд, но вы также можете указать другую оболочку:

   ```dockerfile
   RUN /bin/bash -c "echo Hello, Docker!"
   ```

3. **RUN с использованием `&&`**:
   Когда несколько команд нужно выполнить в одном слое, можно использовать `&&`, чтобы команды выполнялись последовательно, и если одна не выполнится, другие не будут запущены:
   ```dockerfile
   RUN apt-get update && apt-get install -y curl && apt-get clean
   ```

## ⚡ **Важные моменты:**

- **Каждый `RUN` создает новый слой** в Docker образе, и каждый слой хранит изменения, сделанные этой командой. Много команд **`RUN`** может увеличить размер образа, если они не объединены.
- **Понимание кеширования слоев**: Docker кэширует слои, и если в Dockerfile изменяется только одна строка, то все последующие строки могут использовать кэшированное состояние предыдущего слоя. Это экономит время на повторной сборке.

## 📝 **Пример:**

```dockerfile
# Используем RUN для установки nginx
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y nginx
```

В этом примере:

- **`FROM`** указывает базовый образ (в данном случае `ubuntu:20.04`).
- **`RUN`** обновляет репозитории и устанавливает `nginx`.

## 🔑 **Итог:**

Команда **`RUN`** — это мощный инструмент для настройки и создания вашего Docker образа, который позволяет вам настроить все необходимые компоненты внутри контейнера еще до его запуска.

</p></details>

---

### ❓ 8. Как в Dockerfile изменить пользователя, от которого будут выполняться команды?

**[📺 Видео-вопрос (56:31)](https://youtu.be/z0QZ7VLyP7w?t=3391)**

<details><summary><strong>Показать ответ</strong></summary><p>

- В **Dockerfile** для изменения пользователя, от которого будут выполняться команды, используется команда **`USER`**.

## 📜 **Синтаксис:**

```dockerfile
USER <пользователь>
```

После использования этой команды все следующие команды в Dockerfile будут выполняться от имени указанного пользователя, а не от пользователя по умолчанию (обычно это `root`).

## 🛠️ **Пример использования:**

```dockerfile
# Используем образ Ubuntu как базовый
FROM ubuntu:20.04

# Создаем нового пользователя
RUN useradd -m myuser

# Переключаемся на нового пользователя
USER myuser

# Выполняем команду от имени нового пользователя
RUN echo "Hello from myuser"
```

### Объяснение:

1. **`useradd -m myuser`** — создает нового пользователя `myuser`.
2. **`USER myuser`** — переключает текущего пользователя на `myuser`.
3. Все команды, следующие после **`USER myuser`**, будут выполняться от имени этого пользователя, в том числе и команда **`RUN echo "Hello from myuser"`**.

## ⚠️ **Важные моменты:**

- Если вам нужно вернуться к пользователю **root** для выполнения команд с повышенными правами, можно использовать команду **`USER root`**.
- Команда **`USER`** не влияет на команды, которые были выполнены до её использования. Она только изменяет контекст для последующих команд.

## 🔄 **Пример возврата к root:**

```dockerfile
# Возвращаемся к пользователю root
USER root

# Теперь выполняем команду от имени root
RUN apt-get update
```

## 🏁 **Итог:**

С помощью команды **`USER`** можно эффективно управлять правами пользователя в контейнере, что улучшает безопасность и контроль над выполнением команд.

</p></details>

---

### ❓ 9. Что такое мультистейдж сборка (multistage build)?

**[📺 Видео-вопрос (57:03)](https://youtu.be/z0QZ7VLyP7w?t=3423)**

<details><summary><strong>Показать ответ</strong></summary><p>

- **Мультистейдж билд** в Docker — это техника, которая позволяет оптимизировать процесс сборки Docker образа, используя несколько этапов (стадий) в одном Dockerfile. Это позволяет снизить размер конечного образа, улучшив производительность и избавившись от лишних зависимостей.

## 🏗️ **Как работает мультистейдж билд?**

1. **Несколько стадий сборки:** Каждый этап сборки определен с помощью отдельной директивы **`FROM`**, где можно использовать разные образы для разных стадий (например, для сборки приложения, тестирования и т. д.).
2. **Передача артефактов:** На каждом этапе могут быть использованы только те данные, которые необходимы для следующего этапа. Лишние зависимости или инструменты, установленные для сборки, можно исключить в финальном образе.
3. **Оптимизация размера:** За счет удаления ненужных файлов, зависимостей и инструментов на поздних стадиях, конечный образ будет меньше.

## 📦 **Пример использования мультистейдж билда:**

```dockerfile
# Этап 1: Сборка
FROM golang:1.16 AS builder

WORKDIR /app
COPY . .
RUN go build -o myapp .

# Этап 2: Финальный образ
FROM alpine:latest

WORKDIR /root/
COPY --from=builder /app/myapp .

CMD ["./myapp"]
```

### 🔍 **Объяснение:**

1. **Первый этап (builder):**

   - Используется образ **`golang:1.16`** для сборки приложения.
   - Все файлы копируются в контейнер, и выполняется команда **`go build`** для создания исполняемого файла.
   - Этот этап называется `builder`, и он содержит все зависимости для сборки.

2. **Второй этап (финальный):**
   - Используется минималистичный образ **`alpine:latest`**, который не содержит всех инструментов для сборки.
   - В этот образ копируется только нужный исполняемый файл **`myapp`** из предыдущего этапа, с помощью команды **`COPY --from=builder`**.
   - В финальном образе остаются только необходимые артефакты.

## 🛠️ **Преимущества мультистейдж билда:**

- **Снижение размера образа:** Из образа исключаются ненужные инструменты и зависимости, оставляя только то, что необходимо для работы приложения.
- **Упрощение Dockerfile:** Вы можете разделить процесс на несколько этапов, каждый из которых отвечает за свою задачу.
- **Безопасность:** Избегание размещения инструментария для сборки (например, компиляторов или пакетов разработки) в финальном образе, что может снизить риски для безопасности.

## 🎯 **Итог:**

Мультистейдж билды позволяют создавать более эффективные, компактные и безопасные Docker образы, разделяя процесс сборки на несколько стадий и исключая лишние зависимости из финального контейнера.

</p></details>

---

### ❓ 10. Какой командой запускают контейнер?

**[📺 Видео-вопрос (58:24)](https://youtu.be/z0QZ7VLyP7w?t=3504)**

<details><summary><strong>Показать ответ</strong></summary><p>

- Команда для запуска контейнера в Docker — это **`docker run`**.

## 📜 **Синтаксис:**

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

### Основные параметры:

- **`IMAGE`**: Имя Docker образа, из которого будет запущен контейнер.
- **`COMMAND`** (необязательно): Команда, которая будет выполнена внутри контейнера.
- **`ARG...`** (необязательно): Аргументы, передаваемые команде внутри контейнера.

## 📝 **Пример команды:**

```bash
docker run hello-world
```

В этом примере Docker загрузит образ **`hello-world`** (если он не был загружен ранее) и запустит контейнер.

## 🛠️ **Полезные опции для команды `docker run`:**

1. **`-d`** — запуск контейнера в фоновом режиме (detached mode).

   ```bash
   docker run -d myapp
   ```

2. **`-p`** — проброс портов для доступа к контейнеру через определенный порт.

   ```bash
   docker run -p 8080:80 myapp
   ```

3. **`--name`** — задает имя для контейнера.

   ```bash
   docker run --name my_container myapp
   ```

4. **`-e`** — установка переменных окружения в контейнере.

   ```bash
   docker run -e MY_VAR=value myapp
   ```

5. **`-v`** — монтирование тома или каталога на хосте в контейнер.

   ```bash
   docker run -v /host/path:/container/path myapp
   ```

6. **`--rm`** — удаляет контейнер после его остановки.
   ```bash
   docker run --rm myapp
   ```

## 🚀 **Пример с несколькими опциями:**

```bash
docker run -d -p 8080:80 --name webserver nginx
```

В этом примере:

- **`-d`** запускает контейнер в фоновом режиме.
- **`-p 8080:80`** пробрасывает порт 80 контейнера на порт 8080 хоста.
- **`--name webserver`** задает имя контейнера как `webserver`.
- **`nginx`** — это имя Docker образа, который будет запущен.

## 🎯 **Итог:**

Команда **`docker run`** позволяет запускать контейнеры с множеством настроек и параметров, таких как проброс портов, установка переменных окружения и даже автоматическое удаление контейнера после его остановки.

</p></details>

---

### ❓ 11. Как сделать что бы контейнер перезапускался после рестарта сервера?

**[📺 Видео-вопрос (59:25)](https://youtu.be/z0QZ7VLyP7w?t=3565)**

<details><summary><strong>Показать ответ</strong></summary><p>

- Docker предоставляет несколько политик перезапуска контейнеров, чтобы управлять их поведением при сбоях или перезагрузке системы.

###🔁 Поддерживаемые политики перезапуска

| Политика              | Когда применяется перезапуск             | Описание                                                                        |
| --------------------- | ---------------------------------------- | ------------------------------------------------------------------------------- |
| `no` _(по умолчанию)_ | Никогда                                  | Контейнер не будет автоматически перезапущен                                    |
| `always`              | Всегда, при любом завершении             | Контейнер будет перезапущен даже после ручной остановки или перезагрузки Docker |
| `on-failure[:N]`      | При завершении с ошибкой (exit code ≠ 0) | Перезапускается при сбое, можно задать максимальное количество попыток `N`      |
| `unless-stopped`      | Всегда, кроме случаев ручной остановки   | Перезапускается, пока не будет явно остановлен пользователем                    |

### 📍 Где можно применять `restart` политику

### 1. Через `docker run`

```bash
docker run --restart=always my-container
docker run --restart=on-failure:3 my-container
docker run --restart=unless-stopped my-container
```

### 2. В `docker-compose.yml`

```yaml
version: '3.8'
services:
  app:
    image: my-app
    restart: unless-stopped
```

> Параметр `restart` поддерживается начиная с версии Docker Compose 2.

### ⚠️ Где **нельзя** использовать `restart`

- ❌ В `Dockerfile` — политики перезапуска относятся к запуску контейнера, а не к образу.
- ❌ В `docker build` — эти параметры не поддерживаются на этапе сборки.

### 🧠 Как выбрать нужную политику?

| Сценарий                                                    | Рекомендуемая политика |
| ----------------------------------------------------------- | ---------------------- |
| Критичный сервис, который должен работать всегда            | `always`               |
| Приложение может завершаться ошибкой, ограничить перезапуск | `on-failure:3`         |
| Нужно исключить перезапуск после ручной остановки           | `unless-stopped`       |
| Нет необходимости в автоперезапуске                         | `no`                   |

</p></details>

---

### ❓ 12. Если запустить контейнер, который использует определенные данные, будут ли эти данные удалены при удалении контейнера?

**[📺 Видео-вопрос (01:00:09)](https://youtu.be/z0QZ7VLyP7w?t=3609)**

<details><summary><strong>Показать ответ</strong></summary><p>

- Когда вы запускаете контейнер в Docker, данные, которые использует контейнер, могут быть сохранены или потеряны при удалении контейнера, в зависимости от того, как эти данные хранятся.

## 1. 📂 **Данные внутри контейнера (в контейнерной файловой системе):**

- **Да**, данные, находящиеся внутри контейнера, будут удалены, когда контейнер будет удален, если эти данные не были сохранены в **внешний том (volume)** или **каталог на хосте**.
- **Контейнер** в Docker — это временная среда, и все изменения, сделанные внутри контейнера, могут быть удалены при его удалении, если они не были сохранены.

## 2. 📦 **Данные, сохраненные в томах (volumes):**

- **Нет**, если данные были сохранены в **томе (volume)** или **монтированном каталоге** на хосте, они не будут удалены при удалении контейнера. Тома предназначены для хранения данных, которые должны пережить удаление контейнера.
- Тома существуют отдельно от контейнера и могут быть использованы несколькими контейнерами.

### 📝 **Пример с томом:**

```bash
docker run -v /host/path:/container/path myapp
```

- **`/host/path`** — это каталог на хостовой машине, который будет использоваться контейнером.
- **`/container/path`** — это путь внутри контейнера, где данные будут храниться.

Если контейнер удален, данные в **`/host/path`** на хосте останутся.

## 3. 🧹 **Пример использования команды для удаления контейнера с томами:**

Если вы хотите удалить контейнер, но оставить тома, используйте команду:

```bash
docker rm -v <container_name>
```

- Эта команда удалит контейнер, но **не удалит тома**, связанные с ним.

## 🎯 **Итог:**

- **Данные внутри контейнера**: Будут удалены при удалении контейнера.
- **Данные в томах или монтированных каталогах**: Не будут удалены при удалении контейнера, так как тома существуют независимо от контейнера.

</p></details>

---

### ❓ 13. Как в контейнере можно изменить ресурсы на лету?

**[📺 Видео-вопрос (01:02:03)](https://youtu.be/z0QZ7VLyP7w?t=3723)**

<details><summary><strong>Показать ответ</strong></summary><p>

- Изменить ресурсы контейнера **на лету** (то есть без его перезапуска) возможно с помощью нескольких инструментов Docker. Однако важно отметить, что не все параметры можно изменить в процессе работы контейнера.

## 🛠️ **1. Изменение ресурсов с помощью команды `docker update`:**

Команда **`docker update`** позволяет изменить следующие параметры ресурса контейнера:

- **CPU** (количество процессорных ядер)
- **Память** (лимит памяти)
- **Блокировка и параметры сети**

### 📜 **Синтаксис:**

```bash
docker update [OPTIONS] <container_name_or_id>
```

### 📝 **Пример использования:**

- Увеличение выделенной памяти контейнеру до 2 ГБ:

  ```bash
  docker update --memory 2g <container_name_or_id>
  ```

- Изменение ограничения на процессор (например, установка лимита в 2 ядра):

  ```bash
  docker update --cpus 2 <container_name_or_id>
  ```

- Одновременное изменение памяти и процессора:
  ```bash
  docker update --memory 2g --cpus 2 <container_name_or_id>
  ```

## 🔄 **2. Изменение лимитов с помощью `docker run` (при перезапуске контейнера):**

Если вы хотите изменить ресурсы при запуске контейнера (например, лимиты памяти или процессора), это можно сделать через параметры **`docker run`**.

### 📜 **Пример:**

- Ограничение памяти и процессора при запуске контейнера:
  ```bash
  docker run --memory=2g --cpus=2 mycontainer
  ```

## 🌐 **3. Изменение сети или блокировки I/O на лету:**

Docker также позволяет изменять параметры, такие как блокировка ввода/вывода или параметры сети контейнера через команду **`docker network`** или **`docker exec`**. Однако изменение таких параметров требует изменения конфигураций Docker и обычно подразумевает перезапуск контейнера.

## 🖧 **4. Процесс изменения параметров сети:**

Docker не предоставляет прямых инструментов для изменения сетевых параметров "на лету", например, IP-адрес или настройки интерфейса. Для таких изменений обычно требуется остановить контейнер, изменить его настройки и затем перезапустить.

## ⚡ **Важные замечания:**

- Изменение параметров, таких как процессорное время и память через команду **`docker update`**, будет немедленно применяться к работающему контейнеру без его остановки.
- Некоторые ресурсы, такие как количество памяти, могут требовать перезапуска контейнера, чтобы изменения вступили в силу.

</p></details>

---

# 4. CI-CD

### ❓ 1. Какие основные шаги бывают в CI-CD pipeline?

**[📺 Видео-вопрос (01:05:38)](https://youtu.be/z0QZ7VLyP7w?t=3938)**

<details><summary><strong>Показать ответ</strong></summary><p>

**CI/CD pipeline** (непрерывная интеграция и доставка) — это автоматизированный процесс, который помогает ускорить релизы и повысить качество программного обеспечения. Мы разделим процесс на два основных этапа: **CI (Continuous Integration)** и **CD (Continuous Delivery/Continuous Deployment)**.

## 📑 **CI Pipeline** - Непрерывная интеграция

**CI** фокусируется на **сборке, тестировании и интеграции** кода. Он позволяет автоматически интегрировать код из разных источников и тестировать его на наличие ошибок.

| Этап                       | Цель                                             | Инструменты (примеры)                          |
| -------------------------- | ------------------------------------------------ | ---------------------------------------------- |
| **1. Checkout**            | Получение кода из репозитория                    | `git clone`, GitHub Actions, GitLab CI         |
| **2. Build**               | Сборка приложения (бинарник, образ, артефакт)    | `docker build`, `npm run build`, `mvn package` |
| **3. Test**                | Запуск тестов (юнит, интеграционные, end-to-end) | `pytest`, `Jest`, `JUnit`, `Cypress`           |
| **4. Lint / Code Quality** | Проверка стиля кода и качества                   | `ESLint`, `Pylint`, `SonarQube`                |
| **5. Security Scan**       | Поиск уязвимостей в коде и зависимостях          | `Trivy`, `Snyk`, `OWASP ZAP`                   |
| **6. Build Image**         | Создание Docker-образа (если используется)       | `docker build`, `buildah`                      |
| **7. Push Image**          | Отправка образа в registry                       | `docker push`, `ghcr.io`, `ECR`                |

### Шаги в **CI Pipeline:**

1. **Checkout**

   - **Цель:** Получение последней версии кода из репозитория.
   - **Пример в GitHub Actions:**

   ```yaml
   steps:
     - name: Checkout repository
       uses: actions/checkout@v2
   ```

2. **Build**

   - **Цель:** Сборка приложения, включая создание бинарных файлов, Docker-образов или артефактов.
   - **Пример в GitHub Actions:**

   ```yaml
   steps:
     - name: Set up Docker Buildx
       uses: docker/setup-buildx-action@v1

     - name: Build Docker image
       run: |
         docker build -t myapp .
   ```

3. **Test**

   - **Цель:** Запуск тестов для проверки кода на ошибки.
   - **Пример в GitHub Actions:**

   ```yaml
   steps:
     - name: Run tests
       run: |
         npm install
         npm test
   ```

4. **Lint / Code Quality**

   - **Цель:** Проверка кода на соответствие стандартам качества и стиля.
   - **Пример в GitHub Actions:**

   ```yaml
   steps:
     - name: Run ESLint
       run: |
         npm install
         npx eslint .
   ```

5. **Security Scan**

   - **Цель:** Поиск уязвимостей в коде и зависимостях.
   - **Пример в GitHub Actions:**

   ```yaml
   steps:
     - name: Run Snyk Security Scan
       uses: snyk/actions/snyk-scan@v1
   ```

6. **Build Image**

   - **Цель:** Сборка Docker-образа для контейнеризованных приложений.
   - **Пример в GitHub Actions:**

   ```yaml
   steps:
     - name: Build Docker image
       run: |
         docker build -t myapp .
   ```

7. **Push Image**

   - **Цель:** Отправка собранного Docker-образа в реестр.
   - **Пример в GitHub Actions:**

   ```yaml
   steps:
     - name: Log in to Docker Hub
       uses: docker/login-action@v2
       with:
         username: ${{ secrets.DOCKER_USERNAME }}
         password: ${{ secrets.DOCKER_PASSWORD }}

     - name: Push Docker image
       run: |
         docker push myapp
   ```

## 📦 **CD Pipeline** - Непрерывная доставка и развертывание

**CD** фокусируется на **доставке приложения на тестовую или продакшн-среду**. Включает развертывание, постдеплойные тесты и уведомления.

| Этап                                    | Цель                                                | Инструменты (примеры)                       |
| --------------------------------------- | --------------------------------------------------- | ------------------------------------------- |
| **8. Deploy (Staging)**                 | Развёртывание на тестовом окружении                 | Kubernetes, Helm, Ansible                   |
| **9. Integration Tests**                | Тесты в среде, близкой к продакшену                 | Postman, custom scripts                     |
| **10. Approval (ручное подтверждение)** | Ручной апрув перед продакшеном                      | GitHub Environments, GitLab Approvals       |
| **11. Deploy (Production)**             | Автоматическое или ручное развёртывание в продакшен | Argo CD, Flux, Terraform                    |
| **12. Post-Deploy / Smoke Tests**       | Проверка работоспособности после деплоя             | `curl`, health checks, synthetic monitoring |
| **13. Notify**                          | Уведомление команды о результате                    | Slack, Email, MS Teams                      |

### Шаги в **CD Pipeline:**

8. **Deploy (Staging)**

   - **Цель:** Развертывание приложения на **staging** среде для тестирования.
   - **Пример в GitHub Actions:**

   ```yaml
   steps:
     - name: Deploy to Staging
       run: |
         kubectl apply -f k8s/deployment.yaml
   ```

9. **Integration Tests**

   - **Цель:** Запуск интеграционных тестов для проверки приложения в условиях, приближенных к продакшену.
   - **Пример в GitHub Actions:**

   ```yaml
   steps:
     - name: Run integration tests
       run: |
         postman run tests/collection.json
   ```

10. **Approval (ручное подтверждение)**

- **Цель:** Требуется ручное подтверждение для деплоя на продакшн.
- **Пример в GitHub Actions:**

```yaml
steps:
  - name: Approval step
    if: ${{ github.event.pull_request.merged == 'true' }}
    run: echo "Approve the deployment to production"
```

11. **Deploy (Production)**

- **Цель:** Развертывание на продакшн-среду.
- **Пример в GitHub Actions:**

```yaml
steps:
  - name: Deploy to Production
    run: |
      kubectl apply -f k8s/prod-deployment.yaml
```

12. **Post-Deploy / Smoke Tests**

- **Цель:** Проверка базовой работоспособности приложения после деплоя.
- **Пример в GitHub Actions:**

```yaml
steps:
  - name: Smoke test
    run: |
      curl -f http://myapp.com/health
```

13. **Notify**

- **Цель:** Уведомление команды о результате CI/CD процесса.
- **Пример в GitHub Actions:**

```yaml
steps:
  - name: Send Slack notification
    uses: slackapi/slack-github-action@v1.17.0
    with:
      channel: '#ci-notifications'
      text: 'CI pipeline completed successfully'
```

## 📑 **Итоговые шаги в CI pipeline:**

1. **Checkout**
2. **Build**
3. **Test**
4. **Lint / Code Quality**
5. **Security Scan**
6. **Build Image**
7. **Push Image**

## 🚀 **Итоговые шаги в CD pipeline:**

8. **Deploy (Staging)**
9. **Integration Tests**
10. **Approval (ручное подтверждение)**
11. **Deploy (Production)**
12. **Post-Deploy / Smoke Tests**
13. **Notify**

</p></details>

---

### ❓ 2. Как и где хранятся переменные в GitLab CI/CD?

**[📺 Видео-вопрос (01:09:28)](https://youtu.be/z0QZ7VLyP7w?t=4168)**

<details><summary><strong>Показать ответ</strong></summary><p>

- В **GitLab CI/CD** переменные могут храниться в нескольких местах, в зависимости от области их применения и уровня доступа. Рассмотрим основные способы хранения переменных.

## 1. 🗂️ **Переменные проекта (Project-level variables)**

**Что это:** Переменные, определённые на уровне проекта, применяются только для конкретного проекта.

**Где хранятся:** Переменные проекта хранятся в настройках конкретного проекта на платформе GitLab.

**Как добавить:**

1. Перейдите в настройки проекта.
2. Откройте раздел **CI / CD**.
3. Прокрутите вниз до секции **Variables**.
4. Нажмите на кнопку **Add Variable** и укажите имя переменной и её значение.

**Пример в `.gitlab-ci.yml`:**

```yaml
variables:
  MY_VAR: 'some_value'
```

## 2. 🏢 **Переменные группы (Group-level variables)**

**Что это:** Переменные, определённые на уровне группы, применяются ко всем проектам, которые находятся в этой группе.

**Где хранятся:** Переменные группы хранятся в настройках группы в GitLab.

**Как добавить:**

1. Перейдите в настройки группы.
2. Откройте раздел **CI / CD**.
3. Прокрутите вниз до секции **Variables**.
4. Нажмите на кнопку **Add Variable** и укажите имя переменной и её значение.

## 3. 🔐 **Переменные GitLab CI/CD (Secret variables)**

**Что это:** Переменные, которые могут быть настроены как **секретные** (masked) или **защищённые** (protected).

- **Masked variables**: Значения переменной скрываются в логах CI/CD.
- **Protected variables**: Эти переменные доступны только для защищённых веток и тегов.

**Как добавить:**

1. Перейдите в настройки проекта или группы.
2. В секции **Variables** добавьте переменную и установите флажки для **masked** или **protected**, если необходимо.

**Пример в `.gitlab-ci.yml`:**

```yaml
variables:
  MY_SECRET: '${CI_JOB_TOKEN}'
```

## 4. 👤 **Переменные для пользователей (User-level variables)**

**Что это:** Переменные, определённые на уровне пользователя, могут быть использованы для автоматизации действий в **GitLab Runner**.

**Где хранятся:** Эти переменные настраиваются в интерфейсе пользователя GitLab и используются для определённых runner'ов или локальных машин.

**Как добавить:**

1. Перейдите в настройки пользователя.
2. Откройте раздел **CI / CD**.
3. Прокрутите вниз до секции **Variables** и добавьте нужные переменные.

## 5. 💻 **Переменные CI/CD в `.gitlab-ci.yml`**

**Что это:** Вы можете определить переменные прямо в файле `.gitlab-ci.yml`, которые будут доступны только для этого конкретного pipeline.

**Пример:**

```yaml
stages:
  - build
  - deploy

variables:
  DEPLOY_ENV: 'production'

build:
  stage: build
  script:
    - echo "Building application..."

deploy:
  stage: deploy
  script:
    - echo "Deploying to $DEPLOY_ENV"
```

## 6. 🔄 **Как работают переменные в GitLab CI/CD?**

### **Приоритет переменных:**

1. Переменные, определённые в `.gitlab-ci.yml`, имеют **наивысший приоритет**.
2. Переменные, определённые на уровне проекта или группы, могут быть переопределены на уровне runner'а или локальной машины.

### **Типы переменных:**

- **Masking (маскировка):** Для скрытия данных переменной (например, паролей).
- **Protected (защищённые):** Применяются только для защищённых веток.

## 📑 **Итог**

Переменные в **GitLab CI/CD** могут храниться:

- На уровне проекта
- На уровне группы
- На уровне пользователя
- В самом файле `.gitlab-ci.yml`

Это позволяет гибко управлять конфигурациями и секретами в процессе CI/CD, обеспечивая безопасность и доступность данных на разных уровнях.

</p></details>

---

### ❓ 3. Каковы основные встроенные переменные в GitLab CI/CD?

**[📺 Видео-вопрос (01:09:44)](https://youtu.be/z0QZ7VLyP7w?t=4184)**

<details><summary><strong>Показать ответ</strong></summary><p>

**GitLab CI/CD** предоставляет множество встроенных переменных, которые можно использовать в процессе выполнения пайплайнов. Эти переменные позволяют работать с данными о проекте, коммитах, задачах и окружениях. В этом документе представлены основные внутренние переменные, которые GitLab автоматически добавляет в пайплайн.

## 1. 🔑 **Основные переменные GitLab**

| Переменная               | Описание                                             | Пример использования       |
| ------------------------ | ---------------------------------------------------- | -------------------------- |
| **`CI`**                 | Указывает, что процесс работает в контексте CI/CD    | `echo $CI`                 |
| **`CI_COMMIT_SHA`**      | Хэш текущего коммита                                 | `echo $CI_COMMIT_SHA`      |
| **`CI_COMMIT_REF_NAME`** | Название ветки или тега, использованного для коммита | `echo $CI_COMMIT_REF_NAME` |
| **`CI_PIPELINE_ID`**     | Уникальный идентификатор пайплайна                   | `echo $CI_PIPELINE_ID`     |
| **`CI_PROJECT_NAME`**    | Имя проекта                                          | `echo $CI_PROJECT_NAME`    |
| **`CI_PROJECT_URL`**     | URL проекта на GitLab                                | `echo $CI_PROJECT_URL`     |
| **`CI_REGISTRY`**        | URL реестра контейнеров GitLab                       | `echo $CI_REGISTRY`        |
| **`CI_REGISTRY_USER`**   | Имя пользователя для входа в реестр контейнеров      | `echo $CI_REGISTRY_USER`   |
| **`CI_JOB_ID`**          | Уникальный идентификатор задачи (job)                | `echo $CI_JOB_ID`          |
| **`CI_JOB_NAME`**        | Имя текущей задачи (job)                             | `echo $CI_JOB_NAME`        |
| **`CI_JOB_STAGE`**       | Статья текущей задачи (job)                          | `echo $CI_JOB_STAGE`       |

## 2. 🔍 **Переменные для работы с окружениями**

| Переменная                    | Описание                                                | Пример использования            |
| ----------------------------- | ------------------------------------------------------- | ------------------------------- |
| **`CI_COMMIT_BRANCH`**        | Название ветки, в которой был сделан коммит             | `echo $CI_COMMIT_BRANCH`        |
| **`CI_ENVIRONMENT_NAME`**     | Имя окружения, на котором выполняется пайплайн          | `echo $CI_ENVIRONMENT_NAME`     |
| **`CI_COMMIT_REF_PROTECTED`** | Проверка, является ли ветка защищённой                  | `echo $CI_COMMIT_REF_PROTECTED` |
| **`CI_JOB_TOKEN`**            | Токен для аутентификации в API GitLab и других сервисах | `echo $CI_JOB_TOKEN`            |

## 3. 🧑‍💻 **Переменные для работы с пользователями и GitLab Runner**

| Переменная                  | Описание                                         | Пример использования          |
| --------------------------- | ------------------------------------------------ | ----------------------------- |
| **`CI_RUNNER_ID`**          | Уникальный идентификатор GitLab Runner           | `echo $CI_RUNNER_ID`          |
| **`CI_RUNNER_DESCRIPTION`** | Описание GitLab Runner, который выполняет задачу | `echo $CI_RUNNER_DESCRIPTION` |
| **`CI_SERVER_URL`**         | URL GitLab сервера                               | `echo $CI_SERVER_URL`         |

## 4. 🔄 **Переменные для деплоя и безопасности**

| Переменная              | Описание                                           | Пример использования      |
| ----------------------- | -------------------------------------------------- | ------------------------- |
| **`CI_DEPLOY_USER`**    | Пользователь, от имени которого выполняется деплой | `echo $CI_DEPLOY_USER`    |
| **`CI_COMMIT_MESSAGE`** | Сообщение коммита                                  | `echo $CI_COMMIT_MESSAGE` |
| **`CI_JOB_STAGE`**      | Статья текущей задачи (build, test, deploy)        | `echo $CI_JOB_STAGE`      |
| **`CI_JOB_STARTED_AT`** | Время начала выполнения текущей задачи             | `echo $CI_JOB_STARTED_AT` |

## 5. 🚀 **Пример использования переменных в `.gitlab-ci.yml`**

Внутренние переменные GitLab могут быть использованы для различных задач, таких как деплой, логирование и мониторинг.

**Пример в `.gitlab-ci.yml`:**

```yaml
stages:
  - build
  - deploy

variables:
  DEPLOY_ENV: 'production'

build:
  stage: build
  script:
    - echo "Build started at $CI_COMMIT_SHA"
    - echo "Deploying to $CI_ENVIRONMENT_NAME"

deploy:
  stage: deploy
  script:
    - echo "Deploying to $DEPLOY_ENV"
    - echo "Deployed by $CI_JOB_USER"
```

## 📑 **Итог**

В GitLab CI/CD доступно множество встроенных переменных, которые могут быть использованы для управления процессом CI/CD, мониторинга, логирования и деплоя. Эти переменные помогают автоматизировать различные части пайплайна, обеспечивая гибкость и безопасность на всех этапах разработки и развертывания.

</p></details>

---

### ❓ 4. Что такое триггеры в GitLab CI/CD?

**[📺 Видео-вопрос (01:12:39)](https://youtu.be/z0QZ7VLyP7w?t=4359)**

<details><summary><strong>Показать ответ</strong></summary><p>

- **Триггеры** в **GitLab CI/CD** — это механизмы, которые позволяют запускать пайплайны в ответ на определённые события или действия в GitLab. Триггеры помогают автоматизировать процессы, такие как сборка или деплой, без необходимости вручную инициировать эти процессы.

## 🧑‍💻 **Типы триггеров в GitLab CI/CD**

### 1. **Триггеры API (Trigger API)**

**Что это:** Эти триггеры позволяют запускать пайплайны через **GitLab API**. Это полезно, если нужно интегрировать GitLab CI с внешними системами или инструментами.

**Как использовать:** Создаётся **trigger token**, который позволяет инициировать запуск пайплайна через команду **`curl`** или любой другой инструмент для работы с HTTP-запросами.

**Пример:**

```bash
curl --request POST --form token=YOUR_TRIGGER_TOKEN --form ref=master https://gitlab.com/api/v4/projects/PROJECT_ID/trigger/pipeline
```

### 2. **Триггеры на основе событий (Event-based triggers)**

**Что это:** Пайплайны могут быть настроены на запуск в ответ на определённые события, такие как пуш в репозиторий, создание merge request, теги и другие действия.

**Пример:** Запуск пайплайна при пуше в ветку:

```yaml
push:
  branches:
    - master
```

### 3. **Триггеры на основе изменений в репозитории (Push and Merge Request Triggers)**

**Что это:** Эти триггеры запускают пайплайн при каждом пуше в репозиторий или при создании merge request.

**Пример:** В конфигурации `.gitlab-ci.yml` можно настроить пайплайн на пуш в определённую ветку или на создание pull request:

```yaml
stages:
  - test

test:
  script:
    - echo "Testing..."
  only:
    - master
```

### 4. **Триггеры с использованием `scheduled pipelines` (Запланированные пайплайны)**

**Что это:** Запуск пайплайнов по расписанию. Это полезно для автоматизации задач, которые должны выполняться в определённое время (например, еженедельные сборки, проверки безопасности и т.д.).

**Как настроить:** В интерфейсе GitLab можно настроить расписание запуска пайплайнов.

**Пример:** Запуск каждую неделю в понедельник:

```yaml
trigger:
  cron: '0 0 * * 1'
```

### 5. **Триггеры для внешних репозиториев (External Repository Triggers)**

**Что это:** Использование триггеров для запуска пайплайнов из других репозиториев GitLab.

**Пример:** Запуск пайплайна в проекте A, когда происходят изменения в проекте B, через API или webhook.

## 🚀 **Преимущества использования триггеров**

- **Автоматизация:** Триггеры позволяют автоматизировать запуск пайплайнов без участия человека.
- **Интеграция:** Легко интегрировать GitLab CI/CD с внешними системами и инструментами.
- **Гибкость:** Вы можете настроить триггеры под свои уникальные процессы, что делает их универсальными для разных типов задач.

## 📑 **Итог**

Триггеры в **GitLab CI/CD** позволяют запускать пайплайны на основе различных событий, таких как пуши, merge request, API запросы или по расписанию. Это мощный инструмент для автоматизации процессов и интеграции с другими системами.

</p></details>

---

# 5. Postgres

### ❓ 1. В чем отличие схемы от базы данных в PostgreSQL?

**[📺 Видео-вопрос (01:14:34)](https://youtu.be/z0QZ7VLyP7w?t=4474)**

<details><summary><strong>Показать ответ</strong></summary><p>

- В **PostgreSQL** понятия **схемы** и **базы данных** имеют различное значение и предназначение. Давайте разберемся, чем они отличаются:

## 1. 🗂️ **База данных (Database)**

- **Что это:** База данных в PostgreSQL представляет собой контейнер, который содержит все объекты для конкретного проекта, такие как таблицы, индексы, представления, функции и другие элементы.
- **Пример:** У вас может быть база данных `sales_db`, которая содержит информацию о продажах компании.
- **Особенности:**
  - Каждая база данных в PostgreSQL имеет свою изолированную среду.
  - Базы данных не могут быть вложены друг в друга.
  - Для работы с базой данных необходимо подключиться к ней непосредственно.

## 2. 🗃️ **Схема (Schema)**

- **Что это:** Схема — это логическое разделение внутри базы данных, которое группирует объекты, такие как таблицы, представления и индексы. Она помогает организовать и управлять объектами базы данных.
- **Пример:** В базе данных `sales_db` может быть схема `public`, где хранится общая информация о продажах, и схема `archived`, где хранятся старые данные.
- **Особенности:**
  - Схема — это просто структура для организации объектов в базе данных.
  - Схемы могут использоваться для разграничения данных между различными пользователями.
  - В PostgreSQL есть схема по умолчанию — это схема `public`, в которой создаются все объекты, если не указана другая схема.

## 📊 **Основные различия:**

| Характеристика      | **База данных**                                     | **Схема**                                      |
| ------------------- | --------------------------------------------------- | ---------------------------------------------- |
| **Уровень**         | Наиболее высокий уровень, контейнер для данных.     | Логическое разделение объектов в базе данных.  |
| **Изоляция**        | Базы данных изолированы друг от друга.              | Схемы существуют внутри одной базы данных.     |
| **Пример**          | `sales_db`, `users_db`                              | `public`, `admin`, `archived`                  |
| **Множественность** | В одной PostgreSQL может быть несколько баз данных. | В одной базе данных может быть несколько схем. |

## 💡 **Пример использования:**

1. **База данных**: `sales_db`
2. **Схемы в базе данных**: `public`, `archived`, `admin`

В **PostgreSQL** база данных служит для хранения всех данных, а схемы позволяют группировать и организовывать данные внутри этой базы.

</p></details>

---

### ❓ 6. Что такое WAL (Write-Ahead Logging) в PostgreSQL?

**[📺 Видео-вопрос (01:15:43)](https://youtu.be/z0QZ7VLyP7w?t=4543)**

<details><summary><strong>Показать ответ</strong></summary><p>

- **WAL (Write-Ahead Logging)** в **PostgreSQL** — это механизм ведения журнала транзакций, который используется для обеспечения целостности данных и восстановления базы данных в случае сбоя.

## 🛠️ **Как работает WAL?**

1. **Запись в журнал до изменения данных**:

   - При выполнении операции, например, вставки, обновления или удаления строки в таблице, PostgreSQL сначала записывает изменения в специальный журнал транзакций (**WAL**), а затем применяет изменения к данным.

2. **Принцип "Write-Ahead"**:

   - Принцип "Write-Ahead" заключается в том, что изменения должны быть записаны в журнал перед тем, как они будут отражены в данных на диске. Это обеспечивает защиту от потери данных, если система неожиданно отключится или произойдёт сбой.

3. **Реализация журналирования**:
   - Каждая запись в WAL — это лог-файл, который содержит информацию о том, какие изменения были сделаны, когда и в каких данных. Эти логи могут быть использованы для восстановления базы данных.

## 💡 **Зачем нужен WAL?**

1. **Восстановление после сбоя**:

   - Если PostgreSQL сбойнет до того, как транзакция была полностью завершена, система использует WAL для восстановления базы данных до согласованного состояния, применяя журнальные записи.

2. **Моментальная репликация**:

   - WAL используется для реализации **репликации** в PostgreSQL. Вторичные реплики могут следить за изменениями в главной базе данных и синхронизировать свои данные с основным сервером, используя логи WAL.

3. **Устойчивость к сбоям**:
   - WAL обеспечивает высокую степень устойчивости базы данных к сбоям, так как изменения не теряются благодаря тому, что они сначала записываются в журнал, а затем применяются к данным.

## 🔄 **Пример:**

Когда выполняется операция вставки:

```sql
INSERT INTO employees (id, name, position) VALUES (1, 'John Doe', 'Manager');
```

- PostgreSQL сначала записывает информацию об этой вставке в журнал WAL, чтобы сохранить информацию о её выполнении.
- Затем сама вставка данных происходит в таблице.

Если система падает в момент записи в таблицу, PostgreSQL сможет восстановить данные, используя записи в журнале WAL, обеспечивая согласованность базы данных.

## 🏆 **Преимущества WAL:**

- **Безопасность данных**: Данные не будут потеряны при сбоях, так как все изменения сначала записываются в журнал.
- **Производительность**: WAL позволяет ускорить операции записи, так как записи в журнал выполняются быстрее, чем непосредственная запись в данные на диске.
- **Репликация и восстановление**: WAL используется для репликации данных между серверами и для восстановления после сбоя.

## 📑 **Итог:**

**WAL (Write-Ahead Logging)** — это важная функция в PostgreSQL, которая гарантирует целостность данных, позволяет восстанавливать базу данных после сбоев и поддерживает репликацию данных между серверами.

</p></details>

---

### ❓ 7. Что такое PG HBA, что за файл такой?

**[📺 Видео-вопрос (01:15:51)](https://youtu.be/z0QZ7VLyP7w?t=4551)**

<details><summary><strong>Показать ответ</strong></summary><p>

- **PG HBA** (PostgreSQL Host-Based Authentication) — это метод аутентификации в PostgreSQL, который контролирует доступ к базе данных на основе хостов и других параметров. Он конфигурируется в файле **`pg_hba.conf`** и определяет, кто и каким способом может подключаться к базе данных.

## 🛠️ **Как работает HBA в PostgreSQL?**

1. **Конфигурация через `pg_hba.conf`:**

   - Файл **`pg_hba.conf`** находится в каталоге данных PostgreSQL и используется для указания правил доступа.
   - В этом файле задаются правила, которые определяют:
     - **Тип подключения** (например, `host` или `local`).
     - **Базу данных**, к которой разрешено подключение.
     - **Пользователя**, которому разрешено подключение.
     - **IP-адрес клиента** (для подключения через сеть).
     - **Метод аутентификации** (например, `md5`, `trust`, `peer`).

2. **Правила подключения:**
   - Правила обрабатываются сверху вниз, и как только найдется подходящее правило, оно применяется.

## 🔐 **Пример строки в `pg_hba.conf`**:

```plaintext
# Тип   База данных   Пользователь   Хост              Метод аутентификации
host    all            all            192.168.1.0/24     md5
```

- **Тип**: `host` — подключение по сети (TCP/IP).
- **База данных**: `all` — разрешено подключаться ко всем базам данных.
- **Пользователь**: `all` — разрешено подключаться всем пользователям.
- **Хост**: `192.168.1.0/24` — подключение разрешено только с IP-адресов в диапазоне `192.168.1.0` до `192.168.1.255`.
- **Метод аутентификации**: `md5` — аутентификация по паролю с использованием хеширования MD5.

## 🔑 **Основные методы аутентификации в PostgreSQL:**

1. **`trust`**: Разрешает подключение без пароля.
2. **`md5`**: Требует аутентификацию с использованием пароля, хешированного MD5.
3. **`peer`**: Для локальных соединений проверяет имя пользователя на операционной системе.
4. **`password`**: Требует пароль в открытом виде.
5. **`scram-sha-256`**: Современный метод с использованием хеширования SCRAM-SHA-256.

## 🔒 **Зачем использовать PG HBA?**

- **Безопасность**: HBA позволяет точно контролировать, кто и с какого хоста может подключаться к базе данных.
- **Гибкость**: Вы можете настроить различные правила для различных хостов и пользователей.
- **Простота в настройке**: Конфигурация через **`pg_hba.conf`** делает управление безопасностью удобным.

## 📑 **Итог:**

**PG HBA** — это метод аутентификации, который обеспечивает безопасность и контроль доступа к базе данных PostgreSQL на основе хостов и пользователей. Он конфигурируется в файле **`pg_hba.conf`** и позволяет гибко управлять подключениями и методами аутентификации.

</p></details>
