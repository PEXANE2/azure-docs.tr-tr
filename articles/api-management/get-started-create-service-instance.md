---
title: Hızlı başlangıç-Azure API Management örneği oluşturma
description: Azure portal kullanarak yeni bir Azure API Management hizmet örneği oluşturun.
author: vladvino
ms.service: api-management
ms.topic: quickstart
ms.custom: mvc
ms.date: 09/08/2020
ms.author: apimpm
ms.openlocfilehash: 104e1873a56d1ecf6e9f11a271344e9b4073e780
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90033371"
---
# <a name="quickstart-create-a-new-azure-api-management-service-instance-by-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak yeni bir Azure API Management hizmet örneği oluşturma

Azure API Management (APIM), kuruluşların kendi veri ve hizmet potansiyellerini ortaya çıkarmak üzere API’leri dış, iş ortağı ve iç geliştiricilere yayımlamalarına yardımcı olur. API Management; geliştirici katılımı, iş öngörüleri, analizler, güvenlik ve koruma aracılığıyla başarılı bir API programı yürütmeye ilişkin temel uzmanlıklar sağlar. APıM, her yerde barındırılan mevcut arka uç hizmetleri için modern API ağ geçitleri oluşturmanıza ve yönetmenize olanak sağlar. Daha fazla bilgi için bkz. [Genel Bakış](api-management-key-concepts.md).

Bu hızlı başlangıç, Azure portalını kullanarak yeni bir API Management örneği oluşturma adımlarını açıklar.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

:::image type="content" source="media/get-started-create-service-instance/get-started-create-service-instance-created.png" alt-text="API Management örneği":::

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-a-new-service"></a>Yeni hizmet oluşturma

1. Azure portalı menüsünden **Kaynak oluştur**'u seçin. Azure **giriş** sayfasında **kaynak oluştur** ' u de seçebilirsiniz. 
   
   :::image type="content" source="media/get-started-create-service-instance/00-CreateResource-01.png" alt-text="Kaynak oluştur ' u seçin":::

   
1. **Yeni** sayfada **tümleştirme**  >  **API Management**' yi seçin.

   :::image type="content" source="media/get-started-create-service-instance/00-CreateResource-02.png" alt-text="Yeni Azure API Management örneği":::
   
1. **API Management hizmeti** sayfasında, Ayarlar ' ı girin.

   :::image type="content" source="media/get-started-create-service-instance/get-started-create-service-instance-create-new.png" alt-text="Yeni örnek":::
   
   | Ayar                 | Açıklama   |                                                                     
   |-------------------------|-----------------------------------------------|
   | **Ad**                | API Management hizmetiniz için benzersiz bir ad. Ad daha sonra değiştirilemez. Hizmet adı hem hizmeti hem de karşılık gelen Azure kaynağını gösterir. <br/> Hizmet adı, varsayılan bir etki alanı adı oluşturmak için kullanılır: * \<name\> . Azure-api.net.* Özel bir etki alanı adı kullanmak istiyorsanız bkz. [Özel etki alanı yapılandırma](configure-custom-domain.md). |
   | **Abonelik**          | Bu yeni hizmet örneğini barındıran abonelik oluşturulur.   |
   | **Kaynak grubu**      |  Yeni veya mevcut bir kaynak grubu seçin. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. |
   | **Konum**          | Kullanılabilir API Management hizmet konumlarından yakınınızdaki bir coğrafi bölge seçin. | 
   | **Kuruluş adı**   | Kuruluşunuzun adı. Bu ad, geliştirici portalının başlığı ve bildirim e-postalarının göndereni gibi birkaç yerde kullanılır. |                                                         
   | **Yönetici e-postası** | **API Management** giden tüm bildirimlerin gönderileceği e-posta adresi.   |  
   | **Fiyatlandırma katmanı**        | Hizmeti değerlendirmek için **Geliştirici** katmanını seçin. Bu katman üretim kullanımı için değildir. API Management katmanlarını ölçeklendirme hakkında daha fazla bilgi için bkz. [yükseltme ve ölçeklendirme](upgrade-and-scale.md). |

3. **Oluştur**’u seçin.

    > [!TIP]
    > Genellikle, bir API Management hizmeti oluşturmak ve etkinleştirmek için 20 ila 30 dakika arasında sürer. **Panoya Sabitle** öğesinin seçilmesi, yeni oluşturulan hizmetin bulunmasını kolaylaştırır.

[!INCLUDE [api-management-navigate-to-instance](../../includes/api-management-navigate-to-instance.md)]

**Genel bakış** sayfasında hizmetinizin özelliklerini gözden geçirin.

   :::image type="content" source="media/get-started-create-service-instance/get-started-create-service-instance-created.png" alt-text="API Management örneği":::

API Management hizmet örneğiniz çevrimiçi olduğunda, bunu kullanmaya hazırsınız demektir. [Ilk API 'nizi içeri aktarma ve yayımlama](import-and-publish.md)öğreticisiyle başlayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, aşağıdaki adımları izleyerek kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz:

1. Azure portal, **kaynak gruplarını**arayıp seçin. **Giriş** sayfasında **kaynak grupları** ' nı da seçebilirsiniz. 

   :::image type="content" source="media/get-started-create-service-instance/00-DeleteResource-01.png" alt-text="Kaynak grubu gezintisi":::

1. **Kaynak grupları** sayfasında, kaynak grubunuzu seçin.

   :::image type="content" source="media/get-started-create-service-instance/00-DeleteResource-02.png" alt-text="Kaynak grubunu seçme":::

1. Kaynak grubu sayfasında, **kaynak grubunu sil**' i seçin. 
   
1. Kaynak grubunuzun adını yazın ve ardından **Sil**' i seçin.

   :::image type="content" source="media/get-started-create-service-instance/00-DeleteResource-03.png" alt-text="Kaynak grubunu silme":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İlk API’nizi içeri aktarma ve yayımlama](import-and-publish.md)
