---
title: Portalı kullanarak bir Exchange eşlemesi oluşturma veya değiştirme
titleSuffix: Azure
description: Portalı kullanarak bir Exchange eşlemesi oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 2c186decf68d167ab2c5ab7696c2dfb51d77a071
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774581"
---
# <a name="create-or-modify-an-exchange-peering-using-the-portal"></a>Portalı kullanarak bir Exchange eşlemesi oluşturma veya değiştirme

Bu makalede, portalı kullanarak bir Microsoft Exchange bakan nasıl açıklanmaktadır. Bu makalede, kaynağın durumunu denetlemek, güncelleştirmek veya silmek ve deprovision nasıl gösterir.

İsterseniz, [PowerShell](howto-exchange-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [Önkoşullar](prerequisites.md) ve [Exchange bakış gözden geçirin.](walkthrough-exchange-all.md)
* Azure kaynaklarına dönüştürülmemiş microsoft ile exchange eşlemeniz varsa, portalı [kullanarak Azure kaynağına bakan eski bir Exchange'e](howto-legacy-exchange-portal.md) bakın

## <a name="create-and-provision-an-exchange-peering"></a>Exchange eşleme oluşturma ve sağlama

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-an-exchange-peering"></a><a name=create></a>Exchange eşleme oluşturma

**Peering** kaynağını kullanarak yeni bir eşleme isteği oluşturabilirsiniz.

#### <a name="launch-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration.md)]

### <a name="verify-an-exchange-peering"></a><a name=get></a>Exchange eşlemelerini doğrulama
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="modify-an-exchange-peering"></a><a name="modify"></a>Exchange eşleme sini değiştirme
[!INCLUDE [peering-exchange-modify-portal](./includes/exchange-portal-modify.md)]

## <a name="deprovision-an-exchange-peering"></a><a name="delete"></a>Exchange'e bakmayı deprovision
[!INCLUDE [peering-exchange-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Doğrudan eşleme oluşturma veya değiştirme](howto-direct-portal.md)
* [Portalı kullanarak eski bir Doğrudan eşlemeyi bir Azure kaynağına dönüştürme](howto-legacy-direct-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet'e bakan SSS'leri](faqs.md) ziyaret edin
