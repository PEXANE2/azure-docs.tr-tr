---
title: Quickstart - CloudSimple tarafından Özel Bulutta Azure VMware VM Oluşturma - Azure VMware Çözümü
description: CloudSimple Private Cloud'da Azure VMware VM nasıl oluşturulacak açıklanır
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4ac818cfd267b781366c0e32c9f93cc885dff99c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77566157"
---
# <a name="create-vmware-virtual-machines-on-your-private-cloud"></a>Özel Bulut'unuzda VMware sanal makineleri oluşturun

Özel Bulut'unuzda sanal makineler oluşturmak için Azure portalından CloudSimple portalına erişerek başlayın.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="access-the-cloudsimple-portal"></a>CloudSimple portalına erişim

1. **Tüm Hizmetler**’i seçin.
2. **CloudSimple Services'ı**arayın.
3. Özel Bulut'unuzu oluşturmak istediğiniz CloudSimple hizmetini seçin.
4. Genel **Bakış** sayfasından, CloudSimple portalı için yeni bir tarayıcı sekmesi açmak için **CloudSimple portalına git'i** tıklatın.  İstenirse, Azure oturum açınızın kimlik bilgileriyle oturum açın.  

    ![CloudSimple portalına başlat](media/launch-cloudsimple-portal.png)

## <a name="launch-vcenter-web-ui"></a>vCenter web-ui başlat

Artık sanal makineler ve ilkeler ayarlamak için vCenter'ı başlatabilirsiniz.

vCenter'a erişmek için CloudSimple portalından başlayın. Ana sayfada, **Ortak Görevler**altında, **başlat vSphere İstemci'yi**tıklatın.  Özel Bulut'u seçin ve ardından Özel Bulut'ta **VSphere İstemci** Başlat'ı tıklatın.

   ![VSphere İstemciyi Başlat](media/launch-vcenter-from-cloudsimple-portal.png)

## <a name="upload-an-iso-or-vsphere-template"></a>ISO veya vSphere şablonu yükleme

  > [!WARNING]
  > ISO yüklemesi için vSphere HTML5 istemcisini kullanın.  Flash istemcisi kullanmak bir hataya neden olabilir.

1. VM oluşturmak ve yerel sisteminizde kullanılabilir hale getirmek için vCenter'a yüklemek istediğiniz ISO veya vSphere şablonunu edinin.
2. vCenter'da **Disk** simgesini tıklatın ve **vsanDatastore'u**seçin. **Dosyalar'ı** tıklatın ve ardından **Yeni Klasör'ü**tıklatın.
    ![vCenter ISO](media/vciso00.png)

3. 'ISO'lar ve Şablonlar' başlıklı bir klasör oluşturun.

4. ISO'lar ve Şablonlar'daki ISO'lar klasörüne gidin ve **Dosyaları Yükle'yi**tıklatın. ISO'yu yüklemek için ekrandaki yönergeleri izleyin.

## <a name="create-a-virtual-machine-in-vcenter"></a>vCenter'da Sanal Makine Oluşturma

1. vCenter'da, **Ana Bilgisayarlar ve Kümeler** simgesini tıklatın.

2. İş **Yükü'ne** sağ tıklayın ve **Yeni Sanal Makine'yi**seçin.
    ![Yeni VM](media/vcvm01.png)

3. **Yeni sanal makine oluştur'u** seçin ve **İleri'yi**tıklatın.
    ![Yeni VM](media/vcvm02.png)

4. Makineyi adlandırın, **İş Yükü VM konumunu** seçin ve **İleri'yi**tıklatın.
    ![Yeni VM](media/vcvm03.png)

5. İş **Yükü** bilgi işlem kaynağını seçin ve **İleri'yi**tıklatın.
    ![Yeni VM](media/vcvm04.png)

6. **vsanDatastore'u** seçin ve **İleri'yi**tıklatın.
    ![Yeni VM](media/vcvm05.png)

7. Varsayılan ESXi 6.5 uyumluluk seçimini tutun ve **İleri'yi**tıklatın.
    ![Yeni VM](media/vcvm06.png)

8. Oluşturduğunuz VM için ISO'nun konuk işletim sistemi'ni seçin ve **İleri'yi**tıklatın.
    ![Yeni VM](media/vcvm07.png)

9. Sabit disk ve ağ seçeneklerini seçin. Yeni CD/DVD Sürücüsü için **Datastore ISO dosyasini**seçin.  Bu VM'ye Genel IP adresinden trafiğe izin vermek istiyorsanız, ağı **vm-1**olarak seçin.
    ![Yeni VM](media/vcvm08.png)

