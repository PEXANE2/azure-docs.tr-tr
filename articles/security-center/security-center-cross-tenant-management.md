---
title: Azure Güvenlik Merkezi 'nde çapraz Kiracı Yönetimi | Microsoft Docs
description: " Azure Güvenlik Merkezi'nde veri toplamayı etkinleştirmeyi öğrenin. "
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 7d51291a-4b00-4e68-b872-0808b60e6d9c
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2019
ms.author: v-mohabe
ms.openlocfilehash: d6b5b528c3021bfb62bc30ad5910524db36e7e95
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68950544"
---
# <a name="cross-tenant-management-in-security-center"></a>Güvenlik Merkezi 'nde çapraz Kiracı Yönetimi

Çapraz Kiracı Yönetimi, [Azure tarafından atanan kaynak yönetiminden](../lighthouse/concepts/azure-delegated-resource-management.md) yararlanarak Güvenlik Merkezi 'nde birden çok kiracının güvenlik duruşunu görüntülemenize ve yönetmenize olanak sağlar. Her kiracının dizininde oturum açmak zorunda kalmadan, birden çok kiracıyı tek bir görünümden verimli bir şekilde yönetin.

- Hizmet sağlayıcıları, birden çok müşteri için kaynakların güvenlik duruşunu kendi kiracılarının içinden yönetebilir.

- Birden çok kiracıya sahip kuruluşların güvenlik ekipleri, güvenlik duruşunu tek bir konumdan görüntüleyebilir ve yönetebilir.

## <a name="set-up-cross-tenant-management"></a>Çapraz kiracı yönetimini ayarlama

[Azure Temsilcili kaynak yönetimi](../lighthouse/concepts/azure-delegated-resource-management.md)'ni kullanarak kendi kiracınıza yönetilen kiracılar kaynaklarına erişim yetkisi vererek çapraz Kiracı Yönetimi ayarlayın.

> [!NOTE]
> Azure Temsilcili kaynak yönetimi, Azure Use 'ın temel bileşenlerinden biridir.

## <a name="how-does-cross-tenant-management-work-in-security-center"></a>Güvenlik Merkezi 'nde çapraz Kiracı Yönetimi nasıl çalışır?

Birden çok Kiracıdaki abonelikleri, tek bir kiracıda birden çok aboneliği yönettiğiniz şekilde gözden geçirebilir ve yönetebilirsiniz.

Üst menü çubuğundan filtre simgesine tıklayın ve her bir kiracının dizininden görüntülemek istediğiniz abonelikleri seçin.

  ![Kiracıları filtrele](./media/security-center-cross-tenant-management/cross-tenant-filter.png)

Görünümler ve eylemler temelde aynıdır. Bazı örnekler şunlardır:

- **Güvenlik Ilkelerini yönetme**: Tek bir görünümden, [ilkeler](tutorial-security-policy.md)içeren çok sayıda kaynağın güvenlik duruşunu yönetin, güvenlik önerileri ile eylemler gerçekleştirin ve güvenlikle ilgili verileri toplayın ve yönetin.
- **Güvenli puanı ve uyumluluk**duruşunu artırma: Çapraz kiracı görünürlüğü, tüm Kiracılarınızın genel güvenlik duruşunu ve bunların her biri için [güvenli puan](security-center-secure-score.md) ve [Uyumluluk](security-center-compliance-dashboard.md) duruşunu en iyi şekilde nasıl iyileştirebileceğinizi görüntülemenize olanak sağlar.
- **Önerileri düzeltin**: Aynı anda çeşitli kiracılardan çok sayıda kaynağın [önerisini](security-center-recommendations.md) izleyin ve düzeltin. Daha sonra tüm kiracılar genelinde en yüksek riski sunan güvenlik açıklarını hemen ekleyebilirsiniz.
- **Uyarıları Yönet**: Farklı kiracılar genelinde [uyarıları](security-center-alerts-overview.md) tespit edin. Eyleme dönüştürülebilir [Düzeltme adımlarıyla](security-center-managing-and-responding-alerts.md)uyumlu olmayan kaynaklar üzerinde işlem yapın.

- **Gelişmiş bulut savunma özelliklerini ve daha fazlasını yönetin**: [Tam zamanında (JIT) VM erişimi](security-center-just-in-time.md), [Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md), [Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)ve daha fazlası gibi çeşitli tehdit algılama ve koruma hizmetlerini yönetin.
 
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, çapraz kiracı yönetiminin güvenlik merkezi 'nde nasıl çalıştığı açıklanmaktadır. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi ile güvenlik duruşunuzu güçlendirin](security-center-monitoring.md)-Azure kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
