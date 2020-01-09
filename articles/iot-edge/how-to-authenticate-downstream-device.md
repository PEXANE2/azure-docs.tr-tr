---
title: Aşağı akış cihazların kimliğini doğrulama-Azure IoT Edge | Microsoft Docs
description: IoT Hub için aşağı akış cihazlarının veya yaprak cihazların kimliğini doğrulama ve bağlantı Azure IoT Edge ağ geçidi cihazları aracılığıyla yönlendirme.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/13/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 74ef00a1b7310284c5f8c51e3c2c6685ffcd5071
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75434486"
---
# <a name="authenticate-a-downstream-device-to-azure-iot-hub"></a>Azure IoT Hub’da bir aşağı akış cihazının kimliğini doğrulama

Saydam bir ağ geçidi senaryosunda, aşağı akış cihazları (bazen yaprak cihazlar veya alt cihazlar olarak adlandırılır), diğer tüm cihazlar gibi IoT Hub kimliklere ihtiyaç duyar. Bu makalede, IoT Hub için bir aşağı akış cihazının kimliğini doğrulama seçenekleri anlatılır ve sonra Ağ Geçidi bağlantısının nasıl bildirildiği gösterilmektedir.

Başarılı bir saydam ağ geçidi bağlantısı kurmak için üç genel adım vardır. Bu makalede ikinci adım ele alınmaktadır:

1. Ağ Geçidi cihazının aşağı akış cihazlarına güvenli bir şekilde bağlanabilmesi, aşağı akış cihazlarından iletişim alabilmesi ve iletileri uygun hedefe yönlendirmesi gerekir. Daha fazla bilgi için bkz. bir [IoT Edge cihazını saydam bir ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md).
2. **Aşağı akış cihazının IoT Hub kimlik doğrulaması yapabilmesi ve ağ geçidi cihazından iletişim kurmayı bilmesi için bir cihaz kimliği olması gerekir.**
3. Aşağı akış cihazının ağ geçidi cihazına güvenli bir şekilde bağlanması gerekir. Daha fazla bilgi için [bir Azure IOT Edge ağ geçidi için aşağı akış cihaz Bağlayamama](how-to-connect-downstream-device.md).

Aşağı akış cihazları şu üç yöntemden birini kullanarak IoT Hub kimlik doğrulaması yapabilir: simetrik anahtarlar (bazen paylaşılan erişim anahtarları olarak adlandırılır), X. 509.440 otomatik olarak imzalanan sertifikalar veya X. 509.440 sertifika yetkilisi (CA) imzalı sertifikalar. Kimlik doğrulama adımları, ağ geçidi ilişkisini bildirmek için küçük farklılıklar ile, IoT Edge olmayan bir cihazı IoT Hub kurmak için kullanılan adımlara benzerdir.

Bu makaledeki adımlarda, Azure IoT Hub cihaz sağlama hizmeti (DPS) ile otomatik sağlama değil el ile cihaz sağlama gösterilmektedir. DPS ile aşağı akış cihazların sağlanması desteklenmez. 

## <a name="prerequisites"></a>Ön koşullar

[Bir IoT Edge cihazını, saydam bir ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md)adımlarını uygulayın. Aşağı akış cihazınız için X. 509.440 kimlik doğrulaması kullanıyorsanız, saydam ağ geçidi makalesinde ayarladığınız betiği üreten sertifikayı kullanmanız gerekir. 

Bu makale, *ağ geçidi ana bilgisayar adına* birkaç noktada başvurur. Ağ geçidi ana bilgisayar adı, IoT Edge ağ geçidi cihazında config. YAML dosyasının **hostname** parametresinde bildirilmiştir. Bu, aşağı akış cihazının bağlantı dizesinde başvurulur. Ağ geçidi ana bilgisayar adının DNS veya ana bilgisayar dosya girişi kullanılarak bir IP adresi ile çözümlenebilmelidir.

## <a name="register-device-symmetric-key"></a>Cihazı Kaydet (simetrik anahtar)

