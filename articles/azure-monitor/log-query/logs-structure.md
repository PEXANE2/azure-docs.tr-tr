---
title: Azure Monitör Günlüklerinin Yapısı | Microsoft Dokümanlar
description: Azure Monitor'dan günlük verilerini almak için günlük sorgusuna ihtiyacınız var.  Bu makalede, Azure Monitor'da yeni günlük sorgularının nasıl kullanıldığı açıklanır ve bir tane oluşturmadan önce anlamanız gereken kavramlar sağlar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/22/2019
ms.openlocfilehash: 1d647ba7e8d4f0e29252dfff95099e39bab87895
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77662085"
---
# <a name="structure-of-azure-monitor-logs"></a>Azure Monitör Günlüklerinin Yapısı
[Günlük sorgusunu](log-query-overview.md) kullanarak verilerinize hızlı bir şekilde bakış açısı elde edebilme yeteneği Azure Monitor'un güçlü bir özelliğidir. Verimli ve yararlı sorgular oluşturmak için, istediğiniz verilerin nerede bulunduğu ve nasıl yapılandırıldığı gibi bazı temel kavramları anlamanız gerekir. Bu makalede, başlamak için gereken temel kavramları sağlar.

## <a name="overview"></a>Genel Bakış
Azure Monitör Günlükleri'ndeki veriler, Log Analytics çalışma alanında veya Application Insights uygulamasında depolanır. Her ikisi de [Azure Veri Gezgini](/azure/data-explorer/) tarafından desteklenmektedir, bu da güçlü veri motoru ve sorgu dilinden yararlandıkları anlamına gelir.

Hem çalışma alanlarındaki hem de uygulamalardaki veriler, her biri farklı türde verileri depolayan ve kendine özgü özellikleri olan tablolar halinde düzenlenir. Çoğu [veri kaynağı](../platform/data-sources.md) Log Analytics çalışma alanında kendi tablolarına yazarken, Application Insights bir Application Insights uygulamasında önceden tanımlanmış bir tablo kümesine yazar. Günlük sorguları, birden çok tablodaki verileri kolayca birleştirmenize ve hatta birden çok çalışma alanında tablolardaki verileri birleştirmek veya çalışma alanı ve uygulama verilerini birleştiren sorgular yazmak için bir çapraz kaynak sorgusu kullanmanıza olanak tanıyan çok esnektir.

Aşağıdaki resimde, örnek sorgularda kullanılan farklı tablolara yazan veri kaynakları örnekleri gösterilmektedir.

![Tablolar](media/logs-structure/queries-tables.png)

## <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı
Uygulama Öngörüleri dışında Azure Monitor Günlükleri tarafından toplanan tüm veriler bir [Log Analytics çalışma alanında](../platform/manage-access.md)depolanır. Özel gereksinimlerinize bağlı olarak bir veya daha fazla çalışma alanı oluşturabilirsiniz. Azure kaynaklarından gelen etkinlik günlükleri ve kaynak günlükleri, sanal makinelerdeki aracılar ve öngörüler ve izleme çözümlerinden elde edilen veriler gibi veri [kaynakları,](../platform/data-sources.md) biniş lerinin bir parçası olarak yapılandırdığınız bir veya daha fazla çalışma alanlarına veri yazar. Azure Güvenlik [Merkezi](/azure/security-center/) ve [Azure Sentinel](/azure/sentinel/) gibi diğer hizmetler de verilerini depolamak için günlük analizleri ve diğer kaynaklardan gelen verileri izleme yöntemiyle analiz edilebilir.

