---
title: include dosyası
description: include dosyası
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 07/22/2019
ms.author: bwren
ms.custom: include file
ms.openlocfilehash: 627b020ce618a2a1f2646a95e143947876bd6a15
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82072646"
---
### <a name="general-query-limits"></a>Genel sorgu sınırları

| Sınır | Açıklama |
|:---|:---|
| Sorgu dili | Azure Monitor, Azure Veri Gezgini ile aynı [Kusto sorgu dilini](/azure/kusto/query/) kullanır. Azure [Monitor'da](../articles/azure-monitor/log-query/data-explorer-difference.md) desteklenmeyen KQL dil öğeleri için Azure Monitor günlük sorgu dil farklılıklarına bakın. |
| Azure bölgeleri | Günlük sorguları, birden çok Azure bölgesinde Günlük Analizi çalışma alanlarına yayılan veriler aşırı ek yükü yaşayabilir. Ayrıntılar için [Sorgu sınırlarına](../articles/azure-monitor/log-query/scope.md#query-limits) bakın. |
| Çapraz kaynak sorguları | 100 ile sınırlı tek bir sorguda maksimum Uygulama Öngörüleri kaynağı ve Log Analytics çalışma alanı sayısı.<br>Kaynak karşıtması View Designer'da desteklenmez.<br>Günlük uyarılarındaki çapraz kaynak sorgusu yeni zamanlananQueryRules API'sinde desteklenir.<br>Ayrıntılar için [kaynak ötesi sorgu sınırlarına](../articles/azure-monitor/log-query/cross-workspace-query.md#cross-resource-query-limits) bakın. |

### <a name="user-query-throttling"></a>Kullanıcı sorgusu azaltma
Azure Monitor'un, aşırı sayıda sorgu gönderen kullanıcılara karşı korumak için birkaç azaltma sınırı vardır. Bu tür davranışlar, sistem arka uç kaynaklarını aşırı yükleyebilir ve hizmet yanıt verme yeteneğini tehlikeye atabilir. Aşağıdaki sınırlar, müşterileri kesintilerden korumak ve tutarlı hizmet düzeyi sağlamak için tasarlanmıştır. Kullanıcı azaltma ve sınırlamaları yalnızca aşırı kullanım senaryosunu etkileyecek şekilde tasarlanmıştır ve tipik kullanımiçin uygun olmamalıdır.


| Measure | Kullanıcı başına limit | Açıklama |
|:---|:---|:---|
| Eşzamanlı sorgular | 5 | Kullanıcı için çalışan 5 sorgu varsa, kullanıcı başına eşzamanlılık sırana yeni sorgular yerleştirilir. Çalışan sorgulardan biri sona erdiğinde, bir sonraki sorgu kuyruktan çıkarılır ve başlatılır. Bu, uyarı kurallarındaki sorguları içermez.
| Eşzamanlılık kuyruğundaki süre | 2,5 dakika | Bir sorgu başlatılmadan 2,5 dakikadan fazla sırada kalırsa, kod 429 ile bir HTTP hata yanıtı ile sonlandırılır. |
| Eşzamanlılık kuyruğundaki toplam sorgular | 40 | Kuyruktaki sorgu sayısı 40'a ulaştığında, ek sorgular BIR HTTP hata kodu 429 ile reddedilir. Bu sayı, aynı anda çalıştırılabilen 5 sorguya ek olarak verilir. |
| Sorgu oranı | 30 saniyede 200 sorgu | Bu, sorguların tek bir kullanıcı tarafından tüm çalışma alanlarına gönderilebildiği genel orandır.  Bu sınır, Azure panoları ve Log Analytics çalışma alanı özet sayfası gibi görselleştirme bölümleri tarafından başlatılan programlı sorgular veya sorgular için geçerlidir. |

- [Azure Monitor'da En İyi Duruma Getir günlük sorgularında](../articles/azure-monitor/log-query/query-optimization.md)açıklandığı gibi sorgularınızı optimize edin.
- Panolar ve çalışma kitapları, her yüklediklerinde veya yenilediklerinde bir sorgu patlaması oluşturan tek bir görünümde birden çok sorgu içerebilir. Onları isteğe bağlı olarak yüklenen birden çok görünüme bölmeyi düşünün. 
- Power BI'de, ham günlükler yerine yalnızca toplu sonuçları ayıklamayı düşünün.