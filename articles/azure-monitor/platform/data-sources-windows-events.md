---
title: Azure Monitör'de Windows Event günlüklerini toplama ve analiz edin | Microsoft Dokümanlar
description: Windows Event günlüklerinin koleksiyonunu Azure Monitor'a göre nasıl yapılandırıştıracaklarını ve oluşturdukları kayıtların ayrıntılarını açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/28/2018
ms.openlocfilehash: aa34196233ce4037ef6fa49b782b9aa958f7632d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274690"
---
# <a name="windows-event-log-data-sources-in-azure-monitor"></a>Azure Monitor'da Windows olay günlüğü veri kaynakları
Birçok uygulama Windows olay günlüğüne yazdığından, Windows Olay günlükleri Windows aracılarını kullanarak veri toplamak için en yaygın [veri kaynaklarından](agent-data-sources.md) biridir.  İzlemeniz gereken uygulamalar tarafından oluşturulan özel günlükleri belirtmenin yanı sıra Sistem ve Uygulama gibi standart günlüklerden olayları da toplayabilirsiniz.

![Windows Olayları](media/data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Windows Event günlüklerini yapılandırma
[Gelişmiş Ayarlar'da Veri menüsünden](agent-data-sources.md#configuring-data-sources)Windows Event günlüklerini yapılandırın.

Azure Monitor yalnızca ayarlarda belirtilen Windows olay günlüklerinden olayları toplar.  Günlüğün adını yazıp. **+**  Her günlük için yalnızca seçili öneme sahip olaylar toplanır.  Toplamak istediğiniz belirli günlük için önemlerini denetleyin.  Olayları filtrelemek için ek ölçüt ler sağlayamazsınız.

Bir olay günlüğünün adını yazarken, Azure Monitor sık karşılaşılan olay günlüğü adları için öneriler sağlar. Eklemek istediğiniz günlük listede görünmüyorsa, günlüğün tam adını yazarak ekleyebilirsiniz. Olay görüntüleyicisini kullanarak günlüğün tam adını bulabilirsiniz. Olay görüntüleyicide, günlüğün *Özellikleri* sayfasını açın ve dizeyi *Tam Ad* alanından kopyalayın.

![Windows olaylarını yapılandırma](media/data-sources-windows-events/configure.png)

> [!NOTE]
> Windows olay günlüğündeki kritik olaylar, Azure Monitör Günlükleri'nde "Hata" şiddetine sahip olacaktır.

## <a name="data-collection"></a>Veri toplama
Azure Monitör, olay oluşturulurken izlenen olay günlüğünden seçili önemle eşleşen her olayı toplar.  Aracı, topladığı her olay günlüğündeki yerini kaydeder.  Aracı bir süre çevrimdışı kalırsa, aracı çevrimdışıyken bu olaylar oluşturulsa bile olayları en son kaldığı yerden toplar.  Olay günlüğü, aracı çevrimdışıyken toplanmamış olayların üzerine yazıldığıyla bu olayların toplanmaması için bir potansiyel vardır.

>[!NOTE]
>Azure Monitor, SQL Server tarafından oluşturulan denetim olaylarını kaynak *MSSQLSERVER'dan* anahtar kelimeler içeren event ID 18453 ile toplamaz - *Klasik* veya *Denetim Başarısı* ve anahtar kelime *0xa00000000000.*
>

## <a name="windows-event-records-properties"></a>Windows olay kayıtları özellikleri
Windows olay kayıtları bir **olay** türüne sahiptir ve aşağıdaki tablodaki özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olayın toplandığı bilgisayarın adı. |
| EtkinlikKategorisi |Etkinliğin kategorisi. |
| Eventdata |Tüm olay verileri ham formatta. |
| EventID |Olayın sayısı. |
| Eventlevel |Sayısal formda olayın şiddeti. |
| EventLevelName |Metin biçiminde olayın önem derecesi. |
| Eventlog |Olayın toplandığı olay günlüğünün adı. |
| ParametreXml |XML formatında olay parametresi değerleri. |
| ManagementGroupName |System Center Operations Manager aracıları için yönetim grubunun adı.  Diğer aracılar için bu değer`AOI-<workspace ID>` |
| RenderedAçıklama |Parametre değerleri ile olay açıklaması |
| Kaynak |Olayın kaynağı. |
| SourceSystem |Olayın aracı türü nden toplandı. <br> OpsManager – Windows aracısı, doğrudan bağlanma veya Operasyon Yöneticisi yönetilen <br> Linux – Tüm Linux ajanları  <br> AzureDepolama – Azure Tanılama |
| TimeGenerated |Olayın Windows'da oluşturulduğu tarih ve saat. |
| UserName |Olayı günlüğe kaydeden hesabın kullanıcı adı. |

## <a name="log-queries-with-windows-events"></a>Windows Etkinlikleri ile sorguları günlüğe kaydetme
Aşağıdaki tablo, Windows Olay kayıtlarını alan günlük sorgularının farklı örneklerini sağlar.

| Sorgu | Açıklama |
|:---|:---|
| Olay |Tüm Windows olayları. |
| EventLevelName == "hata" olay &#124; |Hata şiddeti ile tüm Windows olayları. |
| Olay &#124; kaynak tarafından sayım özetlemek() |Windows olaylarının kaynağa göre sayısı. |
| EventLevelName == "hata" &#124; kaynak tarafından sayım özetlemek olay &#124; |Windows hata olaylarının kaynağa göre sayısı. |


## <a name="next-steps"></a>Sonraki adımlar
* Log Analytics'i analiz için diğer [veri kaynaklarını](agent-data-sources.md) toplamak üzere yapılandırın.
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.  
* Windows [aracılarınızdan performans sayaçları koleksiyonunu](data-sources-performance-counters.md) yapılandırın.
