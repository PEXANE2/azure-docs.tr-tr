---
title: PowerShell | Microsoft Dokümanlar
description: Azure bildirim merkezi oluşturmak için PowerShell komut dosyasını nasıl kullanacağınızı öğrenin.
author: dimazaid
manager: femila
services: notification-hubs
editor: sethmanheim
ms.service: notification-hubs
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2020
ms.author: dimazaid
ms.openlocfilehash: 01749e1ae5c29a7e77b2143939e25cf1ccd1aec2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76263991"
---
# <a name="use-powershell-to-create-an-azure-notification-hub"></a>Azure bildirim merkezi oluşturmak için PowerShell'i kullanma

Bu örnek PowerShell komut dosyası, örnek bir Azure bildirim hub'ı oluşturur. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/notification-hubs/create-notification-hub/create-notification-hub.ps1 "Create a notification hub")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Örnek komut dosyasını çalıştırdıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanabilirsiniz:

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzNotificationHubsNamespace](/powershell/module/az.notificationhubs/new-aznotificationhubsnamespace) | Bildirim hub'ı için bir ad alanı oluşturur. |
| [Yeni-AzNotificationHub](/powershell/module/az.notificationhubs/new-aznotificationhub) | Bir bildirim hub'ı oluşturur. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).
