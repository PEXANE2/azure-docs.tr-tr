---
title: Azure IoT Hub cihaz sağlama hizmeti-güvenlik kavramları
description: Cihaz sağlama hizmeti ve IoT Hub olan cihazlara özgü güvenlik sağlama kavramlarını açıklar
author: nberdy
ms.author: nberdy
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: ad392d9d979986723c17b43f210959e2504a8fb8
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74228817"
---
# <a name="iot-hub-device-provisioning-service-security-concepts"></a>Cihaz sağlama hizmeti güvenlik kavramlarını IoT Hub 

IoT Hub cihaz sağlama hizmeti, belirli bir IoT Hub 'ına sıfır Touch cihaz sağlamayı yapılandırmak için kullandığınız IoT Hub yardımcı hizmettir. Cihaz sağlama hizmeti ile milyonlarca cihazı güvenli ve ölçeklenebilir bir şekilde [Otomatik](concepts-auto-provisioning.md) olarak sağlayabilirsiniz. Bu makale, cihaz sağlamaya dahil olan *güvenlik* kavramlarıyla ilgili genel bir bakış sunar. Bu makale, bir cihazı dağıtıma hazırlamayla ilgili olarak tüm personbuna yöneliktir.

## <a name="attestation-mechanism"></a>Kanıtlama mekanizması

Kanıtlama mekanizması, bir cihazın kimliğini onaylamak için kullanılan yöntemdir. Kanıtlama mekanizması, sağlama hizmetine belirli bir cihazla hangi kanıtlama yöntemini kullanacağınızı belirten kayıt listesi ile de ilgilidir.

> [!NOTE]
> IoT Hub, söz konusu hizmette benzer bir kavram için "kimlik doğrulama şeması" nı kullanır.

Cihaz sağlama hizmeti aşağıdaki kanıtlama biçimlerini destekler:
* Standart X. 509.440 sertifika kimlik doğrulama akışına göre **X. 509.440 sertifikaları** .
* Anahtar bir paylaşılan erişim Imzası (SAS) belirteci sunmak için anahtarlar için TPM standardı kullanarak, bir kerelik anahtar sınamasını temel alan **Güvenilir Platform Modülü (TPM)** . Bu kanıtlama biçimi cihazda bir fiziksel TPM gerektirmez, ancak hizmet [TPM belirtimi](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)başına onay anahtarını kullanarak test bulmayı bekler.
* Karma **anahtar** , karma bir imza ve katıştırılmış süre sonu içeren paylaşılan erişim IMZASı (SAS) [güvenlik belirteçlerini](../iot-hub/iot-hub-devguide-security.md#security-tokens)temel alır. Daha fazla bilgi için bkz. [simetrik anahtar kanıtlama](concepts-symmetric-key-attestation.md).


## <a name="hardware-security-module"></a>Donanım güvenlik modülü

Donanım güvenlik modülü veya HSM, cihaz gizli dizileri için güvenli, donanım tabanlı depolamada kullanılır ve gizli depolama alanının en güvenli biçimidir. Hem X. 509.440 sertifikaları hem de SAS belirteçleri HSM 'de depolanabilir. HSM 'ler, sağlamanın desteklediği kanıtlama mekanizmalarıyla birlikte kullanılabilir.

> [!TIP]
> Cihazlarınızda gizli dizileri güvenli bir şekilde depolamak için cihazlarla bir HSM kullanmanız önemle önerilir.

Cihaz gizli dizileri de yazılımda (bellek) depolanabilir, ancak bir HSM 'den daha az güvenli bir depolama biçimidir.

## <a name="trusted-platform-module"></a>Güvenilir Platform Modülü

TPM, platformun kimliğini doğrulamak için kullanılan anahtarları güvenli bir şekilde depolamak için bir standarda başvurabilir veya standart uygulayan modüllerle etkileşim kurmak için kullanılan g/ç arabirimine başvurabilir. TPMs ayrık donanım, tümleşik donanım, bellenim tabanlı veya yazılım tabanlı olarak bulunabilir. [TPMS ve TPM kanıtlama](/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation)hakkında daha fazla bilgi edinin. Cihaz sağlama hizmeti yalnızca TPM 2,0 ' i destekler.

TPM kanıtlama, imzalı bir paylaşılan erişim Imzası (SAS) belirteci sunmak için onay ve depolama kök anahtarlarını kullanan bir kerelik anahtar sınamasını temel alır.

### <a name="endorsement-key"></a>Onay anahtarı

Onay anahtarı TPM içinde bulunan, dahili olarak oluşturulan veya üretim zamanında eklenen ve her TPM için benzersiz olan bir asimetrik anahtardır. Onay anahtarı değiştirilemez veya kaldırılamaz. Onay anahtarının özel bölümü hiçbir şekilde TPM dışında serbest bırakılır, ancak onay anahtarının ortak kısmı orijinal TPM 'YI tanımak için kullanılır. [Onay anahtarı](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx)hakkında daha fazla bilgi edinin.

### <a name="storage-root-key"></a>Depolama kök anahtarı

Depolama kök anahtarı TPM 'de depolanır ve uygulamalar tarafından oluşturulan TPM anahtarlarını korumak için kullanılır, böylece bu anahtarlar TPM olmadan kullanılamaz. Depolama kök anahtarı TPM 'nin sahipliğini aldığınızda oluşturulur; TPM 'YI temizleyerek yeni bir kullanıcının sahipliğini kazanması için yeni bir depolama kök anahtarı oluşturulur. [Depolama kök anahtarı](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx)hakkında daha fazla bilgi edinin.

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
- *Aygıt 5*: kök sertifika-> sertifika B-> Cihaz 5 sertifikası

Başlangıçta, tüm beş cihaz için erişimi etkinleştirmek üzere kök sertifika için tek bir etkin grup kayıt girişi oluşturabilirsiniz. Sertifika B daha sonra tehlikeye atılırsa, sertifika B için, *cihaz 4* ve *Cihaz 5* ' in kaydedilmesini engellemek için devre dışı bırakılmış bir kayıt grubu girişi oluşturabilirsiniz. Hala sonraki *Cihaz 3* ' ün güvenliği tehlikeye girerse, sertifikası için devre dışı bırakılmış bir kayıt girişi oluşturabilirsiniz. Bu, *Cihaz 3*' e erişimi iptal eder, ancak hala *cihaz 1* ve *cihaz 2* ' nin kaydetmesine izin verir.