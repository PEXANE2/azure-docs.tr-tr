---
title: "Öğretici: Azure Röle'yi kullanarak REST öğreticisi"
description: 'Öğretici: REST tabanlı bir arabirimi ortaya çıkaran bir Azure Hizmet Veri Mecmuası Röle ana bilgisayarı uygulaması oluşturun.'
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 1312b2db-94c4-4a48-b815-c5deb5b77a6a
ms.service: service-bus-relay
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/05/2019
ms.author: spelluru
ms.openlocfilehash: 229ed2b00582f2c73ce68c47406d68325abda736
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73718839"
---
# <a name="tutorial-azure-wcf-relay-rest-tutorial"></a>Öğretici: Azure WCF Röle REST öğretici

Bu öğretici, REST tabanlı bir arabirimi ortaya çıkaran bir Azure Relay ana bilgisayarı uygulamasının nasıl oluşturulabildiğini açıklar. REST, HTTP istekleri üzerinden Service Bus API'lerine erişmek için web tarayıcısı gibi bir web istemcisi sunar.

Öğretici, Azure Röle'de bir REST hizmeti oluşturmak için Windows Communication Foundation (WCF) REST programlama modelini kullanır. Daha fazla bilgi için [WCF REST Programlama Modeli](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) ve [Hizmetleri Tasarlama ve Uygulama bölümüne](/dotnet/framework/wcf/designing-and-implementing-services)bakın.

Bu öğreticide aşağıdaki görevleri yaparsınız:

