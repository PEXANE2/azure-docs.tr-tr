---
title: 'Öğretici: Özel bir ilke tanımı oluşturma'
description: Bu öğreticide, Azure kaynaklarınızda özel iş kuralları uygulamak için Azure İlkesi için özel bir ilke tanımı hazırlabilirsiniz.
ms.date: 11/25/2019
ms.topic: tutorial
ms.openlocfilehash: f7c303956b209b88ce3c697b5b66243e37071c83
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238947"
---
# <a name="tutorial-create-a-custom-policy-definition"></a>Öğretici: Özel bir ilke tanımı oluşturma

Özel ilke tanımı, müşterilerin Azure'u kullanmak için kendi kurallarını tanımlamasına olanak tanır. Bu kurallar genellikle zorunlu:

- Güvenlik uygulamaları
- Maliyet yönetimi
- Kuruluşa özgü kurallar (adlandırma veya konumlar gibi)

Özel bir ilke oluşturmak için iş sürücüsü ne olursa olsun, adımlar yeni özel ilke tanımlamak için aynıdır.

Özel bir ilke oluşturmadan önce, gereksinimlerinize eşleşen bir ilkenin zaten var olup olmadığını görmek için [ilke örneklerini](../samples/index.md) denetleyin.

Özel bir ilke oluşturma yaklaşımı aşağıdaki adımları izler:

> [!div class="checklist"]
> - İş gereksinimlerinizi belirleyin
> - Her gereksinimi bir Azure kaynak özelliğiyle eşle
> - Özelliği takma adla eşle
> - Hangi efektin kullanılacağını belirleme
> - İlke tanımını oluşturma

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="identify-requirements"></a>Gereksinimleri belirleme

İlke tanımını oluşturmadan önce, ilkenin amacını anlamak önemlidir. Bu öğretici için, ilgili adımları göstermek için amaç olarak ortak bir kurumsal güvenlik gereksinimi kullanacağız:

- HTTPS için her depolama hesabı etkinleştirilmelidir
- HER depolama hesabı HTTP için devre dışı bırakılmalı

Gereksinimleriniz hem "olmak" hem de "olmamak" kaynak durumlarını açıkça tanımlamalıdır.

