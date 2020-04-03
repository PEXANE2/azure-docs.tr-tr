---
title: Azure Service Fabric ters proxy güvenli iletişim
description: Azure Hizmet Kumaşı uygulamasında uçuça güvenli iletişim ietkinleştirmek için ters proxy'yi yapılandırın.
author: kavyako
ms.topic: conceptual
ms.date: 08/10/2017
ms.author: kavyako
ms.openlocfilehash: 61a8d1e766ea576f7d2984add239b0da7e2e8183
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80617120"
---
# <a name="connect-to-a-secure-service-with-the-reverse-proxy"></a>Ters proxy ile güvenli bir hizmete bağlanma

Bu makalede, ters proxy ve hizmetler arasında güvenli bağlantı kurmak için nasıl açıklanmaktadır, böylece güvenli kanal sonuna kadar bir son sağlayan. Ters proxy hakkında daha fazla bilgi edinmek için [Azure Hizmet Kumaşı'nda Ters Proxy'ye](service-fabric-reverseproxy.md) bakın

Güvenli hizmetlere bağlanma, yalnızca ters proxy HTTPS'yi dinleyecek şekilde yapılandırıldıklarında desteklenir. Bu makalede, bu durumda varsayar.
Hizmet [Kumaşı'ndaki](service-fabric-reverseproxy-setup.md) ters proxy'yi yapılandırmak için Azure Hizmet Kumaşı'ndaki Kurulum ters proxy'sine bakın.

## <a name="secure-connection-establishment-between-the-reverse-proxy-and-services"></a>Ters proxy ve hizmetler arasında güvenli bağlantı kurulması 

### <a name="reverse-proxy-authenticating-to-services"></a>Hizmetlere kimlik doğrulama ters:
Ters proxy sertifikasını kullanarak hizmetleriçin kendini tanımlar. Azure kümeleri için sertifika, Kaynak Yöneticisi şablonunun [**Microsoft.ServiceFabric/clusters**](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/clusters) [Kaynak türü bölümünde](../azure-resource-manager/templates/template-syntax.md) ters ***ProxySertifikası*** özelliğiyle belirtilir. Bağımsız kümeler için sertifika, ClusterConfig.json'un **Güvenlik** bölümünde Ters Proxy Sertifikası veya Ters ***ProxySertifikasıOrtak*** ***Adları*** özelliğiyle belirtilir. Daha fazla bilgi edinmek için [bkz.](service-fabric-reverseproxy-setup.md#enable-reverse-proxy-on-standalone-clusters) 

Hizmetler ters proxy tarafından sunulan sertifikayı doğrulamak için mantığı uygulayabilirsiniz. Hizmetler, kabul edilen istemci sertifikası ayrıntılarını yapılandırma paketinde yapılandırma ayarları olarak belirtebilir. Bu, çalışma zamanında okunabilir ve ters proxy tarafından sunulan sertifikayı doğrulamak için kullanılabilir. Yapılandırma ayarlarını eklemek için [uygulama parametrelerini yönet'e](service-fabric-manage-multiple-environment-app-configuration.md) bakın. 

### <a name="reverse-proxy-verifying-the-services-identity-via-the-certificate-presented-by-the-service"></a>Hizmet tarafından sunulan sertifika ile hizmetin kimliğini doğrulayan ters proxy:
Ters proxy, hizmetler tarafından sunulan sertifikaların sunucu sertifikası doğrulaması gerçekleştirmek için aşağıdaki ilkeleri destekler: Yok, ServiceCommonNameAndIssuer ve ServiceCertificateThumbprints.
Kullanılacak ters proxy ilkesini seçmek için, [fabricSettings](service-fabric-cluster-fabric-settings.md)altında **ApplicationGateway/Http** bölümünde **ApplicationCertificateValidationPolicy'yi** belirtin.

Sonraki bölümde, bu seçeneklerin her biri için yapılandırma ayrıntıları gösterilmektedir.

### <a name="service-certificate-validation-options"></a>Hizmet belgesi doğrulama seçenekleri 

- **Yok**: Ters proxy, yakın hizmet sertifikasının doğrulamasını atlar ve güvenli bağlantıyı kurar. Bu varsayılan davranıştır.
[**ApplicationGateway/Http**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) bölümünde **Hiçbir** değere sahip **ApplicationCertificateValidationPolicy'yi** belirtin.

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "ApplicationCertificateValidationPolicy",
                   "value": "None"
                 }
               ]
             }
           ],
           ...
   }
   ```

- **ServiceCommonNameAndIssuer**: Ters proxy, hizmet tarafından sunulan sertifikayı sertifikanın ortak adına ve hemen verenin parmak izine göre doğrular: [**ApplicationGateway/Http**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) bölümünde **ServiceCommonNameAndIssuer** değeri olan **ApplicationCertificateValidationPolicy'yi** belirtiniz.

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "ApplicationCertificateValidationPolicy",
                   "value": "ServiceCommonNameAndIssuer"
                 }
               ]
             }
           ],
           ...
   }
   ```

   Hizmet ortak adı ve veren parmak izlerinin listesini belirtmek için, aşağıda gösterildiği gibi **fabricSettings**altında bir [**ApplicationGateway/Http/ServiceCommonNameAndIssuer**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttpservicecommonnameandissuer) bölümünü ekleyin. **Parametreler** dizisine birden çok sertifika ortak adı ve veren parmak izi çiftleri eklenebilir. 

   Bitiş noktası ters proxy ortak adı ve veren parmak izi burada belirtilen değerlerden herhangi biriyle eşleşen bir sertifika sunar a bağlanıyorsa, bir TLS kanalı oluşturulur.
   Sertifika ayrıntılarıeşleştirememesi üzerine, ters proxy istemcinin isteğini 502 (Kötü Ağ Geçidi) durum koduyla başarısızlığa uğratTır. HTTP durum satırında "Geçersiz SSL Sertifikası" ibaresi de bulunur. 

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http/ServiceCommonNameAndIssuer",
               "parameters": [
                 {
                   "name": "WinFabric-Test-Certificate-CN1",
                   "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 b4 22 11"
                 },
                 {
                   "name": "WinFabric-Test-Certificate-CN2",
                   "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 11 33 44"
                 }
               ]
             }
           ],
           ...
   }
   ```

- **ServiceCertificateThumbprints**: Ters proxy, yakın hizmet sertifikasını parmak izine göre doğrular. Hizmetler kendi imzalı sertifikalarla yapılandırıldığında bu rotayı kullanmayı seçebilirsiniz: [**ApplicationGateway/Http**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) bölümünde value **ServiceCertificateThumbprints** ile **ApplicationCertificateValidationPolicy'yi** belirtin.

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "ApplicationCertificateValidationPolicy",
                   "value": "ServiceCertificateThumbprints"
                 }
               ]
             }
           ],
           ...
   }
   ```

   Ayrıca **ApplicationGateway/Http** bölümünde **ServiceCertificateThumbprints** girişi olan parmak izlerini belirtin. Birden çok parmak izi, aşağıda gösterildiği gibi, değer alanında virgülle ayrılmış bir liste olarak belirtilebilir:

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                   ...
                 {
                   "name": "ServiceCertificateThumbprints",
                   "value": "78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 bf,78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 b9"
                 }
               ]
             }
           ],
           ...
   }
   ```

   Sunucu sertifikasının parmak izi bu config girişinde listelenmişse, ters proxy TLS bağlantısını başarır. Aksi takdirde, bağlantıyı sona erdirir ve istemcinin isteğini 502 (Kötü Ağ Geçidi) ile başarısız olur. HTTP durum satırında "Geçersiz SSL Sertifikası" ibaresi de bulunur.

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a>Hizmetler güvenli ve güvenli olmayan uç noktaları ortaya çıkardığında bitiş noktası seçim mantığı
Hizmet dokusu, bir hizmet için birden çok uç nokta yapılandırmayı destekler. Daha fazla bilgi için [bkz.](service-fabric-service-manifest-resources.md)

Ters [proxy, hizmet URI'deki](./service-fabric-reverseproxy.md#uri-format-for-addressing-services-by-using-the-reverse-proxy) **ListenerName** sorgu parametresini temel alan isteği iletmek için uç noktalardan birini seçer. **ListenerName** parametresi belirtilmemişse, ters proxy uç noktalar listesinden herhangi bir bitiş noktası seçebilir. Hizmet için yapılandırılan uç noktalara bağlı olarak, seçilen bitiş noktası bir HTTP veya HTTPS bitiş noktası olabilir. Ters proxy'nin "yalnızca güvenli modda" çalışmasını istediğiniz senaryolar veya gereksinimler olabilir; diğer bir diğer olarak, güvenli ters proxy'nin güvenli olmayan uç noktalara istekleri iletmesini istemezsinuz. Yalnızca güvenli modda ters proxy ayarlamak için, [**ApplicationGateway/Http**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) bölümünde **geçerli** olan **SecureOnlyMode** yapılandırma girişini belirtin.   

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "SecureOnlyMode",
                "value": true
              }
            ]
          }
        ],
        ...
}
```

