---
title: Azure IoT Hub cihaz sağlama hizmeti-X. 509.440 sertifika kanıtlama
description: Cihaz sağlama hizmeti (DPS) ve IoT Hub ile X. 509.440 sertifika kanıtlama kullanımı ile ilgili kavramları açıklar
author: wesmc7777
ms.author: wesmc
ms.date: 09/14/2020
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 911f819343f675ebe0a2604d912e6e26aa646eb5
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90533093"
---
# <a name="x509-certificate-attestation"></a>X.509 sertifikası kanıtlama

Bu makale, X. 509.440 sertifika kanıtlama kullanarak cihazları sağlarken ilgili kavramlara genel bakış sunar. Bu makale, bir cihazı dağıtıma hazırlamayla ilgili olarak tüm personbuna yöneliktir.

X. 509.440 sertifikaları, bir donanım güvenlik modülü HSM 'de depolanabilir.

> [!TIP]
> Üretim aşamasındaki cihazlarınızda bulunan X. 509.440 sertifikası gibi gizli dizileri güvenli bir şekilde depolamak için cihazlarla bir HSM kullanmanız önemle önerilir.


## <a name="x509-certificates"></a>X. 509.440 sertifikaları

Bir kanıtlama mekanizması olarak X. 509.440 sertifikalarını kullanmak, üretimi ölçeklendirmek ve cihaz sağlamayı basitleştirmek için mükemmel bir yoldur. X. 509.440 sertifikaları genellikle, zincirdeki her bir sertifikanın bir sonraki daha yüksek sertifikanın özel anahtarıyla imzalandığı ve bu şekilde otomatik olarak imzalanan bir kök sertifikada sonlandıralındığı bir sertifika güven zinciri halinde düzenlenir. Bu düzenleme, bir cihazda yüklü olan son varlık "yaprak" sertifikasına her bir ara CA aracılığıyla bir güvenilen kök sertifika yetkilisi (CA) tarafından oluşturulan kök sertifikadan temsilci bir güven zinciri oluşturur. Daha fazla bilgi için bkz. [X. 509.440 CA sertifikalarını kullanarak cihaz kimlik doğrulaması](/azure/iot-hub/iot-hub-x509ca-overview). 

Genellikle sertifika zinciri cihazlarla ilişkilendirilmiş bazı mantıksal veya fiziksel hiyerarşiyi temsil eder. Örneğin, bir üretici şunları içerebilir:
- otomatik olarak imzalanan kök CA sertifikası verme
- Her bir fabrika için benzersiz bir ara CA sertifikası oluşturmak üzere kök sertifikayı kullanma
- Tesisteki her üretim satırı için benzersiz bir ara CA sertifikası oluşturmak üzere her bir fabrikasının sertifikasını kullanın
- son olarak, satır üzerinde üretilen her bir cihaz için benzersiz bir cihaz (son varlık) sertifikası oluşturmak üzere üretim satırı sertifikasını kullanın. 

Daha fazla bilgi edinmek için bkz. [IoT sektöründe X. 509.440 CA sertifikalarının kavramsal olarak anlaşılmasına](/azure/iot-hub/iot-hub-x509ca-concept)bakın. 

### <a name="root-certificate"></a>Kök sertifika

