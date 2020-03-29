---
title: include dosyası
description: include dosyası
services: iot-fundamentals
author: robinsh
ms.service: iot-fundamentals
ms.topic: include
ms.date: 08/07/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 08cca67455df4b2d28bba0a7410fccc11446fcdc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76748876"
---
Bu makalede, Azure IoT tabanlı Nesnelerin İnterneti (IoT) altyapısını güvence altına almak için bir sonraki ayrıntı düzeyi sağlanmaktadır. Her bileşeni yapılandırmak ve dağıtmak için uygulama düzeyi ayrıntılarına bağlanır. Ayrıca, çeşitli rakip yöntemler arasında karşılaştırmalar ve seçenekler sağlar.

Azure IoT dağıtımının güvenliğinin sağlanması aşağıdaki üç güvenlik alanına ayrılabilir:

* **Cihaz Güvenliği**: IoT aygıtının vahşi doğada konuşlandırılırken güvenliğini sağlamak.

* **Bağlantı Güvenliği**: IoT cihazı ile IoT Hub arasında iletilen tüm verilerin gizli ve kurcalamaya karşı dayanıklı olmasını sağlamak.

* **Bulut Güvenliği**: Veri aktarımı sırasında güvenli bir araç sağlamak ve bulutta depolanır.

![Üç güvenlik alanı](./media/iot-secure-your-deployment/overview.png)

## <a name="secure-device-provisioning-and-authentication"></a>Güvenli aygıt sağlama ve kimlik doğrulama

IoT çözüm hızlandırıcıları aşağıdaki iki yöntemi kullanarak IoT aygıtlarını güvenli hale sağlar:

* IoT Hub ile iletişim kurmak için aygıt tarafından kullanılabilecek her aygıt için benzersiz bir kimlik anahtarı (güvenlik belirteçleri) sağlayarak.

* Aygıtın kimliğini IoT Hub'ına doğrulamak için cihaz üzerinde [X.509 sertifikası](https://www.itu.int/rec/T-REC-X.509-201210-S) ve özel anahtar kullanarak. Bu kimlik doğrulama yöntemi, aygıttaki özel anahtarın aygıtın dışında herhangi bir zamanda bilinmemesini sağlayarak daha yüksek bir güvenlik düzeyi sağlar.

Güvenlik belirteç yöntemi, her çağrıiçin simetrik anahtarı ilişkilendirerek aygıt tarafından IoT Hub'a yapılan her arama için kimlik doğrulaması sağlar. X.509 tabanlı kimlik doğrulama, TLS bağlantı kuruluşunun bir parçası olarak fiziksel katmanda bir IoT aygıtının kimlik doğrulamasını sağlar. Güvenlik belirteç tabanlı yöntem, daha az güvenli bir desen olan X.509 kimlik doğrulaması olmadan kullanılabilir. İki yöntem arasındaki seçim, öncelikle aygıt kimlik doğrulamasının ne kadar güvenli olması gerektiği ve aygıtta güvenli depolama nın kullanılabilirliği (özel anahtarı güvenli bir şekilde depolamak için) tarafından belirlenir.

## <a name="iot-hub-security-tokens"></a>IoT Hub güvenlik belirteçleri

IoT Hub, ağa anahtar göndermemek için aygıtların ve hizmetlerin kimliğini doğrulamak için güvenlik belirteçleri kullanır. Ayrıca, güvenlik belirteçleri zaman geçerliliği ve kapsamı sınırlıdır. Azure IoT SDK'lar, özel yapılandırma gerektirmeden otomatik olarak belirteçler oluşturur. Ancak bazı senaryolar, kullanıcının doğrudan güvenlik belirteçleri oluşturmasını ve kullanmasını gerektirir. Bu senaryolar, MQTT, AMQP veya HTTP yüzeylerinin doğrudan kullanımını veya belirteç hizmet deseninin uygulanmasını içerir.

Güvenlik belirteci yapısı ve kullanımı hakkında daha fazla bilgi aşağıdaki makalelerde bulunabilir:

* [Güvenlik belirteç yapısı](../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure)

* [SAS belirteçlerini aygıt olarak kullanma](../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app)

Her IoT Hub'ı, hizmette aygıt başına kaynakları oluşturmak için kullanılabilen (örneğin, uçuş içi buluttan cihaza iletiler içeren bir sıra) ve aygıta bakan uç noktalara erişime izin vermek için kullanılabilecek bir [kimlik kayıt defterine](../articles/iot-hub/iot-hub-devguide-identity-registry.md) sahiptir. IoT Hub kimlik kayıt defteri, bir çözüm için aygıt kimliklerinin ve güvenlik anahtarlarının güvenli bir şekilde depolanmasını sağlar. Tek tek veya aygıt kimlik grupları izin listesine veya aygıt erişimi üzerinde tam denetim sağlayan bir engelleme listesine eklenebilir. Aşağıdaki makaleler, kimlik kayıt defterinin yapısı ve desteklenen işlemler hakkında daha fazla ayrıntı sağlar.

