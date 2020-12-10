---
title: Saydam ağ geçidi cihazı oluşturma-Azure IoT Edge | Microsoft Docs
description: Azure IoT Edge cihazı, aşağı akış cihazlarından bilgileri işleyebileceği saydam bir ağ geçidi olarak kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/15/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 9f81d059c1a71bf6349d0ef9b4aae8f7a47c161f
ms.sourcegitcommit: dea56e0dd919ad4250dde03c11d5406530c21c28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96938792"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>IoT Edge cihazını saydam ağ geçidi olarak davranacak şekilde yapılandırma

Bu makalede, bir IoT Edge cihazının diğer cihazların IoT Hub iletişim kurması için saydam bir ağ geçidi olarak çalışacak şekilde yapılandırılması için ayrıntılı yönergeler sağlanmaktadır. Bu makalede, saydam bir ağ geçidi olarak yapılandırılmış bir IoT Edge cihazına başvurmak için *IoT Edge ağ geçidi* terimi kullanılmaktadır. Daha fazla bilgi için bkz. [bir IoT Edge cihazının ağ geçidi olarak nasıl kullanılabileceği](./iot-edge-as-gateway.md).

<!-- 1.0.10 -->
::: moniker range="iotedge-2018-06"

>[!NOTE]
>Seçili
>
> * Kenar özellikli cihazlar IoT Edge ağ geçitlerine bağlanamaz.
> * Aşağı akış cihazları karşıya dosya yükleme kullanamaz.

::: moniker-end

<!-- 1.2.0 -->
::: moniker range=">=iotedge-2020-11"

>[!NOTE]
>Seçili
>
> * Aşağı akış cihazları karşıya dosya yükleme kullanamaz.

::: moniker-end

Başarılı bir saydam ağ geçidi bağlantısı kurmak için üç genel adım vardır. Bu makalede ilk adım ele alınmaktadır:

1. **Aşağı akış cihazlarının güvenli bir şekilde bağlanabilmesi için ağ geçidi cihazını sunucu olarak yapılandırın. Ağ geçidini, aşağı akış aygıtlarından ileti alacak şekilde ayarlayın ve bunları uygun hedefe yönlendirin.**
2. IoT Hub ile kimlik doğrulayabilmesi için aşağı akış cihazı için bir cihaz kimliği oluşturun. Ağ Geçidi cihazından ileti göndermek için aşağı akış cihazını yapılandırın. Bu adımlar için bkz. [Azure 'da bir aşağı akış cihazının kimliğini doğrulama IoT Hub](how-to-authenticate-downstream-device.md).
3. Aşağı akış cihazını ağ geçidi cihazına bağlayın ve ileti göndermeye başlayın. Bu adımlar için bkz. bir [aşağı akış cihazını Azure IoT Edge bir ağ geçidine bağlama](how-to-connect-downstream-device.md).

Bir cihazın ağ geçidi olarak davranması için, onun aşağı akış cihazlarına güvenli bir şekilde bağlanması gerekir. Azure IoT Edge, cihazlar arasında güvenli bağlantı kurmak için ortak anahtar altyapısı (PKI) kullanmanıza olanak tanır. Bu durumda, bir aşağı akış cihazının saydam bir ağ geçidi görevi gören bir IoT Edge cihazına bağlanmasına izin veriyoruz. Makul güvenliği korumak için, aşağı akış cihazının ağ geçidi cihazının kimliğini onaylamasını gerekir. Bu kimlik denetimi, cihazlarınızın potansiyel olarak kötü amaçlı ağ geçitlerine bağlanmasını engeller.

Bir aşağı akış cihazı, [Azure IoT Hub](../iot-hub/index.yml) bulut hizmeti ile oluşturulmuş bir kimliği olan herhangi bir uygulama veya platform olabilir. Bu uygulamalar genellikle [Azure IoT cihaz SDK 'sını](../iot-hub/iot-hub-devguide-sdks.md)kullanır. Bir aşağı akış cihazı, IoT Edge ağ geçidi cihazının kendisi üzerinde çalışan bir uygulama bile olabilir. Ancak, bir IoT Edge cihaz IoT Edge bir ağ geçidinin bir aşağı akış olamaz.

Cihaz ağ geçidi topolojiniz için gereken güveni sağlayan herhangi bir sertifika altyapısı oluşturabilirsiniz. Bu makalede, belirli bir IoT Hub (IoT Hub 'ı kök CA) ile ilişkili bir X. 509.952 CA sertifikasını, bu CA ile imzalanmış bir sertifika serisini ve IoT Edge cihazının CA 'sını içeren IoT Hub [x. 509.440 CA güvenliğini](../iot-hub/iot-hub-x509ca-overview.md) etkinleştirmek için kullandığınız sertifika kurulumunu kabul ediyoruz.

>[!NOTE]
>Bu makaleler genelinde kullanılan *kök CA sertifikası* , PKI sertifika zincirinin en üst yetkili ortak sertifikasına başvurur ve bir dağıtılmış sertifika yetkilisinin sertifika kökü değildir. Çoğu durumda, aslında bir ara CA genel sertifikasıdır.

Aşağıdaki adımlar, sertifikaları oluşturma ve bunları ağ geçidine doğru yerlere yükleme sürecinde size yol gösterir. Sertifikaları oluşturmak için herhangi bir makine kullanabilir ve sonra bunları IoT Edge cihazınıza kopyalayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

IoT Edge yüklü bir Linux veya Windows cihazı.

Kullanılabilir bir cihazınız yoksa, bir Azure sanal makinesinde bir tane oluşturabilirsiniz. Bir IoT Hub oluşturmak, bir sanal makine oluşturmak ve IoT Edge çalışma zamanını yapılandırmak için [ilk IoT Edge modülünüzü bir sanal Linux cihazına dağıtma](quickstart-linux.md) bölümündeki adımları izleyin. 

## <a name="set-up-the-device-ca-certificate"></a>Cihaz CA sertifikasını ayarlama

Tüm IoT Edge ağ geçitlerine bir cihaz CA sertifikası yüklenmiş olmalıdır. IoT Edge güvenlik arka plan programı, bir iş yükü CA sertifikasını imzalamak için IoT Edge cihaz CA sertifikasını kullanır, bu da IoT Edge hub 'ı için bir sunucu sertifikasını imzalar. Ağ Geçidi, bağlantının başlatılması sırasında sunucu sertifikasını aşağı akış cihazına sunar. Aşağı akış cihazı, sunucu sertifikasının kök CA sertifikasına toplanan bir Sertifika zincirinin parçası olduğundan emin olmak için kontrol eder. Bu işlem, aşağı akış cihazının ağ geçidinin güvenilir bir kaynaktan geldiğini onaylamasını sağlar. Daha fazla bilgi için bkz. [Azure IoT Edge sertifikaları nasıl kullandığını anlama](iot-edge-certs.md).

![Ağ Geçidi sertifikası kurulumu](./media/how-to-create-transparent-gateway/gateway-setup.png)

Kök CA sertifikası ve cihaz CA sertifikasının (özel anahtarıyla birlikte) IoT Edge ağ geçidi cihazında mevcut olması ve IoT Edge config. YAML dosyasında yapılandırılması gerekir. Bu durumda *kök CA sertifikasında* bu IoT Edge senaryosuna yönelik en üst sertifika yetkilisinin ortalamadığını unutmayın. Ağ Geçidi cihaz CA sertifikası ve aşağı akış cihaz sertifikalarının aynı kök CA sertifikasına toplaması gerekir.

>[!TIP]
>IoT Edge bir cihaza kök CA sertifikası ve cihaz CA sertifikası yükleme işlemi, [bir IoT Edge cihazında sertifikaları yönetme](how-to-manage-device-certificates.md)bölümünde daha ayrıntılı olarak da açıklanmaktadır.

Aşağıdaki dosyaları hazırlayın:

* Kök CA sertifikası
* Cihaz CA sertifikası
* Cihaz CA özel anahtarı

Üretim senaryolarında, bu dosyaları kendi sertifika yetkilinizle oluşturmanız gerekir. Geliştirme ve test senaryoları için tanıtım sertifikaları kullanabilirsiniz.

1. Tanıtım sertifikaları kullanıyorsanız, dosyalarınızı oluşturmak için [IoT Edge cihaz özelliklerini test etmek üzere demo sertifikaları oluşturma](how-to-create-test-certificates.md) bölümündeki yönergeleri kullanın. Bu sayfada aşağıdaki adımları uygulamanız gerekir:

   1. Başlamak için cihazınızda sertifika oluşturmaya yönelik betikleri ayarlayın.
   2. Kök CA sertifikası oluşturun. Bu yönergelerin sonunda, bir kök CA sertifika dosyanız olacaktır:
      * `<path>/certs/azure-iot-test-only.root.ca.cert.pem`.
   3. IoT Edge cihaz CA sertifikaları oluşturun. Bu yönergelerin sonunda, bir cihaz CA sertifikanız ve onun özel anahtarı olacaktır:
      * `<path>/certs/iot-edge-device-<cert name>-full-chain.cert.pem` '
      * `<path>/private/iot-edge-device-<cert name>.key.pem`

2. Sertifikaları farklı bir makinede oluşturduysanız, bunları IoT Edge cihazınıza kopyalayın.

3. IoT Edge cihazınızda güvenlik Daemon yapılandırma dosyasını açın.
   * Pencerelerin `C:\ProgramData\iotedge\config.yaml`
   * 'Un `/etc/iotedge/config.yaml`

4. Dosyanın **sertifika ayarları** bölümünü bulun. Sertifikalarla başlayan dört satırın açıklamasını kaldırın **:** ve aşağıdaki özellikler için üç dosyanıza dosya URI 'lerini değer olarak sağlayın:
   * **device_ca_cert**: cihaz CA sertifikası
   * **device_ca_pk**: cihaz CA özel anahtarı
   * **trusted_ca_certs**: kök CA sertifikası

   **Sertifikalar:** satır üzerinde bir önceki boşluk olmadığından ve diğer satırların iki boşlukla girintilendiğinden emin olun.

5. Dosyayı kaydedin ve kapatın.

6. IoT Edge yeniden başlatın.
   * Pencerelerin `Restart-Service iotedge`
   * 'Un `sudo systemctl restart iotedge`

## <a name="deploy-edgehub-and-route-messages"></a>EdgeHub ve yönlendirme iletileri dağıtma

Aşağı akış cihazları, IoT Edge hub modülünün bilgileri diğer modüllere yönlendirmesinden veya IoT Hub için sorumlu olduğu ağ geçidi cihazına telemetri ve iletiler gönderir. Ağ Geçidi cihazınızı bu işlev için hazırlamak üzere şunları yaptığınızdan emin olun:

* IoT Edge hub modülü cihaza dağıtılır.

  IoT Edge bir cihaza ilk kez yüklediğinizde, otomatik olarak yalnızca bir sistem modülü başlatılır: IoT Edge Aracısı. Bir cihaz için ilk dağıtımı oluşturduktan sonra, ikinci sistem modülü IoT Edge hub 'ı da başlatılır. **Edgehub** modülü cihazınızda çalışmıyorsa, cihazınız için bir dağıtım oluşturun.

* IoT Edge hub modülünün yönlendirmeleri, aşağı akış cihazlarından gelen iletileri işleyecek şekilde ayarlanmıştır.

  Ağ geçidi cihazı, aşağı akış cihazlarındaki iletileri işlemek için bir yola sahip olmalıdır, aksi takdirde bu iletiler işlenmez. İletileri ağ geçidi cihazında modüllere veya doğrudan IoT Hub gönderebilirsiniz.

IoT Edge hub modülünü dağıtmak ve gelen iletileri aşağı akış cihazlarından işlemek üzere yollarla yapılandırmak için aşağıdaki adımları izleyin:

1. Azure portalında IoT Hub'ınıza gidin.

2. **IoT Edge** gidin ve ağ geçidi olarak kullanmak istediğiniz IoT Edge cihazınızı seçin.

3. **Modülleri Ayarlama**'yı seçin.

4. **Modüller** sayfasında, ağ geçidi cihazına dağıtmak istediğiniz modülleri ekleyebilirsiniz. Bu makalenin amaçları doğrultusunda, bu sayfada açıkça ayarlanması gerekmeyen edgeHub modülünü yapılandırmaya ve dağıtmaya odaklanıyoruz.

5. **İleri: rotalar**' ı seçin.

6. **Rotalar** sayfasında, aşağı akış aygıtlarından gelen iletileri işlemek için bir yol olduğundan emin olun. Örnek:

   * Bir modülden veya bir aşağı akış cihazdan IoT Hub için tüm iletileri gönderen bir yol:
       * **Ad**: `allMessagesToHub`
       * **Değer**: `FROM /messages/* INTO $upstream`

   * Tüm aşağı akış cihazlarındaki tüm iletileri IoT Hub 'e gönderen bir yol:
      * **Ad**: `allDownstreamToHub`
      * **Değer**: `FROM /messages/* WHERE NOT IS_DEFINED ($connectionModuleId) INTO $upstream`

      Bu yol, IoT Edge modüllerinden gelen iletilerden farklı olarak, aşağı akış cihazlarındaki iletilerin bu kendileriyle ilişkili bir modül KIMLIĞI olmadığından dolayı işe yarar. Yolun **WHERE** yan tümcesinin kullanılması, bu sistem özelliğine sahip tüm iletileri filtreleyebileceğimizi sağlar.

      İleti yönlendirme hakkında daha fazla bilgi için bkz. [modülleri dağıtma ve yolları oluşturma](./module-composition.md#declare-routes).

7. Yolunuz veya rotalarınız oluşturulduktan sonra, **gözden geçir + oluştur**' u seçin.

8. **Gözden geçir + oluştur** sayfasında **Oluştur**' u seçin.

## <a name="open-ports-on-gateway-device"></a>Ağ Geçidi cihazında bağlantı noktalarını açma

Standart IoT Edge cihazların herhangi bir gelen bağlantısı olması gerekmez, çünkü IoT Hub tüm iletişimler giden bağlantılar aracılığıyla yapılır. Ağ Geçidi cihazları, akış aygıtlarından iletiler alması gerektiğinden farklıdır. Bir güvenlik duvarının aşağı akış cihazları ve ağ geçidi cihazı arasında olması durumunda, iletişimin de güvenlik duvarından alınması gerekir.

Bir ağ geçidi senaryosunun çalışması için, IoT Edge hub 'ın desteklenen protokollerinden en az birinin, aşağı akış cihazlarından gelen trafik için açık olması gerekir. Desteklenen protokoller MQTT, AMQP, HTTPS, WebSockets üzerinden MQTT ve WebSockets üzerinden AMQP 'dir.

| Bağlantı noktası | Protokol |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT + WS <br> AMQP + WS |

## <a name="next-steps"></a>Sonraki adımlar

Artık saydam bir ağ geçidi olarak ayarlanmış bir IoT Edge cihazınız olduğuna göre, aşağı akış cihazlarınızı ağ geçidine güvenecek ve ona ileti gönderecek şekilde yapılandırmanız gerekir. Saydam ağ geçidi senaryonuzu ayarlamanın sonraki adımları için [bir aşağı akış cihazının kimliğini Azure IoT Hub doğrulamak](how-to-authenticate-downstream-device.md) için devam edin.