---
title: Azure PowerShell kullanarak VM uzantısını etkinleştirme
description: Bu makalede, Azure PowerShell kullanarak karma bulut ortamlarında çalışan Azure Arc etkin sunucularına sanal makine uzantılarının nasıl dağıtılacağı açıklanır.
ms.date: 10/19/2020
ms.topic: conceptual
ms.openlocfilehash: 631aa323fee8db712acc975336bdbf9436833240
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92462986"
---
# <a name="enable-azure-vm-extensions-using-azure-powershell"></a>Azure PowerShell kullanarak Azure VM uzantılarını etkinleştirme

Bu makalede, Azure Arc etkin sunucuları tarafından desteklenen Azure VM uzantılarının Azure PowerShell kullanarak bir Linux veya Windows hibrit makinesine nasıl dağıtılacağı ve kaldırılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

- Azure PowerShell olan bir bilgisayar. Yönergeler için bkz. [Azure PowerShell yükleyip yapılandırma](/powershell/azure/).

Yay etkin sunucularla yönetilen karma sunucunuzda VM uzantılarını yönetmek için Azure PowerShell kullanmadan önce modülünü yüklemeniz gerekir `Az.ConnectedMachine` . Şu komutu yay etkin sunucunuzda çalıştırın:

`Install-Module -Name Az.ConnectedMachine`.

Yükleme tamamlandığında, aşağıdaki ileti döndürülür:

`The installed extension `Az. ConnectedMachine` is experimental and not covered by customer support. Please use with discretion.`

## <a name="enable-extension"></a>Uzantıyı etkinleştir

Yay etkin sunucunuzda bir VM uzantısını etkinleştirmek için,,,, [New-AzConnectedMachineExtension](/powershell/module/az.connectedmachine/new-azconnectedmachineextension) , `-Name` `-ResourceGroupName` `-MachineName` `-Location` `-Publisher` ,- `ExtensionType` ve `-Settings` parametreleriyle New-azconnectedmachineextension ' ı kullanın.

Aşağıdaki örnek, bir yay etkin Linux sunucusunda Log Analytics VM uzantısını sunar:

```powershell
PS C:\> $Setting = @{ "workspaceId" = "workspaceId" }
PS C:\> $protectedSetting = @{ "workspaceKey" = "workspaceKey" }
PS C:\> New-AzConnectedMachine -Name OMSLinuxAgent -ResourceGroupName "myResourceGroup" -MachineName "myMachine" -Location "eastus" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -TypeHandlerVersion "1.10" -Settings $Setting -ProtectedSetting $protectedSetting -ExtensionType OmsAgentforLinux"
```

Aşağıdaki örnek, bir yay etkin sunucusunda özel betik uzantısının etkinleştirilmesini sunar:

```powershell
PS C:\> $Setting = @{ "commandToExecute" = "powershell.exe -c Get-Process" }
PS C:\> New-AzConnectedMachine -Name custom -ResourceGroupName myResourceGroup -MachineName myMachineName -Location eastus -Publisher "Microsoft.Compute" -TypeHandlerVersion 1.10 -Settings $Setting -ExtensionType CustomScriptExtension
```

## <a name="list-extensions-installed"></a>Yüklü liste uzantıları

Yay etkin sunucunuzdaki VM uzantılarının listesini almak için [Get-AzConnectedMachineExtension](/powershell/module/az.connectedmachine/get-azconnectedmachineextension) ile `-MachineName` ve `-ResourceGroupName` parametrelerini kullanın.

Örnek:

```powershell
Get-AzConnectedMachineExtension -ResourceGroupName myResourceGroup -MachineName myMachineName

Name    Location  PropertiesType        ProvisioningState
----    --------  --------------        -----------------
custom  westus2   CustomScriptExtension Succeeded
```

## <a name="remove-an-installed-extension"></a>Yüklü bir uzantıyı kaldırma

Arc etkin sunucunuzdaki yüklü bir VM uzantısını kaldırmak için, ve parametreleriyle [Remove-AzConnectedMachineExtension](/powershell/module/az.connectedmachine/remove-azconnectedmachineextension) kullanın `-Name` `-MachineName` `-ResourceGroupName` .

Örneğin, Linux için Log Analytics VM uzantısını kaldırmak için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzConnectedMachineExtension -MachineName myMachineName -ResourceGroupName myResourceGroup -Name OmsAgentforLinux
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLI](manage-vm-extensions-cli.md)kullanarak, [Azure Portal](manage-vm-extensions-portal.md)veya [Azure Resource Manager şablonlarından](manage-vm-extensions-template.md)VM uzantılarını dağıtabilir, yönetebilir ve kaldırabilirsiniz.

- Sorun giderme bilgileri, [VM genişletmeleri sorunlarını gider kılavuzunda](troubleshoot-vm-extensions.md)bulunabilir.
