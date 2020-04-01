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
ms.openlocfilehash: 8e44ce594375deeac47f037515d96c57d15c8359
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398397"
---
# <a name="manage-and-respond-to-security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama

Bu konu, kaynaklarınızı korumak için aldığınız uyarıları nasıl görüntüleyip işleyebilirsiniz gösterir. 

* Farklı uyarı türleri hakkında bilgi edinmek için [Güvenlik uyarı türlerine](alerts-reference.md)bakın.
* Güvenlik Merkezi'nin uyarıları nasıl oluşturduğuna genel bir bakış için Azure [Güvenlik Merkezi'nin tehditleri nasıl algılayıp yanıtladığına](security-center-alerts-overview.md)bakın.

> [!NOTE]
> Gelişmiş algılamaları etkinleştirmek için Azure Güvenlik Merkezi Standart sürümüne yükseltme yapın. Ücretsiz deneme sürümü mevcuttur. Yükseltmek için [Güvenlik İlkesi](tutorial-security-policy.md)'nde Fiyatlandırma Katmanı’nı seçin. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi fiyatlandırması](security-center-pricing.md).

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?
Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.

> [!NOTE]
> Güvenlik Merkezi algılama özelliklerinin nasıl çalıştığı hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nin tehditleri nasıl algılayıp yanıtlataolduğunu](security-center-alerts-overview.md#detect-threats)öğrenin.

## <a name="manage-your-security-alerts"></a>Güvenlik uyarılarınızı yönetme

1. Güvenlik Merkezi panosundan, uyarıları görüntülemek ve uyarılara genel bakış için **Tehdit koruma** döşemesine bakın.

    ![Güvenlik Merkezi'nde güvenlik uyarıları kutucuğu](./media/security-center-managing-and-responding-alerts/security-center-dashboard-alert.png)

1. Uyarılar hakkında daha fazla ayrıntı görmek için döşemeyi tıklatın.

   ![Güvenlik Merkezi'nde Güvenlik uyarıları](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. Gösterilen uyarıları filtrelemek için **Filtre'yi**ve açılan **Filtre** bıçağından uygulamak istediğiniz filtre seçeneklerini seçin. Liste, seçili filtreye göre güncellenir. Filtreleme çok yararlı olabilir. Örneğin, sistemde olası bir ihlali araştırdığınız için son 24 saatte oluşan güvenlik uyarılarını ele almak isteyebilirsiniz.

    ![Güvenlik Merkezi'nde uyarıları filtreleme](./media/security-center-managing-and-responding-alerts/security-center-filter-alerts.png)

## <a name="respond-to-security-alerts"></a>Güvenlik uyarılarını yanıtlama

1. Güvenlik **uyarıları** listesinden bir güvenlik uyarısı tıklatın. Bir saldırıyı düzeltmek için ilgili kaynaklar ve atmanız gereken adımlar gösterilir.

    ![Güvenlik uyarılarını yanıtlama](./media/security-center-managing-and-responding-alerts/security-center-alert.png)

1. Bilgileri gözden geçirdikten sonra saldırıya uğrayan bir kaynağı tıklatın.

    ![Güvenlik uyarıları hakkında ne yap›lac›lar›na ilişkin öneriler](./media/security-center-managing-and-responding-alerts/security-center-alert-remediate.png)

    **Genel Bilgiler** bölümü, güvenlik uyarısını neyin tetiklediğine dair bir fikir verebilir. Uyarı hala etkinse hedef kaynak, kaynak IP adresi (varsa) gibi bilgileri ve nasıl düzeltileceklerine ilişkin önerileri görüntüler.  

    > [!NOTE]
    >Bazı durumlarda, kaynak IP adresi kullanılamıyor, bazı Windows güvenlik olayları günlükleri IP adresini içermez.

1. Güvenlik Merkezi tarafından önerilen düzeltme adımları, güvenlik uyarısına göre farklılık gösterir. Her uyarı için onları takip edin. 

    Bazı durumlarda, bir güvenlik uyarısı azaltmak için, önerilen düzeltmeyi uygulamak için diğer Azure denetimlerini veya hizmetlerini kullanmanız gerekebilir. 

## <a name="see-also"></a>Ayrıca bkz.

Bu belgede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Tehditleri izlemek ve bu tehditlere yanıt vermek için Azure Güvenlik Merkezi'nin uyarı özelliklerini nasıl kullanacağınızı anlatan Microsoft Learn modülü](https://docs.microsoft.com/learn/modules/resolve-threats-with-azure-security-center/)
* [Azure Güvenlik Merkezi'nde güvenlik uyarıları.](security-center-alerts-overview.md)
* [Güvenlik olaylarını işleme](security-center-incident.md)