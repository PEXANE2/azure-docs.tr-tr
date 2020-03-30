---
title: Azure Monitör'de DNS Analytics çözümü | Microsoft Dokümanlar
description: Güvenlik, performans ve işlemler le ilgili DNS altyapısı hakkında bilgi toplamak için Azure Monitor'da DNS Analytics çözümünü ayarlayın ve kullanın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/20/2018
ms.openlocfilehash: 2471c29f559df5c347c62ceb4c7fd9b4ae1e5eec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77657342"
---
# <a name="gather-insights-about-your-dns-infrastructure-with-the-dns-analytics-preview-solution"></a>DNS Analytics Preview çözümüyle DNS altyapınız hakkında bilgi edinin

![DNS Analytics sembolü](./media/dns-analytics/dns-analytics-symbol.png)

Bu makalede, güvenlik, performans ve işlemler le ilgili DNS altyapısı hakkında öngörüler toplamak için Azure Monitor'daki Azure DNS Analytics çözümünü nasıl kuracağınız ve kullanacağız açıklanmaktadır.

DNS Analytics şunları anlamanıza yardımcı olur:

- Kötü amaçlı alan adlarını çözmeye çalışan istemcileri tanımlayın.
- Eski kaynak kayıtlarını tanımlayın.
- Sık sorgulanmış alan adlarını ve konuşkan DNS istemcilerini tanımlayın.
- DNS sunucularında istek yükünü görüntüleyin.
- Dinamik DNS kayıt hatalarını görüntüleyin.

Çözüm, DNS sunucularınızdan Windows DNS analitik ve denetim günlüklerini ve diğer ilgili verileri toplar, analiz eder ve ilişkilendirir.

## <a name="connected-sources"></a>Bağlı kaynaklar

Aşağıdaki tabloda, bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| **Bağlı kaynak** | **Destek** | **Açıklama** |
| --- | --- | --- |
| [Windows aracıları](../platform/agent-windows.md) | Evet | Çözüm, Windows aracılarından DNS bilgileri toplar. |
| [Linux aracıları](../learn/quick-collect-linux-computer.md) | Hayır | Çözüm, doğrudan Linux ajanlarından DNS bilgileri toplamaz. |
| [System Center Operations Manager yönetim grubu](../platform/om-agents.md) | Evet | Çözüm, bağlı bir Operasyon Yöneticisi yönetim grubundaki aracılardan DNS bilgileri toplar. Operations Manager aracısından Azure Monitor'a doğrudan bağlantı gerekmez. Veriler yönetim grubundan Log Analytics çalışma alanına iletilir. |
| [Azure depolama hesabı](../platform/collect-azure-metrics-logs.md) | Hayır | Azure depolama çözümü tarafından kullanılmaz. |

### <a name="data-collection-details"></a>Veri toplama ayrıntıları

