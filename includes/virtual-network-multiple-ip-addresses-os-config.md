---
title: include dosyası
description: include dosyası
services: virtual-network
author: jimdial
ms.service: virtual-network
ms.topic: include
ms.date: 05/10/2019
ms.author: anavin
ms.custom: include file
ms.openlocfilehash: 93caf39216ef0479ec2799267a9ba8181f37f802
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84194211"
---
## <a name="add-ip-addresses-to-a-vm-operating-system"></a><a name="os-config"></a>Bir VM işletim sistemine IP adresleri ekleme

Birden çok özel IP adresi ile oluşturduğunuz bir VM 'ye bağlanın ve oturum açın. Sanal makineye eklediğiniz tüm özel IP adreslerini (birincil adres dahil) el ile eklemeniz gerekir. VM işletim sisteminiz için aşağıdaki adımları izleyin.

### <a name="windows-server"></a>Windows Server
<details>
  <summary>Genişlet</summary>

1. Bir komut isteminde *ipconfig/all* yazın.  Yalnızca *Birincil* özel IP adresini (DHCP üzerinden) görürsünüz.
2. Komut istemine *ncpa.cpl* yazarak **Ağ bağlantıları** penceresini açın.
3. Uygun bağdaştırıcının özelliklerini açın: **Ethernet**.
4. İnternet Protokolü sürüm 4 (IPv4) öğesine çift tıklayın.
5. **Aşağıdaki IP adresini kullan**’ı seçip aşağıdaki değerleri girin:

    * **IP adresi**: *Birincil* özel IP adresini girin
    * **Alt ağ maskesi**: Alt ağınıza göre ayarlanır. Örneğin, alt ağ bir /24 alt ağı ise alt ağ maskesi 255.255.255.0 şeklindedir.
    * **Varsayılan ağ geçidi**: Alt ağdaki ilk IP adresi. Alt ağınız 10.0.0.0/24 ise ağ geçidi IP adresi 10.0.0.1 şeklindedir.
    * **AŞAĞıDAKI DNS sunucusu adreslerini kullan** ' ı seçin ve aşağıdaki değerleri girin:
        * **Tercih edilen DNS sunucusu**: Kendi DNS sunucunuzu kullanmıyorsanız 168.63.129.16 girin.  Kendi DNS sunucunuzu kullanıyorsanız kendi sunucunuzun IP adresini girin.  (Alternatif DNS sunucusu için herhangi bir ücretsiz genel DNS sunucusu adresi seçebilirsiniz.)
    * **Gelişmiş** düğmesini seçin ve ek IP adresleri ekleyin. Önceki bir adımda Azure ağ arabirimine eklediğiniz tüm ikincil özel IP adreslerini, Azure ağ arabirimine atanan birincil IP adresinin atandığı Windows ağ arabirimine ekleyin.

        Sanal makinenin işletim sistemi içindeki bir Azure sanal makinesine atanan genel IP adresini asla el ile atamanız gerekir. IP adresini işletim sistemi içinde el ile ayarladığınızda, Azure [ağ arabirimine](../articles/virtual-network/virtual-network-network-interface-addresses.md#change-ip-address-settings)ATANMıŞ özel IP adresi ile aynı adresle aynı olduğundan emin olun veya sanal makineyle olan bağlantıyı kaybedebilirsiniz. [Özel IP adresi](../articles/virtual-network/virtual-network-network-interface-addresses.md#private) ayarları hakkında daha fazla bilgi edinin. Bir Azure genel IP adresini hiçbir şekilde işletim sistemi içinde atamamalısınız.

    * **Tamam**’a tıklayarak TCP/IP ayarlarını kapatın ve ardından tekrar **Tamam**’a tıklayarak bağdaştırıcı ayarlarını kapatın. RDP bağlantınız yeniden kurulur.

6. Bir komut isteminde *ipconfig/all* yazın. Eklediğiniz tüm IP adreslerinin gösterildiğini ve DHCP 'nin kapalı olduğunu doğrulayın.
7. Windows 'ı, Windows için birincil IP adresi olarak Azure 'daki birincil IP yapılandırmasının özel IP adresini kullanacak şekilde yapılandırın. Ayrıntılar için [birden çok IP adresine sahip Azure WINDOWS VM 'Den Internet erişimi yok](https://support.microsoft.com/help/4040882/no-internet-access-from-azure-windows-vm-that-has-multiple-ip-addresse) ' a bakın. 

### <a name="validation-windows-server"></a>Doğrulama (Windows Server)

İkincil IP yapılandırmanızda ilişkili genel IP ile internet 'e bağlanabildiğinizden emin olmak için, yukarıdaki adımları kullanarak doğru şekilde eklendikten sonra aşağıdaki komutu kullanın (kısmına 10.0.0.7 ikincil, özel IP adresi ile değiştiriliyor):

```bash
ping -S 10.0.0.7 outlook.com
```
>[!NOTE]
>İkincil IP yapılandırmaları için, yalnızca yapılandırmaya ilişkili bir genel IP adresi varsa Internet 'e ping gönderebilirsiniz. Birincil IP yapılandırmalarında, Internet 'e ping eklemek için genel bir IP adresi gerekli değildir.
</details>

### <a name="linux-ubuntu-1416"></a>Linux (Ubuntu 14/16)
<details>
  <summary>Genişlet</summary>
Linux dağılımının en son belgelerine bakmaya önerilir. 

1. Bir terminal penceresi açın.
2. Kök kullanıcı olduğunuzdan emin olun. Kök kullanıcı değilseniz aşağıdaki komutu girin:

   ```bash
   sudo -i
   ```

3. Ağ arabiriminin yapılandırma dosyasını güncelleştirin (‘eth0’ olduğu varsayılır).

   * Dhcp için var olan satır öğesini tutun. Birincil IP adresi daha önce olduğu gibi yapılandırılmış olarak kalır.
   * Aşağıdaki komutları kullanarak başka bir statik IP adresi için yapılandırma ekleyin:

     ```bash
     cd /etc/network/interfaces.d/
     ls
     ```

     Bir .cfg dosyası görmeniz gerekir.
4. Dosyayı açın. Dosyanın sonunda aşağıdaki satırları görmeniz gerekir:

   ```bash
   auto eth0
   iface eth0 inet dhcp
   ```

5. Bu dosyada mevcut satırların sonuna aşağıdaki satırları ekleyin:

   ```bash
   iface eth0 inet static
   address <your private IP address here>
   netmask <your subnet mask>
   ```

6. Aşağıdaki komutu kullanarak dosyayı kaydedin:

   ```bash
   :wq
   ```

7. Aşağıdaki komutu kullanarak ağ arabirimini sıfırlayın:

   ```bash
   sudo ifdown eth0 && sudo ifup eth0
   ```

   > [!IMPORTANT]
   > Uzak bağlantı kullanıyorsanız aynı satırda hem ifdown hem de ifup komutunu çalıştırın.
   >

8. IP adresinin ağ arabirimine eklendiğini aşağıdaki komutla doğrulayın:

   ```bash
   ip addr list eth0
   ```

   Listenin bir parçası olarak eklediğiniz IP adresini görmeniz gerekir.

### <a name="validation-ubuntu-1416"></a>Doğrulama (Ubuntu 14/16)

İkincil IP yapılandırmanızla ilişkili genel IP aracılığıyla İnternet’e bağlanabildiğinizden emin olmak için aşağıdaki komutu kullanın:

```bash
ping -I 10.0.0.5 outlook.com
```
>[!NOTE]
>İkincil IP yapılandırmaları için, yalnızca yapılandırmaya ilişkili bir genel IP adresi varsa Internet 'e ping gönderebilirsiniz. Birincil IP yapılandırmalarında, Internet 'e ping eklemek için genel bir IP adresi gerekli değildir.

Linux sanal makineleri için, ikincil bir NIC’den giden bağlantıyı doğrulamaya çalışırken uygun yolları eklemeniz gerekebilir. Bunu yapmanın çok sayıda yolu vardır. Lütfen Linux dağıtımınız için uygun belgelere bakın. Bunu gerçekleştirmeye yönelik bir yöntem aşağıdaki gibidir:

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- Şunu değiştirdiğinizden emin olun:
    - **10.0.0.5** adresini ilişkili bir genel IP adresi olan özel IP adresiyle
    - **10.0.0.1** adresini varsayılan ağ geçidiniz ile
    - ikincil NIC 'nizin adına **eth2**</details>

### <a name="linux-ubuntu-1804"></a>Linux (Ubuntu 18.04 +)
<details>
  <summary>Genişlet</summary>
Ubuntu 18,04 ve üzeri `netplan` işletim sistemi ağ yönetimi için olarak değiştirilmiştir. Linux dağılımının en son belgelerine bakmaya önerilir. 

1. Bir terminal penceresi açın.
2. Kök kullanıcı olduğunuzdan emin olun. Kök kullanıcı değilseniz aşağıdaki komutu girin:

    ```bash
    sudo -i
    ```

3. İkinci arabirim için bir dosya oluşturun ve bir metin düzenleyicisinde açın:

    ```bash
    vi /etc/netplan/60-static.yaml
    ```

4. Aşağıdaki satırları, `10.0.0.6/24` IP/ağ ağlarınız ile değiştirerek dosyaya ekleyin:

    ```bash
    network:
        version: 2
        ethernets:
            eth0:
                addresses:
                    - 10.0.0.6/24
    ```

5. Aşağıdaki komutu kullanarak dosyayı kaydedin:

    ```bash
    :wq
    ```

6. [Netplan](http://manpages.ubuntu.com/manpages/cosmic/man8/netplan-try.8.html) kullanarak değişiklikleri test etme sözdizimini onaylayın:

    ```bash
    netplan try
    ```

> [!NOTE]
> `netplan try`değişiklikleri geçici olarak uygular ve 120 saniye sonra değişiklikleri geri alınacaktır. Bağlantı kaybı varsa lütfen 120 saniye bekleyin ve sonra yeniden bağlanın. Bu sırada, değişiklikler geri alınacaktır.

7. Sorun olmadığı varsayıldığında `netplan try` , yapılandırma değişikliklerini uygulayın:

    ```bash
    netplan apply
    ```

8. IP adresinin ağ arabirimine eklendiğini aşağıdaki komutla doğrulayın:

    ```bash
    ip addr list eth0
    ```

    Listenin bir parçası olarak eklediğiniz IP adresini görmeniz gerekir. Örnek:

    ```bash
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
        valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
        link/ether 00:0d:3a:8c:14:a5 brd ff:ff:ff:ff:ff:ff
        inet 10.0.0.6/24 brd 10.0.0.255 scope global eth0
        valid_lft forever preferred_lft forever
        inet 10.0.0.4/24 brd 10.0.0.255 scope global secondary eth0
        valid_lft forever preferred_lft forever
        inet6 fe80::20d:3aff:fe8c:14a5/64 scope link
        valid_lft forever preferred_lft forever
    ```
### <a name="validation-ubuntu-1804"></a>Doğrulama (Ubuntu 18.04 +)

İkincil IP yapılandırmanızla ilişkili genel IP aracılığıyla İnternet’e bağlanabildiğinizden emin olmak için aşağıdaki komutu kullanın:

```bash
ping -I 10.0.0.5 outlook.com
```
>[!NOTE]
>İkincil IP yapılandırmaları için, yalnızca yapılandırmaya ilişkili bir genel IP adresi varsa Internet 'e ping gönderebilirsiniz. Birincil IP yapılandırmalarında, Internet 'e ping eklemek için genel bir IP adresi gerekli değildir.

Linux sanal makineleri için, ikincil bir NIC’den giden bağlantıyı doğrulamaya çalışırken uygun yolları eklemeniz gerekebilir. Bunu yapmanın çok sayıda yolu vardır. Lütfen Linux dağıtımınız için uygun belgelere bakın. Bunu gerçekleştirmeye yönelik bir yöntem aşağıdaki gibidir:

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- Şunu değiştirdiğinizden emin olun:
    - **10.0.0.5** adresini ilişkili bir genel IP adresi olan özel IP adresiyle
    - **10.0.0.1** adresini varsayılan ağ geçidiniz ile
    - ikincil NIC 'nizin adına **eth2**</details>

### <a name="linux-red-hat-centos-and-others"></a>Linux (Red hat, CentOS ve diğerleri)
<details>
  <summary>Genişlet</summary>

1. Bir terminal penceresi açın.
2. Kök kullanıcı olduğunuzdan emin olun. Kök kullanıcı değilseniz aşağıdaki komutu girin:

    ```bash
    sudo -i
    ```

3. Parolanızı girin ve istenen yönergeleri izleyin. Kök kullanıcı olduktan sonra aşağıdaki komutla ağ betikleri klasörüne gidin:

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. Aşağıdaki komutu kullanarak ilgili ifcfg dosyalarını listeleyin:

    ```bash
    ls ifcfg-*
    ```

    Gördüğünüz dosyalardan biri *ifcfg-eth0* olmalıdır.

5. Bir IP adresi eklemek için aşağıda gösterildiği gibi bir yapılandırma dosyası oluşturun. Her IP yapılandırması için bir dosya oluşturulmalıdır.

    ```bash
    touch ifcfg-eth0:0
    ```

6. *ifcfg-eth0:0* dosyasını aşağıdaki komutla açın:

    ```bash
    vi ifcfg-eth0:0
    ```

7. Dosyaya (bu örnekte *eth0:0*) aşağıdaki komutla içerik ekleyin. Bilgileri IP adresine göre güncelleştirdiğinizden emin olun.

    ```bash
    DEVICE=eth0:0
    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.101.101
    NETMASK=255.255.255.0
    ```

8. Aşağıdaki komutla dosyayı kaydedin:

    ```bash
    :wq
    ```

9. Ağ hizmetlerini yeniden başlatın ve aşağıdaki komutları kullanarak değişikliklerin başarılı olduğundan emin olun:

    ```bash
    /etc/init.d/network restart
    ifconfig
    ```

    Döndürülen listede eklemiş olduğunuz *eth0:0* IP adresini görmeniz gerekir.

### <a name="validation-red-hat-centos-and-others"></a>Doğrulama (Red hat, CentOS ve diğerleri)

İkincil IP yapılandırmanızla ilişkili genel IP aracılığıyla İnternet’e bağlanabildiğinizden emin olmak için aşağıdaki komutu kullanın:

```bash
ping -I 10.0.0.5 outlook.com
```
>[!NOTE]
>İkincil IP yapılandırmaları için, yalnızca yapılandırmaya ilişkili bir genel IP adresi varsa Internet 'e ping gönderebilirsiniz. Birincil IP yapılandırmalarında, Internet 'e ping eklemek için genel bir IP adresi gerekli değildir.

Linux sanal makineleri için, ikincil bir NIC’den giden bağlantıyı doğrulamaya çalışırken uygun yolları eklemeniz gerekebilir. Bunu yapmanın çok sayıda yolu vardır. Lütfen Linux dağıtımınız için uygun belgelere bakın. Bunu gerçekleştirmeye yönelik bir yöntem aşağıdaki gibidir:

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- Şunu değiştirdiğinizden emin olun:
    - **10.0.0.5** adresini ilişkili bir genel IP adresi olan özel IP adresiyle
    - **10.0.0.1** adresini varsayılan ağ geçidiniz ile
    - ikincil NIC 'nizin adına **eth2**</details>