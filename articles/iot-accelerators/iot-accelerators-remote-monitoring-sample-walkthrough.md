---
title: Uzaktan İzleme çözüm hızlandırıcıya genel bakış - Azure | Microsoft Dokümanlar
description: Bu makalede, uzaktan izleme çözümünün bazı temel öğelerine genel bir bakış sağlayarak nasıl çalıştığını anlamanızı sağlar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 33005bc286f7dc2c0ebed74bd9df0309e71346f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73824828"
---
# <a name="remote-monitoring-solution-accelerator-overview"></a>Uzaktan İzleme çözümü hızlandırıcısına genel bakış

Uzaktan İzleme [çözüm hızlandırıcısı,](../iot-accelerators/about-iot-accelerators.md) uzak konumlardaki birden fazla makine için uçtan uca izleme çözümü uygular. Bu çözüm, iş senaryosunun genel uygulamasını sağlamak üzere temel Azure hizmetlerini bir araya getirir. Çözümü kendi uygulamanız için bir başlangıç noktası olarak kullanabilir ve özel iş gereksinimlerinizi karşılayacak şekilde [özelleştirebilirsiniz](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md).

Bu makalede, nasıl çalıştığını anlamak için Uzaktan İzleme çözümünün bazı temel öğeleri ile size yol. Bu bilgiler şunları yapmanıza yardımcı olur:

* Çözümdeki sorunları giderme.
* Çözümü kendinize özel gereksinimleri karşılayacak şekilde nasıl özelleştireceğinizi planlama.
* Azure hizmetlerini kullanan kendi IoT çözümünüzü tasarlama.

Uzaktan İzleme çözüm hızlandırıcı kodu GitHub'da mevcuttur:

