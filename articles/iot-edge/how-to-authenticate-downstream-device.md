---
title: Aşağı akış cihazların kimliğini doğrulama-Azure IoT Edge | Microsoft Docs
description: IoT Hub için aşağı akış cihazlarının veya yaprak cihazların kimliğini doğrulama ve bağlantı Azure IoT Edge ağ geçidi cihazları aracılığıyla yönlendirme.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/02/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 3ccb8d29d0ec52c31913a43358c7daa1c0693df7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84308855"
---
# <a name="authenticate-a-downstream-device-to-azure-iot-hub"></a>Azure IoT Hub’da bir aşağı akış cihazının kimliğini doğrulama

Saydam bir ağ geçidi senaryosunda, aşağı akış cihazları (bazen yaprak cihazlar veya alt cihazlar olarak adlandırılır), diğer tüm cihazlar gibi IoT Hub kimliklere ihtiyaç duyar. Bu makalede, IoT Hub için bir aşağı akış cihazının kimliğini doğrulama seçenekleri anlatılır ve sonra Ağ Geçidi bağlantısının nasıl bildirildiği gösterilmektedir.

Başarılı bir saydam ağ geçidi bağlantısı kurmak için üç genel adım vardır. Bu makalede ikinci adım ele alınmaktadır:

1. Aşağı akış cihazlarının güvenli bir şekilde bağlanabilmesi için ağ geçidi cihazını sunucu olarak yapılandırın. Ağ geçidini, aşağı akış aygıtlarından ileti alacak şekilde ayarlayın ve bunları uygun hedefe yönlendirin. Daha fazla bilgi için bkz. bir [IoT Edge cihazını saydam bir ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md).
2. **IoT Hub ile kimlik doğrulayabilmesi için aşağı akış cihazı için bir cihaz kimliği oluşturun. Ağ Geçidi cihazından ileti göndermek için aşağı akış cihazını yapılandırın.**
3. Aşağı akış cihazını ağ geçidi cihazına bağlayın ve ileti göndermeye başlayın. Daha fazla bilgi için bkz. bir [aşağı akış cihazını Azure IoT Edge bir ağ geçidine bağlama](how-to-connect-downstream-device.md).

Aşağı akış cihazları şu üç yöntemden birini kullanarak IoT Hub kimlik doğrulaması yapabilir: simetrik anahtarlar (bazen paylaşılan erişim anahtarları olarak adlandırılır), X. 509.440 otomatik olarak imzalanan sertifikalar veya X. 509.440 sertifika yetkilisi (CA) imzalı sertifikalar. Kimlik doğrulama adımları, ağ geçidi ilişkisini bildirmek için küçük farklılıklar ile, IoT Edge olmayan bir cihazı IoT Hub kurmak için kullanılan adımlara benzerdir.

Bu makaledeki adımlarda el ile cihaz sağlama gösterilmektedir. Azure IoT Hub cihaz sağlama hizmeti (DPS) ile otomatik olarak aşağı akış cihazları sağlama desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

[Bir IoT Edge cihazını, saydam bir ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md)adımlarını uygulayın.

X. 509.952 kimlik doğrulaması kullanıyorsanız, aşağı akış cihazınız için sertifikalar oluşturacaksınız. Aynı kök CA sertifikasına ve bir kez daha kullanılabilir saydam ağ geçidi makalesi için kullandığınız betiği üreten sertifikaya sahip olursunuz.

Bu makale, *ağ geçidi ana bilgisayar adına* birkaç noktada başvurur. Ağ geçidi ana bilgisayar adı, IoT Edge ağ geçidi cihazında config. YAML dosyasının **hostname** parametresinde bildirilmiştir. Bu, aşağı akış cihazının bağlantı dizesinde başvurulur. Ağ geçidi ana bilgisayar adının, DNS veya aşağı akış cihazında bir konak dosyası girişi kullanılarak bir IP adresi ile çözümlenebilmelidir.

## <a name="register-device-with-iot-hub"></a>Cihazı IoT Hub Kaydet

Aşağı akış cihazınızın IoT Hub kimlik doğrulamasını nasıl istediğinizi seçin:

