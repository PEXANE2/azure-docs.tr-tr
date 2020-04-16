---
title: Azure Hizmet Kumaş ı probları
description: Uygulama ve hizmet bildirim dosyalarını kullanarak Azure Hizmet Dokusunda Liveness Probe nasıl modellendirilir?
ms.topic: conceptual
ms.date: 3/12/2020
ms.openlocfilehash: 38f3888a29bf505b723d40bc7cd08fb0c7e29eff
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431221"
---
# <a name="liveness-probe"></a>Canlılık Sondası
7.1 Service Fabric ile başlayarak [konteyner uygulamaları][containers-introduction-link] için Canlılık Prob mekanizmasını destekler. Liveness Probe konteyner uygulamasının canlılığını duyurmaya yardımcı olur ve zamanında yanıt vermedikleri nde yeniden başlatılamasına neden olur.
Bu makalede, bildirim dosyaları aracılığıyla bir Liveness Probe nasıl tanımlandığına genel bir bakış sağlar.

Bu makaleye geçmeden önce Service Fabric uygulama modeli ve [Service Fabric hosting modelini][hosting-model-link]tanımanızı öneririz. [Service Fabric application model][application-model-link]

> [!NOTE]
> Liveness Probe yalnızca NAT ağ modundaki kapsayıcılar için desteklenir.

## <a name="semantics"></a>Semantiği
Kapsayıcı başına yalnızca 1 Canlılık Sondası belirtebilirsiniz ve bu alanlardaki davranışını denetleyebilirsiniz:

* `initialDelaySeconds`: Konteyner başladıktan sonra sondayı çalıştırmaya başlamak için saniyeler içinde ilk gecikme. Desteklenen değer int. Varsayılan 0'dır. En az 0'dır.

* `timeoutSeconds`: Saniyeler içinde, başarılı bir şekilde tamamlanmamışsa sondayı başarısız olarak görüruz. Desteklenen değer int. Varsayılan 1'dir. Minimum is 1.

* `periodSeconds`: Ne sıklıkta araştırdığımızı belirtmek için saniye cinsinden periyot. Desteklenen değer int. Varsayılan 10'dur. Minimum is 1.

* `failureThreshold`: FailureThreshold tuşuna bastığımızda, konteyner yeniden başlatılacaktır. Desteklenen değer int. Varsayılan 3'dür. Minimum is 1.

* `successThreshold`: Başarısızlıkta, sondanın başarı olarak kabul edilememesi için SuccessThreshold için başarılı bir şekilde yürütülmesi gerekir. Desteklenen değer int. Varsayılan 1'dir. Minimum is 1.

Bir anda konteyner için en fazla 1 sonda olacaktır. Sonda zaman dışarı **Saniye** tamamlanmazsa biz beklemeye devam ve **failureThreshold**doğru sayma . 

Ayrıca, ServiceFabric DeployedServicePackage üzerinde sonda [Sağlık Raporları][health-introduction-link] aşağıdaki yükseltecektir:

* `Ok`: Eğer sonda **başarıiçin** başarılı olursaThreshold o zaman tamam olarak sağlık rapor.

* `Error`: Sonda hatasıCount == **failureThreshold**, kapsayıcıyı yeniden başlatmadan önce Hata bildiririz.

* `Warning`: 
    1. Sonda başarısız olursa ve hataSayısı< **hatasıEşik uyarıyı** bildiririz. Bu sistem durumu raporu, hata Sayısı başarısızlığa ulaşıncaya kadar **kalırEşik** veya **başarıEşik**.
    2. Başarı sonrası başarısızlık, biz hala Uyarı rapor ama güncelleştirilmiş ardışık başarı ile.

## <a name="specifying-liveness-probe"></a>Canlılık Sondası Belirtme

ServiceManifestImport altında ApplicationManifest.xml'de prob belirtebilirsiniz:

Prob ya biri olabilir:

1. HTTP
2. TCP
3. Exec 

## <a name="http-probe"></a>HTTP Sondası

HTTP sondası için Service Fabric, belirtilen bağlantı noktasına ve yola bir HTTP isteği gönderir. 200'den büyük veya eşit ve 400'den küçük kodu döndürme kodu başarıyı gösterir.

Aşağıda HttpGet sondasının nasıl belirtilen bir örneği verilmiştir:

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <CodePackagePolicy CodePackageRef="Code">
        <Probes>
          <Probe Type="Liveness" FailureThreshold="5" SuccessThreshold="2" InitialDelaySeconds="10" PeriodSeconds="30" TimeoutSeconds="20">
            <HttpGet Path="/" Port="8081" Scheme="http">
              <HttpHeader Name="Foo" Value="Val"/>
              <HttpHeader Name="Bar" Value="val1"/>
            </HttpGet>
          </Probe>
        </Probes>
      </CodePackagePolicy>
    </Policies>
  </ServiceManifestImport>
```

HttpGet sondası ayarlayabileceğiniz ek özelliklere sahiptir:

* `path`: HTTP isteğine erişim yolu.

* `port`: Sondalar için erişim noktası. Menzil 1 ile 65535 arasındadır. Zorunlu.

* `scheme`: Kod paketine bağlanmak için kullanılacak şema. HTTPS olarak ayarlanırsa, sertifika doğrulaması atlanır. Varsayılan lar HTTP

* `httpHeader`: Üstbilgi istek te ayarlamak için. Bunlardan birden fazla belirtebilirsiniz.

* `host`: Bağlanmak için IP'yi barındırın.

## <a name="tcp-probe"></a>TCP Sondası

TCP prob için Service Fabric, belirtilen bağlantı noktasına sahip konteynerüzerinde bir soket açmaya çalışır. Eğer bir bağlantı kurabiliyorsa, sonda başarı olarak kabul edilir. Aşağıda, TCP soketi kullanan sondanın nasıl belirtilen bir örneği verilmiştir:

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <CodePackagePolicy CodePackageRef="Code">
        <Probes>
          <Probe Type="Liveness" FailureThreshold="5" SuccessThreshold="2" InitialDelaySeconds="10" PeriodSeconds="30" TimeoutSeconds="20">
            <TcpSocket Port="8081"/>
          </Probe>
        </Probes>
      </CodePackagePolicy>
    </Policies>
  </ServiceManifestImport>
```

## <a name="exec-probe"></a>Exec Sondası

Bu sonda kapsayıcıya bir yönetici verecek ve komutun tamamlanmasını bekleyecek.

> [!NOTE]
> Exec komutu virgülle ayrılmış bir dize alır. Örnekteki aşağıdaki komut Linux kapsayıcısı için çalışacaktır.
> Windows konteyner deniyorsanız, <Command>cmd</Command> kullanın

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <CodePackagePolicy CodePackageRef="Code">
        <Probes>
          <Probe Type="Liveness" FailureThreshold="5" SuccessThreshold="2" InitialDelaySeconds="10" PeriodSeconds="30" TimeoutSeconds="20">
            <Exec>
              <Command>ping,-c,2,localhost</Command>
            </Exec>
          </Probe>        
       </Probes>
      </CodePackagePolicy>
    </Policies>
  </ServiceManifestImport>
```

## <a name="next-steps"></a>Sonraki adımlar
İlgili bilgiler için aşağıdaki makalelere bakın.
* [Servis Kumaş ve konteynerler.][containers-introduction-link]

<!-- Links -->
[containers-introduction-link]: service-fabric-containers-overview.md
[health-introduction-link]: service-fabric-health-introduction.md
[application-model-link]: service-fabric-application-model.md
[hosting-model-link]: service-fabric-hosting-model.md

