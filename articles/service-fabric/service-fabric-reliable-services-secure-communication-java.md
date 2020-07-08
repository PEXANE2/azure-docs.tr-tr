---
title: Java ile güvenli hizmet uzaktan iletişim iletişimleri
description: Azure Service Fabric kümesinde çalışan Java güvenilir Hizmetleri için hizmet uzaktan iletişim tabanlı iletişimin güvenliğini nasıl sağlayacağınızı öğrenin.
author: PavanKunapareddyMSFT
ms.topic: conceptual
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: adefeadf939d398268624343d82c18cbf5ec87cd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75609647"
---
# <a name="secure-service-remoting-communications-in-a-java-service"></a>Java hizmetinde güvenli hizmet uzaktan iletişim iletişimleri
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-secure-communication.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-secure-communication-java.md)
>
>

Güvenlik, iletişimin en önemli yönlerinden biridir. Reliable Services uygulama çerçevesi, güvenliği artırmak için kullanabileceğiniz, önceden oluşturulmuş birkaç iletişim yığını ve aracı sağlar. Bu makalede, bir Java hizmetinde hizmet uzaktan iletişimini kullanırken güvenliğin nasıl artırılabileceği açıklanır. Java 'da yazılmış güvenilir hizmetler için uzaktan iletişimin nasıl ayarlanacağını anlatan mevcut bir [örnek](service-fabric-reliable-services-communication-remoting-java.md) üzerinde oluşturulur. 

Java hizmetleri ile hizmet uzaktan iletişimini kullanırken bir hizmetin güvenliğinin sağlanmasına yardımcı olmak için şu adımları izleyin:

1. `HelloWorldStateless`Hizmetinize bir uzak yordam çağrısı için kullanılabilecek yöntemleri tanımlayan bir arabirim oluşturun. Hizmetiniz `FabricTransportServiceRemotingListener` , pakette belirtilen kullanacaktır `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` . Bu, `CommunicationListener` Uzaktan iletişim özellikleri sağlayan bir uygulamasıdır.

    ```java
    public interface HelloWorldStateless extends Service {
        CompletableFuture<String> getHelloWorld();
    }

    class HelloWorldStatelessImpl extends StatelessService implements HelloWorldStateless {
        @Override
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
        return listeners;
        }

        public CompletableFuture<String> getHelloWorld() {
            return CompletableFuture.completedFuture("Hello World!");
        }
    }
    ```
2. Dinleyici ayarlarını ve güvenlik kimlik bilgilerini ekleyin.

    Hizmet iletişiminizin güvenli hale getirilmesine yardımcı olmak için kullanmak istediğiniz sertifikanın kümedeki tüm düğümlerde yüklü olduğundan emin olun. Linux üzerinde çalışan hizmetler için, sertifika pek-formmasıor dosyası olarak kullanılabilir olmalıdır; `.pem`sertifikayı ve özel anahtarı içeren bir dosya ya da `.crt` sertifikayı içeren bir dosya ve `.key` özel anahtarı içeren bir dosya. Daha fazla bilgi edinmek için bkz. [Linux düğümlerinde X. 509.440 sertifikalarının konumu ve biçimi](./service-fabric-configure-certificates-linux.md#location-and-format-of-x509-certificates-on-linux-nodes).
    
    Dinleyici ayarlarını ve güvenlik kimlik bilgilerini sağlayabilmeniz için iki yol vardır:

   1. Bunları bir [yapılandırma paketi](service-fabric-application-and-service-manifests.md)kullanarak sağlayın:

       `TransportSettings`settings.xml dosyasına bir adlandırılmış bölüm ekleyin.

       ```xml
       <!--Section name should always end with "TransportSettings".-->
       <!--Here we are using a prefix "HelloWorldStateless".-->
        <Section Name="HelloWorldStatelessTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509_2" />
            <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptandSign" />
            <Parameter Name="CertificateRemoteThumbprints" Value="BD1C71E248B8C6834C151174DECDBDC02DE1D954" />
        </Section>

       ```

       Bu durumda `createServiceInstanceListeners` Yöntem şöyle görünür:

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        `TransportSettings`Herhangi bir önek olmadan settings.xml dosyasına bir bölüm eklerseniz, `FabricTransportListenerSettings` Bu bölümdeki tüm ayarları varsayılan olarak yükler.

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        Bu durumda `CreateServiceInstanceListeners` Yöntem şöyle görünür:

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. Güvenli bir hizmette yöntemleri, `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` bir hizmet proxy 'si oluşturmak için sınıfını kullanmak yerine, uzak bir hizmet proxy 'si kullanarak çağırdığınızda, kullanın `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory` .

    İstemci kodu bir hizmetin parçası olarak çalışıyorsa, `FabricTransportSettings` settings.xml dosyasından yükleyebilirsiniz. Daha önce gösterildiği gibi, hizmet koduna benzer bir TransportSettings bölümü oluşturun. İstemci kodunda aşağıdaki değişiklikleri yapın:

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
