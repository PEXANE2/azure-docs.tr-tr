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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76020814"
---
1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

1. **Yayımla** iletişim kutusunda **Microsoft Azure App Service**' ni seçin, **Yeni oluştur**' u seçin ve ardından **Yayımla**' yı seçin.

   ![Yayımlama hedefi seçin](./media/webjobs-publish-netcore/pick-publish-target.png)

1. **App Service oluştur** iletişim kutusunda, görüntünün altındaki tabloda belirtilen barındırma ayarlarını kullanın:

    ![App Service Oluştur iletişim kutusu](./media/webjobs-publish-netcore/app-service-dialog.png)

    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı benzersiz şekilde tanımlayan ad. |
    | **Abonelik** | Aboneliğinizi seçin | Kullanılacak Azure aboneliği. |
    | **[Kaynak grubu](../articles/azure-resource-manager/management/overview.md)** | myResourceGroup |  İşlev uygulamanızın oluşturulacağı kaynak grubunun adı. Yeni kaynak grubu oluşturmak **Yeni**'yi seçin.|
    | **[Barındırma planı](../articles/app-service/overview-hosting-plans.md)** | App Service planı | [App Service planı](../articles/app-service/overview-hosting-plans.md), uygulamanızı barındıran web sunucusu grubunun konumunu, boyutunu ve özelliklerini belirtir. Web uygulamalarında ortak bir App Service planının kullanılacağı şekilde yapılandırma gerçekleştirerek birden fazla uygulama barındırdığınızda maliyet tasarrufu elde edebilirsiniz. App Service planlar bölgeyi, örnek boyutunu, ölçek sayısını ve SKU 'YU (ücretsiz, paylaşılan, temel, standart veya Premium) tanımlar. Yeni bir App Service planı oluşturmak için **Yeni** ' yi seçin. |

1. Azure 'da bu ayarlarla bir WebJob ve ilgili kaynaklar oluşturmak ve proje kodunuzu dağıtmak için **Oluştur** ' a tıklayın.