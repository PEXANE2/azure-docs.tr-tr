---
title: Azure kaynakları için RBAC'daki rol tanımlarını anlama | Microsoft Dokümanlar
description: Azure kaynaklarının ince taneli erişim yönetimi için rol tabanlı erişim denetiminde (RBAC) rol tanımları hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/17/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 777ea7cc29679a3819e94d39913f167ea1cb3453
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641374"
---
# <a name="understand-role-definitions-for-azure-resources"></a>Azure kaynakları için rol tanımlarını anlama

Bir rolün nasıl çalıştığını anlamaya çalışıyorsanız veya Azure kaynakları için kendi [özel rolünüzü](custom-roles.md)oluşturuyorsanız, rollerin nasıl tanımlandığını anlamak yararlıdır. Bu makalede, rol tanımlarının ayrıntıları açıklanır ve bazı örnekler sağlar.

## <a name="role-definition"></a>Rol tanımı

*Rol tanımı*, izinlerden oluşan bir koleksiyondur. Bazen yalnızca *rol* olarak da adlandırılır. Rol tanımı; okuma, yazma ve silme gibi gerçekleştirilebilecek işlemleri listeler. Ayrıca gerçekleştirilemeyen işlemleri veya temel verilerle ilgili işlemleri de listeleyebilir. Rol tanımı aşağıdaki özelliklere sahiptir:

```
Name
Id
IsCustom
Description
Actions []
NotActions []
DataActions []
NotDataActions []
AssignableScopes []
```

| Özellik | Açıklama |
| --- | --- |
| `Name` | Rolün görüntü adı. |
| `Id` | Rolün benzersiz kimliği. |
| `IsCustom` | Bunun özel bir rol olup olmadığını gösterir. Özel `true` roller için ayarlayın. |
| `Description` | Rolün tanımı. |
| `Actions` | Rolün gerçekleştirilmesine izin verdiği yönetim işlemlerini belirten bir dizi dize. |
| `NotActions` | İzin verilenin `Actions`dışında olan yönetim işlemlerini belirten bir dizi dize. |
| `DataActions` | Rolün bu nesne içindeki verilerinize gerçekleştirilmesine izin verdiği veri işlemlerini belirten bir dizi dize. |
| `NotDataActions` | İzin verilenin `DataActions`dışında olan veri işlemlerini belirten bir dizi dize. |
| `AssignableScopes` | Rolün atama için kullanılabilir olduğunu gösteren kapsamları belirten bir dizi dize. |

### <a name="operations-format"></a>İşlem biçimi

İşlemler, aşağıdaki biçime sahip dizeleri ile belirtilir:

- `{Company}.{ProviderName}/{resourceType}/{action}`

İşlem `{action}` dizesinin bölümü, kaynak türünde gerçekleştirebileceğiniz işlem türünü belirtir. Örneğin, aşağıdaki alt dizeleri `{action}`göreceksiniz:

| Eylem alt dize    | Açıklama         |
| ------------------- | ------------------- |
| `*` | Joker karakter, dizeyle eşleşen tüm işlemlere erişim verir. |
| `read` | Okuma işlemlerini (GET) sağlar. |
| `write` | Yazma işlemlerini (PUT veya PATCH) etkinleştirir. |
| `action` | Sanal makineleri yeniden başlatma (POST) gibi özel işlemleri etkinleştirir. |
| `delete` | Silme işlemlerini (DELETE) etkinleştirir. |

### <a name="role-definition-example"></a>Rol tanımı örneği

