---
title: Bir kapsayıcı veya blob için kullanıcı delegasyonu SAS oluşturmak için Azure CLI'yi kullanın
titleSuffix: Azure Storage
description: Azure CLI'yi kullanarak Azure Active Directory kimlik bilgileriyle bir kullanıcı delegasyonu SAS nasıl oluşturabilirsiniz öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: blobs
ms.openlocfilehash: e1a81b25042501a166cee122279d21e3702cd419
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75371998"
---
# <a name="create-a-user-delegation-sas-for-a-container-or-blob-with-the-azure-cli"></a>Azure CLI ile bir kapsayıcı veya blob için bir kullanıcı delegasyonu SAS oluşturun

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

Bu makalede, Azure CLI ile bir kapsayıcı veya blob için bir kullanıcı delegasyonu SAS oluşturmak için Azure Active Directory (Azure AD) kimlik bilgilerini nasıl kullanılacağı gösterilmektedir.

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="install-the-latest-version-of-the-azure-cli"></a>Azure CLI’nin en son sürümünü yükleme

Azure AD kimlik bilgilerine sahip bir SAS'ı güvence altına almak için Azure CLI'yi kullanmak için öncelikle Azure CLI'nin en son sürümünü yüklediğinizden emin olun. Azure CLI'yi yükleme hakkında daha fazla bilgi için Azure [CLI'yi yükleyin'](/cli/azure/install-azure-cli)e bakın.

Azure CLI'yi kullanarak bir kullanıcı delegasyonu SAS oluşturmak için sürüm 2.0.78 veya daha sonra yüklediğinizden emin olun. Yüklü sürümünüzü denetlemek için `az --version` komutu kullanın.

## <a name="sign-in-with-azure-ad-credentials"></a>Azure AD kimlik bilgileriyle oturum açın

Azure REKLAM kimlik bilgilerinizle Azure CLI'de oturum açın. Daha fazla bilgi için bkz. [Azure CLI ile oturum açma](/cli/azure/authenticate-azure-cli).

## <a name="assign-permissions-with-rbac"></a>RBAC ile izin atama

Azure PowerShell'den bir kullanıcı delegasyonu SAS oluşturmak için, Azure CLI'de oturum açmada kullanılan Azure REKLAM hesabına **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini içeren bir rol atanması gerekir. Bu izin, Azure AD hesabının *kullanıcı temsilciliği anahtarını*istemesini sağlar. Kullanıcı delegasyonu anahtarı, kullanıcı delegasyonu SAS'ı imzalamak için kullanılır. **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini sağlayan rol, depolama hesabı, kaynak grubu veya abonelik düzeyinde atanmalıdır.

Bir Azure REKLAM güvenlik ilkesine RBAC rollerini atamak için yeterli izniniz yoksa, hesap sahibinden veya yöneticisinden gerekli izinleri atamasını istemeniz gerekebilir.

Aşağıdaki örnek, **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini içeren **Depolama Blob Veri Katılımcısı** rolünü atar. Rol, depolama hesabı düzeyinde kapsamlıdır.

Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Contributor" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

**Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini içeren yerleşik roller hakkında daha fazla bilgi [için Azure kaynakları için Yerleşik rollere](../../role-based-access-control/built-in-roles.md)bakın.

## <a name="use-azure-ad-credentials-to-secure-a-sas"></a>SAS'yi korumak için Azure REKLAM kimlik bilgilerini kullanma

Azure CLI ile bir kullanıcı delegasyonu SAS oluşturduğunuzda, SAS'ı imzalamak için kullanılan kullanıcı delegasyonu anahtarı sizin için zımni olarak oluşturulur. SAS için belirttiğiniz başlangıç ve son kullanma süresi, kullanıcı delegasyonu anahtarının başlangıç ve son kullanma süresi olarak da kullanılır.

Kullanıcı delegasyonu anahtarının geçerli olduğu maksimum aralık başlangıç tarihinden itibaren 7 gün olduğundan, Başlangıç saatinden itibaren 7 gün içinde SAS için bir son kullanma süresi belirtmeniz gerekir. SAS, kullanıcı delegasyonu anahtarının süresi dolduktan sonra geçersizdir, bu nedenle 7 günden uzun bir süreye sahip bir SAS yalnızca 7 gün boyunca geçerli olacaktır.

Bir kullanıcı delegasyonu Oluştururken SAS, `--auth-mode login` ve `--as-user parameters` gereklidir. Azure *login* Depolama'ya `--auth-mode` yapılan isteklerin Azure REKLAM kimlik bilgilerinizle yetkilendirilemesi için parametre için oturum açma yı belirtin. Döndürülen `--as-user` SAS'ın bir kullanıcı delegasyonu SAS olması gerektiğini belirtmek için parametreyi belirtin.

