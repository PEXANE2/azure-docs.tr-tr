---
title: Azure API Management örneği oluşturma | Microsoft Docs
description: Yeni bir Azure API Management örneği oluşturmak için bu öğreticideki adımları izleyin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cflower
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 63415b221b3980f3b6e3abc7d197cf9f61844f3f
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86077499"
---
# <a name="create-a-new-azure-api-management-service-instance"></a>Yeni bir Azure API Management hizmeti örneği oluşturma

Azure API Management (APIM), kuruluşların kendi veri ve hizmet potansiyellerini ortaya çıkarmak üzere API’leri dış, iş ortağı ve iç geliştiricilere yayımlamalarına yardımcı olur. API Management; geliştirici katılımı, iş öngörüleri, analizler, güvenlik ve koruma aracılığıyla başarılı bir API programı yürütmeye ilişkin temel uzmanlıklar sağlar. APIM, herhangi bir yerde barındırılan mevcut arka uç hizmetleri için modern API ağ geçitleri oluşturmanıza ve yönetmenize olanak sağlar. Daha fazla bilgi için [Genel Bakış](api-management-key-concepts.md) konusuna bakın.

Bu hızlı başlangıç, Azure portalını kullanarak yeni bir API Management örneği oluşturma adımlarını açıklar.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

![API Management örneği](./media/get-started-create-service-instance/get-started-create-service-instance-created.png)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-a-new-service"></a>Yeni hizmet oluşturma

1. Azure portal menüsünde **kaynak oluştur**' u seçin. Azure **giriş** sayfasında **kaynak oluştur** ' u de seçebilirsiniz. 
   
   ![Kaynak oluştur ' u seçin](./media/get-started-create-service-instance/00-CreateResource-01.png)
   
1. **Yeni** ekranda, **tümleştirme** ' i seçin ve ardından **API Management**' yi seçin.
   
   ![Yeni Azure API Management örneği](./media/get-started-create-service-instance/00-CreateResource-02.png)
   
1. **API Management hizmet** ekranında Ayarlar ' ı girin.
   
   ![yeni örnek](./media/get-started-create-service-instance/get-started-create-service-instance-create-new.png)
   
   | Ayar                 | Önerilen değer                               | Description                                                                                                                                                                                                                                                                                                                         |
|-------------------------|-----------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Adı**                | API Management hizmetiniz için benzersiz bir ad | Ad daha sonra değiştirilemez. Hizmet adı, *{name}.azure-api.net* biçiminde varsayılan bir etki alanı adı oluşturmak için kullanılır. Özel bir etki alanı adı kullanmak istiyorsanız bkz. [Özel etki alanı yapılandırma](configure-custom-domain.md). <br/> Hizmet adı, hizmete ve ilgili Azure kaynağına başvurmak için kullanılır. |
| **Abonelik**        | Aboneliğiniz                             | Bu yeni hizmet örneğini barındıran abonelik oluşturulur. Erişiminizin bulunduğu farklı Azure abonelikleri arasından abonelik seçebilirsiniz.                                                                                                                                                            |
| **Kaynak Grubu**      | *apimResourceGroup*                           | Yeni veya var olan bir kaynak seçebilirsiniz. Kaynak grubu; yaşam döngüsünü, izinleri ve ilkeleri paylaşan kaynakların bir koleksiyonudur. [Burada](../azure-resource-manager/management/overview.md#resource-groups) daha fazla bilgi edinin.                                                                                                  |
| **Konum**            | *Batı ABD*                                    | Yakınınızdaki coğrafi bölgeyi seçin. Açılır listede yalnızca kullanılabilir API Management hizmet bölgeleri görünür.                                                                                                                                                                                                          |
| **Kuruluş adı**   | Kuruluşunuzun adı                 | Bu ad, geliştirici portalının başlığı ve bildirim e-postalarının göndereni gibi birkaç yerde kullanılır.                                                                                                                                                                                                             |
| **Yönetici e-postası** | *yönetici \@ org.com*                               | **API Management**’tan tüm bildirimlerin gönderileceği e-posta adresini ayarlayın.                                                                                                                                                                                                                                              |
| **Fiyatlandırma Katmanı**        | *Geliştirici*                                   | Hizmeti değerlendirmek için **Geliştirici** katmanını ayarlayın. Bu katman, üretim kullanımı için değildir. API Management katmanlarını ölçeklendirme hakkında daha fazla bilgi için bkz. [yükseltme ve ölçeklendirme](upgrade-and-scale.md).                                                                                                                                    |

3. **Oluştur**’u seçin.

    > [!TIP]
    > Bir API Management hizmetinin oluşturulması genellikle 20 ile 30 dakika arasında sürer. **Panoya Sabitle** öğesinin seçilmesi, yeni oluşturulan hizmetin bulunmasını kolaylaştırır.

[!INCLUDE [api-management-navigate-to-instance](../../includes/api-management-navigate-to-instance.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, aşağıdaki adımları izleyerek kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz:

1. Azure portal, **kaynak gruplarını**arayıp seçin. **Giriş** sayfasında **kaynak grupları** ' nı da seçebilirsiniz. 

   ![Kaynak grupları gezintisi](./media/get-started-create-service-instance/00-DeleteResource-01.png)

1. **Kaynak grupları** sayfasında, kaynak grubunuzu seçin.

   ![Kaynak grupları gezintisi](./media/get-started-create-service-instance/00-DeleteResource-02.png)

1. Kaynak grubu sayfasında, **kaynak grubunu sil**' i seçin. 
   
1. Kaynak grubunuzun adını yazın ve ardından **Sil**' i seçin.

   ![Kaynak grubunu silme](./media/get-started-create-service-instance/00-DeleteResource-03.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İlk API’nizi içeri aktarma ve yayımlama](import-and-publish.md)

Bulut harcamalarınızı iyileştirmek ve kaydetmek istiyor musunuz?

> [!div class="nextstepaction"]
> [Maliyet yönetimi ile maliyetleri çözümlemeye başlayın](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)