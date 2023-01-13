<h1>AdGuard DNS Home
Dockerized installation of AdGuard DNS Home

<h2>Настройка резолвинга на хосте</h2>
Если попытаться включить AdGuard DNS Home на хосте, где поднят демон <code>resolving</code>, AdGuard DNS Home не будет принимать в себя соединения по 53 порту, потому что демон слушает 127.0.0.53:53. Необходимо выключить <code>DNSStubListener</code> на хосте. Пример для Debain:
<br><br>1. Деактивация <code>DNSStubListener</code> и обновление DNS адреса. Создаем новый файл <code>/etc/systemd/resolved.conf.d/adguardhome.conf</code> и добавляем в него следующее:
```
  [Resolve]
  DNS=127.0.0.1
  DNSStubListener=no
```
  
Указать необходимо 127.0.0.1 так как все соединения по 53 порту он будет прокидывать внутрь себя, что приведет к отправке запроса в докер-контейнер.
<br><br>2. Задаем новый <code>resolv.conf</code>:
```
  mv /etc/resolv.conf /etc/resolv.conf.backup
  ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
```
  
<br><br>3. Выключаем <code>DNSStubListener</code> на хосте:
```
  systemctl stop systemd-resolved
  systemctl disable systemd-resolved
```
