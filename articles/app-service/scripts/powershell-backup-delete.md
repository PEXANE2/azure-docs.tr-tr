---
title: 'PowerShell: Uygulama yedeklemesi silme'
description: Uygulama Hizmeti'nin dağıtımını ve yönetimini otomatikleştirmek için Azure PowerShell'i nasıl kullanacağınızı öğrenin. Bu örnek, bir uygulama yedeklemesi nasıl silinir gösterir.
author: msangapu-msft
tags: azure-service-management
ms.assetid: ebcadb49-755d-4202-a5eb-f211827a9168
ms.topic: sample
ms.date: 10/30/2017
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: 93b53bedef291b9aa740f5bbe9c492185f035f89
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80044831"
---
# <a name="delete-a-backup-for-a-web-using-azure-powershell"></a>Azure PowerShell'i kullanarak bir web için yedeklemeyi silme

Bu örnek betik, App Service’te ilgili kaynaklarıyla birlikte bir web uygulaması oluşturur ve sonra bu web uygulaması için tek seferlik bir yedekleme oluşturur. 

Bu betiği çalıştırmak için bir web uygulamasının var olan bir yedeği gerekir. Bir yedek oluşturmak için bkz. [Bir web uygulamasını yedekleme](powershell-backup-onetime.md) veya [Bir web uygulaması için zamanlanmış yedekleme oluşturma](powershell-backup-scheduled.md).

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-delete/backup-delete.ps1?highlight=1-2,11 "Delete a backup for a web app")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Betik örneği çalıştırıldıktan sonra, kaynak grubunu, web uygulamasını ve ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzWebAppBackupList](/powershell/module/az.websites/get-azwebappbackuplist) | Bir web uygulamasının yedekleme listesini alır. |
| [Remove-AzWebAppBackup](/powershell/module/az.websites/remove-azwebappbackup) | Web uygulamasının belirtilen yedeğini kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../samples-powershell.md) bölümünde bulabilirsiniz.
