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
ms.openlocfilehash: a9473f69d600a86ff71da69c7efe0dea3f2b0a08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76159683"
---
## <a name="add-ip-addresses-to-a-vm-operating-system"></a><a name="os-config"></a>Bir VM işletim sistemine IP adresleri ekleme

Birden çok özel IP adresiyle oluşturduğunuz bir VM'ye bağlanın ve oturum açın. Sanal makineye eklediğiniz tüm özel IP adreslerini (birincil adres dahil) el ile eklemeniz gerekir. VM işletim sisteminiz için aşağıdaki adımları tamamlayın.

### <a name="windows"></a>Windows

1. Bir komut isteminde *ipconfig/all* yazın.  Yalnızca *Birincil* özel IP adresini (DHCP üzerinden) görürsünüz.
2. Komut istemine *ncpa.cpl* yazarak **Ağ bağlantıları** penceresini açın.
3. Uygun bağdaştırıcının özelliklerini açın: **Yerel Ağ Bağlantısı**.
4. İnternet Protokolü sürüm 4 (IPv4) öğesine çift tıklayın.
5. **Aşağıdaki IP adresini kullan**’ı seçip aşağıdaki değerleri girin:

    * **IP adresi**: *Birincil* özel IP adresini girin
    * **Alt ağ maskesi**: Alt ağınıza göre ayarlanır. Örneğin, alt ağ bir /24 alt ağı ise alt ağ maskesi 255.255.255.0 şeklindedir.
    * **Varsayılan ağ geçidi**: Alt ağdaki ilk IP adresi. Alt ağınız 10.0.0.0/24 ise ağ geçidi IP adresi 10.0.0.1 şeklindedir.
    * **Aşağıdaki DNS sunucu adreslerini kullan'ı** seçin ve aşağıdaki değerleri girin:
        * **Tercih edilen DNS sunucusu**: Kendi DNS sunucunuzu kullanmıyorsanız 168.63.129.16 girin.  Kendi DNS sunucunuzu kullanıyorsanız kendi sunucunuzun IP adresini girin.
    * **Gelişmiş** düğmesini seçin ve ek IP adresleri ekleyin. Önceki adımda Azure ağ arabirimine eklediğiniz ikincil özel IP adreslerinin her birini, Azure ağ arabirimine atanan birincil IP adresiatanan Windows ağ arabirimine ekleyin.

        Sanal makinenin işletim sistemi içinde bir Azure sanal makinesine atanan genel IP adresini asla el ile atamamalısınız. İşletim sistemi içindeki IP adresini el ile ayarladığınızda, bunun Azure [ağ arabirimine](../articles/virtual-network/virtual-network-network-interface-addresses.md#change-ip-address-settings)atanan özel IP adresiyle aynı adres olduğundan emin olun veya sanal makineye bağlantınızı kaybedebilirsiniz. [Özel IP adresi](../articles/virtual-network/virtual-network-network-interface-addresses.md#private) ayarları hakkında daha fazla bilgi edinin. İşletim sistemi içinde hiçbir zaman bir Azure genel IP adresi atamayın.

    * **Tamam**’a tıklayarak TCP/IP ayarlarını kapatın ve ardından tekrar **Tamam**’a tıklayarak bağdaştırıcı ayarlarını kapatın. RDP bağlantınız yeniden kurulur.

6. Bir komut isteminde *ipconfig/all* yazın. Eklediğiniz tüm IP adresleri gösterilir ve DHCP kapatılır.
7. Windows'un birincil IP adresi olarak Azure'daki birincil IP yapılandırmasının özel IP adresini kullanacak şekilde Windows'u yapılandırın. Ayrıntılar için [birden çok IP adresi olan Azure Windows VM'den Internet erişimi yok'](https://support.microsoft.com/help/4040882/no-internet-access-from-azure-windows-vm-that-has-multiple-ip-addresse) a bakın. 

### <a name="validation-windows"></a>Doğrulama (Windows)

İkincil IP yapılandırmanızdan, ona bağlı genel IP ile internet’e bağlanabildiğinizden emin olmak için, yukarıdaki adımları doğru şekilde kullanarak ekledikten sonra aşağıdaki komutu kullanın:

```bash
ping -S 10.0.0.5 hotmail.com
```
>[!NOTE]
>İkincil IP yapılandırmaları için, yalnızca yapılandırmayla ilişkili genel bir IP adresi varsa Internet'e ping atlayabilirsiniz. Birincil IP yapılandırmaları için, Internet'e ping yapmak için ortak bir IP adresi gerekmez.

### <a name="linux-ubuntu-1416"></a>Linux (Ubuntu 14/16)

Linux dağıtımınız için en son belgelere bakmanızı öneririz. 

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

### <a name="linux-ubuntu-1804"></a>Linux (Ubuntu 18.04+)

Ubuntu 18.04 ve üzeri `netplan` işletim sistemi ağ yönetimi için değişti. Linux dağıtımınız için en son belgelere bakmanızı öneririz. 

1. Bir terminal penceresi açın.
2. Kök kullanıcı olduğunuzdan emin olun. Kök kullanıcı değilseniz aşağıdaki komutu girin:

    ```bash
    sudo -i
    ```

3. İkinci arabirim için bir dosya oluşturun ve bir metin düzenleyicisinde açın:

    ```bash
    vi /etc/netplan/60-static.yaml
    ```

4. IP/netmask'inizle değiştirerek `10.0.0.6/24` dosyaya aşağıdaki satırları ekleyin:

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

6. Netplan kullanarak değişiklikleri test etme sözdizimini onaylamaya [çalışın:](http://manpages.ubuntu.com/manpages/cosmic/man8/netplan-try.8.html)

    ```bash
    netplan try
    ```

> [!NOTE]
> `netplan try`değişiklikleri geçici olarak uygulayacak ve 120 saniye sonra değişiklikleri geri alacaktır. Bağlantı kaybı varsa, lütfen 120 saniye bekleyin ve sonra yeniden bağlanın. O zaman, değişiklikler geri alınmış olacaktır.

7. Yapılandırma değişiklikleri `netplan try`ile ilgili herhangi bir sorun olmadığını varsayarsak, uygulayın:

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
    
### <a name="linux-red-hat-centos-and-others"></a>Linux (Red Hat, CentOS ve diğerleri)

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

### <a name="validation-linux"></a>Doğrulama (Linux)

İkincil IP yapılandırmanızla ilişkili genel IP aracılığıyla İnternet’e bağlanabildiğinizden emin olmak için aşağıdaki komutu kullanın:

```bash
ping -I 10.0.0.5 hotmail.com
```
>[!NOTE]
>İkincil IP yapılandırmaları için, yalnızca yapılandırmayla ilişkili genel bir IP adresi varsa Internet'e ping atlayabilirsiniz. Birincil IP yapılandırmaları için, Internet'e ping yapmak için ortak bir IP adresi gerekmez.

Linux sanal makineleri için, ikincil bir NIC’den giden bağlantıyı doğrulamaya çalışırken uygun yolları eklemeniz gerekebilir. Bunu yapmanın çok sayıda yolu vardır. Lütfen Linux dağıtımınız için uygun belgelere bakın. Bunu gerçekleştirmeye yönelik bir yöntem aşağıdaki gibidir:

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- Şunu değiştirdiğinizden emin olun:
    - **10.0.0.5** adresini ilişkili bir genel IP adresi olan özel IP adresiyle
    - **10.0.0.1** adresini varsayılan ağ geçidiniz ile
    - **eth2** değerini ikincil NIC’nin adı ile
