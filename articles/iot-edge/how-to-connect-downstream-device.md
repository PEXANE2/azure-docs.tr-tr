---
title: Akış aşağı aygıtları bağlayın - Azure IoT Edge | Microsoft Dokümanlar
description: Azure IoT Edge ağ geçidi aygıtlarına bağlanmak için akış aşağı veya yaprak aygıtları nasıl yapılandırılabilen.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/08/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ff9415ca20b859468528b56d27355430c4fc5a0f
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652060"
---
# <a name="connect-a-downstream-device-to-an-azure-iot-edge-gateway"></a>Aşağı akış cihazını Azure IoT Edge ağ geçidine bağlama

Bu makalede, aşağı akış aygıtları ve IoT Edge saydam ağ geçitleri arasında güvenilir bir bağlantı kurmak için yönergeler sağlar. Saydam bir ağ geçidi senaryosunda, bir veya daha fazla aygıt iletilerini IoT Hub'a bağlantıyı koruyan tek bir ağ geçidi aygıtı üzerinden geçirebilir. Akış aşağı aygıt, [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) bulut hizmetiyle oluşturulmuş bir kimliğe sahip herhangi bir uygulama veya platform olabilir. Çoğu durumda, bu uygulamalar [Azure IoT aygıtı SDK'yı](../iot-hub/iot-hub-devguide-sdks.md)kullanır. Bir downstream aygıtı bile IoT Edge ağ geçidi aygıtı kendisi üzerinde çalışan bir uygulama olabilir.

Başarılı bir saydam ağ geçidi bağlantısı kurmak için üç genel adım vardır. Bu makale, üçüncü adımı kapsar:

1. Ağ geçidi aygıtının akış aşağı aygıtlara güvenli bir şekilde bağlanması, akış aşağı aygıtlarından iletişim alması ve iletileri uygun hedefe yönlendirmesi gerekir. Daha fazla bilgi için [bkz.](how-to-create-transparent-gateway.md)
2. Akış aşağı aygıtının IoT Hub ile kimlik doğrulaması yapabilmesi ve ağ geçidi aygıtı üzerinden iletişim kurabilmesi için bir aygıt kimliğine ihtiyacı vardır. Daha fazla bilgi için [bkz.](how-to-authenticate-downstream-device.md)
3. **Akış aşağı aygıtının ağ geçidi aygıtına güvenli bir şekilde bağlanması gerekir.**

Bu makalede, alt aygıt bağlantıları ile ilgili yaygın sorunları tanımlar ve aşağıdakileri yaparak alt akış aygıtlarınızı kurmanızda size yol açar:

* Taşıma katmanı güvenliği (TLS) ve sertifika temellerini açıklama.
* TLS kitaplıklarının farklı işletim sistemlerinde nasıl çalıştığını ve her işletim sisteminin sertifikalarla nasıl başa çıktığını açıklar.
* Başlamanıza yardımcı olmak için Azure IoT örneklerinde çeşitli dillerde gezinme.

Bu makalede, *ağ geçidi* ve *IoT Edge ağ geçidi terimleri,* saydam ağ geçidi olarak yapılandırılan bir IoT Edge aygıtına başvurur.

## <a name="prerequisites"></a>Ön koşullar

* [Bir IoT Edge aygıtını yapılandırın](how-to-create-transparent-gateway.md) ve akış aşağı aygıtınızda bulunan saydam bir ağ geçidi olarak hareket etmek üzere yapılandırıldığında oluşturulan **azure-iot-test-only.root.ca.cert.pem** sertifika dosyasına sahip olun. Akış aşağı aygıtınız, ağ geçidi aygıtının kimliğini doğrulamak için bu sertifikayı kullanır.
* Bir [aşağı aygıtı Azure IoT Hub'ına Doğrula'da](how-to-authenticate-downstream-device.md)açıklandığı gibi ağ geçidi aygıtına işaret eden değiştirilmiş bağlantı dizesine sahip olun.

## <a name="prepare-a-downstream-device"></a>Akış aşağı aygıtı hazırlama

