---
title: Azure PowerShell betik örneği-Azure sanal makinesini yedekleme | Microsoft Docs
description: Azure PowerShell betik örneği-Azure sanal makinesini yedekleme
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 03/05/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 84581dd4dfd1cc993476e0e85f804f32d28e8ab4
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68467182"
---
# <a name="back-up-an-encrypted-azure-virtual-machine-with-powershell"></a>PowerShell ile şifrelenmiş bir Azure sanal makinesini yedekleme

Bu betik, şifrelenmiş bir Azure sanal makinesi için coğrafi olarak yedekli depolama (GRS) içeren bir kurtarma hizmetleri Kasası oluşturur. Varsayılan koruma ilkesi kasaya uygulanır. İlke, sanal makine için günlük bir yedekleme oluşturur ve her yedeklemeyi 30 gün boyunca tutar. Betik Ayrıca, sanal makine için ilk kurtarma noktasını tetikler ve bu kurtarma noktasını 365 gün boyunca tutar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/backup/backup-encrypted-vm/backup-encrypted-vm.ps1 "Back up encrypted virtual machine")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.


| Komut | Notlar | 
|---|---| 
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. | 
| [New-Azrecoveryserviceskasa](https://docs.microsoft.com/powershell/module/az.recoveryservices/new-azrecoveryservicesvault) | Yedeklemeleri depolamak için bir kurtarma hizmetleri Kasası oluşturur. | 
| [Set-AzRecoveryServicesBackupProperty](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty) | Kurtarma Hizmetleri kasasındaki yedekleme depolama özelliklerini ayarlar. | 
| [New-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy)| Kurtarma Hizmetleri kasasındaki zamanlama ilkesini ve bekletme ilkesini kullanarak koruma ilkesi oluşturur. | 
| [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) | Key Vault üzerinde hizmet sorumlusuna şifreleme anahtarları için erişim verecek izinleri ayarlar. | 
| [Enable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection) | Belirtilen yedekleme koruma ilkesiyle bir öğe için yedeklemeyi mümkün. | 
| [Set-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy)| Mevcut bir yedekleme koruma ilkesini değiştirir. | 
| [Backup-Azrecoveryservicesbackupıtem](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem) | Yedekleme zamanlamaya bağlı olmayan korumalı Azure Backup bir öğe için bir yedekleme başlatır. |
| [Wait-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) | Azure Backup işinin bitmesini bekler. | 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. | 

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

