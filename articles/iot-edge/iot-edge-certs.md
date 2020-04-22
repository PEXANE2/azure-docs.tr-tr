---
title: Cihaz güvenliği sertifikaları - Azure IoT Edge | Microsoft Dokümanlar
description: Azure IoT Edge, aygıtları, modülleri ve yaprak aygıtları doğrulamak ve aralarında güvenli bağlantılar kurmak için sertifikayı kullanır.
author: stevebus
manager: philmea
ms.author: stevebus
ms.date: 10/29/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: mqtt
ms.openlocfilehash: d3e456d57d98b796fb1aea2e82de51f9fae40c68
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733173"
---
# <a name="understand-how-azure-iot-edge-uses-certificates"></a>Azure IoT Edge'in sertifikaları nasıl kullandığını anlama

IoT Edge sertifikaları, [IoT Edge hub](iot-edge-runtime.md#iot-edge-hub) çalışma zamanı modülünün kimliğini ve meşruiyetini doğrulamak için modüller ve aşağı IoT aygıtları tarafından kullanılır. Bu doğrulamalar, çalışma zamanı, modüller ve IoT aygıtları arasında TLS (aktarım katmanı güvenliği) güvenli bir bağlantı sağlar. IoT Hub kendisi gibi, IoT Edge ioT downstream (veya yaprak) cihazlar ve IoT Edge modülleri güvenli ve şifreli bir bağlantı gerektirir. Güvenli bir TLS bağlantısı kurmak için, IoT Edge hub modülü, bağlantı istemcilerine kimliğini doğrulamaları için bir sunucu sertifika zinciri sunar.

Bu makalede, IoT Edge sertifikalarının üretim, geliştirme ve test senaryolarında nasıl çalışabileceği açıklanmaktadır. Komut dosyaları farklı olsa da (Powershell vs. bash), kavramlar Linux ve Windows arasında aynıdır.

## <a name="iot-edge-certificates"></a>IoT Edge sertifikaları

Genellikle, üreticiler bir IoT Edge aygıtının son kullanıcıları değildir. Bazen ikisi arasındaki tek ilişki, son kullanıcının veya operatörün üretici tarafından yapılan genel bir aygıtı satın almasıdır. Diğer zamanlarda, üretici operatör için özel bir aygıt oluşturmak için sözleşme altında çalışır. IoT Edge sertifika tasarımı her iki senaryoyu da dikkate almaya çalışır.

Aşağıdaki şekil, IoT Edge'in sertifika kullanımını göstermektedir. İlgili varlıkların sayısına bağlı olarak kök CA sertifikası ile aygıt CA sertifikası arasında sıfır, bir veya çok sayıda ara imzalama sertifikası olabilir. Burada bir vaka gösteriyoruz.

![Tipik sertifika ilişkilerinin diyagramı](./media/iot-edge-certs/edgeCerts-general.png)

### <a name="certificate-authority"></a>Sertifika yetkilisi

Sertifika yetkilisi veya kısaca 'CA' dijital sertifika lar veren bir varlıktır. Sertifika yetkilisi, sertifika sahibi ve sertifikayı alan arasında güvenilir bir üçüncü taraf olarak görev eder. Dijital sertifika, ortak anahtarın mülkiyetini sertifikayı alan tarafından onaylar. Güven sertifikası zinciri, başlangıçta yetkili makam tarafından verilen tüm sertifikalarda güvenin temelini oluşturan bir kök sertifika vererek çalışır. Daha sonra, sahibi ek ara sertifikalar ('yaprak' sertifikaları) vermek için kök sertifikasını kullanabilir.

### <a name="root-ca-certificate"></a>Kök CA sertifikası

Kök CA sertifikası, tüm işlemin güven kaynağıdır. Üretim senaryolarında, bu CA sertifikası genellikle Baltimore, Verisign veya DigiCert gibi güvenilir bir ticari sertifika yetkilisinden satın alınır. IoT Edge aygıtlarınıza bağlanan aygıtlar üzerinde tam denetime sahipseniz, kurumsal düzeyde sertifika yetkilisi kullanmak mümkündür. Her iki durumda da, IoT Edge hub'ından tüm sertifika zinciri ona gelir, bu nedenle yaprak IoT aygıtları kök sertifikasına güvenmelidir. Kök CA sertifikasını güvenilen kök sertifika yetkilisi deposunda depolayabilir veya uygulama kodunuzda sertifika ayrıntılarını sağlayabilirsiniz.

### <a name="intermediate-certificates"></a>Ara sertifikalar

Güvenli aygıtlar oluşturmak için tipik bir üretim sürecinde, kök CA sertifikaları genellikle doğrudan, öncelikle sızıntı veya maruz kalma riski nedeniyle kullanılır. Kök CA sertifikası bir veya daha fazla ara CA sertifikası oluşturur ve dijital olarak imzalar. Yalnızca bir tane olabilir veya bu ara sertifikalar zinciri olabilir. Ara sertifikalar zinciri gerektiren senaryolar şunlardır:

* Üretici içindeki departmanlar hiyerarşisi.

* Birden fazla şirket seri bir cihazın üretiminde yer aldı.

* Kök CA satın alan ve üreticinin bu müşteri adına yaptıkları cihazları imzalaması için bir imza sertifikası elde eden bir müşteri.

Her durumda, üretici bu zincirin sonunda, son aygıta yerleştirilen aygıt CA sertifikasını imzalamak için bir ara CA sertifikası kullanır. Genellikle, bu ara sertifikalar üretim tesisinde yakından korunmaktadır. Kullanımları için hem fiziksel hem de elektronik olarak sıkı süreçlerden geçerler.

### <a name="device-ca-certificate"></a>Cihaz CA sertifikası

Aygıt CA sertifikası, işlemdeki son ara CA sertifikasından oluşturulur ve bu sertifika tarafından imzalanır. Bu sertifika, tercihen donanım güvenlik modülü (HSM) gibi güvenli depolama alanında, IoT Edge aygıtının kendisine yüklenir. Buna ek olarak, aygıt CA sertifikası benzersiz bir IoT Edge aygıtı tanımlar. Aygıt CA sertifikası diğer sertifikaları imzalayabilir.

### <a name="iot-edge-workload-ca"></a>IoT Edge İş Yükü CA

[IoT Edge Güvenlik Yöneticisi, IoT Edge](iot-edge-security-manager.md) ilk başlatıldığında, işlemin "işleç" tarafında ilk iş yükü CA sertifikası oluşturur. Bu sertifika", "aygıt CA sertifikası" tarafından oluşturulur ve imzalanır. Yalnızca başka bir ara imza sertifikası olan bu sertifika, IoT Edge çalışma zamanı tarafından kullanılan diğer sertifikaları oluşturmak ve imzalamak için kullanılır. Bugün, bu öncelikle aşağıdaki bölümde tartışılan IoT Edge hub sunucu sertifikası, ancak gelecekte IoT Edge bileşenlerinin kimlik doğrulaması için diğer sertifikaları içerebilir.

### <a name="iot-edge-hub-server-certificate"></a>IoT Edge hub sunucu sertifikası

IoT Edge hub sunucu sertifikası, IoT Edge tarafından gerekli olan TLS bağlantısının kurulması sırasında kimlik doğrulaması için yaprak aygıtlara ve modüllere sunulan gerçek sertifikadır. Bu sertifika, yaprak IoT aygıtının güvenmesi gereken kök CA sertifikasına kadar oluşturmak için kullanılan tam imza sertifikaları zincirini sunar. IoT Edge Security Manager tarafından oluşturulduğunda, bu IoT Edge hub sertifikasının ortak adı (CN), küçük harfe dönüştürmeden sonra config.yaml dosyasındaki 'ana bilgisayar adı' özelliğine ayarlanır. Bu yapılandırma, IoT Edge ile sık karşılaşılan bir karışıklık kaynağıdır.

## <a name="production-implications"></a>Üretim etkileri

Makul bir soru "neden IoT Edge 'iş yükü CA' ekstra sertifika gerekir? Doğrudan IoT Edge hub sunucu sertifikasıoluşturmak için aygıt CA sertifikasını kullanamaz mı?". Teknik olarak, olabilir. Ancak, bu "iş yükü" ara sertifikasının amacı, aygıt üreticisi ile aygıt işleci arasındaki endişeleri birbirinden ayırmaktır. Bir IoT Edge aygıtının bir müşteriden diğerine satıldığı veya aktarıldığı bir senaryo düşünün. Üretici tarafından sağlanan aygıt CA sertifikasının değişmez olmasını isteyebilirsiniz. Ancak, aygıtın çalışmasına özgü "iş yükü" sertifikaları silinmeli ve yeni dağıtım için yeniden oluşturulmalıdır.

Üretim ve işletme süreçleri ayrıldığından, üretim cihazları hazırlanırken aşağıdaki sonuçları göz önünde bulundurun:

* Herhangi bir sertifika tabanlı işlemle, kök CA sertifikası ve tüm ara CA sertifikaları, bir IoT Edge aygıtının kullanıma salınması işlemi boyunca güvenli ve izlenmelidir. IoT Edge cihaz üreticisi, ara sertifikalarının doğru şekilde saklanması ve kullanımı için güçlü süreçlere sahip olmalıdır. Buna ek olarak, aygıt CA sertifikası, tercihen bir donanım güvenlik modülü olan aygıtın kendisinde mümkün olduğunca güvenli bir depolama da tutulmalıdır.

* IoT Edge hub sunucu sertifikası, IoT Edge hub'ı tarafından bağlanan istemci aygıtlarına ve modüllerine sunulur. Aygıt CA sertifikasının ortak adı (CN), IoT Edge aygıtında config.yaml'de kullanılacak "hostname" ile aynı **olmamalıdır.** Istemciler tarafından IoT Edge'e bağlanmak için kullanılan ad (örneğin, bağlantı dizesinin GatewayHostName parametresi veya MQTT'deki CONNECT komutu üzerinden) aygıt CA sertifikasında kullanılan ortak adla aynı **olamaz.** Bu kısıtlama, IoT Edge hub'ı istemciler tarafından doğrulama için tüm sertifika zincirini sunar olmasıdır. IoT Edge hub sunucu sertifikası ve aygıt CA sertifikası her ikisi de aynı CN'ye sahipse, doğrulama döngüsüne girebilirsiniz ve sertifika geçersiz kılınar.