Çözüm, Log Analytics aracısının yüklü olduğu DNS sunucularından DNS envanteri ve DNS olayla ilgili verileri toplar. Bu veriler daha sonra Azure Monitor'a yüklenir ve çözüm panosunda görüntülenir. DNS sunucularının, bölgelerinin ve kaynak kayıtlarının sayısı gibi stokla ilgili veriler, DNS PowerShell cmdlets çalıştırılarak toplanır. Veriler iki günde bir güncellenir. Etkinlikle ilgili veriler, Windows Server 2012 R2'de geliştirilmiş DNS günlüğü ve tanılama tarafından sağlanan [analitik ve denetim günlüklerinden](https://technet.microsoft.com/library/dn800669.aspx#enhanc) gerçek zamanlı olarak toplanır.

## <a name="configuration"></a>Yapılandırma

Çözümü yapılandırmak için aşağıdaki bilgileri kullanın:

- İzlemek istediğiniz her DNS sunucusunda bir [Windows](../platform/agent-windows.md) veya [Operations Manager](../platform/om-agents.md) aracısı olmalıdır.
- Azure [Marketi'nden](https://aka.ms/dnsanalyticsazuremarketplace)Log Analytics çalışma alanınıza DNS Analytics çözümlerini ekleyebilirsiniz. [Çözüm Galerisi'nden Azure Monitörü Ekle çözümlerinde](solutions.md)açıklanan işlemi de kullanabilirsiniz.

Çözüm, daha fazla yapılandırmaya gerek kalmadan veri toplamaya başlar. Ancak, veri toplamaözelleştirmek için aşağıdaki yapılandırmayı kullanabilirsiniz.

### <a name="configure-the-solution"></a>Çözümü yapılandırma

Çözüm panosunda, DNS Analytics Configuration sayfasını açmak için **Yapılandırma'yı** tıklatın. Yapabileceğiniz iki tür yapılandırma değişikliği vardır:

- **Beyaz Liste alan adları.** Çözüm tüm arama sorgularını işlemez. Etki alanı adı sonekleri bir beyaz liste tutar. Bu beyaz listedeki etki alanı adı soneklerine eşleşen alan adlarına çözümleyen arama sorguları çözüm tarafından işlenmez. Beyaz listedeki alan adlarının işlenmemesi, Azure Monitor'a gönderilen verileri optimize etmeye yardımcı olur. Varsayılan beyaz liste, www.google.com ve www.facebook.com gibi popüler genel etki alanı adlarını içerir. Kaydırma yaparak varsayılan listenin tamamını görüntüleyebilirsiniz.

  Arama istatistiklerini görüntülemek istediğiniz herhangi bir etki alanı adı eki eklemek için listeyi değiştirebilirsiniz. Ayrıca, arama istatistiklerini görüntülemek istemediğiniz herhangi bir etki alanı adı ekini de kaldırabilirsiniz.

- **Konuşkan İstemci Eşiği**. Arama isteği sayısı için eşiği aşan DNS istemcileri **DNS Istemcileri** bıçağında vurgulanır. Varsayılan eşik 1.000'dir. Eşiği atabilirsiniz.

    ![Beyaz listeye alınan alan adları](./media/dns-analytics/dns-config.png)

## <a name="management-packs"></a>Yönetim paketleri

Log Analytics çalışma alanınıza bağlanmak için Microsoft İzleme Aracısı kullanıyorsanız, aşağıdaki yönetim paketi yüklenir:

- Microsoft DNS Veri Toplayıcı İstihbarat Paketi (Microsoft.IntelligencePacks.Dns)

Operasyon Yöneticisi yönetim grubunuz Log Analytics çalışma alanınıza bağlıysa, bu çözümü eklediğinizde aşağıdaki yönetim paketleri Operations Manager'a yüklenir. Bu yönetim paketlerinin gerekli yapılandırması veya bakımı yoktur:

- Microsoft DNS Veri Toplayıcı İstihbarat Paketi (Microsoft.IntelligencePacks.Dns)
- Microsoft System Center Advisor DNS Analytics Configuration (Microsoft.IntelligencePack.Dns.Configuration)

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../platform/om-agents.md).

## <a name="use-the-dns-analytics-solution"></a>DNS Analytics çözümlerini kullanma

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]


DNS döşemesi, verilerin toplandığı DNS sunucusu sayısını içerir. Ayrıca, son 24 saat içinde kötü amaçlı etki alanlarını çözmek için istemciler tarafından yapılan isteklerin sayısını da içerir. Döşemeyi tıklattığınızda, çözüm panosu açılır.

![DNS Analytics karosu](./media/dns-analytics/dns-tile.png)

### <a name="solution-dashboard"></a>Çözüm panosu

Çözüm panosu, çözümün çeşitli özellikleri için özet bilgileri gösterir. Ayrıca adli analiz ve tanı için ayrıntılı görünüm bağlantıları içerir. Varsayılan olarak, veriler son yedi gün için gösterilir. Aşağıdaki resimde gösterildiği gibi **tarih-saat seçim denetimini**kullanarak tarih ve saat aralığını değiştirebilirsiniz:

![Zaman seçimi kontrolü](./media/dns-analytics/dns-time.png)

Çözüm panosu aşağıdaki bıçakları gösterir:

**DNS Güvenlik**. Kötü amaçlı etki alanlarıyla iletişim kurmaya çalışan DNS istemcilerini raporlar. DNS Analytics, Microsoft tehdit zekası akışlarını kullanarak kötü amaçlı etki adlarına erişmeye çalışan istemci IP'lerini algılayabilir. Çoğu durumda, kötü amaçlı yazılım bulaşmış cihazlar kötü amaçlı yazılım etki alanı adını çözerek kötü amaçlı etki alanının "komut ve denetim" merkezine "dışarı çevirmek".

![DNS Güvenlik bıçağı](./media/dns-analytics/dns-security-blade.png)

Listede bir istemci IP'yi tıklattığınızda, Günlük Arama açılır ve ilgili sorgunun arama ayrıntılarını gösterir. Aşağıdaki örnekte, DNS Analytics iletişimin bir [IRCbot](https://www.microsoft.com/en-us/wdsi/threats/malware-encyclopedia-description?Name=Backdoor:Win32/IRCbot&threatId=2621)ile yapıldığını tespit etti:

![ircbot gösteren günlük arama sonuçları](./media/dns-analytics/ircbot.png)

Bilgiler, aşağıdakileri belirlemenize yardımcı olur:

