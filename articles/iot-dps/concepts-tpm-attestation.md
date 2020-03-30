---
title: Azure IoT Hub Cihaz Sağlama Hizmeti - TPM Attestation
description: Bu makale, IoT Cihaz Sağlama Hizmeti (DPS) kullanarak TPM attestation akışının kavramsal bir genel görünümünü sağlar.
author: nberdy
ms.author: nberdy
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 624171ffc10a06ac3089b6dceb1683c63c88dbda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975287"
---
# <a name="tpm-attestation"></a>TPM kanıtlama

IoT Hub Aygıt Sağlama Hizmeti, ioT Hub için sıfır dokunmatik aygıt sağlama sağlamasını belirli bir IoT hub'ına yapılandırmak için kullandığınız bir yardımcı hizmettir. Cihaz Sağlama Hizmeti ile milyonlarca cihazı güvenli bir şekilde sağlayabilirsiniz.

Bu makalede, [tpm](./concepts-device.md)kullanırken kimlik attestation işlemi açıklanır. TPM, Güvenilir Platform Modülü anlamına gelir ve bir donanım güvenlik modülü türüdür (HSM). Bu makalede, ayrı bir, firmware veya entegre TPM kullandığınızı varsayar. Yazılım taklit TPM'ler prototip oluşturma veya test etmek için çok uygundur, ancak ayrık, firmware veya entegre TPM'ler ile aynı güvenlik düzeyini sağlamaz. Üretimde yazılım TPM'leri kullanmanızı önermiyoruz. TPM türleri hakkında daha fazla bilgi için [TPM'ye Kısa Bir Giriş bölümüne](https://trustedcomputinggroup.org/wp-content/uploads/TPM-2.0-A-Brief-Introduction.pdf)bakın.

Bu makale, yalnızca HMAC anahtar desteği ve onay anahtarları ile TPM 2.0 kullanan aygıtlar için geçerlidir. Kimlik doğrulaması için X.509 sertifikalarını kullanan aygıtlar için değildir. TPM, Trusted Computing Group'un endüstri çapında, ISO standardıdır ve TPM hakkında [tam TPM 2.0 spec](https://trustedcomputinggroup.org/tpm-library-specification/) veya [ISO/IEC 11889 spec](https://www.iso.org/standard/66510.html)adresinden daha fazla bilgi edinebilirsiniz. Bu makalede, ortak ve özel anahtar çiftleri ve şifreleme için nasıl kullanıldığı nı da bildiğinizi varsayar.

Aygıt Sağlama Hizmeti aygıtı SDK'lar bu makalede açıklanan her şeyi sizin için işler. Aygıtlarınızda SDK kullanıyorsanız ek bir şey uygulamanız gerekmez. Bu makale, cihazınız ın ne zaman ve neden bu kadar güvenli olduğunu TPM güvenlik çipinizde neler olup bittiğini kavramsal olarak anlamanıza yardımcı olur.

## <a name="overview"></a>Genel Bakış

TPM'ler güvenin güvenli kökü olarak onay anahtarı (EK) adı verilen bir şey kullanır. EK TPM'ye özgüdür ve bunu değiştirmek cihazı yeni bir cihaza dönüştürür.

TPM'lerin depolama kök anahtarı (SRK) adı verilen başka bir anahtar türü vardır. Bir SRK, TPM'nin sahipliğini aldıktan sonra TPM'nin sahibi tarafından oluşturulabilir. TPM'nin sahipliğini almak, "birisi HSM'de parola ayarlar" demenin TPM'ye özgü yoludur. Bir TPM aygıtı yeni bir sahibine satılırsa, yeni sahibi yeni bir SRK oluşturmak için TPM'nin sahipliğini alabilir. Yeni SRK nesli, önceki sahibinin TPM'yi kullanamamasını sağlar. SRK TPM'nin sahibine özgü olduğundan, SRK bu sahibi için TPM'nin kendisine veri yapıştırmak için kullanılabilir. SRK, sahibinin anahtarlarını depolaması için bir kum havuzu sağlar ve aygıt veya TPM satılırsa erişim intifa edilebilirliği sağlar. Bu yeni bir eve taşınmak gibi: mülkiyet alarak kapılardaki kilitleri değiştiriyor ve önceki sahipleri (SRK) tarafından bırakılan tüm mobilya yok, ama evin adresini değiştiremezsiniz (EK).

Bir aygıt kurulduktan ve kullanıma hazır hale geldikten sonra, hem EK hem de kullanılabilir bir SRK'ya sahip olacaktır.

![Bir TPM'nin sahipliğini alma](./media/concepts-tpm-attestation/tpm-ownership.png)

TPM'nin sahipliğini alma yla ilgili bir not: Bir TPM'nin sahipliğini almak, TPM üreticisi, kullanılan TPM araçları kümesi ve aygıt işletim sistemi gibi birçok şeye bağlıdır. Sahiplik almak için sisteminizle ilgili yönergeleri izleyin.

Cihaz Sağlama Hizmeti, cihazları tanımlamak ve kaydetmek için EK'nin (EK_pub) ortak bölümünü kullanır. Aygıt satıcısı üretim veya son test sırasında EK_pub okuyabilir ve EK_pub sağlama hizmetine yükleyebilir, böylece aygıt hükme bağlandığında tanınabilir. Aygıt Sağlama Hizmeti SRK'yı veya sahibini denetlemez, bu nedenle TPM'nin "temizlenmesi" müşteri verilerini siler, ancak EK (ve diğer satıcı verileri) korunur ve cihaz, hükme bağlandığında Aygıt Sağlama Hizmeti tarafından tanınmaya devam eder.

## <a name="detailed-attestation-process"></a>Ayrıntılı attestation süreci

TPM'li bir aygıt ilk olarak Aygıt Sağlama Hizmetine bağlandığında, hizmet ilk olarak sağlanan EK_pub kayıt listesinde depolanan EK_pub karşı denetler. EK_pubs eşleşmiyorsa, aygıtın sağlamasına izin verilmez. EK_pubs eşleşirse, hizmet daha sonra aygıtın ek'in özel bölümünün mülkiyetini kimliği kanıtlamak için kullanılan güvenli bir meydan okuma olan nonce mücadelesi ile kanıtlamasını gerektirir. Aygıt Sağlama Hizmeti bir nonce oluşturur ve sonra srk ve daha sonra EK_pub ile şifreler, her ikisi de ilk kayıt arama sırasında cihaz tarafından sağlanır. TPM her zaman EK'nin özel bölümünü güvenli tutar. Bu, sahteciliği önler ve SAS belirteçlerinin yetkili cihazlara güvenli bir şekilde sağlanmasını sağlar.

Attestation işlemini ayrıntılı olarak gözden geçirelim.

### <a name="device-requests-an-iot-hub-assignment"></a>Aygıt bir IoT Hub ataması istiyor

Önce cihaz Aygıt Sağlama Hizmeti'ne bağlanır ve tedarik talep eder. Bunu yaparken, cihaz hizmete kayıt kimliği, kimlik kapsamı ve TPM'den EK_pub ve SRK_pub sağlar. Hizmet, şifrelenmiş nonce'yi aygıta geri geçirir ve aygıtdan nonce'nin şifresini çözmesini ister ve bunu yeniden bağlanmak ve sağlamayı bitirmek için bir SAS belirteci imzalamak için kullanır.

![Cihaz sağlama istekleri](./media/concepts-tpm-attestation/step-one-request-provisioning.png)

### <a name="nonce-challenge"></a>Nonce meydan okuma

Cihaz nonce alır ve TPM içine nonce şifresini çözmek için EK ve SRK özel bölümlerini kullanır; nonce şifreleme temsilcilerinin sırası, değişmez olan EK'ten SRK'ya güvenir ve bu da yeni bir sahibin TPM'nin sahipliğini alması durumunda değişebilir.

![Nonce şifresini çözme](./media/concepts-tpm-attestation/step-two-nonce.png)

### <a name="validate-the-nonce-and-receive-credentials"></a>Nonce'yi doğrulayın ve kimlik bilgilerini alın

Aygıt daha sonra şifresi çözülmüş nonce'yi kullanarak bir SAS belirteci imzalayabilir ve imzalı SAS belirteci kullanarak Aygıt Sağlama Hizmetine yeniden bağlantı kurabilir. Nonce mücadelesi tamamlandığında, hizmet aygıtın sağlanmasına olanak tanır.

![Cihaz, EK sahipliğini doğrulamak için Aygıt Sağlama Hizmetine olan bağlantıyı yeniden kurar](./media/concepts-tpm-attestation/step-three-validation.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık cihaz IoT Hub'a bağlanır ve cihazlarınızın anahtarlarının güvenli bir şekilde depolandığı bilgisi ne kadar güvende olursanız olun. Artık Cihaz Sağlama Hizmeti'nin TPM kullanarak bir aygıtın kimliğini nasıl güvenli bir şekilde doğruladığını bildiğinize göre, daha fazla bilgi edinmek için aşağıdaki makalelere göz atın:

* [Otomatik sağlamadaki tüm kavramlar hakkında bilgi edinin](./concepts-auto-provisioning.md)
* Akışı ele almak için SDK'ları kullanarak otomatik sağlama kullanmaya [başlayın.](./quick-setup-auto-provision.md)
