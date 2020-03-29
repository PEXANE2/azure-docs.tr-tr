---
title: Azure API Yönetimi'nde yönetilen kimlikleri kullanma | Microsoft Dokümanlar
description: API Yönetimi'nde yönetilen kimlikleri nasıl kullanacağınızı öğrenin
services: api-management
documentationcenter: ''
author: miaojiang
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/18/2017
ms.author: apimpm
ms.openlocfilehash: 49576b805e6c6d01340e663bfb5d8e9013917625
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249639"
---
# <a name="use-managed-identities-in-azure-api-management"></a>Azure API Yönetimi'nde yönetilen kimlikleri kullanma

Bu makalede, bir API Yönetimi hizmet örneği için yönetilen bir kimlik oluşturma ve diğer kaynaklara nasıl erişileceği gösterilmektedir. Azure Etkin Dizini (Azure AD) tarafından oluşturulan yönetilen bir kimlik, API Yönetimi örneğinizin Azure Anahtar Kasası gibi Azure AD korumalı diğer kaynaklara kolayca ve güvenli bir şekilde erişmesine olanak tanır. Bu kimlik Azure tarafından yönetilir ve herhangi bir sırrı sağlamanızı veya döndürmenizi gerektirmez. Yönetilen kimlikler hakkında daha fazla bilgi için Azure [kaynakları için yönetilen kimliklerin ne olduğunu](../active-directory/managed-identities-azure-resources/overview.md)görün.

## <a name="create-a-managed-identity-for-an-api-management-instance"></a>API Yönetimi örneği için yönetilen bir kimlik oluşturma

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Portalda yönetilen bir kimlik ayarlamak için, önce normal olarak bir API Yönetimi örneği oluşturur ve ardından özelliği etkinleştirin.

1. Normalde yaptığınız gibi portalda bir API Yönetimi örneği oluşturun. Portalda ona gidin.
2. **Yönetilen hizmet kimliklerini**seçin.
3. Azure Active Directory ile Kayıt'ı Açık'a çevirin. Kaydet’e tıklayın.

![MSI’yi etkinleştirme](./media/api-management-msi/enable-msi.png)

### <a name="using-the-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanma

Kaynak tanımına aşağıdaki özelliği ekleyerek kimliği olan bir API Yönetimi örneği oluşturabilirsiniz:

```json
"identity" : {
    "type" : "SystemAssigned"
}
```

Bu, Azure'a API Yönetimi örneğinizin kimliğini oluşturmasını ve yönetmesini söyler.

Örneğin, tam bir Azure Kaynak Yöneticisi şablonu aşağıdaki gibi görünebilir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "resources": [{
        "apiVersion": "2017-03-01",
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
## <a name="use-the-managed-service-identity-to-access-other-resources"></a>Diğer kaynaklara erişmek için yönetilen hizmet kimliğini kullanma

> [!NOTE]
> Şu anda, yönetilen kimlikler API Yönetimi özel etki alanı adları için Azure Key Vault'tan sertifika almak için kullanılabilir. Yakında daha fazla senaryo desteklenecek.
>
>


### <a name="obtain-a-certificate-from-azure-key-vault"></a>Azure Key Vault'tan sertifika alma

#### <a name="prerequisites"></a>Ön koşullar
1. pfx sertifikasını içeren Anahtar Kasa, aynı Azure aboneliğinde ve API Yönetimi hizmetiyle aynı Kaynak Grubunda olmalıdır. Bu, Azure Kaynak Yöneticisi şablonunun bir gereksinimidir.
2. Sırrın İçerik Türü *uygulama/x-pkcs12*olmalıdır. Sertifikayı yüklemek için aşağıdaki komut dosyasını kullanabilirsiniz:

```powershell
$pfxFilePath = "PFX_CERTIFICATE_FILE_PATH" # Change this path 
$pwd = "PFX_CERTIFICATE_PASSWORD" # Change this password 
$flag = [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable 
$collection = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2Collection 
$collection.Import($pfxFilePath, $pwd, $flag) 
$pkcs12ContentType = [System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12 
$clearBytes = $collection.Export($pkcs12ContentType) 
$fileContentEncoded = [System.Convert]::ToBase64String($clearBytes) 
$secret = ConvertTo-SecureString -String $fileContentEncoded -AsPlainText –Force 
$secretContentType = 'application/x-pkcs12' 
Set-AzureKeyVaultSecret -VaultName KEY_VAULT_NAME -Name KEY_VAULT_SECRET_NAME -SecretValue $Secret -ContentType $secretContentType
```

> [!Important]
> Sertifikanın nesne sürümü sağlanmazsa, API Yönetimi, Anahtar Kasası'na yüklendikten sonra sertifikanın yeni sürümünü otomatik olarak alır.

Aşağıdaki örnekte, aşağıdaki adımları içeren bir Azure Kaynak Yöneticisi şablonu gösterilmektedir:

1. Yönetilen bir kimliğe sahip bir API Yönetimi örneği oluşturun.
2. Azure Anahtar Kasası örneğinin erişim ilkelerini güncelleştirin ve API Yönetimi örneğinin bu örnekten sır elde etmesine izin verin.
3. Anahtar Kasa örneğinden bir sertifika aracılığıyla özel bir etki alanı adı ayarlayarak API Yönetimi örneğini güncelleştirin.

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
                "description": "The pricing tier of this API Management service"
            }
        },
        "skuCount": {
            "type": "int",
            "defaultValue": 1,
            "metadata": {
                "description": "The instance size of this API Management service."
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
        "apiVersion": "2017-03-01",
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

## <a name="next-steps"></a>Sonraki adımlar

Azure kaynakları için yönetilen kimlikler hakkında daha fazla bilgi edinin:

* [Azure kaynakları için yönetilen kimlikler nedir](../active-directory/managed-identities-azure-resources/overview.md)
* [Azure Kaynak Yöneticisi şablonları](https://github.com/Azure/azure-quickstart-templates)
* [İlkede yönetilen bir kimlikle kimlik doğrulama](./api-management-authentication-policies.md#ManagedIdentity)
