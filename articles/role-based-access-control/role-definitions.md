---
title: Azure kaynakları için RBAC 'de rol tanımlarını anlama | Microsoft Docs
description: Azure kaynaklarının ayrıntılı erişim yönetimi için rol tabanlı erişim denetimi (RBAC) içindeki rol tanımları hakkında bilgi edinin.
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
ms.date: 09/11/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 3ff4b2cb6a59a35dc6da4748a7c7fbb4758a4fcf
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75981010"
---
# <a name="understand-role-definitions-for-azure-resources"></a>Azure kaynakları için rol tanımlarını anlama

Bir rolün nasıl çalıştığını anlamaya çalışıyorsanız veya [Azure kaynakları için kendi özel rolünüzü](custom-roles.md)oluşturuyorsanız, rollerinin nasıl tanımlandığını anlamanız yararlı olur. Bu makalede, rol tanımlarının ayrıntıları açıklanmakta ve bazı örnekler sağlanmaktadır.

## <a name="role-definition-structure"></a>Rol tanımı yapısı

*Rol tanımı*, izinlerden oluşan bir koleksiyondur. Bazen yalnızca *rol* olarak da adlandırılır. Rol tanımı; okuma, yazma ve silme gibi gerçekleştirilebilecek işlemleri listeler. Ayrıca gerçekleştirilemeyen işlemleri veya temel verilerle ilgili işlemleri de listeleyebilir. Rol tanımı aşağıdaki yapıya sahiptir:

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

İşlemler, aşağıdaki biçime sahip dizeler ile belirtilir:

- `{Company}.{ProviderName}/{resourceType}/{action}`

Bir işlem dizesinin `{action}` bölümü, bir kaynak türü üzerinde gerçekleştirebileceğiniz işlem türünü belirtir. Örneğin, aşağıdaki alt dizeleri `{action}`görürsünüz:

| Eylem alt dizesi    | Açıklama         |
| ------------------- | ------------------- |
| `*` | Joker karakter karakteri dize ile eşleşen tüm işlemlere erişim verir. |
| `read` | Okuma işlemlerini (GET) etkinleştirilir. |
| `write` | Yazma işlemlerini (PUT veya PATCH) etkinleştirilir. |
| `action` | Sanal makineleri yeniden Başlat (POST) gibi özel işlemleri sunar. |
| `delete` | Silme işlemlerini (SILME) mümkün. |

