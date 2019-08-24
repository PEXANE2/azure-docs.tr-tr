---
title: OPC kasa mimarisi-Azure | Microsoft Docs
description: OPC Kasası sertifika yönetimi hizmeti mimarisi
author: mregen
ms.author: mregen
ms.date: 08/16/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 9331473402ddd22180df3b404824969360d48164
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69995826"
---
# <a name="opc-vault-architecture"></a>OPC Kasası mimarisi

Bu makale **OPC Kasası mikro hizmeti** ve **opc Kasası IoT Edge modülüyle**ilgili bir genel bakış sunar.

## <a name="overview"></a>Genel Bakış

OPC UA uygulamaları uygulama düzeyi güvenliği sağlamak için uygulama örneği sertifikaları kullanır. Ortak ve özel anahtar çiftini sağlayan, asimetrik şifreleme kullanılarak güvenli bir bağlantı oluşturulur. Sertifikalar kendinden imzalı olabilir veya bir sertifika yetkilisi (CA) tarafından imzalanabilir.

OPC UA uygulaması, güvendiği uygulamaları temsil eden güvenilir sertifikaların bir listesini içerir. Bu sertifikalar otomatik olarak imzalanabilir, bir CA tarafından imzalanabilir veya bir kök CA ya da bir alt CA olabilir. Güvenilir bir sertifika daha büyük bir Sertifika zincirinin parçasıysa, tam sertifika zinciri doğrulanamadığından, uygulama güven listesindeki sertifikaya zincirleme yapan tüm sertifikalara güvenir.

Otomatik olarak imzalanan sertifikalara güvenen ve CA sertifikasına güvenen önemli fark, güven dağıtmak ve korumak için gereken yükleme çabasıdır ve şirkete özgü bir CA barındırmak için ek çaba sağlar. 

Tek bir istemci uygulaması ile n sunucuları için otomatik olarak imzalanan sertifikalara güven dağıtmak için, tüm n Server uygulama sertifikalarının Istemci uygulama güven listesine yüklenmesi gerekir ve Istemci uygulama sertifikası tüm kullanıcılara yüklenmelidir Sunucu uygulaması güven listeleri. Bu yönetim çabasıyla oldukça bir yük vardır ve sertifika yaşam sürelerinin göz önünde bulundurulması gerektiğinde ve sertifikaların yenilenmesi durumunda daha da fazla yer vardır.

Şirkete özgü CA kullanımı, birden çok sunucu ve Istemcilerle güvenin yönetimini büyük ölçüde basitleştirebilir. Bu durumda, yönetici, kullanılan her Istemci ve sunucu için bir kez CA imzalı uygulama örneği sertifikası oluşturur. Ayrıca, CA sertifikası tüm sunucu ve istemcilerde her uygulama güven listesine yüklenir. Bu yaklaşım ile, yalnızca süre dolmuþ sertifikaların, etkilenen uygulamalar için yenilenmesi ve değiştirilmeleri gerekir.

Azure endüstriyel IoT OPC UA sertifika yönetimi hizmeti, OPC Kasası mikro hizmetini temel alan OPC UA uygulamalarına yönelik şirkete özgü bir CA 'yı yönetme çözümüdür.

OPC Kasası, güvenilir bir bulutta şirkete özgü bir CA 'yı barındırmak için bir mikro hizmet sağlar ve donanım güvenlik modülleri, Cosmos DB ve isteğe bağlı olarak da uygulama deposu olarak IoT Hub Azure Key Vault.

OPC Kasası mikro hizmeti, kişisel olarak imzalanan uygulama sertifikalarının ve güvenlik yöneticilerinin ve onaylayanların bu istekleri onaylama veya reddetme Azure Key Vault oturum açma haklarına sahip olduğu rol tabanlı iş akışını destekleyecek şekilde tasarlanmıştır.

