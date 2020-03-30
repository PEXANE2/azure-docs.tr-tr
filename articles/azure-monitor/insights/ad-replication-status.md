---
title: Etkin Dizin çoğaltma durumunu izleyin
description: Etkin Dizin Çoğaltma Durumu çözümü paketi, herhangi bir çoğaltma hatası için Etkin Dizin ortamınızı düzenli olarak izler.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/24/2018
ms.openlocfilehash: 30b0c7c87f6d55586b931be1445b175ce58565d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80055893"
---
# <a name="monitor-active-directory-replication-status-with-azure-monitor"></a>Azure Monitor ile Etkin Dizin çoğaltma durumunu izleme

![AD Çoğaltma Durumu simgesi](./media/ad-replication-status/ad-replication-status-symbol.png)

Etkin Dizin, kurumsal BT ortamının önemli bir bileşenidir. Yüksek kullanılabilirlik ve yüksek performans sağlamak için, her etki alanı denetleyicisinin Active Directory veritabanının kendi kopyası vardır. Etki alanı denetleyicileri, değişiklikleri kuruluş genelinde yaymak için birbirleriyle çoğaltır. Bu çoğaltma işlemindeki hatalar kuruluş genelinde çeşitli sorunlara neden olabilir.

AD Çoğaltma Durumu çözümü, herhangi bir çoğaltma hatası için Etkin Dizin ortamınızı düzenli olarak izler.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand-solution.md)]

## <a name="installing-and-configuring-the-solution"></a>Çözümü yükleme ve yapılandırma
Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın.

### <a name="prerequisites"></a>Ön koşullar

* AD Çoğaltma Durumu çözümü, Windows için Log Analytics aracısı (Microsoft İzleme Aracısı (MMA) olarak da adlandırılır) yüklü olan her bilgisayarda yüklü olan .NET Framework 4.6.2 veya üzeri desteklenen bir sürümünü gerektirir.  Aracı, System Center 2016 - Operations Manager, Operations Manager 2012 R2 ve Azure Monitor tarafından kullanılır.
* Çözüm, Windows Server 2008 ve 2008 R2, Windows Server 2012 ve 2012 R2 ve Windows Server 2016 çalıştıran etki alanı denetleyicilerini destekler.
* Azure portalındaki Azure pazar yerinden Active Directory Health Check çözümlerini eklemek için Bir Günlük Analizi çalışma alanı. Ek yapılandırma gerekmez.


### <a name="install-agents-on-domain-controllers"></a>Etki alanı denetleyicilerine aracılar yükleme
Değerlendirilecek etki alanı denetleyicilerine aracılar yüklemeniz gerekir. Veya, üye sunuculara aracılar yüklemeniz ve aracıları Azure Monitor'a AD çoğaltma verileri gönderecek şekilde yapılandırmanız gerekir. Windows bilgisayarlarını Azure Monitor'a nasıl bağlayacağımı anlamak [için bkz.](../../azure-monitor/platform/agent-windows.md) Etki alanı denetleyiciniz azure monitor'a bağlanmak istediğiniz varolan bir Sistem Merkezi Operasyon Yöneticisi ortamının zaten bir parçasıysa, Bkz. [İşlem Yöneticisi ni Azure Monitor'a bağlayın.](../../azure-monitor/platform/om-agents.md)

### <a name="enable-non-domain-controller"></a>Etki alanı denetleyicisini etkinleştirme
Etki alanı denetleyicilerinizden herhangi birini doğrudan Azure Monitor'a bağlamak istemiyorsanız, AD Çoğaltma Durumu çözüm paketi için veri toplamak ve verileri göndermesini sağlamak için etki alanınızdaki Azure Monitor'a bağlı başka herhangi bir bilgisayarı kullanabilirsiniz.

