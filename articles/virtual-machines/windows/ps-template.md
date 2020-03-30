---
title: Azure'da şablondan Windows VM oluşturma
description: Kolayca yeni bir Windows VM oluşturmak için Kaynak Yöneticisi şablonu ve PowerShell kullanın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 19129d61-8c04-4aa9-a01f-361a09466805
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 03/22/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 99e292930414ae027c9cbbf3a901d550041899d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74065544"
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a>Kaynak Yöneticisi şablonundan Windows sanal makinesi oluşturma

Azure Bulutu kabuğundan Bir Azure Kaynak Yöneticisi şablonu ve Azure PowerShell kullanarak nasıl Windows sanal makinesi oluşturabilirsiniz öğrenin. Bu makalede kullanılan şablon, Windows Server'ı çalıştıran tek bir sanal makineyi tek bir alt ağla yeni bir sanal ağda dağıtır. Bir Linux sanal makinesi oluşturmak için Azure [Kaynak Yöneticisi şablonları içeren bir Linux sanal makinesinin nasıl oluşturulabildiğini](../linux/create-ssh-secured-vm-from-template.md)öğrenin.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Azure sanal makinesi oluşturmak genellikle iki adım içerir:

- Bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir sanal makineden önce bir kaynak grubu oluşturulmalıdır.
- Sanal makine oluşturur.

Aşağıdaki örnekte, [Azure Quickstart şablonundan](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json)bir VM oluşturulur. Şablonun bir kopyası aşağıda verilmiştir:

[!code-json[create-windows-vm](~/quickstart-templates/101-vm-simple-windows/azuredeploy.json)]

PowerShell komut dosyasını çalıştırmak için Azure Bulutu kabuğunu açmak için **deneyin'i** seçin. Komut dosyasını yapıştırmak için kabuğu sağ tıklatın ve sonra **Yapıştır'ı**seçin:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the administrator username"
$adminPassword = Read-Host -Prompt "Enter the administrator password" -AsSecureString
$dnsLabelPrefix = Read-Host -Prompt "Enter an unique DNS name for the public IP"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json" `
    -adminUsername $adminUsername `
    -adminPassword $adminPassword `
    -dnsLabelPrefix $dnsLabelPrefix

 (Get-AzVm -ResourceGroupName $resourceGroupName).name

```

PowerShell'i Azure Bulut uymu yerine yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici için Azure PowerShell modülü gerekir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, Azure `Connect-AzAccount` ile bağlantı oluşturmak için de çalışmanız gerekir.

Önceki örnekte, GitHub'da depolanan bir şablon belirtilmişsiniz. Ayrıca bir şablon indirebilir veya oluşturabilir ve `--template-file` parametreile yerel yolu belirtebilirsiniz.

Aşağıdaki ek kaynakları da inceleyebilirsiniz:

- Kaynak Yöneticisi şablonlarını nasıl geliştireceklerini öğrenmek için [Azure Kaynak Yöneticisi belgelerine](/azure/azure-resource-manager/)bakın.
- Azure sanal makine şemalarını görmek için [Azure şablonu başvurusuna](/azure/templates/microsoft.compute/allversions)bakın.
- Daha fazla sanal makine şablonu örneği görmek için [Azure Quickstart şablonlarına](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular)bakın.

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

Önceki komut dosyasındaki son PowerShell komutu sanal makine adını gösterir. Sanal makineye bağlanmak için [Windows çalıştıran bir Azure sanal makinesine nasıl bağlanıp oturum](./connect-logon.md)açabilirsiniz'a bakın.

## <a name="next-steps"></a>Sonraki Adımlar

- Dağıtımla ilgili sorunlar varsa, Azure Kaynak [Yöneticisi ile sık karşılaşılan Azure dağıtım hatalarını giderme](../../resource-manager-common-deployment-errors.md)sorununa göz atabilirsiniz.
- [Azure PowerShell modülü yle Windows VM'leri Oluştur'da](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)sanal bir makineoluşturma ve yönetme yi öğrenin.

Şablon oluşturma hakkında daha fazla bilgi edinmek için, dağıttığınız kaynak türlerinin JSON sözdizimini ve özelliklerini görüntüleyin:

- [Microsoft.Network/publicIPAdresleri](/azure/templates/microsoft.network/publicipaddresses)
- [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
- [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
- [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
