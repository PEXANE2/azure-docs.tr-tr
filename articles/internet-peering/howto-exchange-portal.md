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
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774581"
---
# <a name="create-or-modify-an-exchange-peering-using-the-portal"></a>Portalı kullanarak bir Exchange eşlemesi oluşturma veya değiştirme

Bu makalede Portal kullanılarak Microsoft Exchange eşlemesinin nasıl oluşturulacağı açıklanır. Bu makalede ayrıca kaynağın durumunu denetleme, güncelleştirme veya silme ve sağlamayı kaldırma işlemlerinin nasıl yapılacağı gösterilir.

İsterseniz, [PowerShell](howto-exchange-powershell.md)'i kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [Exchange eşleme](walkthrough-exchange-all.md) kılavuzunu gözden geçirin.
* Azure kaynaklarına Dönüştürülmeyen Microsoft ile Exchange eşlerinin zaten olduğu durumlarda, [portalı kullanarak eski Exchange eşlemesini Azure kaynağına dönüştürme](howto-legacy-exchange-portal.md) bölümüne bakın.

## <a name="create-and-provision-an-exchange-peering"></a>Bir Exchange eşlemesi oluşturma ve sağlama

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name=create></a>Exchange eşlemesi oluşturma

**Eşleme** kaynağını kullanarak yeni bir eşleme isteği oluşturabilirsiniz.

#### <a name="launch-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration.md)]

### <a name=get></a>Bir Exchange eşlemesini doğrulama
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="modify"></a>Bir Exchange eşlemesini değiştirme
[!INCLUDE [peering-exchange-modify-portal](./includes/exchange-portal-modify.md)]

## <a name="delete"></a>Bir Exchange eşlemesi sağlamayı kaldırma
[!INCLUDE [peering-exchange-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak doğrudan eşleme oluşturma veya değiştirme](howto-direct-portal.md)
* [Portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet eşlemesi SSS](faqs.md) ' yi ziyaret edin
