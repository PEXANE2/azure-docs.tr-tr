---
title: PowerShell kullanarak doğrudan eşleme oluşturma veya değiştirme
titleSuffix: Azure
description: PowerShell kullanarak doğrudan eşleme oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 69031550bdab1535213c78f81426fa76e8ea62ad
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774243"
---
# <a name="create-or-modify-a-direct-peering-using-powershell"></a>PowerShell kullanarak doğrudan eşleme oluşturma veya değiştirme

Bu makalede PowerShell cmdlet 'leri ve Kaynak Yöneticisi dağıtım modeli kullanılarak Microsoft doğrudan eşlemesinin nasıl oluşturulacağı açıklanır. Bu makalede ayrıca kaynağın durumunu denetleme, güncelleştirme veya silme ve sağlamayı kaldırma işlemlerinin nasıl yapılacağı gösterilir.

İsterseniz, [portalı](howto-direct-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [doğrudan eşleme](walkthrough-direct-all.md) kılavuzunu gözden geçirin.
* Azure kaynaklarına Dönüştürülmeyen Microsoft ile doğrudan eşleme yapmanız durumunda, [PowerShell kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-powershell.md) konusuna bakın

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="create-and-provision-a-direct-peering"></a>Doğrudan eşleme oluşturma ve sağlama

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name=direct-location></a>Doğrudan eşleme için desteklenen eşleme konumlarının listesini al
[!INCLUDE [direct-location](./includes/direct-powershell-create-location.md)]

### <a name=create></a>Doğrudan eşleme oluşturma
[!INCLUDE [direct-peering](./includes/direct-powershell-create-connection.md)]

### <a name=get></a>Doğrudan eşlemeyi doğrula
[!INCLUDE [peering-direct-get](./includes/direct-powershell-get.md)]

## <a name="modify"></a>Doğrudan eşlemeyi değiştirme
[!INCLUDE [peering-direct-modify](./includes/direct-powershell-modify.md)]

## <a name="delete"></a>Doğrudan eşleme sağlamayı kaldırma
[!INCLUDE [peering-direct-delete](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell kullanarak Exchange eşlemesi oluşturun veya değiştirin](howto-exchange-powershell.md).
* [PowerShell kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürün](howto-legacy-exchange-powershell.md).

## <a name="additional-resources"></a>Ek kaynaklar
Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```

Daha fazla bilgi için [Internet eşlemesi SSS](faqs.md) ' yi ziyaret edin
