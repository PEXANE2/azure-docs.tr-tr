---
title: OPC Vault mimarisi - Azure | Microsoft Dokümanlar
description: OPC Vault sertifika yönetimi hizmet mimarisi
author: mregen
ms.author: mregen
ms.date: 08/16/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 1e08968034134e2b9ab3b8064387d18663d5c866
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "71200152"
---
# <a name="opc-vault-architecture"></a>OPC Vault mimarisi

Bu makalede, OPC Vault microservice ve OPC Vault IoT Edge modülü hakkında genel bir bakış sunulmaktadır.

OPC UA uygulamaları, uygulama düzeyi güvenliği sağlamak için uygulama örnek sertifikalarını kullanır. Uygulama sertifikalarının ortak ve özel anahtar çiftini sağladığı asimetrik şifreleme kullanılarak güvenli bir bağlantı kurulur. Sertifikalar kendi imzası olabilir veya Sertifika Yetkilisi (CA) tarafından imzalanabilir.

OPC UA uygulaması, güvendiği uygulamaları temsil eden güvenilir sertifikaların bir listesine sahiptir. Bu sertifikalar kendi kendine imzalanabilir veya bir CA tarafından imzalanabilir veya Root-CA veya Sub-CA'nın kendileri olabilir. Güvenilen bir sertifika daha büyük bir sertifika zincirinin parçasıysa, uygulama güven listesindeki sertifikaya zincirleyen tüm sertifikalara güvenir. Bu, tam sertifika zinciri doğrulanabildiği sürece geçerlidir.

Kendi imzalı sertifikalara güvenmekle CA sertifikasına güvenmek arasındaki en büyük fark, güveni dağıtmak ve korumak için gereken yükleme çabasıdır. Ayrıca, şirkete özgü bir CA'yı barındırmak için ek çaba da vardır. 

Tek bir istemci uygulaması olan birden çok sunucu için kendi imzalı sertifikalar için güven dağıtmak için, istemci uygulama güven listesine tüm sunucu uygulama sertifikaları yüklemeniz gerekir. Ayrıca, istemci uygulama sertifikasını tüm sunucu uygulaması güven listelerine yüklemeniz gerekir. Bu yönetim çabası oldukça büyük bir yüktür ve sertifika yaşam sürelerini göz önünde bulundurmanız ve sertifikaları yenilemeniz gerektiğinde bile artar.

Şirkete özgü bir CA kullanımı, birden çok sunucu ve istemciyle güven yönetimini büyük ölçüde basitleştirebilir. Bu durumda, yönetici kullanılan her istemci ve sunucu için bir kez CA imzalı uygulama örneği sertifikası oluşturur. Buna ek olarak, CA Sertifikası tüm sunucularda ve istemcilerde, her uygulama güven listesinde yüklenir. Bu yaklaşımla, yalnızca süresi dolmuş sertifikaların yenilenmeli ve etkilenen uygulamalar için değiştirilmesi gerekir.

Azure Endüstriyel IoT OPC Sertifika Yönetimi hizmeti, OPC UA uygulamaları için şirkete özgü bir CA yönetmenize yardımcı olur. Bu hizmet OPC Vault microservice dayanmaktadır. OPC Vault, şirkete özgü bir CA'yı güvenli bir bulutta barındırmak için bir mikro hizmet sağlar. Bu çözüm, Azure Active Directory (Azure AD), Donanım Güvenlik Modülleri (HSM'ler ile Azure Anahtar Kasası), Azure Cosmos DB ve isteğe bağlı olarak Uygulama Deposu olarak IoT Hub tarafından güvence altına alınmış hizmetlertarafından desteklenen hizmetlerle birlikte yedeklenir.

OPC Vault microservice, Azure Key Vault'ta imzalama hakkı olan güvenlik yöneticilerinin ve onaylayanların istekleri onayladığı veya reddettiği rol tabanlı iş akışını desteklemek üzere tasarlanmıştır.

Mevcut OPC UA çözümleriyle uyumluluk için hizmetler, OPC Vault mikroservis destekli kenar modülü desteğini içerir. Bu, sertifikaları ve güven listelerini belirtimibölüm 12'ye göre dağıtmak için **OPC UA Global Discovery Server ve Sertifika Yönetimi** arabirimini uygular. 


## <a name="architecture"></a>Mimari

Mimari, fabrika ağı için bir OPC Vault IoT Edge modülü ve iş akışını kontrol etmek için bir web örneği UX ile OPC Vault microservice'e dayanmaktadır:

![OPC Vault mimarisinin diyagramı](media/overview-opc-vault-architecture/opc-vault.png)

## <a name="opc-vault-microservice"></a>OPC Vault microservice

OPC Vault microservice, OPC UA uygulamaları için şirkete özgü bir CA'yı dağıtmak ve yönetmek için iş akışını uygulamak için aşağıdaki arabirimlerden oluşur.

