---
title: IoT Hub Cihaz Sağlama Hizmeti - Otomatik sağlama kavramları
description: Bu makalede, IoT Aygıt Sağlama Hizmeti (DPS), IoT Hub ve istemci SDK'ları kullanarak aygıt otomatik sağlama aşamalarına kavramsal bir genel bakış sağlanmıştır.
author: wesmc7777
ms.author: wesmc
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: c94fa6b851dfc9923628a738a15f7c245204f73f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975338"
---
# <a name="auto-provisioning-concepts"></a>Otomatik sağlama kavramları

[Genel Bakış'ta](about-iot-dps.md)açıklandığı gibi, Cihaz Sağlama Hizmeti, cihazların bir IoT hub'ına tam zamanında sağlanmasını sağlayan, insan müdahalesine gerek kalmadan yardımcı bir hizmettir. Başarılı sağlamadan sonra, aygıtlar kendilerine tahsis edilen IoT Hub'ına doğrudan bağlanır. Bu işlem otomatik sağlama olarak adlandırılır ve aygıtlar için kullanıma hazır bir kayıt ve ilk yapılandırma deneyimi sağlar.

## <a name="overview"></a>Genel Bakış

Azure IoT otomatik sağlama üç aşamaya ayrılabilir:

1. **Hizmet yapılandırması** - Azure IoT Hub ve IoT Hub Aygıt Sağlama Hizmeti örneklerinin tek seferlik yapılandırması, bunları oluşturma ve aralarında bağlantı oluşturma.

   > [!NOTE]
   > IoT çözümünüzün boyutune bakılmaksızın, milyonlarca cihazı desteklemeyi planlasanız bile, bu **tek seferlik**bir yapılandırmadır.

2. **Aygıt kaydı** - Aygıt Sağlama Hizmeti örneğini gelecekte kaydolmaya çalışacak aygıtlardan haberdar etme işlemi. [Kayıt,](concepts-service.md#enrollment) tek bir aygıt için "bireysel kayıt" veya birden çok aygıt için "grup kaydı" olarak, sağlama hizmetindeki aygıt kimlik bilgilerini yapılandırarak gerçekleştirilir. Kimlik, cihazın kullanmak üzere tasarlandığı ve sağlama hizmetinin kayıt sırasında cihazın orijinalliğini doğrulamasını sağlayan [attestation mekanizmasına](concepts-security.md#attestation-mechanism) dayanır:

   - **TPM**: "bireysel kayıt" olarak yapılandırılan cihaz kimliği, TPM kayıt kimliğive genel onay anahtarına dayanır. TPM'nin bir [belirtim](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)olduğu göz önüne alındığında, hizmet, TPM uygulaması (donanım veya yazılım) ne olursa olsun, yalnızca belirtim başına doğrulayıcı olmasını bekler. Bkz. Aygıt sağlama: TPM tabanlı attestation hakkında ayrıntılar için [TPM ile kimlik attestation.](https://azure.microsoft.com/blog/device-provisioning-identity-attestation-with-tpm/) 

   - **X509**: "bireysel kayıt" veya "grup kaydı" olarak yapılandırılan cihaz kimliği, kayda .pem veya .cer dosyası olarak yüklenen X.509 dijital sertifikaya dayanır.

   > [!IMPORTANT]  
   > Aygıt Sağlama Hizmetlerini kullanmak için bir ön koşul olmasa da, cihazınızın tuşlar ve X.509 sertifikaları gibi hassas aygıt kimlik bilgilerini depolamak için bir Donanım Güvenlik Modülü (HSM) kullanmasını önemle tavsiye ediyoruz.

3. **Cihaz kaydı ve yapılandırması** - cihaz ve attestation mekanizması için uygun bir Cihaz Sağlama Hizmeti istemcisi SDK kullanılarak inşa edilen kayıt yazılımı tarafından önyükleme üzerine başlatılan. Yazılım, aygıtın kimlik doğrulaması ve ardından IoT Hub'ına kaydedilmesi için sağlama hizmetine bir bağlantı kurar. Başarılı bir kayıt üzerine cihaz, ioT Hub'a özgü cihaz kimliği ve bağlantı bilgileriyle birlikte sağlanmış ve ilk yapılandırmasını çekmesine ve telemetri işlemine başlamasına olanak tanır. Üretim ortamlarında, bu aşama önceki iki aşamadan haftalar veya aylar sonra oluşabilir.

## <a name="roles-and-operations"></a>Roller ve operasyonlar

Önceki bölümde tartışılan aşamalar, üretim süresi, nakliye, gümrük süreci, vb. gibi üretim gerçeklerinden dolayı haftalara veya aylara yayılabilir. Buna ek olarak, ilgili çeşitli varlıklar göz önüne alındığında birden çok rol arasında faaliyetleri yayılabilir. Bu bölümde, her aşamayla ilgili çeşitli rollere ve işlemlere daha derin bir bakış bakar, ardından bir sıralı diyagramdaki akışı gösterir. 

Otomatik sağlama, aygıt üreticisine, attestation mekanizmasını etkinleştirmeye özel gereksinimler de yerleştirir. Üretim işlemleri, özellikle otomatik tedarik kurulduktan sonra yeni cihazların temin edildiği durumlarda, otomatik sağlama aşamalarının zamanlamasından bağımsız olarak da gerçekleşebilir.

Uygulamalı deneyim yoluyla otomatik sağlamayı açıklamaya yardımcı olmak için, soldaki içerik tablosunda bir dizi Quickstarts sağlanır. Öğrenme sürecini kolaylaştırmak/kolaylaştırmak için yazılım, kayıt ve kayıt için fiziksel bir aygıtı simüle etmek için kullanılır. Bazı Hızlı Başlangıçlar, Quickstarts'ın benzetimli yapısı nedeniyle, var olmayan rollere yönelik işlemler de dahil olmak üzere birden çok rol için işlemleri gerçekleştirmenizi gerektirir.

| Rol | İşlem | Açıklama |
|------| --------- | ------------|
| Üretici | Kimlik ve kayıt URL'sini kodlama | Kullanılan attestation mekanizmasına bağlı olarak, üretici aygıt kimlik bilgilerini ve Aygıt Sağlama Hizmeti kayıt URL'sini kodlamaktan sorumludur.<br><br>**Quickstarts**: cihaz simüle olduğundan, üretici rolü yoktur. Örnek kayıt başvurusunun kodlanmasında kullanılan bu bilgileri nasıl aldığınıza ilişkin ayrıntılar için Geliştirici rolüne bakın. |
| | Cihaz kimliği sağlama | Cihaz kimlik bilgisinin yaratıcısı olarak, üretici bu bilgileri operatöre (veya belirli bir aracıya) iletmek veya API'ler aracılığıyla doğrudan Aygıt Sağlama Hizmetine kaydettirmekten sorumludur.<br><br>**Quickstarts**: cihaz simüle olduğundan, üretici rolü yoktur. Simüle edilmiş bir aygıtı Aygıt Sağlama Hizmeti örneğinize kaydetmek için kullanılan aygıt kimliğini nasıl aldığınızla ilgili ayrıntılar için Operatör rolüne bakın. |
| İşleç | Otomatik sağlamayı yapılandırma | Bu işlem otomatik sağlamanın ilk aşamasına karşılık gelir.<br><br>**Hızlı Başlatmalar**: Azure aboneliğinizde Aygıt Sağlama Hizmeti ve IoT Hub örneklerini yapılandırarak Operatör rolünü gerçekleştirirsiniz. |
|  | Aygıt kimliğini kaydetme | Bu işlem otomatik sağlamanın ikinci aşamasına karşılık gelir.<br><br>**Hızlı Başlatmalar**: Operatör rolünü gerçekleştirerek, simüle ettiğiniz aygıtı Aygıt Sağlama Hizmeti örneğinize kaydedebilirsiniz. Cihaz kimliği, Quickstart'ta (TPM veya X.509) simüle edilen attestation yöntemiyle belirlenir. Attestation ayrıntıları için Geliştirici rolüne bakın. |
| Cihaz Tedarik Hizmeti,<br>IoT Hub | \<tüm işlemler\> | Hem fiziksel aygıtlarla bir üretim uygulaması hem de simüle edilmiş aygıtlarla hızlı başlangıçlar için, bu roller Azure aboneliğinizde yapılandırdığınız IoT hizmetleri aracılığıyla yerine getirilir. IoT hizmetleri fiziksel ve simüle edilmiş aygıtların sağlanmasına kayıtsız kaldığı için roller/işlemler tam olarak aynı işlevi görür. |
| Geliştirici | Kayıt yazılımı oluşturma/dağıtma | Bu işlem, otomatik sağlamanın üçüncü aşamasına karşılık gelir. Geliştirici, uygun SDK'yı kullanarak kayıt yazılımını aygıta oluşturmak ve dağıtmakla sorumludur.<br><br>**Hızlı Başlatmalar**: Oluşturduğunuz örnek kayıt uygulaması, iş istasyonunuzun üzerinde çalışan platformunuz/diliniz için (fiziksel bir cihaza dağıtmak yerine) gerçek bir aygıtı simüle eder. Kayıt uygulaması, fiziksel bir aygıta dağıtılan işlemle aynı işlemleri gerçekleştirir. Attestation yöntemini (TPM veya X.509 sertifikası) ve Cihaz Sağlama Hizmeti örneğinizin kayıt URL'sini ve "Kimlik Kapsamını" belirtirsiniz. Cihaz kimliği, belirttiğiniz yönteme göre çalışma zamanında SDK attestation mantığı tarafından belirlenir: <ul><li>**TPM attestation** - geliştirme iş istasyonu bir [TPM simülatörü uygulaması](how-to-use-sdk-tools.md#trusted-platform-module-tpm-simulator)çalışır. Çalıştırılan bir uygulama, aygıt kimliğini n için kullanılmak üzere TPM'nin "Onay Anahtarı" ve "Kayıt Kimliği"ni ayıklamak için ayrı bir uygulama kullanılır. SDK attestation mantığı, kimlik doğrulama ve kayıt doğrulaması için imzalı bir SAS belirteci sunmak için kayıt sırasında simülatörü de kullanır.</li><li>**X509 attestation** - bir [sertifika oluşturmak](how-to-use-sdk-tools.md#x509-certificate-generator)için bir araç kullanın. Oluşturulduktan sonra, kayıtta kullanılmak üzere gereken sertifika dosyasını oluşturursunuz. SDK attestation mantığı, kimlik doğrulama ve kayıt doğrulaması için sunmak için sertifikayı da kayıt sırasında kullanır.</li></ul> |
| Cihaz | Bootup ve kayıt | Bu işlem, Geliştirici tarafından oluşturulmuş aygıt kayıt yazılımı tarafından yerine getirilen otomatik sağlamanın üçüncü aşamasına karşılık gelir. Ayrıntılar için Geliştirici rolüne bakın. İlk önyükleme üzerine: <ol><li>Uygulama, geliştirme sırasında belirtilen genel URL ve hizmet "Kimlik Kapsamı"na göre Aygıt Sağlama Hizmeti örneğine bağlanır.</li><li>Bağlandıktan sonra, cihaz kayıt sırasında belirtilen attestation yöntemi ve kimliğine karşı kimlik doğrulaması yapılır.</li><li>Kimlik doğrulaması alındıktan sonra, aygıt sağlama hizmeti örneği tarafından belirtilen IoT Hub örneğine kaydedilir.</li><li>Başarılı bir kayıt olduktan sonra, benzersiz bir aygıt kimliği ve IoT Hub bitiş noktası, IoT Hub ile iletişim kurmak için kayıt uygulamasına iade edilir.</li><li> Buradan, aygıt yapılandırma için ilk [aygıtının ikiz](~/articles/iot-hub/iot-hub-devguide-device-twins.md) durumunu aşağı çekebilir ve telemetri verilerini raporlama işlemine başlayabilir.</li></ol>**Quickstarts**: cihaz simüle olduğundan, kayıt yazılımı geliştirme iş istasyonuüzerinde çalışır.|

Aşağıdaki diyagram, aygıt otomatik sağlama sırasında operasyonların rollerini ve sıralamasını özetler:
<br><br>
[![Aygıt için otomatik sağlama sırası](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png)](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png#lightbox) 

> [!NOTE]
> İsteğe bağlı olarak, üretici cihaz sağlama hizmeti API'lerini kullanarak (Operatör yerine) "Aygıt kimliğini kaydedin" işlemini de gerçekleştirebilir. Bu sıralama ve daha fazlası hakkında ayrıntılı bir tartışma için [Azure IoT videosuyla Zero touch aygıt kaydına](https://youtu.be/cSbDRNg72cU?t=2460) bakın (işaret41:00'den başlayarak)

## <a name="roles-and-azure-accounts"></a>Roller ve Azure hesapları

Her rolün bir Azure hesabına nasıl eşleneceğini senaryoya bağlıdır ve dahil olabilecek pek çok senaryo vardır. Aşağıdaki ortak desenler, rollerin genellikle bir Azure hesabına nasıl eşlendirildigine ilişkin genel bir anlayış sağlamaya yardımcı olmalıdır.

#### <a name="chip-manufacturer-provides-security-services"></a>Çip üreticisi güvenlik hizmetleri sağlar

Bu senaryoda, üretici düzey bir müşteriler için güvenliği yönetir. Bu senaryo, ayrıntılı güvenliği yönetmek zorunda olmadıklarından, bu düzey bir müşteriler tarafından tercih edilebilir. 

Üretici Donanım Güvenlik Modülleri (HSM) içine güvenlik tanıttı. Bu güvenlik, üreticinin DPS örnekleri ve kayıt grupları kurulumu olan potansiyel müşterilerden anahtarlar, sertifikalar vb. almasını içerebilir. Üretici, bu güvenlik bilgilerini müşterileri için de oluşturabilir.

Bu senaryoda, iki Azure hesabı söz konusu olabilir:

- **Hesap #1**: Büyük olasılıkla operatör ve geliştirici rolleri arasında bir dereceye kadar paylaşılır. Bu taraf, HSM yongalarını üreticiden satın alabilir. Bu yongalar, Hesap #1 ile ilişkili DPS örneklerine işaret edilir. DPS kayıtlarıyla bu taraf, DPS'deki aygıt kayıt ayarlarını yeniden yapılandırarak aygıtları birden çok düzey iki müşteriye kiralayabilir. Bu taraf ayrıca cihaz telemetrisi vb erişmek için arabirim için son kullanıcı arka uç sistemleri için ayrılmış IoT hub'ları olabilir. Bu ikinci durumda, ikinci bir hesap gerekli olmayabilir.

- **Hesap #2**: Son kullanıcılar, düzey iki müşterilerin kendi IoT hub'ları olabilir. Hesap #1 ilişkili taraf, kiralanan aygıtları bu hesaptaki doğru hub'a işaret ediyor. Bu yapılandırma, DPS ve IoT hub'larının Azure hesapları arasında bağlanmasını gerektirir ve bu da Azure Kaynak Yöneticisi şablonlarıyla yapılabilir.

#### <a name="all-in-one-oem"></a>Hepsi bir ayine oem

Üretici, yalnızca tek bir üretici hesabının gerekli olacağı bir "Hepsi Bir OEM" olabilir. Üretici, güvenliği ve sağlamayı uçuca işler.

Üretici, aygıt satın alan müşterilere bulut tabanlı bir uygulama sağlayabilir. Bu uygulama, üretici tarafından tahsis edilen IoT Hub ile arabirim olur.

Otomatlar veya otomatik kahve makineleri bu senaryoiçin örnekler temsil eder.




## <a name="next-steps"></a>Sonraki adımlar

İlgili otomatik sağlama Quickstarts ile yolunuzu çalışırken, referans noktası olarak bu makaleyi yer imi yararlı bulabilirsiniz. 

"Hizmet yapılandırması" aşamasında size en uygun olan ve yönetim aracı tercihinize en uygun "Otomatik sağlama yı ayarlama" Quickstart'ı tamamlayarak başlayın:

- [Azure CLI kullanarak otomatik sağlama ayarlama](quick-setup-auto-provision-cli.md)
- [Azure portalını kullanarak otomatik sağlama ayarlama](quick-setup-auto-provision.md)
- [Kaynak Yöneticisi şablonu kullanarak otomatik sağlama ayarlama](quick-setup-auto-provision-rm.md)

Ardından cihazınızın cihaz attestation mekanizmasına ve Cihaz Sağlama Hizmeti SDK/dil tercihine uygun "Otomatik sağlama simüle edilmiş bir cihaz" Quickstart ile devam edin. Bu Quickstart'ta, "Aygıt kaydı" ve "Aygıt kaydı ve yapılandırma" aşamalarında niçin geçersiniz: 

|  | Simüle cihaz attestation mekanizması | Quickstart SDK/Dil |  |
|--|--|--|--|
|  | Güvenilir Platform Modülü (TPM) | [C](quick-create-simulated-device.md)<br>[Java](quick-create-simulated-device-tpm-java.md)<br>[C #](quick-create-simulated-device-tpm-csharp.md)<br>[Python](quick-create-simulated-device-tpm-python.md) |  |
|  | X.509 belgesi | [C](quick-create-simulated-device-x509.md)<br>[Java](quick-create-simulated-device-x509-java.md)<br>[C #](quick-create-simulated-device-x509-csharp.md)<br>[Node.js](quick-create-simulated-device-x509-node.md)<br>[Python](quick-create-simulated-device-x509-python.md) |  |




