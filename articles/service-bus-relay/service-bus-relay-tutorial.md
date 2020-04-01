---
title: Bir ön-prem WCF REST hizmetini Azure Röle'yi kullanan istemcilere açıkla
description: Bu öğretici, Azure WCF Relay kullanarak şirket içi WCF REST hizmetini harici bir istemciye nasıl gösterinizin açıklanmaktadır.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 53dfd236-97f1-4778-b376-be91aa14b842
ms.service: service-bus-relay
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: 551c8e662669737d9d074a69cb03d6060ab87ad5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76513091"
---
# <a name="tutorial-expose-an-on-premises-wcf-rest-service-to-external-client-by-using-azure-wcf-relay"></a>Öğretici: Azure WCF Rölesini kullanarak şirket içi WCF REST hizmetini harici istemciye açıkla

Bu öğretici, Azure Relay kullanarak wcf Relay istemci uygulaması ve hizmetinin nasıl oluşturulabildiğini açıklar. Servis Veri Servisi [mesajlaşması](../service-bus-messaging/service-bus-messaging-overview.md)kullanan benzer bir öğretici için [bkz.](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)

Bu öğretici aracılığıyla çalışmak, bir WCF Relay istemcisi ve hizmet uygulaması oluşturma adımlarını anlamanızı sağlar. Orijinal WCF karşılıkları gibi, bir hizmet de bir veya daha fazla uç noktayı ortaya çıkaran bir yapıdır. Her bitiş noktası bir veya daha fazla hizmet işlemi ortaya çıkarır. Bir hizmetin uç noktası hizmetin bulunabileceği bir adres, istemcinin hizmetle iletişiminde paylaşması gereken bilgileri içeren bir bağlama ve hizmet tarafından istemcilerine sağlanan işlevselliği tanımlayan bir sözleşme belirtir. WCF ve WCF Relay arasındaki temel fark, bitiş noktasının bilgisayarınızda yerel olarak değil, bulutta açıkta kalmasıdır.

Bu öğreticideki bölümlerin dizisini inceledikten sonra, çalışan bir hizmetiniz olur. Ayrıca, hizmetin işlemlerini çağırabilecek bir istemciniz de olur. 

Bu öğreticide aşağıdaki görevleri yaparsınız:

> [!div class="checklist"]
>
> * Bu öğretici için ön koşulları yükleyin.
> * Bir Röle ad alanı oluşturun.
> * Bir WCF hizmet sözleşmesi oluşturun.
> * WCF sözleşmesini uygulayın.
> * Röle hizmetine kaydolmak için WCF hizmetini barındırın ve çalıştırın.
> * Hizmet sözleşmesi için bir WCF istemcisi oluşturun.
> * WCF istemcisini yapılandırın.
> * WCF istemcisini uygulayın.
> * Uygulamaları çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Azure aboneliği. Hesabınız yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
* [Visual Studio 2015 veya üzeri](https://www.visualstudio.com). Bu eğitimdeki örnekler Visual Studio 2019'u kullansın.
* .NET için Azure SDK. [SDK indirme sayfasından](https://azure.microsoft.com/downloads/)yükleyin.

## <a name="create-a-relay-namespace"></a>Röle ad alanı oluşturma

İlk adım, bir ad alanı oluşturmak ve [paylaşılan erişim imzası (SAS)](../service-bus-messaging/service-bus-sas.md) anahtarı nı elde etmektir. Ad alanı, röle hizmeti aracılığıyla açığa çıkan her uygulama için bir uygulama sınırı sağlar. Bir hizmet ad alanı oluşturulduğunda sistem tarafından otomatik olarak bir SAS anahtarı oluşturulur. Hizmet ad alanı ve SAS anahtarının birleşimi, bir uygulamaya erişimi doğrulamak için Azure'un kimlik bilgilerini sağlar.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract"></a>WCF hizmet sözleşmesi tanımlama

Hizmet sözleşmesi, hizmetin hangi işlemleri desteklediğini belirtir. İşlemler web hizmeti yöntemleri veya işlevleridir. Sözleşmeler; C++, C# veya Visual Basic arabirimi tanımlamasıyla oluşturulur. Arabirimdeki her yöntem belirli bir hizmet işlemine karşılık gelir. Her arabirimde [ServiceContractAttribute](/dotnet/api/system.servicemodel.servicecontractattribute) özniteliğinin ve her işlemde de [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute) özniteliğinin uygulanmış olması gerekir. [ServiceContractAttribute](/dotnet/api/system.servicemodel.servicecontractattribute) özniteliği olan bir arabirimdeki bir [yöntem, OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute) özniteliğine sahip değilse, bu yöntem açıkta değildir. Bu görevlere ilişkin kod, aşağıdaki yordamın altındaki örnekte sağlanır. Sözleşmeler ve hizmetler le ilgili daha geniş bir tartışma için [bkz.](/dotnet/framework/wcf/designing-and-implementing-services)

### <a name="create-a-relay-contract-with-an-interface"></a>Arabirimli bir röle sözleşmesi oluşturma

1. Microsoft Visual Studio'u yönetici olarak başlatın. Bunu yapmak için Visual Studio program simgesine sağ tıklayın ve **yönetici olarak Çalıştır'ı**seçin.
1. Visual Studio'da **yeni bir proje oluştur'u**seçin.
1. **Yeni bir proje oluştur'da**C# için Konsol **Uygulaması'nı (.NET Framework)** seçin ve **İleri'yi**seçin.
1. Projeye *EchoService* adını ve **Oluştur'u**seçin.

   ![Konsol uygulaması oluşturma][2]

1. **Çözüm Gezgini'nde**projeyi sağ tıklatın ve **NuGet Paketlerini Yönet'i**seçin. **NuGet Paket Yöneticisi'nde** **Gözat'ı**seçin, ardından **WindowsAzure.ServiceBus'u**arayın ve seçin. **Yükle'yi**seçin ve kullanım koşullarını kabul edin.

    ![Servis Otobüs paketi][3]

   Bu paket, Servis Veri Hizmeti kitaplıklarına ve `System.ServiceModel`WCF'ye otomatik olarak referanslar ekler. [System.ServiceModel](/dotnet/api/system.servicemodel), WCF'nin temel özelliklerine programlamayla erişmenizi sağlayan ad alanıdır. Service Bus, hizmet sözleşmelerini tanımlamak için WCF'nin birçok nesnesini ve özniteliklerini kullanır.

1. Program.cs üst `using` kısmında aşağıdaki *Program.cs*ifadeleri ekleyin:

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. `EchoService` olan varsayılan ad alanı adını `Microsoft.ServiceBus.Samples` olarak değiştirin.

   > [!IMPORTANT]
   > Bu öğretici, `Microsoft.ServiceBus.Samples` [WCF istemci sisini Yapılandırma](#configure-the-wcf-client) bölümünde yapılandırma dosyasında kullanılan sözleşme tabanlı yönetilen türün ad alanı olan C# ad alanını kullanır. Bu örneği oluştururken istediğiniz ad alanını belirtebilirsiniz. Ancak, uygulama yapılandırma dosyasında sözleşme ve hizmetin ad alanlarını buna göre değiştirmediğiniz sürece öğretici çalışmaz. *App.config* dosyasında belirtilen ad alanı, C# dosyalarınızda belirtilen ad alanıyla aynı olmalıdır.
   >

1. Ad alanı `Microsoft.ServiceBus.Samples` bildiriminden hemen sonra, ancak ad alanı `IEchoContract` içinde, `ServiceContractAttribute` adlandırılmış yeni bir arabirim tanımlayın ve öznitelik bir ad alanı değeri ile arabirime özniteliği uygulayın. `https://samples.microsoft.com/ServiceModel/Relay/` Ad alanı bildiriminden sonra aşağıdaki kodu yapıştırın:

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    Kodunuzun kapsamında kullandığınız ad alanı ile ad alanı değeri farklılık gösterir. Ad alanı değeri bu sözleşme için benzersiz bir tanımlayıcı olarak kullanılır. Ad alanını açıkça belirlemek, varsayılan ad alanı değerinin sözleşme adına eklenmesini engeller.

   > [!NOTE]
   > Genellikle, hizmet sözleşmesi ad alanı sürüm bilgilerini barındıran bir adlandırma şeması içerir. Sürüm bilgilerini hizmet sözleşmesi ad alanına dahil etmek, hizmetlerin yeni bir ad alanı içeren yeni bir hizmet sözleşmesi tanımlayarak ve bu sözleşmeyi yeni bir uç noktada kullanıma sunarak büyük değişiklikleri yalıtmalarına olanak sağlar. Bu şekilde, istemciler güncelleştirilmeye gerek kalmadan eski hizmet sözleşmesini kullanmaya devam edebilirler. Sürüm bilgileri, bir tarihten veya bir derleme numarasından oluşabilir. Daha fazla bilgi için bkz. [Hizmet Sürümü Oluşturma](/dotnet/framework/wcf/service-versioning). Bu öğretici için, hizmet sözleşmesi ad alanının adlandırma düzeni sürüm bilgilerini içermez.
   >

1. `IEchoContract` Arabirim içinde, sözleşmenin `IEchoContract` arabirimde ortaya çıkardığı tek bir işlem için bir yöntem bildirin ve özniteliği genel WCF Röle sözleşmesinin bir parçası olarak ortaya çıkarmak istediğiniz yönteme aşağıdaki gibi uygulayın: `OperationContractAttribute`

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```

1. `IEchoContract` arabirimi tanımından hemen sonra, aşağıda belirtildiği şekilde `IEchoContract` ve `IClientChannel` arabirimlerinden devralma işlemini gerçekleştiren bir kanal bildirin:

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Kanal, ana bilgisayar ve istemcinin bilgileri birbirlerine göndermek için kullandıkları WCF nesnesidir. Daha sonra, iki uygulama arasındaki bilgileri yankılamak için kanala karşı kod yazarsınız.

1. **Yapı Çözümü'nü** > **Build Solution** seçin veya şimdiye kadar çalışmanızın doğruluğunu doğrulamak için Ctrl+Shift+B'yi seçin.

### <a name="example-of-a-wcf-contract"></a>WCF sözleşmesi örneği

Aşağıdaki kod, WCF Relay sözleşmesini tanımlayan temel bir arabirimi gösterir.

```csharp
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Oluşturulması tamamlandığına göre arabirimi uygulayabilirsiniz.

## <a name="implement-the-wcf-contract"></a>WCF sözleşmesini uygulayın

Azure rölesi oluşturmak, önce bir arabirim kullanarak sözleşmeyi oluşturmanızı gerektirir. Arabirimi oluşturma hakkında daha fazla bilgi için önceki bölüme bakın. Sonraki yordam arabirimi uygular. Bu görev, kullanıcı `EchoService` tanımlı `IEchoContract` arabirimi uygulayan adlandırılmış bir sınıf oluşturmayı içerir. Arabirimi uyguladıktan sonra, bir *App.config* yapılandırma dosyası kullanarak arabirimi yapılandırırsınız. Yapılandırma dosyası uygulama için gerekli bilgileri içerir. Bu bilgiler, hizmetin adını, sözleşmenin adını ve geçiş hizmetiyle iletişim kurmak için kullanılan iletişim kuralı türünü içerir. Bu görevler için kullanılan kod yordamı izleyen örnekte sağlanır. Hizmet sözleşmesinin nasıl uygulanacağı hakkında daha genel bir tartışma için [bkz.](/dotnet/framework/wcf/implementing-service-contracts)

1. `IEchoContract` arabiriminin tanımından hemen sonra `EchoService` adlı yeni bir sınıf oluşturun. `EchoService` sınıfı, `IEchoContract` arabirimini uygular.

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    Diğer arabirim uygulamalarına benzer şekilde, tanımı farklı bir dosyada uygulayabilirsiniz. Ancak bu öğreticide uygulama, arabirim tanımı ve `Main()` yöntemiyle aynı dosyadadır.

1. `IEchoContract` arabirimine [ServiceBehaviorAttribute](/dotnet/api/system.servicemodel.servicebehaviorattribute) özniteliğini uygulayın. Öznitelik, hizmet adını ve ad alanını belirtir. Bunu yaptıktan sonra `EchoService` sınıfı şu şekilde görünür:

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. `EchoService` sınıfındaki `IEchoContract` arabiriminde tanımlanan `Echo` yöntemini uygulayın.

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. **Yapı** > **Çözümü'nü** seçin veya Ctrl+Shift+B'yi seçin.

### <a name="define-the-configuration-for-the-service-host"></a>Servis ana bilgisayarı için yapılandırmayı tanımlama

Yapılandırma dosyası WCF yapılandırma dosyasına benzer. Hizmet adını, bitiş noktasını ve bağlamayı içerir. Son nokta, Azure Röle'nin istemcilerin ve ana bilgisayarların birbirleriyle iletişim kurmaları için ortaya çıkardığı konumdur. Bağlama iletişim kurmak için kullanılan iletişim kuralı türüdür. Temel fark, bu yapılandırılmış hizmet bitiş noktasının .NET Framework'ün bir parçası olmayan [bir NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) bağlamasına atıfta bulunulmasıdır. [NetTcpRelayBinding,](/dotnet/api/microsoft.servicebus.nettcprelaybinding) hizmet tarafından tanımlanan bağlamalardan biridir.

1. **Solution**Explorer'da, Visual Studio düzenleyicisinde dosyayı açmak için **App.config'e** çift tıklayın.
1. `<appSettings>` öğesinde, yer tutucuları hizmet ad alanınızdaki adla ve önceki adımların birinde kopyaladığınız SAS anahtarı ile değiştirin.
1. `<system.serviceModel>` etiketleri içinde, bir `<services>` öğesi ekleyin. Tek bir yapılandırma dosyasında birden çok röle uygulaması tanımlayabilirsiniz. Ancak bu öğreticide yalnızca bir adet tanımlanır.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. `<services>` öğesi içinde, hizmetin adını tanımlamak için `<service>` öğesi ekleyin.

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. `<service>` öğesi içinde, uç nokta sözleşmesinin konumunu ve uç noktaya yönelik bağlama türünü tanımlayın.

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Uç nokta, istemcinin ana bilgisayar uygulamasını nerede arayacağını tanımlar. Daha sonra, öğretici, Azure Röle aracılığıyla ana bilgisayarı tam olarak ortaya çıkaran bir URI oluşturmak için bu adımı kullanır. Bağlama, röle hizmetiyle iletişim kurmak için protokol olarak TCP'yi kullandığımızı bildirir.

1. **Yapı Çözümü'nü** > **Build Solution** seçin veya şimdiye kadar çalışmanızın doğruluğunu doğrulamak için Ctrl+Shift+B'yi seçin.

### <a name="example-of-implementation-of-a-service-contract"></a>Hizmet sözleşmesinin uygulanması örneği

Aşağıdaki kod, hizmet sözleşmesinin uygulamasını gösterir.

```csharp
[ServiceBehavior(Name = "EchoService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Aşağıdaki kod, hizmet ana bilgisayarıyla ilişkili *App.config* dosyasının temel biçimini gösterir.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-the-wcf-service-to-register-with-the-relay-service"></a>Röle hizmetine kaydolmak için WCF hizmetini barındırın ve çalıştırın

Bu adım, bir Azure Röle hizmetinin nasıl çalıştırılabildiğini açıklar.

### <a name="create-the-relay-credentials"></a>Röle kimlik bilgilerini oluşturma

1. `Main()` içinde, konsol penceresinden okunan ad alanını ve SAS anahtarını depolayabileceğiniz iki değişken oluşturun.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    SAS anahtarı daha sonra projenize erişmek için kullanılacaktır. Ad alanı, Hizmet URI'si oluşturmak için `CreateServiceUri` öğesine bir parametre olarak geçirilir.

1. [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) nesnesini kullanarak, kimlik bilgisi türü olarak bir SAS anahtarı kullanacağınızı bildirin. Aşağıdaki kodu son adımda eklenen koddan hemen sonra ekleyin.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-the-service"></a>Hizmet için temel adres oluşturma

Önceki bölüme eklediğiniz koddan sonra, hizmetin temel adresi için bir `Uri` örnek oluşturun. Bu URI; Service Bus şemasını, ad alanını ve hizmet arabiriminin yolunu belirtir.

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

"sb" değeri, Servis Veri Mes'ü şemasının kısaltmasıdır. Protokol olarak TCP'yi kullandığımızı gösterir. Bu şema, [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) bağlama olarak belirtildiğinde yapılandırma dosyasında da daha önce belirtilmiştir.

Bu öğretici için URI şudur: `sb://putServiceNamespaceHere.windows.net/EchoService`

### <a name="create-and-configure-the-service-host"></a>Hizmet ana bilgisayarını oluşturma ve yapılandırma

1. Hala `Main()`çalışıyor, bağlantı modunu `AutoDetect`.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Bağlantı modu, hizmetin röle hizmetiyle iletişim kurmak için kullandığı protokolü açıklar; http veya TCP. Varsayılan ayarı `AutoDetect`kullanarak, hizmet kullanılabilirse TCP üzerinden Azure Relay'e ve TCP kullanılamıyorsa HTTP'ye bağlanmaya çalışır. Bu sonuç, hizmetin istemci iletişimi için belirlediği protokolden farklıdır. Bu protokol, kullanılan bağlamaya göre belirlenir. Örneğin, bir hizmet, bitiş noktasının HTTP üzerinden istemcilerle iletişim kurduğunu belirten [Temel Eki Bağlama](/dotnet/api/microsoft.servicebus.basichttprelaybinding) bağlamayı kullanabilir. Aynı hizmet, `ConnectivityMode.AutoDetect` hizmetin TCP üzerinden Azure Relay ile iletişim kurabilmesi için belirtilebilir.

1. Bu bölümün önceki kısımlarında oluşturduğunuz URI'yi kullanarak hizmet ana bilgisayarını oluşturun.

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Hizmet ana bilgisayarı, hizmetin örneğini oluşturan WCF nesnesidir. Burada, oluşturmak istediğiniz hizmet türünü, bir `EchoService` türünü ve ayrıca hizmeti ortaya çıkarmak istediğiniz adrese geçirirsiniz.

1. *Program.cs* dosyanın üst kısmında, [System.ServiceModel.Description](/dotnet/api/system.servicemodel.description) ve [Microsoft.ServiceBus.Description referansları](/dotnet/api/microsoft.servicebus.description)ekleyin.

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. `Main()` öğesine geri dönüp, genel erişimi etkinleştirmek için uç noktayı yapılandırın.

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Bu adım, projeniziçin Atom beslemesini inceleyerek uygulamanızın herkese açık olarak bulunabileceğini röle hizmetine bildirir. `private`Ayarlarsanız, `DiscoveryType` bir istemci hizmete erişmeye devam edebilir. Ancak, `Relay` ad alanı arandığında hizmet görünmez. Bunun yerine, istemcinin uç nokta yolunu önceden bilmesi gerekir.

1. Hizmet kimlik bilgilerini *App.config* dosyasında tanımlanan hizmet bitiş noktalarına uygulayın:

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Daha önce belirtildiği gibi, yapılandırma dosyasında birden çok hizmet ve uç nokta bildirmiş olabilirsiniz. Birden çok hizmet ve uç nokta bildirirseniz bu kod yapılandırma dosyasına çapraz geçiş yapar ve kimlik bilgilerinin uygulanacağı tüm uç noktalara yönelik arama yapar. Bu öğretici için yapılandırma dosyasının yalnızca bir bitiş noktası vardır.

### <a name="open-the-service-host"></a>Servis ana bilgisayarını açma

1. Hala, `Main()`hizmeti açmak için aşağıdaki satırı ekleyin.

    ```csharp
    host.Open();
    ```

1. Kullanıcıyı hizmetin çalıştığı konusunda bilgilendirin ve hizmetin nasıl kapatılacağını açıklayın.

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Bu işlemi bitirdiğinizde, hizmet ana bilgisayarını kapatın.

    ```csharp
    host.Close();
    ```

1. Projeyi oluşturmak için Ctrl+Shift+B'yi seçin.

### <a name="example-that-hosts-a-service-in-a-console-application"></a>Konsol uygulamasında bir hizmeti barındıran örnek

Tamamlanmış hizmet kodunuz aşağıdaki gibi görünmelidir. Kod, öğreticideki önceki adımlardaki hizmet sözleşmesini ve uygulamasını içerir ve hizmeti bir konsol uygulamasında barındırar.

```csharp
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         

            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Relay credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Hizmet sözleşmesi için bir WCF istemcisi oluşturma

Bir sonraki görev, bir istemci uygulaması oluşturmak ve daha sonra uygulayacağınız hizmet sözleşmesini tanımlamaktır. Bu adımlar, bir hizmet oluşturmak için kullanılan adımlara benzer: bir sözleşme tanımlama, bir *App.config* dosyasını düzenleme, geçiş hizmetine bağlanmak için kimlik bilgilerini kullanma ve benzeri. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır.

1. İstemci için geçerli Visual Studio çözümünde yeni bir proje oluşturun:

   1. **Çözüm Gezgini'nde,** geçerli çözüme (proje değil) sağ tıklayın ve**Yeni Proje** **Ekle'yi** > seçin.
   1. **Yeni bir proje ekle'de**C#için Konsol **Uygulaması'nı (.NET Framework)** seçin ve **İleri'yi**seçin.
   1. Proje *EchoClient'ı* adlandırın ve **Oluştur'u**seçin.

1. **Solution Explorer'da,** **EchoClient** projesinde, dosyayı zaten açık değilse, düzenleyicide açmak için **Program.cs** çift tıklatın.
1. `EchoClient` olan varsayılan ad alanı adını `Microsoft.ServiceBus.Samples` olarak değiştirin.
1. Servis [Veri Servisi NuGet paketini](https://www.nuget.org/packages/WindowsAzure.ServiceBus)yükleyin:

   1. **Solution**Explorer'da, **EchoClient'a** sağ tıklayın ve ardından **NuGet Paketlerini Yönet'i**seçin.
   1. **Browse'ı**seçin, ardından **WindowsAzure.ServiceBus'u**arayın ve seçin. **Yükle'yi**seçin ve kullanım koşullarını kabul edin.

      ![Servis veri servisi paketini yükleme][4]

1. Program.cs `using` dosyasındaki [System.ServiceModel](/dotnet/api/system.servicemodel) ad alanı *Program.cs* için bir deyim ekleyin.

    ```csharp
    using System.ServiceModel;
    ```

1. Hizmet sözleşmesi tanımını ad alanına aşağıdaki örnekte gösterilen şekilde ekleyin. Bu tanım, **Hizmet** projesinde kullanılan tanımla aynıdır. Bu kodu `Microsoft.ServiceBus.Samples` ad alanının en üstüne ekleyin.

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. İstemciyi oluşturmak için Ctrl+Shift+B'yi seçin.

### <a name="example-of-the-echoclient-project"></a>EchoClient projesi örneği

Aşağıdaki kod, **EchoClient** projesindeki *Program.cs* dosyasının geçerli durumunu gösterir.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>WCF istemcisini yapılandırma

Bu adımda, bu öğreticide daha önce oluşturulan hizmete erişen temel bir istemci uygulaması için bir *App.config* dosyası oluşturursunuz. Bu *App.config* dosyası sözleşmeyi, bağlamayı ve bitiş noktasının adını tanımlar. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır.

1. **Solution**Explorer'da, **EchoClient** projesinde, Visual Studio düzenleyicisinde dosyayı açmak için **App.config'e** çift tıklayın.
1. `<appSettings>` öğesinde, yer tutucuları hizmet ad alanınızdaki adla ve önceki adımların birinde kopyaladığınız SAS anahtarı ile değiştirin.
1. Öğe `system.serviceModel` içinde, bir `<client>` öğe ekleyin.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Bu kod, WCF tarzı bir istemci uygulaması tanımladığınızı bildirir.

1. `client` öğesi içinde adı, sözleşmeyi ve uç noktaya yönelik bağlama türünü tanımlayın.

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Bu kod bitiş noktasının adını tanımlar. Ayrıca, hizmette tanımlanan sözleşmeyi ve istemci uygulamasının Azure Relay ile iletişim kurmak için TCP'yi kullanması gerçeğini de tanımlar. Uç nokta adı, bir sonraki adımda bu uç nokta yapılandırmasını hizmet URI'si ile bağlamak için kullanılır.

1. **Dosya** > **Yı Kaydet'i**seçin.

### <a name="example-of-the-appconfig-file"></a>App.config dosyası örneği

Aşağıdaki kod, Echo istemcisi için *App.config* dosyasını gösterir.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client"></a>WCF istemcisini uygulama

Bu bölümde, bu öğreticide daha önce oluşturduğunuz hizmete erişen temel bir istemci uygulaması uygularsınız. Hizmete benzer şekilde, istemci azure relay'e erişmek için aynı işlemlerin çoğunu yapar:

* Bağlantı modunu ayarlar.
* Ana bilgisayar hizmetinin yer aldığı URI'yi oluşturur.
* Güvenlik kimlik bilgilerini tanımlar.
* Bağlantıya kimlik bilgilerini uygular.
* Bağlantıyı açar.
* Uygulamaya özgü görevleri gerçekleştirir.
* Bağlantıyı kapatır.

Ancak, temel farklardan biri istemci uygulaması geçiş hizmetine bağlanmak için bir kanal kullanıyor olmasıdır. Hizmet **ServiceHost**için bir çağrı kullanır. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır.

### <a name="implement-a-client-application"></a>İstemci uygulaması uygulama

1. Bağlantı modunu `AutoDetect` olarak ayarlayın. Aşağıdaki kodu **EchoClient** uygulamasının `Main()` yöntemine ekleyin.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Konsoldan okunan hizmet ad alanı ve SAS anahtarına yönelik değerleri tutması için değişkenleri tanımlayın.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Röle projenizdeki ana bilgisayarın konumunu tanımlayan URI'yi oluşturun.

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Hizmet ad alanı uç noktanız için kimlik bilgileri nesnesini oluşturun.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. *App.config* dosyasında açıklanan yapılandırmayı yükleyen kanal fabrikasını oluşturun.

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Kanal fabrikası, hizmet ve istemcinin iletişim kurmak için kullandığı bir kanal oluşturan WCF nesnesidir.

1. Kimlik bilgilerini uygulayın.

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Kanalı oluşturup hizmet tarafından kullanılması için açın.

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Yankı için işlevselliği ve temel kullanıcı arabirimini yazın.

    ```csharp
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Kod, hizmet için proxy olarak kanal nesnesi örneğini kullanır.

1. Kanalı ve fabrikayı kapatın.

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

### <a name="example-code-for-this-tutorial"></a>Bu öğretici için örnek kod

Tamamlanmış kodunuz aşağıdaki gibi görünmelidir. Bu kod, istemci uygulamasının nasıl oluşturulacağını, hizmetin yönetmeliklerini nasıl arayacağını ve işaraması tamamlaştıktan sonra istemcinin nasıl kapatılacağını gözden görüşüyor.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;


            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

1. Çözümü oluşturmak için Ctrl+Shift+B'yi seçin. Bu eylem, hem istemci projeyi hem de önceki adımlarda oluşturduğunuz hizmet projesini oluşturur.
1. İstemci uygulamasını çalıştırmadan önce hizmet uygulamasının çalıştığından emin olmanız gerekir. **Solution**Explorer'da, **EchoService** çözümüne sağ tıklayın ve **ardından Özellikler'i**seçin.
1. **Özellik Sayfalarında**, Ortak **Özellikler** > **Başlangıç Projesi'nde,** ardından Birden Çok başlangıç **projesi**seçin. **EchoService** çözümünün liste başında olduğundan emin olun.
1. **EchoService** ve **EchoClient** projeleri için **Eylem** kutusunu **Başlat** olarak ayarlayın.

    ![Proje özelliği sayfaları][5]

1. **Proje Bağımlılıkları'nı**seçin. **Projelerde,** **EchoClient'ı**seçin. **Depends için,** **EchoService'in** seçildiğinden emin olun.

    ![Proje bağımlılıkları][6]

1. **Özellik Sayfalarını**kapatmak için **Tamam'ı** seçin.
1. Her iki projeyi çalıştırmak için F5'i seçin.
1. Her iki konsol penceresi de açılır ve sizden ad alanı adı ister. Hizmetin önce çalışması gerekir, bu nedenle **EchoService** konsol penceresinde ad alanını girin ve sonra Enter'u seçin.
1. Ardından, konsol sas anahtarınızı ister. SAS tuşuna girin ve Enter'u seçin.

    Konsol penceresinden örnek çıktı sunulur. Buradaki değerler sadece örneklerdir.

    `Your Service Namespace: myNamespace`

    `Your SAS Key: <SAS key value>`

    Hizmet uygulaması, aşağıdaki örnekte görüldüğü şekilde konsol penceresini dinlediği adrese yazdırır.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`

    `Press [Enter] to exit`

1. **EchoClient** konsol penceresinde, az önce hizmet uygulaması için girdiğiniz bilgileri girin. İstemci uygulaması için aynı hizmet ad alanını ve SAS anahtar değerlerini girin.
1. Bu değerleri girdikten sonra istemci hizmete yönelik bir kanal açar ve aşağıdaki konsol çıktısı örneğinde görüldüğü şekilde bazı metinler girmenizi ister.

    `Enter text to echo (or [Enter] to exit):`

    Hizmet uygulamasına göndermek için bazı metin girin ve Enter'u seçin. Bu metin, Echo hizmet işlemi aracılığıyla hizmete gönderilir ve aşağıdaki örnek çıktıda görüldüğü şekilde hizmet konsol penceresinde görünür.

    `Echoing: My sample text`

    İstemci uygulaması, `Echo` işleminin dönüş değerini, diğer bir deyişle orijinal metni alır ve konsol penceresine yazdırır. Aşağıdaki metin istemci konsol penceresinden örnek çıktıdır.

    `Server echoed: My sample text`

1. Bu şekilde istemciden hizmete metin iletileri göndermeye devam edebilirsiniz. İşi bittiğinde, her iki uygulamayı da sona erdirmek için istemci ve servis konsolu pencerelerine girin'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Şu öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Bir şirket içi WCF REST hizmetini ağınızın dışındaki bir istemcinin kullanımına sunma](service-bus-relay-rest-tutorial.md)

[2]: ./media/service-bus-relay-tutorial/configure-echoservice-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget-service-bus.png
[4]: ./media/service-bus-relay-tutorial/install-nuget-service-bus-client.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