* [Simetrik anahtar kimlik doğrulaması](#symmetric-key-authentication): IoT Hub, aşağı akış cihazına yerleştirdiğiniz bir anahtar oluşturur. Cihazın kimlik doğrulaması yapıldığında, IoT Hub iki anahtarın eşleşip eşleşmediğini denetler. Simetrik anahtar kimlik doğrulaması kullanmak için ek sertifikalar oluşturmanız gerekmez.
* [X. 509.440 otomatik olarak imzalanan kimlik doğrulaması](#x509-self-signed-authentication): parmak izini, cihazın X. 509.440 sertifikasından IoT Hub ile paylaştığınız için bazen parmak izi kimlik doğrulaması olarak adlandırılır.
* [X. 509.440 CA-imzalı kimlik doğrulaması](#x509-ca-signed-authentication): IoT Hub IÇIN kök CA sertifikasını karşıya yükleyin. Cihazlar, kimlik doğrulaması için X. 509.440 sertifikasını sunmuşsa, IoT Hub aynı kök CA sertifikası tarafından imzalanan bir güven zincirine ait olduğunu denetler.

Cihazınızı bu üç yöntemden biriyle kaydettikten sonra, aşağı akış cihazınız için [bağlantı dizesini almak ve değiştirmek](#retrieve-and-modify-connection-string) üzere bir sonraki bölüme geçin.

### <a name="symmetric-key-authentication"></a>Simetrik anahtar kimlik doğrulaması

Simetrik anahtar kimlik doğrulaması veya paylaşılan erişim anahtarı kimlik doğrulaması, IoT Hub kimlik doğrulamasının en kolay yoludur. Simetrik anahtar kimlik doğrulamasıyla, IoT Hub ' deki IoT cihaz KIMLIĞINIZLE bir Base64 anahtarı ilişkilendirilir. Cihazınızın IoT Hub bağlandığı sırada sunabilmesi için bu anahtarı IoT uygulamalarınıza dahil edersiniz.

Visual Studio Code için Azure portal, Azure CLı veya IoT uzantısını kullanarak IoT Hub 'ınıza yeni bir IoT cihazı ekleyin. Aşağı akış cihazlarının, IoT Edge cihazlar değil, normal IoT cihazları olarak IoT Hub tanımlanması gerektiğini unutmayın.

Yeni cihaz kimliğini oluşturduğunuzda, aşağıdaki bilgileri sağlayın:

* Cihazınız için bir KIMLIK oluşturun.

* Kimlik doğrulama türü olarak **simetrik anahtar** ' ı seçin.

* İsteğe bağlı olarak, **bir üst cihaz ayarlamayı** seçin ve bu aşağı akış cihazının bağlanacağı IoT Edge ağ geçidi cihazını seçin. Bu adım simetrik anahtar kimlik doğrulaması için isteğe bağlıdır, ancak bir üst cihaz ayarlandığında, aşağı akış cihazınız için [çevrimdışı yetenekler](offline-capabilities.md) sağladığından önerilir. Üst öğeyi daha sonra eklemek veya değiştirmek için cihaz ayrıntılarını her zaman güncelleştirebilirsiniz.

   ![Portalda simetrik anahtar kimlik doğrulaması ile cihaz KIMLIĞI oluşturma](./media/how-to-authenticate-downstream-device/symmetric-key-portal.png)

Aynı işlemi gerçekleştirmek için [Azure CLI Için IoT uzantısını](https://github.com/Azure/azure-iot-cli-extension) da kullanabilirsiniz. Aşağıdaki örnek, simetrik anahtar kimlik doğrulaması ile yeni bir IoT cihazı oluşturur ve bir üst cihaz atar:

```cli
az iot hub device-identity create -n {iothub name} -d {new device ID} --pd {existing gateway device ID}
```

Cihaz oluşturma ve üst/alt yönetim için Azure CLı komutları hakkında daha fazla bilgi için bkz. [az IoT Hub cihaz kimliği](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/hub/device-identity?view=azure-cli-latest) komutları için başvuru içeriği.

Sonra, cihazınızın ağ geçidi aracılığıyla bağlanmayı bilmesi için [bağlantı dizesini alın ve değiştirin](#retrieve-and-modify-connection-string) .

### <a name="x509-self-signed-authentication"></a>X. 509.440 kendinden imzalı kimlik doğrulaması

X. 509.440 otomatik imzalı kimlik doğrulaması için bazen parmak izi kimlik doğrulaması olarak da adlandırılan, aşağı akış cihazınıza yerleştirilecek sertifikalar oluşturmanız gerekir. Bu sertifikaların kimlik doğrulaması için IoT Hub paylaştığınız bir parmak izi vardır.

1. CA sertifikanızı kullanarak, aşağı akış cihazı için iki cihaz sertifikası (birincil ve ikincil) oluşturun.

   X. 509.952 sertifikaları oluşturmak için bir sertifika yetkiliniz yoksa, IoT Edge tanıtım sertifikası betiklerini kullanarak [aşağı akış cihaz sertifikaları oluşturabilirsiniz](how-to-create-test-certificates.md#create-downstream-device-certificates). Otomatik olarak imzalanan sertifikalar oluşturma adımlarını izleyin. Ağ Geçidi cihazınız için sertifikaları oluşturan aynı kök CA sertifikasını kullanın.

   Kendi sertifikalarınızı oluşturursanız, cihaz sertifikasının konu adının Azure IoT Hub IoT cihazını kaydederken kullandığınız cihaz KIMLIĞINE ayarlandığından emin olun. Bu ayar kimlik doğrulaması için gereklidir.

2. 40 onaltılık bir karakter dizesi olan her bir sertifikadan SHA1 parmak izini (IoT Hub arabiriminde parmak izi denir) alın. Sertifikayı görüntülemek ve parmak izini bulmak için aşağıdaki OpenSSL komutunu kullanın:

   * Windows:

     ```PowerShell
     openssl x509 -in <path to primary device certificate>.cert.pem -text -fingerprint
     ```

   * Linux:

     ```Bash
     openssl x509 -in <path to primary device certificate>.cert.pem -text -fingerprint | sed 's/[:]//g'
     ```

   Birincil sertifika ve ikincil sertifika için bir kez olmak üzere bu komutu iki kez çalıştırın. Otomatik olarak imzalanan X. 509.440 sertifikalarını kullanarak yeni bir IoT cihazını kaydettiğinizde her iki sertifika için de parmak izleri sağlarsınız.

3. Azure portal IoT Hub 'ınıza gidin ve aşağıdaki değerlerle yeni bir IoT cihaz kimliği oluşturun:

   * Cihaz sertifikalarınızın konu adıyla eşleşen **CIHAZ kimliğini** sağlayın.
   * Kimlik doğrulama türü olarak **X. 509.440 otomatik olarak imzalanan** ' ı seçin.
   * Cihazınızın birincil ve ikincil sertifikalarından kopyaladığınız onaltılık dizeleri yapıştırın.
   * **Bir üst cihaz ayarla** ' yı seçin ve bu aşağı akış cihazının bağlanacağı IoT Edge ağ geçidi cihazını seçin. Bir aşağı akış cihazının X. 509.440 kimlik doğrulaması için bir üst cihaz gereklidir.

   ![Portalda X. 509.440 otomatik imzalı kimlik doğrulaması ile cihaz KIMLIĞI oluşturma](./media/how-to-authenticate-downstream-device/x509-self-signed-portal.png)

4. Hem birincil hem de ikincil cihaz sertifikalarını ve bunların anahtarlarını aşağı akış cihazında herhangi bir konuma kopyalayın. Ayrıca, ağ geçidi cihaz sertifikasını ve aşağı akış cihaz sertifikalarını oluşturan paylaşılan kök CA sertifikasının bir kopyasını da taşıyın.

   IoT Hub bağlanan aşağı akış cihazında bulunan uygulamalarda bu sertifika dosyalarına başvurabileceksiniz. Sertifika dosyalarını taşımak için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) veya [Güvenli kopya Protokolü](https://www.ssh.com/ssh/scp/) gibi bir işlev gibi bir hizmet kullanabilirsiniz.

5. Tercih ettiğiniz dile bağlı olarak, X. 509.440 sertifikalarına IoT uygulamalarında nasıl başvurulabilen örnekleri gözden geçirin:

   * C#: [Azure IoT Hub 'ınızda X. 509.440 güvenliğini ayarlama](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates)
   * C: [iotedge_downstream_device_sample. c](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iotedge_downstream_device_sample)
   * Node.js: [simple_sample_device_x509.js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js)
   * Java: [SendEventX509. Java](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples/send-event-x509)
   * Python: [send_message_x509. Kopyala](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/async-hub-scenarios/send_message_x509.py)

Aynı cihaz oluşturma işlemini gerçekleştirmek için [Azure CLI Için IoT uzantısını](https://github.com/Azure/azure-iot-cli-extension) da kullanabilirsiniz. Aşağıdaki örnek, X. 509.440 otomatik imzalı kimlik doğrulaması ile yeni bir IoT cihazı oluşturur ve bir üst cihaz atar:

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_thumbprint --ptp {primary thumbprint} --stp {secondary thumbprint}
```

Cihaz oluşturma, sertifika oluşturma ve üst ve alt yönetim için Azure CLı komutları hakkında daha fazla bilgi için bkz. [az IoT Hub cihaz kimliği](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/hub/device-identity?view=azure-cli-latest) komutları için başvuru içeriği.

Sonra, cihazınızın ağ geçidi aracılığıyla bağlanmayı bilmesi için [bağlantı dizesini alın ve değiştirin](#retrieve-and-modify-connection-string) .

### <a name="x509-ca-signed-authentication"></a>X. 509.440 CA-imzalı kimlik doğrulaması

X. 509.440 sertifika yetkilisi (CA) imzalı kimlik doğrulaması için, aşağı akış cihazınız için sertifikaları imzalamak üzere kullandığınız IoT Hub kayıtlı bir kök CA sertifikasına ihtiyacınız vardır. Kök CA sertifikası veya ara sertifikalarının herhangi biri tarafından karşılaşılan bir sertifikayı kullanan herhangi bir cihazın kimlik doğrulamasına izin verilir.

Bu bölüm, [Azure IoT Hub 'ınızda X. 509.440 güvenliğini ayarlama](../iot-hub/iot-hub-security-x509-get-started.md)IoT Hub makalesinde açıklanan yönergeleri temel alır.

1. CA sertifikanızı kullanarak, aşağı akış cihazı için iki cihaz sertifikası (birincil ve ikincil) oluşturun.

   X. 509.952 sertifikaları oluşturmak için bir sertifika yetkiliniz yoksa, IoT Edge tanıtım sertifikası betiklerini kullanarak [aşağı akış cihaz sertifikaları oluşturabilirsiniz](how-to-create-test-certificates.md#create-downstream-device-certificates). CA imzalı sertifikalar oluşturma adımlarını izleyin. Ağ Geçidi cihazınız için sertifikaları oluşturan aynı kök CA sertifikasını kullanın.

2. *Azure IoT Hub 'ınızda x. 509.952 güvenliğini ayarlama*konusunun [IoT Hub 'ınıza x. 509.952 CA sertifikalarını kaydetme](../iot-hub/iot-hub-security-x509-get-started.md#register-x509-ca-certificates-to-your-iot-hub) bölümündeki yönergeleri izleyin. Bu bölümde, aşağıdaki adımları gerçekleştirirsiniz:

   1. Kök CA sertifikasını karşıya yükleyin. Tanıtım sertifikalarını kullanıyorsanız kök CA ** \<path> /certs/Azure-iot-test-only.root.ca.cert.pem**olur.

   2. Bu kök CA sertifikasına sahip olduğunuzu doğrulayın.

3. *Azure IoT Hub 'ınızda x. 509.440 güvenliği ayarlama*konusunun [IoT Hub 'ınız için x. 509.952 cihazı oluşturma](../iot-hub/iot-hub-security-x509-get-started.md#create-an-x509-device-for-your-iot-hub) bölümündeki yönergeleri izleyin. Bu bölümde, aşağıdaki adımları gerçekleştirirsiniz:

   1. Yeni bir cihaz ekleyin. **CIHAZ kimliği**için küçük harfli bir ad girin ve **X. 509.440 CA imzalı**kimlik doğrulama türünü seçin.

   2. Bir üst cihaz ayarlayın. Aşağı akış cihazları için **bir üst cihaz ayarla** ' yı seçin ve IoT Hub bağlantısını sağlayacak IoT Edge ağ geçidi cihazını seçin.

4. Aşağı akış cihazınız için bir sertifika zinciri oluşturun. IoT Hub için karşıya yüklediğiniz aynı kök CA sertifikasını kullanın ve bu zinciri oluşturun. Portalda cihaz kimliğinize verdiğiniz küçük harfli cihaz KIMLIĞINI kullanın.

5. Cihaz sertifikasını ve anahtarlarını aşağı akış cihazında herhangi bir konuma kopyalayın. Ayrıca, ağ geçidi cihaz sertifikasını ve aşağı akış cihaz sertifikalarını oluşturan paylaşılan kök CA sertifikasının bir kopyasını da taşıyın.

   IoT Hub bağlanan aşağı akış cihazında bulunan uygulamalarda bu dosyalara başvurabileceksiniz. Sertifika dosyalarını taşımak için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) veya [Güvenli kopya Protokolü](https://www.ssh.com/ssh/scp/) gibi bir işlev gibi bir hizmet kullanabilirsiniz.

6. Tercih ettiğiniz dile bağlı olarak, X. 509.440 sertifikalarına IoT uygulamalarında nasıl başvurulabilen örnekleri gözden geçirin:

   * C#: [Azure IoT Hub 'ınızda X. 509.440 güvenliğini ayarlama](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates)
   * C: [iotedge_downstream_device_sample. c](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iotedge_downstream_device_sample)
   * Node.js: [simple_sample_device_x509.js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js)
   * Java: [SendEventX509. Java](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples/send-event-x509)
   * Python: [send_message_x509. Kopyala](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/async-hub-scenarios/send_message_x509.py)

Aynı cihaz oluşturma işlemini gerçekleştirmek için [Azure CLI Için IoT uzantısını](https://github.com/Azure/azure-iot-cli-extension) da kullanabilirsiniz. Aşağıdaki örnek, X. 509.952 CA imzalı kimlik doğrulaması ile yeni bir IoT cihazı oluşturur ve bir üst cihaz atar:

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_ca
```

Daha fazla bilgi için bkz. [az IoT Hub cihaz kimliği](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/hub/device-identity?view=azure-cli-latest) komutları IÇIN Azure CLI başvuru içeriği.

Sonra, cihazınızın ağ geçidi aracılığıyla bağlanmayı bilmesi için [bağlantı dizesini alın ve değiştirin](#retrieve-and-modify-connection-string) .

## <a name="retrieve-and-modify-connection-string"></a>Bağlantı dizesini alma ve değiştirme

Portalda IoT cihaz kimliği oluşturduktan sonra, birincil veya ikincil anahtarlarını alabilirsiniz. Bu anahtarlardan birinin, uygulamaların IoT Hub iletişim kurmak için kullandığı bağlantı dizesine eklenmesi gerekir. Simetrik anahtar kimlik doğrulaması için IoT Hub, kolaylık sağlamak üzere cihaz ayrıntılarında tamamen biçimlendirilmiş bağlantı dizesi sağlar. Ağ geçidi cihazı hakkında bağlantı dizesine ek bilgi eklemeniz gerekir.

Aşağı akış cihazları için bağlantı dizeleri aşağıdaki bileşenlere gerek duyar:

* Cihazın bağlandığı IoT Hub 'ı:`Hostname={iothub name}.azure-devices.net`
* Hub 'a kayıtlı cihaz KIMLIĞI:`DeviceID={device ID}`
* Birincil ya da ikincil anahtar:`SharedAccessKey={key}`
* Cihazın bağlandığı ağ geçidi cihazı. IoT Edge ağ geçidi cihazının config. YAML dosyasından **ana bilgisayar adı** değerini girin:`GatewayHostName={gateway hostname}`

Hepsi birlikte, bir bağlantı dizesinin tamamı şöyle görünür:

```
HostName=myiothub.azure-devices.net;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz;GatewayHostName=myGatewayDevice
```

Bu aşağı akış cihazı için bir üst/alt ilişki kurduysanız, ağ geçidini doğrudan bağlantı ana bilgisayarı olarak çağırarak bağlantı dizesini basitleştirebilirsiniz. X. 509.440 kimlik doğrulaması için üst/alt ilişkileri gerekir, ancak simetrik anahtar kimlik doğrulaması için isteğe bağlıdır. Örneğin:

```
HostName=myGatewayDevice;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz
```

Bu değiştirilmiş bağlantı dizesini, saydam ağ geçidi serisinin bir sonraki makalesinde kullanacaksınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu noktada, IoT Hub 'ınıza kayıtlı ve saydam bir ağ geçidi olarak yapılandırılmış bir IoT Edge cihazınız vardır. IoT Hub 'ınıza kayıtlı ve ağ geçidi cihazını işaret eden bir aşağı akış cihazınız da vardır.

Bu makaledeki adımlar IoT Hub kimlik doğrulaması için aşağı akış cihazınızı ayarlar. Sonra, ağ geçidi cihazına güvenmek ve güvenli bir şekilde bağlanmak için aşağı akış cihazınızı yapılandırmanız gerekir. Saydam ağ geçidi serisinde bir sonraki makaleye devam edin, [bir aşağı akış cihazını Azure IoT Edge bir ağ geçidine bağlayın](how-to-connect-downstream-device.md).
