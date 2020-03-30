---
title: Linux'taki uygulamalar için sertifikaları yapılandırma
description: Linux kümesinde Service Fabric çalışma zamanı ile uygulamanızın sertifikalarını yapılandırın
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: pepogors
ms.openlocfilehash: 802e76614f51e1f6479a311e61a49d83b8125546
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282581"
---
# <a name="certificates-and-security-on-linux-clusters"></a>Linux kümelerinde sertifikalar ve güvenlik

Bu makalede, Linux kümelerinde X.509 sertifikaları yapılandırma hakkında bilgi sağlar.

## <a name="location-and-format-of-x509-certificates-on-linux-nodes"></a>Linux düğümlerinde X.509 sertifikalarının konumu ve biçimi

Service Fabric genellikle X.509 sertifikalarının Linux küme düğümlerinde */var/lib/sfcerts* dizininde bulunmasını bekler. Bu küme sertifikaları, istemci sertifikaları, vb için geçerlidir. Bazı durumlarda, sertifikalar için *var/lib/sfcerts* klasörü dışında bir konum belirtebilirsiniz. Örneğin, Hizmet Kumaşı Java SDK kullanılarak oluşturulmuş Güvenilir Hizmetler ile, uygulamaya özel bazı sertifikalar için yapılandırma paketi (Settings.xml) aracılığıyla farklı bir konum belirtebilirsiniz. Daha fazla bilgi için [yapılandırma paketinde (Settings.xml) başvurulan Sertifikalar'a](#certificates-referenced-in-the-configuration-package-settingsxml)bakın.

Service Fabric, Linux kümeleri için sertifikaların hem sertifika hem de özel anahtar içeren bir .pem dosyası veya sertifikayı ve özel anahtarı içeren bir .key dosyası içeren bir .crt dosyası olarak bulunmasını bekler. Tüm dosyalar PEM formatında olmalıdır. 

Sertifikanızı Azure Key Vault'tan Kaynak [Yöneticisi şablonu](./service-fabric-cluster-creation-create-template.md) veya [PowerShell](https://docs.microsoft.com/powershell/module/az.servicefabric/?view=azps-2.6.0) komutları kullanarak yüklerseniz, sertifika her düğümde */var/lib/sfcerts* dizininde doğru biçimde yüklenir. Bir sertifikayı başka bir yöntemle yüklerseniz, sertifikanın küme düğümlerine doğru şekilde yüklendiğinden emin olmalısınız.

## <a name="certificates-referenced-in-the-application-manifest"></a>Başvuru bildiriminde başvurulan sertifikalar

Uygulama bildiriminde belirtilen sertifikalar, örneğin [**SecretsCertificate**](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#secretscertificate-element) veya [**EndpointCertificate**](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#endpointcertificate-element) öğeleri aracılığıyla */var/lib/sfcerts* dizininde bulunmalıdır. Uygulama bildiriminde sertifikabelirtmek için kullanılan öğeler bir yol özniteliği almaz, bu nedenle sertifikaların varsayılan dizinde bulunması gerekir. Bu öğeler isteğe bağlı bir **X509StoreName** özniteliği alır. Varsayılan değer, Linux düğümleri üzerindeki */var/lib/sfcerts* dizinine işaret eden "My"dir. Başka bir değer bir Linux kümesi üzerinde tanımsızdır. Linux kümelerinde çalışan uygulamalar için **X509StoreName** özniteliğini atlanızır. 

## <a name="certificates-referenced-in-the-configuration-package-settingsxml"></a>Yapılandırma paketinde başvurulan sertifikalar (Settings.xml)

Bazı hizmetler [için, ConfigPackage'da](./service-fabric-application-and-service-manifests.md) X.509 sertifikalarını yapılandırabilirsiniz (varsayılan olarak Ayarlar.xml). Örneğin, Service Fabric .NET Core veya Java SDK'ları ile oluşturulmuş Güvenilir Hizmetler hizmetleri için RPC kanallarını güvenli hale getirmek için kullanılan sertifikaları beyan ettiğinizde durum böyledir. Yapılandırma paketinde sertifikalara başvurmanın iki yolu vardır. Destek .NET Core ve Java SDK'ları arasında değişir.

### <a name="using-x509-securitycredentialstype"></a>X509 SecurityCredentialsType kullanma

.NET veya Java SDK'ları with, **SecurityCredentialsType**için **X509** belirtebilirsiniz. Bu `X509Credentials` ([.NET](https://msdn.microsoft.com/library/system.fabric.x509credentials.aspx)/[Java](https://docs.microsoft.com/java/api/system.fabric.x509credentials)) türüne karşılık gelir `SecurityCredentials` ([.NET](https://msdn.microsoft.com/library/system.fabric.securitycredentials.aspx)/[Java](https://docs.microsoft.com/java/api/system.fabric.securitycredentials)).

**X509** başvurusu sertifikayı bir sertifika deposunda bulur. Aşağıdaki XML, sertifikanın konumunu belirtmek için kullanılan parametreleri gösterir:

```xml
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
```

Linux'ta çalışan bir hizmet için **LocalMachine**/Sertifikaların varsayılan konumuolan */var/lib/sfcerts* dizinine işaret**ediyor.** Linux **için, CertificateStoreLocation** ve **CertificateStoreName'nin** diğer kombinasyonları tanımsızdır. 

**SertifikaStoreLocation** parametresi için **LocalMachine'i** her zaman belirtin. Varsayılan olarak "Benim" olarak da **belirttiğinden, CertificateStoreName** parametresini belirtmenize gerek yoktur. **X509** referansı ile sertifika dosyaları küme düğümündeki */var/lib/sfcerts* dizininde yer almalıdır.  

Aşağıdaki XML, bu stili temel alan bir **TransportSettings** bölümünü gösterir:

```xml
<Section Name="HelloWorldStatefulTransportSettings">
    <Parameter Name="MaxMessageSize" Value="10000000" />
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
    <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
    <Parameter Name="CertificateRemoteThumbprints" Value="9FEF3950642138446CC364A396E1E881DB76B483" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
</Section>
```

### <a name="using-x509_2-securitycredentialstype"></a>X509_2 SecurityCredentialsType kullanma

Java SDK ile **SecurityCredentialsType**için **X509_2** belirtebilirsiniz. Bu `X509Credentials2` `SecurityCredentials` ([Java](https://docs.microsoft.com/java/api/system.fabric.x509credentials2)) türüne karşılık gelir ([Java](https://docs.microsoft.com/java/api/system.fabric.securitycredentials)). 

**X509_2** bir başvuruyla, bir yol parametresi belirtirsiniz, böylece sertifikayı */var/lib/sfcerts*dışındaki bir dizinde bulabilirsiniz.  Aşağıdaki XML, sertifikanın konumunu belirtmek için kullanılan parametreleri gösterir: 

```xml
     <Parameter Name="SecurityCredentialsType" Value="X509_2" />
     <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
```

Aşağıdaki XML, bu stili temel alan bir **TransportSettings** bölümünü gösterir.

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

> [!NOTE]
> Sertifika, önceki XML'de .crt dosyası olarak belirtilir. Bu, aynı konumda özel anahtarı içeren bir .key dosyası da olduğu anlamına gelir.

## <a name="configure-a-reliable-services-app-to-run-on-linux-clusters"></a>Güvenilir Hizmetler uygulamasını Linux kümelerinde çalışacak şekilde yapılandırma

Service Fabric SDK'lar, platformdan yararlanmak için Service Fabric çalışma zamanı API'leri ile iletişim kurmanızı sağlar. Bu işlevi güvenli Linux kümelerinde kullanan herhangi bir uygulamayı çalıştırdığınızda, uygulamanızı Hizmet Kumaşı çalışma süresiyle doğrulamak için kullanabileceği bir sertifikayla yapılandırmanız gerekir. .NET Core veya Java SDK'ları kullanılarak yazılmış Service Fabric Reliable Service hizmetlerini içeren uygulamalar bu yapılandırmayı gerektirir. 

Bir uygulamayı yapılandırmak için, *ApplicationManifest.xml* dosyasındaki **ApplicationManifest** etiketinin altında bulunan **Sertifikalar** etiketinin altına bir [**SecretsCertificate**](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#secretscertificate-element) öğesi ekleyin. Aşağıdaki XML, parmak izinden başlandı bir sertifika gösterir: 

```xml
   <Certificates>
       <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="0A00AA0AAAA0AAA00A000000A0AA00A0AAAA00" />
   </Certificates>   
```

Küme sertifikasına veya her küme düğümüne yüklediğiniz bir sertifikaya başvuruda bulunabilirsiniz. Linux'ta sertifika dosyaları */var/lib/sfcerts* dizininde bulunmalıdır. Daha fazla bilgi için, [Linux düğümlerinde X.509 sertifikalarının konumu ve biçimine](#location-and-format-of-x509-certificates-on-linux-nodes)bakın.



