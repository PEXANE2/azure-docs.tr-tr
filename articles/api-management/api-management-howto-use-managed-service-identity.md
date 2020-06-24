---
title: Azure API Management Yönetilen kimlikler kullanma | Microsoft Docs
description: API Management ' de yönetilen kimlikler kullanmayı öğrenin
services: api-management
documentationcenter: ''
author: miaojiang
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 06/12/2020
ms.author: apimpm
ms.openlocfilehash: 028b26537c9fe8a976dbc68a4776b2ea4101d811
ms.sourcegitcommit: 6571e34e609785e82751f0b34f6237686470c1f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84789453"
---
# <a name="use-managed-identities-in-azure-api-management"></a>Azure API Management Yönetilen kimlikler kullanma

Bu makalede, bir API Management örneği için yönetilen kimlik oluşturma ve diğer kaynaklara erişme gösterilmektedir. Azure Active Directory tarafından oluşturulan yönetilen bir kimlik (Azure AD), API Management örneğinizin Azure Key Vault gibi diğer Azure AD korumalı kaynaklarına kolayca ve güvenli bir şekilde erişmesini sağlar. Bu kimlik Azure tarafından yönetilir ve tüm gizli dizileri sağlamanıza veya döndürmenize gerek yoktur. Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md).

Bir API Management örneğine iki tür kimlik verilebilir:

- **Sistem tarafından atanan bir kimlik** hizmetinize bağlanır ve hizmetiniz silinirse silinir. Hizmette yalnızca bir sistem tarafından atanmış kimlik olabilir.
- **Kullanıcı tarafından atanan bir kimlik** , hizmetinize atanabilecek tek başına bir Azure kaynağıdır. Hizmette birden çok kullanıcı tarafından atanan kimlik (*) bulunabilir.

## <a name="create-a-system-assigned-managed-identity-for-an-api-management-instance"></a>API Management örneği için sistem tarafından atanan yönetilen kimlik oluşturma

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Portalda yönetilen bir kimlik ayarlamak için öncelikle normal olarak bir API Management örneği oluşturup sonra özelliği etkinleştirmeniz gerekir.

1. Portalda genellikle yaptığınız gibi bir API Management örneği oluşturun. Portalda bu sayfaya gidin.
2. **Yönetilen kimlikler**' i seçin.
3. **Sistem atandı** sekmesinde **durumu** **Açık**olarak değiştirin. **Kaydet**’e tıklayın.

    :::image type="content" source="./media/api-management-msi/enable-system-msi.png" alt-text="Sistem tarafından atanan yönetilen kimliği etkinleştirin." border="true":::


### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıdaki adımlar, bir API Management örneği oluşturma ve Azure PowerShell kullanarak bir kimlik atama işleminde size yol gösterecektir. 

1. Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/install-az-ps)bulunan yönergeleri kullanarak Azure PowerShell yükleyip `Connect-AzAccount` Azure ile bağlantı oluşturmak için öğesini çalıştırın.

2. Azure PowerShell kullanarak API Management bir örnek oluşturun. API Management örneği ile Azure PowerShell kullanma hakkında daha fazla örnek için bkz. [API Management PowerShell örnekleri](powershell-samples.md):

    ```azurepowershell-interactive
    # Create a resource group.
    New-AzResourceGroup -Name $resourceGroupName -Location $location

    # Create an API Management Consumption Sku service.
    New-AzApiManagement -ResourceGroupName $resourceGroupName -Name consumptionskuservice -Location $location -Sku Consumption -Organization contoso -AdminEmail contoso@contoso.com -SystemAssignedIdentity
    ```

3. Kimliği oluşturmak için ve var olan örneği güncelleştirin:

    ```azurepowershell-interactive
    # Get an API Management instance
    $apimService = Get-AzApiManagement -ResourceGroupName $resourceGroupName -Name $apiManagementName

    # Update an API Management instance
    Set-AzApiManagement -InputObject $apimService -SystemAssignedIdentity
    ```

### <a name="using-the-azure-resource-manager-template"></a>Azure Resource Manager şablonunu kullanma

Kaynak tanımına aşağıdaki özelliği ekleyerek bir kimlik ile API Management örneği oluşturabilirsiniz:

```json
"identity" : {
    "type" : "SystemAssigned"
}
```

Bu, Azure 'ın API Management örneğiniz için kimlik oluşturmasını ve yönetmesini söyler.