* Aygıt CA sertifikası, son IoT Edge sertifikalarını oluşturmak için IoT Edge güvenlik daemonu tarafından kullanıldığından, bunun kendisi bir imzalama sertifikası olmalıdır, yani sertifika imzalama yetenekleri vardır. Cihaza "V3 Basic kısıtlamaları CA:True" uygulamak CA sertifikası gerekli anahtar kullanım özelliklerini otomatik olarak ayarlar.

>[!Tip]
> "Kolaylık komut dosyaları" (sonraki bölüme bakınız) kullanarak bir dev/test senaryosunda IoT Edge'in saydam ağ geçidi olarak kurulumundan geçtiyseniz ve config.yaml'deki ana bilgisayar adı için yaptığınız gibi aygıt CA sertifikasını oluştururken aynı ana bilgisayar adını kullandıysanız, neden çalıştığını merak ediyor olabilirsiniz. Geliştirici deneyimini basitleştirmek amacıyla, kolaylık komut dosyaları, komut dosyasına geçtiğiniz ismin sonunda bir ".ca" ekler. Bu nedenle, örneğin, komut dosyalarınızda hem de config.yaml'da sunucu adınızda "mygateway" kullandıysanız, eski aygıt CA sertifikası için CN olarak kullanılmadan önce mygateway.ca dönüştürülür.