Mevcut OPC UA GDS tabanlı OT çözümleriyle uyumluluk için hizmetler, OPC *UA küresel bulma sunucusunu ve dağıtım Için sertifika yönetimi* arabirimini uygulayan bir OPC Kasası mikro hizmeti destekli Edge modülü desteği içerir. , belirtiminin 12. bölümüne göre sertifikalar ve güven listeleri. Ancak, bizim bilginiz itibariyle bu GDS sunucu arabirimi henüz yaygın olarak kullanılmaz ve henüz sınırlı işlevlere (okuyucu rolü) sahiptir. [İsteğe bağlı olarak, müşteri isteği (*) deneyimini geliştireceğiz](#yet-unsupported-features).

## <a name="architecture"></a>Mimari

Mimari, fabrika ağı için OPC Kasası IoT Edge modülü ve iş akışını denetlemek için bir Web örneği UX ile OPC Kasası mikro hizmetine dayanır:

![Opckasa mimarisi](media/overview-opc-vault-architecture/opc-vault.png)

## <a name="opc-vault-microservice"></a>OPC Kasası mikro hizmeti

OPC Kasası mikro hizmeti, OPC UA uygulamalarına yönelik şirkete özgü bir CA 'yı dağıtmak ve yönetmek üzere iş akışını uygulamak için aşağıdaki arabirimlerden oluşur:

### <a name="application"></a>Uygulama 
- "OPC UA uygulaması" bir sunucu veya istemci ya da her ikisi olabilir. OPC Kasası, bu durumda bir uygulama kayıt yetkilisi olarak hizmet verir. 
- Uygulamaları kaydetmek, güncelleştirmek ve silmek için temel işlemlerin yanında, arama ifadelerine sahip uygulamaları bulmak ve sorgulamak için de arabirimler vardır. 
- Sertifika isteklerinin, bir isteği işlemek ve OPC UA 'ya özgü uzantılara sahip imzalı bir sertifika vermek için geçerli bir uygulamaya başvurması gerekir. 
- Uygulama Hizmeti, müşteri yapılandırmasına bağlı olarak CosmosDB veritabanı ya da [OpcTwin cihaz kayıt defteri (*)](#yet-unsupported-features)tarafından desteklenir.

### <a name="certificate-group"></a>Sertifika grubu
- Sertifika grubu, sertifikaları imzalamak için özel anahtar dahil bir kök CA veya alt CA sertifikası depolayan bir varlıktır. 
- RSA anahtar uzunluğu, SHA-2 karma uzunluğu ve yaşam süreleri hem veren CA hem de imzalı uygulama sertifikaları için yapılandırılabilir. 
- Https, Kullanıcı veya ECC algoritması sertifika grupları [(*)](#yet-unsupported-features)ile gelecek uzantılara izin vermek için birden çok grup tek bir hizmette barındırılabilir. 
- CA sertifikaları FIPS 140-2 düzey 2 donanım güvenlik modülleri (HSM) ile desteklenen Azure Key Vault depolanır. İmzalama bir AzureAD güvenli Key Vault işlemi tarafından yapıldığından, özel anahtar güvenli depolamayı hiçbir şekilde terk etmez. 
- CA sertifikaları zaman içinde yenilenebilir ve Key Vault geçmişi nedeniyle hala güvenli depolamada kalmaya devam edebilir. 
- Her CA sertifikası için iptal listesi, gizli olarak Key Vault da depolanır. Bir uygulamanın kaydı kaldırıldıktan sonra, bir yönetici tarafından CRL 'de uygulama sertifikası da iptal edilir.
- Toplu ve tek sertifika iptali desteklenir.

### <a name="certificate-request"></a>Sertifika isteği
Bir sertifika isteği, OPC UA uygulaması için "sertifika Imzalama Isteği" (CSR) kullanarak yeni bir anahtar çifti veya imzalı bir sertifika oluşturmak için iş akışını uygular. 
- İstek, konu veya CSR gibi bilgileri içeren bir veritabanında ve OPC UA uygulamasına bir başvuru olarak depolanır. 
- Hizmette iş mantığı, isteği uygulama veritabanında depolanan bilgilere karşı doğrular. Örneğin, veritabanındaki uygulama URI 'Si CSR içindeki uygulama URI 'Siyle eşleşmelidir.
- İmzalama hakları (onaylayan rolü) olan bir güvenlik yöneticisi isteği onaylar veya reddeder. İstek onaylanırsa, yeni bir anahtar çifti ve/veya imzalı bir sertifika oluşturulur. Yeni özel anahtar, Anahtar Kasası 'nda güvenli bir şekilde depolanır ve yeni imzalanmış ortak sertifika, sertifika Isteği veritabanında depolanır.
- İstek sahibi onaylanana veya iptal edilene kadar istek durumunu yoklayabilirler. İstek onaylanmışsa, özel anahtar ve sertifika, OPC UA uygulamasının sertifika deposuna indirilip yüklenebilir.
- İstek sahibi artık istek veritabanından gereksiz bilgileri silme isteğini kabul edebilir. 

İmzalı bir sertifikanın kullanım ömrü boyunca bir uygulama silinebilir veya bir anahtar tehlikeye girebilir. Böyle bir durumda, CA yöneticisi şunları yapabilir:
- Aynı zamanda uygulamanın bekleyen ve onaylanmış tüm sertifika isteklerini de silen bir uygulamayı silin. 
- Yalnızca bir anahtarın yenilenmesi veya tehlikeye düşmesi durumunda yalnızca tek bir sertifika isteğini silmek başka bir seçenektir.
- Artık tehlikede onaylanan ve kabul edilen sertifika istekleri silindi olarak işaretlenir.
- Bir yönetici, sertifikayı veren CA CRL 'sini düzenli olarak yürütebilir. Yenileme sırasında, tüm silinen sertifika Istekleri iptal edilir ve sertifika seri numaraları CRL iptal listesine eklenir. İptal edilen sertifika istekleri iptal edildi olarak işaretlenir.
- Acil olaylarda, tek sertifika istekleri de iptal edilebilir.
- Son olarak, güncelleştirilmiş CRL 'Ler katılan OPC UA istemcilerine ve sunucularına dağıtım için kullanılabilir.

OPC Kasası mikro hizmet API 'SI hakkında daha fazla bilgi için bkz. mikro hizmetin Swagger belgeleri.

## <a name="opc-vault-iot-edge-module"></a>OPC Kasası IoT Edge modülü
Bir fabrika ağı küresel bulma sunucusunu desteklemek için OPC Kasası modülü kenarda dağıtılabilir, yerel bir .Net Core uygulaması olarak yürütülür veya bir Docker kapsayıcısında başlatılabilir. Geçerli OPC UA .net standart yığınında auth2 kimlik doğrulama desteğinin bulunmaması nedeniyle OPC Kasası Edge modülünün işlevselliği bir okuyucu rolüyle sınırlıdır, çünkü bir Kullanıcı, OPC UA GDS standardı kullanılarak Edge modülünden mikro hizmete kimliğe bürünme yapılamaz  arayüz. Bu noktada[(*)](#yet-unsupported-features)yazıcı, yönetici veya onaylayan rolüne gerek olmayan işlemlere izin verilir. 

## <a name="yet-unsupported-features"></a>Henüz desteklenmeyen özellikler

**(*)** henüz desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

OPC Kasası mimarisini öğrendiğinize göre, aşağıda önerilen sonraki adım verilmiştir:

> [!div class="nextstepaction"]
> [OPC Kasası oluşturma ve dağıtma](howto-opc-vault-deploy.md)
