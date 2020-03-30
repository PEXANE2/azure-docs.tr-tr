---
title: Portalı kullanarak eski bir Doğrudan eşlemeyi bir Azure kaynağına dönüştürme
titleSuffix: Azure
description: Portalı kullanarak eski bir Doğrudan eşlemeyi bir Azure kaynağına dönüştürme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 9900414d38bd597d08a80d15e908228c06ce06ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775062"
---
# <a name="convert-a-legacy-direct-peering-to-azure-resource-using-the-portal"></a>Portalı kullanarak eski bir Doğrudan eşlemeyi bir Azure kaynağına dönüştürme

Bu makalede, portalı kullanarak varolan bir eski Doğrudan'ın Azure kaynağına nasıl dönüştürülür.

İsterseniz, [PowerShell](howto-legacy-direct-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [Önkoşulları](prerequisites.md) ve [Doğrudan bakan gözden geçirin.](walkthrough-direct-all.md)


## <a name="convert-legacy-direct-peering-to-azure-resource"></a>Eski doğrudan bakışları Azure kaynağına dönüştürme

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="convert-legacy-direct-peering"></a><a name=create></a>Eski doğrudan eşlemi dönüştürme

**Peering** kaynağını kullanarak eski eşleme bağlantılarını dönüştürebilirsiniz.

#### <a name="launch-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration-legacy.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Doğrudan bakmayı doğrula
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet'e bakan SSS'leri](faqs.md) ziyaret edin

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Doğrudan bir eşleme oluşturun veya değiştirin.](howto-direct-portal.md)