### <a name="create-a-user-delegation-sas-for-a-container"></a>Kapsayıcı için kullanıcı delegasyonu SAS oluşturma

Azure CLI içeren bir kapsayıcı için bir kullanıcı delegasyonu SAS oluşturmak için [az depolama kapsayıcısı oluşturma-sas](/cli/azure/storage/container#az-storage-container-generate-sas) komutunu arayın.

Bir kapsayıcıdaki kullanıcı delegasyonu SAS için desteklenen izinler Ekle, Oluştur, Sil, Listele, Oku ve Yaz'ı içerir. İzinler tek tek veya birleştirilmiş olarak belirtilebilir. Bu izinler hakkında daha fazla bilgi için [bkz.](/rest/api/storageservices/create-user-delegation-sas)

Aşağıdaki örnek, bir kapsayıcı için bir kullanıcı delegasyonu SAS belirteci döndürür. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az storage container generate-sas \
    --account-name <storage-account> \
    --name <container> \
    --permissions acdlrw \
    --expiry <date-time> \
    --auth-mode login \
    --as-user
```

Döndürülen kullanıcı delegasyonu SAS belirteci aşağıdakilere benzer olacaktır:

```
se=2019-07-27&sp=r&sv=2018-11-09&sr=c&skoid=<skoid>&sktid=<sktid>&skt=2019-07-26T18%3A01%3A22Z&ske=2019-07-27T00%3A00%3A00Z&sks=b&skv=2018-11-09&sig=<signature>
```

### <a name="create-a-user-delegation-sas-for-a-blob"></a>Bir damla için bir kullanıcı delegasyonu SAS oluşturma

Azure CLI ile bir blob için bir kullanıcı delegasyonu SAS oluşturmak için [az depolama blob oluşturma-sas](/cli/azure/storage/blob#az-storage-blob-generate-sas) komutunu arayın.

Bir blob üzerinde bir kullanıcı delegasyonu SAS için desteklenen izinler Ekle, Oluştur, Sil, Oku ve Yaz içerir. İzinler tek tek veya birleştirilmiş olarak belirtilebilir. Bu izinler hakkında daha fazla bilgi için [bkz.](/rest/api/storageservices/create-user-delegation-sas)

Aşağıdaki sözdizimi bir damla için bir kullanıcı delegasyonu SAS döndürür. Örnek, eklenen SAS belirteciyle URI blob'u döndüren `--full-uri` parametreyi belirtir. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

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

SAS URI döndürülen kullanıcı delegasyonu aşağıdakilere benzer olacaktır:

```
https://storagesamples.blob.core.windows.net/sample-container/blob1.txt?se=2019-08-03&sp=rw&sv=2018-11-09&sr=b&skoid=<skoid>&sktid=<sktid>&skt=2019-08-02T2
2%3A32%3A01Z&ske=2019-08-03T00%3A00%3A00Z&sks=b&skv=2018-11-09&sig=<signature>
```

> [!NOTE]
> Bir kullanıcı delegasyonu SAS, depolanmış bir erişim ilkesiyle izintanımlamayı desteklemez.

## <a name="revoke-a-user-delegation-sas"></a>Bir kullanıcı delegasyonu SAS iptal

Bir kullanıcı delegasyonu SAS'ı Azure CLI'den iptal etmek için [az depolama hesabını iptal etme-delegasyon anahtarları](/cli/azure/storage/account#az-storage-account-revoke-delegation-keys) komutunu arayın. Bu komut, belirtilen depolama hesabıyla ilişkili tüm kullanıcı delegasyonu anahtarlarını iptal eder. Bu anahtarlarla ilişkili tüm paylaşılan erişim imzaları geçersiz kılınur.

Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az storage account revoke-delegation-keys \
    --name <storage-account> \
    --resource-group <resource-group>
```

> [!IMPORTANT]
> Hem kullanıcı delegasyonu anahtarı hem de RBAC rol atamaları Azure Depolama tarafından önbelleğe alınır, bu nedenle iptal işlemini başlattığınız zaman ile varolan bir kullanıcı delegasyonu SAS'ın geçersiz olması arasında bir gecikme olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir kullanıcı delegasyonu SAS (REST API) oluşturma](/rest/api/storageservices/create-user-delegation-sas)
- [Kullanıcı Delegasyonu Anahtarı işlemi alın](/rest/api/storageservices/get-user-delegation-key)
