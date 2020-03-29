---
title: Azure PowerShell'i kullanarak VHD dosyasından özel bir resim oluşturma
description: PowerShell'i kullanarak Bir VHD dosyasından Azure DevTest Labs'da özel bir görüntü oluşturmayı otomatikleştirin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: cd144659dd8a8e981e267be998c9c783b7482840
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169571"
---
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a>PowerShell'i kullanarak Bir VHD dosyasından özel bir görüntü oluşturma

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar PowerShell kullanarak bir VHD dosyasından özel bir görüntü oluşturmada size yol:

1. PowerShell komut isteminde, **Connect-AzAccount** cmdlet'e aşağıdaki çağrıyla Azure hesabınızda oturum açın.

    ```powershell
    Connect-AzAccount
    ```

1.  **Select-AzSubscription** cmdlet'i arayarak istediğiniz Azure aboneliğini seçin. **$subscriptionId** değişkeni için aşağıdaki yer tutucuyu geçerli bir Azure abonelik kimliğiyle değiştirin.

    ```powershell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzSubscription -SubscriptionId $subscriptionId
    ```

1.  **Get-AzResource** cmdlet'i arayarak laboratuvar nesnesini alın. **$labRg** ve **$labName** değişkenler için aşağıdaki yer tutucuları ortamınız için uygun değerlerle değiştirin.

    ```powershell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```

1.  **$vhdUri** değişkeni için aşağıdaki yer tutucuyu uri ile yükleyin VHD dosyanıza değiştirin. VHD dosyasının URI'sini Azure portalındaki depolama hesabının blob bıçağından alabilirsiniz.

    ```powershell
    $vhdUri = '<Specify the VHD URI here>'
    ```

1.  **Yeni-AzResourceGroupDeployment** cmdlet'i kullanarak özel görüntü oluşturun. **$customImageName** ve **$customImageDescription** değişkenler için aşağıdaki yer tutucuları ortamınız için anlamlı adlarla değiştirin.

    ```powershell
    $customImageName = '<Specify the custom image name>'
    $customImageDescription = '<Specify the custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/samples/DevTestLabs/QuickStartTemplates/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-to-create-a-custom-image-from-a-vhd-file"></a>PowerShell komut dosyası bir VHD dosyasından özel bir görüntü oluşturmak için

Aşağıdaki PowerShell komut dosyası, bir VHD dosyasından özel bir görüntü oluşturmak için kullanılabilir. Yer tutucuları (açı braketleriyle başlayıp bitirme) ihtiyaçlarınıza uygun değerlerle değiştirin.

```powershell
# Log in to your Azure account.
Connect-AzAccount

# Select the desired Azure subscription.
$subscriptionId = '<Specify your subscription ID here>'
Select-AzSubscription -SubscriptionId $subscriptionId

# Get the lab object.
$labRg = '<Specify your lab resource group name here>'
$labName = '<Specify your lab name here>'
$lab = Get-AzResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)

# Set the URI of the VHD file.
$vhdUri = '<Specify the VHD URI here>'

# Set the custom image name and description values.
$customImageName = '<Specify the custom image name>'
$customImageDescription = '<Specify the custom image description>'

# Set up the parameters object.
$parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

# Create the custom image.
New-AzResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/samples/DevTestLabs/QuickStartTemplates/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
```

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel görüntüler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında Özel Görüntüler Kopyalama](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınıza VM ekleme](devtest-lab-add-vm.md)
