# Домашнее задание к занятию «Защита сети»

**Ушаков Игорь Юрьевич**

## Задание 1

При сканировании с Kali, Suricata зафиксировала множество сетевых потоков (flow) на различные порты. Это характерно для сканирования портов. Примеры строк из eve.json:
```
"event_type":"flow","src_ip":"172.28.1.146","dest_ip":"172.28.1.156","dest_port":22
"event_type":"flow","src_ip":"172.28.1.146","dest_ip":"172.28.1.156","dest_port":80
"event_type":"flow","src_ip":"172.28.1.146","dest_ip":"172.28.1.156","dest_port":3306
```
Suricata в режиме IDS зафиксировала аномальную сетевую активность — множественные подключения с одного IP на разные порты, что является признаком port scan. Fail2Ban на сканирование не реагирует, так как его задача — защита от брутфорса SSH.

## Задание 2


```
user@user-VirtualBox:/tmp$ sudo tail -f /var/log/fail2ban.log
2026-09-23 14:20:53,616 fail2ban.jail           [3640]: INFO    Jail 'sshd' uses systemd {}
2026-09-23 14:20:53,616 fail2ban.jail           [3640]: INFO    Initiated 'systemd' backend
2026-09-23 14:20:53,617 fail2ban.filter         [3640]: INFO      maxLines: 1
2026-09-23 14:20:53,632 fail2ban.filtersystemd  [3640]: INFO    [sshd] Added journal match for: '_SYSTEMD_UNIT=sshd.service + _COMM=sshd'
2026-09-23 14:20:53,632 fail2ban.filter         [3640]: INFO      maxRetry: 3
2026-09-23 14:20:53,632 fail2ban.filter         [3640]: INFO      findtime: 600
2026-09-23 14:20:53,632 fail2ban.actions        [3640]: INFO      banTime: 600
2026-09-23 14:20:53,632 fail2ban.filter         [3640]: INFO      encoding: UTF-8
2026-09-23 14:20:53,633 fail2ban.jail           [3640]: INFO    Jail 'sshd' started
2026-09-23 14:20:53,633 fail2ban.filtersystemd  [3640]: INFO    [sshd] Jail is in operation now (process new journal entries)
2026-09-23 15:03:19,699 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,699 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,699 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,700 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,700 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,700 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,700 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,730 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,731 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,742 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,742 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,750 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,751 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:19
2026-09-23 15:03:19,972 fail2ban.actions        [3640]: NOTICE  [sshd] Ban 172.28.1.146
2026-09-23 15:03:21,878 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,880 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,880 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,881 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,881 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,882 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,883 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,884 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,885 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,885 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,886 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,886 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,887 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,887 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,888 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,888 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,888 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,889 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,889 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,890 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,890 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
2026-09-23 15:03:21,891 fail2ban.filter         [3640]: INFO    [sshd] Found 172.28.1.146 - 2026-09-23 15:03:21
```
Fail2Ban обнаружил множественные неудачные попытки входа по SSH с IP 172.28.1.146 (Kali) и после третьей попытки (параметр maxretry = 3 в jail.local) заблокировал этот IP на 10 минут (bantime = 600)

Что попало в логи Suricata:
При брутфорсе SSH Suricata фиксирует множество TCP-подключений к порту 22 с IP Kali. Это видно как серия flow-событий с dest_port: 22. При загруженных правилах ET Open также мог бы появиться алерт ET SCAN Potential SSH Scan, но в  конфигурации (sslipblacklist) только flow-события.

Fail2Ban обнаружил 3 неудачные попытки входа за 10 минут и заблокировал IP атакующего 172.28.1.146 на 10 минут. Hydra после этого не смогла продолжить перебор — соединения с Kali блокируются на уровне iptables. Suricata зафиксировала аномальную активность (множественные подключения к порту 22), но не сгенерировала алерт, так как правила ET Open не были загружены. P.S. Были проблемы с Suricata, но что должно быть я понял. 

![alt text](https://github.com/username/reponame/blob/branch/path/image.png)
