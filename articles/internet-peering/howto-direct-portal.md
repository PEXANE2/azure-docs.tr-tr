---
title: Portalı kullanarak Doğrudan eşleme oluşturma veya değiştirme
titleSuffix: Azure
description: Portalı kullanarak Doğrudan eşleme oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 67a4944082d3ebc14fd564eedee0310afe6e3ff5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775335"
---
# <a name="create-or-modify-a-direct-peering-using-the-portal"></a>Portalı kullanarak Doğrudan eşleme oluşturma veya değiştirme

Bu makalede, portalı kullanarak Microsoft Direct'in nasıl bir bakış oluşturmak için nasıl açıklanmaktadır. Bu makalede, kaynağın durumunu denetlemek, güncelleştirmek veya silmek ve deprovision nasıl gösterir.

İsterseniz, [PowerShell](howto-direct-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [Önkoşulları](prerequisites.md) ve [Doğrudan bakan gözden geçirin.](walkthrough-direct-all.md)
* Azure kaynaklarına dönüştürülmemiş olan Microsoft ile doğrudan bakışınız varsa, [portalı kullanarak eski bir Doğrudan](howto-legacy-direct-portal.md) Azure kaynağına bakış dönüştürme konusuna bakın

## <a name="create-and-provision-a-direct-peering"></a>Doğrudan bir bakış oluşturma ve sağlama

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-a-direct-peering"></a><a name=create></a>Doğrudan eşleme oluşturma

**Peering** kaynağını kullanarak yeni bir eşleme isteği oluşturabilirsiniz.

#### <a name="launch-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Doğrudan bakmayı doğrula
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="modify-a-direct-peering"></a><a name="modify"></a>Doğrudan eşleme değiştirme
[!INCLUDE [peering-direct-modify-portal](./includes/direct-portal-modify.md)]

## <a name="deprovision-a-direct-peering"></a><a name="delete"></a>Doğrudan bir bakış hükmünü deprovision
[!INCLUDE [peering-direct-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Exchange eşlemi oluşturun veya değiştirin.](howto-exchange-portal.md)
* [Portalı kullanarak Azure kaynağına bakan eski bir Exchange'i dönüştürün.](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet'e bakan SSS'leri](faqs.md) ziyaret edin
