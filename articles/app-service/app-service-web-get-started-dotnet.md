---
title: 'Hızlı başlangıç: C# ASP.NET Core uygulaması oluşturma'
description: Visual Studio 'dan varsayılan C# ASP.NET Core Web uygulaması şablonunu dağıtarak Azure App Service Web uygulamalarının nasıl çalıştırılacağını öğrenin.
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.topic: quickstart
ms.date: 04/22/2020
ms.custom: mvc, devcenter, vs-azure, seodec18
ms.openlocfilehash: 6c7ab1290033792cf356c4882811670011df5efe
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82086054"
---
# <a name="quickstart-create-an-aspnet-core-web-app-in-azure"></a>Hızlı başlangıç: Azure 'da ASP.NET Core Web uygulaması oluşturma

Bu hızlı başlangıçta, [Azure App Service](overview.md)için ilk ASP.NET Core Web uygulamanızı nasıl oluşturacağınızı ve dağıtacağınızı öğreneceksiniz. 

İşiniz bittiğinde, bir App Service barındırma planından ve dağıtılan bir Web uygulamasına sahip bir App Service içeren bir Azure Kaynak grubunuz olacaktır.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/dotnet/).
- Bu hızlı başlangıçta Windows üzerinde App Service bir uygulama dağıtılır. _Linux_ üzerinde App Service'e dağıtım yapmak için bkz. [Linux üzerinde App Service'te .NET Core web uygulaması oluşturma](./containers/quickstart-dotnetcore.md).
- **ASP.net ve Web geliştirme** iş yüküyle <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> ' i yükledikten sonra.

  Visual Studio 2019 zaten yüklüyse:

  - **Help** > **Güncelleştirmeler için yardım denetimi**' ni seçerek en son güncelleştirmeleri Visual Studio 'ya yükler.
  - **Araçlar** > **Al araçlar ve Özellikler '** i seçerek iş yükünü ekleyin.


## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Aşağıdaki adımları izleyerek Visual Studio 'da bir ASP.NET Core Web uygulaması oluşturun:

1. Visual Studio 'Yu açın ve **Yeni proje oluştur**' u seçin.

1. **Yeni proje oluştur**' da, **ASP.NET Core Web uygulaması** ' nı seçin ve **C#** ' ın bu seçime Ilişkin dillerde listelendiğini onaylayın, ardından **İleri**' yi seçin.

1. **Yeni projenizi yapılandırın**bölümünde, Web uygulaması projenizi *Myfirstazurewebapp*olarak adlandırın ve **Oluştur**' u seçin.

   ![Web uygulaması projenizi yapılandırma](./media/app-service-web-get-started-dotnet/configure-web-app-project.png)

1. Azure 'a herhangi bir türde ASP.NET Core Web uygulaması dağıtabilirsiniz, ancak bu hızlı başlangıç için **Web uygulaması** şablonunu seçin. **Kimlik doğrulamasının** **kimlik doğrulaması yok**olarak ayarlandığından ve başka bir seçeneğin seçilolmadığından emin olun. Ardından **Oluştur**' u seçin.

   ![Yeni bir ASP.NET Core Web uygulaması oluşturma](./media/app-service-web-get-started-dotnet/create-aspnet-core-web-app.png) 
   
1. Web uygulamanızı yerel olarak çalıştırmak için Visual Studio **menüsünden hata ayıklama** > **olmadan Başlat** ' ı seçin.

   ![Yerel olarak çalışan Web uygulaması](./media/app-service-web-get-started-dotnet/web-app-running-locally.png)

## <a name="publish-your-web-app"></a>Web uygulamanızı yayımlama

Web uygulamanızı yayımlamak için öncelikle uygulamanızı yayımlayacağınız yeni bir App Service oluşturmanız ve yapılandırmanız gerekir. 

App Service ayarlamanın bir parçası olarak şunları oluşturursunuz:

