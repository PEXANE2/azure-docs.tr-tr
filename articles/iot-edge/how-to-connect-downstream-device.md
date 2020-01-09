---
title: Aşağı Akış cihazları - Azure IOT Edge bağlayın | Microsoft Docs
description: Azure IoT Edge ağ geçidi cihazlarına bağlanmak için aşağı akış veya yaprak cihazları yapılandırma.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/08/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 20de7bc55a62a44d1fa852d86705e7596e1776d6
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75434434"
---
# <a name="connect-a-downstream-device-to-an-azure-iot-edge-gateway"></a>Bir Azure IOT Edge ağ geçidi için bir aşağı akış cihazı bağlayın

Bu makalede, aşağı akış cihazları arasında güvenilir bir bağlantı kurmak ve saydam ağ geçitleri IoT Edge için yönergeler sağlanmaktadır. Saydam bir ağ geçidi senaryosunda, bir veya daha fazla cihaz iletilerini IoT Hub bağlantısını tutan tek bir ağ geçidi cihazından geçirebilir. Bir aşağı akış cihaz herhangi bir uygulama veya ile oluşturulan bir kimliğe sahiptir platform olabilir [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub) bulut hizmeti. Çoğu durumda, bu uygulamaları kullanma [Azure IOT cihaz SDK'sını](../iot-hub/iot-hub-devguide-sdks.md). Bir aşağı akış cihazı, IoT Edge ağ geçidi cihazının kendisi üzerinde çalışan bir uygulama bile olabilir. 

Başarılı bir saydam ağ geçidi bağlantısı kurmak için üç genel adım vardır. Bu makalede üçüncü adım ele alınmaktadır:

1. Ağ Geçidi cihazının aşağı akış cihazlarına güvenli bir şekilde bağlanması, aşağı akış cihazlarından iletişim alması ve iletileri uygun hedefe yönlendirmesi gerekir. Daha fazla bilgi için bkz. bir [IoT Edge cihazını saydam bir ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md).
2. Aşağı akış cihazı IoT Hub kimlik doğrulaması yapabilmek ve ağ geçidi cihazından iletişim kurmayı bilmek için bir cihaz kimliğine ihtiyaç duyuyor. Daha fazla bilgi için bkz. [Azure 'da bir aşağı akış cihazının kimliğini doğrulama IoT Hub](how-to-authenticate-downstream-device.md).
3. **Aşağı akış cihazının ağ geçidi cihazına güvenli bir şekilde bağlanması gerekir.**

Bu makalede, aşağı akış cihaz bağlantıları olan yaygın sorunlar tanımlanmakta ve aşağı akış cihazları tarafından ayarlama işleminde size yol gösterir: 

* Aktarım Katmanı Güvenliği (TLS) açıklayan ve sertifika ile ilgili temel bilgiler. 
* TLS kitaplıkları farklı işletim sistemleri arasında nasıl çalıştığı ve her işletim sistemi sertifikaları ile nasıl ilgileneceğini açıklar.
* Azure IOT walking başlamanıza yardımcı olmak üzere çeşitli dillerde örnekleri başlatıldı. 

Bu makalede, koşulları *ağ geçidi* ve *IOT Edge ağ geçidi* saydam bir ağ geçidi olarak yapılandırılmış bir IOT Edge cihazı bakın. 

## <a name="prerequisites"></a>Ön koşullar 

* Bir IoT Edge cihazı yapılandırma bölümünde oluşturulan **Azure-iot-test-only. root. ca. cert. Pee** sertifika dosyasını, aşağı akış cihazınızda bulunan [saydam bir ağ geçidi olarak davranacak şekilde yapılandırın](how-to-create-transparent-gateway.md) . Aşağı akış cihazınız, ağ geçidi cihazının kimliğini doğrulamak için bu sertifikayı kullanır. 
* [Bir aşağı akış cihazının Azure IoT Hub kimlik doğrulaması](how-to-authenticate-downstream-device.md)bölümünde açıklandığı gibi ağ geçidi cihazını işaret eden değiştirilmiş bağlantı dizesine sahip olmalıdır.

## <a name="prepare-a-downstream-device"></a>Bir aşağı akış cihazı hazırlama

Bir aşağı akış cihaz herhangi bir uygulama veya ile oluşturulan bir kimliğe sahiptir platform olabilir [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub) bulut hizmeti. Çoğu durumda, bu uygulamaları kullanma [Azure IOT cihaz SDK'sını](../iot-hub/iot-hub-devguide-sdks.md). Bir aşağı akış cihazı, IoT Edge ağ geçidi cihazının kendisi üzerinde çalışan bir uygulama bile olabilir. Ancak, başka bir IoT Edge cihaz IoT Edge bir ağ geçidinin bir aşağı akış olamaz. 

>[!NOTE]
>IoT Hub kayıtlı kimliklere sahip IoT cihazları, farklı işlem, donanım veya işlevleri tek bir cihazda yalıtmak için [Modül TWINS](../iot-hub/iot-hub-devguide-module-twins.md) kullanabilir. IoT Edge ağ geçitleri, simetrik anahtar kimlik doğrulaması kullanan ancak X. 509.440 sertifikası kimlik doğrulaması olmadan aşağı akış modülü bağlantılarını destekler 

Bir IOT Edge ağ geçidi için bir aşağı akış cihaza bağlanmak için iki şey gerekir:

* Bir cihaz veya uygulama ağ geçidine bağlanmak için gereken bilgileri ile eklenmiş bir IOT Hub cihaz bağlantı dizesi ile yapılandırılmış. 

    Bu adım, [bir aşağı akış cihazının Azure IoT Hub kimlik doğrulaması](how-to-authenticate-downstream-device.md)konusunda açıklanmaktadır.

* Ağ Geçidi cihazına TLS bağlantılarını doğrulamak için cihazın veya uygulamanın ağ geçidinin **kök CA** sertifikasına güvenmesi gerekebilir. 

    Bu adım, makalenin geri kalanında ayrıntılı olarak açıklanmaktadır. Bu adım iki şekilde gerçekleştirilebilir: CA sertifikasını işletim sisteminin sertifika deposuna yükleyerek veya (belirli diller için) Azure IoT SDK 'larını kullanarak uygulamalar içindeki sertifikaya başvurarak.

## <a name="tls-and-certificate-fundamentals"></a>TLS ve sertifika ile ilgili temel bilgiler

İnternet üzerinden oluşan yalnızca herhangi diğer güvenli istemci/sunucu iletişimini güvenli bir aşağı akış cihazları IOT Edge bağlanma zorluklarıyla gibidir. Bir istemci ve sunucu güvenli bir şekilde kullanarak internet üzerinden iletişim [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). TLS standardı kullanılarak oluşturulan [ortak anahtar altyapısı (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure) sertifikaları yapılar çağırılır. TLS oldukça ilgili bir belirtimdir ve iki uç noktanın güvenliğini sağlamaya yönelik çok çeşitli konuları ele alır. Bu bölümde, cihazları bir IoT Edge ağ geçidine güvenli bir şekilde bağlamak için ilgili kavramlar özetlenmektedir.

Bir istemci bir sunucuya bağlandığında, sunucu adı verilen sertifikaları zincirine sunar *sunucu sertifika zinciri*. Bir sertifika zinciri, genellikle bir kök sertifika yetkilisi (CA) sertifikası, bir veya daha fazla ara CA sertifikalarının ve son olarak sunucunun sertifikanın kendisi oluşur. Bir istemci, şifreli olarak tüm sunucu sertifika zinciri doğrulayarak sunucusuyla güven oluşturur. Sunucu sertifikası zincirinin bu istemci doğrulaması, *sunucu zinciri doğrulaması*olarak adlandırılır. İstemci, hizmeti, sahip olma *kanıtı*adlı bir işlemde sunucu sertifikasıyla ilişkili özel anahtara sahip olma işlemini kanıtlamak üzere şifreleme konusunda sizi bir şekilde ele alır. Sunucu zinciri doğrulama ve sahip olma kanıtı 'nın birleşimi *sunucu kimlik doğrulaması*olarak adlandırılır. Sunucu sertifika zinciri doğrulamak için bir istemci bir kopyasını oluşturun (veya sorun) sunucu sertifikasının kullanılan kök CA sertifikası gerekiyor. İstemci sorunsuz bir işlem bu normalde Web sitelerine bağlanmak, bir tarayıcı ile yaygın olarak kullanılan CA sertifikalarını önceden yapılandırılmış olarak gelir. 

Cihaz Azure IOT Hub'ına bağlandığında istemci cihazdır ve sunucu IOT hub'ı bulut hizmetidir. IOT hub'ı bulut hizmeti olarak adlandırılan bir kök CA sertifikası tarafından desteklenen **Baltimore CyberTrust Root**, yaygın olarak kullanılan ve genel olarak kullanılabilir. IOT hub'ı CA sertifika çoğu cihazlarda zaten yüklenmiş olduğundan, birçok TLS uygulamaları (OpenSSL, Schannel, LibreSSL) sunucu sertifika doğrulaması sırasında otomatik olarak kullanır. IOT Hub'ına başarılı bir şekilde bağlanabilir bir cihaz bir IOT Edge ağ geçidi bağlantısı kurulurken sorunlarla karşılaşabilirsiniz.

Bir IOT Edge ağ geçidi için bir cihaz bağlandığında, aşağı akış cihaz istemci ve ağ geçidi cihazı sunucu. Azure IOT Edge, uygun gördüğünüz ancak ağ geçidi sertifika zincirleri oluşturmak işleçleri (veya kullanıcılar) sağlar. İşleç Baltimore gibi bir ortak CA sertifika veya otomatik olarak imzalanan (veya şirket içi) bir kök CA sertifikasını kullanın tercih edebilirsiniz. Ortak CA sertifikaları genellikle ilişkili bir maliyeti vardır, bu nedenle genellikle üretim senaryolarında kullanılır. CA sertifikaları otomatik olarak imzalanan geliştirme ve test için tercih edilir. Giriş bölümünde listelenen saydam ağ geçidi kurulum makaleleri otomatik olarak imzalanan kök CA sertifikalarını kullanır. 

Bir IOT Edge ağ geçidi için bir otomatik olarak imzalanan kök CA sertifikası kullandığınızda, üzerinde yüklü olması gerekir veya ağ geçidine bağlanmaya tüm aşağı akış cihazları için sağlanan. 

![Ağ geçidi sertifikası Kurulumu](./media/how-to-create-transparent-gateway/gateway-setup.png)

Bazı üretim uygulamalarını ve IOT Edge sertifikalar hakkında daha fazla bilgi için bkz: [IOT Edge sertifika kullanım ayrıntılarını](iot-edge-certs.md).

## <a name="provide-the-root-ca-certificate"></a>Kök CA sertifikasını sağlama

Ağ Geçidi cihazının sertifikalarını doğrulamak için, aşağı akış cihazının kök CA sertifikasının kendine ait bir kopyasına ihtiyacı vardır. Test sertifikaları oluşturmak için IoT Edge git deposunda verilen betikleri kullandıysanız, kök CA sertifikasına **Azure-iot-test-only. root. ca. cert. ped**adı verilir. Zaten diğer aşağı akış cihazı hazırlama adımlarının bir parçası değilseniz, bu sertifika dosyasını aşağı akış cihazınızdaki herhangi bir dizine taşıyın. Sertifika dosyasını taşımak için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) veya [Güvenli kopya Protokolü](https://www.ssh.com/ssh/scp/) gibi bir işlev gibi bir hizmet kullanabilirsiniz.

## <a name="install-certificates-in-the-os"></a>İşletim sistemine sertifika yükler

Kök CA sertifikasını işletim sisteminin sertifika deposuna yüklemek genellikle çoğu uygulamanın kök CA sertifikasını kullanmasına izin verir. OS sertifika deposunu kullanmayan, ancak düğüm çalışma zamanının iç sertifika deposunu kullanan NodeJS uygulamaları gibi bazı özel durumlar vardır. Sertifikayı işletim sistemi düzeyinde yükleyemezseniz [Azure IoT SDK 'larını kullanarak sertifikaları kullanmaya](#use-certificates-with-azure-iot-sdks)devam edin. 

### <a name="ubuntu"></a>Ubuntu

Aşağıdaki komutlar bir Ubuntu konakta bir CA sertifikası yüklemek nasıl bir örnektir. Bu örnekte, ön koşullar makalelerindeki **Azure-iot-test-only. root. ca. cert. ped** sertifikasını kullandığınızı ve sertifikayı aşağı akış cihazında bir konuma kopyaladığınızı varsaymaktadır.

```bash
sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
sudo update-ca-certificates
```

"/Etc/SSL/certs..içindeki sertifikaları güncelleştirme" adlı bir ileti görmeniz gerekir. 1 eklenen, 0 kaldırıldı; bitti. "

### <a name="windows"></a>Windows

Aşağıdaki adımları, bir Windows konak üzerinde bir CA sertifikası yüklemek nasıl bir örnektir. Bu örnekte, ön koşullar makalelerindeki **Azure-iot-test-only. root. ca. cert. ped** sertifikasını kullandığınızı ve sertifikayı aşağı akış cihazında bir konuma kopyaladığınızı varsaymaktadır.

PowerShell 'in [Import-Certificate](https://docs.microsoft.com/powershell/module/pkiclient/import-certificate?view=win10-ps) ' i kullanarak sertifikaları yönetici olarak yükleyebilirsiniz:

```powershell
import-certificate  <file path>\azure-iot-test-only.root.ca.cert.pem -certstorelocation cert:\LocalMachine\root
```

Ayrıca, **Certlm** yardımcı programını kullanarak da sertifikalar yükleyebilirsiniz: 

1. Başlat menüsünde arayın ve seçin **bilgisayar sertifikalarını yönetme**. Bir hizmet olarak adlandırılan **certlm** açılır.
2. Gidin **sertifikalar - yerel bilgisayar** > **güvenilen kök sertifika yetkilileri**.
3. Sağ **sertifikaları** seçip **tüm görevler** > **alma**. Sertifika İçeri Aktarma Sihirbazı'nı başlatma. 
4. Yönergelerine uygun olarak aşağıdaki adımları uygulayın ve sertifika dosyasını içe `<path>/azure-iot-test-only.root.ca.cert.pem`. İşlem tamamlandığında bir "Başarıyla içeri aktarıldı" iletisini görmeniz gerekir. 

Bu makalenin devamındaki .NET örneğinde gösterildiği gibi sertifikaları .NET API'lerini program aracılığıyla kullanarak da yükleyebilirsiniz. 

Genellikle TLS yığın adlı sağlanan Windows uygulamaları kullanın [Schannel](https://docs.microsoft.com/windows/desktop/com/schannel) TLS üzerinden güvenli bir şekilde bağlanmak için. Schannel *gerektirir* sertifikalarını TLS bağlantı çalışmadan önce Windows sertifika deposuna yüklenmesi.

## <a name="use-certificates-with-azure-iot-sdks"></a>Azure IOT SDK'ları ile sertifikaları kullan

Bu bölümde, Azure IOT SDK'ları basit örnek uygulamalar kullanarak bir IOT Edge cihazı nasıl bağlanacağını açıklar. Tüm örnekler cihaz istemcisi bağlanma ve telemetri iletilerini göndermek için ağ geçidi, daha sonra bağlantıyı kapatmak ve çıkış hedefidir. 

Uygulama düzeyi örnekleri kullanmadan önce hazır iki şey vardır:

* Aşağı akış cihazınızın IoT Hub bağlantı dizesi, ağ geçidi cihazını işaret etmek üzere değiştirilmiştir ve aşağı akış cihazınızın kimliğini doğrulamak için gereken tüm sertifikaları IoT Hub. Daha fazla bilgi için bkz. [Azure 'da bir aşağı akış cihazının kimliğini doğrulama IoT Hub](how-to-authenticate-downstream-device.md).

* Kök CA sertifikası kopyalanır ve herhangi bir aşağı akış Cihazınızda kaydedilmiş tam yolu.

    Örneğin, `<path>/azure-iot-test-only.root.ca.cert.pem`. 

### <a name="nodejs"></a>NodeJS

Bu bölümde, bir IOT Edge ağ geçidi için bir Azure IOT NodeJS cihaz istemcisi bağlanmak için örnek bir uygulama sağlar. NodeJS uygulamaları için, kök CA sertifikasını burada gösterildiği gibi uygulama düzeyinde yüklemelisiniz. NodeJS uygulamaları sistemin sertifika deposunu kullanmaz. 

1. Örneğe ilişkin edinme **edge_downstream_device.js** gelen [Node.js için Azure IOT cihaz SDK'sı örnekleri deposu](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples). 
2. İnceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun **readme.md** dosya. 
3. Edge_downstream_device.js dosyasında güncelleştirme **connectionString** ve **edge_ca_cert_path** değişkenleri. 
4. Cihazınızda örneği çalıştırmak yönergeler için SDK belgelerine bakın. 

Kullanmakta olduğunuz örnek anlamak için aşağıdaki kod parçacığını nasıl istemci SDK'sı sertifika dosyasını okur ve güvenli bir TLS bağlantı kurmak için kullandığı verilmiştir: 

```javascript
// Provide the Azure IoT device client via setOptions with the X509
// Edge root CA certificate that was used to setup the Edge runtime
var options = {
    ca : fs.readFileSync(edge_ca_cert_path, 'utf-8'),
};
```

### <a name="net"></a>.NET

Bu bölüm, bir IOT Edge ağ geçidi için bir Azure IOT .NET cihaz istemcisi bağlanmak için örnek bir uygulama tanıtır. Ancak, .NET uygulamaları otomatik olarak hem Linux hem de Windows konaklarda sistemin sertifika deposunda yüklü sertifikalarını kullanmanız mümkün değildir.

1. Örneğe ilişkin edinme **EdgeDownstreamDevice** gelen [IOT Edge .NET örnekler klasörüne](https://github.com/Azure/iotedge/tree/master/samples/dotnet/EdgeDownstreamDevice). 
2. İnceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun **readme.md** dosya. 
3. İçinde **özellikleri / launchSettings.json** dosyası, güncelleştirme **DEVICE_CONNECTION_STRING** ve **CA_CERTIFICATE_PATH** değişkenleri. Konak sisteminde güvenilir sertifika deposunda yüklü sertifika kullanmak istiyorsanız, bu değişken boş bırakın. 
4. Cihazınızda örneği çalıştırmak yönergeler için SDK belgelerine bakın. 

Program aracılığıyla bir .NET uygulaması aracılığıyla sertifika deposundaki güvenilen bir sertifika yüklemek için başvurmak **InstallCACert()** işlevi **EdgeDownstreamDevice / Program.cs** dosya. Bu işlem, başka hiçbir etkisi olmadan aynı değerlerle birden çok kez çalıştırılabilir. Bu nedenle etkili olur. 

### <a name="c"></a>C

Bu bölüm, bir IOT Edge ağ geçidi için bir Azure IOT C cihaz istemcisi bağlanmak için örnek bir uygulama tanıtır. C SDK'sı OpenSSL WolfSSL ve Schannel dahil olmak üzere birçok TLS kitaplıkları ile çalışır. Daha fazla bilgi için [Azure IOT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c). 

1. Alma **iotedge_downstream_device_sample** gelen uygulama [C örnekleri için Azure IOT cihaz SDK'sını](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples). 
2. İnceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun **readme.md** dosya. 
3. İotedge_downstream_device_sample.c dosyasında güncelleştirme **connectionString** ve **edge_ca_cert_path** değişkenleri. 
4. Cihazınızda örneği çalıştırmak yönergeler için SDK belgelerine bakın. 

C için Azure IOT cihaz SDK'sını istemcisi kurarken bir CA sertifikası kaydetmek için bir seçenek sunar. Bu işlem, herhangi bir sertifika yüklemez, ancak bunun yerine sertifikanın bir dize biçimi bellekte kullanır. Kaydedilen sertifikayı, temel alınan TLS yığına bağlantı kurarken sağlanır. 

```C
(void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
```

Windows konaklarda OpenSSL veya başka bir TLS kitaplığı kullanmıyorsanız SDK'sı varsayılan Schannel kullanarak. Schannel çalışmak üzere IOT Edge kök CA sertifikasını kullanarak ayarlanmamış Windows sertifika deposunda yüklenmelidir `IoTHubDeviceClient_SetOption` işlemi. 

### <a name="java"></a>Java

Bu bölüm, bir IOT Edge ağ geçidi için bir Azure IOT Java cihaz istemcisi bağlanmak için örnek bir uygulama tanıtır. 

1. Örneğe ilişkin edinme **gönderme olayı** gelen [Java örnekleri için Azure IOT cihaz SDK'sını](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples). 
2. İnceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun **readme.md** dosya. 
3. Cihazınızda örneği çalıştırmak yönergeler için SDK belgelerine bakın.

### <a name="python"></a>Python

Bu bölüm, bir IOT Edge ağ geçidi için bir Azure IOT Python cihaz istemcisi bağlanmak için örnek bir uygulama tanıtır. 

1. [Python örnekleri Için Azure IoT CIHAZ SDK](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/advanced-edge-scenarios)'sından **send_message** örneğini alın. 
2. IoT Edge kapsayıcıda veya bir hata ayıklama senaryosunda çalışırken, `EdgeHubConnectionString` ve `EdgeModuleCACertificateFile` ortam değişkenlerini ayarlamış olduğunuzdan emin olun.
3. Cihazınızda örneği çalıştırmak yönergeler için SDK belgelerine bakın. 


## <a name="test-the-gateway-connection"></a>Ağ geçidi bağlantısını test etme

Aşağı akış cihazınızın ağ geçidi cihazına bağlanabilme sınamasını sınamak için bu örnek komutu kullanın: 

```cmd/sh
openssl s_client -connect mygateway.contoso.com:8883 -CAfile <CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem -showcerts
```

Bu komut MQTTS üzerinden bağlantıları sınar (bağlantı noktası 8883). Farklı bir protokol kullanıyorsanız, AMQPS (5671) veya HTTPS (433) için gereken komutu ayarlayın

Bu komutun çıktısı, zincirdeki tüm sertifikalarla ilgili bilgiler de dahil olmak üzere uzun olabilir. Bağlantınız başarılı olursa, `Verification: OK` veya `Verify return code: 0 (ok)`gibi bir çizgi görürsünüz.

![Ağ Geçidi bağlantısını doğrulama](./media/how-to-connect-downstream-device/verification-ok.png)

## <a name="troubleshoot-the-gateway-connection"></a>Ağ Geçidi bağlantısı sorunlarını giderme

Yaprak cihazınızın ağ geçidi cihazına aralıklı olarak bağlantısı varsa, çözüm için aşağıdaki adımları deneyin. 

1. Bağlantı dizesindeki ağ geçidi ana bilgisayar adı, ağ geçidi cihazındaki IoT Edge config. YAML dosyasında ana bilgisayar adı ile aynı.
2. Ağ geçidi ana bilgisayar adı bir IP adresi olarak çözülebilir mi? Aralıklı bağlantıları DNS kullanarak ya da yaprak cihaza bir konak dosyası girişi ekleyerek çözebilirsiniz.
3. İletişim bağlantı noktaları güvenlik duvarınızdan açık mı? Kullanılan protokolü temel alan iletişim (MQTTS: 8883/AMQPS: 5671/HTTPS: 433), aşağı akış cihazı ile saydam IoT Edge arasında olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

IOT Edge nasıl genişletebileceğinizi öğreneceğiz [çevrimdışı özellikleri](offline-capabilities.md) aşağı akış cihazlara. 
