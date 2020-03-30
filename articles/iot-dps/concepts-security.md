---
title: Azure IoT Hub Aygıt Sağlama Hizmeti - Güvenlik kavramları
description: Aygıt Sağlama Hizmeti (DPS) ve IoT Hub ile aygıtlara özgü güvenlik sağlama kavramlarını açıklar
author: nberdy
ms.author: nberdy
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 3191e9886604af9b2a26b71a89cee699197585c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271570"
---
# <a name="iot-hub-device-provisioning-service-security-concepts"></a>IoT Hub Cihaz Sağlama Hizmeti güvenlik kavramları 

IoT Hub Aygıt Sağlama Hizmeti, ioT Hub için sıfır dokunmatik aygıt sağlama sağlamasını belirli bir IoT hub'ına yapılandırmak için kullandığınız bir yardımcı hizmettir. Aygıt Sağlama Hizmeti ile milyonlarca cihazı güvenli ve ölçeklenebilir bir şekilde [otomatik olarak sağlayabilirsiniz.](concepts-auto-provisioning.md) Bu makalede, aygıt sağlama ile ilgili *güvenlik* kavramlarına genel bir bakış sağlar. Bu makale, bir aygıtı dağıtıma hazır hale getirmekle ilgili tüm kişilikler için geçerlidir.

## <a name="attestation-mechanism"></a>Attestation mekanizması

Attestation mekanizması, bir aygıtın kimliğini doğrulamak için kullanılan yöntemdir. Attestation mekanizması, belirli bir cihazla hangi attestation yönteminin kullanılacağını sağlayan kayıt listesiyle de ilgilidir.

> [!NOTE]
> IoT Hub, söz bu hizmetteki benzer bir kavram için "kimlik doğrulama şeması" kullanır.

Cihaz Sağlama Hizmeti aşağıdaki attestation formlarını destekler:
* **X.509 sertifikaları standart X.509** sertifika kimlik doğrulama akışını temel alınca.
* **Güvenilir Platform Modülü (TPM),** anahtarların imzalı paylaşılan erişim imzası (SAS) belirteci sunmak için TPM standardını kullanarak bir nonce meydan okumaya dayanır. Bu attestation formu cihazda fiziksel bir TPM gerektirmez, ancak hizmet [TPM spec](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)başına onay anahtarı nı kullanarak kanıtlamak için bekliyor.
* Ortak erişim imzası (SAS) [Güvenlik belirteçlerine](../iot-hub/iot-hub-devguide-security.md#security-tokens)dayalı **simetrik anahtar,** bir hashed imza ve gömülü bir son kullanma süresi içerir. Daha fazla bilgi için [Bkz. Simetrik anahtar attestation.](concepts-symmetric-key-attestation.md)


## <a name="hardware-security-module"></a>Donanım güvenlik modülü

Donanım güvenlik modülü veya HSM, aygıt sırlarının güvenli, donanım tabanlı depolanması için kullanılır ve gizli depolamanın en güvenli biçimidir. Hem X.509 sertifikaları hem de SAS belirteçleri HSM'de depolanabilir. HSM'ler, protestation'ın desteklediği her iki attestation mekanizmasıyla da kullanılabilir.

> [!TIP]
> Aygıtlarınızdaki sırları güvenli bir şekilde depolamak için cihazlarla birlikte bir HSM kullanmanızı şiddetle öneririz.

Aygıt sırları da yazılımda (bellekte) depolanabilir, ancak HSM'den daha az güvenli bir depolama biçimidir.

## <a name="trusted-platform-module"></a>Güvenilir Platform Modülü

TPM, platformun kimliğini doğrulamak için kullanılan anahtarları güvenli bir şekilde depolamak için bir standarda veya standardı uygulayan modüllerle etkileşimde bulunmak için kullanılan G/Ç arabirimine başvurabilir. TPM'ler ayrı donanım, tümleşik donanım, yazılım tabanlı veya yazılım tabanlı olarak bulunabilir. [TPM'ler ve TPM attestation](/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation)hakkında daha fazla bilgi edinin. Aygıt Sağlama Hizmeti yalnızca TPM 2.0'ı destekler.

TPM attestation imzalı paylaşılan erişim imzası (SAS) belirteci sunmak için onay ve depolama kök anahtarlarını kullanan bir nonce meydan okuma dayanmaktadır.

### <a name="endorsement-key"></a>Onay anahtarı

