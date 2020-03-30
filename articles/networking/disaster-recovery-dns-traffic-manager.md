---
title: Azure DNS ve Trafik Yöneticisi kullanarak olağanüstü durum kurtarma | Microsoft Dokümanlar
description: Azure DNS ve Trafik Yöneticisi'ni kullanarak olağanüstü durum kurtarma çözümlerine genel bakış.
services: dns
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/08/2018
ms.author: kumud
ms.openlocfilehash: 6eab1803bf5adab42be87b5f8567682c6d75947e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74483529"
---
# <a name="disaster-recovery-using-azure-dns-and-traffic-manager"></a>Azure DNS ve Traffic Manager kullanarak olağanüstü durum kurtarma

Olağanüstü durum kurtarma, ciddi bir uygulama işlevselliği kaybından kurtulmaya odaklanır. Bir olağanüstü durum kurtarma çözümü seçmek için, işletme ve teknoloji sahipleri öncelikle bir felaket sırasında gereken işlevsellik düzeyini belirlemelidir, örneğin - kullanılamayan, kısmen azaltılmış işlevsellik veya gecikmeli kullanılabilirlik yoluyla kullanılabilir veya tamamen kullanılabilir.
Çoğu kurumsal müşteri, bir uygulamaya veya altyapı düzeyinin başarısızlığına karşı dayanıklılık için çok bölgeli bir mimari seçer. Müşteriler, gereksiz mimari aracılığıyla başarısız ve yüksek kullanılabilirlik elde etmek için arayış içinde çeşitli yaklaşımlar seçebilirler. Popüler yaklaşımlardan bazıları şunlardır:

- **Soğuk bekleme ile aktif-pasif**: Bu arıza çözeltisinde, bekleme bölgesinde çalışan VM'ler ve diğer cihazlar, arıza yada ihtiyaç duyulana kadar etkin değildir. Ancak, üretim ortamı yedeklemeler, VM görüntüleri veya Kaynak Yöneticisi şablonları şeklinde farklı bir bölgeye çoğaltılır. Bu arıza mekanizması maliyet-etkindir ancak tam bir başarısızlık üstlenmek için daha uzun zaman alır.
 
    ![Soğuk bekleme ile Aktif/Pasif](./media/disaster-recovery-dns-traffic-manager/active-passive-with-cold-standby.png)
    
    *Şekil - Soğuk bekleme felaket kurtarma yapılandırması ile Aktif/Pasif*

- **Pilot ışıklı Aktif/Pasif**: Bu arıza çözümünde bekleme ortamı en az konfigürasyonla ayarlanır. Kurulum, yalnızca en az ve kritik bir uygulama kümesini desteklemek için çalışan yalnızca gerekli hizmetlere sahiptir. Yerel biçiminde, bu senaryo yalnızca en az işlevselliği yürütebilir, ancak bir hata meydana geldiğinde üretim yükünün büyük bir kısmını almak için ek hizmetleri ölçeklendirebilir ve yumurtlayabilir.
    
    ![Pilot ışıkile Aktif/Pasif](./media/disaster-recovery-dns-traffic-manager/active-passive-with-pilot-light.png)
    
    *Şekil: Aktif/Pasif pilot ışık felaket kurtarma yapılandırması ile*

- **Sıcak bekleme ile Aktif/Pasif**: Bu arıza çözümünde bekleme bölgesi önceden ısıtılır ve temel yükü almaya hazırdır, otomatik ölçekleme açılır ve tüm örnekler çalışır durumdadır. Bu çözüm tam üretim yükünü alacak şekilde ölçeklendirilemese de işlevseldir ve tüm hizmetler çalışır durumdadır. Bu çözüm, pilot ışık yaklaşımının artırılmış bir sürümüdür.
    
    ![Sıcak bekleme ile Aktif/Pasif](./media/disaster-recovery-dns-traffic-manager/active-passive-with-warm-standby.png)
    
    *Şekil: Sıcak bekleme olağanüstü durum kurtarma yapılandırması ile Etkin/Pasif*
    