## <a name="devtest-implications"></a>Geliştirme/Test etkileri

Microsoft, geliştirme ve test senaryolarını kolaylaştırmak için, saydam ağ geçidi senaryosunda IoT Edge için uygun üretim dışı sertifikalar oluşturmak için bir dizi [kolaylık komut dosyası](https://github.com/Azure/azure-iot-sdk-c/tree/master/tools/CACertificates) sağlar. Komut dosyalarının nasıl çalıştığına örnekler [için](how-to-create-test-certificates.md)bkz.

>[!Tip]
> IoT Edge üzerinden IoT cihazımız SDK'yı kullanan cihazınızı IoT "yaprak" aygıtlarına ve uygulamalarına bağlamak için, isteğe bağlı GatewayHostName parametresini aygıtın bağlantı dizesinin sonuna eklemeniz gerekir. Kenar Hub Sunucu Sertifikası oluşturulduğunda, config.yaml'den ana bilgisayar adının küçük örnekli bir sürümüne dayanır, bu nedenle, isimlerin eşleşmesi ve TLS sertifika doğrulamasının başarılı olması için küçük harfle GatewayHostName parametresini girmeniz gerekir.

## <a name="example-of-iot-edge-certificate-hierarchy"></a>IoT Edge sertifika hiyerarşisi örneği

Bu sertifika yolunun bir örneğini göstermek için, aşağıdaki ekran görüntüsü saydam ağ geçidi olarak ayarlanmış çalışan bir IoT Edge aygıtından gelir. OpenSSL, IoT Edge hub'ına bağlanmak, doğrulamak ve sertifikaları atmak için kullanılır.

![Her düzeyde sertifika hiyerarşisinin ekran görüntüsü](./media/iot-edge-certs/iotedge-cert-chain.png)

Ekran görüntüsünde temsil edilen sertifika derinliği hiyerarşisini görebilirsiniz:

| Kök CA Sertifikası         | Yalnızca Azure IoT Hub CA Cert Testi                                                                           |
|-----------------------------|-----------------------------------------------------------------------------------------------------------|
| Ara CA Sertifikası | Yalnızca Azure IoT Hub Ara Cert Testi                                                                 |
| Cihaz CA Sertifikası       | iotgateway.ca ("iotgateway" < ağ geçidi ana bilgisayar adı > olarak geçirildi)   |
| İş Yükü CA Sertifikası     | iotedge iş yükü ca                                                                                       |
| IoT Edge Hub Sunucu Sertifikası | iotedgegw.local (config.yaml gelen 'hostname' maçlar)                                            |

## <a name="next-steps"></a>Sonraki adımlar

[Azure IoT Edge modüllerini anlama](iot-edge-modules.md)

[IoT Edge cihazını saydam ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md)
