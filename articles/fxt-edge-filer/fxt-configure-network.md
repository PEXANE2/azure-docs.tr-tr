---
title: 'Öğretici: Bir Azure FXT Edge Filer kümesinde ağı yapılandırma'
description: Azure FXT Edge Filer kümesini oluşturduktan sonra ağ ayarlarını özelleştirme
author: ekpgh
ms.author: rohogue
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.openlocfilehash: 9b0154889544e0054e309cc5f43851b73b4396b4
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754681"
---
# <a name="tutorial-configure-the-clusters-network-settings"></a>Öğretici: Kümenin ağ ayarlarını yapılandırma

Yeni oluşturulan Azure FXT Edge Filer kümesini kullanmadan önce, iş akışınız için çeşitli ağ ayarlarını denetlemeniz ve özelleştirmeniz gerekir. 

Bu öğretici, yeni bir küme için ayarlamanız gerekebilecek ağ ayarlarını açıklar. 

Şunları öğreneceksiniz: 

> [!div class="checklist"]
> * Küme oluşturduktan sonra hangi ağ ayarlarının güncelleştirilmesi gerekebilir?
> * Hangi Azure FXT Edge Filer kullanım örnekleri bir AD sunucusu veya DNS sunucusu gerektirir 
> * Denge istemci isteklerini FXT kümesine otomatik olarak yüklemek için round-robin DNS (RRDNS) nasıl yapılandırılabilen

Bu adımları tamamlamak için gereken süre, sisteminizde kaç yapılandırma değişikliği gerektiğine bağlıdır:

* Yalnızca öğreticiyi okumanız ve birkaç ayarı kontrol etmeniz gerekiyorsa, 10 ila 15 dakika sürer. 
* Round-robin DNS yapılandırmanız gerekiyorsa, bu görev bir saat veya daha fazla sürebilir.

## <a name="adjust-network-settings"></a>Ağ ayarlarını ayarlama

Ağla ilgili çeşitli görevler, yeni bir Azure FXT Edge Filer kümesi kurmanın bir parçasıdır. Bu listeyi kontrol edin ve hangilerinin sisteminize uygulanacağına karar verin.

Kümenin ağ ayarları hakkında daha fazla bilgi edinmek için Küme Yapılandırma Kılavuzu'nda [Ağ hizmetlerini yapılandırma'yı](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/network_overview.html) okuyun.