10. Seçim penceresi açılır. Daha önce ISO'lar ve Şablonlar klasörüne yüklediğiniz dosyayı seçin ve **Tamam'ı**tıklatın.
    ![Yeni VM](media/vcvm10.png)

11. Ayarları gözden geçirin ve VM'yi oluşturmak için **Tamam'ı** tıklatın.
    ![Yeni VM](media/vcvm11.png)

VM artık İş Yükü hesaplama kaynaklarına eklenir ve kullanıma hazırdır. 
![Yeni VM](media/vcvm12.png)

Temel kurulum tamamlandı. Özel Bulut'unuzu şirket içi VM altyapınızı nasıl kullandığınıza benzer şekilde kullanmaya başlayabilirsiniz.

Aşağıdaki bölümler, Özel Bulut iş yükleri için DNS ve DHCP sunucuları ayarlama ve varsayılan ağ yapılandırmasını değiştirme hakkında isteğe bağlı bilgiler içerir.

## <a name="add-users-and-identity-sources-to-vcenter-optional"></a>vCenter'a Kullanıcı ve kimlik kaynakları ekleme (İsteğe Bağlı)

CloudSimple, varsayılan vCenter kullanıcı hesabını `cloudowner@cloudsimple.local`kullanıcı adı ile atar. Başlamak için ek hesap kurulumu gerekmez.  CloudSimple normalde yöneticilere normal işlemleri gerçekleştirmek için gereksinim duydukları ayrıcalıkları atar.  Şirket içi etkin dizininizi veya Azure AD'ınızı Özel Bulut'unuzda ek bir [kimlik kaynağı](set-vcenter-identity.md) olarak ayarlayın.

## <a name="create-a-dns-and-dhcp-server-optional"></a>DNS ve DHCP sunucusu oluşturma (İsteğe bağlı)

Özel Bulut ortamında çalışan uygulamalar ve iş yükleri, arama ve IP adresi ataması için ad çözümlemesi ve DHCP hizmetleri gerektirir. Bu hizmetleri sağlamak için uygun bir DHCP ve DNS altyapısı gereklidir. Bu hizmetleri Özel Bulut ortamınızda sağlamak için vCenter'da sanal bir makine yapılandırabilirsiniz.

Ön koşullar

* VLAN yapılandırılan dağıtılmış bir bağlantı noktası grubu

* Kurulumu şirket içi veya Internet tabanlı DNS sunucularına yönlendirme

* Sanal makine şablonu veya ISO sanal bir makine oluşturmak için

Aşağıdaki bağlantılar, Linux ve Windows'da DHCP ve DNS sunucularının ayarlanması konusunda kılavuz sağlar.

#### <a name="linux-based-dns-server-setup"></a>Linux tabanlı DNS sunucu kurulumu

Linux, DNS sunucuları kurmak için çeşitli paketler sunar.  Burada bir açık kaynak BIND DNS sunucusu kurmak için talimatlar için bir bağlantıdır.

[Örnek kurulum](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-centos-7)

#### <a name="windows-based-setup"></a>Windows tabanlı kurulum

Bu Microsoft konuları, Bir Windows sunucusunun DNS sunucusu ve DHCP sunucusu olarak nasıl ayarlanır olduğunu açıklar.

