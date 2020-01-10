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
ms.openlocfilehash: 9900414d38bd597d08a80d15e908228c06ce06ea
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75775062"
---
# <a name="convert-a-legacy-direct-peering-to-azure-resource-using-the-portal"></a>Portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme

Bu makalede, mevcut bir eski doğrudan eşlemeyi Portal kullanılarak Azure kaynağına nasıl dönüştürebileceğiniz açıklanır.

İsterseniz, [PowerShell](howto-legacy-direct-powershell.md)'i kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [doğrudan eşleme](walkthrough-direct-all.md) kılavuzunu gözden geçirin.


## <a name="convert-legacy-direct-peering-to-azure-resource"></a>Eski doğrudan eşlemeyi Azure kaynağına Dönüştür

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name=create></a>Eski doğrudan eşlemeyi Dönüştür

**Eşleme** kaynağını kullanarak eski eşleme bağlantılarını dönüştürebilirsiniz.

#### <a name="launch-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration-legacy.md)]

### <a name=get></a>Doğrudan eşlemeyi doğrula
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet eşlemesi SSS](faqs.md) ' yi ziyaret edin

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak doğrudan eşleme oluşturun veya değiştirin](howto-direct-portal.md).
