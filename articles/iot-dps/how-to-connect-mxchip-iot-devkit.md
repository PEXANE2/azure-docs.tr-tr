---
title: IoT Hub ile MXChip IoT DevKit'i kaydetmek için Azure IoT Hub Aygıt Sağlama Hizmeti otomatik sağlama nasıl kullanılır| Microsoft Dokümanlar
description: MXChip IoT DevKit'i IoT Hub'a kaydetmek için Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) otomatik sağlama nasıl kullanılır?
author: liydu
ms.author: liydu
ms.date: 06/25/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: jeffya
ms.openlocfilehash: f05e92f0452b1cfff23e2094354203fd7eaea48b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975661"
---
# <a name="use-azure-iot-hub-device-provisioning-service-auto-provisioning-to-register-the-mxchip-iot-devkit-with-iot-hub"></a>MXChip IoT DevKit'i IoT Hub'a kaydetmek için Azure IoT Hub Aygıt Sağlama Hizmeti otomatik sağlamayı kullanma

Bu makalede, MXChip IoT DevKit'i Azure IoT Hub'ına kaydetmek için Azure IoT Hub Aygıt Sağlama Hizmeti [otomatik sağlama](concepts-auto-provisioning.md)nın nasıl kullanılacağı açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

* Aygıt Sağlama hizmetinin genel bitiş noktasını bir aygıtta yapılandırın.
* X.509 sertifikası oluşturmak için benzersiz bir aygıt sırrı (UDS) kullanın.
* Tek bir aygıtı kaydedin.
* Aygıtın kayıtlı olduğunu doğrulayın.