Onay anahtarı, dahili olarak üretilen veya üretim zamanında enjekte edilen ve her TPM için benzersiz olan TPM içinde bulunan asimetrik bir anahtardır. Onay anahtarı değiştirilemez veya kaldırılamaz. Onay anahtarının özel bölümü TPM dışında asla serbest bırakılmazken, onay anahtarının ortak bölümü gerçek bir TPM'yi tanımak için kullanılır. [Onay anahtarı](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx)hakkında daha fazla bilgi edinin.

### <a name="storage-root-key"></a>Depolama kök anahtarı

Depolama kök anahtarı TPM'de depolanır ve uygulamalar tarafından oluşturulan TPM anahtarlarını korumak için kullanılır, böylece bu anahtarlar TPM olmadan kullanılamaz. Depolama kök anahtarı, TPM'nin sahipliğini aldığınızda oluşturulur; Yeni bir kullanıcının sahipliğini alabilmesi için TPM'yi temizlediğinizde, yeni bir depolama kök anahtarı oluşturulur. Depolama kök [anahtarı](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx)hakkında daha fazla bilgi edinin.

## <a name="x509-certificates"></a>X.509 sertifikaları

X.509 sertifikalarını bir attestation mekanizması olarak kullanmak, üretimi ölçeklendirmenin ve cihaz sağlamayı basitleştirmenin mükemmel bir yoludur. X.509 sertifikaları genellikle, zincirdeki her sertifikanın bir sonraki yüksek sertifikanın özel anahtarı tarafından imzalandığı ve böylece kendi imzaladığı kök sertifikasında sonlandırıldığı bir güven sertifikası zincirinde düzenlenir. Bu düzenleme, güvenilir bir kök sertifika yetkilisi (CA) tarafından oluşturulan kök sertifikasından, her ara CA üzerinden bir aygıta yüklenen son varlık "yaprak" sertifikasına kadar devredilmiş bir güven zinciri kurar. Daha fazla bilgi için [X.509 CA Sertifikalarını kullanarak Aygıt Kimlik Doğrulaması'na](/azure/iot-hub/iot-hub-x509ca-overview)bakın. 

Genellikle sertifika zinciri aygıtlarla ilişkili bazı mantıksal veya fiziksel hiyerarşiyi temsil eder. Örneğin, bir üretici şunları yapabilir:
- kendi imzalı kök CA sertifikası verme
- her fabrika için benzersiz bir ara CA sertifikası oluşturmak için kök sertifikasını kullanın
- fabrikadaki her üretim hattı için benzersiz bir ara CA sertifikası oluşturmak için her fabrikanın sertifikasını kullanın
- ve son olarak, hat üzerinde üretilen her cihaz için benzersiz bir aygıt (son varlık) sertifikası oluşturmak için üretim çizgisi sertifikasını kullanın. 

Daha fazla bilgi için, [IoT endüstrisindeki X.509 CA sertifikalarının Kavramsal anlayışına](/azure/iot-hub/iot-hub-x509ca-concept)bakın. 

### <a name="root-certificate"></a>Kök sertifikası

Kök sertifika, sertifika yetkilisini (CA) temsil eden kendi imzalı x.509 sertifikasıdır. Sertifika zincirinin sonlandırmaveya güven çapasidir. Kök sertifikalar bir kuruluş tarafından kendi kendine verilebilir veya bir kök sertifika yetkilisinden satın alınabilir. Daha fazla bilgi için Bkz. [X.509 CA sertifikaları alın.](/azure/iot-hub/iot-hub-security-x509-get-started#get-x509-ca-certificates) Kök sertifika, kök CA sertifikası olarak da adlandırılabilir.

### <a name="intermediate-certificate"></a>Ara sertifika

Ara sertifika, kök sertifika (veya zincirinde kök sertifikası bulunan başka bir ara sertifika) tarafından imzalanmış bir X.509 sertifikasıdır. Zincirdeki son ara sertifika, yaprak sertifikasını imzalamak için kullanılır. Ara sertifika, ara CA sertifikası olarak da adlandırılabilir.

### <a name="end-entity-leaf-certificate"></a>Son varlık "yaprak" sertifikası

Yaprak sertifika veya son varlık sertifikası, sertifika sahibini tanımlar. Sertifika zincirinde kök sertifikanın yanı sıra sıfır veya daha fazla ara sertifikaya sahiptir. Yaprak sertifikası diğer sertifikaları imzalamak için kullanılmaz. Aygıtı sağlama hizmetine benzersiz bir şekilde tanımlar ve bazen aygıt sertifikası olarak adlandırılır. Kimlik doğrulama sırasında aygıt, hizmetten gelen sahip olma sorununa ilişkin bir kanıta yanıt vermek için bu sertifikayla ilişkili özel anahtarı kullanır.

[Bireysel kayıt](./concepts-service.md#individual-enrollment) girişiyle birlikte kullanılan yaprak sertifikaların, **Konu Adı'nın** Bireysel Kayıt girişinin kayıt kimliğine ayarlanmış olması şartı vardır. [Kayıt grubu](./concepts-service.md#enrollment-group) girişiile birlikte kullanılan yaprak sertifikaları, Kayıt grubunda kimlik doğrulaması yapılan aygıtın **Kayıt Kayıtları'nda** gösterilecek olan **Konu Adı'nı** istenilen cihaz kimliğine ayarlamalıdır.

Daha fazla bilgi için Bkz. [X.509 CA sertifikalarıyla imzalanmış kimlik doğrulama aygıtları.](/azure/iot-hub/iot-hub-x509ca-overview#authenticating-devices-signed-with-x509-ca-certificates)

## <a name="controlling-device-access-to-the-provisioning-service-with-x509-certificates"></a>X.509 sertifikaları ile sağlama hizmetine cihaz erişimini denetleme

Sağlama hizmeti, X.509 attestation mekanizmasını kullanan aygıtlar için erişimi denetlemek için kullanabileceğiniz iki tür kayıt girişi ortaya çıkarır:  

- [Tek tek kayıt](./concepts-service.md#individual-enrollment) girişleri, belirli bir aygıtla ilişkili aygıt sertifikasıyla yapılandırılır. Bu girişler belirli aygıtlar için kayıtları denetler.
- [Kayıt grubu](./concepts-service.md#enrollment-group) girişleri belirli bir ara veya kök CA sertifikası ile ilişkilidir. Bu girişler, sertifika zincirinde bu ara veya kök sertifikaya sahip tüm aygıtların kayıtlarını denetler. 

Bir aygıt sağlama hizmetine bağlandığında, hizmet daha az belirli kayıt girişlerine göre daha spesifik kayıt girişlerine öncelik verir. Diğer bir yazıda, aygıt için tek tek bir kayıt varsa, sağlama hizmeti bu girişi uygular. Aygıt için tek tek kayıt ve aygıtın sertifika zincirindeki ilk ara sertifika için bir kayıt grubu yoksa, hizmet bu girişi uygular ve böylece, kökzincirinin yukarısına doğru. Hizmet, bulduğu ilk geçerli girişi uygular, örneğin:

- Bulunan ilk kayıt girişi etkinse, hizmet aygıtı karşılar.
- Bulunan ilk kayıt girişi devre dışı bırakılırsa, hizmet aygıtı sağlamaz.  
- Aygıtın sertifika zincirindeki sertifikalardan herhangi biri için kayıt girişi bulunmazsa, hizmet aygıtı sağlamaz. 

Bu mekanizma ve sertifika zincirlerinin hiyerarşik yapısı, tek tek aygıtların yanı sıra aygıt grupları için erişimi nasıl denetlenebildiğinizkonusunda güçlü esneklik sağlar. Örneğin, aşağıdaki sertifika zincirlerine sahip beş aygıt düşünün: 

- *Cihaz 1*: kök sertifika -> sertifikası A -> cihazı 1 sertifika
- *Cihaz 2*: kök sertifikası -> sertifikası A -> cihazı 2 sertifikası
- *Cihaz 3*: kök sertifika -> sertifikası A -> cihazı 3 sertifika
- *Cihaz 4*: kök sertifika -> sertifikası B -> cihazı 4 sertifika
- *Cihaz 5*: kök sertifika -> sertifikası B -> cihazı 5 sertifika

Başlangıçta, beş aygıtın tümüne erişimi etkinleştirmek için kök sertifika için tek bir etkin grup kaydı girişi oluşturabilirsiniz. B sertifikası daha sonra tehlikeye girerse, *Aygıt 4* ve *Aygıt 5'in* kaydolmasını önlemek için B sertifikası için devre dışı bırakılmış bir kayıt grubu girişi oluşturabilirsiniz. Aygıt *3* daha da geç kalırsa, sertifikası için engelli bir bireysel kayıt girişi oluşturabilirsiniz. Bu, Aygıt *3'e*erişimi iptal eder, ancak yine de *Aygıt 1* ve *Aygıt 2'nin* kaydolmasına izin verir.