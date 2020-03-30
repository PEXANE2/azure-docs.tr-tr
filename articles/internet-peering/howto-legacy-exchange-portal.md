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
ms.openlocfilehash: a40c7bbc9f37135814b7bba3396d368faf97a166
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775205"
---
# <a name="convert-a-legacy-exchange-peering-to-azure-resource-using-the-portal"></a>Portalı kullanarak eski bir Exchange eşlemesini bir Azure kaynağına dönüştürme

Bu makalede, portalı kullanarak varolan eski exchange'in Azure kaynağına nasıl dönüştürülür.

İsterseniz, [PowerShell](howto-legacy-exchange-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [Önkoşullar](prerequisites.md) ve [Exchange bakış gözden geçirin.](walkthrough-exchange-all.md)

## <a name="convert-a-legacy-exchange-peering-to-azure-resource"></a>Eski bir Exchange eşlemeyi Azure kaynağına dönüştürme

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="convert-legacy-exchange-peering"></a><a name=create></a>Eski Exchange eşlemi dönüştürme

**Peering** kaynağını kullanarak eski eşleme bağlantılarını dönüştürebilirsiniz.

#### <a name="launch-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration-legacy.md)]

### <a name="verify-exchange-peering"></a><a name=get></a>Exchange eşlemedoğru
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet'e bakan SSS'leri](faqs.md) ziyaret edin

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak bir Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-portal.md)
