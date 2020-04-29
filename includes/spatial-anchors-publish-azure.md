---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 0dab71b6d169e26a3d7dc208dd09efe1143fbe13
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67188060"
---
### <a name="open-the-publish-wizard"></a>Yayımlama sihirbazını açın

**Çözüm Gezgini**, **sharingservice** projesine sağ tıklayın ve **Yayımla**' yı seçin.

Yayımla Sihirbazı başlatılır. **App Service oluştur** iletişim kutusunu açmak için **App Service** > **Yayımla** ' yı seçin.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

**App Service oluştur** iletişim kutusunda **Hesap Ekle** ' yi seçin ve Azure aboneliğinizde oturum açın. Zaten oturum açtıysanız, açılan listeden istediğiniz hesabı seçin.

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

**Barındırma planını Yapılandır** iletişim kutusunda şu ayarları kullanın:

| Ayar | Önerilen değer | Açıklama |
|-|-|-|
|App Service Planı| MySharingServicePlan | App Service planının adı. |
| Konum | Batı ABD | Web uygulamasının barındırıldığı veri merkezi. |
| Boyut | Ücretsiz | Barındırma özelliklerini belirleyen [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) . |

**Tamam**’ı seçin.

### <a name="create-and-publish-the-web-app"></a>Web uygulaması oluşturma ve yayımlama

**Uygulama adı**alanına benzersiz bir uygulama adı girin (geçerli karakterler `a-z`, `0-9`ve `-`) veya otomatik olarak oluşturulan benzersiz adı kabul edin. Web uygulamasının URL'si `https://<app_name>.azurewebsites.net` şeklindedir; burada `<app_name>`, uygulamanızın adıdır.

Azure kaynaklarını oluşturmaya başlamak için **Oluştur**’u seçin.

Sihirbaz tamamlandıktan sonra, ASP.NET Core Web uygulamasını Azure 'da yayımlar ve ardından uygulamayı varsayılan tarayıcınızda açar.

![Azure’da yayımlanmış ASP.NET web uygulaması](./media/spatial-anchors-azure/web-app-running-live.png)

Bu bölümde kullandığınız uygulama adı, biçimdeki `https://<app_name>.azurewebsites.net`URL ön eki olarak kullanılır. Bu URL 'YI bir yere göz atın çünkü ihtiyacınız olacak.
