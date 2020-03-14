---
title: Azure depolama 'dan Linux kapsayıcılarına içerik sunma
description: Azure App Service ' de Linux kapsayıcınıza özel ağ paylaşımının nasıl ekleneceğini öğrenin. Uygulamalar arasında dosya paylaşma, statik içeriği uzaktan yönetme ve yerel olarak erişme, vb.
author: msangapu-msft
ms.topic: article
ms.date: 01/02/2020
ms.author: msangapu
ms.openlocfilehash: 79a4e423f7a2b6570234c958ac833cdf5c6a75e4
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79297926"
---
# <a name="serve-content-from-azure-storage-in-app-service-on-linux"></a>Linux üzerinde App Service Azure Storage 'tan içerik sunma

> [!NOTE]
> Bu makale, Linux kapsayıcıları için geçerlidir. Özel Windows kapsayıcılarına dağıtmak için bkz. [App Service bir Windows kapsayıcısında Azure dosyalarını yapılandırma](../configure-connect-to-azure-storage.md). Linux üzerinde App Service Azure Storage bir **Önizleme** özelliğidir. Bu özellik **Üretim senaryolarında desteklenmez**.
>

Bu kılavuzda, Linux üzerinde App Service Azure Storage 'ın nasıl ekleneceği gösterilmektedir. Avantajlar, güvenli içerik, içerik taşınabilirlik, kalıcı depolama, birden çok uygulamaya erişim ve birden fazla aktarım yöntemi içerir.

## <a name="prerequisites"></a>Önkoşullar

- [Azure CLI](/cli/azure/install-azure-cli) (2.0.46 veya üzeri).
- [Linux uygulamasında](https://docs.microsoft.com/azure/app-service/containers/)mevcut bir App Service.
- Bir [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli)
- Bir [Azure dosya paylaşma ve dizini](../../storage/files/storage-how-to-use-files-cli.md).


## <a name="limitations-of-azure-storage-with-app-service"></a>App Service ile Azure depolama 'nın sınırlamaları

- App Service ile Azure depolama, Linux ve Kapsayıcılar için Web App App Service için **Önizleme** aşamasındadır. **Üretim senaryolarında**bu **desteklenmez** .
- App Service ile Azure depolama, **Azure dosya kapsayıcıları** (okuma/yazma) ve **Azure Blob kapsayıcıları** (salt okuma) bağlamayı destekler
- App Service ile Azure depolama, altyapı sınırlamaları nedeniyle **depolama güvenlik duvarı** yapılandırmasını kullanmayı **desteklemez** .
- App Service ile Azure depolama, uygulama başına **en fazla beş** bağlama noktası belirtmenizi sağlar.
- Bir uygulamaya bağlı Azure Storage App Service FTP/FTPs uç noktaları aracılığıyla erişilebilir değildir. [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)'ni kullanın.
- Azure depolama, Web uygulamanıza **dahil değildir** ve ayrı olarak faturalandırılır. [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage)hakkında daha fazla bilgi edinin.

> [!WARNING]
> Azure Blob depolama kullanan App Service yapılandırma, Şubat 2020 ' de salt okunabilir hale gelir. [Daha fazla bilgi](https://github.com/Azure/app-service-linux-docs/blob/master/BringYourOwnStorage/mounting_azure_blob.md)
>

## <a name="configure-your-app-with-azure-storage"></a>Uygulamanızı Azure Storage ile yapılandırma

[Azure depolama hesabınızı, dosya paylaşımınızı ve dizininizi](#prerequisites)oluşturduktan sonra uygulamanızı Azure Storage ile yapılandırabilirsiniz.

Bir depolama hesabını App Service uygulamanızdaki bir dizine bağlamak için [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) komutunu kullanın. Depolama türü AzureBlob veya AzureFiles olabilir. Bu örnekte AzureFiles kullanılır.


> [!CAUTION]
> Web uygulamanızda bağlama yolu olarak belirtilen dizin boş olmalıdır. Dış bağlama eklendiğinde, bu dizinde depolanan içerikler silinir. Mevcut bir uygulama için dosyaları geçiriyorsanız, başlamadan önce uygulamanızın ve içeriğinin yedeğini alın.
>

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureFiles --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory>
```

Bunu, bir depolama hesabına bağlanmasını istediğiniz diğer dizinler için yapmanız gerekir.

## <a name="verify-azure-storage-link-to-the-web-app"></a>Web uygulamasına Azure Storage bağlantısını doğrulama

Bir depolama kapsayıcısı bir Web uygulamasına bağlandıktan sonra, aşağıdaki komutu çalıştırarak bunu doğrulayabilirsiniz:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```

## <a name="use-azure-storage-in-docker-compose"></a>Docker Compose 'de Azure Storage 'ı kullanma

Azure depolama, özel kimlik kullanarak çok Kapsayıcılı uygulamalarla bağlanabilir. Özel kimlik adını görüntülemek için [`az webapp config storage-account list --name <app_name> --resource-group <resource_group>`](/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-list)çalıştırın.

*Docker-Compose. yıml* dosyanızda `volumes` seçeneğini `custom-id`eşleyin. Örnek:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - <custom-id>:<path_in_container>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure App Service Web uygulamalarını yapılandırın](../configure-common.md).

