---
title: Azure Windows Communication Foundation (WCF) Röle hibrit şirket içi/bulut uygulaması (.NET) | Microsoft Dokümanlar
description: Azure Röle'yi kullanarak şirket içi bir WCF hizmetini buluttaki bir web uygulamasına nasıl maruz bırakarak nasıl ortaya çıkarıyarınızı öğrenin
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 9ed02f7c-ebfb-4f39-9c97-b7dc15bcb4c1
ms.service: service-bus-relay
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: spelluru
ms.openlocfilehash: b86d535e4cbc275b3ee777d7c70146f7711c502c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71212929"
---
# <a name="expose-an-on-premises-wcf-service-to-a-web-application-in-the-cloud-by-using-azure-relay"></a>Azure Röle'yi kullanarak şirket içi bir WCF hizmetini buluttaki bir web uygulamasına maruz bırakma

Bu makale, Microsoft Azure ve Visual Studio ile nasıl karma bulut uygulaması derleyeceğinizi gösterir. Bulutta birden çok Azure kaynağı kullanan bir uygulama oluşturursunuz. Bu öğretici şunları öğrenmenize yardımcı olur:

* Bir web çözümünde kullanılması amacıyla web hizmeti oluşturma veya var olan bir web hizmetini uyarlama.
* Azure uygulaması ile başka bir yerde barındırılan bir web hizmeti arasında veri paylaşmak için Azure Windows Communication Foundation (WCF) Röle hizmetini nasıl kullanabilirsiniz?

Bu öğreticide aşağıdaki görevleri yaparsınız:

