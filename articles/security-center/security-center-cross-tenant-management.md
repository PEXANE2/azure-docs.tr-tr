---
title: Azure Güvenlik Merkezi'nde kiracı lar arası yönetim | Microsoft Dokümanlar
description: Azure temsilci kaynak yönetimini kullanarak Güvenlik Merkezi'ndeki birden çok kiracının güvenlik duruşunu yönetmek için kiracılar arası yönetimi nasıl ayarlayayın öğrenin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 7d51291a-4b00-4e68-b872-0808b60e6d9c
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2019
ms.author: memildin
ms.openlocfilehash: 2aeb2ab4cfb4ed5e8652638aaced320cc7119d3e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919490"
---
# <a name="cross-tenant-management-in-security-center"></a>Güvenlik Merkezi'nde kiracı karşıyönetimi

Kiracılar arası yönetim, [Azure temsilci kaynak yönetiminden](../lighthouse/concepts/azure-delegated-resource-management.md) yararlanarak Güvenlik Merkezi'ndeki birden çok kiracının güvenlik duruşunu görüntülemenizi ve yönetmeniz sağlar. Her kiracının dizininde oturum açmadan birden çok kiracıyı tek bir görünümden verimli bir şekilde yönetin.

- Hizmet sağlayıcılar, birden çok müşteri için, kendi kiracıları içinden kaynakların güvenlik duruşunu yönetebilir.

- Birden çok kiracısı olan kuruluşların güvenlik ekipleri güvenlik duruşlarını tek bir konumdan görüntüleyebilir ve yönetebilir.

## <a name="set-up-cross-tenant-management"></a>Kiracılar arası yönetimi ayarlama

[Azure temsilci kaynak yönetimini](../lighthouse/concepts/azure-delegated-resource-management.md)kullanarak yönetilen kiracıların kaynaklarına erişimi kendi kiracınıza devrederek çapraz kiracı yönetimini ayarlayın.

> [!NOTE]
> Azure temsilci kaynak yönetimi, Azure Deniz Feneri'nin önemli bileşenlerinden biridir.

## <a name="how-does-cross-tenant-management-work-in-security-center"></a>Güvenlik Merkezi'nde kiracı lar arası yönetim nasıl çalışır?

Birden çok kiracıdaki abonelikleri, birden çok aboneliği tek bir kiracıda yönettiğiniz şekilde gözden geçirebilir ve yönetebilirsiniz.

Üst menü çubuğundan filtre simgesini tıklatın ve her kiracının dizininden görüntülemek istediğiniz abonelikleri seçin.

  ![Filtre kiracıları](./media/security-center-cross-tenant-management/cross-tenant-filter.png)

Görüşler ve eylemler temelde aynıdır. İşte bazı örnekler:

- **Güvenlik ilkelerini yönetin**: Tek bir bakış açısıyla, [politikalarla](tutorial-security-policy.md)birçok kaynağın güvenlik duruşunu yönetin, güvenlik önerileriyle eylemde bulunun ve güvenlikle ilgili verileri toplayın ve yönetin.
- **Güvenli Puanı ve uyumluluk duruşunu geliştirin**: Kiracı ların çapraz görünürlüğü, tüm kiracılarınızın genel güvenlik duruşunu ve her biri için [Güvenli Skor](security-center-secure-score.md) ve [uyumluluk duruşunu](security-center-compliance-dashboard.md) en iyi şekilde nerede ve nasıl iyileştireceğiniz konusunda sizi görüntülemenize olanak tanır.
- **Düzeltici öneriler**: Aynı anda çeşitli kiracılardan gelen birçok kaynak için bir [tavsiyeyi](security-center-recommendations.md) izleyin ve düzeltin. Daha sonra, tüm kiracılar arasında en yüksek riski sunan güvenlik açıklarını hemen giderebilirsiniz.
- **Uyarıları Yönet**: Farklı kiracılardaki [uyarıları](security-center-alerts-overview.md) algıla. Eyleme geçirilebilir [düzeltme adımlarıyla](security-center-managing-and-responding-alerts.md)uyumlu olmayan kaynaklar üzerinde işlem gerçekleştirin.

- **Gelişmiş bulut savunma özelliklerini ve daha fazlasını yönetin**: [Tam zamanında (JIT) VM erişimi,](security-center-just-in-time.md) [Adaptif Ağ Sertleştirme,](security-center-adaptive-network-hardening.md) [uyarlanabilir uygulama denetimleri](security-center-adaptive-application.md)ve daha fazlası gibi çeşitli tehdit koruma hizmetlerini yönetin.
 
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nde kiracı çapraz yönetimin nasıl çalıştığı açıklanmaktadır. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi ile güvenlik duruşunuzu güçlendirin](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.
* [Azure Güvenlik Merkezi SSSS](faq-general.md) - Hizmeti kullanma yla ilgili sık sorulan soruları bulun.
* [Kurumsal senaryolarda Azure Deniz Feneri hakkında bilgi edinin](https://docs.microsoft.com/azure/lighthouse/concepts/enterprise) - Azure Deniz Feneri'nin birden çok Azure AD kiracısı kullanan bir kuruluşta kiracılar arası yönetimi nasıl basitleştirebileceğini keşfedin.