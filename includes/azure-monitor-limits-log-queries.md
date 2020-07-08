---
title: dosya dahil etme
description: dosya dahil etme
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 07/22/2019
ms.author: bwren
ms.custom: include file
ms.openlocfilehash: 627b020ce618a2a1f2646a95e143947876bd6a15
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82072646"
---
### <a name="general-query-limits"></a>Genel sorgu limitleri

| Sınır | Açıklama |
|:---|:---|
| Sorgu dili | Azure Izleyici, Azure Veri Gezgini ile aynı [kusto sorgu dilini](/azure/kusto/query/) kullanır. Azure Izleyici 'de desteklenmeyen KQL dil öğelerine yönelik [Azure İzleyici günlüğü sorgu dili farklılıkları](../articles/azure-monitor/log-query/data-explorer-difference.md) bölümüne bakın. |
| Azure bölgeleri | Veri, birden çok Azure bölgesindeki çalışma alanlarını Log Analytics yayıldığında günlük sorguları aşırı yük yaşar. Ayrıntılar için bkz. [sorgu sınırları](../articles/azure-monitor/log-query/scope.md#query-limits) . |
| Çapraz kaynak sorguları | Tek bir sorgudaki maksimum Application Insights kaynak sayısı ve Log Analytics çalışma alanı 100 ile sınırlıdır.<br>Görünüm tasarımcısında çapraz kaynak sorgusu desteklenmez.<br>Log uyarılarındaki çapraz kaynak sorgusu, yeni scheduledQueryRules API 'sinde desteklenir.<br>Ayrıntılar için bkz. [çapraz kaynak sorgu sınırları](../articles/azure-monitor/log-query/cross-workspace-query.md#cross-resource-query-limits) . |

### <a name="user-query-throttling"></a>Kullanıcı sorgu azaltma
Azure Izleyici, çok fazla sayıda sorgu gönderen kullanıcılara karşı koruma için birkaç azaltma sınırlarına sahiptir. Bu davranış, sistem arka uç kaynaklarını aşırı yükleyebilir ve hizmet yanıt hızını düşürebilir. Aşağıdaki sınırlar, müşterilerin kesintileri karşı korumak ve tutarlı hizmet düzeyi sağlamak için tasarlanmıştır. Kullanıcı kısıtlama ve sınırları yalnızca Extreme kullanım senaryosunu etkileyecek şekilde tasarlanmıştır ve tipik kullanım için uygun olmamalıdır.


| Measure | Kullanıcı başına sınır | Açıklama |
|:---|:---|:---|
| Eşzamanlı sorgular | 5 | Kullanıcı için çalıştırılan 5 sorgu varsa, tüm yeni sorgular Kullanıcı başına eşzamanlılık kuyruğuna yerleştirilir. Çalışan sorgulardan biri sona erdiğinde, sonraki sorgu kuyruktan çekilir ve başlatılır. Bu, uyarı kurallarından sorgu içermez.
| Eşzamanlılık kuyruğundaki süre | 2,5 dakika | Bir sorgu, başlatılmadan 2,5 dakikadan uzun bir süre içinde yer alıyorsa, kod 429 ile bir HTTP hatası yanıtıyla sonlandırılır. |
| Eşzamanlılık kuyruğundaki toplam sorgu sayısı | 40 | Kuyruktaki sorguların sayısı 40 ' ye ulaştığında, herhangi bir ek sorgu, HTTP hata kodu 429 ile reddedilir. Bu sayı aynı anda çalışabilecek 5 sorguya ek olarak yapılır. |
| Sorgu hızı | 30 saniye başına 200 sorgu | Bu, sorguların tek bir kullanıcı tarafından tüm çalışma alanlarına gönderilebilmesinin genel hızıdır.  Bu sınır, Azure panoları ve Log Analytics çalışma alanı Özet sayfası gibi görselleştirme parçaları tarafından başlatılan programlı sorgular veya sorgular için geçerlidir. |

- [Azure izleyici 'de günlük sorgularını iyileştirme](../articles/azure-monitor/log-query/query-optimization.md)bölümünde açıklandığı gibi sorgularınızı iyileştirin.
- Panolar ve çalışma kitapları, her yüklendiğinde veya yeniledikleri her seferinde sorgu patlaması oluşturan tek bir görünümde birden çok sorgu içerebilir. Bunları isteğe bağlı olarak yüklenen birden çok görünüme bölmek için göz önünde bulundurun. 
- Power BI ' de, ham Günlükler yerine yalnızca toplanmış sonuçları ayıklamayı göz önünde bulundurun.