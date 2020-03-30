---
title: Azure CLI ile blob veya sıra verilerine erişimi yetkilendirme
titleSuffix: Azure Storage
description: Azure CLI ile blob veya sıra verilerine karşı veri işlemlerini nasıl yetkilendireceklerini belirtin. Veri işlemlerini Azure AD kimlik bilgilerini kullanarak, hesap erişim anahtarıyla veya paylaşılan erişim imzası (SAS) belirteciyle yetkilendirebilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 02/26/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: c7091592f8806b6f6655315ae1faace286c2c1f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78207699"
---
# <a name="authorize-access-to-blob-or-queue-data-with-azure-cli"></a>Azure CLI ile blob veya sıra verilerine erişimi yetkilendirme

Azure Depolama, Azure CLI için, blob veya sıra verilerindeki işlemleri nasıl yetkilendirmek istediğinizi belirtmenize olanak tanıyan uzantılar sağlar. Veri işlemlerine aşağıdaki yollarla yetki verebilirsiniz:

- Azure Etkin Dizini (Azure AD) güvenlik ilkesiyle. Microsoft, üstün güvenlik ve kullanım kolaylığı için Azure AD kimlik bilgilerini kullanmanızı önerir.
- Hesap erişim anahtarı veya paylaşılan erişim imzası (SAS) belirteci ile.

## <a name="specify-how-data-operations-are-authorized"></a>Veri işlemlerinin nasıl yetkilendirilmesini belirtin

Blob ve sıra verilerini okumak ve yazmak için `--auth-mode` Azure CLI komutları isteğe bağlı parametreyi içerir. Bir veri işleminin nasıl yetkilendirilenini belirtmek için bu parametreyi belirtin:

- `--auth-mode` Bir Azure AD `login` güvenlik ilkesini kullanarak oturum açabilmek için parametreyi ayarlayın (önerilir).
- Yetkilendirme `--auth-mode` için kullanılacak hesap `key` erişim anahtarını almaya çalışmak için parametreyi eski değere ayarlayın. Parametreyi `--auth-mode` atlarsanız, Azure CLI erişim anahtarını da almaya çalışır.

Parametreyi `--auth-mode` kullanmak için Azure CLI sürüm 2.0.46 veya sonrası sürümünü yüklediğinizden emin olun. Yüklü `az --version` sürümünüzü denetlemek için çalıştırın.

