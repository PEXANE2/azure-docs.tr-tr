---
title: Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme | Microsoft Docs
description: Bu belge, güvenlik uyarılarını yönetmek ve yanıtlamak için Azure Güvenlik Merkezi işlevlerini kullanmanıza yardımcı olur.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/15/2020
ms.author: memildin
ms.openlocfilehash: f8b09c71e9ad55528788f97fb986606f21e8b0ec
ms.sourcegitcommit: 99d016949595c818fdee920754618d22ffa1cd49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769802"
---
# <a name="manage-and-respond-to-security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama

Bu konu başlığı altında, kaynaklarınızı korumak için aldığınız uyarıların nasıl görüntüleneceği ve işlenmesi gösterilmektedir. 

* Farklı Uyarı türleri hakkında bilgi edinmek için bkz. [güvenlik uyarısı türleri](alerts-reference.md).
* Güvenlik Merkezi 'nin nasıl uyarı üretdiğine ilişkin genel bakış için bkz. [Azure Güvenlik Merkezi 'nin tehditleri nasıl algıladığı ve yanıt verdiği](security-center-alerts-overview.md).

> [!NOTE]
> Gelişmiş algılamaları etkinleştirmek için Azure Güvenlik Merkezi Standart sürümüne yükseltme yapın. Ücretsiz deneme sürümü mevcuttur. Yükseltmek için [Güvenlik İlkesi](tutorial-security-policy.md)'nde Fiyatlandırma Katmanı’nı seçin. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi fiyatlandırması](security-center-pricing.md).

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?
Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.

> [!NOTE]
> Güvenlik Merkezi algılama yeteneklerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'nin tehditleri nasıl algıladığı ve yanıt verdiği](security-center-alerts-overview.md#detect-threats).

## <a name="manage-your-security-alerts"></a>Güvenlik uyarılarınızı yönetme

1. Güvenlik Merkezi panosunda, uyarıları görüntülemek ve genel bakış için **tehdit koruması** kutucuğuna bakın.

    ![Güvenlik Merkezi'nde güvenlik uyarıları kutucuğu](./media/security-center-managing-and-responding-alerts/security-center-dashboard-alert.png)

1. Uyarılar hakkında daha fazla ayrıntı görmek için kutucuğa tıklayın.

   ![Güvenlik Merkezi'nde Güvenlik uyarıları](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. Gösterilen uyarıları filtrelemek için **filtre**' ye tıklayın ve açılan **filtre** dikey penceresinde, uygulamak istediğiniz filtre seçeneklerini belirleyin. Liste seçili filtreye göre güncelleştirilir. Filtreleme çok faydalı olabilir. Örneğin, sistemde olası bir ihlali araştırdığınız için son 24 saatte oluşan güvenlik uyarılarını ele almak isteyebilirsiniz.

    ![Güvenlik Merkezi'nde uyarıları filtreleme](./media/security-center-managing-and-responding-alerts/security-center-filter-alerts.png)

## <a name="respond-to-security-alerts"></a>Güvenlik uyarılarını yanıtlama

1. **Güvenlik uyarıları** listesinden bir güvenlik uyarısı ' na tıklayın. Bir saldırının düzeltilmesi için gerçekleştirmeniz gereken kaynaklar ve ilgili adımlar gösterilir.

    ![Güvenlik uyarılarını yanıtlama](./media/security-center-managing-and-responding-alerts/security-center-alert.png)

1. Bilgileri inceledikten sonra saldırıya uğrayan bir kaynağa tıklayın.

    Güvenlik Uyarısı sayfasının sol bölmesi, güvenlik uyarısıyla ilgili üst düzey bilgileri gösterir: Başlık, önem derecesi, durum, etkinlik süresi, şüpheli etkinliğin açıklaması ve etkilenen kaynak. Etkilenen kaynağın yanı sıra kaynakla ilgili Azure etiketlerdir. Uyarıyı araştırırken kaynağın kuruluş bağlamını anlamak için bunları kullanın.

    Sağ bölmede, sorunu araştırmanıza yardımcı olmak üzere uyarının daha ayrıntılı ayrıntılarını içeren **Uyarı ayrıntıları** sekmesi bulunur: IP adresleri, dosyalar, süreçler ve daha fazlası.
     
    ![Güvenlik uyarıları hakkında ne yapacaklarıyla ilgili öneriler](./media/security-center-managing-and-responding-alerts/security-center-alert-remediate.png)

    Sağ bölmedeki **eylem al** sekmesi de bulunur. güvenlik uyarısıyla ilgili daha fazla eylem gerçekleştirmek için bu sekmeyi kullanın. Şöyle eylemler:
    - *Tehdidi azaltma* -bu güvenlik uyarısı için el ile düzeltme adımları sağlar
    - *Gelecekteki saldırıları önleme* -saldırı yüzeyini azaltmaya, güvenlik duruşunu artırmaya ve bu nedenle gelecekteki saldırıları önlemeye yardımcı olmak için güvenlik önerileri sağlar
    - *Otomatik yanıtı Tetikle* -bu güvenlik uyarısı için bir mantıksal uygulamayı bir yanıt olarak tetikleme seçeneği sağlar
    - *Benzer Uyarıları Gizle* -uyarı kuruluşunuza uygun değilse, benzer özelliklerle gelecekteki uyarıları gösterme seçeneği sağlar

    ![Eylem al sekmesi](./media/security-center-managing-and-responding-alerts/alert-take-action.png)




## <a name="see-also"></a>Ayrıca bkz.

Bu belgede, güvenlik uyarılarını görüntülemeyi öğrendiniz. İlgili malzemeler için aşağıdaki sayfalara bakın:

- [Uyarı gizleme kurallarını yapılandırma](alerts-suppression-rules.md)
- [İş akışı otomasyonu ile uyarı ve önerilerin yanıtlarını otomatikleştirin](workflow-automation.md)