Örneğin, Azure Resource Manager bir şablon aşağıdaki gibi görünebilir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "resources": [{
        "apiVersion": "2019-01-01",
        "name": "contoso",
        "type": "Microsoft.ApiManagement/service",
        "location": "[resourceGroup().location]",
        "tags": {},
        "sku": {
            "name": "Developer",
            "capacity": "1"
        },
        "properties": {
            "publisherEmail": "admin@contoso.com",
            "publisherName": "Contoso"
        },
        "identity": {
            "type": "systemAssigned"
        }
    }]
}
```

Örnek oluşturulduğunda, aşağıdaki ek özelliklere sahiptir:

```json
"identity": {
    "type": "SystemAssigned",
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

Tenantıd özelliği, kimliğin ait olduğu Azure AD kiracısını tanımlar. PrincipalId, örnek yeni kimlik için benzersiz bir tanımlayıcıdır. Azure AD 'de hizmet sorumlusu, API Management örneğinize verdiğiniz aynı ada sahiptir.


> [!NOTE]
> Bir API Management örneği aynı anda hem sistem tarafından hem de Kullanıcı tarafından atanan kimliklere sahip olabilir. Bu durumda, `type` özelliği`SystemAssigned,UserAssigned`

### <a name="scenarios-supported"></a>Desteklenen senaryolar

#### <a name="obtain-a-custom-tlsssl-certificate-for-the-api-management-instance-from-azure-key-vault"></a><a name="use-ssl-tls-certificate-from-azure-key-vault"></a>Azure Key Vault API Management örneği için özel bir TLS/SSL sertifikası alın
API Management bir hizmetin sistem tarafından atanan kimliği, Azure Key Vault depolanan özel TLS/SSL sertifikalarını almak için kullanılabilir. Bu sertifikalar, API Management örneğindeki özel etki alanlarına atanabilir.

1. Gizli dizi Içerik türü *Application/x-PKCS12*olmalıdır.
2. Gerçek gizli anahtarı içeren Key Vault sertifika gizli uç noktası kullanılmalıdır.

> [!Important]
> Sertifikanın nesne sürümü sağlanmazsa API Management, sertifikanın 4 saat içinde Key Vault karşıya yüklendikten sonra otomatik olarak yeni sürümünü elde eder

Aşağıdaki örnekte aşağıdaki adımları içeren bir Azure Resource Manager şablonu gösterilmektedir:

1. Yönetilen kimliğe sahip bir API Management örneği oluşturun.
2. Bir Azure Key Vault örneğinin erişim ilkelerini güncelleştirin ve API Management örneğinin bundan gizli dizileri almasına izin verin.
3. Key Vault örneğinden bir sertifika aracılığıyla özel bir etki alanı adı ayarlayarak API Management örneğini güncelleştirin.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publisherEmail": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "The email address of the owner of the service"
            }
        },
        "publisherName": {
            "type": "string",
            "defaultValue": "Contoso",
            "minLength": 1,
            "metadata": {
                "description": "The name of the owner of the service"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": ["Developer",
            "Standard",
            "Premium"],
            "defaultValue": "Developer",
            "metadata": {
                "description": "The pricing tier of this API Management instance"
            }
        },
        "skuCount": {
            "type": "int",
            "defaultValue": 1,
            "metadata": {
                "description": "The instance size of this API Management instance."
            }
        },
        "keyVaultName": {
            "type": "string",
            "metadata": {
                "description": "Name of the vault"
            }
        },
        "proxyCustomHostname1": {
            "type": "string",
            "metadata": {
                "description": "Proxy Custom hostname."
            }
        },
        "keyVaultIdToCertificate": {
            "type": "string",
            "metadata": {
                "description": "Reference to the KeyVault certificate. https://contoso.vault.azure.net/secrets/contosogatewaycertificate."
            }
        }
    },
    "variables": {
        "apiManagementServiceName": "[concat('apiservice', uniqueString(resourceGroup().id))]",
        "apimServiceIdentityResourceId": "[concat(resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName')),'/providers/Microsoft.ManagedIdentity/Identities/default')]"
    },
    "resources": [{
        "apiVersion": "2019-01-01",
        "name": "[variables('apiManagementServiceName')]",
        "type": "Microsoft.ApiManagement/service",
        "location": "[resourceGroup().location]",
        "tags": {
        },
        "sku": {
            "name": "[parameters('sku')]",
            "capacity": "[parameters('skuCount')]"
        },
        "properties": {
            "publisherEmail": "[parameters('publisherEmail')]",
            "publisherName": "[parameters('publisherName')]"
        },
        "identity": {
            "type": "systemAssigned"
        }
    },
    {
        "type": "Microsoft.KeyVault/vaults/accessPolicies",
        "name": "[concat(parameters('keyVaultName'), '/add')]",
        "apiVersion": "2015-06-01",
        "dependsOn": [
            "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
        ],
        "properties": {
            "accessPolicies": [{
                "tenantId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').tenantId]",
                "objectId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').principalId]",
                "permissions": {
                    "secrets": ["get"]
                }
            }]
        }
    },
    {
        "apiVersion": "2017-05-10",
        "name": "apimWithKeyVault",
        "type": "Microsoft.Resources/deployments",
        "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
        ],
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "https://raw.githubusercontent.com/solankisamir/arm-templates/master/basicapim.keyvault.json",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
                "publisherEmail": { "value": "[parameters('publisherEmail')]"},
                "publisherName": { "value": "[parameters('publisherName')]"},
                "sku": { "value": "[parameters('sku')]"},
                "skuCount": { "value": "[parameters('skuCount')]"},
                "proxyCustomHostname1": {"value" : "[parameters('proxyCustomHostname1')]"},
                "keyVaultIdToCertificate": {"value" : "[parameters('keyVaultIdToCertificate')]"}
            }
        }
    }]
}
```

#### <a name="authenticate-using-api-management-identity-to-the-backend"></a>API Management kimliğini arka uca kullanarak kimlik doğrulama

Sistem tarafından atanan kimlik, [kimlik doğrulaması ile yönetilen](api-management-authentication-policies.md#ManagedIdentity) kimlik ilkesi kullanılarak arka ucunuza kimlik doğrulaması yapmak için kullanılabilir.


## <a name="create-a-user-assigned-managed-identity-for-an-api-management-instance"></a>API Management örneği için Kullanıcı tarafından atanan yönetilen kimlik oluşturma

> [!NOTE]
> API Management bir örnek, Kullanıcı tarafından atanan yönetilen kimlik ile ilişkilendirilebilir.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Portalda yönetilen bir kimlik ayarlamak için öncelikle normal olarak bir API Management örneği oluşturup sonra özelliği etkinleştirmeniz gerekir.

1. Portalda genellikle yaptığınız gibi bir API Management örneği oluşturun. Portalda bu sayfaya gidin.
2. **Yönetilen kimlikler**' i seçin.
3. **Kullanıcı atandı** sekmesinde **Ekle**' ye tıklayın.
4. Daha önce oluşturduğunuz kimliği arayın ve seçin. **Ekle**'ye tıklayın.

   :::image type="content" source="./media/api-management-msi/enable-user-assigned-msi.png" alt-text="Kullanıcı tarafından atanan yönetilen kimliği etkinleştirin." border="true":::

### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıdaki adımlar, bir API Management örneği oluşturma ve Azure PowerShell kullanarak bir kimlik atama işleminde size yol gösterecektir. 

1. Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/install-az-ps)bulunan yönergeleri kullanarak Azure PowerShell yükleyip `Connect-AzAccount` Azure ile bağlantı oluşturmak için öğesini çalıştırın.

2. Azure PowerShell kullanarak API Management bir örnek oluşturun. API Management örneği ile Azure PowerShell kullanma hakkında daha fazla örnek için bkz. [API Management PowerShell örnekleri](powershell-samples.md):

    ```azurepowershell-interactive
    # Create a resource group.
    New-AzResourceGroup -Name $resourceGroupName -Location $location

    # Create a user-assigned identity. This requires installation of the "Az.ManagedServiceIdentity" module.
    $userAssignedIdentity = New-AzUserAssignedIdentity -Name $userAssignedIdentityName -ResourceGroupName $resourceGroupName

    # Create an API Management Consumption Sku service.
    $userIdentities = @($userAssignedIdentity.Id)

    New-AzApiManagement -ResourceGroupName $resourceGroupName -Location $location -Name $apiManagementName -Organization contoso -AdminEmail admin@contoso.com -Sku Consumption -UserAssignedIdentity $userIdentities
    ```

3. Hizmete bir kimlik atamak için ve mevcut hizmeti güncelleştirin.

    ```azurepowershell-interactive
    # Get an API Management instance
    $apimService = Get-AzApiManagement -ResourceGroupName $resourceGroupName -Name $apiManagementName

    # Create a user-assigned identity. This requires installation of the "Az.ManagedServiceIdentity" module.
    $userAssignedIdentity = New-AzUserAssignedIdentity -Name $userAssignedIdentityName -ResourceGroupName $resourceGroupName

    # Update an API Management instance
    $userIdentities = @($userAssignedIdentity.Id)
    Set-AzApiManagement -InputObject $apimService -UserAssignedIdentity $userIdentities
    ```

### <a name="using-the-azure-resource-manager-template"></a>Azure Resource Manager şablonunu kullanma

Kaynak tanımına aşağıdaki özelliği ekleyerek bir kimlik ile API Management örneği oluşturabilirsiniz:

```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "<RESOURCEID>": {}
    }
}
```

Kullanıcı tarafından atanan türü eklemek, Azure 'un örneğiniz için belirtilen kullanıcı tarafından atanan kimliğini kullanmasını söyler.

Örneğin, Azure Resource Manager bir şablon aşağıdaki gibi görünebilir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "resources": [{
        "apiVersion": "2019-12-01",
        "name": "contoso",
        "type": "Microsoft.ApiManagement/service",
        "location": "[resourceGroup().location]",
        "tags": {},
        "sku": {
            "name": "Developer",
            "capacity": "1"
        },
        "properties": {
            "publisherEmail": "admin@contoso.com",
            "publisherName": "Contoso"
        },
        "identity": {
            "type": "UserAssigned",
             "userAssignedIdentities": {
                "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]": {}
             }
        },
        "dependsOn": [       
          "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]"
        ]
    }]
}
```

