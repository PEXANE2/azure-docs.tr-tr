---
title: IoT Hub ile Mxyonga IoT DevKit 'i kaydettirmek için Azure IoT Hub cihaz sağlama hizmeti 'nin otomatik sağlamasını kullanma | Microsoft Docs
description: Mxyonga IoT DevKit 'i IoT Hub kaydetmek için Azure IoT Hub cihaz sağlama hizmeti 'nin otomatik sağlamasını kullanma.
author: liydu
ms.author: liydu
ms.date: 06/25/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: jeffya
ms.openlocfilehash: b1aac19885e2b640063e4840f047916ad51e9656
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68855739"
---
# <a name="use-azure-iot-hub-device-provisioning-service-auto-provisioning-to-register-the-mxchip-iot-devkit-with-iot-hub"></a>Mxyonga IoT DevKit 'i IoT Hub kaydetmek için Azure IoT Hub cihaz sağlama hizmeti 'ni otomatik sağlama

Bu makalede, Azure IoT Hub ile Mxyonga IoT DevKit 'i kaydettirmek için Azure IoT Hub cihaz sağlama hizmeti [Otomatik sağlama](concepts-auto-provisioning.md)'nın nasıl kullanılacağı açıklanır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Cihazda cihaz sağlama hizmetinin genel uç noktasını yapılandırın.
* Bir X. 509.440 sertifikası oluşturmak için benzersiz bir cihaz gizli dizisi (UDS) kullanın.
* Tek bir cihazı kaydedin.
* Cihazın kayıtlı olduğunu doğrulayın.