Çalışma alanındaki farklı tablolarda farklı türde veriler depolanır ve her tablonun benzersiz bir özellik kümesi vardır. Standart bir tablo kümesi oluşturulduğunda çalışma alanına eklenir ve farklı veri kaynakları, çözümler ve hizmetler için yerleşik olarak yeni tablolar eklenir. [Veri Toplayıcı API'sini](../platform/data-collector-api.md)kullanarak özel tablolar da oluşturabilirsiniz.

Çalışma alanı için Log Analytics'teki **Schema** sekmesinde bir çalışma alanında ve şemalarındaki tablolara göz atabilirsiniz.

![Çalışma alanı şeması](media/scope/workspace-schema.png)

Çalışma alanındaki tabloları ve bir önceki gün içinde her birinde toplanan kayıt sayısını listelemek için aşağıdaki sorguyu kullanın. 

```Kusto
union withsource = table * 
| where TimeGenerated > ago(1d)
| summarize count() by table
| sort by table asc
```
Oluşturdukları tabloların ayrıntıları için her veri kaynağı için belgelere bakın. Örnek olarak [aracı veri kaynakları,](../platform/agent-data-sources.md) [kaynak günlükleri](../platform/diagnostic-logs-schema.md)ve [izleme çözümleri](../insights/solutions-inventory.md)için makaleler verilebilir.

### <a name="workspace-permissions"></a>Çalışma alanı izinleri
Bkz. Bir çalışma alanındaki verilere erişim sağlamak için erişim denetimi stratejisini ve önerileri anlamak için [Bir Azure Monitörü Günlükleri Dağıtımı Tasarlama.](../platform/design-logs-deployment.md) Çalışma alanının kendisine erişim izni vermenin yanı sıra, [Tablo Düzeyi RBAC'ı](../platform/manage-access.md#table-level-rbac)kullanarak tablolara erişimi sınırlandırabilirsiniz.

## <a name="application-insights-application"></a>Uygulama Öngörüleri uygulaması
Application Insights'ta bir uygulama oluşturduğunuzda, Azure Monitör Günlükleri'nde ilgili bir uygulama otomatik olarak oluşturulur. Veri toplamak için yapılandırma gerekmez ve uygulama sayfa görünümleri, istekler ve özel durumlar gibi izleme verilerini otomatik olarak yazar.

Günlük Analizi çalışma alanının aksine, Application Insights uygulamasının sabit bir tablo kümesi vardır. Ek tablo oluşturulamaz, böylece uygulamaya yazmak için diğer veri kaynaklarını yapılandıramazsınız. 

| Tablo | Açıklama | 
|:---|:---|
| availabilitySonuçlar | Kullanılabilirlik testlerinden özet veriler. |
| browserZamanlamalar      | Gelen verileri işlemek için geçen süre gibi istemci performansı yla ilgili veriler. |
| Customevents        | Uygulamanız tarafından oluşturulan özel olaylar. |
| özelMetrikler       | Uygulamanız tarafından oluşturulan özel ölçümler. |
| bağımlılıklar        | Uygulamadan dış bileşenlere çağrılar. |
| Özel durum          | Uygulama çalışma zamanı tarafından atılan özel durumlar. |
| Pageviews           | Tarayıcı bilgileriyle her web sitesi görünümüyle ilgili veriler. |
| performansSayaçları | Uygulamayı destekleyen bilgi işlem kaynaklarından performans ölçümleri. |
| Istek            | Her başvuru isteğinin ayrıntıları.  |
| Izler              | Dağıtılmış izleme sonuçları. |

Uygulama için Log Analytics'teki **Şema** sekmesinde her tabloiçin şema görüntüleyebilirsiniz.

![Uygulama şeması](media/scope/application-schema.png)

## <a name="standard-properties"></a>Standart özellikler
Azure Monitör Günlükleri'ndeki her tablonun kendi şeması olsa da, tüm tablolar tarafından paylaşılan standart özellikler vardır. Her birinin ayrıntıları için [Azure Monitör Günlükleri'ndeki Standart özelliklere](../platform/log-standard-properties.md) bakın.

| Log Analytics çalışma alanı | Uygulama Öngörüleri uygulaması | Açıklama |
|:---|:---|:---|
| TimeGenerated | timestamp  | Kaydın oluşturulduğu tarih ve saat. |
| Tür          | ıtemtype   | Kaydın alındığı tablonun adı. |
| _ResourceId   |            | Kaydın ilişkili olduğu kaynak için benzersiz tanımlayıcı. |
| _IsBillable   |            | Yutulan verilerin faturalandırılabilir olup olmadığını belirtir. |
| _BilledSize   |            | Faturalandırılacak verilerin baytlarında boyutu belirtir. |

## <a name="next-steps"></a>Sonraki adımlar
- [Günlük aramaları oluşturmak ve yeniden oluşturmak için Log Analytics'i](../log-query/portals.md)kullanma hakkında bilgi edinin.
- Yeni sorgu dilini kullanarak sorgu yazma yla ilgili bir [öğreticiye](../log-query/get-started-queries.md) göz atın.
