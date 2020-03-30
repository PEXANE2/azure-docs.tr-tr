---
title: CloudSimple tarafından Azure VMware Çözümü - vRealize Otomasyonu için Özel Bulut'ta vCenter'ı ayarlama
description: CloudSimple Private Cloud'unuzda VMware vRealize Automation için bir uç nokta olarak vmware vCenter sunucusunun nasıl ayarlanır olduğunu açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: df73acfc469a8b7b5329b61095aefdbd73baafd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024849"
---
# <a name="set-up-vcenter-on-your-private-cloud-for-vmware-vrealize-automation"></a>VMware vRealize Otomasyonu için Özel Bulut'unuzda vCenter'ı ayarlama

CloudSimple Private Cloud'unuzda VMware vRealize Automation için bir bitiş noktası olarak bir VMware vCenter sunucusu ayarlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

vCenter sunucusunu yapılandırmadan önce bu görevleri tamamlayın:

* Şirket ortamınız ile Özel Bulut'unuzun arasında [Siteden Siteye VPN bağlantısı](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway) yapılandırın.
* [Şirket içi DNS isteklerinin DNS iletmesini](on-premises-dns-setup.md) Özel Bulut'unuz için DNS sunucularına yapılandırın.
* Aşağıdaki tabloda listelenen izinler kümesiyle bir vRealize Automation IaaS yönetim kullanıcısı oluşturmak için bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) gönderin.

| Öznitelik Değeri | İzin |
------------ | ------------- |  
| Veri Deposu |  Boşluk Tahsis <br> Datastore'a göz atın |
| Datastore Kümesi | Datastore Kümesini Yapılandırma |
| Klasör | Klasör Oluştur <br>Klasör Sil |
| Genel |  Özel Öznitelikleri Yönetme<br>Özel Öznitelik Ayarlama |
| Ağ | Ağ Ata |
| İzinler | İzinleri Değiştir |
| Kaynak | Kaynak Havuzuna VM Atama<br>Sanal Makine Kapalı Geçir<br>Sanal Makinede Geçiş Gücü |
| Sanal Makine Envanteri |  Varolan dan oluşturma<br>Yeni Oluştur<br>Taşı<br>Kaldır | 
| Sanal Makine Etkileşimi |  CD Ortamını Yapılandırma<br>Konsol Etkileşimi<br>Cihaz Bağlantısı<br>Güç Kapama<br>Güç A)'da<br>Sıfırla<br>Askıya Alma<br>Araçlar Yükleme | 
| Sanal Makine Yapılandırması |  Varolan Disk Ekle<br>Yeni Disk Ekle<br>Ekle veya Kaldır<br>Diski Kaldır<br>Gelişmiş<br>CPU Sayısını Değiştir<br>Kaynağı Değiştir<br>Sanal Diski Genişlet<br>Disk Değiştirme İzleme<br>Bellek<br>Aygıt Ayarlarını Değiştir<br>Yeniden Adlandır<br>Ek Açıklama yı ayarlama (sürüm 5.0 ve sonrası)<br>Ayarlar<br>Takas Dosyası Yerleştirme |
| Sağlama |  Özelleştirme<br>Klon Şablonu<br>Klon Sanal Makine<br>Şablon dağıtma<br>Özelleştirme Özelliklerini Okuyun |
| Sanal Makine Durumu | Anlık Görüntü Oluşturma<br>Anlık Görüntü'yi Kaldırma<br>Anlık Görüntü'ye Geri Dön |

## <a name="install-vrealize-automation-in-your-on-premises-environment"></a>şirket içi ortamınızda vRealize Otomasyonu yükleyin