### <a name="application"></a>Uygulama 
- OPC UA uygulaması bir sunucu veya istemci veya her ikisi de olabilir. OPC Vault bu durumda bir uygulama kayıt yetkilisi olarak hizmet vermektedir. 
- Uygulamaları kaydetmek, güncelleştirmek ve kayıt dışı etmek için temel işlemlere ek olarak, arama ifadeleri olan uygulamaları bulmak ve sorgulamak için arayüzler de vardır. 
- Sertifika istekleri, bir isteği işlemek ve OPC UA'ya özgü tüm uzantıları içeren imzalı bir sertifika vermek için geçerli bir uygulamaya başvurmalıdır. 
- Uygulama hizmeti Azure Cosmos DB'deki bir veritabanı tarafından yedeklenir.

### <a name="certificate-group"></a>Sertifika grubu
- Sertifika grubu, sertifikaları imzalamak için özel anahtar da dahil olmak üzere kök CA veya alt CA sertifikası depolayan bir varlıktır. 
- RSA anahtar uzunluğu, SHA-2 karma uzunluğu ve kullanım ömürleri hem Veren CA hem de imzalı uygulama sertifikaları için yapılandırılabilir. 
- CA sertifikalarını FIPS 140-2 Düzey 2 HSM ile desteklenen Azure Key Vault'ta saklarsınız. İmzalama, Azure AD tarafından güvenli bir Key Vault işlemi yle yapıldığından, özel anahtar hiçbir zaman güvenli depolama dan ayrılmaz. 
- CA sertifikalarını zaman içinde yenileyebilir ve Key Vault geçmişi nedeniyle güvenli depolama da kalmasını sağlayabilirsiniz. 
- Her CA sertifikasının iptal listesi de key vault'ta gizli olarak saklanır. Bir başvuru kayıt dışı olduğunda, başvuru sertifikası bir yönetici tarafından Sertifika İptal Listesi'nde (CRL) de iptal edilir.
- Tek sertifikaların yanı sıra toplu sertifikaları da iptal edebilirsiniz.

### <a name="certificate-request"></a>Sertifika isteği
Sertifika isteği, Bir OPC UA uygulaması için bir Sertifika İmzaLama İsteği (CSR) kullanarak yeni bir anahtar çifti veya imzalı sertifika oluşturmak için iş akışını uygular. 
- İstek, özne veya KSS gibi eşlik eden bilgilerin bulunduğu bir veritabanında ve OPC UA uygulamasına bir başvuruyla depolanır. 
- Hizmetteki iş mantığı, uygulama veritabanında depolanan bilgilere karşı isteği doğrular. Örneğin, veritabanındaki Uri uygulaması, CSR'deki Uri uygulamasıyla eşleşmelidir.
- İmzalama haklarına sahip bir güvenlik yöneticisi (diğer bir süre Önce Onaylayan rolü) isteği onaylar veya reddeder. İstek onaylanırsa, yeni bir anahtar çifti veya imzalı sertifika (veya her ikisi) oluşturulur. Yeni özel anahtar Key Vault'ta güvenli bir şekilde depolanır ve yeni imzalı ortak sertifika sertifika isteği veritabanında depolanır.
- İstekli, onaylanana veya iptal edilene kadar istek durumunu yoklayabilir. İstek onaylanırsa, özel anahtar ve sertifika OPC UA uygulamasının sertifika deposuna indirilebilir ve yüklenebilir.
- İstekçi artık gereksiz bilgileri istek veritabanından silme isteğini kabul edebilir. 

İmzalı bir sertifikanın ömrü boyunca, bir uygulama silinebilir veya bir anahtar tehlikeye atılabilir. Böyle bir durumda, bir CA yöneticisi şunları yapabilir:
- Uygulamanın bekleyen tüm ve onaylı sertifika isteklerini de silen bir uygulamayı silin. 
- Yalnızca bir anahtar yenilenirse veya ele geçirilirse, yalnızca tek bir sertifika isteğini silin.

Şimdi gizliliği ihlal edilen onaylı ve kabul edilen sertifika istekleri silinmiş olarak işaretlenir.

Bir yönetici, Veren CA CRL'yi düzenli olarak yenileyebilir. Yenileme sırasında, silinen tüm sertifika istekleri iptal edilir ve sertifika seri numaraları CRL iptal listesine eklenir. İptal edilen sertifika istekleri iptal edilmiş olarak işaretlenir. Acil durumlarda, tek sertifika istekleri de iptal edilebilir.

Son olarak, güncelleştirilmiş CRL'ler katılımcı OPC UA istemcilerine ve sunucularına dağıtılabilir.

## <a name="opc-vault-iot-edge-module"></a>OPC Vault IoT Edge modülü
Bir fabrika ağı Global Discovery Server'ı desteklemek için, OPC Vault modüllerini kenarda dağıtabilirsiniz. Yerel bir .NET Core uygulaması olarak çalıştırın veya Docker kapsayıcısında çalıştırın. Geçerli OPC UA .NET Standart yığınındaki Auth2 kimlik doğrulama desteğinin eksikliği nedeniyle, OPC Vault kenar modülünün işlevselliğinin Reader rolüyle sınırlı olduğunu unutmayın. OPC UA GDS standart arabirimi kullanılarak bir kullanıcı kenar modülünden mikrohizmete taklit edilemez.

## <a name="next-steps"></a>Sonraki adımlar

Artık OPC Vault mimarisini öğrendiğiniz için şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [OPC Vault'u oluşturun ve dağıtın](howto-opc-vault-deploy.md)