Kaynağın beklenen durumunu tanımlasak da, uyumlu olmayan kaynaklarla ne yapılmasını istediğimizi henüz tanımlamadık. Azure İlkesi bir dizi [efekti](../concepts/effects.md)destekler. Bu öğretici için, iş kurallarına uygun değilse, kaynakların oluşturulmasını engellemek olarak iş gereksinimini tanımlarız. Bu hedefe ulaşmak [için, Reddet](../concepts/effects.md#deny) efektini kullanacağız. Ayrıca, belirli atamalar için ilkeyi askıya alma seçeneği de istiyoruz. Bu nedenle, [Devre Dışı Bırakma](../concepts/effects.md#disabled) efektini kullanırız ve bu etkiyi ilke tanımında bir [parametre](../concepts/definition-structure.md#parameters) haline getireceğiz.

## <a name="determine-resource-properties"></a>Kaynak özelliklerini belirleme

İş gereksinimine bağlı olarak, Azure İlkesi ile denetlenir Azure kaynağı bir depolama hesabıdır. Ancak, ilke tanımında kullanılacak özellikleri bilmiyoruz. Azure İlkesi kaynağın JSON temsiline karşı değerlendirir, bu nedenle bu kaynaktaki özellikleri anlamamız gerekir.

Azure kaynağının özelliklerini belirlemenin birçok yolu vardır. Bu öğretici için her birine bakacağız:

- VS Code için Azure İlkesi uzantısı
- Resource Manager şablonları
  - Varolan kaynağı dışa aktarma
  - Oluşturma deneyimi
  - Hızlı başlatma şablonları (GitHub)
  - Şablon başvuru dokümanları
- Azure Resource Manager

### <a name="view-resources-in-vs-code-extension"></a>VS Kodu uzantısında kaynakları görüntüleme

[VS Kodu uzantısı,](../how-to/extension-for-vscode.md#search-for-and-view-resources) ortamınızdaki kaynaklara göz atmak ve her kaynaktaki Kaynak Yöneticisi özelliklerini görmek için kullanılabilir.

### <a name="resource-manager-templates"></a>Resource Manager şablonları

Yönetmek istediğiniz özelliği içeren [Kaynak Yöneticisi şablonuna](../../../azure-resource-manager/templates/template-tutorial-create-encrypted-storage-accounts.md) bakmanın birkaç yolu vardır.

#### <a name="existing-resource-in-the-portal"></a>Portaldaki varolan kaynak

Özellikleri bulmanın en basit yolu, aynı türdeki varolan bir kaynağa bakmaktır. Zorlamak istediğiniz ayarla zaten yapılandırılan kaynaklar, karşılaştırılabilir değeri de sağlar.
Belirli bir kaynak için Azure portalındaki Dışa Aktarma **şablonu** sayfasına **(Ayarlar**altında) bakın.

![Varolan kaynakta şablon sayfasını dışa aktarma](../media/create-custom-policy-definition/export-template.png)

Bir depolama hesabı için bunu yapmak, bu örneğe benzer bir şablon ortaya çıkarır:

```json
...
"resources": [{
    "comments": "Generalized from resource: '/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount'.",
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
    },
    "kind": "Storage",
    "name": "[parameters('storageAccounts_mystorageaccount_name')]",
    "apiVersion": "2018-07-01",
    "location": "westus",
    "tags": {
        "ms-resource-usage": "azure-cloud-shell"
    },
    "scale": null,
    "properties": {
        "networkAcls": {
            "bypass": "AzureServices",
            "virtualNetworkRules": [],
            "ipRules": [],
            "defaultAction": "Allow"
        },
        "supportsHttpsTrafficOnly": false,
        "encryption": {
            "services": {
                "file": {
                    "enabled": true
                },
                "blob": {
                    "enabled": true
                }
            },
            "keySource": "Microsoft.Storage"
        }
    },
    "dependsOn": []
}]
...
```

**Özellikleri** altında bir değer **desteklerHttpsTrafficOnly** **yanlış**ayarlanır . Bu mülk aradığımız mülk olabilir. Ayrıca, kaynağın **türü** **Microsoft.Storage/storageAccounts'tır.** Tür, ilkeyi yalnızca bu tür kaynaklarla sınırlamamıza sağlar.

#### <a name="create-a-resource-in-the-portal"></a>Portalda kaynak oluşturma

Portal aracılığıyla başka bir yolu kaynak oluşturma deneyimidir. Portal üzerinden bir depolama hesabı oluştururken, **Gelişmiş** sekmesi altında bir seçenek **Güvenlik transferi gereklidir.** Bu özellik _Devre Dışı ve_ _Etkin_ seçenekleri vardır. Bilgi simgesi, bu seçeneği niçin istediğimiz özellik olduğunu doğrulayan ek bir metine sahiptir. Ancak, portal bize bu ekranda özellik adını söylemez.

Gözden **Geçirme + oluştur** sekmesinde, **otomasyon için şablon indirmek için**sayfanın alt kısmında bir bağlantı bulunur. Bağlantıyı seçmek, yapılandırdığımız kaynağı oluşturan şablonu açar. Bu durumda, iki önemli bilgi parçası görürsünüz:

```json
...
"supportsHttpsTrafficOnly": {
    "type": "bool"
}
...
"properties": {
    "accessTier": "[parameters('accessTier')]",
    "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
}
...
```

Bu bilgiler bize özellik türünü söyler ve aynı zamanda **destekleri onaylarHttpsTrafficOnly** aradığımız özelliktir.

#### <a name="quickstart-templates-on-github"></a>GitHub'da Hızlı Başlatma şablonları

GitHub'daki [Azure hızlı başlatma şablonlarında](https://github.com/Azure/azure-quickstart-templates) farklı kaynaklar için oluşturulmuş yüzlerce Kaynak Yöneticisi şablonu vardır. Bu şablonlar, aradığınız kaynak özelliğini bulmak için harika bir yol olabilir. Bazı özellikler aradığınız şey gibi görünebilir, ancak başka bir şeyi kontrol edin.

#### <a name="resource-reference-docs"></a>Kaynak başvuru dokümanları

Destekleri doğrulamak **içinHttpsTrafficOnly** doğru özelliktir, depolama sağlayıcısındaki [depolama hesabı kaynağı](/azure/templates/microsoft.storage/2018-07-01/storageaccounts) için Kaynak Yöneticisi şablon ubaşvurusuna bakın.
Özellikler nesnesi geçerli parametrelerin bir listesi vardır. [StorageAccountPropertiesCreateParameters-object](/azure/templates/microsoft.storage/2018-07-01/storageaccounts#storageaccountpropertiescreateparameters-object) bağlantısını seçmek, kabul edilebilir özellikler tablosunu gösterir. **desteklerHttpsTrafficOnly** mevcut ve açıklama iş gereksinimlerini karşılamak için ne aradığımız eşleşir.

### <a name="azure-resource-explorer"></a>Azure Resource Manager

Azure kaynaklarınızı keşfetmenin bir diğer yolu da [Azure Kaynak Gezgini](https://resources.azure.com) 'nden (Önizleme) geçer. Bu araç aboneliğinizin bağlamını kullanır, bu nedenle Azure kimlik bilgilerinizle web sitesine kimlik doğrulamanız gerekir. Kimlik doğrulaması alındıktan sonra sağlayıcılara, aboneliklere, kaynak gruplarına ve kaynaklara göre göz atabilirsiniz.

Bir depolama hesabı kaynağı bulun ve özelliklerine bakın. Biz de burada **desteklerHttpsTrafficOnly** özelliği bakın. **Belgeler** sekmesini seçerek, özellik açıklamasının başvuru belgelerinde daha önce bulduklarla eşleştiğini görüyoruz.

## <a name="find-the-property-alias"></a>Özellik takma adını bulma

Kaynak özelliğini belirledik, ancak bu özelliği bir [takma adla](../concepts/definition-structure.md#aliases)haritalamalıyız.

Azure kaynağının diğer adlarını belirlemenin birkaç yolu vardır. Bu öğretici için her birine bakacağız:

- VS Code için Azure İlkesi uzantısı
- Azure CLI
- Azure PowerShell
- Azure Kaynak Grafiği

### <a name="get-aliases-in-vs-code-extension"></a>VS Kodu uzantısında diğer adları alma

VS Code uzantısı için Azure İlkesi uzantısı, kaynaklarınıza göz atmanızı ve [takma adları keşfetmenizi](../how-to/extension-for-vscode.md#discover-aliases-for-resource-properties)kolaylaştırır.

### <a name="azure-cli"></a>Azure CLI

Azure CLI'de `az provider` komut grubu kaynak takma adlarını aramak için kullanılır. Azure kaynağı hakkında daha önce aldığımız ayrıntılara göre **Microsoft.Storage** ad alanını filtreleriz yaparız.

```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Get Azure Policy aliases for type Microsoft.Storage
az provider show --namespace Microsoft.Storage --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
```

Sonuçlarda, **supporthttpsTrafficOnly**adlı depolama hesapları tarafından desteklenen bir takma ad görüyoruz. Bu takma adın varlığı, iş gereksinimlerimizi uygulamak için politika yazabileceğimiz anlamına gelir!

### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell'de `Get-AzPolicyAlias` cmdlet kaynak takma adlarını aramak için kullanılır. Azure kaynağı hakkında daha önce aldığımız ayrıntılara göre **Microsoft.Storage** ad alanını filtreleriz yaparız.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Use Get-AzPolicyAlias to list aliases for Microsoft.Storage
(Get-AzPolicyAlias -NamespaceMatch 'Microsoft.Storage').Aliases
```

Azure CLI gibi sonuçlar da **httpsTrafficOnly adlı**depolama hesapları tarafından desteklenen bir diğer ad gösterir.

### <a name="azure-resource-graph"></a>Azure Kaynak Grafiği

[Azure Kaynak Grafiği,](../../resource-graph/overview.md) Azure kaynaklarının özelliklerini bulmak için başka bir yöntem sağlayan bir hizmettir. Kaynak Grafiği ile tek bir depolama hesabına bakmak için örnek bir sorgu aşağıda veda edebilirsiniz:

```kusto
Resources
| where type=~'microsoft.storage/storageaccounts'
| limit 1
```

```azurecli-interactive
az graph query -q "Resources | where type=~'microsoft.storage/storageaccounts' | limit 1"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type=~'microsoft.storage/storageaccounts' | limit 1"
```

Sonuçlar, Kaynak Yöneticisi şablonlarında ve Azure Kaynak Gezgini'nde gördüğümüze benzer. Ancak, Azure Kaynak Grafiği sonuçları, _diğer addizisini_ _yansıtarak_ [diğer ad](../concepts/definition-structure.md#aliases) ayrıntılarını da içerebilir:

```kusto
Resources
| where type=~'microsoft.storage/storageaccounts'
| limit 1
| project aliases
```

```azurecli-interactive
az graph query -q "Resources | where type=~'microsoft.storage/storageaccounts' | limit 1 | project aliases"
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type=~'microsoft.storage/storageaccounts' | limit 1 | project aliases"
```

Diğer adlar için bir depolama hesabından örnek çıktı aşağıda verilmiştir:

```json
"aliases": {
    "Microsoft.Storage/storageAccounts/accessTier": null,
    "Microsoft.Storage/storageAccounts/accountType": "Standard_LRS",
    "Microsoft.Storage/storageAccounts/enableBlobEncryption": true,
    "Microsoft.Storage/storageAccounts/enableFileEncryption": true,
    "Microsoft.Storage/storageAccounts/encryption": {
        "keySource": "Microsoft.Storage",
        "services": {
            "blob": {
                "enabled": true,
                "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
            },
            "file": {
                "enabled": true,
                "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
            }
        }
    },
    "Microsoft.Storage/storageAccounts/encryption.keySource": "Microsoft.Storage",
    "Microsoft.Storage/storageAccounts/encryption.keyvaultproperties.keyname": null,
    "Microsoft.Storage/storageAccounts/encryption.keyvaultproperties.keyvaulturi": null,
    "Microsoft.Storage/storageAccounts/encryption.keyvaultproperties.keyversion": null,
    "Microsoft.Storage/storageAccounts/encryption.services": {
        "blob": {
            "enabled": true,
            "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
        },
        "file": {
            "enabled": true,
            "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
        }
    },
    "Microsoft.Storage/storageAccounts/encryption.services.blob": {
        "enabled": true,
        "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
    },
    "Microsoft.Storage/storageAccounts/encryption.services.blob.enabled": true,
    "Microsoft.Storage/storageAccounts/encryption.services.file": {
        "enabled": true,
        "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
    },
    "Microsoft.Storage/storageAccounts/encryption.services.file.enabled": true,
    "Microsoft.Storage/storageAccounts/networkAcls": {
        "bypass": "AzureServices",
        "defaultAction": "Allow",
        "ipRules": [],
        "virtualNetworkRules": []
    },
    "Microsoft.Storage/storageAccounts/networkAcls.bypass": "AzureServices",
    "Microsoft.Storage/storageAccounts/networkAcls.defaultAction": "Allow",
    "Microsoft.Storage/storageAccounts/networkAcls.ipRules": [],
    "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]": [],
    "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].action": [],
    "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*]": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*].action": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*].id": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*].state": [],
    "Microsoft.Storage/storageAccounts/primaryEndpoints": {
        "blob": "https://mystorageaccount.blob.core.windows.net/",
        "file": "https://mystorageaccount.file.core.windows.net/",
        "queue": "https://mystorageaccount.queue.core.windows.net/",
        "table": "https://mystorageaccount.table.core.windows.net/"
    },
    "Microsoft.Storage/storageAccounts/primaryEndpoints.blob": "https://mystorageaccount.blob.core.windows.net/",
    "Microsoft.Storage/storageAccounts/primaryEndpoints.file": "https://mystorageaccount.file.core.windows.net/",
    "Microsoft.Storage/storageAccounts/primaryEndpoints.queue": "https://mystorageaccount.queue.core.windows.net/",
    "Microsoft.Storage/storageAccounts/primaryEndpoints.table": "https://mystorageaccount.table.core.windows.net/",
    "Microsoft.Storage/storageAccounts/primaryEndpoints.web": null,
    "Microsoft.Storage/storageAccounts/primaryLocation": "eastus2",
    "Microsoft.Storage/storageAccounts/provisioningState": "Succeeded",
    "Microsoft.Storage/storageAccounts/sku.name": "Standard_LRS",
    "Microsoft.Storage/storageAccounts/sku.tier": "Standard",
    "Microsoft.Storage/storageAccounts/statusOfPrimary": "available",
    "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly": false
}
```

Azure Kaynak Grafiği [Bulut Bulutu](https://shell.azure.com)aracılığıyla kullanılabilir ve kaynaklarınızın özelliklerini keşfetmenin hızlı ve kolay bir yoludur.

## <a name="determine-the-effect-to-use"></a>Kullanılacak efekti belirleme

Uyumlu olmayan kaynaklarınızla ne yapacağınıza karar vermek, ilk etapta neyi değerlendireceklerine karar vermek kadar önemlidir. Uyumlu olmayan bir kaynağa verilen her olası yanıta [efekt](../concepts/effects.md)denir. Uyumlu olmayan kaynak günlüğe kaydedilmiş, engellenmiş, veri eklenmiş veya kaynağı uyumlu bir duruma geri koymak için bununla ilişkili bir dağıtım varsa etki denetler.

Örneğin, Azure ortamımızda uyumlu olmayan kaynakların oluşturulmasını istemediğimiz için Reddet istediğimiz etkidir. Denetim, bir ilkenin Reddine ayarlanmasından önce etkinin ne olduğunu belirlemek için bir ilke efekti için iyi bir ilk seçimdir. Atama başına efekti değiştirmeyi kolaylaştıran bir yol, efekti parametrelendirmektir. Nasıl olduğu yla ilgili ayrıntılar için aşağıdaki [parametrelere](#parameters) bakın.

## <a name="compose-the-definition"></a>Tanımı oluşturma

Artık yönetmeyi planladığımız şey için özellik ayrıntıları ve takma adlarımız var. Sonra, politika kuralının kendisini oluşturacağız. İlke dilini henüz bilmiyorsanız, ilke [tanımının](../concepts/definition-structure.md) nasıl yapılandırılabildiğini belirtin. Bir ilke tanımının nasıl göründüğüne ait boş bir şablon aşağıda verilmiştir:

```json
{
    "properties": {
        "displayName": "<displayName>",
        "description": "<description>",
        "mode": "<mode>",
        "parameters": {
                <parameters>
        },
        "policyRule": {
            "if": {
                <rule>
            },
            "then": {
                "effect": "<effect>"
            }
        }
    }
}
```

### <a name="metadata"></a>Meta Veriler

İlk üç bileşen ilke meta verileridir. Bu bileşenler için ne kuralı nı oluşturduğumuzu bildiğimiz için değerler sağlamak kolaydır. [Mod](../concepts/definition-structure.md#mode) öncelikle etiketler ve kaynak konumu hakkındadır. Değerlendirmeyi etiketleri destekleyen kaynaklarla sınırlamamız gerekmediğinden, **mod**için _tüm_ değeri kullanırız.

```json
"displayName": "Deny storage accounts not using only HTTPS",
"description": "Deny storage accounts not using only HTTPS. Checks the supportsHttpsTrafficOnly property on StorageAccounts.",
"mode": "all",
```

### <a name="parameters"></a>Parametreler

Değerlendirmeyi değiştirmek için bir parametre kullanmamış olsak da, sorun giderme **efektinin** değiştirilmesine izin vermek için bir parametre kullanmak istiyoruz. Bir **effectType** parametresi tanımlayacağız ve yalnızca **Reddet** ve **Devre Dışı bırakılabilmek**için sınırlandıracağız. Bu iki seçenek iş gereksinimlerimize uygundur. Bitmiş parametreler bloğu aşağıdaki örnek gibi görünür:

```json
"parameters": {
    "effectType": {
        "type": "string",
        "defaultValue": "Deny",
        "allowedValues": [
            "Deny",
            "Disabled"
        ],
        "metadata": {
            "displayName": "Effect",
            "description": "Enable or disable the execution of the policy"
        }
    }
},
```

### <a name="policy-rule"></a>İlke kuralı

[İlke kuralını](../concepts/definition-structure.md#policy-rule) oluşturmak, özel ilke tanımımızı oluşturmanın son adımıdır. Test etmek için iki ifade belirledik:

- Depolama hesabı **türünün** **Microsoft.Storage/storageAccounts** olduğunu
- Depolama **hesabının desteklediğiHttpsTrafficOnly** **doğru** değil

Bu ifadelerin her ikisinin de doğru olması gerektiğinden, **tüm** [Mantıksal işleci](../concepts/definition-structure.md#logical-operators)kullanacağız. Statik bir bildirim yapmak yerine **effectType** parametresini efekte aktarırız. Bitmiş kuralımız şu örneğe benzer:

```json
"if": {
    "allOf": [
        {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        },
        {
            "field": "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly",
            "notEquals": "true"
        }
    ]
},
"then": {
    "effect": "[parameters('effectType')]"
}
```

### <a name="completed-definition"></a>Tamamlanmış tanım

İlkenin üç bölümü de tanımlandığı için, tamamlanmış tanımımız aşağıda verilmiştir:

```json
{
    "properties": {
        "displayName": "Deny storage accounts not using only HTTPS",
        "description": "Deny storage accounts not using only HTTPS. Checks the supportsHttpsTrafficOnly property on StorageAccounts.",
        "mode": "all",
        "parameters": {
            "effectType": {
                "type": "string",
                "defaultValue": "Deny",
                "allowedValues": [
                    "Deny",
                    "Disabled"
                ],
                "metadata": {
                    "displayName": "Effect",
                    "description": "Enable or disable the execution of the policy"
                }
            }
        },
        "policyRule": {
            "if": {
                "allOf": [
                    {
                        "field": "type",
                        "equals": "Microsoft.Storage/storageAccounts"
                    },
                    {
                        "field": "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly",
                        "notEquals": "true"
                    }
                ]
            },
            "then": {
                "effect": "[parameters('effectType')]"
            }
        }
    }
}
```

Tamamlanan tanım yeni bir ilke oluşturmak için kullanılabilir. Portal ve her SDK (Azure CLI, Azure PowerShell ve REST API) tanımı farklı şekillerde kabul eder, bu nedenle doğru kullanımı doğrulamak için her birinin komutlarını gözden geçirin. Ardından, parametreleştirilmiş efekti kullanarak depolama hesaplarınızın güvenliğini yönetmek için uygun kaynaklara atayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticideki kaynaklarla çalışmayı bitirdiyseniz, yukarıda oluşturulan atamaları veya tanımları silmek için aşağıdaki adımları kullanın:

1. Azure İlkesi sayfasının sol tarafında **Yazma** altında **Tanımlar** 'ı (veya bir atamayı silmeye çalışıyorsanız **Atamalar)** seçin.

1. Kaldırmak istediğiniz yeni girişim veya tanımını (ya da atamayı) arayın.

1. Satıra sağ tıklayın ya da tanımın (veya atamanın) sonundaki üç noktayı seçip **Tanımı sil** (veya **Atamayı sil**) öğesini seçin.

## <a name="review"></a>Gözden geçirme

Bu öğreticide, aşağıdaki görevleri başarıyla gerçekleştirdiniz:

> [!div class="checklist"]
> - İş gereksinimlerinizi belirleme
> - Her gereksinimi bir Azure kaynak özelliğiyle eşledi
> - Özelliği bir takma adla eşledi
> - Kullanılacak efekti belirleme
> - İlke tanımını besteledi

## <a name="next-steps"></a>Sonraki adımlar

Ardından, bir ilke oluşturmak ve atamak için özel ilke tanımınızı kullanın:

> [!div class="nextstepaction"]
> [İlke tanımı oluşturma ve atama](../how-to/programmatically-create.md#create-and-assign-a-policy-definition)