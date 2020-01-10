---
title: Portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme
titleSuffix: Azure
description: Portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: a40c7bbc9f37135814b7bba3396d368faf97a166
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75775205"
---
# <a name="convert-a-legacy-exchange-peering-to-azure-resource-using-the-portal"></a>Portalı kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme

Bu makalede, mevcut bir eski Exchange eşlemesinin Portal kullanılarak Azure kaynağına nasıl dönüştürüleceği açıklanır.

İsterseniz, [PowerShell](howto-legacy-exchange-powershell.md)'i kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [Exchange eşleme](walkthrough-exchange-all.md) kılavuzunu gözden geçirin.

## <a name="convert-a-legacy-exchange-peering-to-azure-resource"></a>Eski bir Exchange eşlemesini Azure kaynağına Dönüştür

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name=create></a>Eski Exchange eşlemesini Dönüştür

**Eşleme** kaynağını kullanarak eski eşleme bağlantılarını dönüştürebilirsiniz.

#### <a name="launch-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration-legacy.md)]

### <a name=get></a>Exchange eşlemesini doğrula
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet eşlemesi SSS](faqs.md) ' yi ziyaret edin

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak bir Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-portal.md)