* [.NET](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

## <a name="logical-architecture"></a>Mantıksal mimari

Aşağıdaki diyagram, [IoT mimarisinde](../iot-fundamentals/iot-introduction.md)yer alan Uzaktan İzleme çözüm hızlandırıcısının mantıksal bileşenlerini özetlemektedir:

![Mantıksal mimari](./media/iot-accelerators-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>Neden mikro hizmetler?

Microsoft'un ilk çözüm hızlandırıcılarını yayınlamasından bu yana bulut mimarisi gelişti. [Mikrohizmetler,](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) geliştirme hızından ödün vermeden ölçek ve esneklik elde etmek için kanıtlanmış bir uygulama olarak ortaya çıkmıştır. Birçok Microsoft hizmeti, bu mimari deseni büyük güvenilirlik ve ölçeklenebilirlik sonuçlarıyla birlikte dahili olarak kullanır. Güncelleştirilmiş çözüm hızlandırıcıları bu öğrenmeleri uygulamaya koyar, böylece siz de bunlardan yararlanabilirsiniz.

> [!TIP]
> Mikro hizmet mimarisi hakkında daha fazla bilgi için [.NET Uygulama Mimarisi](https://www.microsoft.com/net/learn/architecture) ve [Mikro hizmetler: Bulut tarafından desteklenen bir uygulama devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) konusunu inceleyin.

## <a name="device-connectivity"></a>Cihaz bağlantısı

Çözüm, mantıksal mimarinin aygıt bağlantısı bölümünde aşağıdaki bileşenleri içerir:

### <a name="real-devices"></a>Gerçek cihazlar

Gerçek aygıtları çözüme bağlayabilirsiniz. Azure IoT aygıt SDK'larını kullanarak simüle edilmiş aygıtlarınızın davranışını uygulayabilirsiniz.

Çözüm portalındaki panodan gerçek aygıtlar sağlayabilirsiniz.

### <a name="device-simulation-microservice"></a>Cihaz simülasyonu microservice

Çözüm, çözümdeki uçtan uca akışı test etmek için çözüm portalından simüle edilmiş aygıtlardan oluşan bir havuzu yönetmenize olanak tanıyan [cihaz simülasyonu mikro hizmetini](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/device-simulation) içerir. Simüle cihazlar:

* Aygıt-buluttelemetrioluşturun.
* IoT Hub'dan buluttan cihaza yöntem çağrılarına yanıt verin.

Microservice, simülasyonlar oluşturmanız, başlatmanız ve durdurmanız için yeni bir bitiş noktası sağlar. Her simülasyon, telemetri gönderen ve yöntem çağrılarına yanıt veren farklı türde sanal aygıtlardan oluşur.

Çözüm portalındaki panodan simüle edilmiş aygıtlar sağlayabilirsiniz.

### <a name="iot-hub"></a>IoT Hub

[IoT hub'ı](../iot-hub/index.yml) hem gerçek hem de simüle edilmiş aygıtlardan buluta gönderilen telemetriyi yutabilir. IoT hub'ı, telemetriyi ioT çözüm arka ucundaki hizmetlerin işlenmesi için kullanılabilir hale getirir.

IoT hub çözümde aynı zamanda şunları yapar:

* Portala bağlanmasına izin verilen tüm aygıtların kimliklerini ve kimlik doğrulama anahtarlarını depolayan bir kimlik kaydı tutar.
* Çözüm hızlandırıcısı adına cihazlarınızda yöntemler çağırır.
* Tüm kayıtlı cihazlar için cihaz ikizlerini tutar. Cihaz ikizi bir cihaz tarafından bildirilen özellik değerlerini depolar. Cihaz ikizi ayrıca cihaz bir kez daha bağlandığında alabilmesi için çözüm portalında ayarlanmış istenen özellikleri depolar.
* Birden fazla cihaza ait özellikleri ayarlamak veya birden fazla cihaz üzerinde yöntem çağırmak için işleri zamanlar.

## <a name="data-processing-and-analytics"></a>Veri işleme ve analizi

Çözüm, mantıksal mimarinin veri işleme ve analiz kısmında aşağıdaki bileşenleri içerir:

### <a name="iot-hub-manager-microservice"></a>IoT Hub yöneticisi microservice

Çözüm, aşağıdakiler gibi IoT hub'ınızla etkileşimleri işlemek için [IoT Hub yöneticisi microservice'i](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/iothub-manager) içerir:

* IoT aygıtları oluşturma ve yönetme.
* Cihaz ikizleri yönetmek.
* Cihazlarda yöntemleri çağırmak.
* IoT kimlik bilgilerini yönetme.

Bu hizmet, kullanıcı tanımlı gruplara ait aygıtları almak için IoT Hub sorgularını da çalıştırıyor.

Microservice, aygıtları ve aygıt ikizlerini yönetmek, yöntemleri çağırmak ve IoT Hub sorgularını çalıştırmak için yeni bir uç noktası sağlar.

### <a name="device-telemetry-microservice"></a>Cihaz telemetri microservice

[Cihaz telemetri microservice,](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/device-telemetry) Time Series Insights'ta depolanan aygıt telemetrisine okuma erişimi için yeniden bir bitiş noktası sağlar. RESTful bitiş noktası, kural ve depolamadan alarm tanımları için okuma/yazma erişimi crud işlemleri sağlar.

### <a name="storage-adapter-microservice"></a>Depolama adaptörü microservice

[Depolama bağdaştırıcısı microservice,](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/storage-adapter) anahtar değer çiftleri yönetir, depolama hizmeti semantiği özetler ve Azure Cosmos DB kullanarak herhangi bir biçimdeki verileri depolamak için basit bir arabirim sunar.

Değerler koleksiyonlarda düzenlenir. Tek tek değerler üzerinde çalışabilir veya tüm koleksiyonları alabilirsiniz. Karmaşık veri yapıları istemciler tarafından seri hale getirilir ve basit metin yükü olarak yönetilir.

Hizmet, anahtar değeri çiftleri üzerinde CRUD işlemleri için yeniden bir bitiş noktası sağlar. değerler

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Çözüm hızlandırıcı dağıtımları, kuralları, uyarıları, yapılandırma ayarlarını ve diğer tüm soğuk hava depolarını depolamak için [Azure Cosmos DB'yi](https://docs.microsoft.com/azure/cosmos-db/) kullanır.

### <a name="azure-stream-analytics-manager-microservice"></a>Azure Akış Analizi yöneticisi microservice

[Azure Akış Analizi yöneticisi microservice,](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/asa-manager) yapılandırmalarını ayarlama, başlatma ve durdurma ve durumlarını izleme gibi Azure Akış Analizi (ASA) işlerini yönetir.

ASA işi iki başvuru veri kümesi tarafından desteklenir. Bir veri kümesi kuralları, bir veri kümesi aygıt gruplarını tanımlar. Kurallar referans verileri, cihaz telemetri microservice tarafından yönetilen bilgilerden oluşturulur. Azure Akış Analizi yöneticisi microservice, telemetri kurallarını akış işleme mantığına dönüştürür.

Aygıt grupları başvuru verileri, gelen bir telemetri iletisine hangi kural grubunu uygulayacaklarını belirlemek için kullanılır. Aygıt grupları yapılandırma mikro hizmeti tarafından yönetilir ve Azure IoT Hub aygıt çift sorguları kullanır.

ASA işleri, telemetriyi bağlı cihazlardan depolama ve analiz için Time Series Insights'a sunar.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics

[Azure Akış Analizi,](https://docs.microsoft.com/azure/stream-analytics/) aygıtlardan yüksek hacimli veri akışını incelemenize olanak tanıyan bir olay işleme motorudur.

### <a name="azure-time-series-insights"></a>Azure Zaman Serisi Görüşleri

[Azure Time Series Insights,](https://docs.microsoft.com/azure/time-series-insights/) çözüm hızlandırıcısına bağlı aygıtlardan telemetriyi saklar. Ayrıca, çözüm web UI'de aygıt telemetrisini görselleştirme ve sorgulama olanağı sağlar.

> [!NOTE]
> Zaman Serisi Öngörüleri şu anda Azure Çin bulutunda kullanılamıyor. Azure Çin bulutundaki yeni Uzaktan İzleme çözüm hızlandırıcı dağıtımları, tüm depolama için Cosmos DB'yi kullanır.

### <a name="configuration-microservice"></a>Yapılandırma mikrohizmeti

[Yapılandırma microservice,](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/config) çözüm hızlandırıcısındaki aygıt gruplarında, çözüm ayarlarında ve kullanıcı ayarlarındaki CRUD işlemleri için yeniden bir uç nokta sağlar. Yapılandırma verilerini sürdürmek için depolama bağdaştırıcısı microservice ile çalışır.

### <a name="authentication-and-authorization-microservice"></a>Kimlik doğrulama ve yetkilendirme microservice

Kimlik [doğrulama ve yetkilendirme mikrohizmeti,](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/auth) çözüm hızlandırıcısına erişmeye yetkili kullanıcıları yönetir. Kullanıcı yönetimi [OpenId Connect](https://openid.net/connect/)destekleyen herhangi bir kimlik hizmeti sağlayıcısı kullanılarak yapılabilir.

### <a name="azure-active-directory"></a>Azure Active Directory

Çözüm hızlandırıcı dağıtımları, OpenID Connect sağlayıcısı olarak [Azure Active Directory'yi](https://docs.microsoft.com/azure/active-directory/) kullanır. Azure Active Directory, kullanıcı bilgilerini depolar ve JWT belirteç imzalarını doğrulamak için sertifikalar sağlar.

## <a name="presentation"></a>Sunum

Çözüm, mantıksal mimarinin sunu bölümünde aşağıdaki bileşenleri içerir:

[Web kullanıcı arayüzü bir React Javascript uygulamasıdır.](https://github.com/Azure/pcs-remote-monitoring-webui) Uygulama:

* Yalnızca Javascript React kullanır ve tamamen tarayıcıda çalışır.
* CSS ile tarz.
* AJAX çağrıları aracılığıyla kamu yaylası mikro hizmetlerle etkileşime geçer.

Kullanıcı arabirimi tüm çözüm hızlandırıcı işlevselliğini sunar ve aşağıdakiler gibi diğer mikro hizmetlerle etkileşime girer:

* Kullanıcı verilerini korumak için kimlik doğrulama ve yetkilendirme microservice.
* IoT aygıtlarını listelemek ve yönetmek için IoT Hub yöneticisi microservice.

Kullanıcı arabirimi, aygıt telemetrisinin sorgulanmasını ve analizini etkinleştirmek için Azure Time Series Insights gezginini tümleştirir.

Yapılandırma microservice kullanıcı arabiriminin yapılandırma ayarlarını depolamasını ve almasına olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak kodu ve geliştirici belgelerini keşfetmek istiyorsanız, iki GitHub deposundan biriyle başlayın:

* [Azure IoT (.NET) ile Uzaktan İzleme için çözüm hızlandırıcısı.](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Azure IoT (Java) ile Uzaktan İzleme için çözüm hızlandırıcısı.](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

Ayrıntılı çözüm mimarisi diyagramları:
* [Uzaktan İzleme mimarisi için çözüm hızlandırıcı.](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture)

Uzaktan İzleme çözüm hızlandırıcısı hakkında daha fazla kavramsal bilgi için [bkz.](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md)