> [!NOTE]
> **SecureOnlyMode'da**çalışırken, istemci BIR HTTP(güvenli olmayan) bitiş noktasına karşılık gelen bir **Dinleyici Adı** belirtmişse, ters proxy isteği 404 (Bulunamadı) HTTP durum koduyla başarısız olur.

## <a name="setting-up-client-certificate-authentication-through-the-reverse-proxy"></a>Ters proxy üzerinden istemci sertifikası kimlik doğrulaması ayarlama
TLS sonlandırma ters proxy'de gerçekleşir ve tüm istemci sertifikası verileri kaybolur. Hizmetlerin istemci sertifikası kimlik doğrulaması gerçekleştirmek için [**ApplicationGateway/Http**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) bölümünde **ForwardClientCertificate** ayarını belirtin.

1. **ForwardClientCertificate** **false**olarak ayarlandığında, ters proxy istemci ile TLS el sıkışma sırasında istemci sertifikası talep etmez.
Bu varsayılan davranıştır.

2. **ForwardClientCertificate** **doğru**ayarlandığında, ters proxy istemci ile TLS el sıkışma sırasında istemcinin sertifikasını ister.
Daha sonra istemci sertifika verilerini **X-Client-Certificate**adlı özel bir HTTP üstbilgisinde iletir. Üstbilgi değeri, istemcisertifikasının base64 kodlanmış PEM biçimi dizesidir. Hizmet, sertifika verilerini inceledikten sonra uygun durum koduyla isteği gerçekleştirebilir/başarısız olabilir.
İstemci bir sertifika sunmuyorsa, proxy'yi geri alet boş bir üstbilgi iletin ve servisin servisin servisin işlemesine izin verir.

> [!NOTE]
> Ters proxy sadece bir iletmediricidir. İstemcinin sertifikasının herhangi bir doğrulanmasını gerçekleştirmez.


## <a name="next-steps"></a>Sonraki adımlar
* [Küme üzerinde ters proxy'yi ayarlama ve yapılandırma.](service-fabric-reverseproxy-setup.md)
* Güvenli [hizmetlere bağlanmak için ters proxy'yi yapılandırmaya](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/Reverse-Proxy-Sample#configure-reverse-proxy-to-connect-to-secure-services) bakın
* [GitHub'daki örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)bir projedeki hizmetler arasındaki HTTP iletişimörneğine bakın.
* [Güvenilir Hizmetler remoting ile uzaktan yordam çağrıları](service-fabric-reliable-services-communication-remoting.md)
* [Güvenilir Hizmetlerde OWIN kullanan Web API'sı](service-fabric-reliable-services-communication-webapi.md)
* [Küme sertifikalarını yönetme](service-fabric-cluster-security-update-certs-azure.md)