[Mxyongaıot DevKit](https://aka.ms/iot-devkit) , zengin çevre birimleri ve sensörlerden oluşan hepsi bir arada bir uyumlu panodır. Visual Studio Code 'de [Azure IoT cihaz çalışma ekranı](https://aka.ms/iot-workbench) veya [Azure IoT araçları](https://aka.ms/azure-iot-tools) Uzantı paketi ' ni kullanarak bu BT için geliştirme yapabilirsiniz. DevKit, Azure hizmetlerinden yararlanan prototip Nesnelerin İnterneti (IoT) çözümlerine kılavuzluk eden büyüyen bir [Proje kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) ile birlikte gelir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticideki adımları tamamlayabilmeniz için önce aşağıdaki görevleri yapın:

* DevKit 'in Wi-Fi 'nizi yapılandırın ve [IoT DEVKIT AZ3166 'ı Bulutta Azure IoT Hub bağlama](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started#prepare-the-development-environment)bölümündeki "geliştirme ortamını hazırlama" bölümündeki adımları izleyerek geliştirme ortamınızı hazırlayın.
* [Güncelleştirme DevKit bellenim](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) öğreticisi ile en son üretici yazılımına (1.3.0 veya üzeri) yükseltin.
* [IoT Hub cihaz sağlama hizmeti 'ni Azure Portal ayarlama](/azure/iot-dps/quick-setup-auto-provision)bölümündeki adımları Izleyerek bir cihaz sağlama hizmeti örneği ile bir IoT Hub oluşturun ve bağlayın.

## <a name="open-sample-project"></a>Örnek projeyi aç

1. IoT DevKit 'in bilgisayarınıza **bağlı** olmadığından emin olun. Önce VS Code başlatın ve ardından DevKit 'i bilgisayarınıza bağlayın.

1. Komut `F1` paletini açmak için tıklayın, Azure IoT cihaz çalışma **ekranı ' nı yazın ve seçin: Örnekleri aç...** . Ardından, pano olarak **IoT DevKit** ' i seçin.

1. IoT çalışma ekranı örnekleri sayfasında, **DPS Ile cihaz kaydı** bulun ve **örnek aç**' a tıklayın. Ardından örnek kodu indirmek için varsayılan yolu seçer.
    ![Örnek Aç](media/how-to-connect-mxchip-iot-devkit/open-sample.png)

## <a name="save-a-unique-device-secret-on-device-security-storage"></a>Cihaz güvenlik depolaması 'nda benzersiz bir cihaz gizli anahtarı kaydetme

Otomatik sağlama, cihazın [kanıtlama mekanizmasına](concepts-security.md#attestation-mechanism)bağlı olarak bir cihazda yapılandırılabilir. Mxyongaıot DevKit, [Trusted Computing Group](https://trustedcomputinggroup.org) [cihaz kimliği bileşim altyapısını](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) kullanır. DevKit 'teki STSAFE güvenlik yongasında ([stsafe-A100](https://microsoft.github.io/azure-iot-developer-kit/docs/understand-security-chip/)) kaydedilmiş **benzersiz bir cihaz gizli** dizisi (UDS), cihazın benzersiz [X. 509.440 sertifikasını](concepts-security.md#x509-certificates)oluşturmak için kullanılır. Sertifika, daha sonra cihaz sağlama hizmetindeki kayıt işlemi için ve çalışma zamanında kayıt sırasında kullanılır.

Tipik bir UDS, aşağıdaki örnekte görüldüğü gibi 64 karakter dizesidir:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Bir UDS 'yi DevKit 'e kaydetmek için:

1. VS Code, DevKit için COM bağlantı noktasını seçmek üzere durum çubuğuna tıklayın.
  ![COM bağlantı noktası seçin](media/how-to-connect-mxchip-iot-devkit/select-com.png)

1. DevKit üzerinde, **düğme a**' yı basılı tutun, **Sıfırla** düğmesini gönderin ve sonra **A düğmesine**basın. DevKit yapılandırma moduna girer.

1. Komut `F1` paletini açmak için tıklayın, Azure IoT cihaz çalışma **ekranı ' nı yazın ve seçin: Cihaz ayarlarını yapılandır... > Config benzersiz cihaz dizesi (UDS)** .
  ![UDS 'yi yapılandırma](media/how-to-connect-mxchip-iot-devkit/config-uds.png)

1. Oluşturulan UDS dizesini aklınızda edin. X. 509.440 sertifikasını oluşturmak için buna ihtiyacınız olacak. Ardından tuşuna `Enter`basın.
  ![UDS 'yi Kopyala](media/how-to-connect-mxchip-iot-devkit/copy-uds.png)

1. UıDS 'nin STSAFE üzerinde başarıyla yapılandırıldığına ilişkin bildirimden emin olun.
  ![UıDS yapılandırma başarısı](media/how-to-connect-mxchip-iot-devkit/config-uds-success.png)

> [!NOTE]
> Alternatif olarak, PuTTY gibi yardımcı programları kullanarak seri bağlantı noktası aracılığıyla UDS 'leri yapılandırabilirsiniz. Bunu yapmak için [yapılandırma modunu kullanın](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) .

## <a name="update-the-global-device-endpoint-and-id-scope"></a>Genel cihaz uç noktası ve KIMLIK kapsamını güncelleştirme

Cihaz kodunda, kiracı yalıtımının sağlanması için [cihaz sağlama uç noktası](/azure/iot-dps/concepts-service#device-provisioning-endpoint) ve kimlik kapsamı belirtmeniz gerekir.

1. Azure portal, cihaz sağlama hizmetinizin **genel bakış** bölmesini seçin ve **genel cihaz uç noktası** ve **kimlik kapsamı** değerlerini göz önünde edin.
  ![Cihaz sağlama hizmeti genel uç noktası ve KIMLIK kapsamı](media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

1. **Devkitdps. ino**öğesini açın. Yeni not ettiğiniz `[Global Device Endpoint]` değerleri `[ID Scope]` bulun ve değiştirin.
  ![Cihaz sağlama hizmeti uç noktası](media/how-to-connect-mxchip-iot-devkit/endpoint.png)

1. `registrationId` Koddaki değişkeni doldurur. En fazla 128 karakter içeren alfasayısal, küçük harf ve kısa çizgi birleşimine izin verilir. Değer aşağı de belirtilmiştir.
  ![Kayıt KIMLIĞI](media/how-to-connect-mxchip-iot-devkit/registration-id.png)

1. Tıklayın `F1`, yazın ve Azure **IoT cihaz çalışma ekranı ' nı seçin: Cihaz kodunu**karşıya yükleyin. Kodu derleme ve DevKit 'e yüklemeye başlar.
  ![Cihaz karşıya yükleme](media/how-to-connect-mxchip-iot-devkit/device-upload.png)

## <a name="generate-x509-certificate"></a>X. 509.440 sertifikası oluştur

Bu örnek tarafından kullanılan [kanıtlama mekanizması](/azure/iot-dps/concepts-device#attestation-mechanism) X. 509.440 sertifikasıdır. Oluşturmak için bir yardımcı program kullanmanız gerekir.

> [!NOTE]
> X. 509.440 sertifika Oluşturucusu yalnızca Windows 'ı destekliyor.

1. VS Code ' de, `F1`tıklayın, yazın ve **yeni terminali aç** ' ı seçerek Terminal penceresini açın.

1. Klasöründe Çalıştır `dps_cert_gen.exe`. `tool`

1. Derlenmiş ikili dosya konumunu olarak `..\.build\DevKitDPS`belirtin. Ardından, az önce not ettiğiniz **UIDs** ve **RegistrationId** değerlerini yapıştırın. 
  ![X. 509.440 oluştur](media/how-to-connect-mxchip-iot-devkit/gen-x509.png)

1. `.pem` X. 509.440 sertifikası aynı klasörde oluşturulur.
  ![X. 509.440 dosyası](media/how-to-connect-mxchip-iot-devkit/pem-file.png)

## <a name="create-a-device-enrollment-entry"></a>Cihaz kaydı girişi oluşturma

1. Azure portal, cihaz sağlama hizmetinizi açın, kayıtları Yönet bölümüne gidin ve **bireysel kayıt Ekle**' ye tıklayın.
  ![Bireysel kayıt Ekle](media/how-to-connect-mxchip-iot-devkit/add-enrollment.png)

1. Oluşturulan `.pem` dosyayı karşıya yüklemek için **birincil sertifika. pek veya. cer dosyasının** yanındaki dosya simgesine tıklayın.
  ![Karşıya yükle. pek](media/how-to-connect-mxchip-iot-devkit/upload-pem.png)

## <a name="verify-the-devkit-is-registered-with-azure-iot-hub"></a>DevKit 'in Azure IoT Hub kayıtlı olduğunu doğrulayın

DevKit 'teki **Sıfırla** düğmesine basın. **DPS bağlı** ' i görmeniz gerekir! DevKit ekranında. Cihaz yeniden başlatıldıktan sonra aşağıdaki eylemler gerçekleşir:

1. Cihaz, Cihaz Sağlama hizmetinize bir kayıt isteği gönderir.
1. Cihaz sağlama hizmeti, cihazınızın yanıt verdiği bir kayıt sınamasını geri gönderir.
1. Kayıt başarılı olduğunda, cihaz sağlama hizmeti IoT Hub URI 'sini, cihaz KIMLIĞINI ve şifreli anahtarı cihaza geri gönderir.
1. Cihazdaki IoT Hub istemci uygulaması hub 'ınıza bağlanır.
1. Hub 'a başarıyla bağlantı için, cihazın IoT Hub Device Explorer göründüğünü görürsünüz.
  ![Cihaz kaydedildi](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="problems-and-feedback"></a>Sorunlar ve geri bildirim

Sorunlarla karşılaşırsanız IoT DevKit [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/)bölümüne başvurun veya destek için aşağıdaki kanallara ulaşın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide cihaz kimlik oluşturma altyapısını kullanarak cihazı cihaz sağlama hizmetine güvenli bir şekilde kaydetmeyi öğrendiniz, böylece cihaz Azure IoT Hub otomatik olarak kaydolabilir. 

Özet bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Cihazda cihaz sağlama hizmetinin genel uç noktasını yapılandırın.
> * X. 509.440 sertifikası oluşturmak için benzersiz bir cihaz gizli anahtarı kullanın.
> * Tek bir cihazı kaydedin.
> * Cihazın kayıtlı olduğunu doğrulayın.

[Sanal cihaz oluşturmayı ve sağlamayı](./quick-create-simulated-device.md)öğrenin.

