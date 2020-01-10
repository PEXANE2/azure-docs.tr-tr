---
title: PowerShell kullanarak bir Exchange eşlemesi oluşturma veya değiştirme
titleSuffix: Azure
description: PowerShell kullanarak bir Exchange eşlemesi oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 4fd7d345e5efbe6e4e86e5bb410e2df4dd917047
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774412"
---
# <a name="create-or-modify-an-exchange-peering-using-powershell"></a>PowerShell kullanarak bir Exchange eşlemesi oluşturma veya değiştirme

Bu makalede PowerShell cmdlet 'leri ve Kaynak Yöneticisi dağıtım modeli kullanılarak Microsoft Exchange eşlemesi oluşturma açıklanır. Bu makalede ayrıca kaynağın durumunu denetleme, güncelleştirme veya silme ve sağlamayı kaldırma işlemlerinin nasıl yapılacağı gösterilir.

İsterseniz, [portalı](howto-exchange-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [Exchange eşleme](walkthrough-exchange-all.md) kılavuzunu gözden geçirin.
* Azure kaynaklarına Dönüştürülmeyen Microsoft ile Exchange eşlerinin zaten olduğu durumlarda, [PowerShell kullanarak eski Exchange eşlemesini Azure kaynağına dönüştürme](howto-legacy-exchange-powershell.md) bölümüne bakın.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="create-and-provision-an-exchange-peering"></a>Bir Exchange eşlemesi oluşturma ve sağlama

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name=exchange-location></a>Exchange eşlemesi için desteklenen eşleme konumlarının listesini alın
[!INCLUDE [exchange-location](./includes/exchange-powershell-create-location.md)]

### <a name=create></a>Exchange eşlemesi oluşturma
[!INCLUDE [exchange-peering](./includes/exchange-powershell-create-connection.md)]

### <a name=get></a>Exchange eşlemesini al
[!INCLUDE [peering-exchange-get](./includes/exchange-powershell-get.md)]

## <a name="modify"></a>Bir Exchange eşlemesini değiştirme
[!INCLUDE [peering-exchange-modify](./includes/exchange-powershell-modify.md)]

## <a name=delete></a>Bir Exchange eşlemesi sağlamayı kaldırma

[!INCLUDE [peering-exchange-delete](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell kullanarak doğrudan eşleme oluşturma veya değiştirme](howto-direct-powershell.md)
* [PowerShell kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-powershell.md)

## <a name="additional-resources"></a>Ek kaynaklar
Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```

Daha fazla bilgi için [Internet eşlemesi SSS](faqs.md) ' yi ziyaret edin
