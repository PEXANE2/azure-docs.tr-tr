---
title: Data Factory için yönetilen kimlik
description: Azure Veri Fabrikası için yönetilen kimlik hakkında bilgi edinin.
services: data-factory
author: linda33wj
manager: shwang
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: jingwang
ms.openlocfilehash: 45699680ad2003c034bce588857f8b102a0b6d26
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261131"
---
# <a name="managed-identity-for-data-factory"></a>Data Factory için yönetilen kimlik

Bu makale, Veri Fabrikası için yönetilen kimliğin (eskiden Yönetilen Hizmet Kimliği/MSI olarak da bilinir) ne olduğunu ve nasıl çalıştığını anlamanıza yardımcı olur.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış

Bir veri fabrikası oluştururken, yönetilen bir kimlik fabrika oluşturma ile birlikte oluşturulabilir. Yönetilen kimlik, Azure Etkin Dizin'e kayıtlı yönetilen bir uygulamadır ve bu özel veri fabrikasını temsil eder.

Veri Fabrikası için yönetilen kimlik aşağıdaki özelliklerden yararlanır:

- [Azure Key Vault'ta kimlik bilgilerini depolayın](store-credentials-in-key-vault.md), bu durumda veri fabrikası nın yönetilen kimliği Azure Key Vault kimlik doğrulaması için kullanılır.
- [Azure Blob depolama,](connector-azure-blob-storage.md)Azure [Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md), [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md), [Azure SQL Veritabanı](connector-azure-sql-database.md)ve Azure SQL [Veri Ambarı](connector-azure-sql-data-warehouse.md)dahil Bağlayıcılar .
- [Web etkinliği.](control-flow-web-activity.md)

## <a name="generate-managed-identity"></a>Yönetilen kimlik oluşturma

Veri Fabrikası için yönetilen kimlik aşağıdaki gibi oluşturulur:

- **Azure portalı veya PowerShell**üzerinden veri fabrikası oluştururken, yönetilen kimlik her zaman otomatik olarak oluşturulur.
- **SDK**üzerinden veri fabrikası oluştururken, yönetilen kimlik yalnızca oluşturulmak üzere fabrika nesnesinde "Identity = yeni FactoryIdentity()" belirtirseniz oluşturulur. [.NET quickstart](quickstart-create-data-factory-dot-net.md#create-a-data-factory)'daki örneğe bakın - veri fabrikası oluşturun.
- **REST API**aracılığıyla veri fabrikası oluştururken, yönetilen kimlik yalnızca istek gövdesinde "kimlik" bölümünü belirtirseniz oluşturulur. REST quickstart 'daki örneğe bakın [- veri fabrikası oluşturun.](quickstart-create-data-factory-rest-api.md#create-a-data-factory)

Veri [fabrikanızın, yönetilen kimlik kılavuzunu al'ı](#retrieve-managed-identity) izleyen yönetilen bir kimliğin ilişkili olmadığını bulursanız, veri fabrikasını kimlik başlatıcısı programlı olarak güncelleyerek açıkça bir kimlik oluşturabilirsiniz:

- [PowerShell'i kullanarak yönetilen kimlik oluşturma](#generate-managed-identity-using-powershell)
- [REST API'yi kullanarak yönetilen kimlik oluşturma](#generate-managed-identity-using-rest-api)
- [Azure Kaynak Yöneticisi şablonu kullanarak yönetilen kimlik oluşturma](#generate-managed-identity-using-an-azure-resource-manager-template)
- [SDK kullanarak yönetilen kimlik oluşturma](#generate-managed-identity-using-sdk)

>[!NOTE]
>- Yönetilen kimlik değiştirilemez. Zaten yönetilen bir kimliğe sahip bir veri fabrikasının güncelleştirilmesinin herhangi bir etkisi olmaz, yönetilen kimlik değişmeden tutulur.
>- Fabrika nesnesinde "kimlik" parametresini belirtmeden veya REST istek gövdesinde "kimlik" bölümü belirtmeden zaten yönetilen bir kimliğe sahip bir veri fabrikasını güncellerseniz, bir hata alırsınız.
>- Bir veri fabrikasını sildiğinizde, ilişkili yönetilen kimlik birlikte silinir.

### <a name="generate-managed-identity-using-powershell"></a>PowerShell'i kullanarak yönetilen kimlik oluşturma

**Set-AzDataFactoryV2** komutunu yeniden arayın, ardından "Kimlik" alanlarının yeni oluşturulduğunu görürsünüz:

```powershell
PS C:\WINDOWS\system32> Set-AzDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName> -Location <region>

DataFactoryName   : ADFV2DemoFactory
DataFactoryId     : /subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory
ResourceGroupName : <resourceGroupName>
Location          : East US
Tags              : {}
Identity          : Microsoft.Azure.Management.DataFactory.Models.FactoryIdentity
ProvisioningState : Succeeded
```

### <a name="generate-managed-identity-using-rest-api"></a>REST API'yi kullanarak yönetilen kimlik oluşturma

İstek gövdesinde "kimlik" bölümü bulunan API'nin altından çağrı yapın:

```
PATCH https://management.azure.com/subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<data factory name>?api-version=2018-06-01
```

**İstek gövdesi**: ekle "kimlik": { "türü": "SystemAssigned" }.

```json
{
    "name": "<dataFactoryName>",
    "location": "<region>",
    "properties": {},
    "identity": {
        "type": "SystemAssigned"
    }
}
```

**Yanıt**: yönetilen kimlik otomatik olarak oluşturulur ve "kimlik" bölümü buna göre doldurulur.

```json
{
    "name": "<dataFactoryName>",
    "tags": {},
    "properties": {
        "provisioningState": "Succeeded",
        "loggingStorageAccountKey": "**********",
        "createTime": "2017-09-26T04:10:01.1135678Z",
        "version": "2018-06-01"
    },
    "identity": {
        "type": "SystemAssigned",
        "principalId": "765ad4ab-XXXX-XXXX-XXXX-51ed985819dc",
        "tenantId": "72f988bf-XXXX-XXXX-XXXX-2d7cd011db47"
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory",
    "type": "Microsoft.DataFactory/factories",
    "location": "<region>"
}
```

### <a name="generate-managed-identity-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak yönetilen kimlik oluşturma

**Şablon**: "identity": { "type": "SystemAssigned" }.

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "resources": [{
        "name": "<dataFactoryName>",
        "apiVersion": "2018-06-01",
        "type": "Microsoft.DataFactory/factories",
        "location": "<region>",
        "identity": {
            "type": "SystemAssigned"
        }
    }]
}
```

### <a name="generate-managed-identity-using-sdk"></a>SDK kullanarak yönetilen kimlik oluşturma

Kimlik=yeni FactoryIdentity() ile veri fabrikasını arayın create_or_update işlevi. .NET kullanarak örnek kod:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="retrieve-managed-identity"></a>Yönetilen kimliği alma

Yönetilen kimliği Azure portalından veya programlı olarak alabilirsiniz. Aşağıdaki bölümlerde bazı örnekler göster.

>[!TIP]
> Yönetilen kimliği görmüyorsanız, fabrikanızı güncelleyerek [yönetilen kimlik oluşturun.](#generate-managed-identity)

### <a name="retrieve-managed-identity-using-azure-portal"></a>Azure portalını kullanarak yönetilen kimliği alma

Yönetilen kimlik bilgilerini Azure portalından (> veri fabrikanızın özellikleri > bulabilirsiniz.

- Yönetilen Kimlik Nesne Kimliği
- Yönetilen Kimlik Kiracı
- Yönetilen Kimlik Başvuru Kimliği

Yönetilen kimlik bilgileri, Azure Blob, Azure Veri Gölü Depolama, Azure Anahtar Kasası gibi yönetilen kimlik doğrulamasını destekleyen bağlantılı hizmet oluşturduğunuzda da gösterilecek.

İzin verirken, bu kimliği bulmak için nesne kimliği veya veri fabrika adını (yönetilen kimlik adı olarak) kullanın.

### <a name="retrieve-managed-identity-using-powershell"></a>PowerShell'i kullanarak yönetilen kimliği alma

Yönetilen kimlik asıl kimliği ve kiracı kimliği, aşağıdaki gibi belirli bir veri fabrikası aldığınızda döndürülür. Erişim sağlamak için **PrincipalId'i** kullanın:

```powershell
PS C:\WINDOWS\system32> (Get-AzDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Yukarıdaki asıl kimliği kopyalayarak uygulama kimliğini alabilir, ardından temel kimlik parametresi olarak Azure Active Directory komutu altında çalıştırabilirsiniz.

```powershell
PS C:\WINDOWS\system32> Get-AzADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikası nın yönetilen kimliğinin ne zaman ve nasıl kullanılacağını tanıtan aşağıdaki konulara bakın:

- [Azure Anahtar Kasası'nda mağaza kimlik bilgisi](store-credentials-in-key-vault.md)
- [Azure kaynakları kimlik doğrulaması için yönetilen kimlikleri kullanarak verileri Azure Veri Gölü Deposu'ndan kopyalayın](connector-azure-data-lake-store.md)

Bkz. Veri fabrikası nın yönetilen kimliğinin dayandığı Azure kaynakları için yönetilen kimlikler hakkında daha fazla arka plan için [Azure Kaynaklarına Genel Bakış](/azure/active-directory/managed-identities-azure-resources/overview) Için Yönetilen Kimlikler. 
