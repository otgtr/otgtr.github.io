<<<<<<< HEAD
---
title: Yasaklı Sitelere Erişim Engeli Kaldırma
---

# Yasaklı Sitelere Erişim Engeli Kaldırma

Biliyorsunuz ülkemizde gün geçtikte çoğu uygulama ve internet sitesi erişim engeline maruz kalıyor. Ülkemizdeki internet servis sağlayıcıları özellikle (Türk Telekom ve Superonline) başta olmak üzere DPI yani Deep Packet Inspection, türkçe tabiriyle Derin Paket İnceleme adında bir filtreleme yöntemi kullanıyor. Bunun sonucunda bu özel yönteme karşın DNS değiştirme, VPN kullanımı gibi çözümler de işe yaramamaya başladı. İşte bu engeli/sansürü aşmak için kullanacağımız bir yazılım var.

GoodbyeDPI (Windows) ve Zapret (Linux için)

## Windows

[GoodbyeDPI'yi](https://github.com/cagritaskn/GoodbyeDPI-Turkey) ekran görüntüleri ile verdiğim talimatlarla indiriyoruz.

Veya direkt indirmek isteyenler için link: [goodbyedpi-0.2.3rc3-turkey.zip](https://github.com/cagritaskn/GoodbyeDPI-Turkey/releases/download/release-0.2.3rc3-turkey/goodbyedpi-0.2.3rc3-turkey.zip)

![Goodbyedpi1](https://i.postimg.cc/yxJz0Y3J/ihr9twzywx.png)

 **goodbyedpi-0.2.3rc3-turkey.zip** dosyasına tıklayarak indiriyoruz.

![goodbyedpi2](https://i.postimg.cc/hjXS10HP/2h-SFI4dj-QW.png)

İndirdiğimiz dosyayı sağ tık yaparak 7-zip veya WinRAR ile klasöre ayıklıyoruz ve silinmemesi için C: diskine atıyoruz. Dilerseniz Windows'un "Tümünü Ayıkla" özelliğini de kullanabilirsiniz.

![goodbyedpi3](https://i.postimg.cc/VsTJpJRV/f-W7q-EC7-M1-Z.png)

![goodbyedpi4](https://i.postimg.cc/W4X3nzzS/u1-D75qpj-V7.png)

Şimdi normal bir kullanıcı klasör içerisindeki **service_install_dnsredir_turkey.cmd** ve eğer Superonline kullanıyorsanız **service_install_dnsredir_turkey_alternative_superonline.cmd** dosyalarını yönetici olarak çalıştırıp enter'a basıp bilgisayarı yeniden başlatabilirler. Böylece erişim engeliniz kalkmış olacak.

![goodbyedpi5](https://i.postimg.cc/rm4pLJgZ/Ds-IMWe-GOYv.png)

Ancak OPSEC'e önem verenler için not etmek istiyorum: Bu yazılım, otomatik olarak Yandex DNS ve port 1253 kullanıyor. Rus tabanlı bir DNS ve kayıt tutma politikası bilinmiyor. Bu yüzden güvenlik açısından bazı parametreleri değiştirmek gerekebiliyor.

Superonline dışında bir sağlayıcı kullanıyorsanız **service_install_dnsredir_turkey.cmd** dosyasına sağ tık yapıp not defteri ile açın.

Karşımıza birkaç kod satırı çıktı, burada ekran görüntüsünde belirttiğim satıra geliyoruz ve Yandex DNS yerine daha güvenli DNS hizmeti kullanıyoruz.

![goodbyedpi6](https://i.postimg.cc/NGz5rK2x/nlv0e-Fhk60.png)

| DNS Hizmetleri   | DNS Adresi (IPV4) | Port | DNS Adresi (IPV6) |
| ---------------- | ----------------- | -----| ------------------ |
| Quad9            | 9.9.9.9           |   53    |       2620:fe::fe             |
| Cloudflare       | 1.1.1.1           |    53   |    2606:4700:4700::1111                |
| ControlD         | 76.76.2.5         |    53   |         2606:1a40::5           |

Ben Quad9 kullanıyorum, gayet güzel ve hızlı çalışıyor. Son ayarlarım ise şu şekilde:

![goodbyedpi7](https://i.postimg.cc/mkyzvsSg/notepad-Pv-Q10sj0-PY.png)

## Linux
### DNS Değiştirme

Zapret'i indirmeden önce DNS'i ayarlamamız gerekiyor. Wi-Fi ve Ağ kısmından IPV4 sekmesine gelip yöntemi "Kendiliğinden (Yalnızca Adresler)" olarak ayarlıyoruz bir DNS sağlayıcısının IPV4 DNS Adresini yazıyoruz. Ben yine Quad9 kullanıyorum.

![zapret2](https://i.postimg.cc/hGTbMh5k/BZYn-OXLw-PU.png)

Terminali açıp `sudo su` komutunu giriyoruz.

![zapret1](https://i.postimg.cc/TYCVHV1v/Virtual-Box-kdeneon-15-11-2024-17-05-03.png)

### Git İndirme

Ardından sistemimizde git yoksa aşağıdaki komut ile indiriyoruz.

Ubuntu ve Debian:

```bash
sudo apt install git
```

Arch:

```bash
sudo pacman -S git
```

Fedora:

```bash
sudo dnf install git
```

CentOS / RHEL:

*CentOS/RHEL 7 ve öncesi için*:

```bash
sudo yum install git
```

*CentOS/RHEL 8 ve sonrası için*:

```bash
sudo dnf install git
```

openSUSE:

```bash
sudo zypper install git
```

Alpine Linux:

```ash
sudo apk install git
```

### Zapret'i İndirme ve Kurulum

Terminalden zapret'i şu komut ile indiriyoruz.

```bash
git clone https://github.com/bol-van/zapret.git
```
Kurulum için:

```bash
cd zapret
```

```bash
sudo ./install_prereq.sh
```
```bash
sudo ./install_bin.sh
```

Aşağıdaki komutu yazdıktan sonra test etmek istediğimiz siteyi yazıyoruz (örn: pastebin.com). Ardından Çıkan sorular için enter'a basın.
```bash
sudo ./blockcheck.sh
```

Bizim için önemli kısım bu, enter diyip geçmiyoruz ve dördüncü satıra `nfqws --dpi-desync-fake --dpi-desync-ttl=3` ekliyoruz ve ardından Enter'a basabiliriz.
```bash
ipv4 pastebin.com curl_test_http ipv4 pastebin.com curl_test_http
tpws --hostspell=host nfqws --hostspell-host
ipv4 pastebin.com curl_test_https_tls12: tpws not working
ipv4 pastebin.com curl_test_https_tls12:

Press enter to continue
```

Yazılımın son adımlarını kuruyoruz.

```bash
sudo ./install_easy.sh
```
Şimdi sorulan sorulara komut satırında (Y/N) şeklinde cevap verildiği gibi cevap veriyoruz. Boş bıraktığım yerlerde ENTER basıp geçin.
```bash
do you want the installer to copy it for you (default: N) (Y/N) ? Y

select firewall type : 
1: iptables
2: nftables
your choice (default: nftables) : 2


enable ipv6 support (default: N) (Y/N) ?

1: none 
2: software
3: hardware
your choice (default: none) :


select MODE : 
1: tpws
2: tpws-socks
3: nfqws 
4: filter
5: custom
your choice (default: tpws) : 3

do you want to edit the options (default: N) (Y/N) ? Y
```

Gelen nano ekranında parametreler böyle düzenlenmeli.

```bash
NFQWS_OPT_DESYNC="--dpi-desync=fake --dpi-desync-ttl=3"
NFQWS_OPT_DESYNC_HTTP=""
NFQWS_OPT_DESYNC_HTTPS=""
NFQWS_OPT_DESYNC_HTTP6=""
NFQWS_OPT_DESYNC_HTTPS6="
NFQWS_OPT_DESYNC_QUIC="--dpi-desync=fake"
NFQWS_OPT_DESYNC_QUIC6=""
```






=======
---
title: Yasaklı Sitelere Erişim Engeli Kaldırma
---

# Yasaklı Sitelere Erişim Engeli Kaldırma

Biliyorsunuz ülkemizde gün geçtikte çoğu uygulama ve internet sitesi erişim engeline maruz kalıyor. Ülkemizdeki internet servis sağlayıcıları özellikle (Türk Telekom ve Superonline) başta olmak üzere DPI yani Deep Packet Inspection, türkçe tabiriyle Derin Paket İnceleme adında bir filtreleme yöntemi kullanıyor. Bunun sonucunda bu özel yönteme karşın DNS değiştirme, VPN kullanımı gibi çözümler de işe yaramamaya başladı. İşte bu engeli/sansürü aşmak için kullanacağımız bir yazılım var.

GoodbyeDPI (Windows) ve Zapret (Linux için)

## Windows

[GoodbyeDPI'yi](https://github.com/cagritaskn/GoodbyeDPI-Turkey) ekran görüntüleri ile verdiğim talimatlarla indiriyoruz.

Veya direkt indirmek isteyenler için link: [goodbyedpi-0.2.3rc3-turkey.zip](https://github.com/cagritaskn/GoodbyeDPI-Turkey/releases/download/release-0.2.3rc3-turkey/goodbyedpi-0.2.3rc3-turkey.zip)

![Goodbyedpi1](https://i.postimg.cc/yxJz0Y3J/ihr9twzywx.png)

 **goodbyedpi-0.2.3rc3-turkey.zip** dosyasına tıklayarak indiriyoruz.

![goodbyedpi2](https://i.postimg.cc/hjXS10HP/2h-SFI4dj-QW.png)

İndirdiğimiz dosyayı sağ tık yaparak 7-zip veya WinRAR ile klasöre ayıklıyoruz ve silinmemesi için C: diskine atıyoruz. Dilerseniz Windows'un "Tümünü Ayıkla" özelliğini de kullanabilirsiniz.

![goodbyedpi3](https://i.postimg.cc/VsTJpJRV/f-W7q-EC7-M1-Z.png)

![goodbyedpi4](https://i.postimg.cc/W4X3nzzS/u1-D75qpj-V7.png)

Şimdi normal bir kullanıcı klasör içerisindeki **service_install_dnsredir_turkey.cmd** ve eğer Superonline kullanıyorsanız **service_install_dnsredir_turkey_alternative_superonline.cmd** dosyalarını yönetici olarak çalıştırıp enter'a basıp bilgisayarı yeniden başlatabilirler. Böylece erişim engeliniz kalkmış olacak.

![goodbyedpi5](https://i.postimg.cc/rm4pLJgZ/Ds-IMWe-GOYv.png)

Ancak OPSEC'e önem verenler için not etmek istiyorum: Bu yazılım, otomatik olarak Yandex DNS ve port 1253 kullanıyor. Rus tabanlı bir DNS ve kayıt tutma politikası bilinmiyor. Bu yüzden güvenlik açısından bazı parametreleri değiştirmek gerekebiliyor.

Superonline dışında bir sağlayıcı kullanıyorsanız **service_install_dnsredir_turkey.cmd** dosyasına sağ tık yapıp not defteri ile açın.

Karşımıza birkaç kod satırı çıktı, burada ekran görüntüsünde belirttiğim satıra geliyoruz ve Yandex DNS yerine daha güvenli DNS hizmeti kullanıyoruz.

![goodbyedpi6](https://i.postimg.cc/NGz5rK2x/nlv0e-Fhk60.png)

| DNS Hizmetleri   | DNS Adresi (IPV4) | Port | DNS Adresi (IPV6) |
| ---------------- | ----------------- | -----| ------------------ |
| Quad9            | 9.9.9.9           |   53    |       2620:fe::fe             |
| Cloudflare       | 1.1.1.1           |    53   |    2606:4700:4700::1111                |
| ControlD         | 76.76.2.5         |    53   |         2606:1a40::5           |

Ben Quad9 kullanıyorum, gayet güzel ve hızlı çalışıyor. Son ayarlarım ise şu şekilde:

![goodbyedpi7](https://i.postimg.cc/mkyzvsSg/notepad-Pv-Q10sj0-PY.png)

## Linux
### DNS Değiştirme

Zapret'i indirmeden önce DNS'i ayarlamamız gerekiyor. Wi-Fi ve Ağ kısmından IPV4 sekmesine gelip yöntemi "Kendiliğinden (Yalnızca Adresler)" olarak ayarlıyoruz bir DNS sağlayıcısının IPV4 DNS Adresini yazıyoruz. Ben yine Quad9 kullanıyorum.

![zapret2](https://i.postimg.cc/hGTbMh5k/BZYn-OXLw-PU.png)

Terminali açıp `sudo su` komutunu giriyoruz.

![zapret1](https://i.postimg.cc/TYCVHV1v/Virtual-Box-kdeneon-15-11-2024-17-05-03.png)

### Git İndirme

Ardından sistemimizde git yoksa aşağıdaki komut ile indiriyoruz.

Ubuntu ve Debian:

```bash
sudo apt install git
```

Arch:

```bash
sudo pacman -S git
```

Fedora:

```bash
sudo dnf install git
```

CentOS / RHEL:

*CentOS/RHEL 7 ve öncesi için*:

```bash
sudo yum install git
```

*CentOS/RHEL 8 ve sonrası için*:

```bash
sudo dnf install git
```

openSUSE:

```bash
sudo zypper install git
```

Alpine Linux:

```ash
sudo apk install git
```

### Zapret'i İndirme ve Kurulum

Terminalden zapret'i şu komut ile indiriyoruz.

```bash
git clone https://github.com/bol-van/zapret.git
```
Kurulum için:

```bash
cd zapret
```

```bash
sudo ./install_prereq.sh
```
```bash
sudo ./install_bin.sh
```

Aşağıdaki komutu yazdıktan sonra test etmek istediğimiz siteyi yazıyoruz (örn: pastebin.com). Ardından Çıkan sorular için enter'a basın.
```bash
sudo ./blockcheck.sh
```

Bizim için önemli kısım bu, enter diyip geçmiyoruz ve dördüncü satıra `nfqws --dpi-desync-fake --dpi-desync-ttl=3` ekliyoruz ve ardından Enter'a basabiliriz.
```bash
ipv4 pastebin.com curl_test_http ipv4 pastebin.com curl_test_http
tpws --hostspell=host nfqws --hostspell-host
ipv4 pastebin.com curl_test_https_tls12: tpws not working
ipv4 pastebin.com curl_test_https_tls12:

Press enter to continue
```

Yazılımın son adımlarını kuruyoruz.

```bash
sudo ./install_easy.sh
```
Şimdi sorulan sorulara komut satırında (Y/N) şeklinde cevap verildiği gibi cevap veriyoruz. Boş bıraktığım yerlerde ENTER basıp geçin.
```bash
do you want the installer to copy it for you (default: N) (Y/N) ? Y

select firewall type : 
1: iptables
2: nftables
your choice (default: nftables) : 2


enable ipv6 support (default: N) (Y/N) ?

1: none 
2: software
3: hardware
your choice (default: none) :


select MODE : 
1: tpws
2: tpws-socks
3: nfqws 
4: filter
5: custom
your choice (default: tpws) : 3

do you want to edit the options (default: N) (Y/N) ? Y
```

Gelen nano ekranında parametreler böyle düzenlenmeli.

```bash
NFQWS_OPT_DESYNC="--dpi-desync=fake --dpi-desync-ttl=3"
NFQWS_OPT_DESYNC_HTTP=""
NFQWS_OPT_DESYNC_HTTPS=""
NFQWS_OPT_DESYNC_HTTP6=""
NFQWS_OPT_DESYNC_HTTPS6="
NFQWS_OPT_DESYNC_QUIC="--dpi-desync=fake"
NFQWS_OPT_DESYNC_QUIC6=""
```






>>>>>>> eb8b97b (Initial commit)
