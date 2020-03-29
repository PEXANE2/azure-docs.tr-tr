---
title: Azure Güvenlik Merkezi tehdit zekası raporu | Microsoft Docs
description: Bu belge, bir araştırma sırasında güvenlik uyarıları hakkında daha fazla bilgiye ulaşmak için Azure Güvenlik Merkezi Tehdit Zekası Raporlarını kullanmanıza yardımcı olur.
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
ms.date: 11/28/2018
ms.author: memildin
ms.openlocfilehash: f8b4063d87fa9a89dccd42eddea644609bd6ff27
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921258"
---
# <a name="azure-security-center-threat-intelligence-report"></a>Azure Güvenlik Merkezi Tehdit Zekası Raporu
Bu belge, Azure Güvenlik Merkezi Tehdit Zekası Raporlarının, güvenlik uyarılarını oluşturan tehditler hakkında daha fazla bilgi edinmenize nasıl yardımcı olabileceğini açıklamaktadır.

## <a name="what-is-a-threat-intelligence-report"></a>Tehdit zekası rapor nedir?
Güvenlik Merkezi tehdit koruması, Azure kaynaklarınızdan, ağınızdan ve bağlı iş ortağı çözümlerinizden güvenlik bilgilerini izleyerek çalışır. Tehditleri belirlemek amacıyla bu bilgileri genellikle birden fazla kaynaktan bilgileri ilişkilendirerek analiz eder. Daha fazla bilgi için [Azure Güvenlik Merkezi'nin tehditleri nasıl algılayıp yanıtlataolduğunu](security-center-alerts-overview.md#detect-threats)öğrenin.

Güvenlik Merkezi tarafından bir tehdit algılandığında, belirli bir olay için düzeltme önerileri de dahil olmak üzere ayrıntılı bilgiler içeren bir [güvenlik uyarısı](security-center-managing-and-responding-alerts.md) tetikler. Olay müdahale ekiplerine yardımcı olmak, tehditleri araştırmak ve düzeltmek için, Güvenlik Merkezi, tespit edilen tehdit le ilgili bilgileri içeren bir tehdit istihbarat raporu içerir:

* Saldırganların kimliği veya bağlantıları (bu konuda bilgi varsa)
* Saldırganların hedefleri
* Güncel ve geçmiş saldırı kampanyaları (bu konuda bilgi varsa)
* Saldırganların taktikleri, araçları ve prosedürleri
* URL’ler ve dosya karmaları gibi ilgili tehlike göstergeleri (IoC)
* Azure kaynaklarınızın risk altında olup olmadığını belirlemenize yardımcı olmak için sektörel ve coğrafi yaygınlık bilgisi olan mağdur bilimi
* Azaltma ve düzeltme bilgileri

> [!NOTE]
> Raporlardaki bilgi düzeyi değişiklik gösterir. Ayrıntı düzeyi kötü amaçlı yazılımın etkinliğine ve yaygınlığına bağlıdır.
>
>

Güvenlik Merkezi’nde, saldırıya göre değişiklik gösteren üç tehdit raporu türü vardır. Raporlar şunlardır:

* **Etkinlik Grubu Raporu**: Saldırganlar, amaçları ve taktikleri hakkında ayrıntılı bilgi sunar.
* **Kampanya Raporu**: Belirli saldırı kampanyaların ayrıntılarına odaklanır.
* **Tehdit Özeti Raporu**: Yukarıdaki iki raporun tüm maddelerini kapsar.

Bu tür bilgiler, saldırının kaynağını, saldırganın motivasyonlarını ve bu sorunu hafifletmek için ne yapması gerektiğini anlamak için devam eden bir soruşturmanın olduğu olay yanıt süreci sırasında yararlıdır.

## <a name="how-to-access-the-threat-intelligence-report"></a>Tehdit zekası raporuna nasıl erişebilirim?
**Güvenlik uyarıları** kutucuğuna bakarak mevcut uyarılarınızı gözden geçirebilirsiniz. Azure portalını açın ve her uyarı hakkında daha fazla ayrıntı görmek için aşağıdaki adımları izleyin:

1. Güvenlik Merkezi panosunda **Güvenlik uyarıları** kutucuğunu görürsünüz.
2. Kutucuğa tıklayarak uyarılar hakkında daha fazla bilginin yer aldığı **Güvenlik uyarıları** dikey penceresini açın ve daha fazla bilgi almak istediğiniz güvenlik uyarısını seçin.

    ![Güvenlik uyarıları](./media/security-center-threat-report/security-center-threat-report-fig1.png)
3. Bu durumda, **Şüpheli işlem çalıştırılan** bıçak aşağıdaki şekilde gösterildiği gibi uyarı ile ilgili ayrıntıları gösterir:

    ![Güvenlik uyarısı ayrıntıları](./media/security-center-threat-report/security-center-threat-report-fig2.png)
4. Güvenlik uyarılarındaki bilgi miktarı, uyarının türüne göre değişiklik gösterir. **RAPORLAR** alanında, tehdit istihbarat raporuyla bir bağlantınız var. Bağlantıya tıkladığınızda PDF dosyası yeni bir tarayıcı penceresinde açılacaktır.

   ![Storage seçimi](./media/security-center-threat-report/security-center-threat-report-fig3.png)

Buradan raporun PDF dosyasını indirebilir, algılanan güvenlik sorunu hakkında daha fazla bilgi edinebilir ve verilen bilgilere göre işlem yapabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede Azure Güvenlik Merkezi Tehdit Zekası Raporlarının güvenlik uyarısı araştırmaları sırasında nasıl yardımcı olabileceğini öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi planlama ve işlemleri kılavuzu.](security-center-planning-and-operations-guide.md) Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtverme.](security-center-managing-and-responding-alerts.md) Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde Güvenlik Olayını İşleme](security-center-incident.md)