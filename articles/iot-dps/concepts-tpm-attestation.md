---
title: Azure IoT Hub cihaz sağlama hizmeti-TPM kanıtlama
description: Bu makalede IoT cihaz sağlama hizmeti (DPS) kullanılarak TPM kanıtlama akışına kavramsal bir genel bakış sunulmaktadır.
author: nberdy
ms.author: nberdy
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 624171ffc10a06ac3089b6dceb1683c63c88dbda
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74975287"
---
# <a name="tpm-attestation"></a>TPM kanıtlama

IoT Hub cihaz sağlama hizmeti, belirli bir IoT Hub 'ına sıfır Touch cihaz sağlamayı yapılandırmak için kullandığınız IoT Hub yardımcı hizmettir. Cihaz sağlama hizmeti ile milyonlarca cihazı güvenli bir şekilde sağlayabilirsiniz.

Bu makalede [TPM](./concepts-device.md)kullanılırken kimlik kanıtlama süreci açıklanmaktadır. TPM Güvenilir Platform Modülü temsil eder ve bir donanım güvenlik modülü (HSM) türüdür. Bu makalede ayrı, bellenim veya tümleşik TPM kullandığınız varsayılır. Yazılım öykünmesi, prototip oluşturma veya test etme için uygundur, ancak ayrık, bellenim veya tümleşik TPM 'Ler ile aynı güvenlik düzeyini sağlamalardır. Üretimde yazılım TPMs kullanılması önerilmez. TPMs türleri hakkında daha fazla bilgi için bkz. [TPM 'ye giriş](https://trustedcomputinggroup.org/wp-content/uploads/TPM-2.0-A-Brief-Introduction.pdf).

Bu makale yalnızca HMAC anahtar desteği ve bunların onay anahtarlarıyla TPM 2,0 kullanan cihazlar için geçerlidir. Kimlik doğrulaması için X. 509.440 sertifikalarını kullanan cihazlar için değildir. TPM, Trusted Computing Group bir sektör genelinde, ISO standardıdır ve TPM hakkında daha fazla bilgi edinmek için [tam tpm 2,0 belirtiminde](https://trustedcomputinggroup.org/tpm-library-specification/) veya [ıso/IEC 11889 belirtiminde](https://www.iso.org/standard/66510.html)daha fazla bilgi edinebilirsiniz. Bu makalede ayrıca ortak ve özel anahtar çiftleri hakkında bilgi sahibi olduğunuz ve bunların şifreleme için nasıl kullanıldığı varsayılmaktadır.

Cihaz sağlama hizmeti cihaz SDK 'Ları, bu makalede açıklanan her şeyi sizin için işler. Cihazlarınızda SDK 'Ları kullanıyorsanız ek bir şey uygulamanız gerekmez. Bu makale, cihazınızın sağlamasını yaparken TPM güvenlik yongasında neler olduğunu ve neden güvenli olduğunu anlamanıza yardımcı olur.

## <a name="overview"></a>Genel Bakış

TPMs, güvenli güven kökü olarak onay anahtarı (EK) olarak adlandırılan bir şeyi kullanır. EK TPM için benzersizdir ve bunun değiştirilmesi aslında cihazı yeni bir tane olarak değiştirir.

Depolama kök anahtarı (SRK) adlı, TPMs 'nin sahip olduğu başka bir anahtar türü vardır. TPM 'nin sahipliğini aldıktan sonra TPM 'nin sahibi tarafından bir SRK oluşturulabilir. TPM 'nin sahipliğini alma, "bir Kullanıcı HSM 'de bir parola ayarlıyor" diyen, TPM 'ye özgü bir yoldur. TPM cihazı yeni bir sahibe satılıyorsa yeni sahip yeni bir SRK oluşturmak için TPM 'nin sahipliğini alabilir. Yeni SRK oluşturma, önceki sahibin TPM 'YI kullanmamasını sağlar. SRK TPM 'nin sahibine benzersiz olduğundan, bu sahibe ait verileri TPM 'ye mühürlemek için SRK kullanılabilir. SRK, sahibin anahtarlarını depolaması için bir korumalı alan sağlar ve cihaz ya da TPM satılıyorsa erişim geri alınamaz. Yeni bir evye geçme gibi: Sahiplik alma, kapıların kilitlerini değiştiriyor ve önceki sahipler (SRK) tarafından kalan tüm mobilyaları yok etme, ancak evin (EK) adresini değiştiremezsiniz.

Bir cihaz kurulduktan ve kullanıma hazır olduktan sonra, hem EK hem de bir SRK kullanımı kullanılabilir.

![TPM sahipliğini alma](./media/concepts-tpm-attestation/tpm-ownership.png)

TPM 'nin sahipliğini alma hakkında bir dikkat: TPM 'nin sahipliğini alma, TPM üreticisi, kullanılan TPM araçları kümesi ve cihaz işletim sistemi gibi birçok şeyi kullanır. Sahiplik almak için sisteminizle ilgili yönergeleri izleyin.

Cihaz sağlama hizmeti, cihazları tanımlamak ve kaydetmek için EK (EK_pub) ortak bölümünü kullanır. Cihaz satıcısı, üretim veya son test sırasında EK_pub okuyabilir ve EK_pub sağlama hizmetine yükleyerek cihazın sağlama için bağlandığı zaman tanınabilmesi için bu hizmeti sağlama hizmetine yükleyebilirsiniz. Cihaz sağlama hizmeti SRK veya sahibi denetlemez, bu nedenle TPM, müşteri verilerini siler, ancak EK (ve diğer satıcı verileri) korunur ve cihaz sağlama hizmetine bağlanırken cihaz sağlama hizmeti tarafından yine de tanınacaktır.

## <a name="detailed-attestation-process"></a>Ayrıntılı kanıtlama işlemi

TPM 'ye sahip bir cihaz, cihaz sağlama hizmetine ilk kez bağlanırsa, hizmet önce, kayıt listesinde depolanan EK_pub karşı, belirtilen EK_pub denetler. EK_pubs eşleşmiyorsa, cihazın sağlama izni yoktur. EK_pubs eşleşiyorsa, hizmet daha sonra, kimliği kanıtlamak için kullanılan güvenli bir sınama olan bir kerelik bir sınama aracılığıyla cihazın özel bölümünün sahipliğini kanıtlamasını gerektirir. Cihaz sağlama hizmeti bir kerelik anahtar oluşturur ve sonra, her ikisi de ilk kayıt çağrısı sırasında cihaz tarafından sağlandığı EK_pub SRK ile şifreler. TPM, her zaman EK güvenli ' ın özel bölümünü korur. Bu, sahteciliği önler ve SAS belirteçlerinin yetkili cihazlara güvenli bir şekilde sağlanmasını sağlar.

Ayrıntılı bilgi için kanıtlama sürecini inceleyelim.

### <a name="device-requests-an-iot-hub-assignment"></a>Cihaz IoT Hub atamasını ister

İlk olarak cihaz, cihaz sağlama hizmetine ve sağlama isteklerine bağlanır. Bu durumda cihaz, hizmet kayıt KIMLIĞI, KIMLIK kapsamı ve EK_pub ile TPM 'den SRK_pub sağlar. Hizmet, şifreli nonce 'ı cihaza geri geçirir ve cihazın nonce şifresini çözmesini ve bir SAS belirtecini yeniden bağlamak ve sağlamayı tamamlaması için kullanmasını ister.

![Cihaz istekleri sağlama](./media/concepts-tpm-attestation/step-one-request-provisioning.png)

### <a name="nonce-challenge"></a>Nonce sınaması

Cihaz, nonce 'YI alıp TPM 'ye nonce şifresini çözmek için EK ve SRK 'nin özel kısımlarını kullanır; nonce şifrelemesi sırası, yeni bir sahibin TPM sahipliğini alırsa, bu, sabit olan EK 'ten, bu, sabit olan ve SRK 'ye güven devreder.

![Nonce şifresini çözme](./media/concepts-tpm-attestation/step-two-nonce.png)

### <a name="validate-the-nonce-and-receive-credentials"></a>Nonce 'i doğrulama ve kimlik bilgilerini alma

Cihaz daha sonra, şifresi çözülmüş nonce kullanarak bir SAS belirtecini imzalayabilir ve imzalı SAS belirtecini kullanarak cihaz sağlama hizmetine bir bağlantı yeniden oluşturabilir. Nonce sınaması tamamlandığında, hizmet cihazın sağlamasını sağlar.

![Cihaz EK sahipliğini doğrulamak için cihaz sağlama hizmeti 'ne yeniden bağlantı kurar](./media/concepts-tpm-attestation/step-three-validation.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık cihaz IoT Hub bağlanır ve cihazlarınızın anahtarlarının güvenli bir şekilde saklandığı bilgilerden güvende olursunuz. Cihaz sağlama hizmetinin TPM kullanarak bir cihazın kimliğini güvenli bir şekilde doğrulama işlemini öğrenmiş olduğunuza göre, daha fazla bilgi edinmek için aşağıdaki makalelere göz atın:

* [Otomatik sağlama ile ilgili tüm kavramlar hakkında bilgi edinin](./concepts-auto-provisioning.md)
* Akışı gerçekleştirmek için SDK 'Ları kullanarak [otomatik sağlamayı](./quick-setup-auto-provision.md) kullanmaya başlayın.