[DNS Server olarak Windows Server](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

[DHCP Server olarak Windows Server](https://docs.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-top)

## <a name="customize-networking-configuration-optional"></a>Ağ yapılandırması özelleştirme (İsteğe bağlı)

CloudSimple portalındaki Ağ sayfaları, güvenlik duvarı tabloları nın yapılandırmasını ve VM'ler için genel IP adreslerinin yapılandırmasını belirtmenize olanak tanır.

### <a name="allocate-public-ips"></a>Genel IP'leri ayırma

1. CloudSimple portalındaki **Ağ > Genel IP'ye** gidin.
2. **Genel IP Tahsis'i**tıklatın.
3. IP adresi girişini tanımlamak için bir ad girin.
4. Varsayılan konumu koruyun.
5. İstenirse boşta kalan zaman aşmasını değiştirmek için kaydırıcıyı kullanın.
6. Herkese açık bir IP adresi atamak istediğiniz yerel IP adresini girin.
7. İstenirse ilişkili bir DNS adı girin.
8. **Bitti**’ye tıklayın.

    ![Genel IP](media/quick-create-pc-public-ip.png)

Genel IP adresini ayırma görevi başlar. **Görevler etkinliği** sayfasında görevin durumunu > kontrol edebilirsiniz. Ayırma tamamlandığında, yeni giriş Genel IP'ler sayfasında gösterilir.

Bu IP adresinin eşlendiği VM'nin yukarıda belirtilen yerel adresle yapılandırılması gerekir. Bir IP adresini yapılandırma yordamı VM işletim sistemine özgüdür. Doğru yordam için VM işletim sisteminiziçin belgelere başvurun.

#### <a name="example"></a>Örnek

Örneğin, burada Ubuntu 16.04 için ayrıntılar dır.

Dosya /etc/network/interfaces'teki inet adresi aile yapılandırmasına statik yöntemi ekleyin. Adres, netmaske ve ağ geçidi değerlerini değiştirin. Bu örnekiçin, eth0 arabirimini, dahili IP adresini 192.168.24.10, ağ geçidi adresini 192.168.24.1 ve netmask 255.255.255.0'ı kullanıyoruz. Ortamınız için, mevcut alt net bilgileri karşılama e-postasında sağlanır.

```
sudo vi /etc/network/interfaces
```

```
auto eth0
Iface eth0 inet static
iface eth0 inet static
address 192.168.24.10
netmask 255.255.255.0
gateway 192.168.24.1
dns-nameservers 8.8.8.8
dns-domain acme.com
dns-search acme.com
```
Arabirimi el ile devre dışı edin.

```
sudo ifdown eth0
```
Arabirimi el ile yeniden etkinleştirin.

```
sudo ifup eth0
```

Varsayılan olarak, Internet'ten gelen tüm trafik **reddedilir.** Başka bir bağlantı noktası açmak istiyorsanız, bir [güvenlik duvarı tablosu](firewall.md)oluşturun.

Statik IP adresi olarak dahili bir IP adresi yapılandırdıktan sonra, VM içinden Internet'e erişebileceğinizi doğrulayın.

```
ping 8.8.8.8
```
Ayrıca, ortak IP adresini kullanarak Internet'ten VM'ye ulaşabileceğinizi de doğrulayın.

VM'deki iptable kurallarının 80 giriş noktasını engellemediğinden emin olun.
        
```
netstat -an | grep 80
```

Bağlantı noktası 80'i dinleyen bir http sunucusu başlatın.
       
```
python2.7 -m SimpleHTTPServer 80
```

or

```
python3 -m http.server 80
```
Masaüstünüzde bir tarayıcı başlatın ve VM'nizdeki dosyalara göz atmak için ortak IP adresi için 80 bağlantı noktasına yönlendirin.

### <a name="default-cloudsimple-firewall-rules-for-public-ip"></a>Varsayılan CloudBasit genel IP için güvenlik duvarı kuralları

* VPN trafiği: VPN ile tüm iş yükü ağları ve yönetim ağı arasındaki tüm trafiğe (gelen/gelen) izin verilir.
* Özel bulut iç trafiği: (gelen/to) iş yükü ağları ve yönetim ağı (yukarıda gösterildiği) arasındaki tüm doğu-batı trafiğine izin verilir.
* İnternet trafiği:
  * Internet'ten gelen tüm trafik iş yükü ağları ve yönetim ağına reddedilir.
  * İş yükü ağlarından veya yönetim ağından Internet'e giden tüm trafiğe izin verilir.

Güvenlik Duvarı Kuralları özelliğini kullanarak trafiğinizin güvenli hale konbiçimini de değiştirebilirsiniz. Daha fazla bilgi için [bkz.](firewall.md)

## <a name="install-solutions-optional"></a>Yükleme çözümleri (İsteğe bağlı)

Özel Bulut vCenter ortamınızdan tam olarak yararlanmak için CloudSimple Private Cloud'unuza çözümler yükleyebilirsiniz. Sanal makinelerinizi korumak için yedekleme, olağanüstü durum kurtarma, çoğaltma ve diğer işlevleri ayarlayabilirsiniz. Örnekler arasında VMware Site Kurtarma Yöneticisi (VMware SRM) ve Veeam Yedekleme & Çoğaltma verilebilir.

Bir çözüm yüklemek için, sınırlı bir süre için ek ayrıcalıklar istemeniz gerekir. Bkz. [Artırıla ayrıcalıkları.](escalate-private-cloud-privileges.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [Azure ExpressRoute'u kullanarak şirket içi ağa bağlanma](on-premises-connection.md)
* [CloudSimple ağında VPN ağ geçitleri ayarlama](vpn-gateway.md)
