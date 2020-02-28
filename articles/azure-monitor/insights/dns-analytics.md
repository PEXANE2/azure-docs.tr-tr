---
title: Azure Izleyici 'de çözüm DNS Analizi | Microsoft Docs
description: Güvenlik, performans ve işlemlerde DNS altyapısına Öngörüler toplamak için Azure Izleyici 'de DNS Analizi çözümünü ayarlayın ve kullanın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/20/2018
ms.openlocfilehash: 2471c29f559df5c347c62ceb4c7fd9b4ae1e5eec
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77657342"
---
# <a name="gather-insights-about-your-dns-infrastructure-with-the-dns-analytics-preview-solution"></a>DNS analizi Önizleme çözümü, DNS altyapısıyla ilgili Öngörüler toplayın

![DNS analizi simgesi](./media/dns-analytics/dns-analytics-symbol.png)

Bu makalede, güvenlik, performans ve işlemlerde DNS altyapısına Öngörüler toplamak için Azure Izleyici 'de Azure DNS Analytics çözümünün nasıl ayarlanacağı ve kullanılacağı açıklanmaktadır.

DNS analiz etmenize yardımcı olur:

- Kötü amaçlı etki alanı adlarını çözümlemeye çalışan istemcileri belirleyin.
- Eski kaynak kayıtları belirleyin.
- Sık sık sorgulanan etki alanı adlarını ve DNS sık iletişim kuran istemcileri belirleyin.
- DNS sunucularındaki istek yükünü görüntüleme.
- Dinamik DNS kayıt hataları görüntüleyin.

Çözüm, toplar, çözümler ve Windows DNS Analitik ve Denetim günlükleri ve diğer ilgili verileri, DNS sunucularından ilişkilendirir.

## <a name="connected-sources"></a>Bağlı kaynaklar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| **Bağlı kaynak** | **Destek** | **Açıklama** |
| --- | --- | --- |
| [Windows aracıları](../platform/agent-windows.md) | Yes | Çözüm, Windows aracılarından DNS bilgilerini toplar. |
| [Linux aracıları](../learn/quick-collect-linux-computer.md) | Hayır | Çözüm, doğrudan Linux aracılarından DNS bilgi toplamaz. |
| [System Center Operations Manager yönetim grubu](../platform/om-agents.md) | Yes | Çözüm, bağlı Operations Manager yönetim grubundaki aracılardan DNS bilgilerini toplar. Operations Manager aracısından Azure Izleyici 'ye doğrudan bağlantı gerekli değildir. Verileri yönetim grubundan Log Analytics çalışma alanına iletilir. |
| [Azure depolama hesabı](../platform/collect-azure-metrics-logs.md) | Hayır | Azure depolama çözümü tarafından kullanılmaz. |

### <a name="data-collection-details"></a>Veri koleksiyonu ayrıntıları

Çözüm, Log Analytics aracısını yüklendiği DNS sunucularından DNS envanteri ve DNS ilgili olay verilerini toplar. Bu veriler daha sonra Azure Izleyici 'ye yüklenir ve çözüm panosunda görüntülenir. DNS sunucuları, bölge ve kaynak kayıtları sayısı gibi envanterle ilişkili veri DNS PowerShell cmdlet'lerini çalıştırarak toplanır. Verileri iki günde bir kez güncelleştirilir. Olayla ilgili veriler, Windows Server 2012 R2 'de Gelişmiş DNS günlüğü ve tanılama tarafından verilen [analitik ve denetim günlüklerinden](https://technet.microsoft.com/library/dn800669.aspx#enhanc) gerçek zamanlı olarak toplanır.

## <a name="configuration"></a>Yapılandırma

Çözümü yapılandırmak için aşağıdaki bilgileri kullanın:

- İzlemek istediğiniz her DNS sunucusunda bir [Windows](../platform/agent-windows.md) veya [Operations Manager](../platform/om-agents.md) aracısına sahip olmanız gerekir.
- DNS Analizi çözümünü [Azure Marketi](https://aka.ms/dnsanalyticsazuremarketplace)'nden Log Analytics çalışma alanınıza ekleyebilirsiniz. Ayrıca, [Çözüm Galerisi Azure izleyici çözümlerini ekleme](solutions.md)bölümünde açıklanan süreci de kullanabilirsiniz.

Çözüm, daha fazla yapılandırma gerek olmadan veri toplamaya başlar. Ancak, veri toplamayı Özelleştir için şu yapılandırmayı kullanabilirsiniz.

### <a name="configure-the-solution"></a>Çözümü yapılandırma

Çözüm panosunda **yapılandırma** ' ya tıklayarak DNS Analizi yapılandırma sayfasını açın. Yaptığınız değişikliklerin iki tür vardır:

- **Beyaz listelenmiş etki alanı adları**. Çözüm, tüm arama sorguları işlemez. Bu, etki alanı adı son eklerini bir beyaz liste tutar. Bu beyaz liste etki alanı adı sonekleri eşleşen etki alanı adlarını çözümlemeye arama sorguları, çözüm tarafından işlenmez. Beyaz listeye alınan etki alanı adlarının işlenmemesi, Azure Izleyici 'ye gönderilen verileri iyileştirmenize yardımcı olur. Varsayılan Güvenilenler listesinde popüler genel etki alanı adları, www.google.com ve www.facebook.com gibi içerir. Kaydırarak tam varsayılan listesini görüntüleyebilir.

  Arama Öngörüler için görüntülemek istediğiniz herhangi bir etki alanı adı soneki eklemek üzere listeden değiştirebilirsiniz. İçin arama öngörülerini görüntülemek istemediğiniz herhangi bir etki alanı adı sonekini de kaldırabilirsiniz.

- **Kative Istemci eşiği**. Arama istekleri sayısının eşiğini aşan DNS istemcileri **DNS istemcileri** dikey penceresinde vurgulanır. Varsayılan Eşik 1000'dir. Eşik düzenleyebilirsiniz.

    ![İzin verilenler listesinde etki alanı adları](./media/dns-analytics/dns-config.png)

## <a name="management-packs"></a>Yönetim paketleri

Log Analytics çalışma alanınıza bağlanmak için Microsoft Monitoring Agent'ı kullanıyorsanız, aşağıdaki yönetim paketi yüklenir:

- Microsoft DNS veri toplayıcı zekası paketi (Microsoft. ıntelligencepacks. DNS)

Operations Manager yönetim grubunuzu Log Analytics çalışma alanınıza bağlıysa, bu çözümü eklediğinizde, aşağıdaki yönetim paketlerini Operations Manager'da yüklenir. Gerekli yapılandırma veya bakım bu yönetim paketlerinin yoktur:

- Microsoft DNS veri toplayıcı zekası paketi (Microsoft. ıntelligencepacks. DNS)
- Microsoft System Center Advisor DNS Analizi Yapılandırması (Microsoft.IntelligencePack.Dns.Configuration)

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../platform/om-agents.md).

## <a name="use-the-dns-analytics-solution"></a>DNS analizi çözümü kullanın

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]


DNS kutucuğu, verilerin toplandığı DNS sunucusu sayısını içerir. Ayrıca, istemciler tarafından son 24 saatte kötü niyetli alanları çözmek için yapılan isteklerinin sayısı da içerir. Kutucuğa tıkladığınızda, çözüm panosu açılır.

![DNS analizi kutucuğu](./media/dns-analytics/dns-tile.png)

### <a name="solution-dashboard"></a>Çözüm panosu

Çözüm Panosu, çözümün çeşitli özelliklerinin özetlenen bilgileri gösterir. Ayrıca, adli analiz ve tanılama için ayrıntılı görünüm bağlantılarını içerir. Varsayılan olarak, son yedi güne ait veriler gösterilir. Tarih ve saat aralığını, aşağıdaki görüntüde gösterildiği gibi **Tarih-Saat seçim denetimini**kullanarak değiştirebilirsiniz:

![Zaman seçim denetimi](./media/dns-analytics/dns-time.png)

Çözüm Panosu aşağıdaki dikey pencereleri gösterilir:

**DNS güvenliği**. Kötü amaçlı etki alanları ile iletişim kurmaya çalıştığınız DNS istemcileri bildirir. Microsoft tehdit bilgileri akışlarından kullanarak DNS analizi istemci kötü amaçlı etki alanlarına erişmeye çalıştığınız IP'ler algılayabilir. Çoğu durumda, kötü amaçlı yazılımdan etkilenen aygıtların "dışarı"komut ve Denetim"merkezi kötü amaçlı etki alanının için kötü amaçlı yazılım etki alanı adını çözümleme olarak arama".

![DNS güvenlik dikey penceresi](./media/dns-analytics/dns-security-blade.png)

