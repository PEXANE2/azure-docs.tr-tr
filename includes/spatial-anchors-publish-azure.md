---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 0dab71b6d169e26a3d7dc208dd09efe1143fbe13
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67188060"
---
### <a name="open-the-publish-wizard"></a>Yayımlama sihirbazını açma

**Solution**Explorer'da, **SharingService** projesine sağ tıklayın ve **Yayımla'yı**seçin.

Yayımlama Sihirbazı başlar. **Uygulama Hizmeti Oluştur** iletişim kutusunu açmak için Uygulama **Hizmeti** > **Yayımla'yı** seçin.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Uygulama **Hizmeti Oluştur** iletişim **kutusunda, hesap ekle'yi** seçin ve Azure aboneliğinizde oturum açın. Zaten oturum açmışsanız, açılan listeden istediğiniz hesabı seçin.

> [!NOTE]
> Zaten oturum açtıysanız **Oluştur** öğesini henüz seçmeyin.
>

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [resource group intro text](resource-group.md)]

**Kaynak Grubu**’nun yanındaki **Yeni** öğesini seçin.

Kaynak grubunuzu **myResourceGroup** olarak adlandırıp **Tamam**’ı seçin.

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [app-service-plan](app-service-plan.md)]

**Barındırma Planı**'nın yanındaki **Yeni**'yi seçin.

Barındırma **Planını Yapılandırışla** iletişim kutusunda şu ayarları kullanın:

| Ayar | Önerilen değer | Açıklama |
|-|-|-|
|App Service Planı| MySharingServicePlan | App Service planının adı. |
| Konum | Batı ABD | Web uygulamasının barındırıldığı veri merkezi. |
| Boyut | Ücretsiz | Barındırma özelliklerini belirleyen [fiyatlandırma katmanı.](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) |

**Tamam'ı**seçin.

### <a name="create-and-publish-the-web-app"></a>Web uygulaması oluşturma ve yayımlama

Uygulama Adı'nda, benzersiz bir uygulama adı `a-z`girin (geçerli karakterler , ve `0-9` `-`), veya otomatik olarak oluşturulan benzersiz adı kabul. **App Name** Web uygulamasının URL'si `https://<app_name>.azurewebsites.net` şeklindedir; burada `<app_name>`, uygulamanızın adıdır.

Azure kaynaklarını oluşturmaya başlamak için **Oluştur**’u seçin.

Sihirbaz bittikten sonra, ASP.NET Core web uygulamasını Azure'da yayınlar ve uygulamayı varsayılan tarayıcınızda açar.

![Azure’da yayımlanmış ASP.NET web uygulaması](./media/spatial-anchors-azure/web-app-running-live.png)

Bu bölümde kullandığınız uygulama adı, biçimdeki `https://<app_name>.azurewebsites.net`URL öneki olarak kullanılır. İhtiyacınız olacak şekilde bu URL'ye dikkat edin.
