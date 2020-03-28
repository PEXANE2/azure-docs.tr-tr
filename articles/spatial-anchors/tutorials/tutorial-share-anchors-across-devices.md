---
title: 'Öğretici: Bağlantı bağlantılarını oturumlar ve aygıtlar arasında paylaşın'
description: Bu eğitimde, Azure Uzamsal Bağlantı tanımlayıcılarını Bir arada bir hizmetle Birlikte Android/iOS aygıtları arasında nasıl paylaşabileceğinizi öğrenirsiniz.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 3b377f87bdba40c90cb3af6caef2c089d7b7de49
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77615486"
---
# <a name="tutorial-share-azure-spatial-anchors-across-sessions-and-devices"></a>Öğretici: Azure Uzamsal Bağlantı larını oturumlar ve aygıtlar arasında paylaşın

Bu öğreticide, bir oturum sırasında çapa oluşturmak ve bunları aynı aygıtta veya farklı bir oturumda bulmak için [Azure Uzamsal Bağlantı](../overview.md) Larını nasıl kullanacağınızı öğreneceksiniz. Bu aynı çapalar aynı yerde ve aynı anda birden çok aygıt tarafından da bulunabilir.

![Kalıcılık](./media/persistence.gif)

Azure Uzamsal Çapalar, zaman içinde aygıtlar arasında konumlarını devam ettirmeye neden olan nesneleri kullanarak karışık gerçeklik deneyimleri oluşturmanıza olanak tanıyan bir çapraz platform geliştirici hizmetidir. Bitirdikten sonra, iki veya daha fazla cihaza dağıtılabilen bir uygulamanız olur. Bir örnek tarafından oluşturulan Azure Uzamsal Çapalar diğerleriyle paylaşılabilir.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Azure'da, bağlantı larını paylaşmak için kullanılabilecek ve bunları bir süre bellekte depolayan bir ASP.NET Çekirdek Web Uygulaması dağıtın.
> * Paylaşım Çapaları Web Uygulamasından yararlanmak için Quickstarts'ımızdan Unity Sample içindeki AzureSpatialAnchorsLocalSharedDemo sahnesini yapılandırın.
> * Bir veya daha fazla aygıta dağıtın ve çalıştırın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Bu Öğreticide Unity ve ASP.NET Core Web Uygulamasını kullanıyor olsanız da, bunun yalnızca Azure Uzamsal Bağlantı tanımlayıcılarını diğer aygıtlarda nasıl paylaşacağınız hakkında bir örnek göstereceğiniz için olduğunu belirtmekte değerlidir. Aynı amaca ulaşmak için diğer dilleri ve arka uç teknolojilerini kullanabilirsiniz. Ayrıca, bu Öğretici'de kullanılan ASP.NET Core Web Uygulaması .NET Core 2.2 SDK'ya bağımlıdır. Normal Azure Web Apps'ta (Windows için) iyi çalışır, ancak şu anda Linux için Azure Web Apps'ta çalışmaz.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-the-sample-project"></a>Örnek projeyi indirin

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-your-sharing-anchors-service"></a>Paylaşım Çapaları Hizmetinizi Dağıtın

## <a name="visual-studio"></a>[Visual Studio](#tab/VS)

Visual Studio'yu açın ve `Sharing\SharingServiceSample` projeyi klasörde açın.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

## <a name="visual-studio-code"></a>[Visual Studio Code](#tab/VSC)

Hizmeti VS Kodu'nda dağıtmadan önce bir kaynak grubu ve bir Uygulama Hizmet Planı oluşturmanız gerekir.

### <a name="sign-in-to-azure"></a>Azure'da Oturum Açma

<a href="https://portal.azure.com/" target="_blank">Azure portalına</a> gidin ve Azure aboneliğinizde oturum açın.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

**Kaynak Grubu**’nun yanındaki **Yeni** öğesini seçin.

Kaynak grubunuzu **myResourceGroup** olarak adlandırıp **Tamam**’ı seçin.

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [app-service-plan](../../../includes/app-service-plan.md)]

**Barındırma Planı**'nın yanındaki **Yeni**'yi seçin.

Barındırma **Planını Yapılandırışla** iletişim kutusunda şu ayarları kullanın:

| Ayar | Önerilen değer | Açıklama |
|-|-|-|
|App Service Planı| MySharingServicePlan | App Service planının adı. |
| Konum | Batı ABD | Web uygulamasının barındırıldığı veri merkezi. |
| Boyut | Ücretsiz | Barındırma özelliklerini belirleyen [fiyatlandırma katmanı.](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) |

**Tamam'ı**seçin.

Visual Studio Code'u açın ve `Sharing\SharingServiceSample` projeyi klasörde açın. Visual Studio Code aracılığıyla paylaşım hizmetini dağıtmak için <a href="https://docs.microsoft.com/aspnet/core/tutorials/publish-to-azure-webapp-using-vscode?view=aspnetcore-2.2#open-it-with-visual-studio-code" target="_blank">bu öğreticiyi</a> izleyin. 'Visual Studio Code ile aç' bölümünden başlayarak aşağıdaki adımları takip edebilirsiniz. Zaten dağıtılması ve yayınlanması gereken proje- SharingServiceSample var gibi yukarıdaki adımda açıklandığı gibi başka bir mvc proje oluşturmayın.

---

## <a name="deploy-the-sample-app"></a>Örnek uygulamayı dağıtma

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

## <a name="troubleshooting"></a>Sorun giderme

### <a name="unity-20193"></a>Birlik 2019.3

Kırılan değişiklikler nedeniyle, Unity 2019.3 şu anda desteklenmiyor. Lütfen Unity 2019.1 veya 2019.2'yi kullanın.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure'da bir ASP.NET Çekirdek Web Uygulaması dağıttınız ve ardından bir Birlik Uygulaması yapılandırıp dağıttınız. Uygulamayla Uzamsal Çapalar oluşturdunuz ve ASP.NET Core Web Uygulamanızı kullanarak bunları diğer cihazlarla paylaştınız.

paylaşılan Uzamsal Bağlantı tanımlayıcılarınızın depolamasına devam etmek için Azure Cosmos DB'yi kullanması için ASP.NET Core Web Uygulamanızı geliştirebilirsiniz. Azure Cosmos DB desteği eklemek, ASP.NET Çekirdek Web Uygulamanızın bugün bir bağlantı oluşturmasına ve web uygulamanızda depolanan bağlantı tanımlayıcısını kullanarak onu yeniden bulabilmek için günler sonra geri gelmesine olanak tanır.

> [!div class="nextstepaction"]
> [Çapaları Depolamak için Azure Cosmo DB'yi kullanın](./tutorial-use-cosmos-db-to-store-anchors.md)