> [!IMPORTANT]
> Parametreyi `--auth-mode` atlar veya `key`ayarlarsanız, Azure CLI yetkilendirme için hesap erişim anahtarını kullanmaya çalışır. Bu durumda, Microsoft erişim anahtarını komutta veya **AZURE_STORAGE_KEY** ortam değişkeninde sağlamanızı önerir. Çevre değişkenleri hakkında daha fazla bilgi için, [yetkilendirme parametreleri için çevre değişkenlerini ayarla](#set-environment-variables-for-authorization-parameters)başlıklı bölüme bakın.
>
> Erişim anahtarını sağlamazsanız, Azure CLI her işlem için almak üzere Azure Depolama kaynak sağlayıcısını çağırmayı dener. Kaynak sağlayıcısına çağrı gerektiren birçok veri işlemi gerçekleştirmek, azaltmayla sonuçlanabilir. Kaynak sağlayıcı sınırları hakkında daha fazla bilgi için Azure [Depolama kaynak sağlayıcısı için Ölçeklenebilirlik ve performans hedefleri'ne](scalability-targets-resource-provider.md)bakın.

## <a name="authorize-with-azure-ad-credentials"></a>Azure AD kimlik bilgileriyle yetkilendirme

Azure AD kimlik bilgileriyle Azure CLI'de oturum açtığınızda, OAuth 2.0 erişim jetonu döndürülür. Bu belirteç, Azure CLI tarafından Blob veya Queue depolamasına karşı sonraki veri işlemlerini yetkilendirmek için otomatik olarak kullanılır. Desteklenen işlemler için artık komutla bir hesap anahtarı veya SAS belirteci geçmeniz gerekmez.

Rol tabanlı erişim denetimi (RBAC) aracılığıyla bir Azure AD güvenlik ilkesine veri blob ve sıralama izinleri atayabilirsiniz. Azure Depolama'daki RBAC rolleri hakkında daha fazla bilgi için [bkz.](storage-auth-aad-rbac.md)

### <a name="permissions-for-calling-data-operations"></a>Veri işlemlerini arama izinleri

Azure Depolama uzantıları, blob ve sıra verilerindeki işlemler için desteklenir. Hangi işlemleri çağırabileceğiniz, Azure CLI'da oturum açtığınızda Azure AD güvenlik ilkesine verilen izinlere bağlıdır. Azure Depolama kapsayıcılarına veya kuyruklarına izinler RBAC aracılığıyla atanır. Örneğin, **Blob Data Reader** rolü ne atanmışsa, bir kapsayıcıdan veya kuyruktan verileri okuyan komut dosyası komutları çalıştırabilirsiniz. **Blob Veri Katılımcısı** rolü size atanırsa, bir kapsayıcıyı veya sırayı veya içerdikleri verileri okuyan, yazan veya silen komut komutları çalıştırabilirsiniz.

Bir kapsayıcı veya kuyruktaki her Azure Depolama işlemi için gereken izinler hakkında ayrıntılı bilgi için, [OAuth belirteçleri içeren Arama depolama işlemlerine](/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens)bakın.  

### <a name="example-authorize-an-operation-to-create-a-container-with-azure-ad-credentials"></a>Örnek: Azure AD kimlik bilgilerine sahip bir kapsayıcı oluşturmak için bir işlemi yetkilendirme

Aşağıdaki örnek, Azure REKLAM kimlik bilgilerinizi kullanarak Azure CLI'den bir kapsayıcının nasıl oluşturulurunu gösterir. Kapsayıcıyı oluşturmak için Azure CLI'de oturum açmanız ve bir kaynak grubu ve depolama hesabı gerekir. Bu kaynakların nasıl oluşturularak oluşturulabildiğini öğrenmek için [Bkz. Quickstart: Azure CLI ile blob oluşturma, indirme ve listele.](../blobs/storage-quickstart-blobs-cli.md)

1. Kapsayıcıyı oluşturmadan [önce, Depolama Blob Veri Katılımcısı](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) rolünü kendinize atayın. Hesap sahibi olsanız bile, depolama hesabına karşı veri işlemleri gerçekleştirmek için açık izinlere ihtiyacınız vardır. RBAC rolleri atama hakkında daha fazla bilgi için Azure [portalında RBAC ile Azure blob ve kuyruk verilerine erişim izni için](storage-auth-aad-rbac.md)bkz.

    > [!IMPORTANT]
    > RBAC rol atamalarının yayılması birkaç dakika sürebilir.

1. Azure AD kimlik bilgilerinizi kullanarak `--auth-mode` kapsayıcıyı `login` oluşturmak için parametre kümesiyle [az depolama kapsayıcısı oluşturma](/cli/azure/storage/container#az-storage-container-create) komutunu arayın. Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

    ```azurecli
    az storage container create \
        --account-name <storage-account> \
        --name sample-container \
        --auth-mode login
    ```

## <a name="authorize-with-the-account-access-key"></a>Hesap erişim anahtarıyla yetkilendirme

Hesap anahtarına sahipseniz, herhangi bir Azure Depolama veri işlemini arayabilirsiniz. Genel olarak, hesap anahtarını kullanmak daha az güvenlidir. Hesap anahtarı tehlikeye girerse, hesabınızdaki tüm veriler tehlikeye atılabilir.

Aşağıdaki örnekte, hesap erişim anahtarını kullanarak bir kapsayıcının nasıl oluşturulacak olduğu gösterilmektedir. Hesap anahtarını belirtin `--auth-mode` ve parametreye `key` değeri sağlayın:

```azurecli
az storage container create \
    --account-name <storage-account> \
    --name sample-container \
    --account-key <key>
    --auth-mode key
```

## <a name="authorize-with-a-sas-token"></a>SAS belirteci ile yetkilendirme

Bir SAS belirteciniz varsa, SAS tarafından izin verilen veri işlemlerini çağırabilirsiniz. Aşağıdaki örnek, SAS belirteci kullanarak kapsayıcının nasıl oluşturulacak olduğunu gösterir:

```azurecli
az storage container create \
    --account-name <storage-account> \
    --name sample-container \
    --sas-token <token>
```

## <a name="set-environment-variables-for-authorization-parameters"></a>Yetkilendirme parametreleri için ortam değişkenlerini ayarlama

Azure Depolama veri işlemine yapılan her çağrıya dahil etmekten kaçınmak için ortam değişkenlerinde yetkilendirme parametreleri belirtebilirsiniz. Aşağıdaki tabloda kullanılabilir ortam değişkenleri açıklanmaktadır.

| Ortam değişkeni                  | Açıklama                                                                                                                                                                                                                                                                                                                                                                     |
|---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    AZURE_STORAGE_ACCOUNT              |    Depolama hesabı adı. Bu değişken, depolama hesabı anahtarı veya SAS belirteci ile birlikte kullanılmalıdır. İkisi de yoksa, Azure CLI kimliği doğrulanmış Azure AD hesabını kullanarak depolama hesabı erişim anahtarını almaya çalışır. Aynı anda çok sayıda komut yürütülürse, Azure Depolama kaynak sağlayıcısı azaltma sınırına ulaşılabilir. Kaynak sağlayıcı sınırları hakkında daha fazla bilgi için Azure [Depolama kaynak sağlayıcısı için Ölçeklenebilirlik ve performans hedefleri'ne](scalability-targets-resource-provider.md)bakın.             |
|    AZURE_STORAGE_KEY                  |    Depolama hesabı anahtarı. Bu değişken depolama hesabı adı ile birlikte kullanılmalıdır.                                                                                                                                                                                                                                                                          |
|    AZURE_STORAGE_CONNECTION_STRING    |    Depolama hesabı anahtarını veya SAS belirteciiçeren bir bağlantı dizesi. Bu değişken depolama hesabı adı ile birlikte kullanılmalıdır.                                                                                                                                                                                                                       |
|    AZURE_STORAGE_SAS_TOKEN            |    Paylaşılan erişim imzası (SAS) belirteci. Bu değişken depolama hesabı adı ile birlikte kullanılmalıdır.                                                                                                                                                                                                                                                            |
|    AZURE_STORAGE_AUTH_MODE            |    Komutu çalıştırmak için yetkilendirme modu. İzin verilen `login` değerler (önerilir) veya `key`. Belirtirseniz, `login`Azure CLI veri işlemini yetkilendirmek için Azure AD kimlik bilgilerinizi kullanır. Eski `key` modu belirtirseniz, Azure CLI hesap erişim anahtarını sorgulamaya ve komutu anahtarla yetkilendirmeye çalışır.    |

## <a name="next-steps"></a>Sonraki adımlar

- [Blob ve sıra verilerine erişmek için bir RBAC rolü atamak için Azure CLI'yi kullanın](storage-auth-aad-rbac-cli.md)
- [Azure kaynakları için yönetilen kimliklerle blob ve sıra verilerine erişimi yetkilendirme](storage-auth-aad-msi.md)