> [!div class="checklist"]
>
> * Bu öğretici için ön koşulları yükleyin.
> * Senaryoyu gözden geçirin.
> * Ad alanı oluşturun.
> * Şirket içi bir sunucu oluşturun.
> * Bir ASP .NET uygulaması oluşturun.
> * Uygulamayı yerel olarak çalıştırın.
> * Web uygulamasını Azure'a dağıtın.
> * Uygulamayı Azure'da çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Azure aboneliği. Hesabınız yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
* [Visual Studio 2015 veya üzeri](https://www.visualstudio.com). Bu eğitimdeki örnekler Visual Studio 2019'u kullansın.
* .NET için Azure SDK. [SDK indirme sayfasından](https://azure.microsoft.com/downloads/)yükleyin.

## <a name="how-azure-relay-helps-with-hybrid-solutions"></a>Azure Relay geçişinin karma çözümlere yönelik yardımları

İş çözümleri genellikle özel kod ve varolan işlevselliğin bir birleşiminden oluşur. Özel kod, yeni ve benzersiz iş gereksinimlerini ele alıyor. Zaten mevcut olan çözümler ve sistemler varolan işlevselliği sağlar.

Çözüm mimarları, ölçek gereksinimlerini daha kolay bir şekilde karşılamak ve işlem maliyetlerini düşürmek için bulutu kullanmaya başlıyor. Bunu yaparken, çözümleri için yapı taşları olarak kullanmak istedikleri mevcut hizmet varlıklarının kurumsal güvenlik duvarının içinde ve bulut çözümü tarafından kolay ulaşılamayacak şekilde olduğunu fark ederler. Birçok dahili hizmet, kurumsal ağ kenarında kolayca açığa çıkarılabilen bir şekilde oluşturulmadım veya barındırılamayabilir.

[Azure Relay,](https://azure.microsoft.com/services/service-bus/) mevcut WCF web hizmetlerini alır ve bu hizmetleri kurumsal ağ altyapısında müdahaleci değişikliklergerektirmeden şirket çevresi dışındaki çözümler için güvenli bir şekilde erişilebilir hale getirir. Bu tür geçişi hizmetleri, var olan ortamlarında barındırılmaya devam eder ancak bu hizmetler gelen oturumları ve istekleri bulutta barındırılan geçiş hizmetine devreder. Ayrıca, Azure Geçişi [Paylaşılan Erişim İmzası (SAS)](../service-bus-messaging/service-bus-sas.md) kimlik doğrulaması kullanarak bu hizmetleri yetkilendirilmemiş erişime karşı korur.

## <a name="review-the-scenario"></a>Senaryoyu gözden geçirin

Bu eğitimde, ürün stok u sayfasında ürünlerin listesini görmenizi sağlayan bir ASP.NET web sitesi oluşturursunuz.

![Senaryo][0]

Öğretici, var olan şirket içi sistemde ürün bilgilerine sahip olduğunuzu varsayar ve bu sisteme erişmek için Azure Geçişini kullanır. Basit bir konsol uygulamasında çalışan bir web hizmeti bu durumu simüle eder. Bellekiçi bir ürün kümesi içerir. Bu konsol uygulamasını kendi bilgisayarınızda çalıştırabilir ve web rolünü Azure'a dağıtabilirsiniz. Bunu yaparak, Azure veri merkezinde çalışan web rolünün bilgisayarınıza nasıl çağrı yaptığını görürsünüz. Bilgisayarınız en az bir güvenlik duvarı ve ağ adresi çevirisi (NAT) katmanının arkasında olsa bile bu çağrı gerçekleşir.

## <a name="set-up-the-development-environment"></a>Geliştirme ortamını ayarlama

Azure uygulamalarını geliştirmeye başlamadan önce, araçları indirip geliştirme ortamınızı ayarlayın:

1. SDK [indirme sayfasından](https://azure.microsoft.com/downloads/) .NET için Azure SDK'sını yükleyin.
1. **.NET** sütununda, kullandığınız [Visual Studio](https://www.visualstudio.com) sürümünü seçin. Bu öğretici Visual Studio 2019 kullanır.
1. Yükleyicinin çalıştırılması veya kaydedilmesi istendiğinde **Çalıştır'ı**seçin.
1. Web **Platformu Yükleyici** iletişim kutusunda **Yükle'yi** seçin ve yüklemeye devam edin.

Yükleme tamamlandıktan sonra, uygulamayı geliştirmeye başlamak için gereken her şeye sahipsiniz. SDK, Visual Studio'da Azure uygulamalarını kolayca geliştirmenize olanak sağlayan araçları içerir.

## <a name="create-a-namespace"></a>Ad alanı oluşturma

İlk adım, bir ad alanı oluşturmak ve [paylaşılan erişim imzası (SAS)](../service-bus-messaging/service-bus-sas.md) anahtarı nı elde etmektir. Ad alanı, röle hizmeti aracılığıyla açığa çıkan her uygulama için bir uygulama sınırı sağlar. Bir hizmet ad alanı oluşturulduğunda sistem tarafından otomatik olarak bir SAS anahtarı oluşturulur. Hizmet ad alanı ve SAS anahtarının birleşimi, bir uygulamaya erişimi doğrulamak için Azure'un kimlik bilgilerini sağlar.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Şirket içi sunucu oluşturma

İlk olarak, simüle edilmiş bir şirket içi ürün katalog sistemi oluşturursunuz.  Bu proje bir Visual Studio konsol uygulamasıdır ve Service Bus kitaplıkları ile yapılandırma ayarlarını dahil etmek için [Azure Service Bus NuGet paketini](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) kullanır. <a name="create-the-project"></a>

1. Microsoft Visual Studio'u yönetici olarak başlatın. Bunu yapmak için Visual Studio program simgesine sağ tıklayın ve **yönetici olarak Çalıştır'ı**seçin.
1. Visual Studio'da **yeni bir proje oluştur'u**seçin.
1. **Yeni bir proje oluştur'da**C# için Konsol **Uygulaması'nı (.NET Framework)** seçin ve **İleri'yi**seçin.
1. Project *ProductsServer'ı* adlandırın ve **Oluştur'u**seçin.

   ![Yeni projenizi yapılandırın][11]

1. **Solution**Explorer'da, **ProductsServer** projesine sağ tıklayın, ardından **NuGet Paketlerini Yönet'i**seçin.
1. **Browse'ı**seçin, ardından **WindowsAzure.ServiceBus'u**arayın ve seçin. **Yükle'yi**seçin ve kullanım koşullarını kabul edin.

   ![NuGet paketini seçin][13]

   Gerekli istemci derlemelerine başvurulmaktadır.

1. Ürün sözleşmeniz için yeni bir sınıf ekleyin. **Solution Explorer'da,** **ProductsServer** projesine sağ tıklayın ve**Sınıf** **Ekle'yi** > seçin.
1. **Ad'** de , *ProductsContract.cs* adını girin ve **Ekle'yi**seçin.

Çözümünüzde aşağıdaki kod değişikliklerini yapın:

1. *ProductsContract.cs* sınıfında, ad alanı tanımını hizmet sözleşmesini tanımlayan şu kod ile değiştirin:

    ```csharp
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;

        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }

        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();

        }

        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

1. *Program.cs,* profil hizmetini ve ana bilgisayarı ekleyen ad alanı tanımını aşağıdaki kodla değiştirin.

    ```csharp
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;

        // Implement the IProducts interface.
        class ProductsService : IProducts
        {

            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };

            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }

        }

        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();

                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();

                sh.Close();
            }
        }
    }
    ```

1. **Solution**Explorer'da, Visual Studio düzenleyicisinde dosyayı açmak için **App.config'e** çift tıklayın. `<system.ServiceModel>` Öğenin alt kısmında, ama `<system.ServiceModel>`yine de içinde , aşağıdaki XML kodu ekleyin. Ad alanınızın `yourServiceNamespace` adı ile ve `yourKey` portaldan daha önce aldığınız SAS tuşu ile değiştirdiğinizden emin olun:

    ```xml
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```

    > [!NOTE]
    > Neden olduğu `transportClientEndpointBehavior` hata sadece bir uyarıdır ve bu örnek için bir engelleme sorunu değildir.

1. Hala *App.config*, `<appSettings>` öğe, bağlantı dize değeri daha önce portaldan elde edilen bağlantı dizesi ile değiştirin.

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

1. Uygulamayı oluşturmak ve şimdiye kadar çalışmanızın doğruluğunu doğrulamak için Ctrl+Shift+B'yi seçin veya **Yapı** > **Çözümü'nü** seçin.

## <a name="create-an-aspnet-application"></a>ASP.NET uygulaması oluşturma

Bu bölümde, ürün hizmetinizden alınan verileri görüntüleyen basit bir ASP.NET uygulaması oluşturursunuz.

### <a name="create-the-project"></a>Proje oluşturma

1. Visual Studio'nun yönetici olarak çalıştırdığından emin olun.
1. Visual Studio'da **yeni bir proje oluştur'u**seçin.
1. **Yeni bir proje oluştur'da**C# için web uygulaması **(.NET Framework) ASP.NET** ve **İleri'yi**seçin.
1. Proje *ProductsPortal* adını ve **Oluştur'u**seçin.
1. **Yeni bir ASP.NET Web Uygulaması Oluştur'da,** **MVC'yi** seçin ve **Kimlik Doğrulama**altında **Değiştir'i** seçin.

   ![ASP .NET Web Uygulamasını Seçin][16]

1. **Kimlik Doğrulamasını Değiştir'de**Kimlik Doğrulama **Yok'u** seçin ve **ardından Tamam'ı**seçin. Bu öğretici için, oturum açabilmek için kullanıcıya ihtiyaç duymayan bir uygulama dağıtMış oluyorsunuz.

    ![Kimlik doğrulamayı belirtin][18]

1. Yeni **bir ASP.NET Web Uygulaması Oluştur**'da, MVC uygulamasını oluşturmak için **Oluştur'u** seçin.
1. Azure kaynaklarını yeni bir web uygulaması için yapılandırın. [Web uygulamanızı yayımla'daki](../app-service/app-service-web-get-started-dotnet-framework.md#launch-the-publish-wizard)adımları izleyin. Daha sonra, bu öğretici dönün ve bir sonraki adıma devam edin.
1. **Çözüm Gezgini'nde,** **Modeller'i** sağ tıklatın ve ardından**Sınıf** **Ekle'yi** > seçin.
1. Sınıfın *adını Product.cs,* ardından **Ekle'yi**seçin.

    ![Ürün modeli oluştur][17]

### <a name="modify-the-web-application"></a>Web uygulamasını değiştirme

1. Visual *Studio'daki Product.cs* dosyasında, varolan ad alanı tanımını aşağıdaki kodla değiştirin:

   ```csharp
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
       public class Product
       {
           public string Id { get; set; }
           public string Name { get; set; }
           public string Quantity { get; set; }
       }
    }
    ```

1. **Solution Explorer'da,** **Denetleyicileri**genişletin, ardından Visual Studio'da dosyayı açmak için **HomeController.cs** çift tıklatın.
1. *HomeController.cs,* varolan ad alanı tanımını aşağıdaki kodla değiştirin:

    ```csharp
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;

        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

