---
title: 'Hızlı başlangıç: C# ASP.NET uygulaması oluşturma'
description: Visual Studio 'dan varsayılan C# ASP.NET Web uygulaması şablonunu dağıtarak Azure App Service Web uygulamaları çalıştırmayı öğrenin.
ms.assetid: 04a1becf-7756-4d4e-92d8-d9471c263d23
ms.topic: quickstart
ms.date: 08/06/2020
ms.custom: devx-track-csharp, mvc, devcenter, seodec18
ms.openlocfilehash: 5bf774f2b5cf4a6275dc04248aadfd9a94f5122d
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88212562"
---
# <a name="create-an-aspnet-framework-web-app-in-azure"></a>Azure’da ASP.NET Framework web uygulaması oluşturma

[Azure App Service](overview.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.

Bu hızlı başlangıçta, ilk ASP.NET Web uygulamanızın Azure App Service için nasıl dağıtılacağı gösterilmektedir. İşiniz bittiğinde bir App Service planına sahip olacaksınız. Ayrıca dağıtılmış bir Web uygulamasına sahip bir App Service uygulamanız olacaktır.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlayabilmeniz için, **ASP.net ve Web geliştirme** iş yüküyle <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> ' ü yüklemelisiniz.

Visual Studio 2019 zaten yüklüyse:

- **Help**  >  **Güncelleştirmeler için yardım denetimi**' ni seçerek en son güncelleştirmeleri Visual Studio 'ya yükler.
- **Araçlar**  >  **Al araçlar ve Özellikler '** i seçerek iş yükünü ekleyin.

## <a name="create-an-aspnet-web-app"></a>ASP.NET Web uygulaması oluşturma <a name="create-and-publish-the-web-app"></a>

Aşağıdaki adımları izleyerek bir ASP.NET Web uygulaması oluşturun:

1. Visual Studio 'Yu açın ve **Yeni proje oluştur**' u seçin.

2. **Yeni proje oluştur**' da, **ASP.NET Web uygulaması (.NET Framework)** bulun ve seçin ve ardından **İleri**' yi seçin.

3. **Yeni projenizi yapılandırın**bölümünde, uygulamayı _Myfirstazurewebapp_olarak adlandırın ve ardından **Oluştur**' u seçin.

   ![Web uygulaması projenizi yapılandırma](./media/quickstart-dotnet-framework/configure-web-app-project-framework.png)

4. Azure’a herhangi bir türde ASP.NET web uygulaması dağıtabilirsiniz. Bu hızlı başlangıç için **MVC** şablonunu seçin.

5. Kimlik doğrulamanın **kimlik doğrulaması yok**olarak ayarlandığından emin olun. **Oluştur**’u seçin.

   ![ASP.NET Web uygulaması oluşturma](./media/quickstart-dotnet-framework/select-mvc-template-vs2019.png)

6. **Debug**  >  Web uygulamasını yerel olarak çalıştırmak için Visual Studio menüsünden hata ayıklama**olmadan Başlat** ' ı seçin.

   ![Uygulamayı yerel olarak çalıştırma](./media/quickstart-dotnet-framework/local-web-app.png)

## <a name="publish-your-web-app"></a>Web uygulamanızı yayımlama <a name="launch-the-publish-wizard"></a>

1. **Çözüm Gezgini**, **Myfirstazurewebapp** projesine sağ tıklayın ve **Yayımla**' yı seçin.

1. **App Service** seçip **Profil oluştur**' u seçin.

   ![Projeye genel bakış sayfasından yayımlama](./media/quickstart-dotnet-framework/publish-app-framework-vs2019.png)

1. **App Service yeni oluştur**' da seçenekleriniz, Azure 'da oturum açmış olup olmadığınız ve bir Azure hesabına bağlı bir Visual Studio hesabınız olup olmadığına bağlıdır. Azure aboneliğinizde oturum açmak için **Hesap Ekle** veya **oturum aç** seçeneklerinden birini belirleyin. Zaten oturum açtıysanız istediğiniz hesabı seçin.

   > [!NOTE]
   > Zaten oturum açtıysanız **Oluştur** öğesini henüz seçmeyin.
   >
   >

   ![Azure’da oturum açma](./media/quickstart-dotnet-framework/sign-in-azure-framework-vs2019.png)

   [!INCLUDE [resource group intro text](../../includes/resource-group.md)]

1. **Kaynak grubu**için **Yeni**' yi seçin.

1. **Yeni kaynak grubu adı**alanına *Myresourcegroup* yazın ve **Tamam**' ı seçin.

   [!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

1. **Barındırma planına**yönelik için **Yeni**' yi seçin.

1. **Barındırma planını Yapılandır** iletişim kutusunda, aşağıdaki tablodaki değerleri girin ve ardından **Tamam**' ı seçin.

   | Ayar | Önerilen Değer | Açıklama |
   |-|-|-|
   | Barındırma Planı| myAppServicePlan | App Service planının adı. |
   | Konum | West Europe | Web uygulamasının barındırıldığı veri merkezi. |
   | Boyut | Ücretsiz | [Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), barındırma özelliklerini belirler. |

   ![App Service planı oluşturma](./media/quickstart-dotnet-framework/app-service-plan-framework-vs2019.png)

1. **Ad**alanına,,, ve yalnızca geçerli karakterleri içeren benzersiz bir uygulama adı girin `a-z` `A-Z` `0-9` `-` . Otomatik olarak oluşturulan benzersiz adı kabul edebilirsiniz. Web uygulamasının URL'si `http://<app-name>.azurewebsites.net` şeklindedir; burada `<app-name>`, uygulamanızın adıdır.

2. Azure kaynaklarını oluşturmaya başlamak için **Oluştur**’u seçin.

   ![Uygulama adını yapılandırma](./media/quickstart-dotnet-framework/web-app-name-framework-vs2019.png)

    Sihirbaz tamamlandıktan sonra Azure kaynakları sizin için oluşturulur ve yayımlamaya hazırlarsınız.

3. **Yayımla** sayfasında **Yayımla**' ya tıklayın. Visual Studio uygulamayı oluşturur, paketler ve Azure 'da yayımlar ve ardından uygulamayı varsayılan tarayıcıda başlatır.

    ![Azure’da yayımlanmış ASP.NET web uygulaması](./media/quickstart-dotnet-framework/published-azure-web-app.png)

**App Service yeni oluştur** sayfasında belirtilen uygulama adı, biçimdeki URL ön eki olarak kullanılır `http://<app-name>.azurewebsites.net` .

**Tebrikler!** ASP.NET Web uygulamanız Azure App Service ' de canlı olarak çalışıyor.

## <a name="update-the-app-and-redeploy"></a>Uygulamayı güncelleştirme ve yeniden dağıtma

1. **Çözüm Gezgini**, projenizin altında, **views**  >  **Home**  >  **Index. cshtml**dosyasını açın.

1. Üst kısımda `<div class="jumbotron">` HTML etiketini bulun ve tüm öğeyi aşağıdaki kodla değiştirin:

   ```html
   <div class="jumbotron">
       <h1>ASP.NET in Azure!</h1>
       <p class="lead">This is a simple app that we've built that demonstrates how to deploy a .NET app to Azure App Service.</p>
   </div>
   ```

1. Azure’a yeniden dağıtmak için **Çözüm Gezgini**’nde **myFirstAzureWebApp** projesine sağ tıklayıp **Yayımla**’yı seçin. Ardından **Yayımla**' yı seçin.

    Yayımlama tamamlandığında Visual Studio, web uygulamasının URL’si ile bir tarayıcı başlatır.

    ![Azure’da güncelleştirilmiş ASP.NET web uygulaması](./media/quickstart-dotnet-framework/updated-azure-web-app.png)

## <a name="manage-the-azure-app"></a>Azure uygulamasını yönetme

1. Web uygulamasını yönetmek için [Azure Portal](https://portal.azure.com)gidin ve **uygulama hizmetleri**' ni arayıp seçin.

   ![Uygulama hizmetlerini seçin](./media/quickstart-dotnet-framework/app-services.png)

2. **Uygulama hizmetleri** sayfasında, Web uygulamanızın adını seçin.

   ![Azure uygulamasına portal gezintisi](./media/quickstart-dotnet-framework/access-portal-framework-vs2019.png)

   Web uygulamanızın Genel Bakış sayfasını görürsünüz. Burada, tarama, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetimi yapabilirsiniz.

   ![Azure portal App Service Genel Bakış](./media/quickstart-dotnet-framework/web-app-general-framework-vs2019.png)

   Soldaki menü, uygulamanızı yapılandırmak için farklı sayfalar sağlar.

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SQL Veritabanı ile ASP.NET](app-service-web-tutorial-dotnet-sqldatabase.md)

> [!div class="nextstepaction"]
> [ASP.NET uygulamasını yapılandırma](configure-language-dotnet-framework.md)
