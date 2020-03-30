---
title: 'C ile güvenli hizmet remoting iletişim #'
description: Azure Service Fabric kümesinde çalışan C# güvenilir hizmetler için hizmet remoting tabanlı iletişimi nasıl güvenli hale erdireceklerini öğrenin.
author: suchiagicha
ms.topic: conceptual
ms.date: 04/20/2017
ms.author: pepogors
ms.openlocfilehash: ee2f1d70f4094ccc7d80edbfaf16509b5124f607
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75609630"
---
# <a name="secure-service-remoting-communications-in-a-c-service"></a>C# hizmetinde güvenli hizmet remoting iletişimi
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-secure-communication.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-secure-communication-java.md)
>
>

Güvenlik iletişimin en önemli yönlerinden biridir. Güvenilir Hizmetler uygulama çerçevesi, güvenliği artırmak için kullanabileceğiniz birkaç önceden oluşturulmuş iletişim yığını ve aracı sağlar. Bu makalede, c# hizmetinde hizmet remoting kullanırken güvenliği nasıl artırabileceğiniz açıklanmıştır. C# ile yazılmış güvenilir hizmetler için remoting'in nasıl ayarlanolacağını açıklayan varolan bir [örnek](service-fabric-reliable-services-communication-remoting.md) üzerine inşa edin. 

C# hizmetleriyle hizmet remotingini kullanırken bir hizmetin güvenliğini sağlamak için aşağıdaki adımları izleyin:

1. Hizmetinizde uzaktan `IHelloWorldStateful`yordam çağrısı için kullanılabilecek yöntemleri tanımlayan bir arabirim oluşturun. Hizmetiniz, ad alanında bildirilen, kullanır. `FabricTransportServiceRemotingListener` `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` Bu, `ICommunicationListener` remoting yetenekleri sağlayan bir uygulamadır.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```
2. Dinleyici ayarları ve güvenlik kimlik bilgileri ekleyin.

    Hizmet iletişiminizin güvenliğini sağlamak için kullanmak istediğiniz sertifikanın kümedeki tüm düğümlere yüklü olduğundan emin olun. 
    
    > [!NOTE]
    > Linux düğümlerinde sertifikanın */var/lib/sfcerts* dizininde PEM biçimli dosyalar olarak bulunması gerekir. Daha fazla bilgi için, [Linux düğümlerinde X.509 sertifikalarının konumu ve biçimine](./service-fabric-configure-certificates-linux.md#location-and-format-of-x509-certificates-on-linux-nodes)bakın. 

    Dinleyici ayarlarını ve güvenlik kimlik bilgilerini sağlamanın iki yolu vardır:

   1. Bunları doğrudan hizmet kodunda sağlayın:

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           FabricTransportRemotingListenerSettings  listenerSettings = new FabricTransportRemotingListenerSettings
           {
               MaxMessageSize = 10000000,
               SecurityCredentials = GetSecurityCredentials()
           };
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
           };
       }

       private static SecurityCredentials GetSecurityCredentials()
       {
           // Provide certificate details.
           var x509Credentials = new X509Credentials
           {
               FindType = X509FindType.FindByThumbprint,
               FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
               StoreLocation = StoreLocation.LocalMachine,
               StoreName = "My",
               ProtectionLevel = ProtectionLevel.EncryptAndSign
           };
           x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
           x509Credentials.RemoteCertThumbprints.Add("9FEF3950642138446CC364A396E1E881DB76B483");
           return x509Credentials;
       }
       ```
   2. Bir [config paketi](service-fabric-application-and-service-manifests.md)kullanarak bunları sağlayın:

       settings.xml dosyasına adlandırılmış `TransportSettings` bir bölüm ekleyin.

       ```xml
       <Section Name="HelloWorldStatefulTransportSettings">
           <Parameter Name="MaxMessageSize" Value="10000000" />
           <Parameter Name="SecurityCredentialsType" Value="X509" />
           <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
           <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
           <Parameter Name="CertificateRemoteThumbprints" Value="9FEF3950642138446CC364A396E1E881DB76B483" />
           <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
           <Parameter Name="CertificateStoreName" Value="My" />
           <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
           <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
       </Section>
       ```

       Bu durumda, `CreateServiceReplicaListeners` yöntem aşağıdaki gibi görünecektir:

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(
                       context,this,FabricTransportRemotingListenerSettings .LoadFrom("HelloWorldStatefulTransportSettings")))
           };
       }
       ```

        settings.xml `TransportSettings` dosyasına bir bölüm eklerseniz, `FabricTransportRemotingListenerSettings` varsayılan olarak bu bölümdeki tüm ayarları yüklersiniz.

        ```xml
        <!--"TransportSettings" section .-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        Bu durumda, `CreateServiceReplicaListeners` yöntem aşağıdaki gibi görünecektir:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                return new[]{
                        new ServiceReplicaListener(
                            (context) => new FabricTransportServiceRemotingListener(context,this))};
            };
        }
        ```
3. Bir hizmet proxy oluşturmak için `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` sınıfı kullanmak yerine, remoting yığını nı kullanarak güvenli `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`bir hizmetteki yöntemleri çağırırsanız, kullanın. Pass `FabricTransportRemotingSettings`, içeren `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "9FEF3950642138446CC364A396E1E881DB76B483",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
    x509Credentials.RemoteCertThumbprints.Add("4FEF3950642138446CC364A396E1E881DB76B48C");

    FabricTransportRemotingSettings transportSettings = new FabricTransportRemotingSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    İstemci kodu bir hizmetin parçası olarak `FabricTransportRemotingSettings` çalışıyorsa, settings.xml dosyasından yükleyebilirsiniz. Daha önce gösterildiği gibi hizmet koduna benzer bir HelloWorldClientTransportSettings bölümü oluşturun. İstemci kodunda aşağıdaki değişiklikleri yapın:

    ```csharp
    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.LoadFrom("HelloWorldClientTransportSettings")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    İstemci bir hizmetin parçası olarak çalışmıyorsa, client_name.exe'nin bulunduğu konumda bir client_name.settings.xml dosyası oluşturabilirsiniz. Ardından bu dosyada bir TransportSettings bölümü oluşturun.

    Hizmete benzer şekilde, istemci `TransportSettings` settings.xml/client_name.settings.xml'e `FabricTransportRemotingSettings` bir bölüm eklerseniz, varsayılan olarak bu bölümdeki tüm ayarları yükler.

    Bu durumda, önceki kod daha da basitleştirilmiştir:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```


Bir sonraki adım olarak, [Güvenilir Hizmetler OWIN ile Web API](service-fabric-reliable-services-communication-webapi.md)okuyun.
