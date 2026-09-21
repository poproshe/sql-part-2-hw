# Домашнее задание к занятию «Анализ уязвимостей Metasploitable»

**Ушаков Игорь Юрьевич**

## Задание 1

В Metasploitable открыты следующие сетевые службы:
```
Порт	Служба	Версия
21/tcp	FTP	vsftpd 2.3.4
22/tcp	SSH	OpenSSH 4.7p1
23/tcp	Telnet	Linux telnetd
25/tcp	SMTP	Postfix smtpd
53/tcp	DNS	ISC BIND 9.4.2
80/tcp	HTTP	Apache httpd 2.2.8
111/tcp	RPCBind	v2
139, 445/tcp	SMB	Samba smbd 3.X
512/tcp	rexec	netkit-rsh
513/tcp	rlogin	OpenBSD/Solaris rlogind
514/tcp	rsh	tcpwrapped
1099/tcp	Java RMI	GNU Classpath grmiregistry
1524/tcp	bindshell	Metasploitable root shell
2049/tcp	NFS	v2-4
2121/tcp	FTP	ProFTPD 1.3.1
3306/tcp	MySQL	5.0.51a
5432/tcp	PostgreSQL	8.3.0–8.3.7
5900/tcp	VNC	protocol 3.3
6000/tcp	X11	access denied
6667/tcp	IRC	UnrealIRCd
8009/tcp	AJP13	Apache Jserv
8180/tcp	HTTP	Apache Tomcat 5.5
```
Уязвимости, которые были обнаружены:
vsftpd 2.3.4 — Backdoor Command Execution
Ссылка: https://www.exploit-db.com/exploits/17491
CVE-2011-2523. В дистрибутив vsftpd 2.3.4 был внедрён бэкдор: при отправке имени пользователя, содержащего :), на порту 6200 открывается root-шелл.

UnrealIRCd 3.2.8.1 — Backdoor Command Execution
Ссылка: https://www.exploit-db.com/exploits/16922
CVE-2010-2075. В исходный код UnrealIRCd 3.2.8.1 была добавлена вредоносная вставка, позволяющая выполнить произвольные команды через специальную строку.

Samba 3.0.20 < 3.0.25rc3 — Username map script Command Execution
Ссылка: https://www.exploit-db.com/exploits/16320
CVE-2007-2447. Уязвимость в опции username map script: через метасимволы оболочки в имени пользователя можно выполнить произвольные команды на сервере.


## Задание 2

Что я делал
Запустил Wireshark в Kali Linux, выбрал интерфейс с адресом 172.28.1.146 и поставил фильтр ip.addr == 172.28.1.145, чтобы видеть только трафик между Kali и Metasploitable. Затем по очереди выполнил четыре сканирования, каждый раз останавливал захват и сохранял результат в отдельный файл:

```
sudo nmap -sS -p 1-1000 172.28.1.145   # SYN
sudo nmap -sF -p 1-1000 172.28.1.145   # FIN
sudo nmap -sX -p 1-1000 172.28.1.145   # Xmas
sudo nmap -sU -p 1-1000 172.28.1.145   # UDP
```
После каждого сканирования сохранял захват в отдельный .pcapng-файл.

Отличия режимов
SYN (-sS) — nmap шлёт пакет с флагом SYN. Открытый порт отвечает SYN+ACK, сканер сразу отправляет RST (соединение не устанавливается). Закрытый порт отвечает RST.

FIN (-sF) — шлётся пакет только с флагом FIN. Открытые порты игнорируют его, закрытые отвечают RST.

Xmas (-sX) — то же, что FIN, но с флагами FIN+PSH+URG. Открытые порты молчат, закрытые отвечают RST.

UDP (-sU) — шлётся пустой UDP-пакет. Закрытый порт отвечает ICMP Port Unreachable, открытый может ответить UDP-пакетом или промолчать.

Как отвечает сервер
Режим	Открытый порт	Закрытый порт
SYN	SYN+ACK	RST
FIN	нет ответа	RST
Xmas	нет ответа	RST
UDP	UDP-ответ или тишина	ICMP Port Unreachable
В Wireshark это видно по TCP-флагам: в SYN-скане 0x0002 → 0x0012 → 0x0004, в FIN-скане 0x0001 → 0x0014, в Xmas-скане 0x0029 → 0x0014. В UDP-скане вместо TCP-флагов — ICMP-пакеты с типом 3.

