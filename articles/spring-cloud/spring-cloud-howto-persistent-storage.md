---
title: Azure İlkbahar Bulutu'nda kalıcı depolama nasıl kullanılır | Microsoft Dokümanlar
description: Azure İlkbahar Bulutu'nda kalıcı depolama nasıl kullanılır?
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/07/2019
ms.author: brendm
ms.openlocfilehash: 0e49d59386b19aa8da46b8c8e6acfe50e2124541
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278527"
---
# <a name="use-persistent-storage-in-azure-spring-cloud"></a>Azure Spring Cloud'da kalıcı depolama kullanma

Azure Yay Bulutu, uygulamanız için iki tür depolama alanı sağlar: kalıcı ve geçici.

Varsayılan olarak, Azure İlkbahar Bulutu her uygulama örneği için geçici depolama alanı sağlar. Geçici depolama, varsayılan montaj yolu /tmp ile örnek başına 5 GB ile sınırlıdır.

> [!WARNING]
> Bir uygulama örneğini yeniden başlatırsanız, ilişkili geçici depolama kalıcı olarak silinir.

Kalıcı depolama, Azure tarafından yönetilen ve uygulama başına ayrılan bir dosya paylaşım kapsayıcısıdır. Kalıcı depolama alanında depolanan veriler, bir uygulamanın tüm örnekleri tarafından paylaşılır. Azure İlkbahar Bulutu örneğinde kalıcı depolama etkinleştirilmiş en fazla 10 uygulama olabilir. Her uygulamaya 50 GB kalıcı depolama alanı tahsis edilir. Kalıcı depolama için varsayılan montaj yolu /kalıcıdır.

> [!WARNING]
> Bir uygulamanın kalıcı depolama sını devre dışı ederseniz, tüm bu depolama alanı ayrılır ve depolanan tüm veriler kaybolur.

## <a name="use-the-azure-portal-to-enable-persistent-storage"></a>Kalıcı depolamayı etkinleştirmek için Azure portalını kullanma

1. Azure portalınızın **Giriş** sayfasından **Tüm Kaynaklar'ı**seçin.

    >![Tüm Kaynaklar simgesini bulun](media/portal-all-resources.jpg)

1. Kalıcı depolama ihtiyacı olan Azure İlkbahar Bulutu kaynağını seçin. Bu örnekte, seçili uygulama **upspring**olarak adlandırılır.

    > ![Uygulamanızı seçin](media/select-service.jpg)

1. **Ayarlar** başlığı altında **Uygulamalar'ı**seçin.

1. Azure İlkbahar Bulutu hizmetleriniz bir tabloda görünür.  Kalıcı depolama alanı eklemek istediğiniz hizmeti seçin. Bu örnekte, **ağ geçidi** hizmeti seçilir.

    > ![Hizmetinizi seçin](media/select-gateway.jpg)

1. Hizmetin yapılandırma sayfasından **Yapılandırma'yı** seçin

1. Kalıcı **Depolama** sekmesini seçin ve **Etkinleştir'i**seçin.

    > ![Kalıcı depolamayı etkinleştirme](media/enable-persistent-storage.jpg)

Kalıcı depolama etkinleştirildikten sonra, boyutu ve yolu yapılandırma sayfasında gösterilir.

## <a name="use-the-azure-cli-to-modify-persistent-storage"></a>Kalıcı depolama alanını değiştirmek için Azure CLI'yi kullanın

Gerekirse, Azure CLI için Bahar Bulutu uzantısını yükleyin:

```azurecli
az extension add --name spring-cloud
```
Diğer işlemler:

* Kalıcı depolama alanı etkinleştirilmiş bir uygulama oluşturmak için:

    ```azurecli
    az spring-cloud app create -n <app> -g <resource-group> -s <service-name> --enable-persistent-storage true
    ```

* Varolan bir uygulama için kalıcı depolamayı etkinleştirmek için:

    ```azurecli
    az spring-cloud app update -n <app> -g <resource-group> -s <service-name> --enable-persistent-storage true
    ```

* Varolan bir uygulamada kalıcı depolamayı devre dışı kalmak için:

    ```azurecli
    az spring-cloud app update -n <app> -g <resource-group> -s <service-name> --enable-persistent-storage false
    ```

    > [!WARNING]
    > Bir uygulamanın kalıcı depolamasını devre dışı ederseniz, bu depolama alanının tümü ayrılır ve depolanan tüm veriler kalıcı olarak kaybolur.

## <a name="next-steps"></a>Sonraki adımlar

* Uygulama [ve hizmet kotaları](spring-cloud-quotas.md)hakkında bilgi edinin.
* Uygulamanızı el ile nasıl [ölçeklendireceklerini](spring-cloud-tutorial-scale-manual.md)öğrenin.
