---
title: Rol atamaları oluşturma ve yönetme - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins'te rol atamaları oluşturma ve yönetme hakkında bilgi edinin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: 1c83ca0abfd17db873bec62f0a0d052703862a45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110412"
---
# <a name="create-and-manage-role-assignments-in-azure-digital-twins"></a>Azure Digital Twins'te rol atamaları oluşturma ve yönetme

Azure Digital Twins, kaynaklara erişimi yönetmek için rol tabanlı erişim denetimini[(RBAC)](./security-role-based-access-control.md)kullanır.

## <a name="role-assignments-overview"></a>Rol atamalarına genel bakış

Her rol ataması aşağıdaki tanıma uygundur:

```JSON
{
  "roleId": "00e00ad7-00d4-4007-853b-b9968ad000d1",
  "objectId": "be2c6daa-a3a0-0c0a-b0da-c000000fbc5f",
  "objectIdType": "ServicePrincipalId",
  "path": "/",
  "tenantId": "00f000bf-86f1-00aa-91ab-2d7cd000db47"
}
```

Aşağıdaki tabloda her öznitelik açıklanmaktadır:

| Öznitelik | Adı | Gerekli | Tür | Açıklama |
| --- | --- | --- | --- | --- |
| Roleıd | Rol tanımı tanımlayıcısı | Evet | Dize | İstenilen rol atamasının benzersiz kimliği. Sistem API'sini sorgulayarak veya aşağıdaki tabloyu gözden geçirerek rol tanımlarını ve bunların tanımlayıcısını bulun. |
| Objectıd | Nesne tanımlayıcısı | Evet | Dize | Azure Etkin Dizin Kimliği, hizmet temel nesne kimliği veya etki alanı adı. Rol atamasının neye veya kime atandığı. Rol ataması ilişkili türüne göre biçimlendirilmelidir. objectIdType için `DomainName` objectId `“@”` karakteriyle başlamalıdır. |
| objectIdType | Nesne tanımlayıcı türü | Evet | Dize | Kullanılan Nesne Tanımlayıcısı türü. Aşağıdaki **Desteklenen ObjectIdTypes'a** bakın. |
| yol | Uzay yolu | Evet | Dize | `Space` Nesneye tam erişim yolu. `/{Guid}/{Guid}` bunun bir örneğidir. Bir tanımlayıcının tüm grafik için rol ataması gerekiyorsa, belirtin. `"/"` Bu karakter kökü belirtir, ancak kullanımı önerilmez. Her zaman En Az Ayrıcalık Prensibi'ne uyun. |
| tenantId | Kiracı tanımlayıcısı | Değişir | Dize | Çoğu durumda, bir Azure Etkin Dizin kiracı kimliği. İzin `DeviceId` verilmez ve `TenantId` ObjectIdTypes. Için `UserId` gerekli `ServicePrincipalId` ve ObjectIdTypes. DomainName ObjectIdType için isteğe bağlıdır. |

### <a name="supported-role-definition-identifiers"></a>Desteklenen rol tanımı tanımlayıcıları

Her rol ataması, Azure Digital Twins ortamınızdaki bir varlıkla bir rol tanımını ilişkilendirer.

[!INCLUDE [digital-twins-roles](../../includes/digital-twins-roles.md)]

### <a name="supported-object-identifier-types"></a>Desteklenen nesne tanımlayıcı türleri

Daha önce, **objectIdType** özniteliği tanıtıldı.

[!INCLUDE [digital-twins-object-types](../../includes/digital-twins-object-id-types.md)]

## <a name="role-assignment-operations"></a>Rol atama işlemleri

Azure Digital Twins, rol atamaları için tam *CREATE*, *READ*ve *DELETE* işlemlerini destekler. *UPDATE* işlemleri rol atamaları ekleyerek, rol atamaları kaldırarak veya rol atamaları erişim verdiği [Uzamsal İstihbarat Grafiği](./concepts-objectmodel-spatialgraph.md) düğümleri değiştirerek işlenir.