* İstemciye bakan ağ için yuvarlak robin DNS'yi yapılandırın (isteğe bağlı)

  [FXT Edge Filer kümesi için DNS'de](#configure-dns-for-load-balancing)açıklandığı şekilde DNS sistemini yapılandırarak yük dengesi küme trafiği.

* NTP ayarlarını doğrulama

* Etkin Dizin ve kullanıcı adı/grup adı indirmelerini yapılandırın (gerekirse)

  Ağ ana bilgisayarlarınız Active Directory veya başka bir tür dış dizin hizmeti kullanıyorsa, kümenin kullanıcı adı ve grup bilgilerini nasıl karşıdan yükleyiş sini ayarlamak için kümenin dizin hizmetleri yapılandırmasını değiştirmeniz gerekir. Ayrıntılar için Küme Yapılandırma Kılavuzu'nda **Küme** > **Dizini Hizmetlerini** okuyun.

  SMB desteği istiyorsanız bir AD sunucusu gereklidir. SMB'yi ayarlamaya başlamadan önce AD'yi yapılandırın.

* VLAN'ları tanımlayın (isteğe bağlı)
  
  Kümenizin vservers ve genel ad alanı tanımlamadan önce gerekli diğer VLAN'ları yapılandırın. Daha fazla bilgi edinmek için Küme Yapılandırma Kılavuzu'nda [V'lerle Çalışma'yı](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/network_overview.html#vlan-overview) okuyun.

* Proxy sunucularını yapılandırma (gerekirse)

  Kümeniz dış adreslere ulaşmak için bir proxy sunucusu kullanıyorsa, onu ayarlamak için aşağıdaki adımları izleyin:

  1. **Proxy Yapılandırma** ayarları sayfasında proxy sunucusunu tanımlama
  1. Proxy sunucusu yapılandırmasını **Cluster** > **General Setup** sayfası veya Core **Filer Ayrıntıları** sayfasıyla uygulayın.
  
  Daha fazla bilgi için Küme Yapılandırma Kılavuzu'nda [web yakınlıklarını kullanma'yı](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/proxy_overview.html) okuyun.

* Kümenin kullanması için [şifreleme sertifikaları](#encryption-certificates) yükleme (isteğe bağlı)

### <a name="encryption-certificates"></a>Şifreleme sertifikaları

FXT Edge Filer kümesi bu işlevler için X.509 sertifikalarını kullanır:

* Küme yönetimi trafiğini şifrelemek için

* Bir istemci adına üçüncü taraf KMIP sunucularına kimlik doğrulaması yapmak için

* Bulut sağlayıcılarının sunucu sertifikalarını doğrulamak için

Kümeye sertifika yüklemeniz gerekiyorsa, **Küme** > **Sertifikaları** ayarları sayfasını kullanın. Ayrıntılar, Küme Yapılandırma Kılavuzu'nun [> Sertifikaları Küme](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_certificates.html) sayfasında dır.

Küme yönetimi iletişimini şifrelemek için, yönetim TLS için hangi sertifikanın kullanılacağını seçmek için **Cluster** > **Genel Kurulum** ayarları sayfasını kullanın.

> [!Note] 
> Bulut hizmeti erişim **anahtarları, Bulut Kimlik Bilgileri** yapılandırma sayfası kullanılarak depolanır. Yukarıdaki [çekirdek filer](fxt-add-storage.md#add-a-core-filer) ekle bölümü bir örnek gösterir; ayrıntılar için Küme Yapılandırma Kılavuzu [Bulut Kimlik Bilgileri](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_cloud_credentials.html) bölümünü okuyun. 

## <a name="configure-dns-for-load-balancing"></a>Yük dengeleme için DNS'yi yapılandırın

Bu bölümde, istemci yükünü FXT Edge Filer kümenizdeki istemciye bakan tüm IP adresleri arasında dağıtmak için bir round-robin DNS (RRDNS) sistemi yapılandırmatemelleri açıklanmaktadır. 

### <a name="decide-whether-or-not-to-use-dns"></a>DNS kullanıp kullanmamaya karar verin

Yük dengeleme her zaman önerilir, ancak her zaman DNS kullanmak zorunda değilsiniz. Örneğin, bazı istemci iş akışları türlerinde kümeyi monte ettiklerinde istemciler arasında eşit olarak küme IP adresleri atamak için bir komut dosyası kullanmak daha mantıklı olabilir. [Kümedağı'nda](fxt-mount-clients.md)bazı yöntemler açıklanmıştır. 

Bir DNS sunucusu kullanıp kullanmamaya karar verirken bunları aklınızda bulundurun: 

* Sisteminize yalnızca NFS istemcileri tarafından erişilirse, DNS gerekmez. Sayısal IP adreslerini kullanarak tüm ağ adreslerini belirtmek mümkündür. 

* Etkin Dizin sunucusu için bir DNS etki alanı belirtmeniz gerektiğinden, sisteminiz SMB (CIFS) erişimini destekliyorsa, DNS gereklidir.

* Kerberos kimlik doğrulamasını kullanmak istiyorsanız DNS gereklidir.

### <a name="round-robin-dns-configuration-details"></a>Round-robin DNS yapılandırma ayrıntıları

İstemciler kümeye eriştığında, RRDNS isteklerini kullanılabilir tüm arabirimler arasında otomatik olarak dengeler.

En iyi performans için, DNS sunucunuzu aşağıdaki diyagramda gösterildiği gibi istemciye bakan küme adreslerini işletecek şekilde yapılandırın.

Bir küme vserver solda gösterilir ve IP adresleri ortada ve sağda görünür. Her istemci erişim noktasını gösterildiği gibi A kayıtları ve işaretçileri ile yapılandırın.

![Küme round-robin DNS diyagramı -](media/fxt-cluster-config/fxt-rrdns-diagram.png) 
ayrıntılı alt metin bağlantısı görüntü[ayrıntılı metin açıklaması](https://azure.github.io/Avere/legacy/Azure-FXT-EdgeFilerDNSconfiguration-alt-text.html) aşağıdaki

İstemcilere bakan her IP adresi, küme tarafından iç kullanım için benzersiz bir ada sahip olmalıdır. (Bu diyagramda, istemci IP'ler netlik için vs1-client-IP-* olarak adlandırılır, ancak üretimde muhtemelen istemci gibi daha kısa bir şey kullanmalısınız*.)

İstemciler, sunucu bağımsız değişkeni olarak vserver adını kullanarak kümeye monte edilir. 

Vserver'ınızdaki sorgular ``named.conf`` için döngüsel sıra ayarlamak için DNS sunucunuzun dosyasını değiştirin. Bu seçenek, kullanılabilir değerlerin tümünün geçişini sağlar. Aşağıdaki gibi bir deyim ekleyin:

```
options {
    rrset-order {
        class IN A name "vserver1.example.com" order cyclic;
    };
};
```

Aşağıdaki ``nsupdate`` komutlar DNS'yi doğru yapılandırmaya bir örnek sağlar:

```
update add vserver1.example.com. 86400 A 10.0.0.10
update add vserver1.example.com. 86400 A 10.0.0.11
update add vserver1.example.com. 86400 A 10.0.0.12
update add vs1-client-IP-10.example.com. 86400 A 10.0.0.10
update add vs1-client-IP-11.example.com. 86400 A 10.0.0.11
update add vs1-client-IP-12.example.com. 86400 A 10.0.0.12
update add 10.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-10.example.com
update add 11.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-11.example.com
update add 12.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-12.example.com
```

### <a name="enable-dns-in-the-cluster"></a>Kümede DNS'yi etkinleştirme 

**Kümenin Küme** > **Yönetim Ağı** ayarları sayfasında kullandığı DNS sunucusunu belirtin. Bu sayfadaki ayarlar şunlardır:

* DNS sunucu adresi
* DNS etki alanı adı
* DNS arama etki alanları

Daha fazla bilgi için Küme Yapılandırma Kılavuzu'nda [DNS Ayarları'nı](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_admin_network.html#gui-dns>) okuyun.

## <a name="next-steps"></a>Sonraki adımlar

Bu, Azure FXT Edge Filer kümesi için son temel yapılandırma adımıdır. 

* [Monitör donanım durumunda](fxt-monitor.md)sistemin LED'leri ve diğer göstergeler hakkında bilgi edinin.
* İstemcilerin KÜMEye Monte'de FXT Edge Filer [kümesini](fxt-mount-clients.md)nasıl monte etmesi gerektiği hakkında daha fazla bilgi edinin. 
* Bir FXT Edge Filer kümesini çalıştırma ve yönetme hakkında daha fazla bilgi için [Küme Yapılandırma Kılavuzu'na](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/ops_conf_index.html)bakın. 