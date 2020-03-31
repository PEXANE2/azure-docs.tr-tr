---
title: Azure IoT Hub X.509 CA güvenliğine genel bakış | Microsoft Dokümanlar
description: Genel Bakış - X.509 Sertifika Yetkilileri'ni kullanarak aygıtların IoT Hub'ına nasıl doğrulaşacağım.
author: eustacea
manager: arjmands
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/18/2017
ms.author: eustacea
ms.openlocfilehash: 3d02d3573902964a8549fa0eeb1f4f1471de1752
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284518"
---
# <a name="device-authentication-using-x509-ca-certificates"></a>X.509 CA Sertifikaları ile Cihaz Kimlik Doğrulaması

Bu makalede, IoT Hub'a bağlanan aygıtların kimliğini doğrulamak için X.509 Sertifika Yetkilisi (CA) sertifikalarının nasıl kullanılacağı açıklanmaktadır.  Bu makalede öğreneceksiniz:

* X.509 CA sertifikası nasıl alınır?
* X.509 CA sertifikasıi IoT Hub'a nasıl kaydedilir?
* X.509 CA sertifikalarını kullanarak aygıtları imzalama
* X.509 CA ile imzalanan aygıtların kimlik doğrulaması nasıl yapılır?

## <a name="overview"></a>Genel Bakış

X.509 CA özelliği, sertifika yetkilisi (CA) kullanarak aygıt kimlik doğrulamasını IoT Hub'a sağlar. Bu büyük ölçüde ilk cihaz kayıt süreci kolaylaştırır ve tedarik zinciri lojistik cihaz üretimi sırasında. Bu senaryo makalesinde cihaz kimlik doğrulaması için [X.509 CA sertifikalarını kullanmanın değeri hakkında daha fazla bilgi edinin.](iot-hub-x509ca-concept.md)  İzleyen adımların neden var olduğunu açıklarken, devam etmeden önce bu senaryo makalesini okumanızı öneririz.

## <a name="prerequisite"></a>Önkoşul

X.509 CA özelliğini kullanmak için bir IoT Hub hesabınız olması gerekir.  Zaten bir Hub'ınyoksa [Nasıl IoT Hub örneği oluşturabileceğinizi öğrenin.](quickstart-send-telemetry-dotnet.md)

## <a name="how-to-get-an-x509-ca-certificate"></a>X.509 CA sertifikası nasıl alınır?

X.509 CA sertifikası, her aygıtınız için sertifika zincirinin en üstündeyer alınır.  Nasıl kullanmayı planladığınıza bağlı olarak satın alabilir veya bir tane oluşturabilirsiniz.

Üretim ortamı için, ortak köklü bir sertifika yetkilisinden X.509 CA sertifikası satın almanızı öneririz. CA sertifikası satın almak, aygıtlarınızın meşruiyetine kefil olmak için güvenilir bir üçüncü taraf olarak hareket eden kök CA'nın yararınadır. Aygıtlarınızın üçüncü taraf ürün veya hizmetlerle etkileşimkurmalarının beklendiği açık bir IoT ağının parçası olmasını istiyorsanız bu seçeneği göz önünde bulundurun.

Ayrıca deneme veya kapalı IoT ağlarında kullanmak için kendi imzalı bir X.509 CA oluşturabilirsiniz.

X.509 CA sertifikanızı nasıl aldığınızdan bağımsız olarak, ilgili özel anahtarınızı her zaman gizli ve korunmuş olarak sakladığınızdan emin olun.  Bu, X.509 CA kimlik doğrulaması güven oluşturmak için gereklidir.

Bu özellik açıklaması boyunca deneme için kullanabileceğiniz [kendi imzalı bir CA sertifikasını](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md)nasıl oluşturabileceğinizi öğrenin.

## <a name="sign-devices-into-the-certificate-chain-of-trust"></a>Cihazları sertifika güven zincirinde oturum açma

X.509 CA sertifikasının sahibi, son ara CA bir aygıtı imzalayarak bu işlemi sonlandırAna kadar, başka bir ara CA ve benzeri bir ara CA'yı şifreleme olarak imzalayabilir. Sonuç, güven sertifikası zinciri olarak bilinen basamaklı bir sertifika zinciridir. Gerçek hayatta bu, imza cihazlarına yönelik güven delegasyonu olarak ortaya çıkar. Bu delegasyon, kriptografik olarak değişken bir gözaltı zinciri oluşturduğu ve anahtarimzalamayı önlediği için önemlidir.

![img-jenerik-cert-chain-of-güven](./media/generic-cert-chain-of-trust.png)

Aygıt sertifikasının (yaprak sertifikası olarak da adlandırılır) Azure IoT Hub'ına IoT aygıtını kaydederken kullanılan **Aygıt Kimliği'ne** *Konu Adı* ayarlanması gerekir. Bu ayar kimlik doğrulaması için gereklidir.