1. CloudSimple Desteğinin sizin için oluşturduğu IaaS yöneticisi olarak vRealize Automation IaaS sunucu cihazında oturum açın.
2. vRealize Automation bitiş noktası için bir vSphere Agent dağıtın.
    1. *vra-url*https:// gidin :5480/installer, *vra-url* vRealize Otomasyon yönetim uI erişmek için kullandığınız URL nerede.
    2. Yükleyiciyi indirmek için **IaaS Yükleyici'yi** tıklatın.<br>
    Yükleyici dosyası için adlandırma kuralı*vra-url*@5480.exesetup_.
    3. Yükleyiciyi çalıştırın. Hoş Geldiniz ekranında **İleri**’ye tıklayın.
    4. EULA'yı kabul edin ve **İleri'yi**tıklatın.
    5. Oturum açma bilgilerini sağlayın, **Sertifikayı Kabul Et'i**tıklatın ve sonra **İleri'yi**tıklatın.
    ![vRA kimlik bilgileri](media/configure-vra-endpoint-login.png)
    6. **Özel Yükle** ve **Proxy Aracıları'nı** seçin ve **İleri'yi**tıklatın.
    ![vRA yükleme türü](media/configure-vra-endpoint-install-type.png)
    7. IaaS sunucu oturum açma bilgilerini girin ve **İleri'yi**tıklatın. Active Directory kullanıyorsanız, kullanıcı adını **etki alanı\kullanıcı** biçiminde girin. Aksi takdirde, biçimi kullanın. **user@domain**
    ![vRA giriş bilgileri](media/configure-vra-endpoint-account.png)
    8. Proxy ayarları **için, Agent türü**için **vSphere** girin. Aracı için bir ad girin.
    9. **Yönetici Servis Ana Bilgisayar** ve Model Yöneticisi Web Service **Host** alanlarına IaaS sunucusu FQDN'yi girin. FQDN değerlerinin her biri için bağlantıyı sınamak için **Test'i** tıklatın. Test başarısız olursa, IaaS sunucu ana bilgisayar adı çözülecek şekilde DNS ayarlarınızı değiştirin.
    10. Özel Bulut için vCenter sunucu bitiş noktası için bir ad girin. Yapılandırma işleminde daha sonra kullanılmak üzere adı kaydedin.

        ![vRA proxy yüklemek](media/configure-vra-endpoint-proxy.png)

    11. **İleri**'ye tıklayın.
    12. **Yükle'yi**tıklatın.

## <a name="configure-the-vsphere-agent"></a>vSphere aracısını yapılandırma

1. *https:// vra-url*/vcac adresine gidin ve **ConfigurationAdmin**olarak oturum açın.
2. **Altyapı** > **Bitiş Noktaları** > **Bitiş Noktalarını**seçin.
3. **Yeni** > **Sanal** > **vSphere**seçin.
4. Önceki yordamda belirttiğiniz vSphere uç nokta adını girin.
5. **Adres**için,*vcenter-fqdn/sdk https:// vcenter-fqdn*biçiminde Özel Bulut vCenter Server URL'sini girin, *vcenter-fqdn* vCenter sunucusunun adıdır.
6. CloudSimple Support'un sizin için oluşturduğu vRealize Automation IaaS yönetim kullanıcısının kimlik bilgilerini girin.
7. Kullanıcı kimlik bilgilerini doğrulamak için **Test Bağlantısı'nı** tıklatın. Test başarısız olursa, URL'yi, hesap bilgilerini ve [bitiş noktası adını](#verify-the-endpoint-name) doğrulayın ve tekrar test edin.
8. Başarılı bir testten sonra, vSphere bitiş noktasını oluşturmak için **Tamam'ı** tıklatın.
    ![vRA uç nokta config erişim](media/configure-vra-endpoint-vra-edit.png)

### <a name="verify-the-endpoint-name"></a>Bitiş noktası adını doğrulama

Doğru vCenter sunucu bitiş noktası adını tanımlamak için aşağıdakileri yapın:

1. IaaS cihazında bir komut istemi açın.
2. Dizini C:\Program Dosyaları (x86)\VMware\vCAC\Aracılar\aracıadı olarak *değiştirin, burada aracı adı* vCenter sunucu bitiş noktasına atadığınız addır.
3. Şu komutu çalıştırın.

```
..\..\Server\DynamicOps.Vrm.VRMencrypt.exe VRMAgent.exe.config get
```

Çıktı aşağıdakine benzer. `managementEndpointName` Alanın değeri bitiş noktası adıdır.

```
managementEndpointName: cslab1pc3-vc
doDeletes: true
```
