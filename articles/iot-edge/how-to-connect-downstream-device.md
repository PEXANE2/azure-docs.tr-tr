---
title: Aşağı Akış cihazları - Azure IOT Edge bağlayın | Microsoft Docs
description: Azure IoT Edge ağ geçidi cihazlarına bağlanmak için aşağı akış veya yaprak cihazları yapılandırma.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/08/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 719ec736fd2f28f8d8b3b226109bc988c872d10f
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74457130"
---
# <a name="connect-a-downstream-device-to-an-azure-iot-edge-gateway"></a>Bir Azure IOT Edge ağ geçidi için bir aşağı akış cihazı bağlayın

Bu makalede, aşağı akış cihazları arasında güvenilir bir bağlantı kurmak ve saydam ağ geçitleri IoT Edge için yönergeler sağlanmaktadır. Saydam bir ağ geçidi senaryosunda, bir veya daha fazla cihaz iletilerini IoT Hub bağlantısını tutan tek bir ağ geçidi cihazından geçirebilir. Bir aşağı akış cihazı, [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) bulut hizmeti ile oluşturulmuş bir kimliği olan herhangi bir uygulama veya platform olabilir. Çoğu durumda bu uygulamalar [Azure IoT cihaz SDK 'sını](../iot-hub/iot-hub-devguide-sdks.md)kullanır. Bir aşağı akış cihazı, IoT Edge ağ geçidi cihazının kendisi üzerinde çalışan bir uygulama bile olabilir. 

Başarılı bir saydam ağ geçidi bağlantısı kurmak için üç genel adım vardır. Bu makalede üçüncü adım ele alınmaktadır:

1. Ağ Geçidi cihazının aşağı akış cihazlarına güvenli bir şekilde bağlanması, aşağı akış cihazlarından iletişim alması ve iletileri uygun hedefe yönlendirmesi gerekir. Daha fazla bilgi için bkz. bir [IoT Edge cihazını saydam bir ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md).
2. Aşağı akış cihazı IoT Hub kimlik doğrulaması yapabilmek ve ağ geçidi cihazından iletişim kurmayı bilmek için bir cihaz kimliğine ihtiyaç duyuyor. Daha fazla bilgi için bkz. [Azure 'da bir aşağı akış cihazının kimliğini doğrulama IoT Hub](how-to-authenticate-downstream-device.md).
3. **Aşağı akış cihazının ağ geçidi cihazına güvenli bir şekilde bağlanabiliyor olması gerekir.**

Bu makalede, aşağı akış cihaz bağlantıları olan yaygın sorunlar tanımlanmakta ve aşağı akış cihazları tarafından ayarlama işleminde size yol gösterir: 

* Aktarım Katmanı Güvenliği (TLS) açıklayan ve sertifika ile ilgili temel bilgiler. 
* TLS kitaplıkları farklı işletim sistemleri arasında nasıl çalıştığı ve her işletim sistemi sertifikaları ile nasıl ilgileneceğini açıklar.
* Azure IOT walking başlamanıza yardımcı olmak üzere çeşitli dillerde örnekleri başlatıldı. 

Bu makalede, terimler *ağ geçidi* ve *IoT Edge Ağ* geçidi, saydam bir ağ geçidi olarak yapılandırılmış bir IoT Edge cihazına başvurur. 

## <a name="prerequisites"></a>Ön koşullar 

Bir IoT Edge cihazı yapılandırma bölümünde oluşturulan **Azure-iot-test-only. root. ca. cert. Pee** sertifika dosyasını, aşağı akış cihazınızda bulunan [saydam bir ağ geçidi olarak davranacak şekilde yapılandırın](how-to-create-transparent-gateway.md) . Aşağı akış cihazınız, ağ geçidi cihazının kimliğini doğrulamak için bu sertifikayı kullanır. 

## <a name="prepare-a-downstream-device"></a>Bir aşağı akış cihazı hazırlama

Bir aşağı akış cihazı, [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) bulut hizmeti ile oluşturulmuş bir kimliği olan herhangi bir uygulama veya platform olabilir. Çoğu durumda bu uygulamalar [Azure IoT cihaz SDK 'sını](../iot-hub/iot-hub-devguide-sdks.md)kullanır. Bir aşağı akış cihazı, IoT Edge ağ geçidi cihazının kendisi üzerinde çalışan bir uygulama bile olabilir. Ancak, başka bir IoT Edge cihaz IoT Edge bir ağ geçidinin bir aşağı akış olamaz. 

>[!NOTE]
>IoT Hub kayıtlı kimliklere sahip IoT cihazları, farklı işlem, donanım veya işlevleri tek bir cihazda yalıtmak için [Modül TWINS](../iot-hub/iot-hub-devguide-module-twins.md) kullanabilir. IoT Edge ağ geçitleri, simetrik anahtar kimlik doğrulaması kullanan ancak X. 509.440 sertifikası kimlik doğrulaması olmadan aşağı akış modülü bağlantılarını destekler 

Bir IOT Edge ağ geçidi için bir aşağı akış cihaza bağlanmak için iki şey gerekir:

* Bir cihaz veya uygulama ağ geçidine bağlanmak için gereken bilgileri ile eklenmiş bir IOT Hub cihaz bağlantı dizesi ile yapılandırılmış. 

    Bu adım, [bir aşağı akış cihazının Azure IoT Hub kimlik doğrulaması](how-to-authenticate-downstream-device.md)konusunda açıklanmaktadır.

* Ağ Geçidi cihazına TLS bağlantılarını doğrulamak için cihazın veya uygulamanın ağ geçidinin **kök CA** sertifikasına güvenmesi gerekebilir. 

    Bu adım, makalenin geri kalanında ayrıntılı olarak açıklanmaktadır. Bu adım iki şekilde gerçekleştirilebilir: CA sertifikasını işletim sisteminin sertifika deposuna yükleyerek veya (belirli diller için) Azure IoT SDK 'larını kullanarak uygulamalar içindeki sertifikaya başvurarak.

## <a name="tls-and-certificate-fundamentals"></a>TLS ve sertifika ile ilgili temel bilgiler

İnternet üzerinden oluşan yalnızca herhangi diğer güvenli istemci/sunucu iletişimini güvenli bir aşağı akış cihazları IOT Edge bağlanma zorluklarıyla gibidir. İstemci ve sunucu, [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security)kullanarak internet üzerinden güvenli bir şekilde iletişim kurar. TLS, sertifikalar adlı standart [ortak anahtar altyapısı (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure) yapıları kullanılarak oluşturulmuştur. TLS oldukça ilgili bir belirtimdir ve iki uç noktanın güvenliğini sağlamaya yönelik çok çeşitli konuları ele alır. Bu bölümde, cihazları bir IoT Edge ağ geçidine güvenli bir şekilde bağlamak için ilgili kavramlar özetlenmektedir.

İstemci bir sunucuya bağlandığı zaman sunucu, *sunucu sertifika zinciri*olarak adlandırılan bir sertifika zinciri sunar. Bir sertifika zinciri, genellikle bir kök sertifika yetkilisi (CA) sertifikası, bir veya daha fazla ara CA sertifikalarının ve son olarak sunucunun sertifikanın kendisi oluşur. Bir istemci, şifreli olarak tüm sunucu sertifika zinciri doğrulayarak sunucusuyla güven oluşturur. Sunucu sertifikası zincirinin bu istemci doğrulaması, *sunucu zinciri doğrulaması*olarak adlandırılır. İstemci, hizmeti, sahip olma *kanıtı*adlı bir işlemde sunucu sertifikasıyla ilişkili özel anahtara sahip olma işlemini kanıtlamak üzere şifreleme konusunda sizi bir şekilde ele alır. Sunucu zinciri doğrulama ve sahip olma kanıtı 'nın birleşimi *sunucu kimlik doğrulaması*olarak adlandırılır. Sunucu sertifika zinciri doğrulamak için bir istemci bir kopyasını oluşturun (veya sorun) sunucu sertifikasının kullanılan kök CA sertifikası gerekiyor. İstemci sorunsuz bir işlem bu normalde Web sitelerine bağlanmak, bir tarayıcı ile yaygın olarak kullanılan CA sertifikalarını önceden yapılandırılmış olarak gelir. 

Cihaz Azure IOT Hub'ına bağlandığında istemci cihazdır ve sunucu IOT hub'ı bulut hizmetidir. IoT Hub bulut hizmeti, genel olarak kullanılabilen ve yaygın olarak kullanılan, **Baltimore CyberTrust kökü**adlı BIR kök CA sertifikası tarafından desteklenir. IOT hub'ı CA sertifika çoğu cihazlarda zaten yüklenmiş olduğundan, birçok TLS uygulamaları (OpenSSL, Schannel, LibreSSL) sunucu sertifika doğrulaması sırasında otomatik olarak kullanır. IOT Hub'ına başarılı bir şekilde bağlanabilir bir cihaz bir IOT Edge ağ geçidi bağlantısı kurulurken sorunlarla karşılaşabilirsiniz.

Bir IOT Edge ağ geçidi için bir cihaz bağlandığında, aşağı akış cihaz istemci ve ağ geçidi cihazı sunucu. Azure IOT Edge, uygun gördüğünüz ancak ağ geçidi sertifika zincirleri oluşturmak işleçleri (veya kullanıcılar) sağlar. İşleç Baltimore gibi bir ortak CA sertifika veya otomatik olarak imzalanan (veya şirket içi) bir kök CA sertifikasını kullanın tercih edebilirsiniz. Ortak CA sertifikaları genellikle ilişkili bir maliyeti vardır, bu nedenle genellikle üretim senaryolarında kullanılır. CA sertifikaları otomatik olarak imzalanan geliştirme ve test için tercih edilir. Giriş bölümünde listelenen saydam ağ geçidi kurulum makaleleri otomatik olarak imzalanan kök CA sertifikalarını kullanır. 

Bir IOT Edge ağ geçidi için bir otomatik olarak imzalanan kök CA sertifikası kullandığınızda, üzerinde yüklü olması gerekir veya ağ geçidine bağlanmaya tüm aşağı akış cihazları için sağlanan. 

![Ağ geçidi sertifikası Kurulumu](./media/how-to-create-transparent-gateway/gateway-setup.png)

IoT Edge sertifikaları ve bazı üretim etkileri hakkında daha fazla bilgi edinmek için bkz. [IoT Edge sertifikası kullanım ayrıntıları](iot-edge-certs.md).

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

1. Başlat menüsünde **bilgisayar sertifikalarını Yönet**' i arayın ve seçin. **Certlm** adlı bir yardımcı program açılır.
2. **Sertifikalar-Yerel bilgisayar** > **Güvenilen kök sertifika yetkilileri**' ne gidin.
3. **Sertifikalar** ' a sağ tıklayın ve **Içeri aktarma** > **Tüm görevler** ' i seçin. Sertifika İçeri Aktarma Sihirbazı'nı başlatma. 
4. `<path>/azure-iot-test-only.root.ca.cert.pem`, yönlendirilmiş ve içeri aktarma sertifika dosyası olan adımları izleyin. İşlem tamamlandığında bir "Başarıyla içeri aktarıldı" iletisini görmeniz gerekir. 

Bu makalenin devamındaki .NET örneğinde gösterildiği gibi sertifikaları .NET API'lerini program aracılığıyla kullanarak da yükleyebilirsiniz. 

Genellikle uygulamalar, TLS üzerinden güvenli bir şekilde bağlanmak için [Schannel](https://docs.microsoft.com/windows/desktop/com/schannel) adlı Windows tarafından sunulan TLS yığınını kullanır. Schannel, bir TLS bağlantısı kurmayı denemeden önce Windows sertifika deposunda tüm sertifikaların yüklenmesini *gerektirir* .

## <a name="use-certificates-with-azure-iot-sdks"></a>Azure IOT SDK'ları ile sertifikaları kullan

Bu bölümde, Azure IOT SDK'ları basit örnek uygulamalar kullanarak bir IOT Edge cihazı nasıl bağlanacağını açıklar. Tüm örnekler cihaz istemcisi bağlanma ve telemetri iletilerini göndermek için ağ geçidi, daha sonra bağlantıyı kapatmak ve çıkış hedefidir. 

Uygulama düzeyi örnekleri kullanmadan önce hazır iki şey vardır:

* Aşağı akış cihazınızın IoT Hub bağlantı dizesi, ağ geçidi cihazını işaret etmek üzere değiştirilmiştir ve aşağı akış cihazınızın kimliğini doğrulamak için gereken tüm sertifikaları IoT Hub. Daha fazla bilgi için bkz. [Azure 'da bir aşağı akış cihazının kimliğini doğrulama IoT Hub](how-to-authenticate-downstream-device.md).

* Kök CA sertifikası kopyalanır ve herhangi bir aşağı akış Cihazınızda kaydedilmiş tam yolu.

    Örneğin, `<path>/azure-iot-test-only.root.ca.cert.pem`. 

### <a name="nodejs"></a>NodeJS

Bu bölümde, bir IOT Edge ağ geçidi için bir Azure IOT NodeJS cihaz istemcisi bağlanmak için örnek bir uygulama sağlar. NodeJS uygulamaları için, kök CA sertifikasını burada gösterildiği gibi uygulama düzeyinde yüklemelisiniz. NodeJS uygulamaları sistemin sertifika deposunu kullanmaz. 

1. [Node. js örnek deposu Için Azure IoT cihaz SDK 'sına](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples) **edge_downstream_device. js** örneğini alın. 
2. **README.MD** dosyasını inceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun. 
3. Edge_downstream_device. js dosyasında, **ConnectionString** ve **edge_ca_cert_path** değişkenlerini güncelleştirin. 
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

1. [IoT Edge .NET örnekleri klasöründen](https://github.com/Azure/iotedge/tree/master/samples/dotnet/EdgeDownstreamDevice) **Edgedownstreamdevice** örneğine yönelik örneği alın. 
2. **README.MD** dosyasını inceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun. 
3. **Properties/launchSettings. JSON** dosyasında **DEVICE_CONNECTION_STRING** ve **CA_CERTIFICATE_PATH** değişkenlerini güncelleştirin. Konak sisteminde güvenilir sertifika deposunda yüklü sertifika kullanmak istiyorsanız, bu değişken boş bırakın. 
4. Cihazınızda örneği çalıştırmak yönergeler için SDK belgelerine bakın. 

Bir .NET uygulaması aracılığıyla sertifika deposuna programlı olarak güvenilir bir sertifika yüklemek için, **Edgedownstreamdevice/program.cs** dosyasındaki **ınstallcacert ()** işlevine bakın. Bu işlem, başka hiçbir etkisi olmadan aynı değerlerle birden çok kez çalıştırılabilir. Bu nedenle etkili olur. 

### <a name="c"></a>C

Bu bölüm, bir IOT Edge ağ geçidi için bir Azure IOT C cihaz istemcisi bağlanmak için örnek bir uygulama tanıtır. C SDK'sı OpenSSL WolfSSL ve Schannel dahil olmak üzere birçok TLS kitaplıkları ile çalışır. Daha fazla bilgi için bkz. [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c). 

1. [C örnekleri Için Azure IoT CIHAZ SDK](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples)'sinden **iotedge_downstream_device_sample** uygulamayı alın. 
2. **README.MD** dosyasını inceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun. 
3. İotedge_downstream_device_sample. c dosyasında, **ConnectionString** ve **edge_ca_cert_path** değişkenlerini güncelleştirin. 
4. Cihazınızda örneği çalıştırmak yönergeler için SDK belgelerine bakın. 

C için Azure IOT cihaz SDK'sını istemcisi kurarken bir CA sertifikası kaydetmek için bir seçenek sunar. Bu işlem, herhangi bir sertifika yüklemez, ancak bunun yerine sertifikanın bir dize biçimi bellekte kullanır. Kaydedilen sertifikayı, temel alınan TLS yığına bağlantı kurarken sağlanır. 

```C
(void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
```

Windows konaklarda OpenSSL veya başka bir TLS kitaplığı kullanmıyorsanız SDK'sı varsayılan Schannel kullanarak. Schannel 'nin çalışması için, IoT Edge kök CA sertifikasının, `IoTHubDeviceClient_SetOption` işlemi kullanılarak ayarlanmayan Windows sertifika deposuna yüklenmesi gerekir. 

### <a name="java"></a>Java

Bu bölüm, bir IOT Edge ağ geçidi için bir Azure IOT Java cihaz istemcisi bağlanmak için örnek bir uygulama tanıtır. 

1. [Java örnekleri Için Azure IoT CIHAZ SDK](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples)'dan **Send-Event** örneğini alın. 
2. **README.MD** dosyasını inceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun. 
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

Bu komut MQTTS üzerinden bağlantıları sınar (bağlantı noktası 8883). Farklı bir protokol kullanıyorsanız, AMQPS (5671) veya HTTPS (433) için gereken komutu ayarlayın.

Bu komutun çıktısı, zincirdeki tüm sertifikalarla ilgili bilgiler de dahil olmak üzere uzun olabilir. Bağlantınız başarılı olursa, `Verification: OK` veya `Verify return code: 0 (ok)`gibi bir çizgi görürsünüz.

![Ağ Geçidi bağlantısını doğrulama](./media/how-to-connect-downstream-device/verification-ok.png)

## <a name="troubleshoot-the-gateway-connection"></a>Ağ Geçidi bağlantısı sorunlarını giderme

Yaprak cihazınızın ağ geçidi cihazına aralıklı olarak bağlantısı varsa, çözüm için aşağıdaki adımları deneyin. 

1. Bağlantı dizesindeki ağ geçidi ana bilgisayar adı, ağ geçidi cihazındaki IoT Edge config. YAML dosyasında ana bilgisayar adı ile aynı.
2. Ağ geçidi ana bilgisayar adı bir IP adresi olarak çözülebilir mi? Aralıklı bağlantıları DNS kullanarak ya da yaprak cihaza bir konak dosyası girişi ekleyerek çözebilirsiniz.
3. İletişim bağlantı noktaları güvenlik duvarınızdan açık mı? Kullanılan protokolü temel alan iletişim (MQTTS: 8883/AMQPS: 5671/HTTPS: 433), aşağı akış cihazı ile saydam IoT Edge arasında olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

IoT Edge, [çevrimdışı özellikleri](offline-capabilities.md) aşağı akış cihazlarına nasıl genişletebileceğinizi öğrenin. 