1. **Solution Explorer'da,****Paylaşılan** **Görünümleri** > genişletin, ardından Visual Studio düzenleyicisinde dosyayı açmak için **_Layout.cshtml'e** çift tıklayın.
1. `My ASP.NET Application` *Northwind Traders Ürünleri*tüm oluşumları değiştirin.
1. `Home`, ve `About` `Contact` bağlantıları kaldırın. Aşağıdaki örnekte vurgulanmış kodu silin.

    ![Oluşturulan liste öğelerini silme][41]

1. Solution Explorer'da, **Görünümler** > **Ana Sayfa'yı**genişletin, ardından Visual Studio düzenleyicisinde dosyayı açmak için **Index.cshtml'e** çift tıklayın. **Solution Explorer** Dosyanın tüm içeriğini aşağıdaki kodla değiştirin:

   ```html
   @model IEnumerable<ProductsWeb.Models.Product>

   @{
            ViewBag.Title = "Index";
   }

   <h2>Prod Inventory</h2>

   <table>
             <tr>
                 <th>
                     @Html.DisplayNameFor(model => model.Name)
                 </th>
                 <th></th>
                 <th>
                     @Html.DisplayNameFor(model => model.Quantity)
                 </th>
             </tr>

   @foreach (var item in Model) {
             <tr>
                 <td>
                     @Html.DisplayFor(modelItem => item.Name)
                 </td>
                 <td>
                     @Html.DisplayFor(modelItem => item.Quantity)
                 </td>
             </tr>
   }

   </table>
   ```