Simetrik anahtar kimlik doğrulaması veya paylaşılan erişim anahtarı kimlik doğrulaması, IoT Hub kimlik doğrulamasının en kolay yoludur. Simetrik anahtar kimlik doğrulamasıyla, IoT Hub ' deki IoT cihaz KIMLIĞINIZLE bir Base64 anahtarı ilişkilendirilir. Cihazınızın IoT Hub bağlandığı sırada sunabilmesi için bu anahtarı IoT uygulamalarınıza dahil edersiniz. 

### <a name="create-the-device-identity"></a>Cihaz kimliği oluşturma 

Visual Studio Code için Azure portal, Azure CLı veya IoT uzantısını kullanarak IoT Hub 'ınıza yeni bir IoT cihazı ekleyin. Aşağı akış cihazlarının, IoT Edge cihazlar değil, normal IoT cihazları olarak IoT Hub tanımlanması gerektiğini unutmayın. 

Yeni cihaz kimliğini oluşturduğunuzda, aşağıdaki bilgileri sağlayın: 

* Cihazınız için bir KIMLIK oluşturun.

* Kimlik doğrulama türü olarak **simetrik anahtar** ' ı seçin. 

* İsteğe bağlı olarak, **bir üst cihaz ayarlamayı** seçin ve bu aşağı akış cihazının bağlanacağı IoT Edge ağ geçidi cihazını seçin. Bu adım simetrik anahtar kimlik doğrulaması için isteğe bağlıdır, ancak bir üst cihaz ayarlandığında, aşağı akış cihazınız için [çevrimdışı yetenekler](offline-capabilities.md) sağladığından önerilir. Üst öğeyi daha sonra eklemek veya değiştirmek için cihaz ayrıntılarını her zaman güncelleştirebilirsiniz. 

   ![Portalda simetrik anahtar kimlik doğrulaması ile cihaz KIMLIĞI oluşturma](./media/how-to-authenticate-downstream-device/symmetric-key-portal.png)

