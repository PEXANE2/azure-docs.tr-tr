---
title: Azure Güvenlik Merkezi tehdit zekası raporu | Microsoft Docs
description: Bu sayfa, güvenlik uyarıları hakkında daha fazla bilgi edinmek için araştırma sırasında Azure Güvenlik Merkezi tehdit bilgileri raporlarını kullanmanıza yardımcı olur
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5662e312-e8c2-4736-974e-576eeb333484
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2020
ms.author: memildin
ms.openlocfilehash: a4fdbab4a69fac1376779f37d5fa69fef587bf52
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84888210"
---
# <a name="azure-security-center-threat-intelligence-report"></a>Azure Güvenlik Merkezi tehdit zekası raporu

Bu sayfada, Azure Güvenlik Merkezi 'nin tehdit bilgileri raporlarının güvenlik uyarısı tetikleyen bir tehdit hakkında daha fazla bilgi edinmenize nasıl yardımcı olduğu açıklanmaktadır.


## <a name="what-is-a-threat-intelligence-report"></a>Tehdit zekası rapor nedir?

Güvenlik Merkezi tehdit koruması, Azure kaynaklarınızdan, ağınızdan ve bağlı iş ortağı çözümlerinden güvenlik bilgilerini izleyerek işe yarar. Tehditleri belirlemek amacıyla bu bilgileri genellikle birden fazla kaynaktan bilgileri ilişkilendirerek analiz eder. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'nin tehditleri nasıl algıladığı ve yanıt verdiği](security-center-alerts-overview.md#detect-threats).

Güvenlik Merkezi bir tehdidi tanımlarsa, düzeltme önerileri de dahil olmak üzere olayla ilgili ayrıntılı bilgileri içeren bir [güvenlik uyarısını](security-center-managing-and-responding-alerts.md)tetikler. Güvenlik Merkezi, olay yanıt ekiplerinin tehditleri araştırıp düzeltmesini sağlamak için algılanan tehditler hakkında bilgi içeren tehdit bilgileri raporları sağlar. Rapor aşağıdakiler gibi bilgiler içerir:

* Saldırganların kimliği veya bağlantıları (bu konuda bilgi varsa)
* Saldırganların hedefleri
* Güncel ve geçmiş saldırı kampanyaları (bu konuda bilgi varsa)
* Saldırganlar, Araçlar ve yordamlar
* URL’ler ve dosya karmaları gibi ilgili tehlike göstergeleri (IoC)
* Azure kaynaklarınızın risk altında olup olmadığını belirlemenize yardımcı olmak için sektörel ve coğrafi yaygınlık bilgisi olan mağdur bilimi
* Azaltma ve düzeltme bilgileri

> [!NOTE]
> Raporlardaki bilgi düzeyi değişiklik gösterir. Ayrıntı düzeyi kötü amaçlı yazılımın etkinliğine ve yaygınlığına bağlıdır.

Güvenlik Merkezi’nde, saldırıya göre değişiklik gösteren üç tehdit raporu türü vardır. Raporlar şunlardır:

* **Etkinlik grubu raporu**: saldırganlar, amaçları ve tackleri için derin öngörüleri sağlar.
* **Kampanya Raporu**: Belirli saldırı kampanyaların ayrıntılarına odaklanır.
* **Tehdit Özeti Raporu**: Yukarıdaki iki raporun tüm maddelerini kapsar.

Bu tür bilgiler, saldırı kaynağını, saldırganın yaptığı ilerlemeyi ve gelecekte bu sorunu azaltmak için ne yapılacağını anlamak üzere devam eden bir araştırma olan olay yanıtı işlemi sırasında yararlıdır.



## <a name="how-to-access-the-threat-intelligence-report"></a>Tehdit zekası raporuna nasıl erişebilirim?

1. Güvenlik Merkezi 'nin kenar çubuğundan **güvenlik uyarıları** sayfasını açın.
1. Bir uyarı seçin. 
    Uyarı ayrıntıları sayfası, uyarı hakkında daha fazla ayrıntı ile açılır. Aşağıda, **fidye yazılımı göstergeleri** Uyarı ayrıntıları sayfası algılandı.

    [![Fidye göstergeleri, uyarı ayrıntıları sayfası algıladı](media/security-center-threat-report/ransomware-indicators-detected-link-to-threat-intel-report.png)](media/security-center-threat-report/ransomware-indicators-detected-link-to-threat-intel-report.png#lightbox)

1. Raporun bağlantısını seçin ve bir PDF varsayılan tarayıcınızda açılır.

    [![Güvensiz olabilecek eylem uyarısı ayrıntıları sayfası](media/security-center-threat-report/threat-intelligence-report.png)](media/security-center-threat-report/threat-intelligence-report.png#lightbox)

    PDF raporunu isteğe bağlı olarak indirebilirsiniz. 

    >[!TIP]
    > Güvenlik uyarılarındaki bilgi miktarı, uyarının türüne göre değişiklik gösterir.



## <a name="next-steps"></a>Sonraki adımlar

Bu sayfada güvenlik uyarılarını araştırırken tehdit bilgileri raporlarının nasıl açılacağı açıklanmıştır. İlgili bilgiler için, aşağıdaki sayfalara bakın:

* [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md). Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'nde güvenlik olaylarını işleme](security-center-incident.md)