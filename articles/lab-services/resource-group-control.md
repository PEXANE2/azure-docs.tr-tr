---
title: Azure DevTest Labs'daki VM'ler için kaynak grubunu belirtin | Microsoft Dokümanlar
description: Azure DevTest Labs'daki bir laboratuvarda VM'ler için bir kaynak grubunu nasıl belirtebilirsiniz öğrenin.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2019
ms.author: spelluru
ms.openlocfilehash: 29816d158cf1428727b7ff17bcc2c347f402dedf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77134525"
---
# <a name="specify-a-resource-group-for-lab-virtual-machines-in-azure-devtest-labs"></a>Azure DevTest Labs'daki laboratuvar sanal makineleri için bir kaynak grubu belirtin

Bir laboratuvar sahibi olarak, laboratuvar sanal makinelerinizi belirli bir kaynak grubunda oluşturulacak şekilde yapılandırabilirsiniz. Bu özellik, aşağıdaki senaryolarda size yardımcı olur:

- Aboneliğinizde laboratuvarlar tarafından oluşturulan daha az kaynak grubuna sahip olun.
- Laboratuvarlarınızın, yapılandırdığınız sabit bir kaynak grupları kümesi içinde çalışmasını sorun.
- Azure aboneliğinizde kaynak grupları oluşturmak için gereken kısıtlamalar ve onaylar üzerinde çalışın.
- Bu kaynakları izlemeyi ve kaynak grubu düzeyinde kaynakları yönetmek için [ilkeler](../governance/policy/overview.md) uygulamayı kolaylaştırmak için tüm laboratuvar kaynaklarınızı tek bir kaynak grubunda birleştirin.

Bu özellik sayesinde, tüm laboratuvar VM'leriniz için Azure aboneliğinizde yeni veya varolan bir kaynak grubu belirtmek için bir komut dosyası kullanabilirsiniz. Şu anda Azure DevTest Labs bu özelliği bir API aracılığıyla destekler.

> [!NOTE]
> DevTest Labs'da laboratuvar oluşturduğunuzda tüm abonelik sınırları geçerlidir. Bir laboratuarı aboneliğinizdeki diğer kaynaklar olarak düşünün. Kaynak grupları söz konusu olduğunda, sınır [abonelik başına 980 kaynak grubudur.](../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits) 

## <a name="use-azure-portal"></a>Azure portalı kullanma
Laboratuvarda oluşturulan tüm VM'ler için bir kaynak grubu belirtmek için aşağıdaki adımları izleyin. 

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol navigasyon menüsünden **Tüm Hizmetler'i** seçin. 
3. Listeden **DevTest Labs**'i seçin.
4. Laboratuvarlar listesinden, **laboratuvarınızı**seçin.  
5. Sol menüdeki **Ayarlar** bölümünde **Yapılandırma ve ilkeler'i** seçin. 
6. Sol menüde **Lab ayarlarını** seçin. 
7. **Tek bir kaynak grubundaki tüm sanal makineleri**seçin. 
8. Açılan listede varolan bir kaynak grubu seçin (veya) **yeni oluştur'u**seçin, kaynak grubu için bir **ad** girin ve **Tamam'ı**seçin. 

    ![Tüm laboratuvar VM'leri için kaynak grubunu seçin](./media/resource-group-control/select-resource-group.png)

## <a name="use-powershell"></a>PowerShell kullanma 
Aşağıdaki örnek, yeni bir kaynak grubundaki tüm laboratuvar sanal makinelerini oluşturmak için PowerShell komut dosyasının nasıl kullanılacağını gösterir.

```powershell
[CmdletBinding()]
Param(
    $subId,
    $labRg,
    $labName,
    $vmRg
)

az login | out-null

az account set --subscription $subId | out-null

$rgId = "/subscriptions/"+$subId+"/resourceGroups/"+$vmRg

"Updating lab '$labName' with vm rg '$rgId'..."

az resource update -g $labRg -n $labName --resource-type "Microsoft.DevTestLab/labs" --api-version 2018-10-15-preview --set properties.vmCreationResourceGroupId=$rgId

"Done. New virtual machines will now be created in the resource group '$vmRg'."
```

Aşağıdaki komutu kullanarak komut dosyasını çağırın. ResourceGroup.ps1 önceki komut dosyası içeren dosyadır:

```powershell
.\ResourceGroup.ps1 -subId <subscriptionID> -labRg <labRGNAme> -labName <LanName> -vmRg <RGName> 
```

## <a name="use-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanma
Bir laboratuvar oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi şablonunuzun laboratuvar özellikleri **bölümündevmCreationResourceGroupId** özelliğini kullanın:

```json
        {
            "type": "microsoft.devtestlab/labs",
            "name": "[parameters('lab_name')]",
            "apiVersion": "2018_10_15_preview",
            "location": "eastus",
            "tags": {},
            "scale": null,
            "properties": {
                "vmCreationResourceGroupId": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>",
                "labStorageType": "Premium",
                "premiumDataDisks": "Disabled",
                "provisioningState": "Succeeded",
                "uniqueIdentifier": "000000000f-0000-0000-0000-00000000000000"
            },
            "dependsOn": []
        },
```


## <a name="api-to-configure-a-resource-group-for-lab-vms"></a>Laboratuvar VM'leri için bir kaynak grubunu yapılandırmak için API
Bu API'yi kullanırken laboratuvar sahibi olarak aşağıdaki seçeneklere sahipsiniz:

- Tüm sanal makineler için **laboratuvarın kaynak grubunu** seçin.
- Tüm sanal makineler için laboratuvarın kaynak grubu dışında **varolan** bir kaynak grubu seçin.
- Tüm sanal makineler için yeni bir **kaynak grubu** adı girin.
- Laboratuardaki her VM için bir kaynak grubu oluşturulduğu varolan davranışı kullanmaya devam edin.
 
Bu ayar, laboratuvarda oluşturulan yeni sanal makineler için geçerlidir. Laboratuarınızda kendi kaynak gruplarında oluşturulan eski VM'ler etkilenmez. Laboratuvarınızda oluşturulan ortamlar kendi kaynak gruplarında kalmaya devam eder.

Bu API nasıl kullanılır:
- **API**sürümünü 2018_10_15_preview kullanın.
- Yeni bir kaynak grubu belirtirseniz, aboneliğinizdeki **kaynak gruplarına yazma izinleriniz** olduğundan emin olun. Yazma izinleri yoksa, belirtilen kaynak grubunda yeni sanal makineler oluşturmak başarısız olur.
- API kullanırken, tam **kaynak grubu kimliği**geçmek. Örneğin: `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>`. Kaynak grubunun laboratuarla aynı abonelikte olduğundan emin olun. 


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Laboratuvar için ilkeleri ayarlama](devtest-lab-get-started-with-lab-policies.md)
- [Sık sorulan sorular](devtest-lab-faq.md)