- İletişimi başlatan istemci IP..
- Kötü amaçlı IP'ye giderilen alan adı.
- Etki alanı adının çözüme kavuşturuldığı IP adresleri.
- Kötü amaçlı IP adresi.
- Sorunun ciddiyeti.
- Kötü niyetli IP kara listeye nedeni.
- Algılama zamanı.

**Sorgulanmış Etki Alanları**. Ortamınızdaki DNS istemcileri tarafından sorgulanan en sık alan adlarını sağlar. Sorgulanmış tüm alan adlarının listesini görüntüleyebilirsiniz. Ayrıca, Günlük Arama'da belirli bir etki alanı adının arama isteği ayrıntılarını da inceleyebilirsiniz.

![Etki Alanları Sorgulanmış bıçak](./media/dns-analytics/domains-queried-blade.png)

**DNS Istemcileri**. Seçilen zaman diliminde sorgu sayısı *için eşiği ihlal eden* istemcileri raporlar. Tüm DNS istemcilerinin listesini ve onlar tarafından yapılan sorguların ayrıntılarını Günlük Arama'da görüntüleyebilirsiniz.

![DNS İstemci ler bıçak](./media/dns-analytics/dns-clients-blade.png)

**Dinamik DNS Kayıtları**. Raporlar ad kayıt hataları. Adres [kaynak kayıtları](https://en.wikipedia.org/wiki/List_of_DNS_record_types) (Tip A ve AAAA) için tüm kayıt hataları, kayıt isteklerini yapan istemci IP'leri ile birlikte vurgulanır. Daha sonra aşağıdaki adımları izleyerek kayıt hatasının temel nedenini bulmak için bu bilgileri kullanabilirsiniz:

1. İstemcinin güncelleştirmeye çalıştığı ad için yetkili olan bölgeyi bulun.

1. O bölgenin stok bilgilerini denetlemek için çözümü kullanın.

1. Bölge için dinamik güncelleştirmenin etkin olduğunu doğrulayın.

1. Güvenli dinamik güncelleştirme için bölgenin yapılandırılıp yapılandırılmadığını denetleyin.

    ![Dinamik DNS Kayıtları bıçak](./media/dns-analytics/dynamic-dns-reg-blade.png)

**Ad kayıt istekleri.** Üst döşeme, başarılı ve başarısız DNS dinamik güncelleştirme isteklerinin eğilim çizgisini gösterir. Alt döşeme, başarısız DNS güncelleştirme isteklerini DNS sunucularına gönderen en iyi 10 istemciyi listeler ve hata sayısına göre sıralanır.

![İsim kayıt talepleri bıçak](./media/dns-analytics/name-reg-req-blade.png)

**Örnek DDI Analitik Sorguları.** Ham analitik verilerini doğrudan getiren en yaygın arama sorgularının listesini içerir.


![Örnek sorgular](./media/dns-analytics/queries.png)

Bu sorguları, özelleştirilmiş raporlama için kendi sorgularınızı oluşturmak için başlangıç noktası olarak kullanabilirsiniz. Sorgular, sonuçların görüntülendiği DNS Analytics Log Search sayfasına bağlanır:

- **DNS Sunucuları listesi.** İlişkili FQDN, etki alanı adı, orman adı ve sunucu IP'leri ile tüm DNS sunucularının listesini gösterir.
- **DNS Bölgeleri listesi.** İlişkili bölge adı, dinamik güncelleştirme durumu, ad sunucuları ve DNSSEC imzalama durumuna sahip tüm DNS bölgelerinin listesini gösterir.
- **Kullanılmayan Kaynak Kayıtları.** Kullanılmayan/eski kaynak kayıtlarının listesini gösterir. Bu liste kaynak kayıt adı, kaynak kayıt türü, ilişkili DNS sunucusu, kayıt oluşturma zamanı ve bölge adı içerir. Bu listeyi, artık kullanılmamaktadır DNS kaynak kayıtlarını tanımlamak için kullanabilirsiniz. Bu bilgilere dayanarak, bu girişleri DNS sunucularından kaldırabilirsiniz.
- **DNS Sunucular Sorgu Yük**. DNS sunucularınızdaki DNS yüküne bir bakış açısı kazandıracak şekilde bilgileri gösterir. Bu bilgiler, sunucuların kapasitesini planlamanıza yardımcı olabilir. Görünümü grafik görselleştirmeyle değiştirmek için **Ölçümler** sekmesine gidebilirsiniz. Bu görünüm, DNS yükünün DNS sunucularınıza nasıl dağıtıldığını anlamanıza yardımcı olur. Her sunucu için DNS sorgu oranı eğilimlerini gösterir.

    ![DNS sunucuları günlük arama sonuçlarını sorgular](./media/dns-analytics/dns-servers-query-load.png)

- **DNS Bölgeleri Sorgu Yükü**. Çözüm tarafından yönetilen DNS sunucularında bulunan tüm bölgelerin saniye başına DNS bölge sorgusu istatistiklerini gösterir. Görünümü ayrıntılı kayıtlardan sonuçların grafik görselleştirmesine değiştirmek için **Ölçümler** sekmesini tıklatın.
- **Yapılandırma Olayları**. Tüm DNS yapılandırma değişikliği olaylarını ve ilişkili iletileri gösterir. Daha sonra bu olayları olay zamanına, olay kimliğine, DNS sunucusuna veya görev kategorisine göre filtreleyebilirsiniz. Veriler, belirli zamanlarda belirli DNS sunucularında yapılan değişiklikleri denetlemenize yardımcı olabilir.
- **DNS Analitik Günlük**. Çözüm tarafından yönetilen tüm DNS sunucularında tüm analitik olayları gösterir. Daha sonra bu olayları olay zamanına, olay kimliğine, DNS sunucusuna, arama sorgusunu yapan istemci IP'sine ve sorgu türü görev kategorisine göre filtreleyebilirsiniz. DNS sunucu analitik olayları, DNS sunucusunda etkinlik izlemeyi sağlar. Sunucu Her DNS bilgisini gönderdiğinde veya aldığında bir analitik olay günlüğe kaydedilir.

### <a name="search-by-using-dns-analytics-log-search"></a>DNS Analytics Günlük Arama'yı kullanarak arama yapın

Günlük Arama sayfasında bir sorgu oluşturabilirsiniz. Fason denetimlerini kullanarak arama sonuçlarınızı filtreleyebilirsiniz. Ayrıca, sonuçlarınızı dönüştürmek, filtrelemek ve raporlamak için gelişmiş sorgular da oluşturabilirsiniz. Aşağıdaki sorguları kullanarak başlayın:

1. Arama **sorgusu kutusunda,** `DnsEvents` çözüm tarafından yönetilen DNS sunucuları tarafından oluşturulan tüm DNS olaylarını görüntülemek için yazın. Sonuçlar, arama sorguları, dinamik kayıtlar ve yapılandırma değişiklikleriyle ilgili tüm olayların günlük verilerini listeler.

    ![DnsEvents günlük arama](./media/dns-analytics/log-search-dnsevents.png)  

    a. Arama sorguları için günlük verilerini görüntülemek için, soldaki facet denetiminden **Alt tür** filtresi olarak **LookUpQuery'yi** seçin. Seçili dönemiçin tüm arama sorgusu olaylarını listeleyen bir tablo görüntülenir.

    b. Dinamik kayıtlar için günlük verilerini görüntülemek için, soldaki fason denetiminden **Alt tür** filtresi olarak **DynamicRegistration'ı** seçin. Seçili döneme ait tüm dinamik kayıt olaylarını listeleyen bir tablo görüntülenir.

    c. Yapılandırma değişiklikleri için günlük verilerini görüntülemek için, soldaki fason denetiminden **Alt tür** filtresi olarak **ConfigurationChange'i** seçin. Seçili dönemiçin tüm yapılandırma değişikliği olaylarını listeleyen bir tablo görüntülenir.

1. Arama **sorgu kutusunda,** `DnsInventory` çözüm tarafından yönetilen DNS sunucuları için DNS stokla ilgili tüm verileri görüntülemek için yazın. Sonuçlar, DNS sunucuları, DNS bölgeleri ve kaynak kayıtları için günlük verilerini listeler.

    ![DnsInventory günlük arama](./media/dns-analytics/log-search-dnsinventory.png)
    
## <a name="troubleshooting"></a>Sorun giderme

Sık karşılaşılan sorun giderme adımları:

1. Eksik DNS Arama Verileri - Bu sorunu gidermek için config'i sıfırlamayı veya yapılandırma sayfasını portala bir kez yüklemeyi deneyin. Sıfırlama için, bir ayarı başka bir değere değiştirmenin, ardından özgün değere geri değiştirmeve config'i kaydetmen.

## <a name="feedback"></a>Geri Bildirim

Geri bildirim sağlamak için, Üzerinde çalışılabilmek için DNS Analytics özellikleri yle ilgili fikirler göndermek için [Log Analytics UserVoice sayfasını](https://aka.ms/dnsanalyticsuservoice) ziyaret edin. 

## <a name="next-steps"></a>Sonraki adımlar

Ayrıntılı DNS günlük kayıtlarını görüntülemek için [günlükleri sorgula.](../log-query/log-query-overview.md)
