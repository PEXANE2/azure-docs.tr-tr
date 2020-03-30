---
title: Akış aşağı aygıtları kimlik doğrulama - Azure IoT Edge | Microsoft Dokümanlar
description: Alt akit aygıtlarının veya yaprak aygıtların IoT Hub'ına nasıl doğruaktarılanması ve bağlantılarını Azure IoT Edge ağ geçidi aygıtları üzerinden yönlendirme.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/13/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b8db3fedc5886e86d5f49739b87b26535665bdbc
ms.sourcegitcommit: 0553a8b2f255184d544ab231b231f45caf7bbbb0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80389333"
---
# <a name="authenticate-a-downstream-device-to-azure-iot-hub"></a>Azure IoT Hub’da bir aşağı akış cihazının kimliğini doğrulama

Saydam ağ geçidi senaryosunda, alt akış aygıtları (bazen yaprak aygıtlar veya alt aygıtlar olarak da adlandırılır) diğer aygıtlar gibi IoT Hub'da kimliklere ihtiyaç duyar. Bu makalede, bir alt akit aygıtının Kimlik Doğrulaması için IoT Hub'a doğrulanması seçenekleri arasında yürür ve ardından ağ geçidi bağlantısının nasıl bildirilen gösteriş gösterir.

Başarılı bir saydam ağ geçidi bağlantısı kurmak için üç genel adım vardır. Bu makale, ikinci adımı kapsar:

1. Ağ geçidi aygıtının akış aşağı aygıtlara güvenli bir şekilde bağlanabilmesi, akış aşağı aygıtlarından iletişim alabilmeleri ve iletileri uygun hedefe yönlendirebilmesi gerekir. Daha fazla bilgi için [bkz.](how-to-create-transparent-gateway.md)
2. **Alt akış aygıtının IoT Hub ile kimlik doğrulaması yapabilmesi ve ağ geçidi aygıtı üzerinden iletişim kurabilmesi için bir aygıt kimliğine sahip olması gerekir.**
3. Akış aşağı aygıtının ağ geçidi aygıtına güvenli bir şekilde bağlanması gerekir. Daha fazla bilgi için [bkz.](how-to-connect-downstream-device.md)

Alt akış aygıtları, ioT Hub ile üç yöntemden birini kullanarak kimlik doğrulaması yapabilir: simetrik anahtarlar (bazen paylaşılan erişim anahtarları olarak da adlandırılır), X.509 kendi imzalı sertifikalar veya X.509 sertifika yetkilisi (CA) imzalı sertifikalar. Kimlik doğrulama adımları, ağ geçidi ilişkisini bildirmek için küçük farklılıklarla IoT Hub ile IoT Edge olmayan aygıtları ayarlamak için kullanılan adımlara benzer.

Bu makaledeki adımlar, Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) ile otomatik olarak sağlama değil, el ile aygıt sağlamayı gösterir. Alt aygıtların DPS ile sağlanması desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

Saydam bir [ağ geçidi olarak hareket etmek için bir IoT Edge aygıtı yapılandırma](how-to-create-transparent-gateway.md)adımlarını tamamlayın. Alt akış aygıtınız için X.509 kimlik doğrulaması kullanıyorsanız, saydam ağ geçidi makalesinde ayarladığınız aynı sertifika oluşturma komut dosyasını kullanmanız gerekir.

Bu makalede, birkaç noktada *ağ geçidi ana bilgisayar adı* anlamına gelir. Ağ geçidi ana bilgisayar adı, IoT Edge ağ geçidi aygıtındaki config.yaml dosyasının **ana bilgisayar** adı parametresinde bildirilir. Aşağı akım aygıtının bağlantı dizesinde adı sürülecek. Ağ geçidi ana bilgisayar adı, DNS veya ana bilgisayar dosya girişi kullanarak bir IP Adresi için çözülebilir olması gerekir.

## <a name="register-device-symmetric-key"></a>Kayıt cihazı (Simetrik anahtar)