İşte JSON formatında [Katılımcı](built-in-roles.md#contributor) rol tanımı. `Actions` altındaki joker karakter (`*`) işlemi, bu role atanan sorumlunun tüm eylemleri gerçekleştirebileceğini gösterir veya başka bir deyişle her şeyi yönetebilir. Bu, Azure yeni kaynak türleri ekledikçe gelecekte tanımlanacak eylemleri de içerir. `NotActions` altındaki işlemler `Actions` işlemlerinden çıkarılır. [Katkıda Bulunan](built-in-roles.md#contributor) rolünde, `NotActions` bu rolün kaynakları erişimi yönetme becerisini kaldırır ve kaynaklara erişim atar.

```json
{
  "Name": "Contributor",
  "Id": "b24988ac-6180-42a0-ab88-20f7382dd24c",
  "IsCustom": false,
  "Description": "Lets you manage everything except access to resources.",
  "Actions": [
    "*"
  ],
  "NotActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/"
  ]
}
```

## <a name="management-and-data-operations"></a>Yönetim ve veri işlemleri

Yönetim işlemleri için rol tabanlı erişim `Actions` denetimi, rol tanımının özelliklerinde ve `NotActions` özelliklerinde belirtilir. Azure'daki yönetim işlemlerine bazı örnekler aşağıda verilmiştir:

- Depolama hesabına erişimi yönetme
- Blob kapsayıcısı oluşturma, güncelleştirme veya silme
- Kaynak grubunu ve tüm kaynaklarını silme

Kapsayıcı kimlik doğrulama yönteminin "Access Key" değil de "Azure AD Kullanıcı Hesabı" olarak ayarlanması koşuluyla, yönetim erişimi verilerinize devredilmez. Bu ayrım, joker karakterli`*`rollerin verilerinize sınırsız erişimini önler. Örneğin, bir kullanıcının abonelikte [Reader](built-in-roles.md#reader) rolü varsa, depolama hesabını görüntüleyebilir, ancak varsayılan olarak temel verileri görüntüleyemezler.

Önceden, veri işlemleri için rol tabanlı erişim denetimi kullanılmadı. Veri işlemleri için yetkilendirme kaynak sağlayıcılar arasında farklılık gösterir. Yönetim işlemleri için kullanılan aynı rol tabanlı erişim denetimi yetkilendirme modeli veri işlemlerine genişletildi.

Veri işlemlerini desteklemek için rol tanımına yeni veri özellikleri eklendi. Veri işlemleri `DataActions` ve `NotDataActions` özelliklerinde belirtilir. Bu veri özellikleri eklenerek, yönetim ve veri arasındaki ayrım korunur. Ayrıca joker karakter (`*`) içeren geçerli rol atamalarının aniden verilere erişim almasını da önler. Burada, `DataActions` ve `NotDataActions` özelliklerinde belirtilebilecek bazı veri işlemleri verilmiştir:

- Kapsayıcıdaki blobların listesini okuma
- Kapsayıcıda depolama blobu yazma
- Kuyruktaki iletiyi silme

Burada Depolama [Blob Veri Okuyucu](built-in-roles.md#storage-blob-data-reader) rol tanımı, hem `Actions` de `DataActions` özellikleri işlemleri içerir. Bu rol, blob kapsayıcı ve aynı zamanda altta yatan blob verileri okumanızı sağlar.

```json
{
  "Name": "Storage Blob Data Reader",
  "Id": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
  "IsCustom": false,
  "Description": "Allows for read access to Azure Storage blob containers and data",
  "Actions": [
    "Microsoft.Storage/storageAccounts/blobServices/containers/read"
  ],
  "NotActions": [],
  "DataActions": [
    "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
  ],
  "NotDataActions": [],
  "AssignableScopes": [
    "/"
  ]
}
```

`DataActions` ve `NotDataActions` özelliklerine yalnızca veri işlemleri eklenebilir. Kaynak sağlayıcıları, özelliği `isDataAction` `true`' ye ayarlayarak hangi işlemlerin veri işlemleri olduğunu belirler. Operasyonların `isDataAction` listesini görmek için `true` [Kaynak sağlayıcı işlemlerine](resource-provider-operations.md)bakın. Veri işlemleri olmayan rollerin rol tanımı `DataActions` `NotDataActions` içinde olması ve özellikleri olması gerekmez.

Tüm yönetim işlemi API çağrıları için yetkilendirme Azure Kaynak Yöneticisi tarafından işlenir. Veri işlemi API çağrıları için yetkilendirme bir kaynak sağlayıcısı veya Azure Kaynak Yöneticisi tarafından işlenir.

### <a name="data-operations-example"></a>Veri işlemleri örneği

Yönetim ve veri işlemlerinin nasıl çalıştığını daha iyi anlamak için belirli bir örnek düşünelim. Alice abonelik kapsamında [Sahibi](built-in-roles.md#owner) rolü atandı. Bob' a bir depolama hesabı kapsamında [Depolama Blob Veri Katılımcısı](built-in-roles.md#storage-blob-data-contributor) rolü atandı. Aşağıdaki diyagram bu örneği gösterir.

![Hem yönetim hem de veri işlemlerini desteklemek için rol tabanlı erişim denetimi genişletildi](./media/role-definitions/rbac-management-data.png)

Gamze'nin [Sahibi](built-in-roles.md#owner) rolü ve Bob için [Depolama Blob Veri Katılımcısı](built-in-roles.md#storage-blob-data-contributor) rolü aşağıdaki eylemlere sahiptir:

Sahip

&nbsp;&nbsp;&nbsp;&nbsp;Eylem<br>
&nbsp;&nbsp;&nbsp;&nbsp;`*`

Depolama Blob Veri Katılımcısı

&nbsp;&nbsp;&nbsp;&nbsp;Eylem<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/write`<br>
&nbsp;&nbsp;&nbsp;&nbsp;DataActions<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write`

Alice'in bir abonelik`*`kapsamında bir joker kartı ( ) eylemi olduğundan, tüm yönetim eylemlerini gerçekleştirmelerini sağlamak için izinleri devralır. Alice kapsayıcıları okuyabilir, yazabilir ve silebilir. Ancak, Gamze ek adımlar atmadan veri işlemleri gerçekleştiremez. Örneğin, varsayılan olarak, Gamze bir kapsayıcının içindeki lekeleri okuyamaz. Lekeleri okumak için, Alice depolama erişim anahtarlarını almak ve lekeler erişmek için bunları kullanmak gerekir.

Bob'un izinleri yalnızca Depolama `Actions` `DataActions` [Blob Veri Katılımcısı](built-in-roles.md#storage-blob-data-contributor) rolünde belirtilenlerle sınırlıdır. Role bağlı olarak, Bob hem yönetim hem de veri işlemleri gerçekleştirebilir. Örneğin, Bob belirtilen depolama hesabındaki kapsayıcıları okuyabilir, yazabilir ve silebilir ve lekeleri okuyabilir, yazabilir ve silebilir.

Depolama için yönetim ve veri düzlemi güvenliği hakkında daha fazla bilgi için [Azure Depolama güvenlik kılavuzuna](../storage/blobs/security-recommendations.md)bakın.

### <a name="what-tools-support-using-rbac-for-data-operations"></a>Veri işlemleri için RBAC'ı kullanan araçlar nelerdir?

Veri işlemlerini görüntülemek ve bu işlemlerle çalışmak için araçların veya SDK'ların doğru sürümlerine sahip olmalısınız:

| Araç  | Sürüm  |
|---------|---------|
| [Azure PowerShell](/powershell/azure/install-az-ps) | 1.1.0 veya sonrası |
| [Azure CLI](/cli/azure/install-azure-cli) | 2.0.30 veya sonrası |
| [.NET için Azure](/dotnet/azure/) | 2.8.0-önizleme veya sonrası |
| [Go için Azure SDK](/azure/go/azure-sdk-go-install) | 15.0.0 veya sonrası |
| [Java için Azure](/java/azure/) | 1.9.0 veya sonrası |
| [Python için Azure](/azure/python/) | 0.40.0 veya sonrası |
| [Ruby için Azure SDK](https://rubygems.org/gems/azure_sdk) | 0.17.1 veya sonrası |

REST API'deki veri işlemlerini görüntülemek ve kullanmak için **api sürüm** parametresini aşağıdaki sürümveya daha sonra olarak ayarlamanız gerekir:

- 2018-07-01

## <a name="actions"></a>Eylemler

İzin, `Actions` rolün gerçekleştirilmesine izin verdiği yönetim işlemlerini belirtir. Azure kaynak sağlayıcılarının önemli işlemlerini tanımlayan bir işlem dizeleri topluluğudur. Aşağıda, 'de `Actions`kullanılabilecek yönetim işlemlerinin bazı örnekleri verilmiştir.

> [!div class="mx-tableFixed"]
> | Çalışma dizesi    | Açıklama         |
> | ------------------- | ------------------- |
> | `*/read` | Tüm Azure kaynak sağlayıcılarının tüm kaynak türleri için okuma işlemlerine erişim sağlar.|
> | `Microsoft.Compute/*` | Microsoft.Compute kaynak sağlayıcısındaki tüm kaynak türlerine erişim verir.|
> | `Microsoft.Network/*/read` | Microsoft.Network kaynak sağlayıcısındaki tüm kaynak türlerinin okuma işlemlerine erişim sağlar.|
> | `Microsoft.Compute/virtualMachines/*` | Sanal makinelerin tüm işlemlerine ve alt kaynak türlerine erişim verir.|
> | `microsoft.web/sites/restart/Action` | Bir web uygulamasını yeniden başlatmak için erişim verir.|

## <a name="notactions"></a>NotActions

İzin, `NotActions` izin verilenin `Actions`dışında olan yönetim işlemlerini belirtir. İzin `NotActions` vermek istediğiniz işlem kümesi kısıtlanmış işlemleri hariç tutarak daha kolay tanımlanırsa izni kullanın. Bir rol tarafından verilen erişim (etkili izinler) `NotActions` `Actions` işlemleri işlemlerden çıkarılarak hesaplanır.

> [!NOTE]
> Bir kullanıcıya, bu işlemdeki `NotActions`bir işlemi hariç tutan bir rol atanmışsa ve aynı işlemiçin erişim sağlayan ikinci bir rol atanırsa, kullanıcının bu işlemi gerçekleştirmesine izin verilir. `NotActions`bir inkar kuralı değildir – belirli operasyonların dışlanmaması gerektiğinde izin verilen bir dizi işlem oluşturmak için uygun bir yoldur.
>

## <a name="dataactions"></a>DataActions

İzin, `DataActions` rolün bu nesne içindeki verilerinize gerçekleştirilmesine izin verdiği veri işlemlerini belirtir. Örneğin, bir kullanıcı bir depolama hesabına blob veri erişimini okumuşsa, o depolama hesabındaki lekeleri okuyabilir. Aşağıda, 'de `DataActions`kullanılabilecek bazı veri işlemleri örnekleri verilmiştir.

> [!div class="mx-tableFixed"]
> | Çalışma dizesi    | Açıklama         |
> | ------------------- | ------------------- |
> | `Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read` | Bir blob veya lekelerin listesini verir. |
> | `Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write` | Bir blob yazma sonucunu döndürür. |
> | `Microsoft.Storage/storageAccounts/queueServices/queues/messages/read` | Bir iletiyi döndürür. |
> | `Microsoft.Storage/storageAccounts/queueServices/queues/messages/*` | İletiyi veya bir iletiyi yazmanın veya silmenin sonucunu döndürür. |

## <a name="notdataactions"></a>NotDataActions

İzin, `NotDataActions` izin verilen `DataActions`indışındaki veri işlemlerini belirtir. Bir rol tarafından verilen erişim (etkili izinler) `NotDataActions` `DataActions` işlemleri işlemlerden çıkarılarak hesaplanır. Her kaynak sağlayıcısı, veri işlemlerini gerçekleştirmek için kendi AP'lerini sağlar.

> [!NOTE]
> Bir `NotDataActions`kullanıcıya, bir veri işlemini dışlayan bir rol atanırsa ve aynı veri işlemine erişim sağlayan ikinci bir rol atanırsa, kullanıcının bu veri işlemini gerçekleştirmesine izin verilir. `NotDataActions`bir inkar kuralı değildir – belirli veri işlemlerinin hariç edilmesi gerektiğinde izin verilen veri işlemleri kümesi oluşturmak için uygun bir yoldur.
>

## <a name="assignablescopes"></a>Atanabilir Skoplar

Özellik, `AssignableScopes` bu rol tanımını kullanılabilir olan kapsamları (yönetim grupları, abonelikler veya kaynak grupları) belirtir. Rolü yalnızca gerektiren yönetim gruplarında, aboneliklerde veya kaynak gruplarında atama için kullanılabilir hale getirebilirsiniz. En az bir yönetim grubu, abonelik veya kaynak grubu kullanmanız gerekir.

Yerleşik roller kök `AssignableScopes` kapsamına ayarlanmış`"/"`( ). Kök kapsamı, rolün tüm kapsamlarda atama için kullanılabilir olduğunu gösterir. Geçerli atanabilir kapsamlara örnek olarak şunlar verilebilir:

> [!div class="mx-tableFixed"]
> | Rol atama için kullanılabilir | Örnek |
> |----------|---------|
> | Tek abonelik | `"/subscriptions/{subscriptionId1}"` |
> | İki abonelik | `"/subscriptions/{subscriptionId1}", "/subscriptions/{subscriptionId2}"` |
> | Ağ kaynak grubu | `"/subscriptions/{subscriptionId1}/resourceGroups/Network"` |
> | Bir yönetim grubu | `"/providers/Microsoft.Management/managementGroups/{groupId1}"` |
> | Yönetim grubu ve abonelik | `"/providers/Microsoft.Management/managementGroups/{groupId1}", /subscriptions/{subscriptionId1}",` |
> | Tüm kapsamlar (yalnızca yerleşik roller için geçerlidir) | `"/"` |

Özel roller `AssignableScopes` hakkında bilgi için [Azure kaynakları için Özel rollere](custom-roles.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynakları için yerleşik roller](built-in-roles.md)
* [Azure kaynakları için özel roller](custom-roles.md)
* [Azure Kaynak Yöneticisi kaynak sağlayıcısı işlemleri](resource-provider-operations.md)
