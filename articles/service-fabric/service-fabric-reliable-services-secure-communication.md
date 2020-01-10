---
title: İle güvenli hizmet uzaktan iletişim iletişimleriC#
description: Azure Service Fabric kümesinde çalışan güvenilir hizmetler için C# hizmet uzaktan iletişim tabanlı iletişimin güvenliğini nasıl sağlayacağınızı öğrenin.
author: suchiagicha
ms.topic: conceptual
ms.date: 04/20/2017
ms.author: pepogors
ms.openlocfilehash: ee2f1d70f4094ccc7d80edbfaf16509b5124f607
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75609630"
---
# <a name="secure-service-remoting-communications-in-a-c-service"></a>C# Hizmette güvenli hizmet uzaktan iletişim iletişimleri
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-secure-communication.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-secure-communication-java.md)
>
>

Güvenlik, iletişimin en önemli yönlerinden biridir. Reliable Services uygulama çerçevesi, güvenliği artırmak için kullanabileceğiniz, önceden oluşturulmuş birkaç iletişim yığını ve aracı sağlar. Bu makalede, bir C# hizmette hizmet uzaktan iletişimini kullanırken güvenliğin nasıl artırılabileceği açıklanır. İçinde C#yazılmış güvenilir hizmetler için uzaktan iletişimin nasıl ayarlanacağını açıklayan mevcut bir [örnek](service-fabric-reliable-services-communication-remoting.md) üzerinde oluşturulur. 

Hizmetler ile C# hizmet uzaktan iletişimini kullanırken bir hizmetin güvenliğinin sağlanmasına yardımcı olmak için şu adımları izleyin:

1. Hizmetinize bir uzak yordam çağrısı için kullanılabilecek yöntemleri tanımlayan `IHelloWorldStateful`bir arabirim oluşturun. Hizmetiniz, `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` ad alanında belirtilen `FabricTransportServiceRemotingListener`kullanır. Bu, uzaktan iletişim özellikleri sağlayan bir `ICommunicationListener` uygulamasıdır.

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
2. Dinleyici ayarlarını ve güvenlik kimlik bilgilerini ekleyin.

    Hizmet iletişiminizin güvenli hale getirilmesine yardımcı olmak için kullanmak istediğiniz sertifikanın kümedeki tüm düğümlerde yüklü olduğundan emin olun. 
    
    > [!NOTE]
    > Linux düğümlerinde, sertifikanın */var/lib/sfcerts* dizininde pek biçimli dosyalar olarak bulunması gerekir. Daha fazla bilgi edinmek için bkz. [Linux düğümlerinde X. 509.440 sertifikalarının konumu ve biçimi](./service-fabric-configure-certificates-linux.md#location-and-format-of-x509-certificates-on-linux-nodes). 

    Dinleyici ayarlarını ve güvenlik kimlik bilgilerini sağlayabilmeniz için iki yol vardır:

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
   2. Bunları bir [yapılandırma paketi](service-fabric-application-and-service-manifests.md)kullanarak sağlayın:

       Settings. xml dosyasına adlandırılmış bir `TransportSettings` bölümü ekleyin.

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

       Bu durumda `CreateServiceReplicaListeners` yöntemi şöyle görünür:

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

        Settings. xml dosyasına bir `TransportSettings` bölümü eklerseniz, `FabricTransportRemotingListenerSettings` tüm ayarları varsayılan olarak bu bölümden yükler.

        ```xml
        <!--"TransportSettings" section .-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        Bu durumda `CreateServiceReplicaListeners` yöntemi şöyle görünür:

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
3. Güvenli bir hizmette yöntemleri, bir hizmet proxy 'si oluşturmak için `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` sınıfını kullanmak yerine, güvenli bir hizmet üzerinde çağırdığınızda, `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`kullanın. `SecurityCredentials`içeren `FabricTransportRemotingSettings`geçirin.

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

    İstemci kodu bir hizmetin parçası olarak çalışıyorsa, Settings. xml dosyasından `FabricTransportRemotingSettings` yükleyebilirsiniz. Daha önce gösterildiği gibi, hizmet koduna benzer bir Merhaba Dünya Clienttransportsettings bölümü oluşturun. İstemci kodunda aşağıdaki değişiklikleri yapın:

    ```csharp
    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.LoadFrom("HelloWorldClientTransportSettings")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    İstemci bir hizmetin parçası olarak çalışmıyorsa, client_name. exe ' nin bulunduğu konumda bir client_name. Settings. xml dosyası oluşturabilirsiniz. Ardından bu dosyada bir TransportSettings bölümü oluşturun.

    Hizmete benzer şekilde, Client Settings. xml/client_name. Settings. xml ' de bir `TransportSettings` bölümü eklerseniz, `FabricTransportRemotingSettings` bu bölümdeki tüm ayarları varsayılan olarak yükler.

    Bu durumda, önceki kod daha da basitleştirilmiştir:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```


Sonraki adım olarak, [Reliable Services içinde OWIN Ile Web API 'sini](service-fabric-reliable-services-communication-webapi.md)okuyun.
