---
title: Özel depolama alanı ekleme (Windows kapsayıcısı)
description: Azure Uygulama Hizmeti'ndeki özel bir Windows kapsayıcısına özel ağ paylaşımını nasıl ekleştirebilirsiniz öğrenin. Dosyalar arasında paylaşın, statik içeriği uzaktan yönetin ve yerel olarak erişin, vb.
author: msangapu-msft
ms.topic: article
ms.date: 7/01/2019
ms.author: msangapu
ms.openlocfilehash: 64ef4dfe81e6415f1285a74962e2123507715119
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77120677"
---
# <a name="configure-azure-files-in-a-windows-container-on-app-service"></a>Uygulama Hizmetinde Windows Kapsayıcısında Azure Dosyalarını Yapılandırma

> [!NOTE]
> Bu makale, özel Windows kapsayıcıları için geçerlidir. _Linux'taki_Uygulama Hizmeti'ne dağıtmak için [Azure Depolama'dan Hizmet İçeriği'ne](./containers/how-to-serve-content-from-azure-storage.md)bakın.
>

Bu kılavuz, Windows Kapsayıcılarında Azure Depolama'ya nasıl erişilenleri gösterir. Yalnızca [Azure Dosyaları Paylaşımları](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli) ve [Premium Dosya Paylaşımları](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-premium-fileshare) desteklenir. Azure Files Shares'i bu nasıl yapılsa da kullanırsınız. Avantajlar arasında güvenli içerik, içerik taşınabilirliği, birden çok uygulamaya erişim ve birden çok aktarım yöntemi yer alıyor.

## <a name="prerequisites"></a>Ön koşullar

- [Azure CLI](/cli/azure/install-azure-cli) (2.0.46 veya sonrası).
- [Azure Uygulama Hizmeti'nde varolan bir Windows Kapsayıcı uygulaması](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-windows-container)
- [Azure dosya paylaşımı oluşturma](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli)
- [Dosyaları Azure Dosyası paylaşımına yükleme](https://docs.microsoft.com/azure/storage/files/storage-files-deployment-guide)

> [!NOTE]
> Azure Dosyaları varsayılan olmayan depolama alanıdır ve web uygulamasına dahil olmayan ayrı ayrı faturalandırılır. Altyapı sınırlamaları nedeniyle Güvenlik Duvarı yapılandırmasını kullanmayı desteklemez.
>

## <a name="limitations"></a>Sınırlamalar

- Windows kapsayıcılarında Bulunan Azure Depolama **önizlemede** ve **üretim senaryoları**için **desteklenmiyor.**
- Windows kapsayıcılarında bulunan Azure Depolama, **yalnızca Azure Dosyaları kapsayıcılarına** (Oku / Yaz) montajı destekler.
- Windows kapsayıcılarında bulunan Azure Depolama, Windows Uygulama Hizmeti planlarında kendi kod senaryolarınızı getirmek için şu anda **desteklenmez.**
- Windows kapsayıcılarında bulunan Azure Depolama, altyapı sınırlamaları nedeniyle **Depolama Güvenlik Duvarı** yapılandırmasını kullanmayı **desteklemez.**
- Windows kapsayıcılarında bulunan Azure Depolama, uygulama başına **en fazla beş** montaj noktası belirtmenize olanak tanır.
- Bir uygulamaya monte edilmiş Azure Depolama'ya App Service FTP/FTPs uç noktaları üzerinden erişilemez. [Azure Depolama gezginini](https://azure.microsoft.com/features/storage-explorer/)kullanın.
- Azure Depolama bağımsız olarak faturalandırılır ve web uygulamanıza **dahil edilmez.** [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage)hakkında daha fazla bilgi edinin.

## <a name="link-storage-to-your-web-app-preview"></a>Depolama alanını web uygulamanıza bağla (önizleme)

 Uygulama Hizmeti uygulamanızdaki bir dizine Azure Dosyaları Paylaşımı monte [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) etmek için komutu kullanırsınız. Depolama Türü Azure Dosyaları olmalıdır.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureFiles --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory of form c:<directory name> >
```

Bunu, Azure Dosyaları paylaşımına bağlı olmak istediğiniz diğer dizinler için yapmalısınız.

## <a name="verify"></a>Doğrulama

Azure Dosyaları paylaşımı bir web uygulamasına bağlandıktan sonra, aşağıdaki komutu çalıştırarak bunu doğrulayabilirsiniz:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Bir ASP.NET uygulamasını Windows kapsayıcısı (Önizleme) kullanarak Azure Uygulama Hizmetine geçirin.](app-service-web-tutorial-windows-containers-custom-fonts.md)
