---
title: Mantıksal kuruluş için kaynakları, kaynak gruplarını ve abonelikleri etiketleme
description: Faturalandırma ve yönetme için Azure kaynaklarını düzenlemek için etiketlerin nasıl uygulanacağı gösterin.
ms.topic: conceptual
ms.date: 04/01/2020
ms.openlocfilehash: 76f9f61b3fe7002508bbd884f427efcfee698579
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548489"
---
# <a name="use-tags-to-organize-your-azure-resources-and-management-hierarchy"></a>Azure kaynaklarınızı ve yönetim hiyerarşinizi düzenlemek için etiketleri kullanın

Etiketleri Azure kaynaklarınıza, kaynak gruplarınıza ve aboneliklerinize mantıksal olarak taksonomi olarak düzenlemek için uygularsınız. Her etiket bir ad ve bir değer çiftinden oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz.

Etiketleme stratejisinin nasıl uygulanacağı yla ilgili öneriler için [Kaynak adlandırma ve etiketleme karar kılavuzuna](/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json)bakın.

[!INCLUDE [Handle personal data](../../../includes/gdpr-intro-sentence.md)]

## <a name="required-access"></a>Gerekli erişim

Bir kaynağa etiket uygulamak için **Microsoft.Resources/tags** kaynak türüne yazma erişiminiz olması gerekir. [Tag Contributor](../../role-based-access-control/built-in-roles.md#tag-contributor) rolü, varlığın kendisine erişmeden etiketleri bir varlığa uygulamanızı sağlar. Şu anda, etiket katılımcısı rolü portal aracılığıyla kaynaklara veya kaynak gruplarına etiketler uygulayamaz. Portal üzerinden aboneliklere etiketler uygulayabilir. PowerShell ve REST API aracılığıyla tüm etiket işlemlerini destekler.  

[Katılımcı](../../role-based-access-control/built-in-roles.md#contributor) rolü ayrıca etiketleri herhangi bir varlığa uygulamak için gerekli erişimi de sağlar. Etiketleri yalnızca bir kaynak türüne uygulamak için, söz leilgili kaynak rolünü kullanın. Örneğin, sanal makinelere etiketler uygulamak için [Sanal Makine Katılımcısı'nı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)kullanın.

## <a name="powershell"></a>PowerShell

### <a name="apply-tags"></a>Etiketleri uygula

Azure PowerShell, etiketleri uygulamak için iki komut sunar - [Yeni-AzTag](/powershell/module/az.resources/new-aztag) ve [Update-AzTag](/powershell/module/az.resources/update-aztag). Az.Resources modülü 1.12.0 veya daha sonra olmalıdır. Sürümünüzü `Get-Module Az.Resources`. Bu modülü yükleyebilir veya [Azure PowerShell](/powershell/azure/install-az-ps) 3.6.1 veya sonrası yükleyebilirsiniz.

**Yeni-AzTag** kaynak, kaynak grubu veya abonelikteki tüm etiketlerin yerini alır. Komutu ararken, etiketlemek istediğiniz varlığın kaynak kimliğini geçirin.

Aşağıdaki örnek, bir depolama hesabına bir etiket kümesi uygular:

```azurepowershell-interactive
$tags = @{"Dept"="Finance"; "Status"="Normal"}
$resource = Get-AzResource -Name demoStorage -ResourceGroup demoGroup
New-AzTag -ResourceId $resource.id -Tag $tags
```

Komut tamamlandığında, kaynağın iki etiketi olduğunu unutmayın.

```output
Properties :
        Name    Value
        ======  =======
        Dept    Finance
        Status  Normal
```

Komutu yeniden çalıştırıyorsanız ancak bu sefer farklı etiketlerle çalıştırılırsa, önceki etiketlerin kaldırıldığına dikkat edin.

```azurepowershell-interactive
$tags = @{"Team"="Compliance"; "Environment"="Production"}
New-AzTag -ResourceId $resource.id -Tag $tags
```

```output
Properties :
        Name         Value
        ===========  ==========
        Environment  Production
        Team         Compliance
```

Zaten etiketleri olan bir kaynağa etiket eklemek için **Update-AzTag'ı**kullanın. **-İşlem** parametresini **Birleştirme**olarak ayarlayın.

```azurepowershell-interactive
$tags = @{"Dept"="Finance"; "Status"="Normal"}
Update-AzTag -ResourceId $resource.id -Tag $tags -Operation Merge
```

İki yeni etiketin varolan iki etikete eklenmiştir dikkat edin.

```output
Properties :
        Name         Value
        ===========  ==========
        Status       Normal
        Dept         Finance
        Team         Compliance
        Environment  Production
```

Her etiket adının yalnızca bir değeri olabilir. Bir etiket için yeni bir değer sağlarsanız, birleştirme işlemini kullansanız bile eski değer değiştirilir. Aşağıdaki örnek, Durum etiketini Normal'den Yeşil'e değiştirir.

```azurepowershell-interactive
$tags = @{"Status"="Green"}
Update-AzTag -ResourceId $resource.id -Tag $tags -Operation Merge
```

```output
Properties :
        Name         Value
        ===========  ==========
        Status       Green
        Dept         Finance
        Team         Compliance
        Environment  Production
```

**-İşlem** parametresini **Değiştir'e**ayarladığınızda, varolan etiketler yeni etiket kümesiyle değiştirilir.

```azurepowershell-interactive
$tags = @{"Project"="ECommerce"; "CostCenter"="00123"; "Team"="Web"}
Update-AzTag -ResourceId $resource.id -Tag $tags -Operation Replace
```

Kaynakta yalnızca yeni etiketler kalır.

```output
Properties :
        Name        Value
        ==========  =========
        CostCenter  00123
        Team        Web
        Project     ECommerce
```

Aynı komutlar kaynak grupları veya aboneliklerle de çalışır. Etiketlemek istediğiniz kaynak grubu veya abonelik için tanımlayıcıdan geçersiniz.

Kaynak grubuna yeni bir etiket kümesi eklemek için şunları kullanın:

```azurepowershell-interactive
$tags = @{"Dept"="Finance"; "Status"="Normal"}
$resourceGroup = Get-AzResourceGroup -Name demoGroup
New-AzTag -ResourceId $resourceGroup.ResourceId -tag $tags
```

Kaynak grubunun etiketlerini güncelleştirmek için şunları kullanın:

```azurepowershell-interactive
$tags = @{"CostCenter"="00123"; "Environment"="Production"}
$resourceGroup = Get-AzResourceGroup -Name demoGroup
Update-AzTag -ResourceId $resourceGroup.ResourceId -Tag $tags -Operation Merge
```

Aboneye yeni bir etiket kümesi eklemek için şunları kullanın:

```azurepowershell-interactive
$tags = @{"CostCenter"="00123"; "Environment"="Dev"}
$subscription = (Get-AzSubscription -SubscriptionName "Example Subscription").Id
New-AzTag -ResourceId "/subscriptions/$subscription" -Tag $tags
```

Aboneliğin etiketlerini güncelleştirmek için şunları kullanın:

```azurepowershell-interactive
$tags = @{"Team"="Web Apps"}
$subscription = (Get-AzSubscription -SubscriptionName "Example Subscription").Id
Update-AzTag -ResourceId "/subscriptions/$subscription" -Tag $tags -Operation Merge
```

Kaynak grubunda aynı ada sahip birden fazla kaynağınız olabilir. Bu durumda, her kaynağı aşağıdaki komutlarla ayarlayabilirsiniz:

```azurepowershell-interactive
$resource = Get-AzResource -ResourceName sqlDatabase1 -ResourceGroupName examplegroup
$resource | ForEach-Object { Update-AzTag -Tag @{ "Dept"="IT"; "Environment"="Test" } -ResourceId $_.ResourceId -Operation Merge }
```

### <a name="list-tags"></a>Etiketleri listeleme

Kaynak, kaynak grubu veya abonelik etiketlerini almak için [Get-AzTag](/powershell/module/az.resources/get-aztag) komutunu kullanın ve varlık için kaynak kimliğine geçin.

Bir kaynağın etiketlerini görmek için şunları kullanın:

```azurepowershell-interactive
$resource = Get-AzResource -Name demoStorage -ResourceGroup demoGroup
Get-AzTag -ResourceId $resource.id
```

Kaynak grubunun etiketlerini görmek için şunları kullanın:

```azurepowershell-interactive
$resourceGroup = Get-AzResourceGroup -Name demoGroup
Get-AzTag -ResourceId $resourceGroup.ResourceId
```

Aboneliğin etiketlerini görmek için şunları kullanın:

```azurepowershell-interactive
$subscription = (Get-AzSubscription -SubscriptionName "Example Subscription").Id
Get-AzTag -ResourceId "/subscriptions/$subscription"
```

### <a name="list-by-tag"></a>Etikete göre listele

Belirli bir etiket adı ve değeri olan kaynakları elde etmek için şunları kullanın:

```azurepowershell-interactive
(Get-AzResource -Tag @{ "CostCenter"="00123"}).Name
```

Herhangi bir etiket değerine sahip belirli bir etiket adı olan kaynakları elde etmek için şunları kullanın:

```azurepowershell-interactive
(Get-AzResource -TagName "Dept").Name
```

Belirli bir etiket adı ve değeri olan kaynak gruplarını almak için şunları kullanın:

```azurepowershell-interactive
(Get-AzResourceGroup -Tag @{ "CostCenter"="00123" }).ResourceGroupName
```

### <a name="remove-tags"></a>Etiketleri kaldırma

Belirli etiketleri kaldırmak için **Update-AzTag'ı** kullanın ve **-İşlemi** **Sil'e**ayarlayın. Silmek istediğiniz etiketleri geçirin.

```azurepowershell-interactive
$removeTags = @{"Project"="ECommerce"; "Team"="Web"}
Update-AzTag -ResourceId $resource.id -Tag $removeTags -Operation Delete
```

Belirtilen etiketler kaldırılır.

```output
Properties :
        Name        Value
        ==========  =====
        CostCenter  00123
```

Tüm etiketleri kaldırmak için [Kaldır-AzTag](/powershell/module/az.resources/remove-aztag) komutunu kullanın.

```azurepowershell-interactive
$subscription = (Get-AzSubscription -SubscriptionName "Example Subscription").Id
Remove-AzTag -ResourceId "/subscriptions/$subscription"
```

## <a name="azure-cli"></a>Azure CLI

### <a name="apply-tags"></a>Etiketleri uygula

Bir kaynak grubuna veya kaynağa etiketler eklerken, varolan etiketlerin üzerine yazabilir veya varolan etiketlere yeni etiketler ekleyebilirsiniz.

Bir kaynağa etiketlerin üzerine yazmak için şunları kullanın:

```azurecli-interactive
az resource tag --tags 'Dept=IT' 'Environment=Test' -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Kaynaktaki varolan etiketlere etiket eklemek için şunları kullanın:

```azurecli-interactive
az resource update --set tags.'Status'='Approved' -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Kaynak grubundaki varolan etiketlerin üzerine yazmak için şunları kullanın:

```azurecli-interactive
az group update -n examplegroup --tags 'Environment=Test' 'Dept=IT'
```

Kaynak grubundaki varolan etiketlere etiket eklemek için şunları kullanın:

```azurecli-interactive
az group update -n examplegroup --set tags.'Status'='Approved'
```

Şu anda Azure CLI, aboneliklere etiket uygulanmasını desteklemez.

### <a name="list-tags"></a>Etiketleri listeleme

Bir kaynak için varolan etiketleri görmek için şunları kullanın:

```azurecli-interactive
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Bir kaynak grubunun mevcut etiketlerini görmek şunu kullanın:

```azurecli-interactive
az group show -n examplegroup --query tags
```

Bu betik aşağıdaki biçimde veri döndürür:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

### <a name="list-by-tag"></a>Etikete göre listele

Belirli bir etiket ve değere sahip tüm `az resource list`kaynakları elde etmek için şunları kullanın:

```azurecli-interactive
az resource list --tag Dept=Finance
```

Belirli bir etiketi olan kaynak gruplarını almak için şunları kullanın: `az group list`

```azurecli-interactive
az group list --tag Dept=IT
```

### <a name="handling-spaces"></a>Kullanım alanları

Etiket adlarınız veya değerleriniz boşluklar içeriyorsa, birkaç ek adım atmalısınız. Aşağıdaki örnek, etiketler boşluk içerebileceğinde kaynak grubundaki tüm etiketleri kaynaklarına uygular.

```azurecli-interactive
jsontags=$(az group show --name examplegroup --query tags -o json)
tags=$(echo $jsontags | tr -d '{}"' | sed 's/: /=/g' | sed "s/\"/'/g" | sed 's/, /,/g' | sed 's/ *$//g' | sed 's/^ *//g')
origIFS=$IFS
IFS=','
read -a tagarr <<< "$tags"
resourceids=$(az resource list -g examplegroup --query [].id --output tsv)
for id in $resourceids
do
  az resource tag --tags "${tagarr[@]}" --id $id
done
IFS=$origIFS
```

## <a name="templates"></a>Şablonlar

Dağıtım sırasında kaynakları, kaynak gruplarını ve abonelikleri Kaynak Yöneticisi şablonuyla etiketleyebilirsiniz.

### <a name="apply-values"></a>Değerleri uygulama

Aşağıdaki örnekte, üç etiketli bir depolama hesabı dağıtılır. Etiketlerden ikisi`Dept` ( `Environment`ve ) gerçek değerlere ayarlanır. Bir etiket`LastDeployed`( ) geçerli tarihe varsayılan bir parametre olarak ayarlanır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcShort": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": {
                "Dept": "Finance",
                "Environment": "Production",
                "LastDeployed": "[parameters('utcShort')]"
            },
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

### <a name="apply-an-object"></a>Nesne uygulama

Birkaç etiketi depolayan bir nesne parametresi tanımlayabilir ve bu nesneyi etiket öğesine uygulayabilirsiniz. Nesne farklı özelliklere sahip olabileceğinden, bu yaklaşım önceki örnekten daha fazla esneklik sağlar. Nesnedeki her özellik, kaynak için ayrı bir etiket haline gelir. Aşağıdaki örnekte etiket parametresine uygulanan `tagValues` adlı bir parametre kullanılmaktadır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        },
        "tagValues": {
            "type": "object",
            "defaultValue": {
                "Dept": "Finance",
                "Environment": "Production"
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": "[parameters('tagValues')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

### <a name="apply-a-json-string"></a>JSON dizesi uygulama

Çok sayıda değeri tek bir etikete depolamak için, değerleri temsil eden bir JSON dizesi uygulayın. Tüm JSON dizesi, 256 karakteri geçemeyecek tek bir etiket olarak depolanır. Aşağıdaki örnekte bir JSON dizesindeki çok sayıda değeri içeren `CostCenter` adlı tek bir etiket kullanılmaktadır:  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": {
                "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
            },
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

### <a name="apply-tags-from-resource-group"></a>Kaynak grubundan etiketler i

Kaynak grubundaki etiketleri kaynağa uygulamak için [kaynak Grubu](../templates/template-functions-resource.md#resourcegroup) işlevini kullanın. Etiket değerini alırken, bazı `tags[tag-name]` karakterler nokta `tags.tag-name` gösteriminde doğru şekilde ayrışmadığından sözdizimi yerine sözdizimini kullanın.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": {
                "Dept": "[resourceGroup().tags['Dept']]",
                "Environment": "[resourceGroup().tags['Environment']]"
            },
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

### <a name="apply-tags-to-resource-groups-or-subscriptions"></a>Kaynak gruplarına veya aboneliklere etiket uygulama

**Microsoft.Resources/tags** kaynak türünü dağıtarak kaynak grubuna veya aboneye etiket ekleyebilirsiniz. Etiketler, hedef kaynak grubuna veya dağıtım aboneliğine uygulanır. Şablonu her dağıttığınızda, daha önce uygulanmış olan etiketleri değiştirirsiniz.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "tagName": {
            "type": "string",
            "defaultValue": "TeamName"
        },
        "tagValue": {
            "type": "string",
            "defaultValue": "AppTeam1"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/tags",
            "name": "default",
            "apiVersion": "2019-10-01",
            "dependsOn": [],
            "properties": {
                "tags": {
                    "[parameters('tagName')]": "[parameters('tagValue')]"
                }
            }
        }
    ]
}
```

Etiketleri bir kaynak grubuna uygulamak için PowerShell veya Azure CLI'yi kullanın. Etiketlemek istediğiniz kaynak grubuna dağıtın.

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName exampleGroup -TemplateFile https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/tags.json
```

```azurecli-interactive
az deployment group create --resource-group exampleGroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/tags.json
```

Etiketleri bir aboneye uygulamak için PowerShell veya Azure CLI'yi kullanın. Etiketlemek istediğiniz aboneye dağıtın.

```azurepowershell-interactive
New-AzSubscriptionDeployment -name tagresourcegroup -Location westus2 -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/tags.json
```

```azurecli-interactive
az deployment sub create --name tagresourcegroup --location westus2 --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/tags.json
```

Aşağıdaki şablon, bir nesnedeki etiketleri bir kaynak grubuna veya aboneye ekler.

```json
"$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "tags": {
            "type": "object",
            "defaultValue": {
                "TeamName": "AppTeam1",
                "Dept": "Finance",
                "Environment": "Production"
            }
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/tags",
            "name": "default",
            "apiVersion": "2019-10-01",
            "dependsOn": [],
            "properties": {
                "tags": "[parameters('tags')]"
            }
        }
    ]
}
```

## <a name="portal"></a>Portal

[!INCLUDE [resource-manager-tag-resource](../../../includes/resource-manager-tag-resources.md)]

## <a name="rest-api"></a>REST API

Azure REST API'si aracılığıyla etiketlerle çalışmak için şunları kullanın:

* [Etiketler - Kapsamda Oluştur veya Güncelleştir](/rest/api/resources/tags/createorupdateatscope) (PUT işlemi)
* [Etiketler - Kapsamda Güncelleştirme](/rest/api/resources/tags/updateatscope) (YAMA işlemi)
* [Etiketler - Kapsam al](/rest/api/resources/tags/getatscope) (GET işlemi)
* [Etiketler - Sil Kapsamı](/rest/api/resources/tags/deleteatscope) (DELETE işlemi)

## <a name="inherit-tags"></a>Etiketleri devralma

Kaynak grubuna veya aboneye uygulanan etiketler kaynaklar tarafından devralınmıyor. Abonelik veya kaynak grubundaki etiketleri kaynaklara uygulamak için [Azure İlkeleri -](tag-policies.md)etiketleri'ne bakın.

## <a name="tags-and-billing"></a>Etiketler ve faturalandırma

Etiketleri kullanarak faturalama verilerinizi gruplandırabilirsiniz. Örneğin, farklı organizasyonlar için birden çok sanal makine çalıştırıyorsanız, maliyet merkezine göre kullanımı gruplandırmak için etiketleri kullanın. Ayrıca etiketleri kullanarak, üretim ortamında çalışan sanal makineler için faturalama kullanımı gibi, maliyetleri çalışma zamanı ortamına göre kategorilere ayırabilirsiniz.

Azure Kaynak Kullanımı ve [RateCard API'leri](../../billing/billing-usage-rate-card-overview.md) veya kullanım virgülle ayrılmış değerler (CSV) dosyası aracılığıyla etiketler hakkında bilgi alabilirsiniz. Kullanım dosyasını Azure [Hesap Merkezi'nden](https://account.azure.com/Subscriptions) veya Azure portalından indirirsiniz. Daha fazla bilgi için [Azure fatura faturanızı ve günlük kullanım verilerinizi karşıdan yükleme veya görüntüleme](../../billing/billing-download-azure-invoice-daily-usage-date.md)ye bakın. Kullanım dosyasını Azure Hesap Merkezi'nden indirirken **Sürüm 2'yi**seçin. Etiketleri faturalandırmayla destekleyen hizmetler için etiketler **Etiketler** sütununda görünür.

REST API işlemleri için [Azure FaturaLAMA REST API Başvurusu'na](/rest/api/billing/)bakın.

## <a name="limitations"></a>Sınırlamalar

Etiketler için aşağıdaki sınırlamalar geçerlidir:

* Tüm kaynak türleri destek etiketleri değil. Kaynak türüne etiket uygulayıp uygulayamayacağınızı belirlemek [için Azure kaynakları için Etiket desteğine](tag-support.md)bakın.
* Yönetim grupları şu anda etiketleri destekletmiyor.
* Her kaynak, kaynak grubu ve aboneliğin en fazla 50 etiket adı/değer çifti olabilir. İzin verilen maksimum sayıdan daha fazla etiket uygulamanız gerekiyorsa, etiket değeri için bir JSON dizesi kullanın. JSON dizesi, tek etiket adına uygulanan birden fazla değer içerebilir. Bir kaynak grubu veya abonelik, her biri 50 etiket adı/değer çifti olan birçok kaynak içerebilir.
* Etiket adı 512 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır. Depolama hesapları için etiket adı 128 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır.
* Genelleştirilmiş VM'ler etiketleri desteklemez.
* Etiketler Bulut Hizmetleri gibi klasik kaynaklara uygulanamaz.
* Etiket `<`adları bu karakterleri içeremez: `>`, `\`, `?` `%` `&`, , ,`/`

   > [!NOTE]
   > Şu anda Azure DNS bölgeleri ve Traffic Manger hizmetleri de etiketteki boşlukların kullanılmasına izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

* Tüm kaynak türleri destek etiketleri değil. Kaynak türüne etiket uygulayıp uygulayamayacağınızı belirlemek [için Azure kaynakları için Etiket desteğine](tag-support.md)bakın.
* Etiketleme stratejisinin nasıl uygulanacağı yla ilgili öneriler için [Kaynak adlandırma ve etiketleme karar kılavuzuna](/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json)bakın.
