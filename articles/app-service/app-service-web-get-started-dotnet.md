---
title: 'Quickstart: C# ASP.NET Core uygulaması oluşturma'
description: Visual Studio'dan varsayılan C# ASP.NET Core web uygulaması şablonuna dağıtarak Azure Uygulama Hizmeti'nde web uygulamalarını nasıl çalıştırırabilirsiniz öğrenin.
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.topic: quickstart
ms.date: 03/17/2020
ms.custom: mvc, devcenter, vs-azure, seodec18
ms.openlocfilehash: be7c4a2fb65f913bc97617af1a0f21ee8fcca714
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81313291"
---
# <a name="quickstart-create-an-aspnet-core-web-app-in-azure"></a>Hızlı başlangıç: Azure'da ASP.NET Core web uygulaması oluşturma

Bu hızlı başlangıçta, ilk ASP.NET Core web uygulamanızı nasıl oluşturup Azure [Uygulama Hizmeti'ne](overview.md)dağıtacağınız öğreneceksiniz. 

İşi nizi bitirdiğinizde, bir Uygulama Hizmeti barındırma planı ve dağıtılmış bir web uygulamasına sahip bir Uygulama Hizmetinden oluşan bir Azure kaynak grubunuz olur.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/dotnet/)
- Bu hızlı başlangıç, bir uygulamayı Windows'daki Uygulama Hizmeti'ne dağıtır. _Linux_ üzerinde App Service'e dağıtım yapmak için bkz. [Linux üzerinde App Service'te .NET Core web uygulaması oluşturma](./containers/quickstart-dotnetcore.md).
- Visual <a href="https://www.visualstudio.com/downloads/" target="_blank">Studio 2019'u</a> **ASP.NET ve web geliştirme** iş yüküyle yükleyin.

  Visual Studio 2019'u zaten yüklediyseniz:

  - **Güncellemeler için** **Yardım** > Denetimi'ni seçerek Visual Studio'daki en son güncelleştirmeleri yükleyin.
  - **Araçları** > **Al Araçları ve Özellikleri**seçerek iş yükünü ekleyin.


## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Visual Studio'da aşağıdaki adımları izleyerek bir ASP.NET Core web uygulaması oluşturun:

1. Visual Studio'u açın ve **yeni bir proje oluştur'u**seçin.

1. **Yeni bir proje oluştur'da,** Çekirdek Web Uygulaması **ASP.NET'yi** seçin ve **C#'ın** bu seçimiçin dillerde listeli olduğunu onaylayın ve **ardından İleri'yi**seçin.

1. **Yeni projenizi yapılandırın,** web uygulama projenizi *benimFirstAzureWebApp olarak*adlandırın ve **Oluştur'u**seçin.

   ![Web uygulaması projenizi yapılandırın](./media/app-service-web-get-started-dotnet/configure-web-app-project.png)

1. Azure'a istediğiniz ASP.NET Web uygulamasını dağıtabilirsiniz, ancak bu hızlı başlangıç için **Web Uygulaması** şablonu'nu seçin. Kimlik **Doğrulama'nın** **Kimlik Doğrulama Yok**olarak ayarlandıklarına ve başka bir seçeneğin seçilmediğinden emin olun. Ardından **Oluştur'u**seçin.

   ![Yeni bir ASP.NET Core web uygulaması oluşturma](./media/app-service-web-get-started-dotnet/create-aspnet-core-web-app.png) 
   
1. Visual Studio menüsünden, web uygulamanızı yerel olarak çalıştırmak için Hata Ayıklama olmadan **Hata** > **Ayıklama Başlat'ı** seçin.

   ![Yerel olarak çalışan web uygulaması](./media/app-service-web-get-started-dotnet/web-app-running-locally.png)

## <a name="publish-your-web-app"></a>Web uygulamanızı yayımlayın

Web uygulamanızı yayınlamak için öncelikle uygulamanızı yayınlayabileceğiniz yeni bir Uygulama Hizmeti oluşturmanız ve yapılandırmanız gerekir. 

Uygulama Hizmetini ayarlamanın bir parçası olarak şunları oluşturursunuz:

- Hizmetiçin tüm Azure kaynaklarını içeren yeni bir [kaynak grubu.](https://docs.microsoft.com/azure/azure-resource-manager/management/overview#terminology)
- Uygulamanızı barındıran web sunucusu çiftliğinin konumunu, boyutunu ve özelliklerini belirten yeni bir [Barındırma Planı.](https://docs.microsoft.com/azure/app-service/overview-hosting-plans)

Uygulama Hizmetinizi oluşturmak ve web uygulamanızı yayınlamak için aşağıdaki adımları izleyin:

1. **Solution**Explorer'da, **myFirstAzureWebApp** projesine sağ tıklayın ve **Yayımla'yı**seçin. Visual Studio'dan Azure hesabınızda oturum açmadıysanız, **hesap ekle** veya oturum **aç'ı**seçin. Ayrıca ücretsiz bir Azure hesabı da oluşturabilirsiniz.

1. **Yayımlama hedef** iletişim kutusunda, **Uygulama Hizmeti'ni**seçin , **Yeni Oluştur'u**seçin ve ardından **Profil Oluştur'u**seçin.

   ![Yayımlama hedefi seçme](./media/app-service-web-get-started-dotnet/pick-publish-target-vs2019.png)

1. Uygulama **Hizmetinde: Yeni** iletişim kutusu oluşturun, varsayılan adı kabul ederek veya yeni bir ad girerek uygulamanız için genel olarak benzersiz bir **Ad** sağlayın. Geçerli karakterler `a-z`şunlardır: , `A-Z`, `0-9`, ve `-`. Bu **Ad,** web uygulamanızın URL öneki olarak `http://<app_name>.azurewebsites.net`kullanılır.

1. **Abonelik**için, listelenen aboneliği kabul edin veya açılan listeden yenisini seçin.

1. **Kaynak grubunda** **Yeni'yi**seçin. **Yeni kaynak grubu adında,** *myResourceGroup'u* girin ve **Tamam'ı**seçin. 

1. **Hosting Planı**için **Yeni'yi**seçin. 

1. Barındırma **Planında: Yeni** iletişim kutusu oluşturun, aşağıdaki tabloda belirtilen değerleri girin:

   | Ayar  | Önerilen Değer | Açıklama |
   | -------- | --------------- | ----------- |
   | **Barındırma Planı**  | *myFirstAzureWebAppPlan* | App Service planının adı. |
   | **Konum**      | *Batı Avrupa* | Web uygulamasının barındırıldığı veri merkezi. |
   | **Boyut**          | *Ücretsiz* | [Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), barındırma özelliklerini belirler. |
   
   ![Yeni Barındırma Planı Oluşturun](./media/app-service-web-get-started-dotnet/create-new-hosting-plan-vs2019.png)

1. **Uygulama Öngörülerini** *Yok*olarak ayarlayın.

1. Uygulama **Hizmetinde: Yeni** iletişim kutusu oluşturun, Azure kaynaklarını oluşturmaya başlamak için **Oluştur'u** seçin.

   ![Yeni uygulama hizmeti oluşturun](./media/app-service-web-get-started-dotnet/create-new-app-service-vs2019.png)

1. Sihirbaz tamamlandıktan sonra **Yayımla'yı**seçin.

   ![Web uygulamasını Azure'da yayımlama](./media/app-service-web-get-started-dotnet/publish-web-app-vs2019.png)

   Visual Studio, ASP.NET Core web uygulamanızı Azure'da yayınlar ve uygulamayı varsayılan tarayıcınızda başlatır. 

   ![Azure'da çalışan web uygulaması ASP.NET yayınlandı](./media/app-service-web-get-started-dotnet/web-app-running-live.png)

**Tebrikler!** ASP.NET Core web uygulamanız Azure Uygulama Hizmeti'nde canlı olarak yayınlanmaktadır.

## <a name="update-the-app-and-redeploy"></a>Uygulamayı güncelleştirme ve yeniden dağıtma

Web uygulamanızı güncellemek ve yeniden dağıtmak için aşağıdaki adımları izleyin:

1. **Çözüm Gezgini'nde**, projeniz **altında, Pages** > **Index.cshtml'i**açın.

1. İki `<div>` etiketini aşağıdaki kodla değiştirin:

   ```HTML
   <div class="jumbotron">
       <h1>ASP.NET in Azure!</h1>
       <p class="lead">This is a simple app that we've built that demonstrates how to deploy a .NET app to Azure App Service.</p>
   </div>
   ```

1. Azure’a yeniden dağıtmak için **Çözüm Gezgini**’nde **myFirstAzureWebApp** projesine sağ tıklayıp **Yayımla**’yı seçin.

1. **Yayımla** özeti sayfasında **Yayımla'yı**seçin.

   ![Web uygulamasına güncelleme yayınlama](./media/app-service-web-get-started-dotnet/publish-update-to-web-app-vs2019.png)

Yayımlama tamamlandığında Visual Studio, web uygulamasının URL’si ile bir tarayıcı başlatır.

![Azure'da çalışan ASP.NET web uygulaması güncellendi](./media/app-service-web-get-started-dotnet/updated-web-app-running-live.png)

## <a name="manage-the-azure-app"></a>Azure uygulamasını yönetme

Web uygulamanızı yönetmek için [Azure portalına](https://portal.azure.com)gidin ve **Uygulama Hizmetlerini**arayın ve seçin.

![Uygulama Hizmetlerini Seçin](./media/app-service-web-get-started-dotnet/app-services.png)

Uygulama **Hizmetleri** sayfasında web uygulamanızın adını seçin.

![Azure uygulamasına portal gezintisi](./media/app-service-web-get-started-dotnet/select-app-service.png)

Web uygulamanızın **Genel Bakış** sayfası, göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim seçenekleri içerir. Sol menü, uygulamanızı yapılandırmak için başka sayfalar sağlar.

![Azure portalında Uygulama Hizmeti](./media/app-service-web-get-started-dotnet/web-app-overview-page.png)

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Uygulama Hizmeti'ne bir ASP.NET Çekirdek web uygulaması oluşturmak ve dağıtmak için Visual Studio'yı kullandınız.

Bir .NET Core uygulamasının nasıl oluşturulup bir SQL Veritabanına bağlanılmayı öğrenmek için bir sonraki makaleye ilerleyin:

> [!div class="nextstepaction"]
> [SQL Veritabanı ile ASP.NET Core](app-service-web-tutorial-dotnetcore-sqldb.md)
