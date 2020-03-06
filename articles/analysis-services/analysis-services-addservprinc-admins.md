---
title: Hizmet sorumlusunu Azure Analysis Services Yönetici rolüne Ekle | Microsoft Docs
description: Azure Analysis Services Server yönetici rolüne bir Automation hizmet sorumlusu eklemeyi öğrenin
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.custom: fasttrack-edit
ms.openlocfilehash: 1370f65405963ebf825e986e6801607a0d96156e
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78298097"
---
# <a name="add-a-service-principal-to-the-server-administrator-role"></a>Sunucu Yöneticisi rolüne hizmet sorumlusu ekleme 

 Katılımsız PowerShell görevlerini otomatikleştirmek için bir hizmet sorumlusu, yönetilmekte olan Analysis Services sunucuda **Sunucu Yöneticisi** ayrıcalıklarına sahip olmalıdır. Bu makalede, bir Azure AS Server 'daki sunucu yöneticileri rolüne bir hizmet sorumlusu nasıl ekleyeceğiniz açıklanır. Bunu SQL Server Management Studio veya bir Kaynak Yöneticisi şablonu kullanarak yapabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
Bu görevi tamamlamadan önce, Azure Active Directory kayıtlı bir hizmet sorumlusu olması gerekir.

[Hizmet sorumlusu oluşturma-Azure portal](../active-directory/develop/howto-create-service-principal-portal.md)   
[Hizmet sorumlusu oluşturma - PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="using-sql-server-management-studio"></a>SQL Server Management Studio Kullanımı

Sunucu yöneticilerini, SQL Server Management Studio (SSMS) kullanarak yapılandırabilirsiniz. Bu görevi gerçekleştirmek için Azure 'da sunucu [Yöneticisi](analysis-services-server-admins.md) izinlerine sahip olmanız gerekir. 

1. SSMS 'de, Azure 'u sunucunuza bağlayın.
2. **Sunucu özellikleri** > **güvenlik**' te, **Ekle**' ye tıklayın.
3. **Kullanıcı veya Grup Seç**' te, kayıtlı uygulamanızı ada göre arayın, öğesini seçin ve ardından **Ekle**' ye tıklayın.

    ![Hizmet sorumlusu hesabı ara](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Hizmet sorumlusu hesap KIMLIĞINI doğrulayın ve ardından **Tamam**' a tıklayın.
    
    ![Hizmet sorumlusu hesabı ara](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)

## <a name="using-a-resource-manager-template"></a>Resource Manager şablonu kullanma

Ayrıca, bir Azure Resource Manager şablonu kullanarak Analysis Services sunucusunu dağıtarak sunucu yöneticilerini yapılandırabilirsiniz. Dağıtımı çalıştıran kimlik, [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/overview.md)içindeki kaynak için **katkıda** bulunan rolüne ait olmalıdır.

> [!IMPORTANT]
> Hizmet sorumlusu `app:{service-principal-client-id}@{azure-ad-tenant-id}`biçim kullanılarak eklenmelidir.

Aşağıdaki Kaynak Yöneticisi şablonu, Analysis Services Yönetici rolüne eklenen belirli bir hizmet sorumlusu ile bir Analysis Services sunucusu dağıtır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "analysisServicesServerName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "analysisServicesSkuName": {
            "type": "string"
        },
        "analysisServicesCapacity": {
            "type": "int"
        },
        "servicePrincipalClientId": {
            "type": "string"
        },
        "servicePrincipalTenantId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('analysisServicesServerName')]",
            "type": "Microsoft.AnalysisServices/servers",
            "apiVersion": "2017-08-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('analysisServicesSkuName')]",
                "capacity": "[parameters('analysisServicesCapacity')]"
            },
            "properties": {
                "asAdministrators": {
                    "members": [
                        "[concat('app:', parameters('servicePrincipalClientId'), '@', parameters('servicePrincipalTenantId'))]"
                    ]
                }
            }
        }
    ]
}
```

## <a name="using-managed-identities"></a>Yönetilen kimlikleri kullanma

Yönetilen bir kimlik Analysis Services yöneticileri listesine de eklenebilir. Örneğin, [sistem tarafından atanan yönetilen kimliğe sahip bir mantıksal uygulamanız](../logic-apps/create-managed-service-identity.md)olabilir ve bu uygulamaya Analysis Services sunucunuzu yönetme özelliği vermek isteyebilirsiniz.

Azure portal ve API 'lerin çoğu bölümünde, Yönetilen kimlikler hizmet sorumlusu nesne KIMLIĞI kullanılarak tanımlanır. Ancak, Analysis Services istemci KIMLIKLERI kullanılarak tanımlanmaları gerekir. Bir hizmet sorumlusu için istemci KIMLIĞINI almak için Azure CLı 'yi kullanabilirsiniz:

```bash
az ad sp show --id <ManagedIdentityServicePrincipalObjectId> --query appId -o tsv
```

Alternatif olarak PowerShell 'i de kullanabilirsiniz:

```powershell
(Get-AzureADServicePrincipal -ObjectId <ManagedIdentityServicePrincipalObjectId>).AppId
```

Daha sonra bu istemci KIMLIĞINI, yukarıda açıklandığı gibi Analysis Services yöneticileri listesine eklemek için kiracı KIMLIĞIYLE birlikte kullanabilirsiniz.

## <a name="related-information"></a>İlgili bilgiler

* [PowerShell modülünü indir SQL Server](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS 'yi indir](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


