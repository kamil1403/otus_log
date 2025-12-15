<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/35/Tux.svg/1200px-Tux.svg.png" alt="Linux Logo" width="10%">
</p>

## ![Lesson](https://img.shields.io/badge/Lesson-otus__logs-0A84FF?style=for-the-badge&logo=linux&logoColor=white&labelColor=111827)![Author](https://img.shields.io/badge/Author-Kamil%20Ibragimov-10B981?style=for-the-badge&logo=github&logoColor=white&labelColor=111827)![Date](https://img.shields.io/badge/Date-15.12.2025-F59E0B?style=for-the-badge&logo=calendar&logoColor=white&labelColor=111827)

### 📌 Задание
1. Настроить централизованный сбор логов (rsyslog).
2. `web`: отправляет логи Nginx на удаленный сервер.
3. `log`: принимает логи и раскладывает по папкам.

### ✅ Результат
- [x] Стенд развернут (Vagrant).
- [x] Логи Nginx пишутся на удаленный сервер.
- [x] Тесты пройдены. Результат см. на скриншоте:
  - 🖼️ [Логи на сервере](logs_proof.png)

### 🧭 Оглавление
- [🧰 Шаг 1 - Автоматизация Vagrant](#one)
- [🧰 Шаг 2 - Проверка](#two)

---

<a id="one"></a>
## 🧰 Шаг 1 - Автоматизация Vagrant
Настройка полностью автоматизирована через `Vagrantfile`.

**Log Server (192.168.56.15):**
* Открыты порты 514 (UDP/TCP).
* Настроен путь: `/var/log/rsyslog/%HOSTNAME%/%PROGRAMNAME%.log`.

**Web Server (192.168.56.10):**
* Установлен Nginx и Auditd.
* Логи Nginx (`access_log`, `error_log`) перенаправлены в syslog.
* Настроено правило аудита: `-w /etc/nginx/nginx.conf -p wa`.

<a id="two"></a>
## 🧰 Шаг 2 - Проверка
**1. Генерация событий на клиенте (Web):**
```bash
curl [http://192.168.56.10](http://192.168.56.10)
vagrant ssh web -c "sudo touch /etc/nginx/nginx.conf"
root@log:~# ls -R /var/log/rsyslog/
/var/log/rsyslog/:
log  web

/var/log/rsyslog/web:
nginx_access.log
