---
title: Azure portalını kullanarak eski doğrudan bir Azure kaynağına doğrudan bakış dönüştürme
titleSuffix: Azure
description: Azure portalını kullanarak eski doğrudan bir Azure kaynağına doğrudan bakış dönüştürme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: e49e4d5debe63b99039bbafbc14f7788367314f3
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678843"
---
# <a name="convert-a-legacy-direct-peering-to-an-azure-resource-by-using-the-azure-portal"></a>Azure portalını kullanarak eski doğrudan bir Azure kaynağına doğrudan bakış dönüştürme

Bu makalede, Azure portalını kullanarak varolan bir eski Doğrudan'ın Azure kaynağına nasıl dönüştürülür.

İsterseniz, [PowerShell](howto-legacy-direct-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [ön koşulları](prerequisites.md) ve Doğrudan bakan [gözden geçirin.](walkthrough-direct-all.md)


## <a name="convert-a-legacy-direct-peering-to-an-azure-resource"></a>Eski bir Azure kaynağına doğrudan bakış dönüştürme

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="convert-a-legacy-direct-peering"></a><a name=create></a>Eskiye doğrudan bakmayı dönüştürme

**Eşleme** kaynağını kullanarak eski eşleme bağlantılarını dönüştürebilirsiniz.

#### <a name="launch-the-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırır
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration-legacy.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Doğrudan bakmayı doğrula
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet'e bakan SSS'lere](faqs.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Doğrudan bir bakış oluşturma veya değiştirme](howto-direct-portal.md)
