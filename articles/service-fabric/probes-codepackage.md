---
title: Azure Service Fabric araştırmaları
description: Uygulama ve hizmet bildirim dosyalarını kullanarak Azure Service Fabric 'da bir araştırmanın nasıl modelleyini.
ms.topic: conceptual
author: tugup
ms.author: tugup
ms.date: 3/12/2020
ms.openlocfilehash: 07a1b836ca7ea79244e303f54654dfcaa6e5fcb9
ms.sourcegitcommit: 1ed0230c48656d0e5c72a502bfb4f53b8a774ef1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82137595"
---
# <a name="liveness-probe"></a>Lizleştirme araştırması
Azure Service Fabric, sürüm 7,1 ' den başlayarak [Kapsayıcılı][containers-introduction-link] uygulamalar için bir araştırma mekanizmasını destekler. Bir müdahale araştırması, kapsayıcılı bir uygulamanın, hızla yanıt vermezse yeniden başlatılacak şekilde raporlanmasına yardımcı olur.
Bu makalede, bildirim dosyalarını kullanarak bir lizleştirme araştırması tanımlama hakkında genel bakış sunulmaktadır.

Bu makaleye devam etmeden önce [Service Fabric uygulama modeliyle][application-model-link] ve [Service Fabric barındırma modeliyle][hosting-model-link]tanıdık olun.

> [!NOTE]
> Lizleştirme araştırması yalnızca NAT ağ modundaki kapsayıcılar için desteklenir.

## <a name="semantics"></a>İçeriyor
Kapsayıcı başına yalnızca bir lizleştirme araştırması belirtebilir ve bu alanları kullanarak davranışını kontrol edebilirsiniz:

* `initialDelaySeconds`: Kapsayıcı başladıktan sonra araştırmayı yürütmeye başlamak için saniye cinsinden ilk gecikme. Desteklenen değer **int**'tir. Varsayılan değer 0 ' dır ve minimum 0 ' dır.

* `timeoutSeconds`: Başarılı bir şekilde bitmemişse, araştırmanın başarısız olarak kabul ettiğimiz saniye cinsinden süre. Desteklenen değer **int**'tir. Varsayılan değer 1 ' dir ve en az 1 ' dir.

* `periodSeconds`: Yoklamanın sıklığını belirtmek için saniye cinsinden süre. Desteklenen değer **int**'tir. Varsayılan değer 10 ' dur ve en az 1 ' dir.

* `failureThreshold`: Bu değere ulaşıyoruz, kapsayıcı yeniden başlatılır. Desteklenen değer **int**'tir. Varsayılan değer 3 ' dir ve en az 1 ' dir.

* `successThreshold`: Hatada, araştırmanın başarılı olarak kabul edilmesi için, bu değer için başarıyla çalıştırılması gerekir. Desteklenen değer **int**'tir. Varsayılan değer 1 ' dir ve en az 1 ' dir.

Herhangi bir anda bir kapsayıcıda en çok bir yoklama olabilir. Yoklama, **TimeoutSeconds**'da ayarlanan sürede bitmiyorsa, bekleyin ve **FailureThreshold**değerine doğru süreyi Sayın. 

Ayrıca, Service Fabric **Deployedservicepackage**üzerinde aşağıdaki araştırma [sistem durumu raporlarını][health-introduction-link] yükseltir:

* `OK`: Yoklama **, başarılı bir şekilde ayarlanan**değer için başarılı oldu.

* `Error`: Kapsayıcı yeniden başlatılmadan önce araştırma **FailureCount** ==  **FailureThreshold**.

* `Warning`: 
    * Araştırma başarısız olur ve **FailureCount** < **FailureThreshold**. Bu sistem durumu raporu, **FailureCount** , **FailureThreshold** veya **başarılı eşikte**ayarlanan değere ulaşıncaya kadar kalır.
    * Hatadan sonra başarılı olduğunda, uyarı, art arda güncelleştirilmiş başarıları ile kalır.

## <a name="specifying-a-liveness-probe"></a>Bir lizliği araştırması belirtme

**Servicemanifestımport**altındaki ApplicationManifest. xml dosyasında bir araştırma belirtebilirsiniz.

Araştırma, aşağıdakilerden herhangi biri olabilir:

* HTTP
* TCP
* Exec 

### <a name="http-probe"></a>HTTP araştırma

Bir HTTP araştırması için Service Fabric, belirttiğiniz bağlantı noktasına ve yola bir HTTP isteği gönderir. 200 ' den büyük veya buna eşit ve 400 ' den küçük olan bir dönüş kodu, başarıyı gösterir.

HTTP araştırması belirtme hakkında bir örnek aşağıda verilmiştir:

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

HTTP araştırmasına ayarlayabileceğiniz ek özellikler vardır:

* `path`: HTTP isteğinde kullanılacak yol.

* `port`: Yoklamalar için kullanılacak bağlantı noktası. Bu özellik zorunludur. Aralık 1 ile 65535 arasındadır.

* `scheme`: Kod paketine bağlanmak için kullanılacak düzen. Bu özellik HTTPS olarak ayarlandıysa, sertifika doğrulaması atlanır. Varsayılan ayar HTTP ' dir.

* `httpHeader`: İstekte ayarlanacak üst bilgiler. Birden çok üst bilgi belirleyebilirsiniz.

* `host`: Bağlanılacak ana bilgisayar IP adresi.

### <a name="tcp-probe"></a>TCP araştırma

TCP araştırması için Service Fabric, belirtilen bağlantı noktasını kullanarak kapsayıcıda bir yuva açmaya çalışacaktır. Bir bağlantı kurabildiğinden, araştırma başarılı olarak kabul edilir. TCP yuvası kullanan bir araştırmanın nasıl belirtilbir örneği aşağıda verilmiştir:

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

### <a name="exec-probe"></a>Yürütme araştırması

Bu araştırma, kapsayıcıda bir **Exec** komutu oluşturacak ve komutun bitmesini bekleyecek.

> [!NOTE]
> **Exec** komutu, virgülle ayrılmış bir dize alır. Aşağıdaki örnekteki komut bir Linux kapsayıcısı için çalışacaktır.
> Bir Windows kapsayıcısını araştırmayı deniyorsanız, **cmd**kullanın.

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
İlgili bilgiler için aşağıdaki makaleye bakın:
* [Service Fabric ve kapsayıcılar][containers-introduction-link]

<!-- Links -->
[containers-introduction-link]: service-fabric-containers-overview.md
[health-introduction-link]: service-fabric-health-introduction.md
[application-model-link]: service-fabric-application-model.md
[hosting-model-link]: service-fabric-hosting-model.md

