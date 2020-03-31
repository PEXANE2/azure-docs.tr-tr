---
title: Azure Depolama'dan Linux kapsayıcılarına içerik sunma
description: Azure Uygulama Hizmeti'nde Linux kapsayıcınıza özel ağ paylaşımını nasıl ekleştirebilirsiniz öğrenin. Dosyalar arasında paylaşın, statik içeriği uzaktan yönetin ve yerel olarak erişin, vb.
author: msangapu-msft
ms.topic: article
ms.date: 01/02/2020
ms.author: msangapu
ms.openlocfilehash: 79a4e423f7a2b6570234c958ac833cdf5c6a75e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79297926"
---
# <a name="serve-content-from-azure-storage-in-app-service-on-linux"></a>Linux'ta Uygulama Hizmetinde Azure Depolama'dan içerik sunma

> [!NOTE]
> Bu makale Linux kapsayıcıları için geçerlidir. Özel Windows kapsayıcılarına dağıtmak için, [Uygulama Hizmeti'ndeki bir Windows Kapsayıcısında Azure Dosyalarını Yapılandırma'ya](../configure-connect-to-azure-storage.md)bakın. Linux'ta Uygulama Hizmetinde Azure Depolama bir **önizleme** özelliğidir. Bu özellik **üretim senaryoları için desteklenmez.**
>

Bu kılavuz, Linux'ta Azure Depolama'nın Uygulama Hizmetine nasıl eklenilen gösteriş olduğunu gösterir. Avantajlar arasında güvenli içerik, içerik taşınabilirliği, kalıcı depolama, birden çok uygulamaya erişim ve birden çok aktarım yöntemi yer alıyor.

## <a name="prerequisites"></a>Ön koşullar

- [Azure CLI](/cli/azure/install-azure-cli) (2.0.46 veya sonrası).
- Linux uygulamasında mevcut bir [Uygulama Hizmeti.](https://docs.microsoft.com/azure/app-service/containers/)
- Azure [Depolama Hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli)
- [Azure dosya paylaşımı ve dizin.](../../storage/files/storage-how-to-use-files-cli.md)


## <a name="limitations-of-azure-storage-with-app-service"></a>Uygulama Hizmeti ile Azure Depolama Sınırlamaları

- Uygulama Hizmeti ile Azure Depolama, Linux'ta Uygulama Hizmeti ve Kapsayıcılar için Web Uygulaması için **önizlemededir.** **Üretim senaryoları**için **desteklenmez.**
- Uygulama Hizmeti ile Azure Depolama, **Azure Dosyaları kapsayıcıları** (Oku / Yaz) ve **Azure Blob kapsayıcıları** (Yalnızca Oku) montajDestekler
- Uygulama Hizmeti ile Azure Depolama, altyapı sınırlamaları nedeniyle **Depolama Güvenlik Duvarı** yapılandırmasını kullanmayı **desteklemez.**
- Uygulama Hizmeti ile Azure Depolama, uygulama başına **en fazla beş** montaj noktası belirtmenize olanak tanır.
- Bir uygulamaya monte edilmiş Azure Depolama'ya App Service FTP/FTPs uç noktaları üzerinden erişilemez. [Azure Depolama gezginini](https://azure.microsoft.com/features/storage-explorer/)kullanın.
- Azure Depolama web uygulamanıza **dahil değildir** ve ayrıca faturalandırılır. [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage)hakkında daha fazla bilgi edinin.

> [!WARNING]
> Azure Blob Depolama'yı kullanan Uygulama Hizmeti yapılandırmaları yalnızca Şubat 2020'de Okunur olacak. [Daha fazlasını öğrenin](https://github.com/Azure/app-service-linux-docs/blob/master/BringYourOwnStorage/mounting_azure_blob.md)
>

## <a name="configure-your-app-with-azure-storage"></a>Uygulamanızı Azure Depolama ile yapılandırın

[Azure Depolama hesabınızı, dosya paylaşımınızı ve dizininizi](#prerequisites)oluşturduktan sonra artık uygulamanızı Azure Depolama ile yapılandırabilirsiniz.

Uygulama Hizmeti uygulamanızdaki bir dizine bir depolama hesabı [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) takmak için komutu kullanırsınız. Depolama Türü AzureBlob veya AzureFiles olabilir. AzureFiles bu örnekte kullanılır.


> [!CAUTION]
> Web uygulamanızdaki montaj yolu olarak belirtilen dizin boş olmalıdır. Bu dizinde depolanan tüm içerik, harici bir montaj eklendiğinde silinir. Varolan bir uygulama için dosya geçişi yapıyorsunuz, başlamadan önce uygulamanızın ve içeriğinin yedeğini yapın.
>

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureFiles --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory>
```

Bunu, bir depolama hesabına bağlı olmak istediğiniz diğer dizinler için yapmalısınız.

## <a name="verify-azure-storage-link-to-the-web-app"></a>Azure Depolama bağlantısını web uygulamasına doğrula

Bir depolama kapsayıcısı bir web uygulamasına bağlandıktan sonra, aşağıdaki komutu çalıştırarak bunu doğrulayabilirsiniz:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```

## <a name="use-azure-storage-in-docker-compose"></a>Docker Oluşturma'da Azure Depolama'yı kullanma

Azure Depolama, özel id kullanılarak çoklu kapsayıcı uygulamalarıyla monte edilebilir. Özel kimlik adını görüntülemek için [`az webapp config storage-account list --name <app_name> --resource-group <resource_group>`](/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-list)çalıştırın.

*Docker-compose.yml* dosyanızda, seçeneğini `volumes` `custom-id`belirleyin. Örnek:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - <custom-id>:<path_in_container>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Uygulama Hizmeti'nde web uygulamalarını yapılandırın.](../configure-common.md)