JSON biçiminde [katılımcı](built-in-roles.md#contributor) rolü tanımı aşağıda verilmiştir. `Actions` altındaki joker karakter (`*`) işlemi, bu role atanan sorumlunun tüm eylemleri gerçekleştirebileceğini gösterir veya başka bir deyişle her şeyi yönetebilir. Bu, Azure yeni kaynak türleri ekledikçe gelecekte tanımlanacak eylemleri de içerir. `NotActions` altındaki işlemler `Actions` işlemlerinden çıkarılır. [Katkıda Bulunan](built-in-roles.md#contributor) rolünde, `NotActions` bu rolün kaynakları erişimi yönetme becerisini kaldırır ve kaynaklara erişim atar.

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

Yönetim işlemleri için rol tabanlı erişim denetimi, rol tanımının `Actions` ve `NotActions` özelliklerinde belirtilir. Azure 'daki yönetim işlemlerine ilişkin bazı örnekler aşağıda verilmiştir:

- Bir depolama hesabına erişimi yönetme
- Blob kapsayıcısı oluşturma, güncelleştirme veya silme
- Bir kaynak grubunu ve tüm kaynaklarını Sil

Yönetim erişimi, kapsayıcı kimlik doğrulama yönteminin "Azure AD Kullanıcı hesabı" olarak ayarlandığı ve "erişim anahtarı" olmadığı için verilerinize devralınmaz. Bu ayrım, joker karakter (`*`) olan rollerin verilerinize sınırsız erişmesini önler. Örneğin, bir kullanıcı abonelik üzerinde bir [okuyucu](built-in-roles.md#reader) rolüne sahipse, depolama hesabını görüntüleyebilirler, ancak varsayılan olarak temel alınan verileri görüntüleyemez.

Daha önce, rol tabanlı erişim denetimi veri işlemleri için kullanılmadı. Kaynak sağlayıcıları arasında değişen veri işlemlerine yönelik yetkilendirme. Yönetim işlemleri için kullanılan rol tabanlı erişim denetimi yetkilendirme modeli, veri işlemlerine genişletildi.

Veri işlemlerini desteklemek için, rol tanımı yapısına yeni veri özellikleri eklenmiştir. Veri işlemleri `DataActions` ve `NotDataActions` özelliklerinde belirtilir. Bu veri özellikleri eklenerek, yönetim ve veriler arasındaki ayrım korunur. Ayrıca joker karakter (`*`) içeren geçerli rol atamalarının aniden verilere erişim almasını da önler. Burada, `DataActions` ve `NotDataActions` özelliklerinde belirtilebilecek bazı veri işlemleri verilmiştir:

- Kapsayıcıdaki blobların listesini okuma
- Kapsayıcıda depolama blobu yazma
- Kuyruktaki iletiyi silme

Hem `Actions` hem de `DataActions` özelliklerinde işlemler içeren [Depolama Blobu veri okuyucusu](built-in-roles.md#storage-blob-data-reader) rol tanımı aşağıda verilmiştir. Bu rol, blob kapsayıcısını ve ayrıca temel alınan blob verilerini okumanızı sağlar.

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

`DataActions` ve `NotDataActions` özelliklerine yalnızca veri işlemleri eklenebilir. Kaynak sağlayıcıları, `isDataAction` özelliğini `true`olarak ayarlayarak hangi işlemlerin veri işlemleri olduğunu belirler. `isDataAction` `true`olduğu işlemlerin listesini görmek için bkz. [kaynak sağlayıcısı işlemleri](resource-provider-operations.md). Veri işlemlerine sahip olmayan rollerin, rol tanımında `DataActions` ve `NotDataActions` özelliklerinin olması gerekmez.

Tüm yönetim işlemi API çağrıları için yetkilendirme Azure Resource Manager tarafından işlenir. Veri işlemi API çağrıları için yetkilendirme, bir kaynak sağlayıcısı veya Azure Resource Manager tarafından işlenir.

### <a name="data-operations-example"></a>Veri işlemleri örneği

Yönetim ve veri işlemlerinin nasıl çalıştığını daha iyi anlamak için, belirli bir örneği ele alalım. Çiğdem 'e abonelik kapsamında [sahip](built-in-roles.md#owner) rolü atandı. Emre, depolama [blobu veri katılımcısı](built-in-roles.md#storage-blob-data-contributor) rolüne bir depolama hesabı kapsamında atandı. Aşağıdaki diyagramda Bu örnek gösterilmektedir.

![Rol tabanlı erişim denetimi hem yönetim hem de veri işlemlerini destekleyecek şekilde genişletildi](./media/role-definitions/rbac-management-data.png)

Gamze için [sahip](built-in-roles.md#owner) rolü ve Bob Için [Depolama Blobu veri katılımcısı](built-in-roles.md#storage-blob-data-contributor) rolü aşağıdaki eylemlere sahiptir:

Sahip

&nbsp;&nbsp;&nbsp;&nbsp;eylemler<br>
&nbsp;&nbsp;&nbsp;&nbsp;`*`

Depolama Blobu veri Katılımcısı

&nbsp;&nbsp;&nbsp;&nbsp;eylemler<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/write`<br>
&nbsp;&nbsp;&nbsp;&nbsp;veri eylemleri<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write`

Gamze 'nin bir abonelik kapsamında bir joker (`*`) eylemi olduğundan, izinleri tüm yönetim eylemlerini gerçekleştirmesini sağlamak için devralınır. Gamze kapsayıcıları okuyabilir, yazabilir ve silebilir. Ancak, Çiğdem ek adımlar uygulamadan veri işlemleri gerçekleştiremez. Örneğin, varsayılan olarak, Gamze bir kapsayıcı içindeki Blobları okuyamaz. Blobları okumak için, Çiğdem 'in depolama erişim anahtarlarını alması ve bloblara erişmek için onları kullanması gerekir.

Bob 'un izinleri yalnızca `Actions` ve [Depolama Blobu veri katılımcısı](built-in-roles.md#storage-blob-data-contributor) rolünde belirtilen `DataActions` kısıtlıdır. Bob, rol temelinde hem yönetim hem de veri işlemlerini gerçekleştirebilir. Örneğin Bob, belirtilen depolama hesabındaki kapsayıcıları okuyabilir, yazabilir ve silebilir ve ayrıca Blobları okuyabilir, yazabilir ve silebilir.

Depolama için yönetim ve veri düzlemi güvenliği hakkında daha fazla bilgi için bkz. [Azure depolama Güvenlik Kılavuzu](../storage/blobs/security-recommendations.md).

### <a name="what-tools-support-using-rbac-for-data-operations"></a>Veri işlemleri için RBAC kullanılarak hangi araçlar desteklenir?

Veri işlemlerini görüntülemek ve bunlarla çalışmak için, araçların veya SDK 'ların doğru sürümlerine sahip olmanız gerekir:

| Araç  | Sürüm  |
|---------|---------|
| [Azure PowerShell](/powershell/azure/install-az-ps) | 1.1.0 veya üzeri |
| [Azure CLI](/cli/azure/install-azure-cli) | 2.0.30 veya üzeri |
| [.NET için Azure](/dotnet/azure/) | 2.8.0-Önizleme veya üzeri |
| [Go için Azure SDK](/azure/go/azure-sdk-go-install) | 'sının 15.0.0 veya üzeri |
| [Java için Azure](/java/azure/) | 1.9.0 veya üzeri |
| [Python için Azure](/azure/python/) | 0.40.0 veya üzeri |
| [Ruby için Azure SDK](https://rubygems.org/gems/azure_sdk) | 0.17.1 veya üzeri |

REST API veri işlemlerini görüntülemek ve kullanmak için, **API sürümü** parametresini aşağıdaki sürüme veya daha yenisine ayarlamanız gerekir:

- 2018-07-01

## <a name="actions"></a>Eylemler

`Actions` izni, rolün gerçekleştirilmesine izin verdiği yönetim işlemlerini belirtir. Azure Kaynak sağlayıcılarının güvenli kılınabilir işlemlerini tanımlayan bir işlem dizeleri koleksiyonudur. Aşağıda, `Actions`kullanılabilen yönetim işlemlerine ilişkin bazı örnekler verilmiştir.

| İşlem dizesi    | Açıklama         |
| ------------------- | ------------------- |
| `*/read` | Tüm Azure Kaynak sağlayıcılarının tüm kaynak türleri için okuma işlemlerine erişim izni verir.|
| `Microsoft.Compute/*` | Microsoft. COMPUTE kaynak sağlayıcısındaki tüm kaynak türleri için tüm işlemlere erişim verir.|
| `Microsoft.Network/*/read` | Microsoft. Network kaynak sağlayıcısındaki tüm kaynak türleri için okuma işlemlerine erişim izni verir.|
| `Microsoft.Compute/virtualMachines/*` | Sanal makinelerin tüm işlemlerine ve alt kaynak türlerine erişim verir.|
| `microsoft.web/sites/restart/Action` | Bir Web uygulamasını yeniden başlatma erişimine izin verir.|

## <a name="notactions"></a>NotActions

`NotActions` izni, izin verilen `Actions`dışlanan yönetim işlemlerini belirtir. İzin vermek istediğiniz işlem kümesi kısıtlanmış işlemleri dışlayarak daha kolay tanımlanmazsa `NotActions` iznini kullanın. Bir rol tarafından verilen erişim (geçerli izinler), `Actions` işlemlerinden `NotActions` işlemleri çıkararak hesaplanır.

> [!NOTE]
> Bir kullanıcıya `NotActions`bir işlemi dışlayan bir rol atanırsa ve aynı işleme erişim izni veren ikinci bir rol atanırsa, kullanıcının bu işlemi gerçekleştirmesine izin verilir. `NotActions` reddetme kuralı değildir. belirli işlemler dışlanmanız gerektiğinde, izin verilen bir işlem kümesi oluşturmanın kolay bir yoludur.
>

## <a name="dataactions"></a>Veri eylemleri

`DataActions` izni, rolün bu nesne içindeki verilerinize yapılmasına izin verdiği veri işlemlerini belirler. Örneğin, bir kullanıcının depolama hesabına yönelik blob veri okuma erişimi varsa, bu depolama hesabındaki Blobları okuyabilirler. İşte kullanılabilecek veri işlemlerine ilişkin bazı örnekler aşağıda verilmiştir `DataActions`.

| İşlem dizesi    | Açıklama         |
| ------------------- | ------------------- |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/read` | Blob veya Blobların listesini döndürür. |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/write` | Blob yazma sonucunu döndürür. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/read` | Bir ileti döndürür. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/*` | Bir ileti veya bir ileti yazmanın ya da silmenin sonucunu döndürür. |

## <a name="notdataactions"></a>NotDataActions

`NotDataActions` izni, izin verilen `DataActions`dışlanan veri işlemlerini belirtir. Bir rol tarafından verilen erişim (geçerli izinler), `DataActions` işlemlerinden `NotDataActions` işlemleri çıkararak hesaplanır. Her kaynak sağlayıcı, veri işlemlerini karşılamak için ilgili API kümesini sağlar.

> [!NOTE]
> Bir kullanıcıya `NotDataActions`bir veri işlemini dışlayan bir rol atanırsa ve aynı veri işlemine erişim veren ikinci bir rol atanırsa, kullanıcının bu veri işlemini gerçekleştirmesine izin verilir. `NotDataActions` reddetme kuralı değildir; belirli veri işlemlerinin dışlanması gerektiğinde, izin verilen veri işlemleri kümesini oluşturmanın kolay bir yoludur.
>

## <a name="assignablescopes"></a>Astifblescopes

`AssignableScopes` özelliği, bu rol tanımının kullanılabildiği kapsamları (Yönetim grupları, abonelikler, kaynak grupları veya kaynaklar) belirtir. Rolü yalnızca gereken yönetim gruplarında, aboneliklerde veya kaynak gruplarında atama için kullanılabilir hale getirebilirsiniz. En az bir yönetim grubu, abonelik, kaynak grubu veya kaynak KIMLIĞI kullanmanız gerekir.

Yerleşik rollerin `AssignableScopes` kök kapsamına (`"/"`) ayarlanmış olması gerekir. Kök kapsamı, rolün tüm kapsamlardaki atamaya uygun olduğunu gösterir. Geçerli atanabilir kapsamların örnekleri şunlardır:

| Rol atama için kullanılabilir | Örnek |
|----------|---------|
| Bir abonelik | `"/subscriptions/{subscriptionId1}"` |
| İki abonelik | `"/subscriptions/{subscriptionId1}", "/subscriptions/{subscriptionId2}"` |
| Ağ kaynak grubu | `"/subscriptions/{subscriptionId1}/resourceGroups/Network"` |
| Bir yönetim grubu | `"/providers/Microsoft.Management/managementGroups/{groupId1}"` |
| Yönetim grubu ve abonelik | `"/providers/Microsoft.Management/managementGroups/{groupId1}", /subscriptions/{subscriptionId1}",` |
| Tüm kapsamlar (yalnızca yerleşik roller için geçerlidir) | `"/"` |

Özel roller için `AssignableScopes` hakkında daha fazla bilgi için bkz. [Azure kaynakları Için özel roller](custom-roles.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynakları için yerleşik roller](built-in-roles.md)
* [Azure kaynakları için özel roller](custom-roles.md)
* [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md)
