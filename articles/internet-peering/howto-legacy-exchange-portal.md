---
title: Azure portalını kullanarak eski bir Exchange'i Azure kaynağına dönüştürün
titleSuffix: Azure
description: Azure portalını kullanarak eski bir Exchange'i Azure kaynağına dönüştürün
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 87a7a6bca608f1748d3b659eabdc3e941b537377
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678579"
---
# <a name="convert-a-legacy-exchange-peering-to-an-azure-resource-by-using-the-azure-portal"></a>Azure portalını kullanarak eski bir Exchange'i Azure kaynağına dönüştürün

Bu makalede, Azure portalını kullanarak varolan eski exchange'in Azure kaynağına nasıl dönüştürülür.

İsterseniz, [PowerShell](howto-legacy-exchange-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [ön koşulları](prerequisites.md) ve Exchange bakış [geçiş ini](walkthrough-exchange-all.md) gözden geçirin.

## <a name="convert-a-legacy-exchange-peering-to-an-azure-resource"></a>Azure kaynağına bakan eski bir Exchange'i dönüştürme

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="convert-legacy-exchange-peering"></a><a name=create></a>Eski Exchange eşlemi dönüştürme

**Eşleme** kaynağını kullanarak eski eşleme bağlantılarını dönüştürebilirsiniz.

#### <a name="launch-the-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırır
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration-legacy.md)]

### <a name="verify-exchange-peering"></a><a name=get></a>Exchange eşlemedoğru
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet'e bakan SSS'lere](faqs.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Exchange eşlemi oluşturma veya değiştirme](howto-exchange-portal.md)
