# Configure AAA Authentication
[Practical_File](AAA_Authentication.pkt)

1

![Image Alt Text](AAA%20Authentication.png)

---
2

![Image Alt Text](AAA%20Authentication2.png)

---
3

![Image Alt Text](AAA%20Authentication3.png)

### Type the following commands in the CLI mode of the Router0
```bash
Router(config)#aaa new-model
Router(config)#tacacs-server host 192.168.2.3 key cisco
Router(config)#radius-server host 192.168.2.2 key cisco
Router(config)#aaa authentication login ismail group tacacs+ group radius local
Router(config)#line vty 0 4
Router(config-line)#login authentication ismail
Router(config-line)#exit
Router(config)#
```
![Image Alt Text](AAA%20Authentication4.png)