1. Şimdiye kadar çalışmanızın doğruluğunu doğrulamak için, projeyi oluşturmak için Ctrl+Shift+B'yi seçebilirsiniz.

### <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Çalışır durumda olduğunu doğrulamak için uygulamayı çalıştırın.

1. **ProductsPortal** projesinin, etkin olduğundan emin olun. **Solution Explorer'da** proje adını sağ tıklatın ve Başlangıç Projesi **Olarak Ayarla'yı**seçin.
1. Visual Studio'da F5'i seçin.

Uygulamanız, bir tarayıcıda çalışıyor olarak görüntülenmelidir.

![Web uygulaması][21]

## <a name="put-the-pieces-together"></a>Parçaları bir araya getirme

Sonraki adım, şirket içi ürünlerin sunucusu ile ASP.NET uygulamasını birleştirmektir.

1. Zaten açık değilse, Visual Studio'da, [ASP.NET uygulama](#create-an-aspnet-application) oluştur bölümünde oluşturduğunuz **ProductsPortal** projesini açın.
1. Şirket içi sunucu [oluşturma](#create-an-on-premises-server) bölümündeki adıma benzer şekilde, Proje başvurularına NuGet paketini ekleyin. **Solution**Explorer'da, **ProductsPortal** projesine sağ tıklayın, ardından **NuGet Paketlerini Yönet'i**seçin.
1. *WindowsAzure.ServiceBus* araması yapın ve **WindowsAzure.ServiceBus** öğesini seçin. Ardından yüklemeyi tamamlayın ve bu iletişim kutusunu kapatın.
1. **Solution Explorer'da,** **ProductsPortal** projesine sağ tıklayın ve ardından**Varolan Öğeekle'yi** **Add** > seçin.
1. **ProductsServer** konsol projesinden *ProductsContract.cs* dosyasına gidin. *ProductsContract.cs*vurgulayın. **Ekle'nin**yanındaki aşağı oku seçin, ardından **Bağlantı Olarak Ekle'yi**seçin.

   ![Bağlantı olarak ekle][24]

1. Şimdi Visual Studio düzenleyicisindeki *HomeController.cs* dosyasını açın ve ad alanı tanımını aşağıdaki kodla değiştirin. Hizmet ad `yourServiceNamespace` alanınızın adı ve `yourKey` SAS anahtarınızla değiştirdiğinizden emin olun. Bu kod, istemcinin arama sonucunu döndürerek şirket içi hizmeti aramasını sağlar.

   ```csharp
   namespace ProductsWeb.Controllers
   {
       using System.Linq;
       using System.ServiceModel;
       using System.Web.Mvc;
       using Microsoft.ServiceBus;
       using Models;
       using ProductsServer;

       public class HomeController : Controller
       {
           // Declare the channel factory.
           static ChannelFactory<IProductsChannel> channelFactory;

           static HomeController()
           {
               // Create shared access signature token credentials for authentication.
               channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                   "sb://yourServiceNamespace.servicebus.windows.net/products");
               channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                   TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                       "RootManageSharedAccessKey", "yourKey") });
           }

           public ActionResult Index()
           {
               using (IProductsChannel channel = channelFactory.CreateChannel())
               {
                   // Return a view of the products inventory.
                   return this.View(from prod in channel.GetProducts()
                                    select
                                        new Product { Id = prod.Id, Name = prod.Name,
                                            Quantity = prod.Quantity });
               }
           }
       }
   }
   ```

1. **Solution Explorer'da** **ProductsPortal** çözümüne sağ tıklayın. Projeyi değil, çözümü sağ tıklattığından emin olun. **Varolan Proje** **Ekle'yi** > seçin.
1. **ProductsServer** projesine gidip *ProductsServer.csproj* çözümüne çift tıklayarak ekleyin.
1. **ProductsServer,** **ProductsPortal'daki**verileri görüntülemek için çalışıyor olmalıdır. **Solution**Explorer'da, **ProductsPortal** çözümüne sağ tıklayın ve **Özellik Sayfalarını**görüntülemek için **Özellikler'i** seçin.
1. **Ortak Özellikler** > **Başlangıç Projesi'ni** seçin ve Birden Çok başlangıç **projesi**seçin. **ProductsServer** ve **ProductsPortal'ın** bu sırada göründüğünden ve her ikisi için de **Eylem'in** **Başlat**olduğundan emin olun.

      ![Birden çok başlangıç projesi][25]

1. Sol **taraftaki Ortak Özellikler** > **Proje Bağımlılıkları'nı** seçin.
1. **Projeler**için, **ProductsPortal**seçin. **ProductsServer** projesinin seçili olduğundan emin olun.

    ![Proje bağımlılıkları][26]

1. **Projeler**için **ProductsServer'ı**seçin. **ProductsPortal'ın** seçilmediğinden emin olun ve değişikliklerinizi kaydetmek için **Tamam'ı** seçin.

## <a name="run-the-project-locally"></a>Projeyi yerel olarak çalıştırma

Uygulamayı yerel olarak test etmek için Visual Studio'da F5'i seçin. Şirket içi sunucu, **ProductsServer,** önce başlamalı, sonra **ProductsPortal** uygulaması bir tarayıcı penceresinde başlamalıdır. Bu kez, ürün envanterinin ürün hizmeti şirket içi sistemden alınan verileri listelediğini görürsünüz.

![Web uygulaması][10]

**ProductsPortal** sayfasında **Yenile'yi** seçin. Sayfayı her yenilediğinizde, `GetProducts()` **ProductsServer'dan** çağrıldığınızda sunucu uygulamasının bir ileti görüntülediğinizi görürsünüz.

Sonraki bölüme geçmeden önce her iki uygulamayı da kapatın.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>ProductsPortal projesini bir Azure web uygulamasına dağıtma

Bir sonraki adım, Azure Web uygulaması **ProductsPortal** ön uçyeniden yayımlamaktır:

1. **Solution**Explorer'da, **ProductsPortal** projesine sağ tıklayın ve **Yayımla'yı**seçin. **Yayımla** sayfasında **Yayımla'yı**seçin.

   > [!NOTE]
   > Dağıtımdan sonra **ProductsPortal** web projesinin otomatik olarak başlatılması durumunda tarayıcıda bir hata iletisi görüntülenebilir. **ProductsServer** uygulaması henüz çalışmadığından bu durumun meydana gelmesi olasıdır.
   >

1. Dağıtılan web uygulamasının URL'sini kopyalayın. URL'ye daha sonra ihtiyacınız olacak. Bu URL'yi Visual Studio'daki **Azure Uygulama Hizmeti Etkinliği** penceresinden de alabilirsiniz:

   ![Dağıtılan uygulamanın URL'si][9]

1. Çalışan uygulamayı durdurmak için tarayıcı penceresini kapatın.

<a name="set-productsportal-as-web-app"></a>Uygulamayı bulutta çalıştırmadan önce, **ProductsPortal'ın** Visual Studio'nun içinden bir web uygulaması olarak başlatıldığından emin olmalısınız.

1. Visual Studio'da, **ProductsPortal** projesine sağ tıklayın ve **Özellikler'i**seçin.
1. **Web'i**seçin. **Start Action**altında, **URL'yi Başlat'ı**seçin. Bu örnekte, daha önce dağıtılan web uygulamanızın `https://productsportal20190906122808.azurewebsites.net/`URL'sini girin.

    ![URL'yi başlat][27]

1. **Dosya** > **Yı Kaydet'i**seçin.
1. **Yapı** > **Çözümünü**Seçin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı oluşturmak ve çalıştırmak için F5'i seçin. **ProductsServer** konsol uygulaması olan şirket içi sunucu önce başlamalıdır, ardından **ProductsPortal** uygulaması burada gösterildiği gibi bir tarayıcı penceresinden başlamalıdır:

   ![Web uygulamasını Azure'da çalıştırın][1]

Ürün envanteri, ürün hizmeti şirket içi sistemden alınan verileri listeler ve bu verileri web uygulamasında görüntüler. **ProductsPortal**'ın bir Azure web uygulaması olarak bulutta çalıştığından emin olmak için URL'yi kontrol edin.

   > [!IMPORTANT]
   > **ProductsServer** konsol uygulamasının çalışır ve **ProductsPortal** uygulamasına veri sunma işlemini gerçekleştirebilir durumda olması gerekir. Tarayıcı bir hata görüntülerse, **ProductsServer'ın** aşağıdaki iletiyi yüklemesi ve görüntülemesi için birkaç saniye daha bekleyin ve ardından tarayıcıyı yenileyin.
   >

**Tarayıcıda, ProductsPortal** sayfasını yenileyin. Sayfayı her yenilediğinizde, `GetProducts()` **ProductsServer'dan** çağrıldığınızda sunucu uygulamasının bir ileti görüntülediğinizi görürsünüz.

![Güncelleştirilmiş çıktı][38]

## <a name="next-steps"></a>Sonraki adımlar

Şu öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Bir şirket içi WCF hizmetini ağınızın dışındaki bir WCF istemcisinin kullanımına sunma](service-bus-relay-tutorial.md)

[0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
[1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/run-web-app.png
[NuGet]: https://nuget.org

[11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/configure-productsserver.png
[13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/install-nuget-service-bus-productsserver.png
[15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
[16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/choose-web-application-template.png
[17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/add-class-productsportal.png
[18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/change-authentication.png
[9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/web-publish-activity.png
[10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/run-web-app-locally.png

[21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/run-web-app-locally-no-content.png
[24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/add-existing-item-link.png
[25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
[26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
[27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/launch-app-as-web-app.png

[36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
[38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
[41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
[43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png
