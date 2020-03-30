---
title: Azure IoT Hub Cihaz Sağlama Hizmeti SDK araçlarını kullanma
description: Bu belge, geliştirme için Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) SDK'larında sağlanan araçları inceler
author: robinsh
ms.author: robinsh
ms.date: 04/09/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: b817b3cfe47ed08cae9e7e0b1c2c24363f2ccfed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271531"
---
# <a name="how-to-use-tools-provided-in-the-sdks-to-simplify-development-for-provisioning"></a>Sağlama için geliştirmeyi kolaylaştırmak için SDK'larda sağlanan araçların nasıl kullanılacağı
IoT Hub Aygıt Sağlama Hizmeti, güvenli ve ölçeklenebilir bir şekilde sıfır dokunuşla, tam zamanında [otomatik sağlama](concepts-auto-provisioning.md) yla sağlama işlemini basitleştirir.  X.509 sertifikası veya Güvenilir Platform Modülü (TPM) şeklinde güvenlik attestation gereklidir.  Microsoft, IoT dağıtımının güvenliğini sağlama güvenini artırmak için [diğer güvenlik donanım ortaklarıyla](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) da ortaklık yapmaktadır. Donanım güvenliği gereksinimini anlamak geliştiriciler için oldukça zor olabilir. Geliştiricilerin sağlama hizmetiyle konuşan istemcileri yazmak için bir kolaylık katmanı kullanabilmeleri için bir dizi Azure IoT Sağlama Hizmeti SDK'sı sağlanır. SDK'lar ayrıca, geliştirmedeki güvenlik attestation'ını basitleştirmek için yaygın senaryoların yanı sıra bir dizi araç için örnekler de sağlar.

## <a name="trusted-platform-module-tpm-simulator"></a>Güvenilir Platform Modülü (TPM) simülatörü
[TPM,](https://docs.microsoft.com/azure/iot-dps/concepts-security) platformun kimliğini doğrulamak için anahtarları güvenli bir şekilde depolamak için bir standarda veya standardı uygulayan modüllerle etkileşimde bulunmak için kullanılan G/Ç arabirimine başvurabilir. TPM'ler ayrı donanım, tümleşik donanım, yazılım tabanlı veya yazılım tabanlı olarak bulunabilir.  Üretimde, TPM aygıtta ayrı donanım, tümleşik donanım veya firmware tabanlı olarak bulunur. Sınama aşamasında, geliştiricilere yazılım tabanlı bir TPM simülatörü sağlanır.  Bu simülatör şimdilik yalnızca Windows platformunda geliştirilebilir.

TPM simülatörü kullanma adımları şunlardır:
1. [Geliştirme ortamını hazırlayın](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java) ve GitHub deposunu klonlayın:
   ```
   git clone https://github.com/Azure/azure-iot-sdk-java.git
   ```
2. Altında ```azure-iot-sdk-java/provisioning/provisioning-tool/tpm-simulator/```TPM simülatörü klasörüne gidin.
3. Sağlama aygıtı için herhangi bir istemci uygulamasını çalıştırmadan önce Simulator.exe'yi çalıştırın.
4. Kayıt kimliği ve Onay Anahtarı elde etmek için simülatörün sağlama işlemi boyunca arka planda çalışmasına izin verin.  Her iki değer de çalıştırmanın yalnızca bir örneği için geçerlidir.

## <a name="x509-certificate-generator"></a>X.509 sertifika jeneratörü
[X.509 sertifikaları,](https://docs.microsoft.com/azure/iot-dps/concepts-security#x509-certificates) üretimi ölçeklendirmek ve cihaz sağlamayı basitleştirmek için bir attestation mekanizması olarak kullanılabilir.  X.509 sertifikası almanın [birkaç yolu](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#how-to-get-an-x509-ca-certificate) vardır:
* Üretim ortamı için, bir ortak kök sertifika yetkilisinden X.509 CA sertifikası satın almanızı öneririz.
* Test ortamı için, bir X.509 kök sertifikası veya X.509 sertifika zinciri kullanarak oluşturabilirsiniz:
    * OpenSSL: Sertifika oluşturma için komut dosyalarını kullanabilirsiniz:
        * [Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/tools)
        * [PowerShell veya Bash](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md)
        
    * Cihaz Kimlik Bileşim Motoru (DICE) Emülatörü: DICE, TLS protokolü ve X.509 istemci sertifikalarına dayalı olarak kriptografik cihaz kimliği ve attestation için kullanılabilir.  DICE ile cihaz kimliği hakkında daha fazla [bilgi edinin.](https://www.microsoft.com/research/publication/device-identity-dice-riot-keys-certificates/)

### <a name="using-x509-certificate-generator-with-dice-emulator"></a>DICE emülatörü ile X.509 sertifika jeneratörü kullanma
SDK'lar [Java SDK'da](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-tools/provisioning-x509-cert-generator)bulunan DICE emülatörüne sahip x.509 sertifika jeneratörü sağlar.  Bu jeneratör çapraz platformda çalışıyor.  Oluşturulan sertifika diğer dillerde geliştirme için kullanılabilir.

Şu anda, DICE Emülatörü bir kök sertifika, bir ara sertifika, bir yaprak sertifikası ve ilişkili özel anahtar çıktırırken.  Ancak, kök sertifika veya ara sertifika ayrı bir yaprak sertifika imzalamak için kullanılamaz.  Birden çok aygıtın yaprak sertifikalarını imzalamak için bir imzalama sertifikasının kullanıldığı grup kayıt senaryosunu test etmek istiyorsanız, sertifikalar zinciri oluşturmak için OpenSSL'yi kullanabilirsiniz.

Bu jeneratör kullanarak X.509 sertifikası oluşturmak için:
1. [Geliştirme ortamını hazırlayın](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java) ve GitHub deposunu klonlayın:
   ```
   git clone https://github.com/Azure/azure-iot-sdk-java.git
   ```
2. Kökü azure-iot-sdk-java olarak değiştirin.
3. Gerekli ```mvn install -DskipTests=true``` tüm paketleri indirmek ve SDK derlemek için çalıştırın
4. X.509 Sertifika Jeneratörü için ```azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator```kök gidin.
5. Ile oluştur```mvn clean install```
6. Aşağıdaki komutları kullanarak aracı çalıştırın:
   ```
   cd target
   java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
   ```
7. İstendiğinde sertifikalarınız için _Common Name_ (Ortak Ad) girebilirsiniz.
8. Araç yerel bir **İstemci Cert,** **İstemci Cert Private Key**, **Ara Cert**ve Kök **Cert**oluşturur.

**Client Cert,** aygıttaki yaprak sertifikadır.  Aygıt istemcisinde **Istemci Cert** ve ilişkili **Client Cert Private Key** gereklidir. Seçtiğiniz dile bağlı olarak, istemci uygulamasına bunu koyacak mekanizma farklı olabilir.  Daha fazla bilgi için, daha fazla bilgi için X.509'u kullanarak simüle edilmiş aygıt oluşturmada [Hızlı Başlangıçlar'a](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device-x509) bakın.

Kök sertifika veya ara bir kayıt grubu veya bireysel kayıt [programlı](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments-sdks) veya [portal](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments)kullanarak oluşturmak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure IoT Hub ve Azure IoT Hub Aygıt Sağlama Hizmeti için [Azure IoT SDK'yı]( https://github.com/Azure/azure-iot-sdks) kullanarak geliştirme
