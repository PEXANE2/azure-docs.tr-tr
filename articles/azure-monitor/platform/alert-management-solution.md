---
title: Azure Log Analytics'te Uyarı Yönetimi çözümü | Microsoft Dokümanlar
description: Log Analytics'teki Uyarı Yönetimi çözümü, ortamınızdaki tüm uyarıları analiz yapmanıza yardımcı olur.  Log Analytics içinde oluşturulan uyarıları birleştirmenin yanı sıra, bağlı Sistem Merkezi Operasyon Yöneticisi yönetim gruplarından günlük analytics'e uyarılar da içeri girer.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/19/2018
ms.openlocfilehash: 48a825f31a1c5f2eab2fbb71b6f030b8acb5617d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77668392"
---
# <a name="alert-management-solution-in-azure-log-analytics"></a>Azure Log Analytics'te Uyarı Yönetimi çözümü

![Uyarı Yönetimi simgesi](media/alert-management-solution/icon.png)

> [!NOTE]
>  Azure Monitor artık, [Sistem Merkezi Operasyon Yöneticisi, Zabbix veya Nagios gibi izleme araçları](https://aka.ms/managing-alerts-other-monitoring-services)tarafından oluşturulanlar da dahil olmak üzere [uyarılarınızı ölçekte yönetmek](https://aka.ms/azure-alerts-overview)için geliştirilmiş yetenekleri destekler.
>  


Uyarı Yönetimi çözümü, Log Analytics deponuzdaki tüm uyarıları analiz yapmanıza yardımcı olur.  Bu [uyarılar, Log Analytics tarafından oluşturulan](../../azure-monitor/platform/alerts-overview.md) veya [Nagios veya Zabbix'ten alınan](../../azure-monitor/learn/quick-collect-linux-computer.md)kaynaklar da dahil olmak üzere çeşitli kaynaklardan gelmiş olabilir. Çözüm ayrıca bağlı sistem [merkezi operasyon yöneticisi yönetim gruplarından](../../azure-monitor/platform/om-agents.md)uyarılar da almaktadır.

## <a name="prerequisites"></a>Ön koşullar
Çözüm, Log Analytics deposundaki herhangi bir kayıtla bir **Uyarı**türüyle birlikte çalışır, bu nedenle bu kayıtları toplamak için gereken yapılandırmayı gerçekleştirmeniz gerekir.

- Günlük Analizi uyarıları için, doğrudan depoda uyarı kayıtları oluşturmak için [uyarı kuralları oluşturun.](../../azure-monitor/platform/alerts-overview.md)
- Nagios ve Zabbix uyarıları için, bu sunucuları Log Analytics'e uyarı gönderecek şekilde [yapılandırın.](../../azure-monitor/learn/quick-collect-linux-computer.md)
- Sistem Merkezi Operasyon Yöneticisi uyarıları [için, Operasyon Yöneticisi yönetim grubunuzu Log Analytics çalışma alanınıza bağlayın.](../../azure-monitor/platform/om-agents.md)  System Center Operations Manager'da oluşturulan tüm uyarılar Log Analytics'e aktarılır.  

## <a name="configuration"></a>Yapılandırma
[Ekle çözümlerinde](../../azure-monitor/insights/solutions.md)açıklanan işlemi kullanarak Günlük Analizi çalışma alanınıza Uyarı Yönetimi çözümlerini ekleyin. Başka bir yapılandırma işlemi gerekmez.

## <a name="management-packs"></a>Yönetim paketleri
Sistem Merkezi Operasyon Yöneticisi yönetim grubunuz Log Analytics çalışma alanınıza bağlıysa, bu çözümü eklediğinizde Aşağıdaki yönetim paketleri Sistem Merkezi Operasyon Yöneticisi'ne yüklenir.  Yönetim paketlerinin yapılandırması veya bakımı gerekmez.

* Microsoft System Center Danışmanı Uyarı Yönetimi (Microsoft.IntelligencePacks.AlertManagement)

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../../azure-monitor/platform/om-agents.md).

## <a name="data-collection"></a>Veri toplama
### <a name="agents"></a>Aracılar
Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Destek | Açıklama |
|:--- |:--- |:--- |
| [Windows aracıları](agent-windows.md) | Hayır |Doğrudan Windows aracıları uyarı oluşturmaz.  Günlük Analizi uyarıları, Windows aracılarından toplanan olaylar ve performans verilerinden oluşturulabilir. |
| [Linux aracıları](../../azure-monitor/learn/quick-collect-linux-computer.md) | Hayır |Doğrudan Linux aracıları uyarı üretmez.  Log Analytics uyarıları, Linux ajanlarından toplanan olaylar ve performans verilerinden oluşturulabilir.  Nagios ve Zabbix uyarıları Linux aracısı gerektiren bu sunuculardan toplanır. |
| [System Center Operations Manager yönetim grubu](../../azure-monitor/platform/om-agents.md) |Evet |Operasyon Yöneticisi temsilcileri üzerinde oluşturulan uyarılar yönetim grubuna teslim edilir ve daha sonra Log Analytics'e iletilir.<br><br>Operasyon Yöneticisi ajanlarından Log Analytics'e doğrudan bağlantı gerekmez. Uyarı verileri yönetim grubundan Log Analytics deposuna iletilir. |


### <a name="collection-frequency"></a>Toplama sıklığı
- Uyarı kayıtları, depoda depolanır depolanmaz çözüme kullanılabilir.
- Uyarı verileri, Operasyon Yöneticisi yönetim grubundan her üç dakikada bir Log Analytics'e gönderilir.  

## <a name="using-the-solution"></a>Çözümü kullanma
Log Analytics çalışma alanınıza Uyarı Yönetimi çözümlerini eklediğinizde, **Uyarı Yönetimi** döşemesi panonuza eklenir.  Bu döşeme, son 24 saat içinde oluşturulan şu anda etkin olan uyarıların sayısının sayısını ve grafik gösterimini görüntüler.  Bu zaman aralığını değiştiremezsiniz.

![Uyarı Yönetimi döşemesi](media/alert-management-solution/tile.png)

**Uyarı Yönetimi** panosunu açmak için Uyarı **Yönetimi** döşemesini tıklatın.  Pano aşağıdaki tabloda gösterilen sütunları içerir.  Her sütun, belirtilen kapsam ve zaman aralığı için bu sütunun ölçütleriyle eşleşen sayıya göre en iyi 10 uyarıyı listeler.  Sütunun altındaki **tümleri gör'e** tıklayarak veya sütun üstbilgisini tıklatarak tüm listeyi sağlayan bir günlük araması çalıştırabilirsiniz.

| Sütun | Açıklama |
|:--- |:--- |
| Kritik Uyarılar |Kritik önemderecesi ne uyarı adına göre gruplandırılan tüm uyarılar.  Bu uyarıiçin tüm kayıtları döndüren bir günlük araması çalıştırmak için bir uyarı adını tıklatın. |
| Uyarı Uyarıları |Uyarı ada göre gruplandırılan Uyarı şiddetine sahip tüm uyarılar.  Bu uyarıiçin tüm kayıtları döndüren bir günlük araması çalıştırmak için bir uyarı adını tıklatın. |
| Aktif Sistem Merkezi Operasyon Yöneticisi Uyarıları |Uyarıyı oluşturan kaynağa göre gruplanmış *Kapalı* dışında herhangi bir durumla Birlikte Operasyon Yöneticisi'nden toplanan tüm uyarılar. |
| Tüm Etkin Uyarılar |Uyarı adına göre gruplanmış herhangi bir öneme sahip tüm uyarılar. Yalnızca *Kapalı*dışında herhangi bir durumda olan Operasyon Yöneticisi uyarılarını içerir. |

Sağa kaydırırsanız, panouyarı verileri için [günlük araması](../../azure-monitor/log-query/log-query-overview.md) yapmak için tıklayabileceğiniz birkaç yaygın sorguyu listeler.

![Uyarı Yönetimi panosu](media/alert-management-solution/dashboard.png)


## <a name="log-analytics-records"></a>Log Analytics kayıtları
Uyarı Yönetimi çözümü, herhangi bir kaydı bir **Uyarı**türüyle analiz eder.  Log Analytics tarafından oluşturulan veya Nagios veya Zabbix'ten toplanan uyarılar doğrudan çözüm tarafından toplanmaz.

Çözüm, System Center Operations Manager'dan uyarı alma yapar ve her biri için bir **uyarı** türü ve **Bir SourceSystem**of OpsManager ile karşılık gelen bir kayıt oluşturur.  Bu kayıtların özellikleri aşağıdaki tabloda dır:  

| Özellik | Açıklama |
|:--- |:--- |
| `Type` |*Uyarı* |
| `SourceSystem` |*OpsManager* |
| `AlertContext` |Uyarının XML biçiminde oluşturulmasına neden olan veri öğesinin ayrıntıları. |
| `AlertDescription` |Uyarının ayrıntılı açıklaması. |
| `AlertId` |Uyarının GUID'i. |
| `AlertName` |Uyarının adı. |
| `AlertPriority` |Uyarının öncelik düzeyi. |
| `AlertSeverity` |Uyarının önem düzeyi. |
| `AlertState` |Uyarının son çözüm durumu. |
| `LastModifiedBy` |Uyarıyı en son değiştiren kullanıcının adı. |
| `ManagementGroupName` |Uyarının oluşturulduğu yönetim grubunun adı. |
| `RepeatCount` |Aynı izlenen nesne için aynı uyarının kaç kez oluşturulduğu çözüldü. |
| `ResolvedBy` |Uyarıyı çözen kullanıcının adı. Uyarı henüz çözülmediyse boş. |
| `SourceDisplayName` |Uyarıyı oluşturan izleme nesnesinin görüntü adı. |
| `SourceFullName` |Uyarıyı oluşturan izleme nesnesinin tam adı. |
| `TicketId` |Sistem Merkezi Operasyon Yöneticisi ortamı uyarılar için bilet atama işlemiyle entegre edilmişse, uyarı için bilet kimliği.  Boş bilet kimliği atanır. |
| `TimeGenerated` |Uyarının oluşturulduğu tarih ve saat. |
| `TimeLastModified` |Uyarının en son değiştirilme tarihi ve saati. |
| `TimeRaised` |Uyarının oluşturulduğu tarih ve saat. |
| `TimeResolved` |Uyarının çözüldüğü tarih ve saat. Uyarı henüz çözülmediyse boş. |

## <a name="sample-log-searches"></a>Örnek günlük aramaları
Aşağıdaki tablo, bu çözüm tarafından toplanan uyarı kayıtları için örnek günlük aramaları sağlar: 

| Sorgu | Açıklama |
|:---|:---|
| SourceSystem == "OpsManager" ve AlertSeverity == "hata" ve TimeRaised > önce (24h) nerede Uyarı &#124; |Kritik uyarılar son 24 saat içinde yükseltildi |
| Alert &#124; nerede AlertSeverity == "uyarı" ve TimeRaised > önce(24h) |Son 24 saat içinde uyarı uyarıları yükseltildi |
| SourceSystem == "OpsManager" ve AlertState != "Kapalı" ve TimeRaised > önce (24h) &#124; SourceDisplayName tarafından Count = saymak özetlemek nerede Uyarı &#124; |Son 24 saat içinde aktif uyarıları yükseltilmiş kaynaklar |
| SourceSystem == "OpsManager" ve AlertSeverity == "hata" ve TimeRaised > önce (24h) ve AlertState != "Kapalı" nerede Uyarı &#124; |Son 24 saat içinde hala etkin olan kritik uyarılar yükseltildi |
| SourceSystem == "OpsManager" ve TimeRaised > önce(24h) ve AlertState == "Kapalı" nerede Uyarı &#124; |Son 24 saat içinde yükseltilen ve şu anda kapalı olan uyarılar |
| SourceSystem == "OpsManager" ve TimeRaised > önce(1d) &#124; AlertSeverity tarafından Count = count() özetlemek nerede Uyarı &#124; |Son 1 gün içinde yükseltilen uyarılar, şiddetiyle gruplandı |
| SourceSystem == "OpsManager" ve TimeRaised > önce (1d) RepeatCount desc göre sıralama &#124; &#124; uyarı |Son 1 gün içinde yükseltilen uyarılar, yinelenen sayım değerine göre sıralanır |



## <a name="next-steps"></a>Sonraki adımlar
* Log Analytics’ten uyarı oluşturma hakkında daha ayrıntılı bilgi edinmek için bkz. [Log Analytics’teki Uyarılar](../../azure-monitor/platform/alerts-overview.md) .