Simetrik anahtar kimlik doğrulaması veya paylaşılan erişim anahtarı kimlik doğrulaması, IoT Hub ile kimlik doğrulamanın en basit yoludur. Simetrik anahtar kimlik doğrulamasıyla, base64 tuşu IoT Hub'daki IoT aygıt kimliğinizle ilişkilendirilir. Aygıtınızın IoT Hub'a bağlandığında bu anahtarı sunabilmesi için Bu anahtarı IoT uygulamalarınıza eklersiniz.

### <a name="create-the-device-identity"></a>Aygıt kimliğini oluşturma

Azure portalını, Azure CLI'yi veya Visual Studio Code için IoT uzantısını kullanarak IoT hub'ınıza yeni bir IoT aygıtı ekleyin. Akış aşağı aygıtların IoT Hub'da IoT Edge aygıtları olarak değil, normal IoT aygıtları olarak tanımlanması gerektiğini unutmayın.

Yeni aygıt kimliğini oluşturduğunuzda, aşağıdaki bilgileri sağlayın:

* Cihazınız için bir kimlik oluşturun.

* Kimlik doğrulama türü olarak **Simetrik anahtarı** seçin.

* İsteğe bağlı olarak, **bir üst aygıt ayarlamayı** seçin ve bu akış aşağı aygıtının bağlandığı IoT Edge ağ geçidi aygıtını seçin. Bu adım simetrik anahtar kimlik doğrulaması için isteğe bağlıdır, ancak bir üst aygıt ayarlama aşağı akış aygıtınız için [çevrimdışı özellikleri](offline-capabilities.md) etkinleştirdiği için önerilir. Üst öğeyi daha sonra eklemek veya değiştirmek için aygıt ayrıntılarını her zaman güncelleştirebilirsiniz.

   ![Portalda simetrik anahtar auth ile aygıt kimliği oluşturma](./media/how-to-authenticate-downstream-device/symmetric-key-portal.png)

