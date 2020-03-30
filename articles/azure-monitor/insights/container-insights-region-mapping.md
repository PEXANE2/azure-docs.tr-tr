---
title: Kapsayıcılar bölge eşlemeleri için Azure Monitörü
description: Bu makalede, kapsayıcılar için Azure Monitor arasında desteklenen bölge eşlemeleri, Günlük Analizi Çalışma Alanı ve özel ölçümler açıklanmaktadır.
ms.topic: conceptual
ms.date: 06/26/2019
ms.openlocfilehash: a058f9cac987bb5c7130019f50370c6a176b09ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75403416"
---
# <a name="region-mappings-supported-by-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Monitor tarafından desteklenen bölge eşlemeleri

 Kapsayıcılar için Azure Monitor'u etkinleştirirken, bir Log Analytics çalışma alanı ve AKS kümesini bağlamak ve Azure Monitor'a gönderilen özel ölçümleri toplamak için yalnızca belirli bölgeler desteklenir.

## <a name="log-analytics-workspace-supported-mappings"></a>Log Analytics çalışma alanı desteklenen eşlemeler

AKS küme kaynakları veya Log Analytics çalışma alanı diğer bölgelerde bulunabilir ve aşağıdaki tablo eşlemelerimizi gösterir.

|**AKS Küme bölgesi** | **Günlük Analitik Çalışma Alanı bölgesi** |
|-----------------------|------------------------------------|
|**Afrika** | |
|GüneyAfrikaKuzey |WestEurope |
|GüneyAfrikaBatı |WestEurope |
|**Avustralya** | |
|AvustralyaDoğu |AvustralyaDoğu |
|AvustralyaMerkez |AvustralyaMerkez |
|AvustralyaMerkez2 |AvustralyaMerkez |
|AvustralyaDoğu |AvustralyaDoğu |
|**Asya Pasifik** | |
|Doğuasya |Doğuasya |
|Güneydoğu Asya |Güneydoğu Asya |
|**Brezilya** | |
|BrazilSouth | GüneyCentralUS |
|**Kanada** ||
|KanadaMerkez |KanadaMerkez |
|KanadaDoğu |KanadaMerkez |
|**Avrupa** | |
|FransaMerkez |FransaMerkez |
|FransaGüney |FransaMerkez |
|Kuzey Avrupa |Kuzey Avrupa |
|UkSouth |UkSouth |
|UkWest |UkSouth |
|WestEurope |WestEurope |
|**Hindistan** | |
|Orta Hindistan |Orta Hindistan |
|Güney Hindistan |Orta Hindistan |
|Batı Hindistan |Orta Hindistan |
|**Japonya** | |
|JapanDoğu |JapanDoğu |
|JapanWest |JapanDoğu |
|**Güney Kore** | |
|KoreMerkez |KoreMerkez |
|KoreaSouth |KoreMerkez |
|**ABD** | |
|CentralUS |CentralUS|
|DoğuUS |DoğuUS |
|DoğuUS2 |DoğuUS2 |
|WestUS |WestUS |
|BatıUS2 |BatıUS2 |
|WestCentralUS<sup>1</sup>|DoğuUS<sup>1</sup>|
|US Gov Virginia |US Gov Virginia |

<sup>1</sup> Kapasite kısıtlamaları nedeniyle, yeni kaynaklar oluşturulurken bölge kullanılamaz. Buna Bir Log Analytics çalışma alanı dahildir. Ancak, bölgede önceden var olan bağlantılı kaynaklar çalışmaya devam etmelidir.

## <a name="custom-metrics-supported-regions"></a>Özel ölçümler desteklenen bölgeler

Azure Kubernetes Services (AKS) kümeleri düğümleri ve bölmelerinden ölçümler toplama, yalnızca aşağıdaki [Azure bölgelerinde](../platform/metrics-custom-overview.md#supported-regions)özel ölçümler olarak yayımlanması için desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

AKS kümenizi izlemeye başlamak için, denetimi etkinleştirmek için gereksinimleri ve kullanılabilir yöntemleri anlamak [için kapsayıcılar için Azure Monitörünü nasıl etkinleştirin](container-insights-onboard.md) irdeleyin.  