Aynı işlemi gerçekleştirmek için [Azure CLI Için IoT uzantısı](https://github.com/Azure/azure-iot-cli-extension) 'nı kullanabilirsiniz. Aşağıdaki örnek, simetrik anahtar kimlik doğrulaması ile yeni bir IoT cihazı oluşturur ve bir üst cihaz atar: 

```cli
az iot hub device-identity create -n {iothub name} -d {new device ID} --pd {existing gateway device ID}
```

Cihaz oluşturma ve üst/alt yönetim için Azure CLı komutları hakkında daha fazla bilgi için bkz. [az IoT Hub cihaz kimliği](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) komutları için başvuru içeriği.


Sonra, cihazınızın ağ geçidi aracılığıyla bağlanmayı bilmesi için [bağlantı dizesini alın ve değiştirin](#retrieve-and-modify-connection-string) . 

## <a name="register-device-x509-self-signed"></a>Cihazı Kaydet (X. 509.440 otomatik imzalı) 

X. 509.440 otomatik imzalı kimlik doğrulaması için bazen parmak izi kimlik doğrulaması olarak da adlandırılan, IoT cihazınıza yerleştirilecek yeni sertifikalar oluşturmanız gerekir. Bu sertifikaların kimlik doğrulaması için IoT Hub paylaştığınız bir parmak izi vardır. 

X. 509.952 sertifikaları oluşturmak için bir sertifika yetkiliniz yoksa [IoT Edge cihaz özelliklerini test etmek için tanıtım sertifikaları oluşturabilirsiniz](how-to-create-test-certificates.md). Aşağı akış cihazınız için test sertifikaları oluştururken, ağ geçidi cihazınız için sertifikaları oluşturan aynı kök CA sertifikasını kullanın. 

1. CA sertifikanızı kullanarak, aşağı akış cihazı için iki cihaz sertifikası (birincil ve ikincil) oluşturun. 

   Cihaz sertifikası, IoT cihazını Azure IoT Hub kaydederken kullanacağınız cihaz KIMLIĞI 'ne ayarlanmış konu adına sahip olmalıdır. Bu ayar kimlik doğrulaması için gereklidir.

2. 40 onaltılık bir karakter dizesi olan her bir sertifikadan SHA1 parmak izini (IoT Hub arabiriminde parmak izi denir) alın. Sertifikayı görüntülemek ve parmak izini bulmak için aşağıdaki OpenSSL komutunu kullanın:

   ```PowerShell/bash
   openssl x509 -in <primary device certificate>.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

   Birincil sertifika ve ikincil sertifika için bir kez olmak üzere bu komutu iki kez çalıştırın. Otomatik olarak imzalanan X. 509.440 sertifikalarını kullanarak yeni bir IoT cihazını kaydettiğinizde her iki sertifika için de parmak izleri sağlarsınız. 

3. Azure portal IoT Hub 'ınıza gidin ve aşağıdaki değerlerle yeni bir IoT cihaz kimliği oluşturun: 

   * Cihaz sertifikalarınızın konu adıyla eşleşen **CIHAZ kimliğini** sağlayın. 
   * Kimlik doğrulama türü olarak **X. 509.440 otomatik olarak imzalanan** ' ı seçin.
   * Cihazınızın birincil ve ikincil sertifikalarından kopyaladığınız onaltılık dizeleri yapıştırın.
   * **Bir üst cihaz ayarla** ' yı seçin ve bu aşağı akış cihazının bağlanacağı IoT Edge ağ geçidi cihazını seçin. Bir aşağı akış cihazının X. 509.440 kimlik doğrulaması için bir üst cihaz gereklidir. 

   ![Portalda X. 509.440 otomatik imzalı kimlik doğrulaması ile cihaz KIMLIĞI oluşturma](./media/how-to-authenticate-downstream-device/x509-self-signed-portal.png)

4. Cihaz sertifikasını ve anahtarlarını aşağı akış cihazında herhangi bir konuma kopyalayın. Ayrıca, ağ geçidi cihaz sertifikasını ve aşağı akış cihaz sertifikalarını oluşturan paylaşılan kök CA sertifikasının bir kopyasını da taşıyın. 

   Bu dosyalara IoT Hub bağlanan yaprak cihaz uygulamalarında başvuracağız. Sertifika dosyalarını taşımak için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) veya [Güvenli kopya Protokolü](https://www.ssh.com/ssh/scp/) gibi bir işlev gibi bir hizmet kullanabilirsiniz.

5. Tercih ettiğiniz dile bağlı olarak, X. 509.440 sertifikalarına IoT uygulamalarında nasıl başvurulabilen örnekleri gözden geçirin: 

   * C#: [Azure IoT Hub 'ınızda X. 509.440 güvenliğini ayarlama](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates)
   * C: [iotedge_downstream_device_sample](https://github.com/Azure/azure-iot-sdk-c/tree/x509_edge_bugbash/iothub_client/samples/iotedge_downstream_device_sample)
   * Node. js: [simple_sample_device_x509. js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js)
   * Java: [SendEventX509. Java](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py)
   * Python: [send_message_x509. Kopyala](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/advanced-hub-scenarios/send_message_x509.py)

Aynı cihaz oluşturma işlemini gerçekleştirmek için [Azure CLI Için IoT uzantısı](https://github.com/Azure/azure-iot-cli-extension) 'nı kullanabilirsiniz. Aşağıdaki örnek, X. 509.440 otomatik imzalı kimlik doğrulaması ile yeni bir IoT cihazı oluşturur ve bir üst cihaz atar: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_thumbprint --ptp {primary thumbprint} --stp {secondary thumbprint}
```

Cihaz oluşturma, sertifika oluşturma ve üst ve alt yönetim için Azure CLı komutları hakkında daha fazla bilgi için bkz. [az IoT Hub cihaz kimliği](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) komutları için başvuru içeriği.


Sonra, cihazınızın ağ geçidi aracılığıyla bağlanmayı bilmesi için [bağlantı dizesini alın ve değiştirin](#retrieve-and-modify-connection-string) . 


## <a name="register-device-x509-ca-signed"></a>Cihazı Kaydet (X. 509.440 CA imzalanmış)

X. 509.440 sertifika yetkilisi (CA) imzalı kimlik doğrulaması için, IoT cihazınız için sertifikaları imzalamak üzere kullandığınız IoT Hub kayıtlı bir kök CA sertifikasına ihtiyacınız vardır. Kök CA sertifikası veya ara sertifikalarının herhangi biri tarafından karşılaşılan bir sertifikayı kullanan herhangi bir cihazın kimlik doğrulamasına izin verilir. 

Bu bölüm, [Azure IoT Hub 'ınızda X. 509.440 güvenliğini ayarlama](../iot-hub/iot-hub-security-x509-get-started.md)IoT Hub makalesinde açıklanan yönergeleri temel alır. Bir ağ geçidi üzerinden bağlanan bir aşağı akış cihazını ayarlamak için hangi değerlerin kullanılacağını bilmek için bu bölümdeki adımları izleyin. 

X. 509.952 sertifikaları oluşturmak için bir sertifika yetkiliniz yoksa [IoT Edge cihaz özelliklerini test etmek için tanıtım sertifikaları oluşturabilirsiniz](how-to-create-test-certificates.md). Aşağı akış cihazınız için test sertifikaları oluştururken, ağ geçidi cihazınız için sertifikaları oluşturan aynı kök CA sertifikasını kullanın. 

1. *Azure IoT Hub 'ınızda x. 509.952 güvenliğini ayarlama*konusunun [IoT Hub 'ınıza x. 509.952 CA sertifikalarını kaydetme](../iot-hub/iot-hub-security-x509-get-started.md#register-x509-ca-certificates-to-your-iot-hub) bölümündeki yönergeleri izleyin. Bu bölümde, aşağıdaki adımları gerçekleştirirsiniz: 

   1. Kök CA sertifikasını karşıya yükleyin. Tanıtım sertifikaları kullanıyorsanız, kök CA **\<yol >/certs/Azure-iot-test-only.root.ca.cert.pem**. 

   2. Bu kök CA sertifikasına sahip olduğunuzu doğrulayın.

2. *Azure IoT Hub 'ınızda x. 509.440 güvenliği ayarlama*konusunun [IoT Hub 'ınız için x. 509.952 cihazı oluşturma](../iot-hub/iot-hub-security-x509-get-started.md#create-an-x509-device-for-your-iot-hub) bölümündeki yönergeleri izleyin. Bu bölümde, aşağıdaki adımları gerçekleştirirsiniz: 

   1. Yeni bir cihaz ekleyin. **CIHAZ kimliği**için küçük harfli bir ad girin ve **X. 509.440 CA imzalı**kimlik doğrulama türünü seçin. 
   2. Bir üst cihaz ayarlayın. Aşağı akış cihazları için **bir üst cihaz ayarla** ' yı seçin ve IoT Hub bağlantısını sağlayacak IoT Edge ağ geçidi cihazını seçin. 

3. Aşağı akış cihazınız için bir sertifika zinciri oluşturun. IoT Hub için karşıya yüklediğiniz aynı kök CA sertifikasını kullanın ve bu zinciri oluşturun. Portalda cihaz kimliğinize verdiğiniz küçük harfli cihaz KIMLIĞINI kullanın.

4. Cihaz sertifikasını ve anahtarlarını aşağı akış cihazında herhangi bir konuma kopyalayın. Ayrıca, ağ geçidi cihaz sertifikasını ve aşağı akış cihaz sertifikalarını oluşturan paylaşılan kök CA sertifikasının bir kopyasını da taşıyın. 

   Bu dosyalara IoT Hub bağlanan yaprak cihaz uygulamalarında başvuracağız. Sertifika dosyalarını taşımak için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) veya [Güvenli kopya Protokolü](https://www.ssh.com/ssh/scp/) gibi bir işlev gibi bir hizmet kullanabilirsiniz.

5. Tercih ettiğiniz dile bağlı olarak, X. 509.440 sertifikalarına IoT uygulamalarında nasıl başvurulabilen örnekleri gözden geçirin: 

   * C#: [Azure IoT Hub 'ınızda X. 509.440 güvenliğini ayarlama](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates)
   * C: [iotedge_downstream_device_sample](https://github.com/Azure/azure-iot-sdk-c/tree/x509_edge_bugbash/iothub_client/samples/iotedge_downstream_device_sample)
   * Node. js: [simple_sample_device_x509. js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js)
   * Java: [SendEventX509. Java](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py)
   * Python: [send_message_x509. Kopyala](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/advanced-hub-scenarios/send_message_x509.py)

Aynı cihaz oluşturma işlemini gerçekleştirmek için [Azure CLI Için IoT uzantısı](https://github.com/Azure/azure-iot-cli-extension) 'nı kullanabilirsiniz. Aşağıdaki örnek, X. 509.952 CA imzalı kimlik doğrulaması ile yeni bir IoT cihazı oluşturur ve bir üst cihaz atar: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_ca
```

Cihaz oluşturma ve üst/alt yönetim için Azure CLı komutları hakkında daha fazla bilgi için bkz. [az IoT Hub cihaz kimliği](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) komutları için başvuru içeriği.


Sonra, cihazınızın ağ geçidi aracılığıyla bağlanmayı bilmesi için [bağlantı dizesini alın ve değiştirin](#retrieve-and-modify-connection-string) . 


## <a name="retrieve-and-modify-connection-string"></a>Bağlantı dizesini alma ve değiştirme

Portalda IoT cihaz kimliği oluşturduktan sonra, birincil veya ikincil anahtarlarını alabilirsiniz. Bu anahtarlardan birinin, IoT Hub ile iletişim kuran herhangi bir uygulamaya dahil ettiğiniz bağlantı dizesine eklenmesi gerekir. Simetrik anahtar kimlik doğrulaması için IoT Hub, kolaylık sağlamak üzere cihaz ayrıntılarında tamamen biçimlendirilmiş bağlantı dizesi sağlar. Ağ geçidi cihazı hakkında bağlantı dizesine ek bilgi eklemeniz gerekir. 

Aşağı akış cihazları için bağlantı dizeleri aşağıdaki bileşenlere gerek duyar: 

* Cihazın bağlandığı IoT Hub 'ı: `Hostname={iothub name}.azure-devices.net`
* Hub 'a kayıtlı cihaz KIMLIĞI: `DeviceID={device ID}`
* Birincil veya ikincil anahtar: `SharedAccessKey={key}`
* Cihazın bağlandığı ağ geçidi cihazı. IoT Edge ağ geçidi cihazının config. YAML dosyasından **konak adı** değerini sağlayın: `GatewayHostName={gateway hostname}`

Hepsi birlikte, bir bağlantı dizesinin tamamı şöyle görünür:

``` 
HostName=myiothub.azure-devices.net;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz;GatewayHostName=myGatewayDevice
```

Bu aşağı akış aygıtı için bir üst/alt ilişki oluşturduysanız (X. 509.440 kimlik doğrulaması için gereklidir ancak simetrik anahtar kimlik doğrulaması için isteğe bağlı), ağ geçidini doğrudan bağlantı ana bilgisayarı olarak çağırarak bağlantı dizesini basitleştirebilirsiniz. Örneğin: 

```
HostName=myGatewayDevice;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz
```

Bu noktada, bir IoT Edge cihazının kayıtlı ve ağ geçidi olarak yapılandırılmış olması gerekir. Ayrıca, kayıtlı bir aşağı akış IoT cihazınız var ve ağ geçidi cihazını işaret ediyor. Son adım, sertifikaların, ağ geçidine güvenli bir şekilde bağlanabilmesi için aşağı akış cihazınıza yerleştirmeleri. 

Ağ Geçidi serisinde bir sonraki makaleye devam edin, [bir aşağı akış cihazını Azure IoT Edge bir ağ geçidine bağlayın](how-to-connect-downstream-device.md).


## <a name="next-steps"></a>Sonraki adımlar

Bu makaleyi tamamlayarak, saydam bir ağ geçidi olarak çalışan bir IoT Edge cihazının ve IoT Hub ile kaydedilmiş bir aşağı akış cihazının olması gerekir. Sonra, ağ geçidi cihazına güvenmek ve güvenli bir şekilde bağlanmak için aşağı akış cihazlarınızı yapılandırmanız gerekir. Daha fazla bilgi için [bir Azure IOT Edge ağ geçidi için aşağı akış cihaz Bağlayamama](how-to-connect-downstream-device.md).
