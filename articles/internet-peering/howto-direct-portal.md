---
title: Portalı kullanarak doğrudan eşleme oluşturma veya değiştirme
titleSuffix: Azure
description: Portalı kullanarak doğrudan eşleme oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 67a4944082d3ebc14fd564eedee0310afe6e3ff5
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75775335"
---
# <a name="create-or-modify-a-direct-peering-using-the-portal"></a>Portalı kullanarak doğrudan eşleme oluşturma veya değiştirme

Bu makalede Portal kullanılarak Microsoft doğrudan eşlemesinin nasıl oluşturulacağı açıklanır. Bu makalede ayrıca kaynağın durumunu denetleme, güncelleştirme veya silme ve sağlamayı kaldırma işlemlerinin nasıl yapılacağı gösterilir.

İsterseniz, [PowerShell](howto-direct-powershell.md)'i kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [doğrudan eşleme](walkthrough-direct-all.md) kılavuzunu gözden geçirin.
* Azure kaynaklarına Dönüştürülmeyen Microsoft ile doğrudan eşleme yapmanız durumunda, [portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-portal.md) konusuna bakın

## <a name="create-and-provision-a-direct-peering"></a>Doğrudan eşleme oluşturma ve sağlama

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name=create></a>Doğrudan eşleme oluşturma

**Eşleme** kaynağını kullanarak yeni bir eşleme isteği oluşturabilirsiniz.

#### <a name="launch-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration.md)]

### <a name=get></a>Doğrudan eşlemeyi doğrula
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="modify"></a>Doğrudan eşlemeyi değiştirme
[!INCLUDE [peering-direct-modify-portal](./includes/direct-portal-modify.md)]

## <a name="delete"></a>Doğrudan eşleme sağlamayı kaldırma
[!INCLUDE [peering-direct-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Exchange eşlemesi oluşturun veya değiştirin](howto-exchange-portal.md).
* [Portalı kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürün](howto-legacy-exchange-portal.md).

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet eşlemesi SSS](faqs.md) ' yi ziyaret edin
