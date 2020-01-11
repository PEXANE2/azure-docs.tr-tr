---
title: Özel depolama ekleme (Windows kapsayıcı)
description: Azure App Service özel bir Windows kapsayıcısında özel ağ paylaşımının nasıl iliştirileyeceğinizi öğrenin. Uygulamalar arasında dosya paylaşma, statik içeriği uzaktan yönetme ve yerel olarak erişme, vb.
author: msangapu-msft
ms.topic: article
ms.date: 7/01/2019
ms.author: msangapu
ms.openlocfilehash: c5543470f790d00158297cb7c3f0c06c5fc05e14
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75866990"
---
# <a name="configure-azure-files-in-a-windows-container-on-app-service"></a>App Service bir Windows kapsayıcısında Azure dosyalarını yapılandırma

> [!NOTE]
> Bu makale özel Windows kapsayıcıları için geçerlidir. _Linux_üzerinde App Service dağıtmak için bkz. [Azure Storage 'dan içerik](./containers/how-to-serve-content-from-azure-storage.md)sunma.
>

Bu kılavuzda, Windows kapsayıcılarında Azure depolama 'ya nasıl erişebileceğiniz gösterilmektedir. Yalnızca [Azure dosya paylaşımları](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli) ve [Premium dosya paylaşımları](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-premium-fileshare) desteklenir. Azure dosya paylaşımlarını bu nasıl yapılır ile kullanıyorsunuz. Avantajlar güvenli içerik, içerik taşınabilirlik, birden çok uygulamaya erişim ve birden çok aktarım yöntemi içerir.

## <a name="prerequisites"></a>Ön koşullar

- [Azure CLI](/cli/azure/install-azure-cli) (2.0.46 veya üzeri).
- [Azure App Service var olan bir Windows kapsayıcı uygulaması](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-windows-container)
- [Azure dosya paylaşma oluşturma](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli)
- [Azure dosya paylaşımında dosya yükleme](https://docs.microsoft.com/azure/storage/files/storage-files-deployment-guide)

> [!NOTE]
> Azure dosyaları varsayılan olmayan depolama ve Web uygulamasına dahil edilmeyen ayrı olarak faturalandırılır. Altyapı sınırlamaları nedeniyle güvenlik duvarı yapılandırması kullanımını desteklemez.
>

## <a name="limitations"></a>Sınırlamalar

- Windows kapsayıcılarındaki Azure depolama **Önizleme** aşamasındadır ve **Üretim senaryolarında** **desteklenmez** .
- Windows kapsayıcılarındaki Azure depolama, yalnızca **Azure dosya kapsayıcıları** (okuma/yazma) bağlamasını destekler.
- Windows kapsayıcılarındaki Azure depolama, Windows App Service planlarına kendi kod senaryolarınızı getirmek için şu anda **desteklenmiyor** .
- Windows kapsayıcılarındaki Azure depolama, altyapı sınırlamaları nedeniyle **depolama güvenlik duvarı** yapılandırmasını kullanmayı **desteklemez** .
- Windows kapsayıcılarındaki Azure depolama, uygulama başına **en fazla beş** bağlama noktası belirtmenizi sağlar.
- Azure depolama, bağımsız olarak faturalandırılır ve Web uygulamanıza **dahil edilmez** . [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage)hakkında daha fazla bilgi edinin.

## <a name="link-storage-to-your-web-app-preview"></a>Depolama alanını Web uygulamanıza bağlama (Önizleme)

 Azure dosya paylaşımından App Service uygulamanızdaki bir dizine bağlamak için [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) komutunu kullanın. Depolama türü AzureFiles olmalıdır.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureFiles --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory of form c:<directory name> >
```

Bunu, bir Azure dosya paylaşımıyla bağlantılı olmasını istediğiniz diğer dizinler için yapmanız gerekir.

## <a name="verify"></a>Doğrulama

Bir Azure dosya paylaşımının bir Web uygulamasıyla bağlantısı kurulduktan sonra, aşağıdaki komutu çalıştırarak bunu doğrulayabilirsiniz:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Windows kapsayıcısı (Önizleme) kullanarak bir ASP.NET uygulamasını Azure App Service geçirme](app-service-web-tutorial-windows-containers-custom-fonts.md).