[MXChip IoT DevKit,](https://aka.ms/iot-devkit) zengin çevre birimleri ve sensörlere sahip hepsi bir e-bir adet Arduino uyumlu bir tahtadır. Visual Studio Code'daki [Azure IoT Aygıt Çalışma Tezgahı](https://aka.ms/iot-workbench) veya [Azure IoT Araçları](https://aka.ms/azure-iot-tools) uzantı paketini kullanarak bunun için geliştirebilirsiniz. DevKit, Azure hizmetlerinden yararlanan prototip Nesnelerin İnterneti (IoT) çözümlerinize rehberlik etmek için büyüyen bir [proje kataloğuyla](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) birlikte gelir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticideki adımları tamamlamak için önce aşağıdaki görevleri yapın:

* DevKit'inizin Wi-Fi'ını yapılandırın ve [Connect IoT DevKit AZ3166'daki](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started#prepare-the-development-environment)"Geliştirme ortamını hazırla" bölümünü buluttaki Azure IoT Hub'ına izleyerek geliştirme ortamınızı hazırlayın.
* [DevKit'i Güncelle'nin verdiği](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) en son firmware'e (1.3.0 veya sonraki) yükseltin.
* [IoT Hub Aygıt Sağlama Hizmetini Azure portalıyla ayarlama](/azure/iot-dps/quick-setup-auto-provision)adımlarını izleyerek bir IoT Hub'ı Aygıt Sağlama hizmeti örneğiyle ilişkilendirin ve bağlayın.

## <a name="open-sample-project"></a>Açık örnek proje

1. IoT DevKit'inizin bilgisayarınıza **bağlı olmadığından** emin olun. Önce VS Kodu'nu başlatın ve ardından DevKit'i bilgisayarınıza bağlayın.

1. Komut `F1` paletini açmak, Azure **IoT Aygıt Çalışma Tezgahı**yazın ve seçin: Örnekleri Aç... seçeneğini tıklatın. Ardından tahta olarak **IoT DevKit'i** seçin.

1. IoT Çalışma Tezgahı Örnekleri sayfasında, **DPS ile Cihaz Kaydı'nı** bulun ve **Örnek**Aç'ı tıklatın. Ardından örnek kodu indirmek için varsayılan yolu seçer.
    ![Açık örnek](media/how-to-connect-mxchip-iot-devkit/open-sample.png)

## <a name="save-a-unique-device-secret-on-device-security-storage"></a>Benzersiz Aygıt Sırrını aygıt güvenlik depolama sına kaydetme

Otomatik sağlama, aygıtın [attestation mekanizmasına](concepts-security.md#attestation-mechanism)dayalı bir aygıt üzerinde yapılandırılabilir. MXChip IoT [DevKit, Güvenilir Bilgi İşlem Grubu'nun](https://trustedcomputinggroup.org)Aygıt Kimlik [KompozisyonU Altyapısını](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) kullanır. DevKit'te STSAFE güvenlik yongasında[(STSAFE-A100)](https://microsoft.github.io/azure-iot-developer-kit/docs/understand-security-chip/)kaydedilen **Benzersiz Aygıt Sırrı** (UDS), aygıtın benzersiz [X.509 sertifikasını](concepts-security.md#x509-certificates)oluşturmak için kullanılır. Sertifika daha sonra Aygıt Sağlama hizmetine kayıt işlemi için ve çalışma zamanında kayıt sırasında kullanılır.

Tipik bir UDS, aşağıdaki örnekte görüldüğü gibi 64 karakterlik bir dizedir:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

DevKit'te bir ÜDS kaydetmek için:

1. VS Code'da, DevKit için COM bağlantı noktasını seçmek için durum çubuğunu tıklatın.
  ![COM Bağlantı Noktasını Seçin](media/how-to-connect-mxchip-iot-devkit/select-com.png)

1. DevKit'te **A tuşuna**basılı tutun, **sıfırlama** düğmesine basın ve bırakın ve ardından **A düğmesini**bırakın. DevKit'iniz yapılandırma moduna girer.

1. Komut `F1` paletini açmak, Azure **IoT Aygıt Çalışma Tezgahı yazın ve seçin: Aygıt Ayarlarını Yapılandır... > Config Benzersiz Aygıt Dizesi (UDS)** seçeneğini tıklatın.
  ![UdS'yi yapılandır](media/how-to-connect-mxchip-iot-devkit/config-uds.png)

1. Oluşturulan UDS dizesini not edin. X.509 sertifikasını oluşturmak için ihtiyacınız olacaktır. Sonra `Enter`basın .
  ![UDS'yi kopyala](media/how-to-connect-mxchip-iot-devkit/copy-uds.png)

1. BILDIRIMden, UDS'nin STSAFE üzerinde başarıyla yapılandırıldığını doğrulayın.
  ![UDS Başarısını Yapılandır](media/how-to-connect-mxchip-iot-devkit/config-uds-success.png)

> [!NOTE]
> Alternatif olarak, Macun gibi yardımcı programları kullanarak UDS'yi seri bağlantı noktası üzerinden yapılandırabilirsiniz. Bunu yapmak için [yapılandırmamodunu kullan'ı](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) izleyin.

## <a name="update-the-global-device-endpoint-and-id-scope"></a>Genel Aygıt Bitiş Noktası ve Kimlik Kapsamını Güncelleştirme

Aygıt kodunda, kiracı yalıtımını sağlamak için [Aygıt sağlama bitiş noktası](/azure/iot-dps/concepts-service#device-provisioning-endpoint) ve kimlik kapsamını belirtmeniz gerekir.

1. Azure portalında, Aygıt Sağlama hizmetinizin **Genel Bakış** bölmesini seçin ve Genel aygıt **bitiş noktası** ve Kimlik **Kapsamı** değerlerini not edin.
  ![Cihaz Sağlama Hizmeti Genel Bitiş Noktası ve Kimlik Kapsamı](media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

1. Açık **DevKitDPS.ino**. Az önce `[Global Device Endpoint]` `[ID Scope]` belirttiğiniz değerleri bulun ve değiştirin.
  ![Cihaz Sağlama Hizmeti Bitiş Noktası](media/how-to-connect-mxchip-iot-devkit/endpoint.png)

1. Koddaki `registrationId` değişkeni doldurun. Yalnızca alfasayısal, küçük ve tire kombinasyonuile en fazla 128 karaktere izin verilir. Ayrıca değeri aşağı kaydetti.
  ![Kayıt Kimliği](media/how-to-connect-mxchip-iot-devkit/registration-id.png)

1. Azure `F1` **IoT Aygıt Çalışma Tezgahı:Yükleme Aygıt Kodu'na**tıklayın ve seçin. Kodu derlemeye ve DevKit'e yüklemeye başlar.
  ![Cihaz Yükleme](media/how-to-connect-mxchip-iot-devkit/device-upload.png)

## <a name="generate-x509-certificate"></a>X.509 sertifikası oluşturma

Bu örnek tarafından kullanılan [attestation mekanizması](/azure/iot-dps/concepts-device#attestation-mechanism) X.509 sertifikasıdır. Oluşturmak için bir yardımcı program kullanmanız gerekir.

1. VS Kodu'nda, terminal penceresini açmak için Yeni Terminalaç'ı tıklatın `F1`ve Yeni **Terminali Aç'ı** seçin.

1. `dps_cert_gen.exe` Klasörde `tool` çalıştırın.

1. Derlenen ikili dosya konumunu `..\.build\DevKitDPS`. Sonra **uds** ve **registrationId** yapıştırın sadece aşağı kaydetti. 
  ![X.509 üretin](media/how-to-connect-mxchip-iot-devkit/gen-x509.png)

1. X.509 `.pem` sertifikası aynı klasörde oluşturur.
  ![X.509 dosyası](media/how-to-connect-mxchip-iot-devkit/pem-file.png)

## <a name="create-a-device-enrollment-entry"></a>Cihaz kaydı girişi oluşturma

1. Azure portalında Cihaz Sağlama Hizmetinizi açın, kayıtları Yönet bölümüne gidin ve **tek tek kayıt ekle'yi**tıklatın.
  ![Tek tek kayıt ekleme](media/how-to-connect-mxchip-iot-devkit/add-enrollment.png)

1. Oluşturulan dosyayı yüklemek için **Birincil Sertifika .pem veya .cer dosyasının** yanındaki dosya simgesini `.pem` tıklatın.
  ![Yükleyin .pem](media/how-to-connect-mxchip-iot-devkit/upload-pem.png)

## <a name="verify-the-devkit-is-registered-with-azure-iot-hub"></a>DevKit'in Azure IoT Hub'ına kayıtlı olduğunu doğrulama

DevKit'inizdeki **Sıfırla** düğmesine basın. **DPS Connected'i görmelisiniz!** DevKit ekranında. Aygıt yeniden başlattıktan sonra aşağıdaki işlemler gerçekleşir:

1. Cihaz, Cihaz Sağlama hizmetinize bir kayıt isteği gönderir.
1. Cihaz Sağlama hizmeti, cihazınızın yanıtladığı bir kayıt sorunu geri gönderir.
1. Başarılı bir kayıt ta, Aygıt Sağlama hizmeti IoT Hub URI'yi, aygıt kimliğini ve şifreli anahtarı aygıta geri gönderir.
1. Aygıttaki IoT Hub istemci uygulaması hub'ınıza bağlanır.
1. Hub'a başarılı bir bağlantı da aygıtın IoT Hub Aygıt Gezgini'nde göründüğünü görürsünüz.
  ![Aygıt kayıtlı](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="problems-and-feedback"></a>Sorunlar ve geri bildirim

Sorunlarla karşılaşırsanız, Iot DevKit [SSS'lerine](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/)bakın veya destek için aşağıdaki kanallara ulaşın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, aygıtın otomatik olarak Azure IoT Hub'ına kaydolabilmesi için Aygıt Kimlik Kompozisyonu Motoru'nu kullanarak bir aygıtı Aygıt Sağlama Hizmetine güvenli bir şekilde kaydetmeyi öğrendiniz. 

Özetle, nasıl öğrendim:

> [!div class="checklist"]
> * Aygıt Sağlama hizmetinin genel bitiş noktasını bir aygıtta yapılandırın.
> * X.509 sertifikası oluşturmak için benzersiz bir aygıt sırrı kullanın.
> * Tek bir aygıtı kaydedin.
> * Aygıtın kayıtlı olduğunu doğrulayın.

Simüle edilmiş bir aygıtı nasıl [oluşturup sağlayabilirsiniz](./quick-create-simulated-device.md)öğrenin.

