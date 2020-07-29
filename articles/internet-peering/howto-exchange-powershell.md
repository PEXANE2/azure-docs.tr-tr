---
title: PowerShell kullanarak bir Exchange eşlemesi oluşturma veya değiştirme
titleSuffix: Azure
description: PowerShell kullanarak bir Exchange eşlemesi oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 1c0620875e7da4582156080c4a52fd6fbe0e573c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84700255"
---
# <a name="create-or-modify-an-exchange-peering-by-using-powershell"></a>PowerShell kullanarak bir Exchange eşlemesi oluşturma veya değiştirme

Bu makalede PowerShell cmdlet 'leri ve Kaynak Yöneticisi dağıtım modeli kullanılarak Microsoft Exchange eşlemesi oluşturma açıklanır. Bu makalede ayrıca kaynağın durumunu denetleme, güncelleştirme veya silme ve sağlamayı kaldırma işlemlerinin nasıl yapılacağı gösterilir.

İsterseniz, Azure [portalını](howto-exchange-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [Exchange eşleme](walkthrough-exchange-all.md) Kılavuzu ' nu gözden geçirin.
* Azure kaynaklarına Dönüştürülmeyen Microsoft ile Exchange eşlemeleri zaten varsa, bkz. [PowerShell kullanarak eski Exchange eşlemesini Azure kaynağına dönüştürme](howto-legacy-exchange-powershell.md).

### <a name="work-with-azure-powershell"></a>Azure PowerShell çalışın
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="create-and-provision-an-exchange-peering"></a>Bir Exchange eşlemesi oluşturma ve sağlama

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-the-list-of-supported-peering-locations-for-exchange-peering"></a><a name=exchange-location></a>Exchange eşlemesi için desteklenen eşleme konumlarının listesini alın
[!INCLUDE [exchange-location](./includes/exchange-powershell-create-location.md)]

### <a name="create-an-exchange-peering"></a><a name=create></a>Exchange eşlemesi oluşturma
[!INCLUDE [exchange-peering](./includes/exchange-powershell-create-connection.md)]

### <a name="get-exchange-peering"></a><a name=get></a>Exchange eşlemesini al
[!INCLUDE [peering-exchange-get](./includes/exchange-powershell-get.md)]

## <a name="modify-an-exchange-peering"></a><a name="modify"></a>Bir Exchange eşlemesini değiştirme
[!INCLUDE [peering-exchange-modify](./includes/exchange-powershell-modify.md)]

## <a name="deprovision-an-exchange-peering"></a><a name=delete></a>Bir Exchange eşlemesi sağlamayı kaldırma

[!INCLUDE [peering-exchange-delete](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell kullanarak doğrudan eşleme oluşturma veya değiştirme](howto-direct-powershell.md)
* [PowerShell kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-powershell.md)

## <a name="additional-resources"></a>Ek kaynaklar
Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```

Daha fazla bilgi için bkz. [Internet eşlemesi SSS](faqs.md).
