---
title: Azure Analiz Hizmetleri yönetici rolüne hizmet sorumlusu ekleme | Microsoft Dokümanlar
description: Azure Analiz Hizmetleri sunucu yöneticisi rolüne nasıl bir otomasyon hizmeti yöneticisi ekleyeceğinizi öğrenin
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.custom: fasttrack-edit
ms.openlocfilehash: 1370f65405963ebf825e986e6801607a0d96156e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78298097"
---
# <a name="add-a-service-principal-to-the-server-administrator-role"></a>Sunucu yöneticisi rolüne bir hizmet ilkesi ekleme 

 Gözetimsiz PowerShell görevlerini otomatikleştirmek için, bir hizmet yöneticisinin Çözümleme Hizmetleri sunucusunda **sunucu yöneticisi** ayrıcalıkları yönetilmelidir. Bu makalede, bir Azure AS sunucusundaki sunucu yöneticileri rolüne bir hizmet yöneticisinin nasıl ekleyeceğiniz açıklanmaktadır. Bunu SQL Server Management Studio veya Resource Manager şablonu kullanarak yapabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
Bu görevi tamamlamadan önce, Azure Etkin Dizini'nde kayıtlı bir hizmet yöneticisi olması gerekir.

[Hizmet sorumlusu oluşturma - Azure portalı](../active-directory/develop/howto-create-service-principal-portal.md)   
[Hizmet sorumlusu oluşturma - PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="using-sql-server-management-studio"></a>SQL Server Management Studio Kullanımı

SQL Server Management Studio 'yı (SSMS) kullanarak sunucu yöneticilerini yapılandırabilirsiniz. Bu görevi tamamlamak için Azure AS sunucusunda [sunucu yöneticisi](analysis-services-server-admins.md) izinlerine sahip olmalısınız. 

1. SSMS'de Azure AS sunucunuza bağlanın.
2. **Sunucu Özellikleri** > **Güvenliği'nde** **Ekle'yi**tıklatın.
3. **Bir Kullanıcı veya Grup Seç'te**kayıtlı uygulamanızı ada göre arayın, seçin ve sonra **Ekle'yi**tıklatın.

    ![Servis ana hesabı nı ara](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Servis ana hesap kimliğini doğrulayın ve ardından **Tamam'ı**tıklatın.
    
    ![Servis ana hesabı nı ara](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)

## <a name="using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanma

Ayrıca, Çözümleme Hizmetleri sunucusunu Azure Kaynak Yöneticisi şablonu kullanarak dağıtarak sunucu yöneticilerini yapılandırabilirsiniz. Dağıtımı çalıştıran kimlik, [Azure Role Tabanlı Erişim Denetimi'ndeki (RBAC)](../role-based-access-control/overview.md)kaynak için **Katılımcı** rolüne ait olmalıdır.

> [!IMPORTANT]
> Hizmet sorumlusu biçimi `app:{service-principal-client-id}@{azure-ad-tenant-id}`kullanılarak eklenmelidir.

Aşağıdaki Kaynak Yöneticisi şablonu, Çözümleme Hizmetleri Yöneticisi rolüne eklenen belirli bir hizmet ilkesine sahip bir Çözümleme Hizmetleri sunucusu dağıtır:

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

Yönetilen bir kimlik, Çözümleme Hizmetleri Yöneticileri listesine de eklenebilir. Örneğin, sistem tarafından [atanmış yönetilen bir kimliğe sahip bir Mantık Uygulamanız](../logic-apps/create-managed-service-identity.md)olabilir ve çözümleme hizmetleri sunucunuzu yönetme olanağı vermek isteyebilirsiniz.

Azure portalı nın ve API'lerin çoğu yerinde yönetilen kimlikler, hizmet temel nesne kimliklerini kullanarak tanımlanır. Ancak, Çözümleme Hizmetleri, istemci kimliklerini kullanarak tanımlanmasını gerektirir. Bir hizmet sorumlusu için istemci kimliğini elde etmek için Azure CLI'yi kullanabilirsiniz:

```bash
az ad sp show --id <ManagedIdentityServicePrincipalObjectId> --query appId -o tsv
```

Alternatif olarak PowerShell kullanabilirsiniz:

```powershell
(Get-AzureADServicePrincipal -ObjectId <ManagedIdentityServicePrincipalObjectId>).AppId
```

Daha sonra, yönetilen kimliği yukarıda açıklandığı gibi Çözümleme Hizmetleri Yöneticileri listesine eklemek için bu istemci kimliğini kiracı kimliğiyle birlikte kullanabilirsiniz.

## <a name="related-information"></a>İlgili bilgiler

* [SQL Server PowerShell Modüllerini İndir](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS İndir](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