- Hizmet için tüm Azure kaynaklarını içerecek yeni bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/management/overview#terminology) .
- Uygulamanızı barındıran Web sunucusu grubunun konumunu, boyutunu ve özelliklerini belirten yeni bir [barındırma planı](https://docs.microsoft.com/azure/app-service/overview-hosting-plans) .

App Service oluşturup Web uygulamanızı yayımlamak için aşağıdaki adımları izleyin:

1. **Çözüm Gezgini**, **Myfirstazurewebapp** projesine sağ tıklayın ve **Yayımla**' yı seçin. Daha önce Visual Studio 'dan Azure hesabınızda oturum açmadıysanız, **Hesap Ekle** veya **oturum aç**' ı seçin. Ayrıca, ücretsiz bir Azure hesabı da oluşturabilirsiniz.

1. **Bir yayımlama hedefi seç** iletişim kutusunda **App Service**' i seçin, **Yeni oluştur**' u seçin ve ardından **Profil oluştur**' u seçin.

   ![Yayımlama hedefi seçme](./media/app-service-web-get-started-dotnet/pick-publish-target-vs2019.png)

1. **App Service: Yeni oluştur** iletişim kutusunda varsayılan adı kabul ederek ya da yeni bir ad girerek uygulamanız için genel olarak benzersiz bir **ad** sağlayın. `a-z`Geçerli karakterler: `A-Z`,, `0-9`, ve. `-` Bu **ad** , Web uygulamanızın URL ön eki olarak biçiminde `http://<app_name>.azurewebsites.net`kullanılır.

1. **Abonelik**için, listelenen aboneliği kabul edin veya açılan listeden yeni bir tane seçin.

1. **Kaynak grubu**' nda **Yeni**' yi seçin. **Yeni kaynak grubu adı**alanına *Myresourcegroup* yazın ve **Tamam**' ı seçin. 

1. **Barındırma planı**için **Yeni**' yi seçin. 

1. **Barındırma planı: Yeni oluştur** iletişim kutusunda, aşağıdaki tabloda belirtilen değerleri girin:

   | Ayar  | Önerilen Değer | Açıklama |
   | -------- | --------------- | ----------- |
   | **Barındırma Planı**  | *myFirstAzureWebAppPlan* | App Service planının adı. |
   | **Konum**      | *Batı Avrupa* | Web uygulamasının barındırıldığı veri merkezi. |
   | **Boyut**          | *Ücretsiz* | [Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), barındırma özelliklerini belirler. |
   
   ![Yeni barındırma planı oluştur](./media/app-service-web-get-started-dotnet/create-new-hosting-plan-vs2019.png)

1. **Application Insights** , *none*olarak ayarlı bırakın.

1. **App Service: Yeni oluştur** iletişim kutusunda, Azure kaynaklarını oluşturmaya başlamak için **Oluştur** ' u seçin.

   ![Yeni App Service oluştur](./media/app-service-web-get-started-dotnet/create-new-app-service-vs2019.png)

1. Sihirbaz tamamlandıktan sonra **Yayımla**' yı seçin.

   ![Web uygulamasını Azure 'da yayımlama](./media/app-service-web-get-started-dotnet/publish-web-app-vs2019.png)

   Visual Studio ASP.NET Core Web uygulamanızı Azure 'da yayımlar ve uygulamayı varsayılan tarayıcınızda başlatır. 

   ![Azure 'da çalışan yayımlanmış ASP.NET Web uygulaması](./media/app-service-web-get-started-dotnet/web-app-running-live.png)

**Tebrikler!** ASP.NET Core Web uygulamanız Azure App Service canlı olarak çalışıyor.

## <a name="update-the-app-and-redeploy"></a>Uygulamayı güncelleştirme ve yeniden dağıtma

Web uygulamanızı güncelleştirmek ve yeniden dağıtmak için aşağıdaki adımları izleyin:

1. **Çözüm Gezgini**' de, projeniz altında, **Sayfalar** > **Index. cshtml**dosyasını açın.

1. Tüm `<div>` etiketi aşağıdaki kodla değiştirin:

   ```HTML
   <div class="jumbotron">
       <h1>ASP.NET in Azure!</h1>
       <p class="lead">This is a simple app that we've built that demonstrates how to deploy a .NET app to Azure App Service.</p>
   </div>
   ```

1. Azure’a yeniden dağıtmak için **Çözüm Gezgini**’nde **myFirstAzureWebApp** projesine sağ tıklayıp **Yayımla**’yı seçin.

1. **Yayımla** Özeti sayfasında **Yayımla**' yı seçin.

   ![Web uygulamasında güncelleştirme yayımlama](./media/app-service-web-get-started-dotnet/publish-update-to-web-app-vs2019.png)

Yayımlama tamamlandığında Visual Studio, web uygulamasının URL’si ile bir tarayıcı başlatır.

![Azure 'da çalışan güncelleştirilmiş ASP.NET Web uygulaması](./media/app-service-web-get-started-dotnet/updated-web-app-running-live.png)

## <a name="manage-the-azure-app"></a>Azure uygulamasını yönetme

Web uygulamanızı yönetmek için [Azure Portal](https://portal.azure.com)gidin ve **uygulama hizmetleri**' ni arayıp seçin.

![Uygulama hizmetlerini seçin](./media/app-service-web-get-started-dotnet/app-services.png)

**Uygulama hizmetleri** sayfasında, Web uygulamanızın adını seçin.

![Azure uygulamasına portal gezintisi](./media/app-service-web-get-started-dotnet/select-app-service.png)

Web uygulamanıza ilişkin **genel bakış** sayfası, göz at, durdur, Başlat, yeniden Başlat ve Sil gibi temel yönetime yönelik seçenekler içerir. Sol menü, uygulamanızı yapılandırmak için daha fazla sayfa sağlar.

![Azure portal App Service](./media/app-service-web-get-started-dotnet/web-app-overview-page.png)

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure App Service bir ASP.NET Core Web uygulaması oluşturmak ve dağıtmak için Visual Studio 'Yu kullandınız.

Bir .NET Core uygulaması oluşturmayı ve bir SQL veritabanına bağlamayı öğrenmek için bir sonraki makaleye ilerleyin:

> [!div class="nextstepaction"]
> [SQL Veritabanı ile ASP.NET Core](app-service-web-tutorial-dotnetcore-sqldb.md)
