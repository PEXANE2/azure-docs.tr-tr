---
title: dosya dahil etme
description: dosya dahil etme
services: app-service
author: ggailey777
ms.service: app-service
ms.topic: include
ms.date: 06/26/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: e208b52c67f173bd0d289715b63562df656b1ec9
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009821"
---
1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

1. **Yayımla** Iletişim kutusunda **hedef**için **Azure** ' u seçin ve ardından **İleri**' yi seçin. 

1. **Belirli bir hedef**Için **Azure Web işleri** ' ni seçin ve ardından **İleri**' yi seçin.

1. **Yeni bir Azure WebJob oluştur**' u seçin.

   ![Yayımlama hedefi seçin](./media/webjobs-publish-netcore/pick-publish-target.png)

1. **App Service (Windows)** iletişim kutusunda, aşağıdaki tablodaki barındırma ayarlarını kullanın.

    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Ad** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı benzersiz şekilde tanımlayan ad. |
    | **Abonelik** | Aboneliğinizi seçin | Kullanılacak Azure aboneliği. |
    | **[Kaynak grubu](../articles/azure-resource-manager/management/overview.md)** | myResourceGroup |  İşlev uygulamanızın oluşturulacağı kaynak grubunun adı. Yeni kaynak grubu oluşturmak **Yeni**'yi seçin.|
    | **[Barındırma Planı](../articles/app-service/overview-hosting-plans.md)** | App Service planı | [App Service planı](../articles/app-service/overview-hosting-plans.md), uygulamanızı barındıran web sunucusu grubunun konumunu, boyutunu ve özelliklerini belirtir. Web uygulamalarında ortak bir App Service planının kullanılacağı şekilde yapılandırma gerçekleştirerek birden fazla uygulama barındırdığınızda maliyet tasarrufu elde edebilirsiniz. App Service planlar bölgeyi, örnek boyutunu, ölçek sayısını ve SKU 'YU (ücretsiz, paylaşılan, temel, standart veya Premium) tanımlar. Yeni bir App Service planı oluşturmak için **Yeni** ' yi seçin. |

    ![App Service Oluşturma iletişim kutusu](./media/webjobs-publish-netcore/app-service-dialog.png)

1. Azure 'da bu ayarlarla bir WebJob ve ilgili kaynak oluşturmak ve proje kodunuzu dağıtmak için **Oluştur** ' u seçin.