[![Rol atama sıyrıkları](media/security-roles/role-assignments.png)](media/security-roles/role-assignments.png#lightbox)

Sağlanan Swagger başvuru belgeleri, kullanılabilir tüm API uç noktaları, istek işlemleri ve tanımlar hakkında daha fazla bilgi içerir.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

### <a name="grant-permissions-to-your-service-principal"></a>Hizmet müdürünüze izin verme

Hizmet müdürünüze izin vermek genellikle Azure Digital Twins ile çalışırken atacağınız ilk adımlardan biridir. Bu gerektirir:

1. [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) veya [PowerShell](https://docs.microsoft.com/powershell/azure/)üzerinden Azure örneğinize giriş yapmak.
1. Hizmet temel bilgilerinizi edinme.
1. İstenilen rolü hizmet müdürünüze atamak.

Uygulama kimliğiniz Azure Active Directory'de size verilir. Active Directory'de Azure Dijital İkizler'i yapılandırma ve sağlama hakkında daha fazla bilgi edinmek için [Quickstart'ı](./quickstart-view-occupancy-dotnet.md)okuyun.

Uygulama kimliğini aldıktan sonra, aşağıdaki komutlardan birini uygulayın. Azure CLI'de:

```azurecli
az login
az ad sp show --id <ApplicationId>
```

Powershell içinde:

```powershell
Login-AzAccount
Get-AzADServicePrincipal -ApplicationId <ApplicationId>
```

**Yönetici** rolüne sahip bir kullanıcı, URL'ye kimlik doğrulaması yapılan bir HTTP POST isteği yaparak Alan Yöneticisi rolünü kullanıcıya atayabilir:

```URL
YOUR_MANAGEMENT_API_URL/roleassignments
```

Aşağıdaki JSON gövdesi ile:

```JSON
{
  "roleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
  "objectId": "YOUR_SERVICE_PRINCIPLE_OBJECT_ID",
  "objectIdType": "ServicePrincipalId",
  "path": "YOUR_PATH",
  "tenantId": "YOUR_TENANT_ID"
}
```

### <a name="retrieve-all-roles"></a>Tüm rolleri alma

[![Sistem rolleri](media/security-roles/system-api.png)](media/security-roles/system-api.png#lightbox)

Kullanılabilir tüm rolleri (rol tanımlarını) listelemek için, kimlik doğrulaması http GET isteğinde bulunun:

```URL
YOUR_MANAGEMENT_API_URL/system/roles
```

Başarılı bir istek, atanabilecek her rol için girişleri olan bir JSON dizidöndürecektir:

```JSON
[
    {
        "id": "3cdfde07-bc16-40d9-bed3-66d49a8f52ae",
        "name": "DeviceAdministrator",
        "permissions": [
            {
                "notActions": [],
                "actions": [
                    "Read",
                    "Create",
                    "Update",
                    "Delete"
                ],
                "condition": "@Resource.Type Any_of {'Device', 'DeviceBlobMetadata', 'DeviceExtendedProperty', 'Sensor', 'SensorBlobMetadata', 'SensorExtendedProperty'} || ( @Resource.Type == 'ExtendedType' && (!Exists @Resource.Category || @Resource.Category Any_of { 'DeviceSubtype', 'DeviceType', 'DeviceBlobType', 'DeviceBlobSubtype', 'SensorBlobSubtype', 'SensorBlobType', 'SensorDataSubtype', 'SensorDataType', 'SensorDataUnitType', 'SensorPortType', 'SensorType' } ) )"
            },
            {
                "notActions": [],
                "actions": [
                    "Read"
                ],
                "condition": "@Resource.Type == 'Space' && @Resource.Category == 'WithoutSpecifiedRbacResourceTypes' || @Resource.Type Any_of {'ExtendedPropertyKey', 'SpaceExtendedProperty', 'SpaceBlobMetadata', 'SpaceResource', 'Matcher'}"
            }
        ],
        "accessControlPath": "/system",
        "friendlyPath": "/system",
        "accessControlType": "System"
    }
]
```

### <a name="check-a-specific-role-assignment"></a>Belirli bir rol ataması denetleme

Belirli bir rol atamasını denetlemek için, kimlik doğrulaması http GET isteğinde bulunun:

```URL
YOUR_MANAGEMENT_API_URL/roleassignments/check?userId=YOUR_USER_ID&path=YOUR_PATH&accessType=YOUR_ACCESS_TYPE&resourceType=YOUR_RESOURCE_TYPE
```

| **Parametre değeri** | **Gerekli** |  **Tür** |  **Açıklama** |
| --- | --- | --- | --- |
| YOUR_USER_ID |  True | Dize |   UserId objectIdType için objectId. |
| YOUR_PATH | True | Dize |   Erişimi denetlemek için seçilen yol. |
| YOUR_ACCESS_TYPE |  True | Dize |   *Oku*, *Oluştur*, *Güncelleştir*veya *Sil* |
| YOUR_RESOURCE_TYPE | True | Dize |  *Cihaz*, *DeviceBlobMetadata*, *DeviceExtendedProperty*, *ExtendedPropertyKey*, *ExtendedType*, *Endpoint*, *KeyStore*, *Matcher*, *Ontoloji*, *Rapor*, *RoleDefinition*, *Sensör*, *SensorExtendedProperty*, *Space* *, SpaceBlobMetadata*, *SpaceExtendedProperty*, *SpaceResource*, *SpaceRoleAssignment*, *Sistem*, * UerDefinedFunction*, *Kullanıcı*, *UserBlobMetadata*, veya *UserExtendedProperty* |

Başarılı bir istek bir `true` boolean döndürür veya `false` erişim türünün verilen yol ve kaynak için kullanıcıya atanmış olup olmadığını gösterir.

### <a name="get-role-assignments-by-path"></a>Rol atamalarını yola göre alma

Bir yol için tüm rol atamaları almak için, doğrulanmış bir HTTP GET isteği yapmak için:

```URL
YOUR_MANAGEMENT_API_URL/roleassignments?path=YOUR_PATH
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_PATH | Boşluğa giden tam yol |

Başarılı bir istek, seçili **yol** parametresi ile ilişkili her rol ataması ile bir JSON dizi döndürecek:

```JSON
[
    {
        "id": "0000c484-698e-46fd-a3fd-c12aa11e53a1",
        "roleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
        "objectId": "0de38846-1aa5-000c-a46d-ea3d8ca8ee5e",
        "objectIdType": "UserId",
        "path": "/"
    }
]
```

### <a name="revoke-a-permission"></a>İznini iptal edin

Alıcıdan izin almak için, kimlik doğrulaması http delete isteği yaparak rol atamasını silin:

```URL
YOUR_MANAGEMENT_API_URL/roleassignments/YOUR_ROLE_ASSIGNMENT_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_ROLE_ASSIGNMENT_ID* | Kaldırılacak rol atamasının **kimliği** |

Başarılı bir DELETE isteği 204 yanıt durumunu döndürecek. Rol atamasının hala geçerli olup olmadığını [denetleyerek](#check-a-specific-role-assignment) rol atamasının kaldırılmasını doğrulayın.

### <a name="create-a-role-assignment"></a>Rol ataması oluşturma

Bir rol ataması oluşturmak için URL'ye doğrulanmış bir HTTP POST isteği yapın:

```URL
YOUR_MANAGEMENT_API_URL/roleassignments
```

JSON gövdesinin aşağıdaki şemaya uyduğundan doğrulayın:

```JSON
{
  "roleId": "YOUR_ROLE_ID",
  "objectId": "YOUR_OBJECT_ID",
  "objectIdType": "YOUR_OBJECT_ID_TYPE",
  "path": "YOUR_PATH",
  "tenantId": "YOUR_TENANT_ID"
}
```

Başarılı bir istek, yeni oluşturulan rol atamasının **kimliğiyle** birlikte 201 yanıt durumunu döndürecektir:

```JSON
"d92c7823-6e65-41d4-aaaa-f5b32e3f01b9"
```

## <a name="configuration-examples"></a>Yapılandırma örnekleri

Aşağıdaki örnekler, JSON gövdenizi sık karşılaşılan birkaç rol atama senaryosunda nasıl yapılandırılabildiğini göstermektedir.

* **Örnek**: Bir kullanıcının kiracı alanının bir katına yönetimerişimi ne gerekvardır.

   ```JSON
   {
    "roleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
    "objectId" : " 0fc863aa-eb51-4704-a312-7d635d70e000",
    "objectIdType" : "UserId",
    "tenantId": " a0c20ae6-e830-4c60-993d-a00ce6032724",
    "path": "/ 000e349c-c0ea-43d4-93cf-6b00abd23a44/ d84e82e6-84d5-45a4-bd9d-006a000e3bab"
   }
   ```

* **Örnek**: Bir uygulama, aygıtlar ve sensörlerle alay eden test senaryoları çalıştırıyor.

   ```JSON
   {
    "roleId": "98e44ad7-28d4-0007-853b-b9968ad132d1",
    "objectId" : "cabf7aaa-af0b-41c5-000a-ce2f4c20000b",
    "objectIdType" : "ServicePrincipalId",
    "tenantId": " a0c20ae6-e000-4c60-993d-a91ce6000724",
    "path": "/"
   }
    ```

* **Örnek**: Bir etki alanının parçası olan tüm kullanıcılar, alanlar, sensörler ve kullanıcılar için okuma erişimi alır. Bu erişim, karşılık gelen ilgili nesneleri içerir.

   ```JSON
   {
    "roleId": " b1ffdb77-c635-4e7e-ad25-948237d85b30",
    "objectId" : "@microsoft.com",
    "objectIdType" : "DomainName",
    "path": "/000e349c-c0ea-43d4-93cf-6b00abd23a00"
   }
   ```

## <a name="next-steps"></a>Sonraki adımlar

- Azure Digital Twins rol tabanlı erişim denetimini gözden geçirmek için [Rol tabanı-erişim denetimi'ni](./security-authenticating-apis.md)okuyun.

- Azure Digital Twins API kimlik doğrulaması hakkında bilgi edinmek için [API kimlik doğrulamasını](./security-authenticating-apis.md)okuyun.
