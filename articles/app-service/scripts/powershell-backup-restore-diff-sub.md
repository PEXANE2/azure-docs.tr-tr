---
title: 'PowerShell: Başka bir aboneye yedekleme geri yükleme'
description: Uygulama Hizmeti'nin dağıtımını ve yönetimini otomatikleştirmek için Azure PowerShell'i nasıl kullanacağınızı öğrenin. Bu örnek, başka bir abonelikte yedeklemenin nasıl geri yüklenir olduğunu gösterir.
author: msangapu-msft
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.topic: sample
ms.date: 11/21/2018
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: 4a22148d4151fd3c80c7be439b5dd281200e5456
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80044584"
---
# <a name="restore-a-web-app-from-a-backup-in-another-subscription-using-powershell"></a>PowerShell'i kullanarak başka bir abonelikteki bir yedeklemeden web uygulamasını geri yükleme

Bu örnek komut dosyası, varolan bir web uygulamasından önceden tamamlanmış bir yedeklemeyi alır ve başka bir abonelikteki bir web uygulamasına geri yüklenir. 

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview)bulunan yönergeyi kullanarak Azure PowerShell'i yükleyin ve ardından Azure ile bağlantı oluşturmak için çalıştırın. `Connect-AzAccount` 

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore-diff-sub/backup-restore-diff-sub.ps1?highlight=1-6 "Restore a web app from a backup in another subscription")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Artık web uygulamasına ihtiyacınız yoksa, kaynak grubunu, web uygulamasını ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Ekle-AzHesap](/powershell/module/az.accounts/connect-azaccount) | Azure Resource Manager cmdlet’i istekleri için kullanılacak bir kimliği doğrulanmış hesap ekler.  |
| [Get-AzWebAppBackupList](/powershell/module/az.websites/get-azwebappbackuplist) | Bir web uygulamasının yedekleme listesini alır. |
| [Yeni-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Bir web uygulaması oluşturur |
| [Geri Yükleme-AzWebAppBackup](/powershell/module/az.websites/restore-azwebappbackup) | Önceden tamamlanmış bir yedeklemeden bir web uygulamasını geri yükler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../samples-powershell.md) bölümünde bulabilirsiniz.
