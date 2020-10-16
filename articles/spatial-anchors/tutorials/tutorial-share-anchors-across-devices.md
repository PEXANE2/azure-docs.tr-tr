---
title: 'Öğretici: oturumlar ve cihazlar arasında bağlantıları paylaşma'
description: Bu öğreticide, bir arka uç hizmetiyle Unity 'de Android/iOS cihazları arasında Azure uzamsal bağlayıcı tanımlayıcıları paylaşmayı öğreneceksiniz.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 07/31/2020
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: b29873e161b64c52abbfdf3f2611714f6b012361
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92097295"
---
# <a name="tutorial-share-spatial-anchors-across-sessions-and-devices"></a>Öğretici: oturumlarda ve cihazlarda uzamsal bağlayıcıları paylaşma

Azure uzamsal bağlantıları, zaman içinde cihazlarda konumlarını sürekli tutan nesneleri kullanarak karma gerçeklik deneyimleri oluşturabileceğiniz platformlar arası bir geliştirici hizmetidir. 

Bu öğreticide, bir oturum sırasında bağlayıcı oluşturmak ve ardından bunları aynı cihazda veya farklı bir cihazla bulmak için [Azure uzamsal bağlayıcılarını](../overview.md) kullanırsınız. Aynı bağlayıcı aynı yerde ve aynı anda birden çok cihaz tarafından da bulunabilir.

![Bir mobil cihazla oluşturulan ve günler boyunca farklı bir cihazla kullanılan uzamsal bağlantıları gösteren animasyon.](./media/persistence.gif)


Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Bağlantıları paylaşmak için kullanabileceğiniz Azure 'da bir ASP.NET Core Web uygulaması dağıtın ve belirli bir süre için bağlantıları bellekte saklayın.
> * Paylaşım bağlantıları web uygulamasından yararlanmak için, hızlı başlangıçlarımızın Unity örneğindeki AzureSpatialAnchorsLocalSharedDemo sahneyi yapılandırın.
> * Bağlayıcıları dağıtıp bir veya daha fazla cihaza çalıştırın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

> [!NOTE]
> Bu öğreticide Unity ve bir ASP.NET Core Web uygulaması kullanıyorsunuz, ancak buradaki yaklaşım yalnızca Azure uzamsal bağlayıcı tanımlayıcılarının diğer cihazlarda nasıl paylaşılacağını gösteren bir örnek sağlamaktır. Aynı amaca ulaşmak için diğer dilleri ve arka uç teknolojilerini kullanabilirsiniz.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-the-sample-project"></a>Örnek projeyi indirin

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-the-sharing-anchors-service"></a>Paylaşım bağlantıları hizmetini dağıtma

## <a name="visual-studio"></a>[Visual Studio](#tab/VS)

Visual Studio 'yu açın ve ardından projeyi *Sharing\sharingservicesample* klasöründe açın.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

## <a name="visual-studio-code"></a>[Visual Studio Code](#tab/VSC)

Hizmeti Visual Studio Code dağıtmadan önce bir kaynak grubu ve bir App Service planı oluşturmanız gerekir.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

<a href="https://portal.azure.com/" target="_blank">Azure Portal</a>gidin ve Azure aboneliğinizde oturum açın.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

**Kaynak Grubu**’nun yanındaki **Yeni** öğesini seçin.

Kaynak grubunu **Myresourcegroup**olarak adlandırın ve ardından **Tamam**' ı seçin.

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [app-service-plan](../../../includes/app-service-plan.md)]

**Barındırma Planı**'nın yanındaki **Yeni**'yi seçin.

**Barındırma planını Yapılandır** bölmesinde, şu ayarları kullanın:

| Ayar | Önerilen değer | Açıklama |
|-|-|-|
|App Service planı| MySharingServicePlan | App Service planının adı |
| Konum | Batı ABD | Web uygulamasının barındırıldığı veri merkezi |
| Boyut | Ücretsiz | Barındırma özelliklerini belirleyen [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) |

**Tamam**’ı seçin.

Visual Studio Code açın ve ardından projeyi *Sharing\sharingservicesample* klasöründe açın. 

Paylaşım hizmetini Visual Studio Code aracılığıyla dağıtmak için, <a href="/aspnet/core/tutorials/publish-to-azure-webapp-using-vscode?view=aspnetcore-2.2#open-it-with-visual-studio-code" target="_blank">Visual Studio Code Ile Azure 'da ASP.NET Core uygulama yayımlama</a>bölümündeki yönergeleri izleyin. "Visual Studio Code ile aç" bölümünde başlatın. Zaten dağıtılacak ve yayımlanacak bir projeniz olduğundan, önceki adımda açıklandığı gibi başka bir ASP.NET projesi oluşturmayın: SharingServiceSample.

---

## <a name="deploy-the-sample-app"></a>Örnek uygulamayı dağıtma

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure 'da bir ASP.NET Core Web uygulaması dağıttınız ve bir Unity uygulamasını yapılandırmış ve dağıttınız. Uygulamayla uzamsal bağlayıcı oluşturdunuz ve bunları ASP.NET Core Web uygulamanızı kullanarak diğer cihazlarla paylaştırmadınız.

ASP.NET Core Web uygulamanızı, paylaşılan uzamsal bağlayıcı tanımlayıcılarınızın depolanmasını sürdürmek için Azure Cosmos DB kullanacak şekilde geliştirebilirsiniz. Azure Cosmos DB destek ekleyerek, ASP.NET Core Web uygulamanızın bir bağlantı noktasını hemen oluşturmasını sağlayabilirsiniz. Daha sonra, Web uygulamanızda saklanan tutturucu tanımlayıcıyı kullanarak, uygulamanın daha sonra bağlayıcıyı yeniden bulması için gün döndürmesini sağlayabilirsiniz.

> [!div class="nextstepaction"]
> [Bağlayıcıları depolamak için Azure Cosmos DB kullanma](./tutorial-use-cosmos-db-to-store-anchors.md)