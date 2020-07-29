---
title: Azure PowerShell kullanarak VHD dosyasından özel görüntü oluşturma
description: PowerShell kullanarak bir VHD dosyasından Azure DevTest Labs özel görüntü oluşturmayı otomatikleştirin
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 4b0712fdbec1ce23ad9e09d972e425cb7941107b
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288985"
---
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a>PowerShell kullanarak bir VHD dosyasından özel görüntü oluşturma

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar, PowerShell kullanarak bir VHD dosyasından özel görüntü oluşturma işleminde size yol gösterir:

1. Bir PowerShell isteminde, **Connect-AzAccount** cmdlet 'ine aşağıdaki çağrı ile Azure hesabınızda oturum açın.

    ```powershell
    Connect-AzAccount
    ```

1.  **Select-AzSubscription** cmdlet 'ini çağırarak istenen Azure aboneliğini seçin. **$SubscriptionID** değişkeni için aşağıdaki yer tutucusunu geçerli bir Azure abonelik kimliğiyle değiştirin.

    ```powershell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzSubscription -SubscriptionId $subscriptionId
    ```

1.  **Get-AzResource** cmdlet 'ini çağırarak laboratuvar nesnesini alın. **$LabRg** ve **$labName** değişkenleri için aşağıdaki yer tutucuları, ortamınız için uygun değerlerle değiştirin.

    ```powershell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```

1.  **$VhdUri** değişkeni için aşağıdaki yer tutucusunu, KARŞıYA yüklenen VHD dosyanıza URI ile değiştirin. VHD dosyasının URI 'sini Azure portal depolama hesabının blob dikey penceresinden alabilirsiniz.

    ```powershell
    $vhdUri = '<Specify the VHD URI here>'
    ```

1.  **New-AzResourceGroupDeployment** cmdlet 'ini kullanarak özel görüntü oluşturun. **$CustomImageName** ve **$customImageDescription** değişkenleri için aşağıdaki yer tutucuları, ortamınız için anlamlı adlarla değiştirin.

    ```powershell
    $customImageName = '<Specify the custom image name>'
    $customImageDescription = '<Specify the custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/samples/DevTestLabs/QuickStartTemplates/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-to-create-a-custom-image-from-a-vhd-file"></a>Bir VHD dosyasından özel bir görüntü oluşturmak için PowerShell betiği

Aşağıdaki PowerShell betiği, bir VHD dosyasından özel bir görüntü oluşturmak için kullanılabilir. Yer tutucuları (açılı parantez ile başlayıp biter) gereksinimlerinize uygun değerlerle değiştirin.

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

- [Özel görüntüler veya formüller mi?](./devtest-lab-faq.md#blog-post)
- [Azure DevTest Labs arasında özel görüntüleri kopyalama](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınızda VM ekleme](devtest-lab-add-vm.md)
