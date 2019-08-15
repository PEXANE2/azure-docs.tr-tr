---
title: Azure CLı (Önizleme)-Azure depolama ile bir kapsayıcı veya blob için Kullanıcı temsili SAS oluşturma
description: Azure CLı kullanarak Azure Storage 'da Azure Active Directory kimlik bilgilerini kullanarak Kullanıcı temsili SAS oluşturmayı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/12/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: blobs
ms.openlocfilehash: c8b17c4a2c58595d12a88ed3aff6c928b56f5e2c
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990812"
---
# <a name="create-a-user-delegation-sas-for-a-container-or-blob-with-the-azure-cli-preview"></a>Azure CLı (Önizleme) ile bir kapsayıcı veya blob için Kullanıcı temsili SAS oluşturma

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

Bu makalede, Azure CLı (Önizleme) ile bir kapsayıcı veya blob için Kullanıcı temsili SAS oluşturmak üzere Azure Active Directory (Azure AD) kimlik bilgilerinin nasıl kullanılacağı gösterilmektedir.

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="install-the-latest-version-of-the-azure-cli"></a>Azure CLı 'nın en son sürümünü yükler

Azure AD kimlik bilgileriyle SAS güvenliğini sağlamak için Azure CLı 'yı kullanmak için önce Azure CLı 'nin en son sürümünü yüklediğinizden emin olun. Azure CLı yükleme hakkında daha fazla bilgi için bkz. [Azure CLI 'Yı yükleme](/cli/azure/install-azure-cli).

## <a name="sign-in-with-azure-ad-credentials"></a>Azure AD kimlik bilgileriyle oturum açın

Azure AD kimlik bilgilerinizle Azure CLı 'da oturum açın. Daha fazla bilgi için bkz. [Azure CLI Ile oturum açma](/cli/azure/authenticate-azure-cli).

## <a name="assign-permissions-with-rbac"></a>RBAC ile izin atama

Azure PowerShell bir Kullanıcı temsili SAS oluşturmak için Azure CLı 'de oturum açmak üzere kullanılan Azure AD hesabına **Microsoft. Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini içeren bir rol atanmalıdır. Bu izin, Azure AD hesabının *Kullanıcı temsili anahtarını*istemesine olanak sağlar. Kullanıcı temsili anahtarı, Kullanıcı temsili SAS imzalamak için kullanılır. Depolama hesabı, kaynak grubu veya abonelik düzeyinde **Microsoft. Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini sağlayan rolün atanması gerekir.

Azure AD güvenlik sorumlusuna RBAC rolleri atamak için yeterli izniniz yoksa, hesap sahibine veya yöneticiden gerekli izinleri atamasını isteyebilirsiniz.

Aşağıdaki örnek, **Microsoft. Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini Içeren **Depolama Blobu veri katılımcısı** rolünü atar. Rol, depolama hesabı düzeyinde kapsamlandırılır.

Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Contributor" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

**Microsoft. Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini içeren yerleşik roller hakkında daha fazla bilgi için bkz. [Azure kaynakları için yerleşik roller](/role-based-access-control/built-in-roles).

## <a name="use-azure-ad-credentials-to-secure-a-sas"></a>SAS güvenliğini sağlamak için Azure AD kimlik bilgilerini kullanma

Azure CLı ile bir Kullanıcı temsili SAS oluşturduğunuzda, SAS imzalamak için kullanılan Kullanıcı temsili anahtarı sizin için örtülü olarak oluşturulur. SAS için belirttiğiniz başlangıç saati ve bitiş saati, Kullanıcı temsili anahtarı için başlangıç saati ve süre sonu zamanı olarak da kullanılır.

Kullanıcı temsili anahtarının geçerli olduğu maksimum Aralık, başlangıç tarihinden itibaren 7 gün olduğundan, başlangıç zamanının 7 gün içinde olan SAS için bir süre sonu zamanı belirtmeniz gerekir. Kullanıcı temsili anahtarının süresi dolduktan sonra SAS geçersiz, bu nedenle süre sonu 7 günden daha fazla olan bir SAS yalnızca 7 gün için geçerli olacaktır.

Bir Kullanıcı temsili SAS `--auth-mode login` oluşturulurken, ve `--as-user parameters` gereklidir. Azure depolama 'ya yapılan `--auth-mode` isteklerin Azure AD kimlik bilgilerinizle yetkilendirilmesini sağlamak için parametresi için *oturum açma* belirtin. Döndürülen sa 'ların bir Kullanıcı temsili SAS olması gerektiğini belirten parametreyibelirtin.`--as-user`

### <a name="create-a-user-delegation-sas-for-a-container"></a>Bir kapsayıcı için Kullanıcı temsili SAS oluşturma

Azure CLı ile bir kapsayıcı için Kullanıcı temsili SAS oluşturmak için [az Storage Container Generate-SAS](/cli/azure/storage/container#az-storage-container-generate-sas) komutunu çağırın.

Kapsayıcıda bir Kullanıcı temsili SAS için desteklenen izinler ekleme, oluşturma, silme, listeleme, okuma ve yazma içerir. İzinler, listedir veya Birleşik olarak belirtilebilir. Bu izinler hakkında daha fazla bilgi için bkz. [Kullanıcı TEMSILI SAS oluşturma](/rest/api/storageservices/create-a-user-delegation-sas).

Aşağıdaki örnek bir kapsayıcı için Kullanıcı temsili SAS belirteci döndürür. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az storage container generate-sas \
    --account-name <storage-account> \
    --name <container> \
    --permissions acdlrw \
    --expiry <date-time> \
    --auth-mode login \
    --as-user
```

Döndürülen Kullanıcı temsili SAS belirteci şuna benzer:

```
se=2019-07-27&sp=r&sv=2018-11-09&sr=c&skoid=<skoid>&sktid=<sktid>&skt=2019-07-26T18%3A01%3A22Z&ske=2019-07-27T00%3A00%3A00Z&sks=b&skv=2018-11-09&sig=<signature>
```

### <a name="create-a-user-delegation-sas-for-a-blob"></a>Blob için Kullanıcı temsili SAS oluşturma

Azure CLı ile bir blob için Kullanıcı temsili SAS oluşturmak için [az Storage blob Generate-SAS](/cli/azure/storage/blob#az-storage-blob-generate-sas) komutunu çağırın.

Blob üzerinde Kullanıcı temsili SAS için desteklenen izinler ekleme, oluşturma, silme, okuma ve yazma içerir. İzinler, listedir veya Birleşik olarak belirtilebilir. Bu izinler hakkında daha fazla bilgi için bkz. [Kullanıcı TEMSILI SAS oluşturma](/rest/api/storageservices/create-a-user-delegation-sas).

Aşağıdaki sözdizimi bir blob için Kullanıcı temsili SAS döndürür. Örnek, alt SAS `--full-uri` belirtecine eklenen blob URI 'sini döndüren parametresini belirtir. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az storage blob generate-sas \
    --account-name <storage-account> \
    --container-name <container> \
    --name <blob> \
    --permissions acdrw \
    --expiry <date-time> \
    --auth-mode login \
    --as-user
    --full-uri
```

Döndürülen Kullanıcı temsili SAS URI 'SI şuna benzer olacaktır:

```
https://storagesamples.blob.core.windows.net/sample-container/blob1.txt?se=2019-08-03&sp=rw&sv=2018-11-09&sr=b&skoid=<skoid>&sktid=<sktid>&skt=2019-08-02T2
2%3A32%3A01Z&ske=2019-08-03T00%3A00%3A00Z&sks=b&skv=2018-11-09&sig=<signature>
```

> [!NOTE]
> Kullanıcı temsili SAS, depolanan erişim ilkesiyle izin tanımlamayı desteklemez.

## <a name="see-also"></a>Ayrıca bkz.

- [Kullanıcı temsilciliğini oluşturma SAS (REST API)](/rest/api/storageservices/create-a-user-delegation-sas)
- [Kullanıcı temsilciyi anahtar işlemi al](/rest/api/storageservices/get-user-delegation-key)