Bir istemci IP listesinde tıkladığınızda, günlük araması açılır ve ilgili sorgu arama ayrıntılarını gösterir. Aşağıdaki örnekte, DNS Analizi bir [IRCBot](https://www.microsoft.com/en-us/wdsi/threats/malware-encyclopedia-description?Name=Backdoor:Win32/IRCbot&threatId=2621)ile iletişimin yapıldığını algıladı:

![Ircbot gösteren günlük arama sonuçları](./media/dns-analytics/ircbot.png)

Bilgileri tanımlamanıza yardımcı olur:

- İstemci iletişimi başlatılan IP.
- Çözümlenen kötü amaçlı IP ile etki alanı adı.
- IP adresleri etki alanı adını çözer.
- Kötü amaçlı IP adresi.
- Sorunun önem derecesi.
- Kötü amaçlı IP kara nedeni.
- Algılama zamanı.

**Sorgulanan etki alanları**. Ortamınızda DNS istemcileri tarafından sorgulanan en sık kullanılan etki alanı adlarını sağlar. Sorgulanan etki alanı adlarının listesini görüntüleyebilirsiniz. Ayrıca belirli bir etki alanı adı günlük araması'nda arama isteği ayrıntılara detaya gidebilirsiniz.

![Etki alanları sorgulanan dikey penceresi](./media/dns-analytics/domains-queried-blade.png)

**DNS istemcileri**. Seçilen dönemdeki sorgu sayısı için *eşiğe ulaşan* istemcileri bildirir. Tüm DNS istemcilerini ve onlar tarafından günlük aramasında yapılan sorguları ayrıntılarını listesini görüntüleyebilirsiniz.

![DNS Clıents dikey](./media/dns-analytics/dns-clients-blade.png)

**Dınamık DNS kayıtları**. Kayıt hataları rapor adı. Adres [kaynak kayıtları](https://en.wikipedia.org/wiki/List_of_DNS_record_types) (tür A ve AAAA) için tüm kayıt hatalarının kayıt isteklerini gerçekleştiren Istemci IP 'leri ile birlikte vurgulanacaktır. Sonra bu bilgileri, aşağıdaki adımları izleyerek kayıt hatası kök nedenini bulmak için de kullanabilirsiniz:

1. İstemci güncelleştirilmeye çalışılırken ad için yetkiliyse bölge bulun.

1. Çözüm bölge Envanter bilgilerini kullanın.

1. Dinamik güncelleştirme bölgesi için etkin olduğunu doğrulayın.

1. Veya bölgenin güvenli dinamik güncelleştirme için yapılandırılmış olup olmadığını denetleyin.

    ![Dinamik DNS kayıtları dikey penceresi](./media/dns-analytics/dynamic-dns-reg-blade.png)

**Ad kayıt istekleri**. Üst kutucuk DNS dinamik güncelleştirme isteklerinin başarılı ve başarısız bir eğilim çizgisi gösterir. Daha düşük kutucuk hataları sayısına göre sıralanmış, DNS sunucularına başarısız DNS güncelleştirme istekleri gönderen üst 10 istemcileri de listeler.

![Ad kaydı istekleri dikey penceresi](./media/dns-analytics/name-reg-req-blade.png)

**Örnek DDI Analizi sorguları**. Ham analiz verileri doğrudan getiren en yaygın arama sorgularının listesini içerir.


![Örnek sorgular](./media/dns-analytics/queries.png)

Özel raporlama için kendi sorgularınızı oluşturmak için başlangıç noktası olarak bu sorguları kullanabilirsiniz. Sorguların sonuçlarını görüntüleyen DNS Analytics günlük araması sayfaya bağlantı:

- **DNS sunucularının listesi**. Kendi ilişkili FQDN, etki alanı adı, orman adı ve sunucu IP'leri ile tüm DNS sunucularının bir listesini gösterir.
- **DNS bölgeleri listesi**. Tüm DNS bölgelerinin ilişkili bölge adı, dinamik güncelleştirme durumu, ad sunucularını ve DNSSEC imzalama durumu ile bir listesini gösterir.
- **Kullanılmayan kaynak kayıtları**. Tüm kullanılmayan/eski kaynak kayıtlarının bir listesini gösterir. Bu liste, kaynak kayıt adı, kaynak kaydı türü, ilişkili bir DNS sunucusu, kayıt oluşturma süresi ve bölge adını içerir. Artık kullanımda olan DNS kaynak kayıtları tanımlamak için bu listeyi kullanabilirsiniz. Bu bilgilere dayanarak, DNS sunucularından girişler daha sonra kaldırabilirsiniz.
- **DNS sunucuları sorgu yükü**. DNS sunucularınızda DNS iş yükünün bir perspektif alabilmeniz bilgileri gösterir. Bu bilgiler sunucular için kapasite planlamaya yardımcı olabilir. Görünümü grafik görselleştirmeye dönüştürmek için **ölçümler** sekmesine gidebilirsiniz. Bu görünüm, DNS yük DNS sunucularınızın nasıl dağıtıldığını anlamanıza yardımcı olur. Bu DNS sorgusu her sunucu için oranı eğilimleri gösterir.

    ![DNS sunucuları sorgu günlük arama sonuçları](./media/dns-analytics/dns-servers-query-load.png)

- **Sorgu yükü DNS bölgeleri**. Çözüm tarafından yönetilen DNS sunucularında tüm bölgeleri DNS bölgesi sorgu başına saniye istatistiklerini gösterir. Ayrıntılı kayıtlardan görünümü sonuçların grafik görselleştirmesine değiştirmek için **ölçümler** sekmesine tıklayın.
- **Yapılandırma olayları**. Tüm DNS yapılandırma değişikliği olayları ve ilişkili iletilerini gösterir. Daha sonra bu olayları olay, olay kimliği, DNS sunucusunun saatini temel alan filtre veya görev Kategorisi'ne. Verileri, belirli DNS sunucularına yönelik olarak belirli zamanlarda yapılan değişiklikleri denetim yardımcı olabilir.
- **DNS analitik günlüğü**. Çözüm tarafından yönetilen DNS sunucularında tüm analitik olaylarını gösterir. Daha sonra bu olayları olay, olay kimliği, DNS sunucusunun saatini temel alan filtre arama sorgu ve sorgu türünü görev Kategorisi'ne yapılan istemci IP adresi. DNS sunucusu analitik olaylarını etkinlik DNS sunucusunda izlemeyi etkinleştirin. Sunucu tarafından gönderilen veya alınan DNS bilgilerini her zaman analitik bir olay günlüğe kaydedilir.

### <a name="search-by-using-dns-analytics-log-search"></a>DNS analizi günlük araması'nı kullanarak arama

Günlük araması sayfasında, bir sorgu oluşturabilirsiniz. Model denetimlerini kullanarak arama sonuçlarınızı filtreleyebilirsiniz. Dönüştürme, filtre ve raporlamak için Gelişmiş sorgu sonuçlarınız üzerinde de oluşturabilirsiniz. Aşağıdaki sorgular kullanarak başlatın:

1. **Arama sorgusu kutusuna**, çözüm tarafından yönetilen DNS sunucuları tarafından oluşturulan tüm DNS olaylarını görüntülemek için `DnsEvents` yazın. Sonuçları arama sorguları, dinamik kayıtları ve yapılandırma değişiklikleri ile ilgili tüm olaylar için günlük verileri listeleyin.

    ![DnsEvents günlük araması](./media/dns-analytics/log-search-dnsevents.png)  

    a. Arama sorgularının günlük verilerini görüntülemek için, sol taraftaki model denetiminden **alt tür** filtresi olarak **lookupquery** ' yi seçin. Seçili süre için tüm arama sorgu olaylarını listeleyen bir tablo görüntülenir.

    b. Dinamik kayıtlara ait günlük verilerini görüntülemek için, sol taraftaki model denetiminden **alt tür** filtresi olarak **dynamicregistration** ' u seçin. Seçili süre için tüm dinamik kayıt olaylarını listeleyen bir tablo görüntülenir.

    c. Yapılandırma değişikliklerinin günlük verilerini görüntülemek için, sol taraftaki model denetiminden **alt tür** filtresi olarak **ConfigurationChange** ' ı seçin. Seçili süre için tüm yapılandırma değişikliği olaylarını listeleyen bir tablo görüntülenir.

1. **Arama sorgusu kutusuna**, çözüm tarafından yönetilen DNS sunucuları IÇIN tüm DNS envanteriyle ilgili verileri görüntülemek üzere `DnsInventory` yazın. Sonuçlar, DNS sunucuları, DNS bölgeleri ve kaynak kayıtları için günlük verileri listeler.

    ![DnsInventory günlük araması](./media/dns-analytics/log-search-dnsinventory.png)
    
## <a name="troubleshooting"></a>Sorun giderme

Ortak sorun giderme adımları:

1. Eksik DNS arama verileri-bu sorunu gidermek Için yapılandırmayı sıfırlamayı veya portalda yalnızca yapılandırma sayfasını yüklemeyi deneyin. Sıfırlamak için, bir ayarı başka bir değerle değiştirmeniz, sonra yeniden özgün değere değiştirmeniz ve yapılandırmayı kaydetmeniz yeterlidir.

## <a name="feedback"></a>Geri Bildirim

Geri bildirim sağlamak için [Log Analytics UserVoice sayfasını](https://aka.ms/dnsanalyticsuservoice) ziyaret ederek DNS analizi özellikleri üzerinde çalışmak üzere fikirler gönderin. 

## <a name="next-steps"></a>Sonraki adımlar

Ayrıntılı DNS günlüğü kayıtlarını görüntülemek için [sorgu günlükleri](../log-query/log-query-overview.md) .