1. Bilgisayarın AD Çoğaltma Durumu çözümünü kullanarak izlemek istediğiniz etki alanının bir üyesi olduğunu doğrulayın.
2. [Windows bilgisayarını Azure Monitor'a bağlayın](../../azure-monitor/platform/om-agents.md) veya [mevcut İşlem Yöneticisi ortamınızı kullanarak Azure Monitor'a bağlayın](../../azure-monitor/platform/om-agents.md), zaten bağlı değilse.
3. Bu bilgisayarda, aşağıdaki kayıt defteri anahtarını ayarlayın:<br>Anahtar: **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\HealthService\Parameters\Management\<Groups ManagementGroupName>\Solutions\ADReplication**<br>Değer: **IsTarget**<br>Değer Verileri: **true**

   > [!NOTE]
   > Bu değişiklikler, Microsoft İzleme Aracısı hizmetini (HealthService.exe) yeniden başlatana kadar geçerli olmaz.
   > ### <a name="install-solution"></a>Yükleme çözümü
   > Log Analytics çalışma alanınıza **Active Directory Replication Status** çözümünü eklemek için [bir izleme çözümünü](solutions.md#install-a-monitoring-solution) yükleyin'de açıklanan işlemi izleyin. Başka bir yapılandırma işlemi gerekmez.


## <a name="ad-replication-status-data-collection-details"></a>AD Çoğaltma Durumu veri toplama ayrıntıları
Aşağıdaki tabloda veri toplama yöntemleri ve AD Çoğaltma Durumu için verilerin nasıl toplandığıyla ilgili diğer ayrıntılar gösterilmektedir.

| platform | Doğrudan Temsilci | SCOM temsilcisi | Azure Storage | SCOM gerekli mi? | Yönetim grubu aracılığıyla gönderilen SCOM aracı verileri | toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |her beş günde bir |



## <a name="understanding-replication-errors"></a>Çoğaltma hatalarını anlama

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

AD Çoğaltma Durumu döşemesi, şu anda kaç çoğaltma hatanız olduğunu görüntüler. **Kritik Çoğaltma Hataları,** Active Directory ormanınız için [mezar taşı ömrünün](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) %75'inde veya üzerinde olan hatalardır.

![AD Çoğaltma Durumu döşemesi](./media/ad-replication-status/oms-ad-replication-tile.png)

Döşemeyi tıklattığınızda, hatalar hakkında daha fazla bilgi görüntüleyebilirsiniz.
![AD Çoğaltma Durum panosu](./media/ad-replication-status/oms-ad-replication-dash.png)

### <a name="destination-server-status-and-source-server-status"></a>Hedef Sunucu Durumu ve Kaynak Sunucu Durumu
Bu sütunlar, çoğaltma hataları yaşayan hedef sunucuların ve kaynak sunucuların durumunu gösterir. Her etki alanı denetleyici soyunun sonraki sayısı, bu etki alanı denetleyicisindeki çoğaltma hatalarının sayısını gösterir.

Bazı sorunların kaynak sunucu perspektifinden, diğerlerinin de hedef sunucu açısından giderilen sorunları gidermek daha kolay olduğundan, hem hedef sunucular hem de kaynak sunucular için hatalar gösterilir.

Bu örnekte, birçok hedef sunucuda kabaca aynı sayıda hata olduğunu görebilirsiniz, ancak diğerlerinden çok daha fazla hataya sahip bir kaynak sunucu (ADDC35) vardır. ADDC35'te çoğaltma ortaklarına veri göndermemesine neden olan bazı bir sorun olması olasıdır. ADDC35'teki sorunları gidermek, hedef sunucu alanında görünen hataların çoğunu çözebilir.

### <a name="replication-error-types"></a>Çoğaltma Hata Türleri
Bu alan, işletmeniz genelinde algılanan hata türleri hakkında bilgi verir. Her hatanın benzersiz bir sayısal kodu ve hatanın temel nedenini belirlemenize yardımcı olabilecek bir ileti vardır.

Üstteki donut, ortamınızda hangi hataların giderek daha az sıklıkta göründüğü hakkında bir fikir verir.

Birden çok etki alanı denetleyicisi aynı çoğaltma hatası yla karşılaştığında size gösterir. Bu durumda, bir etki alanı denetleyicisinde bir çözümü keşfedebilir veya tanımlayabilir, ardından aynı hatadan etkilenen diğer etki alanı denetleyicilerinde yineleyebilirsiniz.

### <a name="tombstone-lifetime"></a>Tombstone Ömür Boyu
Mezar taşı ömrü, mezar taşı olarak adlandırılan silinen bir nesnenin Active Directory veritabanında ne kadar süre saklanmış olduğunu belirler. Silinen bir nesne mezar taşı yaşam boyu geçtiğinde, çöp toplama işlemi onu Etkin Dizin veritabanından otomatik olarak kaldırır.

Varsayılan mezar taşı ömrü, Windows'un en son sürümleri için 180 gündür, ancak eski sürümler için 60 gündür ve etkin dizin yöneticisi tarafından açıkça değiştirilebilir.

Yaklaşan çoğaltma hataları yaşıyorsanız veya mezar taşı yaşam boyu geçmiş olup olmadığını bilmek önemlidir. İki etki alanı denetleyicisi, mezar taşı yaşam süresi boyunca devam eden bir çoğaltma hatasıyla karşılaşırsa, temel çoğaltma hatası düzeltilmiş olsa bile bu iki etki alanı denetleyicisi arasında çoğaltma devre dışı bırakılır.

Tombstone Yaşam Alanı, devre dışı çoğaltmanın gerçekleşme tehlikesi yle karşı karşıya olduğu yerleri belirlemenize yardımcı olur. **%100'den fazla TSL** kategorisindeki her hata, kaynak ve hedef sunucusu arasında en az orman için mezar taşı ömrü boyunca çoğaltılmayan bir bölümü temsil eder.

Bu durumda, yalnızca çoğaltma hatası düzeltme yeterli olmayacaktır. En azından, çoğaltmayı yeniden başlatmadan önce kalan nesneleri tanımlamak ve temizlemek için el ile araştırmanız gerekir. Bir etki alanı denetleyicisini devre dışı bırakmanız bile gerekebilir.

Mezar taşı ömrünü geçmiş kalıcı olan çoğaltma hatalarını tanımlamanın yanı sıra, **%50-75 TSL** veya **%75-100 TSL** kategorilerine düşen hatalara da dikkat etmek istersiniz.

Bunlar açıkça kalıcı olan hatalardır, geçici değil, bu yüzden büyük olasılıkla çözmek için müdahalenize ihtiyaçları vardır. İyi haber, henüz mezar taşı ömrüne ulaşmamış olmaları. Bu sorunları hemen ve mezar taşı ömrüne *ulaşmadan önce* giderirseniz, çoğaltma en az el ile müdahale yle yeniden başlatılabilir.

Daha önce de belirtildiği gibi, AD Çoğaltma Durumu çözümünün pano döşemesi, mezar taşı ömrünün %75'inden fazlaolan (TSL'nin %100'ünden fazlaolan hatalar dahil) hataları olarak tanımlanan ortamınızdaki *kritik* çoğaltma hatalarının sayısını gösterir. Bu sayıyı 0'da tutmaya gayret edin.

> [!NOTE]
> Tüm mezar taşı yaşam boyu yüzdesi hesaplamaları Active Directory ormanınız için gerçek mezar taşı yaşam boyu hesaplamalarını temel alınarak, özel bir mezar taşı yaşam boyu değer kümeniz olsa bile bu yüzdelerin doğru olduğuna güvenebilirsiniz.
>
>

### <a name="ad-replication-status-details"></a>AD Çoğaltma durum ayrıntıları
Listelerden birinde herhangi bir öğeyi tıklattığınızda, günlük sorgusu nu kullanarak öğeyle ilgili ek ayrıntılar görürsünüz. Sonuçlar yalnızca bu öğeyle ilgili hataları göstermek için filtrelenir. Örneğin, **Hedef Sunucu Durumu (ADDC02)** altında listelenen ilk etki alanı denetleyicisini tıklattığınızda, hedef sunucu olarak listelenen etki alanı denetleyicisiyle ilgili hataları göstermek için filtre uygulanmış sorgu sonuçlarını görürsünüz:

![Sorgu sonuçlarında AD çoğaltma durum hataları](./media/ad-replication-status/oms-ad-replication-search-details.png)

Buradan daha fazla filtre uygulayabilir, günlük sorgusunu değiştirebilir ve böylece devam edebilirsiniz. Azure Monitor'da günlük sorgularını kullanma hakkında daha fazla bilgi için [bkz.](../../azure-monitor/log-query/log-query-overview.md)

**HelpLink** alanı, bir TechNet sayfasının URL'sini gösterir ve bu belirli hatayla ilgili ek ayrıntılar alabilirsiniz. Sorun giderme ve hatayı düzeltme hakkındaki bilgileri görmek için bu bağlantıyı tarayıcı pencerenize kopyalayıp yapıştırabilirsiniz.

Sonuçları Excel'e aktarmak için **Dışa** Aktar'ı da tıklatabilirsiniz. Verileri dışa aktarmak, çoğaltma hatası verilerini istediğiniz şekilde görselleştirmenize yardımcı olabilir.

![Excel'de dışa aktarılan AD çoğaltma durumu hataları](./media/ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>AD Çoğaltma Durumu SSS
**S: AD çoğaltma durumu verileri ne sıklıkta güncelleştirilir?**
C: Bilgiler her beş günde bir güncellenir.

**S: Bu verilerin ne sıklıkta güncelleştirilebildiğini yapılandırmanın bir yolu var mı?**
C: Şu anda değil.

**S: Çoğaltma durumunu görmek için tüm etki alanı denetleyicilerimi Log Analytics çalışma alanıma eklemem gerekiyor mu?**
C: Hayır, yalnızca tek bir etki alanı denetleyicisi eklenmelidir. Log Analytics çalışma alanınızda birden çok etki alanı denetleyiciniz varsa, hepsinden elde edilen veriler Azure Monitor'a gönderilir.

**S: Log Analytics çalışma alanıma herhangi bir etki alanı denetleyicisi eklemek istemiyorum. AD Çoğaltma Durumu çözümlerini kullanmaya devam edebilir miyim?**

C: Evet. Bunu etkinleştirmek için bir kayıt defteri anahtarının değerini ayarlayabilirsiniz. Bkz. [Etki alanı denetimini etkinleştir.](#enable-non-domain-controller)

**S: Veri toplama işleminin adı nedir?**
A: AdvisorAssessment.exe

**S: Verilerin toplanması ne kadar sürer?**
C: Veri toplama süresi Active Directory ortamının boyutuna bağlıdır, ancak genellikle 15 dakikadan az sürer.

**S: Ne tür veriler toplanır?**
C: Çoğaltma bilgileri LDAP üzerinden toplanır.

**S: Veriler toplandığında yapılandırmanın bir yolu var mı?**
C: Şu anda değil.

**S: Veri toplamak için hangi izinlere ihtiyacım var?**
C: Active Directory için normal kullanıcı izinleri yeterlidir.

## <a name="troubleshoot-data-collection-problems"></a>Veri toplama sorunlarını giderme
Veri toplamak için, AD Çoğaltma Durumu çözüm paketinin Log Analytics çalışma alanınıza bağlanması için en az bir etki alanı denetleyicisi gerekir. Bir etki alanı denetleyicisi bağlanana kadar, **verilerin hala toplanmaya devam ettiğini**belirten bir ileti görüntülenir.

Etki alanı denetleyicilerinizden birini bağlamak için yardıma ihtiyacınız varsa, [Windows bilgisayarlarını Azure Monitor'a](../../azure-monitor/platform/om-agents.md)bağlayın'daki belgeleri görüntüleyebilirsiniz. Alternatif olarak, etki alanı denetleyiciniz zaten varolan bir Sistem Merkezi Operasyon Yöneticisi ortamına bağlıysa, Connect System Center Operations Manager'daki belgeleri [Azure Monitor'a](../../azure-monitor/platform/om-agents.md)görüntüleyebilirsiniz.

Etki alanı denetleyicilerinizden hiçbirini doğrudan Azure Monitor'a veya Sistem Merkezi Operasyon Yöneticisi'ne bağlamak istemiyorsanız, [bkz.](#enable-non-domain-controller)

## <a name="next-steps"></a>Sonraki adımlar
* Ayrıntılı Active Directory Çoğaltma durum verilerini görüntülemek için [Azure Monitor'da Günlük sorgularını](../../azure-monitor/log-query/log-query-overview.md) kullanın.