[IoT Hub, MQTT, AMQP ve HTTP gibi protokolleri destekler.](../articles//iot-hub/iot-hub-devguide-security.md) Bu protokollerin her biri, IoT aygıtından IoT Hub'ına kadar güvenlik belirteçlerini farklı şekilde kullanır:

* AMQP: SASL PLAIN ve AMQP`{policyName}@sas.root.{iothubName}` Talep tabanlı güvenlik (IoT hub düzeyinde belirteçleri ile; `{deviceId}` cihaz kapsamlı belirteçleri ile).

* MQTT: CONNECT `{deviceId}` `{ClientId}`paketi, `{IoThubhostname}/{deviceId}` Kullanıcı **adı** alanında ve **Parola** alanında bir SAS belirteci olarak kullanır.

* HTTP: Geçerli belirteç yetkilendirme isteği üstbilgisinde yer alan bir belirteçtir.

IoT Hub kimlik kayıt defteri, aygıt başına güvenlik kimlik bilgilerini ve erişim denetimini yapılandırmak için kullanılabilir. Ancak, bir IoT çözümü zaten özel bir [aygıt kimlik kayıt defteri ve / veya kimlik doğrulama düzeni](../articles/iot-hub/iot-hub-devguide-security.md#custom-device-and-module-authentication)önemli bir yatırım varsa, bir belirteç hizmeti oluşturarak IoT Hub ile varolan bir altyapıya entegre edilebilir.

### <a name="x509-certificate-based-device-authentication"></a>X.509 sertifika tabanlı aygıt kimlik doğrulaması

Aygıt tabanlı [X.509 sertifikası](../articles/iot-hub/iot-hub-devguide-security.md) ve ilişkili özel ve ortak anahtar çiftinin kullanımı, fiziksel katmanda ek kimlik doğrulamasına olanak tanır. Özel anahtar aygıtta güvenli bir şekilde saklanır ve cihazın dışında bulunamaz. X.509 sertifikası, aygıt kimliği ve diğer kuruluş ayrıntıları gibi aygıt hakkında bilgiler içerir. Sertifikanın imzası özel anahtar kullanılarak oluşturulur.

Üst düzey cihaz sağlama akışı:

* Aygıt imalatı veya devreye alınması sırasında aygıtla ilişkili aygıt kimliği ve/veya X.509 sertifikası olan fiziksel bir aygıtla ilişkilendirin.

* IoT Hub'da karşılık gelen bir kimlik girişi oluşturun – aygıt kimliği ve ioT Hub kimlik kayıt defterinde ilişkili aygıt bilgileri.

* X.509 sertifika parmak izini IoT Hub kimlik kayıt defterinde güvenle saklayın.

### <a name="root-certificate-on-device"></a>Aygıtta kök sertifikası

IoT Hub ile güvenli bir TLS bağlantısı kurarken, IoT aygıtı SDK aygıtının bir parçası olan bir kök sertifikası kullanarak IoT Hub'ı doğrular. C istemcisi SDK için sertifika, repo'nun kökü altında "c\\\\certs" klasörünün altında yer alır. Bu kök sertifikalar uzun ömürlü olsa da, yine de süresi dolabilir veya iptal edilebilir. Sertifikayı aygıtta güncelleştirmenin bir yolu yoksa, aygıt daha sonra IoT Hub'ına (veya başka bir bulut hizmetine) bağlanamayabilir. IoT aygıtı dağıtıldıktan sonra kök sertifikasını güncelleştirmek için bir araç olması bu riski etkin bir şekilde azaltır.

## <a name="securing-the-connection"></a>Bağlantıyı sağlama

IoT aygıtı ile IoT Hub arasındaki Internet bağlantısı, Taşıma Katmanı Güvenliği (TLS) standardı kullanılarak güvenli hale getirilir. Azure IoT, [TLS 1.2](https://tools.ietf.org/html/rfc5246), TLS 1.1 ve TLS 1.0'ı bu sırada destekler. TLS 1.0 desteği yalnızca geriye dönük uyumluluk için sağlanmaktadır. En fazla güvenliği sağladığı için hub'ınızı TLS 1.2'yi kullanacak şekilde nasıl yapılandırılabildiğinizi görmek için [IoT Hub'daki TLS desteğini](../articles/iot-hub/iot-hub-tls-support.md) kontrol edin.

## <a name="securing-the-cloud"></a>Bulutu güvence altına alma

Azure IoT Hub, her güvenlik anahtarı için [erişim denetimi ilkelerinin](../articles/iot-hub/iot-hub-devguide-security.md) tanımlanmasına izin verir. IoT Hub'ın uç noktalarının her birine erişim sağlamak için aşağıdaki izinkümesini kullanır. İzinler, IoT Hub erişimini işleve göre sınırlar.

* **Kayıt DefteriRead**. Verilen ler kimlik kaydına erişim okunur. Daha fazla bilgi için [kimlik kaydına](../articles/iot-hub/iot-hub-devguide-identity-registry.md)bakın.

* **Kayıt DefteriReadWrite**. Kimlik kaydına erişim izni verir. Daha fazla bilgi için [kimlik kaydına](../articles/iot-hub/iot-hub-devguide-identity-registry.md)bakın.

* **ServiceConnect**. Bulut hizmetine bakan iletişim ve izleme uç noktalarına erişim sağlar. Örneğin, arkadan bulut hizmetlerine aygıttan buluta iletiler alma, buluttan aygıta iletiler gönderme ve ilgili teslim bildirimlerini alma izni verir.

* **DeviceConnect**. Aygıta bakan uç noktalara erişim sağlar. Örneğin, aygıttan buluta iletiler gönderme ve buluttan aygıta iletiler alma izni verir. Bu izin aygıtlar tarafından kullanılır.

[Güvenlik belirteçleri](../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app)ile IoT Hub ile **DeviceConnect** izinleri almanın iki yolu vardır: aygıt kimlik anahtarı veya paylaşılan erişim anahtarı nı kullanarak. Ayrıca, cihazlardan erişilebilen tüm işlevlerin önek ile uç noktalarda `/devices/{deviceId}`tasarım tarafından ortaya çıkarılabildiğine dikkat etmek önemlidir.

Hizmet bileşenleri yalnızca uygun izinleri veren paylaşılan erişim ilkelerini kullanarak [güvenlik belirteçleri oluşturabilir.](../articles/iot-hub/iot-hub-devguide-security.md#use-security-tokens-from-service-components)

Azure IoT Hub ve çözümün bir parçası olabilecek diğer hizmetler, Azure Etkin Dizini kullanan kullanıcıların yönetimine olanak tanır.

Azure IoT Hub tarafından alınan veriler, Azure Akış Analizi ve Azure blob depolama gibi çeşitli hizmetler tarafından tüketilebilir. Bu hizmetler yönetim erişimine izin verir. Bu hizmetler ve kullanılabilir seçenekler hakkında daha fazla bilgi edinin:

* [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/): Öznitelikler, yapılandırma ve güvenlik özellikleri gibi sağladığınız aygıtlar için meta verileri yöneten yarı yapılandırılmış veriler için ölçeklenebilir, tam dizine sahip veritabanı hizmeti. Azure Cosmos DB, yüksek performanslı ve yüksek iş işlemcisi işleme, verilerin şema-agnostik dizini ve zengin bir SQL sorgu arabirimi sunar.

* [Azure Akış Analizi](https://azure.microsoft.com/services/stream-analytics/): Cihazlardan, sensörlerden, altyapıdan ve uygulamalardan gerçek zamanlı öngörüler ortaya çıkarmak için düşük maliyetli bir analitik çözüm geliştirmenizi ve dağıtmanızı sağlayan bulutta gerçek zamanlı akış işleme. Bu tam olarak yönetilen hizmetten elde edilen veriler, yüksek iş hacmi, düşük gecikme gecikmesi ve esneklik elde ederken herhangi bir hacme ölçeklenebilir.

* [Azure Uygulama Hizmetleri](https://azure.microsoft.com/services/app-service/): Her yerde verilere bağlanan güçlü web ve mobil uygulamalar oluşturmak için bir bulut platformu; bulutta veya şirket içinde. iOS, Android ve Windows için ilgi çekici mobil uygulamalar oluşturun. Hizmet Olarak Yazılımınızla (SaaS) ve kurumsal uygulamalarınızla, düzinelerce bulut tabanlı hizmet ve kurumsal uygulamayla güncel olmayan bağlantıyla tümleştirin. Web uygulamaları ve API'leri her zamankinden daha hızlı oluşturmak için en sevdiğiniz dilde ve IDE'de (.NET, Node.js, PHP, Python veya Java) kodlayın.

* [Logic Apps](https://azure.microsoft.com/services/app-service/logic/): Azure Uygulama Hizmetinin Mantıksal Uygulamalar özelliği, IoT çözümünüzü mevcut iş hattı sistemlerinize entegre etmeye ve iş akışı süreçlerini otomatikleştirmenize yardımcı olur. Logic Apps, geliştiricilerin tetikleyiciden başlayan iş akışlarını tasarlamasına ve ardından iş süreçlerinizle tümleştirmek için güçlü bağlayıcılar kullanan bir dizi adım-kural ve eylem yürütmesine olanak tanır. Logic Apps, SaaS, bulut tabanlı ve şirket içi uygulamalardan oluşan geniş bir ekosisteme güncel bağlantılar sunar.

* [Azure Blob depolama](https://azure.microsoft.com/services/storage/): Aygıtlarınızın buluta gönderdiği veriler için güvenilir ve ekonomik bulut depolama.

## <a name="conclusion"></a>Sonuç

Bu makalede, Azure IoT kullanarak bir IoT altyapısı tasarlamak ve dağıtmak için uygulama düzeyi ayrıntılarına genel bakış sağlanmaktadır. Her bileşenin güvenli olması için yapılandırılmak, genel IoT altyapısını güvence altına almak için önemlidir. Azure IoT'de kullanılabilen tasarım seçenekleri, bazı esneklik ve seçenek düzeyi sağlar; ancak, her seçimgüvenlik etkileri olabilir. Bu seçeneklerin her birinin bir risk/maliyet değerlendirmesi ile değerlendirilmesi önerilir.