Başarısız lık ve yüksek kullanılabilirlik hakkında daha fazla bilgi edinmek için Azure [Uygulamaları için Olağanüstü Durum Kurtarma'ya](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications)bakın.


## <a name="planning-your-disaster-recovery-architecture"></a>Olağanüstü durum kurtarma mimarinizi planlama

Olağanüstü durum kurtarma mimarinizi ayarlamanın iki teknik yönü vardır:
-  Birincil ve bekleme ortamları arasındaki örnekleri, verileri ve yapılandırmaları çoğaltmak için bir dağıtım mekanizması kullanma. Bu tür olağanüstü durum kurtarma, Veritas veya NetApp gibi Microsoft Azure iş ortağı cihazları/hizmetleri aracılığıyla Azure Site Kurtarma üzerinden yerel olarak yapılabilir. 
- Ağ/web trafiğini birincil siteden bekleme sitesine yönlendirmek için bir çözüm geliştirmek. Bu tür olağanüstü durum kurtarma, Azure DNS, Azure Trafik Yöneticisi (DNS) veya üçüncü taraf genel yük dengeleyicileri aracılığıyla elde edilebilir.

Bu makale, Ağ ve Web trafiğinin yeniden yönlendirmesi yoluyla yaklaşımlarla sınırlıdır. Azure Site Kurtarma'yı ayarlama yönergeleri için [Azure Site Kurtarma Belgeleri'ne](https://docs.microsoft.com/azure/site-recovery/)bakın.
DNS genellikle küresel ve veri merkezinin dışında olduğundan ve herhangi bir bölgesel veya kullanılabilirlik bölgesi (AZ) düzeyi hatalarından izole olduğundan, DNS ağ trafiğini yönlendirmek için en etkili mekanizmalardan biridir. DNS tabanlı bir arıza mekanizması kullanabilirsiniz ve Azure'da iki DNS hizmeti bazı moda olarak aynı şeyi gerçekleştirebilir - Azure DNS (yetkili DNS) ve Azure Trafik Yöneticisi (DNS tabanlı akıllı trafik yönlendirme). 

DNS'de bu makalede sağlanan çözümleri tartışmak için yaygın olarak kullanılan birkaç kavramı anlamak önemlidir:
- **DNS A Record** – A Records, etki alanını IPv4 adresine yönlendiren işaretçilerdir. 
- **CNAME veya Canonical adı** - Bu kayıt türü başka bir DNS kaydını işaret etmek için kullanılır. CNAME bir IP adresiyle değil, IP adresini içeren kaydın işaretçisiyle yanıt verir. 
- **Ağırlıklı Yönlendirme** – bir ağırlığı hizmet bitiş noktalarıyla ilişkilendirmeyi ve trafiği atanan ağırlıklara göre dağıtmayı seçebilirsiniz. Bu yönlendirme yöntemi, Trafik Yöneticisi içinde bulunan dört trafik yönlendirme mekanizmalarından biridir. Daha fazla bilgi için [Ağırlıklı yönlendirme yöntemine](../traffic-manager/traffic-manager-routing-methods.md#weighted)bakın.
- **Öncelik Yönlendirme** – Öncelik yönlendirme, uç noktaların sistem durumu denetimlerine dayanır. Varsayılan olarak, Azure Trafik yöneticisi tüm trafiği en yüksek öncelikli bitiş noktasına gönderir ve bir arıza veya felaket üzerine Trafik Yöneticisi trafiği ikincil bitiş noktasına yönlendirir. Daha fazla bilgi için [Öncelik yönlendirme yöntemine](../traffic-manager/traffic-manager-routing-methods.md#priority-traffic-routing-method)bakın.

## <a name="manual-failover-using-azure-dns"></a>Azure DNS kullanarak manuel hata
Olağanüstü durum kurtarma için Azure DNS manuel hata çözümü, yedekleme sitesine başarısız olmak için standart DNS mekanizmasını kullanır. Azure DNS ile manuel seçenek, soğuk bekleme veya pilot ışık yaklaşımıyla birlikte kullanıldığında en iyi şekilde çalışır. 

![Azure DNS kullanarak manuel hata](./media/disaster-recovery-dns-traffic-manager/manual-failover-using-dns.png)

*Şekil - Azure DNS kullanarak manuel arıza*

Çözüm için yapılan varsayımlar şunlardır:
- Hem birincil hem de ikincil uç noktalarda sık sık değişmeyen statik IP'ler vardır. Birincil site için IP 100.168.124.44 ve ikincil site için IP 100.168.124.43 olduğunu söylüyorlar.
- Hem birincil hem de ikincil site için bir Azure DNS bölgesi vardır. Birincil site için bitiş noktasının prod.contoso.com ve yedekleme sitesi için dr.contoso.com olduğunu söyleyin. Www\.contoso.com olarak bilinen ana uygulama için bir DNS kaydı da vardır.   
- TTL, kuruluşta belirlenen RTO SLA'nın altında veya altındadır. Örneğin, bir kuruluş uygulama felaket yanıtı RTO 60 dakika olarak ayarlarsa, o zaman TTL az 60 dakika, tercihen daha düşük daha iyi olmalıdır. 
  Azure DNS'yi el ile başarısız olmak için aşağıdaki şekilde ayarlayabilirsiniz:
- DNS bölgesi oluşturma
- DNS bölge kayıtları oluşturma
- CNAME kaydını güncelleştirme

### <a name="step-1-create-a-dns"></a>Adım 1: DNS oluşturma
Aşağıda gösterildiği gibi bir DNS\.bölgesi (örneğin, www contoso.com) oluşturun:

![Azure'da bir DNS bölgesi oluşturma](./media/disaster-recovery-dns-traffic-manager/create-dns-zone.png)

*Şekil - Azure'da bir DNS bölgesi oluşturma*

### <a name="step-2-create-dns-zone-records"></a>Adım 2: DNS bölge kayıtları oluşturma

Bu bölge içinde aşağıda göstereceğimizi\.gibi üç kayıt (örneğin - www contoso.com, prod.contoso.com ve dr.consoto.com) oluşturun.

![DNS bölge kayıtları oluşturma](./media/disaster-recovery-dns-traffic-manager/create-dns-zone-records.png)

*Şekil - Azure'da DNS bölge kayıtları oluşturma*

Bu senaryoda, site, www\.contoso.com 30 dakika, çok belirtilen RTO altında bir TTL vardır ve üretim sitesi prod.contoso.com işaret ediyor. Bu yapılandırma normal iş işlemleri sırasında. prod.contoso.com ve dr.contoso.com TTL'si 300 saniye veya 5 dakika olarak belirlenmiştir. Azure Monitor veya Azure App Insights gibi bir Azure izleme hizmeti veya Dynatrace gibi iş ortağı izleme çözümlerini, uygulamaları veya sanal altyapı düzeyi hatalarını izleyebilir veya algılayabilen evde yetiştirilen çözümleri bile kullanabilirsiniz.

### <a name="step-3-update-the-cname-record"></a>Adım 3: CNAME kaydını güncelleştirme

Hata algılandıktan sonra, kayıt değerini aşağıda gösterildiği gibi dr.contoso.com işaret etmek için değiştirin:
       
![CNAME kaydını güncelleştirme](./media/disaster-recovery-dns-traffic-manager/update-cname-record.png)

*Şekil - Azure'da CNAME kaydını güncelleştirme*

Çoğu çözümleyicinin önbelleğe alınmış bölge dosyasını yenileyeceği 30\.dakika içinde, www contoso.com'a yapılan tüm sorgular dr.contoso.com yönlendirilir.
CNAME değerini değiştirmek için aşağıdaki Azure CLI komutunu da çalıştırabilirsiniz:
 ```azurecli
   az network dns record-set cname set-record \
   --resource-group 123 \
   --zone-name contoso.com \
   --record-set-name www \
   --cname dr.contoso.com
```
Bu adım el ile veya otomasyon yoluyla yürütülebilir. Konsol veya Azure CLI üzerinden el ile yapılabilir. Azure SDK ve API, cname güncelleştirmesini otomatikleştirmek için kullanılabilir, böylece el ile müdahale gerekmez. Otomasyon, Azure işlevleri aracılığıyla veya bir üçüncü taraf izleme uygulaması içinde veya hatta şirket içinde oluşturulabilir.

### <a name="how-manual-failover-works-using-azure-dns"></a>Azure DNS kullanarak manuel hata nasıl çalışır?
DNS sunucusu arıza veya felaket bölgesinin dışında olduğundan, herhangi bir kapalı kalma süresine karşı yalıtımlıdır. Bu, kullanıcının maliyet etkin olan basit bir başarısız senaryoyu mimarına çevirmesine olanak tanır ve operatörün felaket sırasında ağ bağlantısına sahip olduğunu ve çevirmeyi yapabileceğini varsayarak her zaman çalışır. Çözüm komut dosyası ise, komut dosyasını çalıştıran sunucu veya hizmetin üretim ortamını etkileyen soruna karşı yalıtılması gerektiğinden emin olmak gerekir. Ayrıca, dünya çapında hiçbir çözümleyicinin bitiş ucunu uzun süre önbelleğe alabilmesi ve müşterilerin RTO içindeki siteye erişebilmeleri için bölgeye karşı ayarlanan düşük TTL'yi de aklınızda bulundurun. Soğuk bir bekleme ve pilot ışık için, bazı ön ısınma ve diğer idari faaliyet gerekli olabilir beri - bir de flip yapmadan önce yeterli zaman vermelidir.

## <a name="automatic-failover-using-azure-traffic-manager"></a>Azure Trafik Yöneticisi'ni kullanarak otomatik hata
Karmaşık mimarileriniz ve aynı işlevi gerçekleştirebilen birden çok kaynak kümeniz olduğunda, kaynaklarınızın durumunu kontrol etmek ve trafiği sağlıklı olmayan kaynaktan sağlıklı kaynağa yönlendirmek için Azure Trafik Yöneticisi'ni (DNS'ye göre) yapılandırabilirsiniz Kaynak. Aşağıdaki örnekte, hem birincil bölge hem de ikincil bölge tam bir dağıtıma sahiptir. Bu dağıtım bulut hizmetlerini ve eşitlenmiş bir veritabanını içerir. 

![Azure Trafik Yöneticisi'ni kullanarak otomatik hata](./media/disaster-recovery-dns-traffic-manager/automatic-failover-using-traffic-manager.png)

*Şekil - Azure Trafik Yöneticisi'ni kullanarak otomatik arıza*

Ancak, yalnızca birincil bölge kullanıcılardan gelen ağ isteklerini etkin olarak ele almaktadır. İkincil bölge yalnızca birincil bölge bir hizmet kesintisi yaşadığında etkin hale gelir. Bu durumda, tüm yeni ağ istekleri ikinci bölgeye yönlendirin. Veritabanının yedekleme anlık yakın olduğundan, hem yük dengeleyicileri sağlık kontrol edilebilir IP'ler var, ve örnekleri her zaman çalışır durumda, Bu topoloji düşük bir RTO için gitmek için bir seçenek sağlar ve herhangi bir manuel müdahale olmadan başarısız. İkincil arıza bölgesi, birincil bölgenin başarısızlığından hemen sonra canlı yayına hazır olmalıdır.
Bu senaryo, http / https ve TCP dahil olmak üzere çeşitli sistem durumu denetimleri için dahili sondaları bulunan Azure Trafik Yöneticisi'nin kullanımı için idealdir. Azure Traffic yöneticisinin, aşağıda açıklandığı gibi bir hata oluştuğunda başarısız olacak şekilde yapılandırılabilen bir kural altyapısı da vardır. Trafik Yöneticisi'ni kullanarak aşağıdaki çözümü ele alalım:
- Müşteri, 100.168.124.44 olarak statik IP'si olan prod.contoso.com olarak bilinen Bölge #1 bitiş noktası ve 100.168.124.43 olarak statik IP'si olan dr.contoso.com olarak bilinen Bölge #2 uç noktasıvardır. 
-   Bu ortamların her biri, yük dengeleyicisi gibi kamuya açık bir özellik aracılığıyla ön plana çıkar. Yük dengeleyicisi, yukarıda gösterildiği gibi DNS tabanlı bir bitiş noktası veya tam nitelikli bir etki alanı adı (FQDN) olacak şekilde yapılandırılabilir.
-   Bölge 2'deki tüm örnekler, Bölge 1 ile neredeyse gerçek zamanlı çoğaltma içindedir. Ayrıca, makine görüntüleri günceldir ve tüm yazılım/yapılandırma verileri yamalı ve Bölge 1 ile uyumludur.  
-   Otomatik ölçekleme önceden yapılandırılmıştır. 

Azure Trafik Yöneticisi ile başarısız olmayı yapılandırmak için atılan adımlar aşağıdaki gibidir:
1. Yeni bir Azure Trafik Yöneticisi profili oluşturma
2. Trafik Yöneticisi profilinde uç noktalar oluşturma
3. Sistem durumu denetimi ve başarısız yapılandırmasını ayarlama

### <a name="step-1-create-a-new-azure-traffic-manager-profile"></a>Adım 1: Yeni bir Azure Trafik Yöneticisi profili oluşturun
Contoso123 adında yeni bir Azure Trafik yöneticisi profili oluşturun ve Yönlendirme yöntemini Öncelik olarak seçin. Ilişkilendirmek istediğiniz önceden varolan bir kaynak grubunuz varsa, varolan bir kaynak grubu seçebilirsiniz, aksi takdirde yeni bir kaynak grubu oluşturabilirsiniz.

![Trafik Yöneticisi profili oluştur](./media/disaster-recovery-dns-traffic-manager/create-traffic-manager-profile.png)

*Şekil - Trafik Yöneticisi profili oluşturma*

### <a name="step-2-create-endpoints-within-the-traffic-manager-profile"></a>Adım 2: Trafik Yöneticisi profilinde uç noktaları oluşturma

Bu adımda, üretim ve olağanüstü durum kurtarma sitelerine işaret eden uç noktalar oluşturursunuz. Burada, dış uç nokta olarak **Türü** seçin, ancak kaynak Azure'da barındırılıyorsa, **Azure bitiş noktasını** da seçebilirsiniz. **Azure bitiş noktasını**seçerseniz, Uygulama **Hizmeti** veya Azure tarafından ayrılan genel **bir IP** olan bir Hedef **kaynağı** seçin. Bölge **1'in** birincil hizmeti olduğundan öncelik 1 olarak ayarlanır.
Benzer şekilde, Trafik Yöneticisi içinde de olağanüstü durum kurtarma bitiş noktası oluşturun.

![Olağanüstü durum kurtarma uç noktaları oluşturma](./media/disaster-recovery-dns-traffic-manager/create-disaster-recovery-endpoint.png)

*Şekil - Olağanüstü durum kurtarma uç noktaları oluşturma*

### <a name="step-3-set-up-health-check-and-failover-configuration"></a>Adım 3: Sistem durumu denetimi ve başarısız yapılandırmasını ayarlama

Bu adımda, DNS TTL'yi 10 saniyeye ayarlarsınız ve bu da internete bakan çoğu özyinelemeli çözümleyici tarafından onurlandırılır. Bu yapılandırma, hiçbir DNS çözümleyicisinin bilgileri 10 saniyeden uzun süre önbelleğe alacağı anlamına gelir. Bitiş noktası izleme ayarları için, yol geçerli / veya kök olarak ayarlanır, ancak bir yol değerlendirmek için bitiş noktası ayarlarını özelleştirebilirsiniz, örneğin, prod.contoso.com/index. Aşağıdaki örnekte **https** sondalama protokolü olarak gösterilmektedir. Ancak, **http** veya **tcp** de seçebilirsiniz. Protokol seçimi son uygulamaya bağlıdır. Sondalama aralığı 10 saniye olarak ayarlanır, bu da hızlı sondalama sağlar ve yeniden deneme 3 olarak ayarlanır. Sonuç olarak, art arda üç aralık bir hata kaydederse Trafik Yöneticisi ikinci bitiş noktasına kadar başarısız olur. Aşağıdaki formül otomatik bir arıza için toplam süreyi tanımlar: Failover için zaman = TTL + Yeniden deneme * Sondalama aralığı Ve bu durumda, değer 10 + 3 * 10 = 40 saniyedir (Max).
Retry 1 ve TTL 10 saniye olarak ayarlanırsa, o zaman 10 + 1 * 10 = 20 saniye üzerinde başarısız lık süresi. Yanlış pozitif sonuçlar veya herhangi bir küçük ağ blips nedeniyle başarısız olma olasılığını ortadan kaldırmak **için** 1'den büyük bir değere Yeniden Deneme'yi ayarlayın. 


![Sistem durumu denetimini ayarlama](./media/disaster-recovery-dns-traffic-manager/set-up-health-check.png)

*Şekil - Sistem durumu denetimi ve başarısız yapılandırmasını ayarlama*

### <a name="how-automatic-failover-works-using-traffic-manager"></a>Trafik Yöneticisi'ni kullanarak otomatik arıza nasıl çalışır?

Bir felaket sırasında, birincil bitiş noktası araştırılır ve durum değişiklikleri **bozulur** ve olağanüstü durum kurtarma sitesi **Çevrimiçi**kalır. Traffic Manager varsayılan olarak tüm trafiği birincil (en yüksek öncelikli) uç noktaya gönderir. Birincil bitiş noktası bozulmuş görünüyorsa, Trafik Yöneticisi trafiği sağlıklı kaldığı sürece ikinci bitiş noktasına yönlendirir. Trafik Yöneticisi'nde ek arıza uç noktaları olarak hizmet verebilen daha fazla uç nokta yı veya yükü uç noktalar arasında paylaşan yük dengeleyicileri olarak yapılandırma seçeneği vardır.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md)hakkında daha fazla bilgi edinin.
- [Azure DNS](../dns/dns-overview.md)hakkında daha fazla bilgi edinin.









