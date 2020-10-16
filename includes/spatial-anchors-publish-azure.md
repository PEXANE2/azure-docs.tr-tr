---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 0e6f68daf22659b880827ad027268d9757c60063
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91971434"
---
### <a name="open-the-publish-wizard"></a>Yayımlama sihirbazını açın

**Çözüm Gezgini**, **sharingservice** projesine sağ tıklayın ve ardından **Yayımla**' yı seçin.

Yayımla Sihirbazı başlatılır. 

**App Service**  >  **Oluştur App Service** bölmesini açmak için App Service**Yayımla** ' yı seçin.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure Portal’da oturum açın.

**App Service oluştur** bölmesinde **Hesap Ekle**' yi seçin ve ardından Azure aboneliğinizde oturum açın. Zaten oturum açtıysanız, açılan listeden istediğiniz hesabı seçin.

   > [!NOTE]
   > Zaten oturum açtıysanız **Oluştur** öğesini henüz seçmeyin.
   >

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [resource group intro text](resource-group.md)]

**Kaynak Grubu**’nun yanındaki **Yeni** öğesini seçin.

Kaynak grubunu **Myresourcegroup**olarak adlandırın ve ardından **Tamam**' ı seçin.

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [app-service-plan](app-service-plan.md)]

**Barındırma Planı**'nın yanındaki **Yeni**'yi seçin.

**Barındırma planını Yapılandır** bölmesinde, şu ayarları kullanın:

| Ayar | Önerilen değer | Açıklama |
|-|-|-|
|App Service planı| MySharingServicePlan | App Service planının adı |
| Konum | Batı ABD | Web uygulamasının barındırıldığı veri merkezi |
| Boyut | Ücretsiz | Barındırma özelliklerini belirleyen [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) |

**Tamam**’ı seçin.

### <a name="create-and-publish-the-web-app"></a>Web uygulaması oluşturma ve yayımlama

**Uygulama adı**alanına benzersiz bir uygulama adı girin. Geçerli karakterler şunlardır-z, 0-9 ve tireler (-) veya otomatik olarak oluşturulan benzersiz adı kabul eder. Web uygulamasının URL'si `https://<app_name>.azurewebsites.net` şeklindedir; burada `<app_name>`, uygulamanızın adıdır.

Azure kaynaklarını oluşturmaya başlamak için **Oluştur**’u seçin.

   Sihirbaz tamamlandıktan sonra, ASP.NET Core Web uygulamasını Azure 'da yayımlar ve ardından uygulamayı varsayılan tarayıcınızda açar.

  ![Azure 'da yayınlanmış bir ASP.NET Web uygulamasının ekran görüntüsü.](./media/spatial-anchors-azure/web-app-running-live.png)

Bu bölümde kullandığınız uygulama adı, biçimdeki URL ön eki olarak kullanılır `https://<app_name>.azurewebsites.net` . Bu URL 'YI daha sonra kullanmak üzere bir metin düzenleyicisine kopyalayın.