Akış aşağı aygıt, [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) bulut hizmetiyle oluşturulmuş bir kimliğe sahip herhangi bir uygulama veya platform olabilir. Çoğu durumda, bu uygulamalar [Azure IoT aygıtı SDK'yı](../iot-hub/iot-hub-devguide-sdks.md)kullanır. Bir downstream aygıtı bile IoT Edge ağ geçidi aygıtı kendisi üzerinde çalışan bir uygulama olabilir. Ancak, başka bir IoT Edge aygıtı bir IoT Edge ağ geçidinin aşağı akışı olamaz.

>[!NOTE]
>IoT Hub'da kayıtlı kimlikleri olan IoT aygıtları, farklı işlemleri, donanımı veya işlevleri tek bir aygıtta yalıtmak için [modül ikizlerini](../iot-hub/iot-hub-devguide-module-twins.md) kullanabilir. IoT Edge ağ geçitleri, simetrik anahtar kimlik doğrulamasını kullanarak aşağı modül bağlantılarını destekler, ancak X.509 sertifika kimlik doğrulaması kullanmaz.

Bir alt aygıtı Bir IoT Edge ağ geçidine bağlamak için iki şeye ihtiyacınız vardır:

* Ağ geçidine bağlamak için bilgilerle eklenen bir IoT Hub aygıt bağlantı dizesi ile yapılandırılan bir aygıt veya uygulama.

    Bu adım, [bir aşağı aygıtı Azure IoT Hub'ına Authenticate'da](how-to-authenticate-downstream-device.md)açıklanmıştır.

* Aygıt veya uygulama, ağ geçidi aygıtına TLS bağlantılarını doğrulamak için ağ geçidinin **kök CA** sertifikasına güvenmek zorundadır.

    Bu adım, bu makalenin geri kalanında ayrıntılı olarak açıklanmıştır. Bu adım iki şekilde gerçekleştirilebilir: Ca sertifikasını işletim sisteminin sertifika deposuna yükleyerek veya (belirli diller için) Azure IoT SDK'larını kullanarak uygulamalarda sertifikaya başvurarak.

## <a name="tls-and-certificate-fundamentals"></a>TLS ve sertifika temelleri

Aşağı akım aygıtlarını IoT Edge'e güvenli bir şekilde bağlamanın zorluğu, internet üzerinden gerçekleşen diğer güvenli istemci/sunucu iletişimi gibidir. Bir istemci ve [sunucu, Aktarım katmanı güvenliğini (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security)kullanarak internet üzerinden güvenli bir şekilde iletişim kurar. TLS, sertifika adı verilen standart [Ortak anahtar altyapısı (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure) yapıları kullanılarak oluşturulur. TLS oldukça ilgili bir belirtimdir ve iki uç noktanın güvenliğini sağlamakla ilgili çok çeşitli konuları ele alır. Bu bölümde, aygıtları bir IoT Edge ağ geçidine güvenli bir şekilde bağlamanız için ilgili kavramlar özetlenmiştir.

İstemci bir sunucuya bağlandığında, sunucu *sunucu sertifika zinciri*adı verilen bir sertifika zinciri sunar. Sertifika zinciri genellikle bir kök sertifika yetkilisi (CA) sertifikası, bir veya daha fazla ara CA sertifikası ve son olarak sunucunun sertifikasının kendisinden oluşur. İstemci, tüm sunucu sertifika zincirini şifreleme olarak doğrulayarak bir sunucuyla güven kurar. Sunucu sertifika zincirinin bu istemci doğrulaması *sunucu zinciri doğrulama*denir. İstemci, sunucu sertifikasıyla ilişkili özel anahtarın sahip olduğunu kanıtlaması için *hizmete*şifreleme yle meydan okuyor. Sunucu zinciri doğrulaması ve sahip olma kanıtı nın birleşimine *sunucu kimlik doğrulaması*denir. Bir sunucu sertifika zincirini doğrulamak için istemcinin sunucu nun sertifikasını oluşturmak (veya vermek) için kullanılan kök CA sertifikasının bir kopyasına ihtiyacı vardır. Normalde web sitelerine bağlanırken, istemci sorunsuz bir işlem olması için bir tarayıcı yaygın olarak kullanılan CA sertifikaları ile önceden yapılandırılmış olarak gelir.

Bir aygıt Azure IoT Hub'ına bağlandığında, aygıt istemci, IoT Hub bulut hizmeti sunucudur. IoT Hub bulut hizmeti, genel kullanıma açık ve yaygın olarak kullanılan **Baltimore CyberTrust Root**adlı bir kök CA sertifikası tarafından desteklenmektedir. IoT Hub CA sertifikası çoğu cihazda zaten yüklendiğinden, birçok TLS uygulaması (OpenSSL, Schannel, LibreSSL) sunucu sertifikası doğrulama sırasında otomatik olarak kullanır. IoT Hub'a başarıyla bağlanabilen bir aygıt, bir IoT Edge ağ geçidine bağlanmaya çalışırken sorunlar yaşayabilir.

Bir aygıt Bir IoT Edge ağ geçidine bağlandığında, akış aşağı aygıt istemcive ağ geçidi aygıtı sunucudur. Azure IoT Edge, operatörlerin (veya kullanıcıların) ağ geçidi sertifika zincirlerini uygun gördükleri şekilde oluşturmalarına olanak tanır. Operatör, Baltimore gibi herkese açık bir CA sertifikası kullanmayı veya kendi imzasını taşıyan (veya şirket içi) bir CA sertifikası kullanmayı seçebilir. Ortak CA sertifikaları genellikle onlarla ilişkili bir maliyeti vardır, bu nedenle genellikle üretim senaryolarında kullanılır. Geliştirme ve test için kendi imzalı CA sertifikaları tercih edilir. Girişte listelenen saydam ağ geçidi kurulum makaleleri, kendi imzalı kök CA sertifikalarını kullanır.

IoT Edge ağ geçidi için kendi imzalı kök CA sertifikası kullandığınızda, ağ geçidine bağlanmaya çalışan tüm alt aygıtlara yüklenmesi veya sağlanması gerekir.

![Ağ geçidi sertifikası kurulumu](./media/how-to-create-transparent-gateway/gateway-setup.png)

IoT Edge sertifikaları ve bazı üretim sonuçları hakkında daha fazla bilgi edinmek için [IoT Edge sertifika kullanım ayrıntılarına](iot-edge-certs.md)bakın.

## <a name="provide-the-root-ca-certificate"></a>Kök CA sertifikasını sağlama

Ağ geçidi aygıtının sertifikalarını doğrulamak için, akış aşağı aygıtının kök CA sertifikasının kendi kopyasına ihtiyacı vardır. Test sertifikaları oluşturmak için IoT Edge git deposunda sağlanan komut dosyalarını kullandıysanız, kök CA **sertifikasına azure-iot-test-only.root.ca.cert.pem**denir. Diğer alt aygıt hazırlama adımlarının bir parçası değilseniz, bu sertifika dosyasını akış aşağı aygıtınızdaki herhangi bir dizine taşıyın. Sertifika dosyasını taşımak için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) gibi bir hizmeti veya Güvenli kopyalama [protokolü](https://www.ssh.com/ssh/scp/) gibi bir işlevi kullanabilirsiniz.

## <a name="install-certificates-in-the-os"></a>İşletim Sistemi'ne sertifika yükleme

Root CA sertifikasının işletim sisteminin sertifika deposuna yüklenmesi genellikle çoğu uygulamanın kök CA sertifikasını kullanmasına olanak tanır. Os sertifika deposunu kullanmayan, daha çok Düğüm çalışma zamanının iç sertifika deposunu kullanan NodeJS uygulamaları gibi bazı özel durumlar vardır. Sertifikayı işletim sistemi düzeyinde yükleyemezseniz, [Azure IoT SDK'lı sertifikaları kullanma](#use-certificates-with-azure-iot-sdks)için ileri ye atın.

### <a name="ubuntu"></a>Ubuntu

Aşağıdaki komutlar, Bir Ubuntu ana bilgisayara CA sertifikasının nasıl yüklenir olduğuna ilişkin bir örnektir. Bu örnek, önkoşul makalelerinden **azure-iot-test-only.root.ca.cert.pem** sertifikasını kullandığınızı ve sertifikayı akış aşağı aygıtındaki bir konuma kopyaladığınızı varsayar.

```bash
sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
sudo update-ca-certificates
```

"/etc/ssl/certs'teki sertifikaları güncellemek... 1 eklendi, 0 kaldırıldı; bitti."

### <a name="windows"></a>Windows

Aşağıdaki adımlar, Bir Windows ana bilgisayara CA sertifikası yükleme nin bir örneğidir. Bu örnek, önkoşul makalelerinden **azure-iot-test-only.root.ca.cert.pem** sertifikasını kullandığınızı ve sertifikayı akış aşağı aygıtındaki bir konuma kopyaladığınızı varsayar.

PowerShell'in [Alma Sertifikasını](https://docs.microsoft.com/powershell/module/pkiclient/import-certificate?view=win10-ps) kullanarak sertifikaları yönetici olarak yükleyebilirsiniz:

```powershell
import-certificate  <file path>\azure-iot-test-only.root.ca.cert.pem -certstorelocation cert:\LocalMachine\root
```

**Certlm** yardımcı programını kullanarak sertifikaları da yükleyebilirsiniz:

1. Başlat menüsünde, **bilgisayar sertifikalarını araştırın**ve Yönet'i seçin. **Certlm** adlı bir yardımcı program açılır.
2. **Sertifikalara** > Gidin - Yerel Bilgisayar**Güvenilen Kök Sertifika Yetkilileri.**
3. Sağ tıklayarak **Sertifikalar** ve **Tüm Görevler** > **Alma**seçin. Sertifika alma sihirbazı başlatMalıdır.
4. Yönettiği adımları izleyin ve sertifika `<path>/azure-iot-test-only.root.ca.cert.pem`dosyası alma. Tamamlandığında, "Başarıyla içe aktarılan" bir ileti görmeniz gerekir.

Bu makalede daha sonra .NET örneğinde gösterildiği gibi ,NET API'lerini kullanarak sertifikaları programlı olarak yükleyebilirsiniz.

Genellikle uygulamalar, TLS üzerinden güvenli bir şekilde bağlanmak için [Schannel](https://docs.microsoft.com/windows/desktop/com/schannel) adı verilen Windows sağlanan TLS yığınını kullanır. Schannel, TLS bağlantısı kurmaya çalışmadan önce windows sertifika deposuna herhangi bir sertifikanın yüklenmesini *gerektirir.*

## <a name="use-certificates-with-azure-iot-sdks"></a>Azure IoT SDK'ları ile sertifikaları kullanma

Bu bölümde, Azure IoT SDK'larının basit örnek uygulamaları kullanarak bir IoT Edge aygıtına nasıl bağlanabildiğini açıklanmaktadır. Tüm örneklerin amacı, aygıt istemcisini bağlamak ve ağ geçidine telemetri iletileri göndermek, ardından bağlantıyı kapatmak ve çıkmaktır.

Uygulama düzeyi örneklerini kullanmadan önce iki şeyi hazır layın:

* Alt akış aygıtınızın IoT Hub bağlantı dizesi ağ geçidi aygıtını işaret etmek üzere değiştirilmiş ve alt akış aygıtınızın kimliğini IoT Hub'a doğrulamak için gereken sertifikalar. Daha fazla bilgi için [bkz.](how-to-authenticate-downstream-device.md)

* Alt cihazınızda kopyaladığınız ve kaydettiğiniz kök CA sertifikasına giden tam yol.

    Örneğin, `<path>/azure-iot-test-only.root.ca.cert.pem`.

### <a name="nodejs"></a>NodeJS

Bu bölümde, bir Azure IoT NodeJS aygıt istemcisini bir IoT Edge ağ geçidine bağlamak için örnek bir uygulama sağlar. NodeJS uygulamaları için kök CA sertifikasını burada gösterildiği gibi uygulama düzeyinde yüklemeniz gerekir. NodeJS uygulamaları sistemin sertifika deposunu kullanmaz.

1. [Node.js örnekleri repo için Azure IoT cihazı SDK'dan](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples) **edge_downstream_device.js** için örnek alın.
2. **readme.md** dosyasını inceleyerek örneği çalıştırmak için tüm ön koşullara sahip olduğunuzdan emin olun.
3. edge_downstream_device.js dosyasında, **connectionString** ve **edge_ca_cert_path** değişkenlerini güncelleştirin.
4. Örneğin cihazınızda nasıl çalıştırılalaacağına ilişkin talimatlar için SDK belgelerine bakın.

Çalıştırdığınız örneği anlamak için, aşağıdaki kod parçacığı istemci SDK'nın sertifika dosyasını nasıl okuduğu ve güvenli bir TLS bağlantısı kurmak için nasıl kullandığıdır:

```javascript
// Provide the Azure IoT device client via setOptions with the X509
// Edge root CA certificate that was used to setup the Edge runtime
var options = {
    ca : fs.readFileSync(edge_ca_cert_path, 'utf-8'),
};
```

### <a name="net"></a>.NET

Bu bölümde, bir Azure IoT .NET aygıt istemcisini bir IoT Edge ağ geçidine bağlamak için örnek bir uygulama tanıtılıyor. Ancak, .NET uygulamaları, sistemin sertifika deposundaki yüklü sertifikaları hem Linux hem de Windows ana bilgisayarlarda otomatik olarak kullanabilir.

1. [IoT Edge .NET örnekleri klasöründen](https://github.com/Azure/iotedge/tree/master/samples/dotnet/EdgeDownstreamDevice) **EdgeDownstreamDevice** örneğini alın.
2. **readme.md** dosyasını inceleyerek örneği çalıştırmak için tüm ön koşullara sahip olduğunuzdan emin olun.
3. Özellikler **/ launchSettings.json** dosyasında, **DEVICE_CONNECTION_STRING** ve **CA_CERTIFICATE_PATH** değişkenleri güncelleyin. Ana bilgisayar sisteminde ki güvenilen sertifika deposunda yüklü olan sertifikayı kullanmak istiyorsanız, bu değişkeni boş bırakın.
4. Örneğin cihazınızda nasıl çalıştırılalaacağına ilişkin talimatlar için SDK belgelerine bakın.

Bir .NET uygulaması aracılığıyla sertifika deposuna programlı bir sertifika yüklemek için **EdgeDownstreamDevice / Program.cs** dosyasındaki **InstallCACert()** işlevine bakın. Bu işlem idempotent, bu nedenle ek bir etkisi ile aynı değerlere birden çok kez çalıştırılabilir.

### <a name="c"></a>C

Bu bölümde, bir Azure IoT C aygıt istemcisini bir IoT Edge ağ geçidine bağlamak için örnek bir uygulama tanıtılıyor. C SDK OpenSSL, WolfSSL ve Schannel dahil olmak üzere birçok TLS kitaplığı ile çalışabilir. Daha fazla bilgi için [Azure IoT C SDK'ya](https://github.com/Azure/azure-iot-sdk-c)bakın.

1. C örnekleri için [Azure IoT cihazı SDK'dan](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples) **iotedge_downstream_device_sample** uygulama alın.
2. **readme.md** dosyasını inceleyerek örneği çalıştırmak için tüm ön koşullara sahip olduğunuzdan emin olun.
3. iotedge_downstream_device_sample.c dosyasında, **connectionString** ve **edge_ca_cert_path** değişkenlerini güncelleştirin.
4. Örneğin cihazınızda nasıl çalıştırılalaacağına ilişkin talimatlar için SDK belgelerine bakın.

C için Azure IoT aygıt SDK istemcisini ayarlarken ca sertifikası kaydetmek için bir seçenek sağlar. Bu işlem sertifikayı hiçbir yere yüklemez, ancak bellekte sertifikanın dize biçimini kullanır. Kaydedilen sertifika, bağlantı kurarken temel TLS yığınına sağlanır.

```C
(void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
```

Windows ana bilgisayarlarında, OpenSSL veya başka bir TLS kitaplığı kullanmıyorsanız, SDK Schannel'ı kullanmayı varsayılan olarak kullanır. Schannel'un çalışması için, IoT Edge kök CA sertifikasının windows sertifika deposuna `IoTHubDeviceClient_SetOption` yüklenmesi gerekir, işlem kullanılarak ayarlanmamalıdır.

### <a name="java"></a>Java

Bu bölümde, bir Azure IoT Java aygıt istemcisini bir IoT Edge ağ geçidine bağlamak için örnek bir uygulama tanıtılıyor.

1. [Java örnekleri için Azure IoT cihazı SDK'dan](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)Gönder **etkinliği** örneğini alın.
2. **readme.md** dosyasını inceleyerek örneği çalıştırmak için tüm ön koşullara sahip olduğunuzdan emin olun.
3. Örneğin cihazınızda nasıl çalıştırılalaacağına ilişkin talimatlar için SDK belgelerine bakın.

### <a name="python"></a>Python

Bu bölümde, Bir Azure IoT Python aygıt istemcisini bir IoT Edge ağ geçidine bağlamak için örnek bir uygulama tanıtılıyor.

1. [Python örnekleri için Azure IoT aygıtı SDK'dan](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-edge-scenarios) **send_message** örneğini alın.
2. Bir IoT Edge kapsayıcısında veya hata ayıklama senaryosunda çalışırken `EdgeHubConnectionString` ve `EdgeModuleCACertificateFile` ortam değişkenlerini ayarladığınızdan emin olun.
3. Örneğin cihazınızda nasıl çalıştırılalaacağına ilişkin talimatlar için SDK belgelerine bakın.

## <a name="test-the-gateway-connection"></a>Ağ geçidi bağlantısını test edin

Akış aşağı aygıtınızın ağ geçidi aygıtına bağlanabileni test etmek için bu örnek komutu kullanın:

```cmd/sh
openssl s_client -connect mygateway.contoso.com:8883 -CAfile <CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem -showcerts
```

Bu komut, MQTTS (port 8883) üzerindeki bağlantıları sınar. Farklı bir protokol kullanıyorsanız, AMQPS (5671) veya HTTPS (433) için gerekli olan komutu ayarlayın

Bu komutun çıktısı, zincirdeki tüm sertifikalar hakkında bilgi de dahil olmak üzere uzun olabilir. Bağlantınız başarılı olursa, gibi `Verification: OK` bir satır `Verify return code: 0 (ok)`görürsünüz veya .

![Ağ geçidi bağlantısını doğrulama](./media/how-to-connect-downstream-device/verification-ok.png)

## <a name="troubleshoot-the-gateway-connection"></a>Ağ geçidi bağlantısını giderme

Yaprak aygıtınızın ağ geçidi aygıtına aralıklı bağlantısı varsa, çözüm için aşağıdaki adımları deneyin.

1. Bağlantı dizesindeki ağ geçidi ana adı, ağ geçidi aygıtındaki IoT Edge config.yaml dosyasındaki ana bilgisayar adı değeriyle aynı mıdır?
2. Ağ geçidi ana bilgisayar adı bir IP Adresiiçin çözülebilir mi? Aralıklı bağlantıları DNS kullanarak veya yaprak aygıta ana bilgisayar dosya girişi ekleyerek çözebilirsiniz.
3. Güvenlik duvarınızda iletişim noktaları açık mı? Kullanılan protokole dayalı iletişim (MQTTS:8883/AMQPS:5671/HTTPS:433) akış aşağı aygıtı ile saydam IoT Edge arasında mümkün olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

IoT [Edge'in çevrimdışı yetenekleri](offline-capabilities.md) akış aşağı aygıtlara nasıl genişletebileceğini öğrenin.
