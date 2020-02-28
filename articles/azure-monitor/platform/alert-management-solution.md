---
title: Uyarı yönetimi çözümü Azure Log analytics'te | Microsoft Docs
description: Log analytics'teki uyarı yönetimi çözümü tüm uyarıları ortamınız analiz etmenize yardımcı olur.  Log Analytics içinde oluşturulan sağlamlaştırmak uyarıların yanı sıra, bu uyarılar bağlı System Center Operations Manager yönetim gruplarından Log Analytics'e içeri aktarır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/19/2018
ms.openlocfilehash: 48a825f31a1c5f2eab2fbb71b6f030b8acb5617d
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77668392"
---
# <a name="alert-management-solution-in-azure-log-analytics"></a>Azure Log analytics'teki uyarı yönetimi çözümü

![Uyarı Yönetimi simgesi](media/alert-management-solution/icon.png)

> [!NOTE]
>  Azure Izleyici, [System Center Operations Manager, Zabbix veya Nagios gibi izleme araçlarıyla](https://aka.ms/managing-alerts-other-monitoring-services)oluşturulanlar da dahil olmak üzere, [uyarılarınızı ölçeklendirmeye](https://aka.ms/azure-alerts-overview)yönelik gelişmiş özellikleri desteklemektedir.
>  


Uyarı yönetimi çözümü tüm uyarıları Log Analytics deponuzdaki analiz etmenize yardımcı olur.  Bu uyarılar, [Log Analytics tarafından oluşturulan](../../azure-monitor/platform/alerts-overview.md) veya [Nagios ya da Zabbix 'ten içeri aktarılan](../../azure-monitor/learn/quick-collect-linux-computer.md)kaynaklar dahil olmak üzere çeşitli kaynaklardan gelmiş olabilir. Çözüm Ayrıca, [bağlı System Center Operations Manager yönetim gruplarından](../../azure-monitor/platform/om-agents.md)gelen uyarıları içeri aktarır.

## <a name="prerequisites"></a>Önkoşullar
Çözüm, Log Analytics deposundaki kayıtlarla birlikte **çalışarak, bu**kayıtları toplamak için hangi yapılandırmanın gerekli olduğunu yapmanız gerekir.

- Log Analytics uyarılar için, doğrudan depoda uyarı kayıtları oluşturmak üzere [Uyarı kuralları oluşturun](../../azure-monitor/platform/alerts-overview.md) .
- Nagios ve Zabbix uyarıları için, [Bu sunucuları](../../azure-monitor/learn/quick-collect-linux-computer.md) Log Analytics Uyarıları gönderecek şekilde yapılandırın.
- System Center Operations Manager uyarılar için [Operations Manager yönetim grubunuzu Log Analytics çalışma alanınıza bağlayın](../../azure-monitor/platform/om-agents.md).  System Center Operations Manager'da oluşturulan tüm uyarılar, Log Analytics'e aktarılır.  

## <a name="configuration"></a>Yapılandırma
Çözüm [ekleme](../../azure-monitor/insights/solutions.md)bölümünde açıklanan işlemi kullanarak Log Analytics çalışma alanınıza uyarı yönetimi çözümünü ekleyin. Başka bir yapılandırma işlemi gerekmez.

## <a name="management-packs"></a>Yönetim paketleri
Ardından System Center Operations Manager yönetim grubunuzun Log Analytics çalışma alanınıza bağlıysa, bu çözümü eklediğinizde, aşağıdaki yönetim paketlerini System Center Operations Manager'da yüklenir.  Yapılandırma veya bakım gerekli yönetim paketleri yoktur.

* Microsoft System Center Advisor uyarı Yönetimi (Microsoft.IntelligencePacks.AlertManagement)

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../../azure-monitor/platform/om-agents.md).

## <a name="data-collection"></a>Veri toplama
### <a name="agents"></a>Aracılar
Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Destek | Açıklama |
|:--- |:--- |:--- |
| [Windows aracıları](agent-windows.md) | Hayır |Doğrudan Windows aracıları uyarılar oluşturmaz.  Log Analytics uyarılarını, olaylarından oluşturulabilir ve Windows aracıları toplanan performans verileri. |
| [Linux aracıları](../../azure-monitor/learn/quick-collect-linux-computer.md) | Hayır |Doğrudan Linux aracıları uyarılar oluşturmaz.  Log Analytics uyarılarını olayları ve performans verilerinden Linux aracılarından toplanan oluşturulabilir.  Nagios ve Zabbix uyarıları Linux Aracısı gerektiren bu sunuculardan toplanır. |
| [System Center Operations Manager yönetim grubu](../../azure-monitor/platform/om-agents.md) |Yes |Operations Manager aracıları üzerinde oluşturulan uyarıların yönetim grubu için teslim ve sonra Log Analytics'e iletilir.<br><br>Operations Manager aracılarının doğrudan Log analytics'e bağlantı gerekli değildir. Uyarı verileri yönetim grubundan Log Analytics deposuna iletilir. |


### <a name="collection-frequency"></a>Toplama sıklığı
- Depoda depolanan hemen sonra çözüme var olan uyarı kayıt.
- Uyarı verileri Operations Manager yönetim grubundan üç dakikada Log Analytics'e gönderilir.  

## <a name="using-the-solution"></a>Çözümü kullanma
Uyarı Yönetimi çözümünü Log Analytics çalışma alanınıza eklediğinizde **uyarı yönetimi** kutucuğu panonuza eklenir.  Bu kutucuk, sayı ve grafik temsilini son 24 saat içinde oluşturulan etkin uyarıların sayısını görüntüler.  Bu zaman aralığını değiştiremezsiniz.

![Uyarı Yönetimi kutucuğu](media/alert-management-solution/tile.png)

**Uyarı yönetimi** panosunu açmak için **uyarı yönetimi** kutucuğuna tıklayın.  Pano aşağıdaki tabloda gösterilen sütunları içerir.  Her bir sütunun Belirtilen kapsam ve zaman aralığı için söz konusu sütunun ölçütlerle eşleşen sayısına göre en çok kullanılan 10 uyarılar listeler.  Listenin en altında bulunan **Tümünü görüntüle ' ye** tıklayarak veya sütun başlığına tıklayarak, tüm listeyi sağlayan bir günlük araması çalıştırabilirsiniz.

| Sütun | Açıklama |
|:--- |:--- |
| Kritik Uyarılar |Tüm uyarılar ile uyarı adına göre gruplandırılmış kritik bir önem düzeyi.  Bu uyarı için tüm kayıtları döndüren bir günlük araması çalıştırmak için bir uyarı adına tıklayın. |
| Uyarı Bildirimleri |Tüm uyarı uyarı uyarı adına göre gruplandırılmış bir önem derecesi.  Bu uyarı için tüm kayıtları döndüren bir günlük araması çalıştırmak için bir uyarı adına tıklayın. |
| Etkin System Center Operations Manager uyarıları |Uyarıyı oluşturan kaynağa *kapalı* olarak gruplandırılmış dışında bir durum ile Operations Manager toplanan tüm uyarılar. |
| Tüm etkin uyarıları |Tüm uyarılar ile uyarı adına göre gruplandırılmış tüm önem derecesi. Yalnızca *kapalı*dışında bir durum Operations Manager uyarıları içerir. |

Sağa kaydırırsanız panoda, uyarı verileri için [günlük araması](../../azure-monitor/log-query/log-query-overview.md) gerçekleştirmek üzere tıkladığınız birkaç ortak sorgu listelenir.

![Uyarı Yönetim Panosu](media/alert-management-solution/dashboard.png)


## <a name="log-analytics-records"></a>Log Analytics kayıtları
Uyarı Yönetimi çözümü, bir **Uyarı**türü ile tüm kayıtları analiz eder.  Log Analytics tarafından oluşturulan veya Nagios veya Zabbix toplanan uyarılar çözüm tarafından doğrudan toplanmadı.

Çözüm, uyarıları System Center Operations Manager içeri aktarır ve her biri için bir **Uyarı** türü ve **Opsmanager**dir için karşılık gelen bir kayıt oluşturur.  Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:  

| Özellik | Açıklama |
|:--- |:--- |
| `Type` |*Daki* |
| `SourceSystem` |*OpsManager* |
| `AlertContext` |XML biçiminde oluşturulacak uyarıya neden veri öğesi ayrıntıları. |
| `AlertDescription` |Uyarı ayrıntılı açıklaması. |
| `AlertId` |Uyarı GUİD'si. |
| `AlertName` |Uyarının adı. |
| `AlertPriority` |Uyarı öncelik düzeyi. |
| `AlertSeverity` |Uyarı önem derecesi. |
| `AlertState` |Uyarı çözümleme durumu son. |
| `LastModifiedBy` |Uyarıyı son değiştiren kullanıcı adı. |
| `ManagementGroupName` |Uyarının verildiği yeri yönetim grubunun adı. |
| `RepeatCount` |Aynı aynı uyarının verildiği sayısı, çözümlenen bu yana nesne izlediniz. |
| `ResolvedBy` |Uyarıyı çözümleyen kullanıcı adı. Uyarı henüz çözümlenmedi, boş. |
| `SourceDisplayName` |Uyarıyı üreten izleme nesnesinin görünen adı. |
| `SourceFullName` |Uyarıyı üreten izleme nesnesi tam adı. |
| `TicketId` |System Center Operations Manager ortamı biletleri için uyarı atamak için bir işlem ile tümleşikse uyarı için bilet kimliği.  Boş yok biletin kimliği atanır. |
| `TimeGenerated` |Tarihi ve uyarının oluşturulduğu saat. |
| `TimeLastModified` |Tarihi ve uyarının son değiştirilme saati. |
| `TimeRaised` |Tarihi ve uyarının verildiği saat. |
| `TimeResolved` |Tarih ve saat, uyarı çözümlendi. Uyarı henüz çözümlenmedi, boş. |

## <a name="sample-log-searches"></a>Örnek günlük aramaları
Aşağıdaki tabloda bu çözüm tarafından toplanan uyarı kayıtlarına ilişkin örnek günlük aramaları sağlar: 

| Sorgu | Açıklama |
|:---|:---|
| Uyarı &#124; burada Analytics'teki "OpsManager" ve AlertSeverity == "error" ve TimeRaised == > ago(24h) |Son 24 saatte oluşturulan kritik uyarılar |
| Uyarı &#124; burada AlertSeverity "uyarı" ve TimeRaised == > ago(24h) |Son 24 saatte oluşturulan uyarı bildirimleri |
| Uyarı &#124; burada Analytics'teki "OpsManager" ve AlertState ==! "Kapalı" ve TimeRaised = > ago(24h) &#124; sayısı özetlemek = count() SourceDisplayName tarafından |Son 24 saatte oluşturulan etkin uyarılara sahip kaynaklar |
| Uyarı &#124; burada Analytics'teki "OpsManager" ve AlertSeverity == "error" ve TimeRaised == > ago(24h) ve AlertState! = "Kapalı" |Son 24 hala etkin olan saatte oluşturulan kritik uyarılar |
| Uyarı &#124; burada Analytics'teki "OpsManager" ve TimeRaised == > ago(24h) ve AlertState "Kapalı" == |Son 24 artık kapatılan saatte oluşturulan uyarılar |
| Uyarı &#124; burada Analytics'teki "OpsManager" ve TimeRaised == > ago(1d) &#124; sayısı özetlemek = count() AlertSeverity tarafından |Önem derecesine göre gruplandırılmış halde, son 1 günde oluşturulan uyarılar |
| Uyarı &#124; burada Analytics'teki "OpsManager" ve TimeRaised == > ago(1d) &#124; RepeatCount desc göre sırala |Yineleme sayısına göre sıralanmış halde, son 1 günde oluşturulan uyarılar |



## <a name="next-steps"></a>Sonraki adımlar
* Log Analytics’ten uyarı oluşturma hakkında daha ayrıntılı bilgi edinmek için bkz. [Log Analytics’teki Uyarılar](../../azure-monitor/platform/alerts-overview.md) .