Cihazları imzalarken nasıl bir [sertifika zinciri oluşturabilirsiniz](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) burada öğrenin.

## <a name="how-to-register-the-x509-ca-certificate-to-iot-hub"></a>X.509 CA sertifikasıi IoT Hub'a nasıl kaydedilir?

X.509 CA sertifikanızı, kayıt ve bağlantı sırasında cihazlarınızın kimliğini doğrulamak için kullanılacak olan IoT Hub'a kaydedin.  X.509 CA sertifikasının kaydedilmesi, sertifika dosyası yüklemeve sahip olma kanıtından oluşan iki aşamalı bir işlemdir.

Yükleme işlemi, sertifikanızı içeren bir dosyayüklemeyi gerektirir.  Bu dosya hiçbir zaman özel anahtar içermemelidir.

Bulundurma adımının kanıtı, siz ve IoT Hub arasında bir şifreleme mücadelesi ve yanıt süreci içerir.  Dijital sertifika içeriğinin herkese açık olduğu ve bu nedenle gizlice dinlemeye açık olduğu göz önüne alındığında, IoT Hub CA sertifikasına gerçekten sahip olduğunuzu tespit etmek ister.  Bunu, CA sertifikasının ilgili özel anahtarıyla imzalamanız gereken rasgele bir meydan okuma oluşturarak yapacaktır.  Eğer özel anahtar gizli tuttu ve daha önce tavsiye olarak korunan, o zaman sadece bu adımı tamamlamak için bilgiye sahip olacaktır. Özel anahtarların gizliliği bu yöntemde güven kaynağıdır.  Meydan okumayı imzaladıktan sonra, sonuçları içeren bir dosya yükleyerek bu adımı tamamlayın.

CA sertifikanızı nasıl [kaydedebilirsiniz](iot-hub-security-x509-get-started.md#register-x509-ca-certificates-to-your-iot-hub) buradan öğrenebilirsiniz

## <a name="how-to-create-a-device-on-iot-hub"></a>IoT Hub'da aygıt oluşturma

Aygıt kimliğe bürünmesini engellemek için, IoT Hub hangi cihazların beklediğini bilmenizi gerektirir.  Bunu, IoT Hub'ın aygıt kayıt defterinde bir aygıt girişi oluşturarak yaparsınız.  Bu işlem, IoT Hub [Aygıt Sağlama Hizmeti](https://azure.microsoft.com/blog/azure-iot-hub-device-provisioning-service-preview-automates-device-connection-configuration/)kullanırken otomatiktir. 

[IoT Hub'da](iot-hub-security-x509-get-started.md#create-an-x509-device-for-your-iot-hub)bir aygıtı el ile nasıl oluşturabilirsiniz' u buradan öğrenin.

IoT hub'ınız için bir X.509 cihazı oluşturma

## <a name="authenticating-devices-signed-with-x509-ca-certificates"></a>X.509 CA sertifikaları yla imzalanmış aygıtların doğrulanması

X.509 CA sertifikası kayıtlı ve aygıtlar güven sertifikası zincirinde oturum açmışken, cihaz ilk kez bağlansa bile cihaz kimlik doğrulaması kalır.  X.509 CA imzalı bir aygıt bağlandığında, onay için sertifika zincirini yükler. Zincir, tüm ara CA ve aygıt sertifikalarını içerir.  Bu bilgilerle, IoT Hub aygıtı iki adımlı bir işlemle doğrular.  IoT Hub, iç tutarlılık için sertifika zincirini şifreleme olarak doğrular ve ardından aygıta bir sahip kanıtı sorunu yayınlar.  IoT Hub, aygıtın gerçek olduğunu, aygıttan gelen başarılı bir sahip kanıtı yanıtına göre beyan eder.  Bu bildirim, aygıtın özel anahtarının korunduğunu ve bu soruna yalnızca aygıtın başarıyla yanıt verebileceğini varsayar.  Özel tuşları korumak için cihazlarda Donanım Güvenli Modülleri (HSM) gibi güvenli yongaların kullanılmasını öneririz.

IoT Hub'a başarılı bir aygıt bağlantısı kimlik doğrulama işlemini tamamlar ve aynı zamanda uygun bir kurulumun göstergesidir.

Bu aygıt [bağlantısı adımLarını](iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates)nasıl tamamlayabilirsiniz burada öğrenin.

## <a name="next-steps"></a>Sonraki Adımlar

[IoT'de X.509 CA kimlik doğrulamasının değeri](iot-hub-x509ca-concept.md) hakkında bilgi edinin.

IoT Hub [Cihaz Sağlama Hizmeti](https://docs.microsoft.com/azure/iot-dps/)ile başlayın.
