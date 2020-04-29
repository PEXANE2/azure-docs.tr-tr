---
title: Azure portal kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme
titleSuffix: Azure
description: Azure portal kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 87a7a6bca608f1748d3b659eabdc3e941b537377
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678579"
---
# <a name="convert-a-legacy-exchange-peering-to-an-azure-resource-by-using-the-azure-portal"></a>Azure portal kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme

Bu makalede, Azure portal kullanarak mevcut bir eski Exchange eşlemesinin Azure kaynağına nasıl dönüştürüleceği açıklanır.

İsterseniz, [PowerShell](howto-legacy-exchange-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [Exchange eşleme](walkthrough-exchange-all.md) Kılavuzu ' nu gözden geçirin.

## <a name="convert-a-legacy-exchange-peering-to-an-azure-resource"></a>Eski bir Exchange eşlemesini Azure kaynağına dönüştürme

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="convert-legacy-exchange-peering"></a><a name=create></a>Eski Exchange eşlemesini Dönüştür

**Eşleme** kaynağını kullanarak eski eşleme bağlantılarını dönüştürebilirsiniz.

#### <a name="launch-the-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration-legacy.md)]

### <a name="verify-exchange-peering"></a><a name=get></a>Exchange eşlemesini doğrula
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için bkz. [Internet eşlemesi SSS](faqs.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak bir Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-portal.md)