Kök sertifika, bir sertifika yetkilisini (CA) temsil eden kendinden imzalı bir X. 509.440 sertifikasıdır. Sertifika zincirinin sonlandıralım veya güven çıpasıdır. Kök sertifikalar bir kuruluş tarafından kendi kendine verilebilir veya bir kök sertifika yetkilisinden satın alınabilir. Daha fazla bilgi edinmek için bkz. [X. 509.440 CA sertifikalarını alma](/azure/iot-hub/iot-hub-security-x509-get-started#get-x509-ca-certificates). Kök sertifika, kök CA sertifikası olarak da adlandırılabilir.

### <a name="intermediate-certificate"></a>Ara sertifika

Ara sertifika, kök sertifika tarafından imzalanmış bir X. 509.440 sertifikasıdır (veya zincirinde kök sertifikaya sahip başka bir ara sertifika tarafından). Bir zincirdeki son ara sertifika, yaprak sertifikayı imzalamak için kullanılır. Ara sertifika, ara CA sertifikası olarak da adlandırılır.

### <a name="end-entity-leaf-certificate"></a>Son varlık "yaprak" sertifikası

Yaprak sertifikası veya son varlık sertifikası, sertifika sahibini tanımlar. Sertifika zincirinde kök sertifikaya ve sıfır veya daha fazla ara sertifikaya sahiptir. Yaprak sertifikası diğer sertifikaları imzalamak için kullanılmaz. Cihazı, sağlama hizmetine benzersiz olarak tanımlar ve bazen cihaz sertifikası olarak adlandırılır. Kimlik doğrulama sırasında cihaz, hizmetten gelen bir itiraz kanıtını yanıtlamak için bu sertifikayla ilişkili özel anahtarı kullanır.

[Tek bir kayıt](./concepts-service.md#individual-enrollment) girişiyle kullanılan yaprak sertifikaların, **konu adının** ayrı kayıt girişinin kayıt kimliğine ayarlanması gerekir. Bir [kayıt grubu](./concepts-service.md#enrollment-group) girdisiyle kullanılan yaprak sertifikaları, kayıt grubundaki kimliği doğrulanmış cihazın **kayıt kayıtlarında** GÖSTERILECEK olan Istenen cihaz kimliğine ayarlanmış **konu adına** sahip olmalıdır.

Daha fazla bilgi edinmek için bkz. [X. 509.440 CA sertifikaları ile imzalanmış cihazların kimliğini doğrulama](/azure/iot-hub/iot-hub-x509ca-overview#authenticating-devices-signed-with-x509-ca-certificates).

## <a name="controlling-device-access-to-the-provisioning-service-with-x509-certificates"></a>X. 509.440 sertifikalarıyla sağlama hizmetine cihaz erişimini denetleme

Sağlama Hizmeti, X. 509.440 kanıtlama mekanizmasını kullanan cihazlara erişimi denetlemek için kullanabileceğiniz iki tür kayıt girişini kullanıma sunar:  

- [Ayrı kayıt](./concepts-service.md#individual-enrollment) girdileri, belirli bir cihazla ilişkili cihaz sertifikasıyla yapılandırılır. Bu girişler belirli cihazlar için kayıtları denetler.
- [Kayıt grubu](./concepts-service.md#enrollment-group) girdileri, belirli bir ara veya kök CA sertifikası ile ilişkilendirilir. Bu girişler, kendi sertifika zincirinde ara veya kök sertifikaya sahip tüm cihazların kayıtlarını denetler. 

Bir cihaz sağlama hizmetine bağlanırsa, hizmet daha az özel kayıt girişi üzerinde daha belirli kayıt girdilerine öncelik verir. Diğer bir deyişle, cihaz için tek bir kayıt varsa, sağlama hizmeti bu girişi uygular. Cihaz için tek bir kayıt yoksa ve cihazın sertifika zincirindeki ilk ara sertifikaya yönelik bir kayıt grubu varsa, hizmet bu girişi uygular ve bu şekilde köke zincirin. Hizmet, bulduğu ilk geçerli girişi uygular, örneğin:

- Bulunan ilk kayıt girdisi etkinse hizmet cihazı sağlar.
- Bulunan ilk kayıt girdisi devre dışıysa, hizmet cihazı sağlamaz.  
- Cihazın sertifika zincirindeki herhangi bir sertifika için kayıt girişi bulunmazsa, hizmet cihazı sağlamaz. 

Sertifika zincirlerinin bu mekanizması ve hiyerarşik yapısı, tek tek cihazlar ve cihaz grupları için erişimi nasıl denetleyebileceği konusunda güçlü esneklik sağlar. Örneğin, aşağıdaki sertifika zincirleriyle beş cihaz düşünün: 

- *Cihaz 1*: kök sertifika-> sertifika A > cihaz 1 sertifikası
- *Cihaz 2*: kök sertifika-> sertifika A-> cihaz 2 sertifikası
- *Cihaz 3*: kök sertifika-> sertifika A > cihaz 3 sertifikası
- *Cihaz 4*: kök sertifika-> sertifika B-> cihaz 4 sertifikası
- *Aygıt 5*: kök sertifika-> sertifika B-> cihaz 5 sertifikası

Başlangıçta, tüm beş cihaz için erişimi etkinleştirmek üzere kök sertifika için tek bir etkin grup kayıt girişi oluşturabilirsiniz. Sertifika B daha sonra tehlikeye atılırsa, sertifika B için, *cihaz 4* ve *Cihaz 5* ' in kaydedilmesini engellemek için devre dışı bırakılmış bir kayıt grubu girişi oluşturabilirsiniz. Hala sonraki *Cihaz 3* ' ün güvenliği tehlikeye girerse, sertifikası için devre dışı bırakılmış bir kayıt girişi oluşturabilirsiniz. Bu, *Cihaz 3*' e erişimi iptal eder, ancak hala *cihaz 1* ve *cihaz 2* ' nin kaydetmesine izin verir.