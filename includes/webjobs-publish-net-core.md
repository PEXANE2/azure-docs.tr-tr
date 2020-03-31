---
title: include dosyası
description: include dosyası
services: app-service
author: ggailey777
ms.service: app-service
ms.topic: include
ms.date: 02/19/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 4860532e59227618ce819772887556719ecb53fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76020814"
---
1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

1. **Yayımla** iletişim kutusunda, **Microsoft Azure Uygulama Hizmeti'ni**seçin , Yeni **Oluştur'u**seçin ve ardından **Yayımla'yı**seçin.

   ![Yayımlama hedefini seçin](./media/webjobs-publish-netcore/pick-publish-target.png)

1. Uygulama **Hizmeti Oluştur** iletişim kutusunda, görüntünün altındaki tabloda belirtildiği gibi barındırma ayarlarını kullanın:

    ![App Service Oluştur iletişim kutusu](./media/webjobs-publish-netcore/app-service-dialog.png)

    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama Adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı benzersiz şekilde tanımlayan ad. |
    | **Abonelik** | Aboneliğinizi seçin | Kullanılacak Azure aboneliği. |
    | **[Kaynak Grubu](../articles/azure-resource-manager/management/overview.md)** | myResourceGroup |  İşlev uygulamanızın oluşturulacağı kaynak grubunun adı. Yeni kaynak grubu oluşturmak **Yeni**'yi seçin.|
    | **[Hosting Planı](../articles/app-service/overview-hosting-plans.md)** | App Service planı | [App Service planı](../articles/app-service/overview-hosting-plans.md), uygulamanızı barındıran web sunucusu grubunun konumunu, boyutunu ve özelliklerini belirtir. Web uygulamalarında ortak bir App Service planının kullanılacağı şekilde yapılandırma gerçekleştirerek birden fazla uygulama barındırdığınızda maliyet tasarrufu elde edebilirsiniz. Uygulama Hizmeti planları bölgeyi, örnek boyutunu, ölçek sayısını ve SKU'yu (Ücretsiz, Paylaşılan, Temel, Standart veya Premium) tanımlar. Yeni bir Uygulama Hizmeti planı oluşturmak için **Yeni'yi** seçin. |

1. Bu ayarlarla Azure'da bir Webİş ve ilgili kaynaklar oluşturmak ve proje kodunuzu dağıtmak için **Oluştur'u** tıklatın.