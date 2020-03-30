---
title: Java ile güvenli hizmet remoting iletişim
description: Azure Service Fabric kümesinde çalışan Java güvenilir hizmetleri için hizmet remoting tabanlı iletişimi nasıl güvenli hale erdireceklerini öğrenin.
author: PavanKunapareddyMSFT
ms.topic: conceptual
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: adefeadf939d398268624343d82c18cbf5ec87cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75609647"
---
# <a name="secure-service-remoting-communications-in-a-java-service"></a>Java hizmetinde güvenli hizmet remoting iletişimi
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-secure-communication.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-secure-communication-java.md)
>
>

Güvenlik iletişimin en önemli yönlerinden biridir. Güvenilir Hizmetler uygulama çerçevesi, güvenliği artırmak için kullanabileceğiniz birkaç önceden oluşturulmuş iletişim yığını ve aracı sağlar. Bu makalede, bir Java hizmetinde hizmet remoting kullanırken güvenliği artırmak için nasıl açıklanmıştır. Java'da yazılmış güvenilir hizmetler için remoting'in nasıl ayarlanolacağını açıklayan varolan bir [örnek](service-fabric-reliable-services-communication-remoting-java.md) üzerine inşa edin. 

Java hizmetleriyle hizmet remoting'i kullanırken bir hizmetin güvenliğini sağlamak için aşağıdaki adımları izleyin:

1. Hizmetinizde uzaktan `HelloWorldStateless`yordam çağrısı için kullanılabilecek yöntemleri tanımlayan bir arabirim oluşturun. Hizmetiniz, pakette beyan edilen, kullanacaktır. `FabricTransportServiceRemotingListener` `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` Bu, `CommunicationListener` remoting yetenekleri sağlayan bir uygulamadır.

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
2. Dinleyici ayarları ve güvenlik kimlik bilgileri ekleyin.

    Hizmet iletişiminizin güvenliğini sağlamak için kullanmak istediğiniz sertifikanın kümedeki tüm düğümlere yüklü olduğundan emin olun. Linux'ta çalışan hizmetler için sertifikanın PEM biçimli bir dosya olarak kullanılabilmesi gerekir; sertifika `.pem` ve özel anahtar içeren bir `.crt` dosya veya sertifika ve `.key` özel anahtar içeren bir dosya. Daha fazla bilgi için, [Linux düğümlerinde X.509 sertifikalarının konumu ve biçimine](./service-fabric-configure-certificates-linux.md#location-and-format-of-x509-certificates-on-linux-nodes)bakın.
    
    Dinleyici ayarlarını ve güvenlik kimlik bilgilerini sağlamanın iki yolu vardır:

   1. Bir [config paketi](service-fabric-application-and-service-manifests.md)kullanarak bunları sağlayın:

       settings.xml dosyasına adlandırılmış `TransportSettings` bir bölüm ekleyin.

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

       Bu durumda, `createServiceInstanceListeners` yöntem aşağıdaki gibi görünecektir:

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        Settings.xml `TransportSettings` dosyasına herhangi bir önek olmadan `FabricTransportListenerSettings` bir bölüm eklerseniz, varsayılan olarak bu bölümdeki tüm ayarları yüklersiniz.

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        Bu durumda, `CreateServiceInstanceListeners` yöntem aşağıdaki gibi görünecektir:

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. Bir hizmet proxy oluşturmak için `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` sınıfı kullanmak yerine, remoting yığını nı kullanarak güvenli `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`bir hizmetteki yöntemleri çağırırsanız, kullanın.

    İstemci kodu bir hizmetin parçası olarak `FabricTransportSettings` çalışıyorsa, settings.xml dosyasından yükleyebilirsiniz. Daha önce gösterildiği gibi hizmet koduna benzer bir TransportSettings bölümü oluşturun. İstemci kodunda aşağıdaki değişiklikleri yapın:

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