Aynı işlemi tamamlamak [için Azure CLI için IoT uzantısını](https://github.com/Azure/azure-iot-cli-extension) kullanabilirsiniz. Aşağıdaki örnek, simetrik anahtar kimlik doğrulaması içeren yeni bir IoT aygıtı oluşturur ve bir üst aygıt atar:

```cli
az iot hub device-identity create -n {iothub name} -d {new device ID} --pd {existing gateway device ID}
```

Aygıt oluşturma ve üst/alt yönetimi için Azure CLI komutları hakkında daha fazla bilgi için [az iot hub aygıt kimliği](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) komutları için başvuru içeriğine bakın.


Ardından, [bağlantı dizesini alın ve değiştirin,](#retrieve-and-modify-connection-string) böylece aygıtınızın ağ geçidi üzerinden bağlanmayı bilebilir.

## <a name="register-device-x509-self-signed"></a>Kayıt cihazı (X.509 kendi imzalı)

X.509 kendi imzalı kimlik doğrulaması için, bazen parmak izi kimlik doğrulaması olarak da adlandırılır, IoT aygıtınıza yerleştirmek için yeni sertifikalar oluşturmanız gerekir. Bu sertifikalarda kimlik doğrulaması için IoT Hub ile paylaştığınız bir parmak izi vardır.

X.509 sertifikaları oluşturmak için bir sertifika yetkiniz yoksa, [IoT Edge aygıt özelliklerini test etmek için demo sertifikaları oluşturabilirsiniz.](how-to-create-test-certificates.md) Akış aşağı aygıtınız için test sertifikaları oluştururken, ağ geçidi aygıtınızın sertifikalarını oluşturan kök CA sertifikasını kullanın.

1. CA sertifikanızı kullanarak, akış aşağı aygıtı için iki aygıt sertifikası (birincil ve ikincil) oluşturun.

   Aygıt sertifikası, IoT aygıtını Azure IoT Hub'ına kaydederken kullanacağınız Aygıt Kimliği'ne Konu Adı ayarlamış olmalıdır. Bu ayar kimlik doğrulaması için gereklidir.

2. 40 hexadecimal karakter dizesi olan her sertifikadan SHA1 parmak izini (IoT Hub arabiriminde parmak izi olarak adlandırılır) alın. Sertifikayı görüntülemek ve parmak izini bulmak için aşağıdaki openssl komutunu kullanın:

   ```PowerShell/bash
   openssl x509 -in <primary device certificate>.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

   Bu komutu iki kez, birincil sertifika için bir kez ve ikincil sertifika için bir kez çalıştırın. Kendi imzalı X.509 sertifikalarını kullanarak yeni bir IoT aygıtı kaydettirdiğinizde her iki sertifika için de parmak izi sağlarsınız.

3. Azure portalındaki IoT hub'ınıza gidin ve aşağıdaki değerlere sahip yeni bir IoT aygıt kimliği oluşturun:

   * Aygıt sertifikalarınızın özne adıyla eşleşen **Aygıt Kimliği** sağlayın.
   * Kimlik doğrulama türü olarak **X.509 Kendi Kendine İmzalı'yı** seçin.
   * Cihazınızın birincil ve ikincil sertifikalarından kopyaladığınız hexadecimal dizeleri yapıştırın.
   * **Bir üst aygıt ayarla'yı** seçin ve bu akış aşağı aygıtının bağlandığı IoT Edge ağ geçidi aygıtını seçin. Bir alt aygıt X.509 kimlik doğrulaması için bir üst aygıt gereklidir.

   ![Portalda X.509 kendi imzalı auth ile aygıt kimliği oluşturma](./media/how-to-authenticate-downstream-device/x509-self-signed-portal.png)

4. Aygıt sertifikasını ve alt aygıttaki herhangi bir konumun anahtarlarını kopyalayın. Ayrıca, hem ağ geçidi aygıtı sertifikasını hem de alt akit aygıt sertifikalarını oluşturan paylaşılan kök CA sertifikasının bir kopyasını taşıyın.

   Bu dosyalara IoT Hub'a bağlanan yaprak aygıt uygulamalarında başvurursunuz. Sertifika dosyalarını taşımak için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) gibi bir hizmeti veya Güvenli kopyalama [protokolü](https://www.ssh.com/ssh/scp/) gibi bir işlevi kullanabilirsiniz.

5. Tercih ettiğiniz dile bağlı olarak, X.509 sertifikalarının IoT uygulamalarında nasıl başvurulabileceğine ilişkin örnekleri gözden geçirin:

   * C#: [Azure IoT hub'ınızda X.509 güvenliğini ayarlama](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates)
   * C: [iotedge_downstream_device_sample](https://github.com/Azure/azure-iot-sdk-c/tree/x509_edge_bugbash/iothub_client/samples/iotedge_downstream_device_sample)
   * Düğüm.js: [simple_sample_device_x509.js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js)
   * Java: [SendEventX509.java](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py)
   * Python: [send_message_x509.py](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/advanced-hub-scenarios/send_message_x509.py)

Aynı aygıt oluşturma işlemini tamamlamak [için Azure CLI için IoT uzantısını](https://github.com/Azure/azure-iot-cli-extension) kullanabilirsiniz. Aşağıdaki örnek, X.509 kendi imzalı kimlik doğrulamasını içeren yeni bir IoT aygıtı oluşturur ve bir üst aygıt atar:

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_thumbprint --ptp {primary thumbprint} --stp {secondary thumbprint}
```

Aygıt oluşturma, sertifika oluşturma ve üst ve alt yönetim için Azure CLI komutları hakkında daha fazla bilgi için [az iot hub aygıt kimliği](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) komutları için başvuru içeriğine bakın.

Ardından, [bağlantı dizesini alın ve değiştirin,](#retrieve-and-modify-connection-string) böylece aygıtınızın ağ geçidi üzerinden bağlanmayı bilebilir.

## <a name="register-device-x509-ca-signed"></a>Kayıt cihazı (X.509 CA imzalı)

X.509 sertifika yetkilisi (CA) imzalı kimlik doğrulaması için, IoT Hub'Da kayıtlı ve IoT aygıtınızın sertifikalarını imzalamak için kullandığınız bir kök CA sertifikasına ihtiyacınız vardır. Kök CA sertifikası veya ara sertifikalarından herhangi biri tarafından sorun olan bir sertifikayı kullanan tüm aygıtların kimlik doğrulaması yapmasına izin verilir.

Bu bölüm, [Azure IoT hub'ınızda X.509 güvenliğini ayarla](../iot-hub/iot-hub-security-x509-get-started.md)makalesinde ayrıntılı olarak verilen yönergelere dayanmaktadır. Ağ geçidinden bağlanan bir akış aşağı aygıtı nı ayarlamak için hangi değerlerin kullanılacağını bilmek için bu bölümdeki adımları izleyin.

X.509 sertifikaları oluşturmak için bir sertifika yetkiniz yoksa, [IoT Edge aygıt özelliklerini test etmek için demo sertifikaları oluşturabilirsiniz.](how-to-create-test-certificates.md) Akış aşağı aygıtınız için test sertifikaları oluştururken, ağ geçidi aygıtınızın sertifikalarını oluşturan kök CA sertifikasını kullanın.

1. [Register X.509 CA sertifikalarındaki](../iot-hub/iot-hub-security-x509-get-started.md#register-x509-ca-certificates-to-your-iot-hub) yönergeleri Azure *IoT hub'ınızda X.509*güvenlik ayarlama'nın IoT hub bölümüne uygulayın. Bu bölümde, aşağıdaki adımları gerçekleştirin:

   1. Kök CA sertifikası yükleyin. Demo sertifikalarını kullanıyorsanız, KÖK CA ** \<>/certs/azure-iot-test-only.root.ca.cert.pem**yoludur.

   2. Kök CA sertifikasına sahip olduğunuzu doğrulayın.

2. *Azure IoT hub'ınızda X.509 güvenlik ayarlama'nın* [IoT hub bölümünüz için X.509 oluştur aygıtındaki](../iot-hub/iot-hub-security-x509-get-started.md#create-an-x509-device-for-your-iot-hub) yönergeleri izleyin. Bu bölümde, aşağıdaki adımları gerçekleştirin:

   1. Yeni bir aygıt ekleyin. **Aygıt kimliği**için küçük bir ad sağlayın ve kimlik doğrulama türü **X.509 CA İmzalı'yı**seçin.
   2. Bir üst aygıt ayarlayın. Akış aşağı aygıtlar için **bir üst aygıt ayarla'yı** seçin ve IoT Hub'a bağlantı sağlayacak IoT Edge ağ geçidi aygıtını seçin.

3. Akış aşağı aygıtınız için bir sertifika zinciri oluşturun. Bu zinciri yapmak için IoT Hub'a yüklediğiniz kök CA sertifikasını kullanın. Portalda aygıt kimliğinize vermiş olduğunuz küçük aygıt kimliğini kullanın.

4. Aygıt sertifikasını ve alt aygıttaki herhangi bir konumun anahtarlarını kopyalayın. Ayrıca, hem ağ geçidi aygıtı sertifikasını hem de alt akit aygıt sertifikalarını oluşturan paylaşılan kök CA sertifikasının bir kopyasını taşıyın.

   Bu dosyalara IoT Hub'a bağlanan yaprak aygıt uygulamalarında başvurursunuz. Sertifika dosyalarını taşımak için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) gibi bir hizmeti veya Güvenli kopyalama [protokolü](https://www.ssh.com/ssh/scp/) gibi bir işlevi kullanabilirsiniz.

5. Tercih ettiğiniz dile bağlı olarak, X.509 sertifikalarının IoT uygulamalarında nasıl başvurulabileceğine ilişkin örnekleri gözden geçirin:

   * C#: [Azure IoT hub'ınızda X.509 güvenliğini ayarlama](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates)
   * C: [iotedge_downstream_device_sample.c](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iotedge_downstream_device_sample)
   * Düğüm.js: [simple_sample_device_x509.js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js)
   * Java: [SendEventX509.java](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples/send-event-x509)
   * Python: [send_message_x509.py](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/async-hub-scenarios/send_message_x509.py)

Aynı aygıt oluşturma işlemini tamamlamak [için Azure CLI için IoT uzantısını](https://github.com/Azure/azure-iot-cli-extension) kullanabilirsiniz. Aşağıdaki örnek, X.509 CA imzalı kimlik doğrulaması içeren yeni bir IoT aygıtı oluşturur ve bir üst aygıt atar:

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_ca
```

Daha fazla bilgi [için, az iot hub aygıt kimliği](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) komutları için Azure CLI başvuru içeriğine bakın.

Ardından, [bağlantı dizesini alın ve değiştirin,](#retrieve-and-modify-connection-string) böylece aygıtınızın ağ geçidi üzerinden bağlanmayı bilebilir.

## <a name="retrieve-and-modify-connection-string"></a>Bağlantı dizesini alma ve değiştirme

Portalda bir IoT aygıt kimliği oluşturduktan sonra, birincil veya ikincil anahtarlarını alabilirsiniz. Bu anahtarlardan birinin, uygulamaların IoT Hub ile iletişim kurmak için kullandığı bağlantı dizesine dahil edilmesi gerekir. Simetrik anahtar kimlik doğrulaması için IoT Hub, size kolaylık sağlamak için cihaz ayrıntılarında tam olarak oluşturulmuş bağlantı dizesini sağlar. Bağlantı dizesine ağ geçidi aygıtı hakkında ek bilgi eklemeniz gerekir.

Akış aşağı aygıtları için bağlantı dizeleri aşağıdaki bileşenlere ihtiyaç duyar:

* Aygıtın bağlandığı IoT hub'ı:`Hostname={iothub name}.azure-devices.net`
* Cihaz kimliği hub'a kayıtlıdır:`DeviceID={device ID}`
* Birincil veya ikincil anahtar:`SharedAccessKey={key}`
* Aygıtın bağlandığı ağ geçidi aygıtı. IoT Edge ağ geçidi aygıtının config.yaml dosyasından **ana bilgisayar adı** değerini sağlayın:`GatewayHostName={gateway hostname}`

Hep birlikte, tam bir bağlantı dizesi gibi görünür:

```
HostName=myiothub.azure-devices.net;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz;GatewayHostName=myGatewayDevice
```

Bu akış aşağı aygıt için bir üst/alt ilişkisi kurduysanız, ağ geçidini doğrudan bağlantı ana bilgisayarı olarak çağırarak bağlantı dizesini basitleştirebilirsiniz. X.509 kimlik doğrulaması için üst/alt ilişkileri gereklidir, ancak simetrik anahtar kimlik doğrulaması için isteğe bağlıdır. Örnek:

```
HostName=myGatewayDevice;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz
```

Bu noktada, bir IoT Edge aygıtıkayıtlı ve bir ağ geçidi olarak yapılandırılan olmalıdır. Ayrıca, kayıtlı ve ağ geçidi aygıtını işaret eden bir aşağı IoT aygıtınız da vardır. Son adım, ağ geçidine güvenli bir şekilde bağlanabilmesi için sertifikaları akış aşağı aygıtınıza yerleştirmektir.

Ağ geçidi serisindeki bir sonraki makaleye devam edin, [bir alt akit aygıtı Azure IoT Edge ağ geçidine bağlayın.](how-to-connect-downstream-device.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleyi tamamlayarak, saydam ağ geçidi olarak çalışan bir IoT Edge aygıtına ve bir IoT hub'ına kayıtlı bir akış aşağı aygıtına sahip olmalısınız. Ardından, ağ geçidi aygıtına güvenecek ve güvenli bir şekilde bağlanacak şekilde akış aşağı aygıtlarınızı yapılandırmanız gerekir. Daha fazla bilgi için [bkz.](how-to-connect-downstream-device.md)
