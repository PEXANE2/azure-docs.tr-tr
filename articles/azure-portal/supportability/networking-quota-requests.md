---
title: Ağ sınırı artışı | Microsoft Dokümanlar
description: Ağ sınırını artırma
author: anavinahar
ms.author: anavin
ms.date: 01/23/2020
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9b5c7043b06172c2d4931ca1c3fd3ac5d0e80883
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76547814"
---
# <a name="networking-limit-increase"></a>Ağ sınırını artırma

Ağ kotanızı artırmak için [Azure portalını](https://portal.azure.com) kullanın.

Geçerli Ağ kullanımınızı ve kotanızı Azure portalında görüntülemek için aboneliğinizi açın ve ardından **Kullanımlar + kotalar'ı**seçin. Ağ kullanımınızı ve sınırlarınızı görüntülemek için aşağıdaki seçenekleri de kullanabilirsiniz.

* [Kullanım CLI](/cli/azure/network#az-network-list-usages)
* [Powershell](/powershell/module/azurerm.network/get-azurermnetworkusage)
* [Ağ kullanımı API'si](/rest/api/virtualnetwork/virtualnetworks/listusage)

**Yardım + destek** kullanarak veya portaldaki **Kullanımlar + kotalar** kullanarak artış talep edebilirsiniz.

> [!Note]
> **Genel IP Önekleri**varsayılan boyutunu değiştirmek için açılır listeden **Min Public IP InterNetwork Önek Uzunluğu'nu** seçin.

## <a name="request-networking-quota-increase-at-subscription-level-using-help--support"></a>Yardım + destek kullanarak abonelik düzeyinde Ağ kotası artışı isteğinde bulunun

Azure portalında **Yardım + destek** kullanarak bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin.

1. [Azure portalında](https://portal.azure.com)oturum açın ve ardından Azure portalı menüsünden **Yardım + destek'i** seçin veya Yardım **+ destek'i**arayın ve seçin.

    ![Yardım + Destek](./media/networking-quota-request/help-plus-support.png)

1. **Yeni destek isteği**’ni seçin.

    ![Yeni destek isteği](./media/networking-quota-request/new-support-request.png)

1. **Sorun türü için**Hizmet ve abonelik **limitlerini (kotalar)** seçin.

    ![Sorun türü açılır tarihinden abonelik sınırlarını seçme](./media/networking-quota-request/select-quota-issue-type.png)

1. Kotasını artırmanız gereken aboneliği seçin.

    ![Abonelik yeniSR'i seçin](./media/networking-quota-request/select-subscription-support-request.png)

1. **Kota türü altında,** **Ağ'ı**seçin. **Sonraki seçin: Çözümler**.

    ![Kota türünü seçin](./media/networking-quota-request/select-quota-type-network.png)

1. **SORUN BİLGİlerİ'nde,** **ayrıntıları bildir'i** seçin ve isteğinizi işleme yardımcı olmak için ek bilgiler doldurun.

    ![Ayrıntıları sağlayın](./media/networking-quota-request/provide-details-link.png)

1. Kota **ayrıntıları** panelinde, isteğinize dahil etmek için bir dağıtım modeli, bir konum ve kaynakları seçin.

    ![Kota Detayları DM](./media/networking-quota-request/quota-details-network.png)

1. Abonelikte istediğiniz yeni sınırları girin. Bir satırı kaldırmak için **Kaynak** menüsünden kaynağı seçin veya "x" simgesini atın'ı seçin. Her kaynak için kotayı girdikten sonra **Kaydet'i** seçin ve destek isteği oluşturmayla devam edin.

    ![Yeni Sınırlar](./media/networking-quota-request/network-new-limits.png)

## <a name="request-networking-quota-increase-at-subscription-level-using-usages--quotas"></a>Kullanımlar + kotalar kullanarak abonelik düzeyinde Ağ kotası artışı isteğinde bulunun

Azure portalında **Kullanım + kotasını** kullanarak bir destek isteği oluşturmak için bu yönergeleri izleyin.

1. From, https://portal.azure.comarama ve **Abonelikleri**seçin.

    ![Abonelikler](./media/networking-quota-request/search-for-suscriptions.png)

1. Kotasını artırmanız gereken aboneliği seçin.

    ![Abonelik seçme](./media/networking-quota-request/select-subscription-change-quota.png)

1. **Kullanım + kotaları** seçin

    ![Kullanımı ve kotaları seçin](./media/networking-quota-request/select-usage-plus-quotas.png)

1. Sağ üst **köşede, Artış İste'yi**seçin.

    ![İstek artışı](./media/networking-quota-request/request-increase-from-subscription.png)

1. [Abonelik düzeyinde Ağ İsteği kotası artışında](#request-networking-quota-increase-at-subscription-level-using-help--support)3.

## <a name="about-networking-limits"></a>Ağ sınırları hakkında

Ağ ağı sınırları hakkında daha fazla bilgi edinmek için, sınırlar sayfasının [Ağ bölümüne](../../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits) veya Ağ Sınırları SSS'mize bakın.