> [!div class="checklist"]
>
> * Bu öğretici için ön koşulları yükleyin.
> * Bir Röle ad alanı oluşturun.
> * REST tabanlı bir WCF hizmet sözleşmesi tanımlayın.
> * REST tabanlı WCF sözleşmesini uygulayın.
> * REST tabanlı WCF hizmetini barındırın ve çalıştırın.
> * Çalıştırın ve hizmeti test edin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Azure aboneliği. Hesabınız yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
* [Visual Studio 2015 veya üzeri](https://www.visualstudio.com). Bu eğitimdeki örnekler Visual Studio 2019'u kullansın.
* .NET için Azure SDK. [SDK indirme sayfasından](https://azure.microsoft.com/downloads/)yükleyin.

## <a name="create-a-relay-namespace"></a>Röle ad alanı oluşturma

Azure'da geçiş özelliklerini kullanmaya başlamak için öncelikle bir hizmet ad alanı oluşturmanız gerekir. Ad alanı, uygulamanızda bulunan Azure kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sunar. [Buradaki yönergeleri](relay-create-namespace-portal.md) izleyerek bir Geçiş ad alanı oluşturun.

## <a name="define-a-rest-based-wcf-service-contract-to-use-with-azure-relay"></a>Azure Relay ile kullanmak üzere REST tabanlı bir WCF hizmet sözleşmesi tanımlayın

WCF REST tarzı bir hizmet oluşturduğunuzda, sözleşmeyi tanımlamanız gerekir. Sözleşmede ana bilgisayarın hangi işlemleri desteklediği belirtilir. Bir hizmet işlemi bir web hizmeti yöntemini benzer. C++, C#veya Visual Basic arabirimiyle bir sözleşme tanımlayın. Arabirimdeki her yöntem belirli bir hizmet işlemine karşılık gelir. [ServiceContractAttribute özniteliğini](/dotnet/api/system.servicemodel.servicecontractattribute) her arabirime uygulayın ve her işlem için [OperationContractAttribute özniteliği](/dotnet/api/system.servicemodel.operationcontractattribute) uygulayın. 

> [!TIP]
> [ServiceContractAttribute](/dotnet/api/system.servicemodel.servicecontractattribute) olan bir arabirimdeki bir yöntem [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute)yoksa, bu yöntem açıkta değildir. Bu görevler için kullanılan kod yordamı izleyen örnekte görünür.

WCF sözleşmesi ile REST tarzı sözleşme arasındaki temel fark, bir mülkün [OperationContractAttribute'e](/dotnet/api/system.servicemodel.operationcontractattribute)eklenmesidir : [WebGetAttribute](/dotnet/api/system.servicemodel.web.webgetattribute). Bu özellik sayesinde arabiriminizdeki bir yöntem ile arabirimin diğer tarafındaki bir yöntemi eşleyebilirsiniz. Bu örnek, [WebGetAttribute](/dotnet/api/system.servicemodel.web.webgetattribute) bir yöntemi `HTTP GET`. Bu yaklaşım, Service Bus'un arabirime gönderilen komutları doğru bir şekilde alıp yorumlamasını sağlar.

### <a name="to-create-a-contract-with-an-interface"></a>Arabirimli bir sözleşme oluşturmak için

1. Microsoft Visual Studio'u yönetici olarak başlatın. Bunu yapmak için Visual Studio program simgesine sağ tıklayın ve **yönetici olarak Çalıştır'ı**seçin.
1. Visual Studio'da **yeni bir proje oluştur'u**seçin.
1. **Yeni bir proje oluştur'da**C# için Konsol **Uygulaması'nı (.NET Framework)** seçin ve **İleri'yi**seçin.
1. Proje *ImageListener*adı . Varsayılan **Konum'u**kullanın ve sonra **Oluştur'u**seçin.

   Bir C# projesi için Visual Studio *bir Program.cs* dosyası oluşturur. Bu sınıf, bir konsol uygulaması projesinin doğru şekilde derlenmesi için gerekli olan boş `Main()` yöntemi içerir.

1. **Solution Explorer'da** **ImageListener** projesine sağ tıklayın ve **Ardından NuGet Paketlerini Yönet'i**seçin.
1. **Browse'ı**seçin, ardından **WindowsAzure.ServiceBus'u**arayın ve seçin. **Yükle'yi**seçin ve kullanım koşullarını kabul edin.

    Bu adım, Servis Veri Ve *System.ServiceModel.dll*referansları ekler. Bu paket, Servis Veri Hizmeti kitaplıklarına ve `System.ServiceModel`WCF'ye otomatik olarak referanslar ekler.

1. Projeye açıkça bir `System.ServiceModel.Web.dll` başvuru ekleyin. **Çözüm Gezgini'nde,** proje klasörü altındaki **Başvurular'ı** sağ tıklatın ve **Başvuru Ekle'yi**seçin.
1. **Başvuru Ekle'de** **Framework'u** seçin ve *Search'te System.ServiceModel.Web* girin. **Search** **System.ServiceModel.Web** öğesinin onay kutusunu işaretleyin ve **Tamam**'a tıklayın.

Ardından, projede aşağıdaki kod değişikliklerini yapın:

1. aşağıdaki `using` ifadeleri *Program.cs* dosyasının üst kısmında ekleyin.

    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    * [System.ServiceModel](/dotnet/api/system.servicemodel), WCF'nin temel özelliklerine programlama erişimi sağlayan ad alanıdır. WCF Relay, hizmet sözleşmelerini tanımlamak için WCF'nin birçok nesnesini ve özniteliklerini kullanır. Bu ad alanını geçiş uygulamalarınızın çoğunda kullanırsınız.
    * [System.ServiceModel.Channels,](/dotnet/api/system.servicemodel.channels) Azure Relay ve istemci web tarayıcısı ile iletişim kurduğunuz nesne olan kanalı tanımlamaya yardımcı olur.
    * [System.ServiceModel.Web,](/dotnet/api/system.servicemodel.web) web tabanlı uygulamalar oluşturmanızı sağlayan türleri içerir.

1. Ad alanını `ImageListener` `Microsoft.ServiceBus.Samples`' niçin yeniden adlandırın

    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Ad alanı bildiriminin açılış kıvırcık ayraç doğrudan `IImageContract` sonra, `ServiceContractAttribute` adlı yeni bir arabirim `https://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1`tanımlayın ve bir değeri ile arabirliğe öznitelik uygulayın. 

    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

    Kodunuzun kapsamında kullandığınız ad alanı ile ad alanı değeri farklılık gösterir. Ad alanı değeri, bu sözleşme için benzersiz bir tanımlayıcıdır ve sürüm bilgilerine sahip olmalıdır. Daha fazla bilgi için bkz. [Hizmet Sürümü Oluşturma](/dotnet/framework/wcf/service-versioning). Ad alanını açıkça belirlemek, varsayılan ad alanı değerinin sözleşme adına eklenmesini engeller.

1. `IImageContract` Arabirim içinde, sözleşmenin `IImageContract` arabirimde ortaya çıkardığı tek bir işlem için bir yöntem bildirin ve kamu Hizmeti Veri Servisi sözleşmesinin bir parçası olarak ortaya çıkarmak istediğiniz yönteme özniteliği uygulayın. `OperationContract`

    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

1. `WebGet` Özniteliğe, `OperationContract` değeri ekleyin.

    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

   Değerin `WebGet` eklenmesi, röle hizmetinin HTTP `GetImage`GET isteklerini yönlendirmesini `GetImage` ve `HTTP GETRESPONSE` iade değerlerini yanıta çevirmesini sağlar. Öğreticinin ilerleyen saatlerinde, bu yönteme erişmek ve görüntüyü tarayıcıda görüntülemek için bir web tarayıcısı kullanırsınız.

1. `IImageContract` tanımından hemen sonra, `IImageContract` ve `IClientChannel` arabirimlerinden devralma işlemini gerçekleştiren bir kanal bildirin.

    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

   Kanal, hizmet ve istemcilerin bilgileri birbirlerine göndermek için kullandıkları WCF nesnesidir. Daha sonra, ana bilgisayar uygulamasında kanalı oluşturursunuz. Azure Relay daha sonra tarayıcıdan gelen HTTP GET `GetImage` isteklerini uygulamanıza geçirmek için bu kanalı kullanır. Röle, `GetImage` döndürme değerini almak ve istemci `HTTP GETRESPONSE` tarayıcısına çevirmek için kanalı da kullanır.

1. Şimdiye kadar çalışmanızın doğruluğunu doğrulamak için **Yapı** > **Çözümünü** seçin.

### <a name="example-that-defines-a-wcf-relay-contract"></a>WCF Röle sözleşmesini tanımlayan örnek

Aşağıdaki kod, WCF Relay sözleşmesini tanımlayan temel bir arabirimi gösterir.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="implement-the-rest-based-wcf-service-contract"></a>REST tabanlı WCF hizmet sözleşmesini uygulayın

REST tarzı bir WCF Relay hizmeti oluşturmak için önce bir arabirim kullanarak sözleşmeoluşturun. Bir sonraki adım ise bu arabirimi uygulamaktır. Bu yordam, kullanıcı `ImageService` tanımlı `IImageContract` arabirimi uygulayan adlı bir sınıf oluşturmayı içerir. Sözleşmeyi uyguladıktan sonra, bir *App.config* dosyası kullanarak arabirimi yapılandırırsınız. Yapılandırma dosyası uygulama için gerekli bilgileri içerir. Bu bilgiler, hizmetin adını, sözleşmenin adını ve geçiş hizmetiyle iletişim kurmak için kullanılan iletişim kuralı türünü içerir. Bu görevler için kullanılan kod yordamı izleyen örnekte görünür.

Önceki adımlarda olduğu gibi, REST tarzı bir sözleşme ile WCF Relay sözleşmesi nin uygulanması arasında çok az fark vardır.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>REST stilinde Service Bus sözleşmesini uygulama

1. `IImageContract` arabiriminin tanımından hemen sonra `ImageService` adlı yeni bir sınıf oluşturun. `ImageService` sınıfı, `IImageContract` arabirimini uygular.

    ```csharp
    class ImageService : IImageContract
    {
    }
    ```

    Diğer arabirim uygulamalarına benzer şekilde, tanımı farklı bir dosyada uygulayabilirsiniz. Ancak bu öğreticide uygulama, arabirim tanımı ve `Main()` yöntemiyle aynı dosyada görünmelidir.

1. Sınıfın bir WCF sözleşmesinin `IImageService` uygulanması olduğunu belirtmek için [Class'a ServiceBehaviorAttribute özniteliği](/dotnet/api/system.servicemodel.servicebehaviorattribute) uygulayın.

    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Daha önce de belirtildiği gibi, bu ad alanı geleneksel bir ad alanı değildir. Sözleşmeyi tanımlayan WCF mimarisinin bir parçası. Daha fazla bilgi için [Veri Sözleşmesi Adları'na](/dotnet/framework/wcf/feature-details/data-contract-names/)bakın.

1. Projenize *bir .jpg* görüntüsü ekleyin. Bu dosya, hizmetin alıcı tarayıcıda görüntülenebilen bir resimdir.

   1. Projenize sağ tıklayın ve **Ekle'yi**seçin.
   1. Ardından **Varolan Öğe'yi**seçin.
   1. Uygun bir .jpg'ye göz atmak için **Varolan Öğe ekle'yi** kullanın ve ardından **Ekle'yi**seçin. Dosyayı eklerken, Dosya adının yanındaki açılır listeden **Tüm** **Dosyalar'ı**seçin.

   Bu öğreticinin geri kalanı görüntünün adının *image.jpg*olduğunu varsayar. Farklı bir dosyanız varsa, görüntüyü yeniden adlandırmanız veya telafi etmek için kodunuzu değiştirmeniz gerekir.

1. Çalışan hizmetin resim dosyasını bulabileceğinden emin olmak **için, Çözüm Gezgini'nde** resim dosyasına sağ tıklayın ve ardından **Özellikler'i**seçin. **Özellikler'de,** **Kopyayı Çıktı Dizini'ne** **yeniyse Kopyala'ya**ayarlayın.

1. *Projeye System.Drawing.dll* derlemesine bir başvuru eklemek için [arabirimli bir sözleşme oluşturmak](#to-create-a-contract-with-an-interface) için yordamı kullanın.

1. Aşağıdaki ilişkili `using` ifadeleri ekleyin:

    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

1. `ImageService` Sınıfta, bit eşlemi yükler ve istemci tarayıcıya göndermek için hazırlar aşağıdaki oluşturucu ekleyin:

    ```csharp
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";
   
        Image bitmap;
   
        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

1. Önceki koddan hemen sonra, `GetImage` görüntüyü `ImageService` içeren bir HTTP iletisini döndürmek için sınıfa aşağıdaki yöntemi ekleyin.

    ```csharp
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);
   
        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";
   
        return stream;
    }
    ```

    Bu uygulama, görüntüyü almak ve tarayıcıya akış için hazırlamak için kullanır. `MemoryStream` Akış konumunu sıfırdan başlatır, akış içeriğini *.jpg*olarak bildirir ve bilgileri aktarRz.

1. **Yapı** > **Çözümünü**Seçin.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>Service Bus üzerinde web hizmetini çalıştırmak için yapılandırma tanımlama

1. **Solution**Explorer'da, Visual Studio düzenleyicisinde dosyayı açmak için **App.config'e** çift tıklayın.

    *App.config* dosyası hizmet adını, bitiş noktasını ve bağlamayı içerir. Son nokta, Azure Röle'nin istemcilerin ve ana bilgisayarların birbirleriyle iletişim kurmaları için ortaya çıkardığı konumdur. Bağlama iletişim kurmak için kullanılan iletişim kuralı türüdür. Buradaki temel fark, yapılandırılan hizmet bitiş noktasının [Bir WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) bağlayıcısı anlamına gelmeleridir.

1. `<system.serviceModel>` XML öğesi, bir veya birden çok hizmeti tanımlayan WCF öğesidir. Burada, hizmet adını ve bitiş noktasını tanımlamak için kullanılır. `<system.serviceModel>` Öğenin alt kısmında, ama `<system.serviceModel>`yine de `<bindings>` içinde , aşağıdaki içeriğe sahip bir öğe ekleyin:

    ```xml
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Bu içerik, uygulamada kullanılan bağlamaları tanımlar. Birden çok bağlama tanımlayabilirsiniz, ancak bu öğretici için yalnızca birini tanımlıyorsunuz.

    Önceki kod bir WCF Röle [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) 'e `relayClientAuthenticationType` `None`ayarla ile ' yi tanımlar. Bu ayar, bu bağlamayı kullanan bir bitiş noktasının istemci kimlik bilgisi gerektirmediğini gösterir.

1. `<bindings>` öğesinden sonra `<services>` öğesini ekleyin. Bağlamalara benzer şekilde tek bir yapılandırma dosyasında birden çok hizmet tanımlayabilirsiniz. Ancak bu öğreticide yalnızca bir tane tanımlayacaksınız.

    ```xml
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Bu içerik, daha önce tanımlanan varsayılan `webHttpRelayBinding`ı kullanan bir hizmeti yapılandırır. Ayrıca, bir `sbTokenProvider`sonraki adımda tanımlanan varsayılan ı da kullanır.

1. Öğeden `<services>` sonra, `<behaviors>` Paylaşılan Erişim İmzası (SAS) anahtarıyla değiştirerek `SAS_KEY` aşağıdaki içeriğe sahip bir öğe oluşturun. [Azure portalından][Azure portal]Bir SAS anahtarı almak için [bkz.](service-bus-relay-tutorial.md#get-management-credentials)

    ```xml
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

1. Hala *App.config*, `<appSettings>` öğe, daha önce portaldan elde edilen bağlantı dizeilesi ile tüm bağlantı dize değeri değiştirin.

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```

1. Tüm çözümü oluşturmak için **Yapı** > **Çözümünü** seçin.

### <a name="example-that-implements-the-rest-based-wcf-service-contract"></a>REST tabanlı WCF hizmet sözleşmesini uygulayan örnek

Aşağıdaki kod, bağlamayı kullanarak Servis Veri Yolunda çalışan REST tabanlı bir `WebHttpRelayBinding` hizmetin sözleşmesini ve hizmet uygulamasını gösterir.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Aşağıdaki örnekte, hizmetle ilişkili *App.config* dosyası gösterilmektedir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app settings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY>"/>
    </appSettings>
</configuration>
```

## <a name="host-the-rest-based-wcf-service-to-use-azure-relay"></a>Azure Röle'yi kullanmak için REST tabanlı WCF hizmetini barındırın

Bu bölümde, WCF Relay içeren bir konsol uygulamasını kullanarak bir web hizmetinin nasıl çalıştırılabildiğini açıklanmaktadır. Bu bölümde yazılan kodun tam bir listesi yordamı izleyen örnekte görünür.

### <a name="to-create-a-base-address-for-the-service"></a>Hizmet için taban adresi oluşturma

1. İşlev `Main()` bildiriminde, projenizin ad alanını depolamak için bir değişken oluşturun. Daha önce `yourNamespace` oluşturduğunuz Röle ad alanının adıyla değiştirdiğinden emin olun.

    ```csharp
    string serviceNamespace = "yourNamespace";
    ```

    Service Bus, benzersiz bir URI oluşturmak için ad alanınızdaki adı kullanır.

1. Ad alanını temel alan hizmetin taban adresine yönelik `Uri` örneğini oluşturun.

    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Web hizmeti ana bilgisayarını oluşturma ve yapılandırma

Yine `Main()`de, bu bölümde daha önce oluşturulan URI adresini kullanarak web servis ana bilgisayarını oluşturun.
  
```csharp
WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
```

Hizmet ana bilgisayarı, ana bilgisayar uygulamasının örneğini oluşturan WCF nesnesidir. Bu örnek, oluşturmak istediğiniz ana bilgisayar türünü ve `ImageService`ana bilgisayar uygulamasını ortaya çıkarmak istediğiniz adresi geçer.

### <a name="to-run-the-web-service-host"></a>Web hizmeti ana bilgisayarını çalıştırma

1. Hala, `Main()`hizmeti açmak için aşağıdaki satırı ekleyin.

    ```csharp
    host.Open();
    ```

    Hizmet artık çalışır durumdadır.

1. Hizmetin çalıştığını ve nasıl durdurulacağını belirten bir ileti görüntüleyin.

    ```csharp
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Bu işlemi bitirdiğinizde, hizmet ana bilgisayarını kapatın.

    ```csharp
    host.Close();
    ```

### <a name="example-of-the-service-contract-and-implementation"></a>Hizmet sözleşmesi ve uygulama örneği

Aşağıdaki örnek, hizmet sözleşmesini ve bu öğreticinin önceki kısımlarında yer alan uygulamayı içerir ve hizmeti bir konsol uygulamasında barındırır. Aşağıdaki kodu *GörüntüListener.exe*adlı bir çalıştırılabilir içine derlemek.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

## <a name="run-and-test-the-service"></a>Hizmeti çalıştırın ve test edin

Çözümü derledikten sonra uygulamayı çalıştırmak için şunları yapın:

1. Hizmeti çalıştırmak için F5'i seçin veya yürütülebilir dosya *konumuimageListener\bin\Debug\ImageListener.exe'ye*göz atın. Bir sonraki adım için gerekli olduğundan uygulamayı çalışır durumda tutun.
1. Görüntüye bakmak için komut istemindeki adresi kopyalayıp bir tarayıcıya yapıştırın.
1. İşi nizi bitirdiğinde, uygulamayı kapatmak için komut istemi penceresinde Enter'ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Röle hizmetini kullanan bir uygulama oluştursanız, daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Azure WCF Relay kullanarak şirket içi WCF REST hizmetini harici istemciye açık bir şekilde](service-bus-relay-tutorial.md)

[Azure portal]: https://portal.azure.com
