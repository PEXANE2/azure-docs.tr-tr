---
title: Blob veya kuyruk verilerine erişmek için Azure AD kimlik bilgileriyle Azure CLı komutlarını çalıştırma
titleSuffix: Azure Storage
description: Azure CLı, Azure Depolama Blobu ve kuyruklar verilerinde komut çalıştırmak için Azure AD kimlik bilgileriyle oturum açmayı destekler. Oturum için bir erişim belirteci sağlanır ve çağrı işlemlerine yetki vermek için kullanılır. İzinler, Azure AD güvenlik sorumlusu 'na atanan RBAC rolüne bağımlıdır.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/30/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: bfbb466b7679e841c9c0154dbe28de14574d4282
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75553468"
---
# <a name="run-azure-cli-commands-with-azure-ad-credentials-to-access-blob-or-queue-data"></a>Blob veya kuyruk verilerine erişmek için Azure AD kimlik bilgileriyle Azure CLı komutlarını çalıştırma

Azure depolama, Azure CLı için, Azure Active Directory (Azure AD) kimlik bilgileriyle oturum açmanızı ve betik komutlarını çalıştırmanızı sağlayan uzantılar sağlar. Azure CLı 'da Azure AD kimlik bilgileriyle oturum açtığınızda bir OAuth 2,0 erişim belirteci döndürülür. Bu belirteç, sonraki veri işlemlerini blob veya kuyruk depolamaya göre yetkilendirmek için Azure CLı tarafından otomatik olarak kullanılır. Desteklenen işlemler için artık komutuyla bir hesap anahtarını veya SAS belirtecini iletmeniz gerekmez.

Rol tabanlı erişim denetimi (RBAC) aracılığıyla bir Azure AD güvenlik sorumlusuna blob ve kuyruk verilerine izinler atayabilirsiniz. Azure depolama 'daki RBAC rolleri hakkında daha fazla bilgi için bkz. [RBAC Ile Azure depolama verilerine erişim haklarını yönetme](storage-auth-aad-rbac.md).

## <a name="supported-operations"></a>Desteklenen işlemler

Azure depolama uzantıları, blob ve kuyruk verilerinde işlemler için desteklenir. Çağırabilmeniz gereken işlemler, Azure CLı 'de oturum açarken Azure AD güvenlik sorumlusuna verilen izinlere bağlıdır. Azure depolama kapsayıcılarının veya kuyruklarının izinleri RBAC aracılığıyla atanır. Örneğin, **BLOB veri okuyucusu** rolünü atadıysanız, bir kapsayıcıdan veya kuyruktan veri okuyan betik komutlarını çalıştırabilirsiniz. **BLOB veri katılımcısı** rolü atandıysa, bir kapsayıcı veya kuyruğu veya içerdikleri verileri okuyan, yazan veya silen betik komutlarını çalıştırabilirsiniz.

Bir kapsayıcı veya kuyruktaki her bir Azure depolama işlemi için gereken izinler hakkında daha fazla bilgi için bkz. [OAuth belirteçleriyle depolama Işlemlerini çağırma](/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens).  

## <a name="call-azure-cli-commands-using-azure-ad-credentials"></a>Azure AD kimlik bilgilerini kullanarak Azure CLı komutlarını çağırma

Azure CLı, blob ve kuyruk verileri işlemleri için `--auth-mode` parametresini destekler:

- Azure AD güvenlik sorumlusu kullanarak oturum açmak için `--auth-mode` parametresini `login` olarak ayarlayın.
- Hesap için kimlik doğrulama parametresi sağlanmazsa, bir hesap anahtarı sorgulama girişiminde `--auth-mode` parametresini eski `key` değerine ayarlayın.

Aşağıdaki örnek Azure AD kimlik bilgilerinizi kullanarak Azure CLı 'dan yeni bir depolama hesabında bir kapsayıcının nasıl oluşturulacağını göstermektedir. Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

1. Azure CLı sürüm 2.0.46 veya üstünü yüklediğinizden emin olun. Yüklü sürümünüzü denetlemek için `az --version` çalıştırın.

1. `az login` çalıştırma ve tarayıcı penceresinde kimlik doğrulama:

    ```azurecli
    az login
    ```

1. İstediğiniz aboneliği belirtin. [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) ile bir kaynak grubu oluşturun. [Az Storage Account Create](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create)kullanılarak bu kaynak grubunda bir depolama hesabı oluşturun:

    ```azurecli
    az account set --subscription <subscription-id>

    az group create \
        --name sample-resource-group-cli \
        --location eastus

    az storage account create \
        --name <storage-account> \
        --resource-group sample-resource-group-cli \
        --location eastus \
        --sku Standard_LRS \
        --encryption-services blob
    ```

1. Kapsayıcıyı oluşturmadan önce, [Depolama Blobu veri katılımcısı](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) rolünü kendinize atayın. Hesap sahibi olsanız bile, depolama hesabında veri işlemleri gerçekleştirmek için açık izinlere sahip olmanız gerekir. RBAC rolleri atama hakkında daha fazla bilgi için bkz. [Azure Blob 'a erişim verme ve Azure Portal RBAC ile kuyruk verileri](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > RBAC rol atamalarının yayılması birkaç dakika sürebilir.

1. Azure AD kimlik bilgilerinizi kullanarak kapsayıcıyı oluşturmak için `--auth-mode` parametresi `login` olarak ayarlanan [az Storage Container Create](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) komutunu çağırın:

    ```azurecli
    az storage container create \ 
        --account-name <storage-account> \ 
        --name sample-container \
        --auth-mode login
    ```

`--auth-mode` parametresiyle ilişkili ortam değişkeni `AZURE_STORAGE_AUTH_MODE`. Bir Azure depolama veri işlemine yapılan her çağrıya dahil kaçınmak için ortam değişkeninde uygun değeri belirtebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Blob ve kuyruk verilerine erişim için bir RBAC rolü atamak üzere Azure CLı 'yi kullanma](storage-auth-aad-rbac-cli.md)
- [Azure kaynakları için yönetilen kimliklerle blob ve kuyruk verilerine erişim yetkisi verme](storage-auth-aad-msi.md)