Hizmet oluşturulduğunda, aşağıdaki ek özelliklere sahiptir:

```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "<RESOURCEID>": {
            "principalId": "<PRINCIPALID>",
            "clientId": "<CLIENTID>"
        }
    }
}
```

PrincipalId, Azure AD yönetimi için kullanılan kimlik için benzersiz bir tanımlayıcıdır. ClientID, uygulamanın çalışma zamanı çağrıları sırasında hangi kimliğin kullanılacağını belirtmek için kullanılan yeni kimliği için benzersiz bir tanımlayıcıdır.

> [!NOTE]
> Bir API Management örneği aynı anda hem sistem tarafından hem de Kullanıcı tarafından atanan kimliklere sahip olabilir. Bu durumda, `type` özelliği`SystemAssigned,UserAssigned`

### <a name="scenarios-supported"></a>Desteklenen senaryolar

#### <a name="authenticate-using-user-assigned-identity-to-the-backend"></a>Kullanıcı tarafından atanan kimliği kullanarak arka uca kimlik doğrulama

Kullanıcı tarafından atanan kimlik, [kimlik doğrulaması ile yönetilen](api-management-authentication-policies.md#ManagedIdentity) kimlik ilkesini kullanarak arka ucunuza kimlik doğrulaması yapmak için kullanılabilir.


## <a name="remove-an-identity"></a><a name="remove"></a>Kimlik kaldırma

Portal veya Azure Resource Manager şablonu kullanılarak oluşturulduğu gibi özellik devre dışı bırakılarak sistem tarafından atanan bir kimlik kaldırılabilir. Kullanıcı tarafından atanan kimlikler tek tek kaldırılabilir. Tüm kimlikleri kaldırmak için kimlik türünü "none" olarak ayarlayın.

Sistem tarafından atanan bir kimliğin bu şekilde kaldırılması, Azure AD 'den de silinecek. API Management örneği silindiğinde, sistem tarafından atanan kimlikler de Azure AD 'den otomatik olarak kaldırılır.

Azure Resource Manager şablonu kullanarak tüm kimlikleri kaldırmak için bu bölümü güncelleştirin:

```json
"identity": {
    "type": "None"
}
```

> [!Important]
> Bir API Management örnek, anahtar kasasından özel SSL sertifikası ile yapılandırıldıysa ve yönetilen kimliği devre dışı bırakmak için girişim yapılırsa istek başarısız olur.
> Müşteri, Azure Key Vault sertifikasından satır içi kodlanmış sertifika sağlayarak ve sonra yönetilen kimliği devre dışı bırakarak kendi engellemesini kaldırabilir. [Özel etki alanını yapılandırma](configure-custom-domain.md) bölümüne bakın

## <a name="next-steps"></a>Sonraki adımlar

Azure kaynakları için Yönetilen kimlikler hakkında daha fazla bilgi edinin:

* [Azure kaynakları için Yönetilen kimlikler nelerdir?](../active-directory/managed-identities-azure-resources/overview.md)
* [Azure Resource Manager şablonları](https://github.com/Azure/azure-quickstart-templates)
* [İlkede yönetilen kimlikle kimlik doğrulama](./api-management-authentication-policies.md#ManagedIdentity)
