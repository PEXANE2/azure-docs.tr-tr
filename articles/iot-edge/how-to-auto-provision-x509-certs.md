---
title: X. 509.440 sertifikalarını kullanarak DPS cihazları otomatik olarak Sağlama-Azure IoT Edge | Microsoft Docs
description: Cihaz sağlama hizmeti ile Azure IoT Edge için otomatik cihaz sağlamayı test etmek üzere X. 509.440 sertifikalarını kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: kevindaw
ms.date: 04/09/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: c54690645286a4fceb3fd786d85652b1cf77d7aa
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86260029"
---
# <a name="create-and-provision-an-iot-edge-device-using-x509-certificates"></a>X. 509.440 sertifikalarını kullanarak bir IoT Edge cihazı oluşturma ve sağlama

[Azure IoT Hub cihaz sağlama hizmeti (DPS)](../iot-dps/index.yml)Ile, X. 509.440 sertifikalarını kullanarak IoT Edge cihazları otomatik olarak sağlayabilirsiniz. Otomatik sağlama işlemini tanımıyorsanız, devam etmeden önce [Otomatik sağlama kavramlarını](../iot-dps/concepts-auto-provisioning.md) gözden geçirin.

Bu makalede, aşağıdaki adımlarla bir IoT Edge cihazında X. 509.440 sertifikalarını kullanarak bir cihaz sağlama hizmeti kaydı oluşturma işlemi gösterilmektedir:

* Sertifikalar ve anahtarlar oluşturun.
* Bir cihaz için tek bir kayıt veya bir dizi cihaz için Grup kaydı oluşturun.
* IoT Edge çalışma zamanını yükleyip cihazı IoT Hub kaydedin.

Bir kanıtlama mekanizması olarak X. 509.440 sertifikalarını kullanmak, üretimi ölçeklendirmek ve cihaz sağlamayı basitleştirmek için mükemmel bir yoldur. Genellikle, X. 509.440 sertifikaları bir sertifika güven zincirinde düzenlenir. Otomatik olarak imzalanan veya güvenilen bir kök sertifikayla başlayarak, zincirdeki her bir sertifika sonraki alt sertifikayı imzalar. Bu model, bir cihazda yüklü olan son "yaprak" sertifikaya, her ara sertifika aracılığıyla kök sertifikadan bir güven zinciri oluşturur.

## <a name="prerequisites"></a>Önkoşullar

* Etkin bir IoT Hub.
* IoT Edge cihaz olması için fiziksel veya sanal bir cihaz.
* [Git](https://git-scm.com/download/) 'in en son sürümü yüklendi.
* Azure 'da IoT Hub 'ınıza bağlı IoT Hub cihaz sağlama hizmeti örneği.
  * Cihaz sağlama hizmeti örneğiniz yoksa [IoT Hub DPS 'Yi ayarlama](../iot-dps/quick-setup-auto-provision.md)bölümündeki yönergeleri izleyin.
  * Cihaz sağlama hizmetini çalıştırdıktan sonra, genel bakış sayfasından **kimlik kapsamının** değerini kopyalayın. IoT Edge çalışma zamanını yapılandırırken bu değeri kullanırsınız.

## <a name="generate-device-identity-certificates"></a>Cihaz kimlik sertifikaları oluşturma

Cihaz kimlik sertifikası, en üstteki X. 509.440 sertifika yetkilisi (CA) sertifikasına bir sertifika güven zinciri üzerinden bağlanan bir yaprak sertifikadır. Cihaz kimliği sertifikası, cihazın IoT Hub 'ınızda olmasını istediğiniz cihaz KIMLIĞINE ayarlanmış olan ortak adı (CN) olmalıdır.

Cihaz kimlik sertifikaları yalnızca IoT Edge cihazı sağlamak ve cihazın Azure IoT Hub kimlik doğrulamasını yapmak için kullanılır. IoT Edge cihazın, doğrulama için modüllerde veya yaprak cihazlarda sundukları CA sertifikalarının aksine, sertifikaları imzalarlar. Daha fazla bilgi için bkz. [Azure IoT Edge sertifika kullanımı ayrıntısı](iot-edge-certs.md).

Cihaz kimliği sertifikasını oluşturduktan sonra iki dosyanız olmalıdır: sertifikanın ortak bölümünü içeren bir. cer veya. ped dosyası ve sertifikanın özel anahtarıyla bir. cer veya. PEA dosyası. DPS 'de grup kaydı kullanmayı planlıyorsanız, aynı sertifika güven zincirindeki bir ara veya kök CA sertifikasının genel bölümüne de ihtiyacınız vardır.

X. 509.952 ile otomatik sağlamayı ayarlamak için aşağıdaki dosyalara ihtiyacınız vardır:

* Cihaz kimliği sertifikası ve onun özel anahtar sertifikası. Tek bir kayıt oluşturursanız cihaz kimliği sertifikası, DPS 'e yüklenir. Özel anahtar IoT Edge çalışma zamanına geçirilir.
* En azından cihaz kimliği ve ara sertifikaları olması gereken tam bir zincir sertifikası. Tam zincir sertifikası IoT Edge çalışma zamanına geçirilir.
* Sertifika güven zincirindeki bir ara veya kök CA sertifikası. Bu sertifika, bir grup kaydı oluşturursanız, DPS 'e yüklenir.

> [!NOTE]
> Şu anda libiothsm içindeki bir sınırlama 1 Ocak 2050 tarihinde veya sonrasında sona ermekte olan sertifikaların kullanılmasını engelliyor.

### <a name="use-test-certificates"></a>Test sertifikalarını kullanma

Yeni kimlik sertifikaları oluşturmak için kullanabileceğiniz bir sertifika yetkiliniz yoksa ve bu senaryoyu denemek istiyorsanız, Azure IoT Edge git deposu, test sertifikaları oluşturmak için kullanabileceğiniz betikleri içerir. Bu sertifikalar yalnızca geliştirme testi için tasarlanmıştır ve üretimde kullanılmamalıdır.

Test sertifikaları oluşturmak için, [IoT Edge cihaz özelliklerini test etmek için tanıtım sertifikaları oluşturma](how-to-create-test-certificates.md)bölümündeki adımları izleyin. Sertifika oluşturma betikleri ayarlamak ve bir kök CA sertifikası oluşturmak için gereken iki bölümü doldurun. Ardından, bir cihaz kimlik sertifikası oluşturma adımlarını izleyin. İşiniz bittiğinde, aşağıdaki sertifika zinciri ve anahtar çiftine sahip olmanız gerekir:

Linux:

* `<WRKDIR>/certs/iot-edge-device-identity-<name>-full-chain.cert.pem`
* `<WRKDIR>/private/iot-edge-device-identity-<name>.key.pem`

Windows:

* `<WRKDIR>\certs\iot-edge-device-identity-<name>-full-chain.cert.pem`
* `<WRKDIR>\private\iot-edge-device-identity-<name>.key.pem`

IoT Edge cihazında bu sertifikaların her ikisi de gereklidir. DPS 'de tek kayıt kullanacaksanız,. cert. ped dosyasını karşıya yüklersiniz. DPS 'de grup kaydı 'nı kullanacaksanız, karşıya yüklemek için aynı sertifika güven zincirindeki bir ara veya kök CA sertifikasına de ihtiyacınız vardır. Tanıtım sertifikaları kullanıyorsanız, `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem` Grup kaydı için sertifikayı kullanın.

## <a name="create-a-dps-individual-enrollment"></a>DPS bireysel kaydı oluşturma

Tek bir IoT Edge cihaz için DPS 'de tek bir kayıt oluşturmak üzere oluşturulan sertifikalarınızı ve anahtarlarınızı kullanın. Bireysel kayıtlar bir cihazın kimlik sertifikasının genel bölümünü alır ve cihazdaki sertifikayla eşleşir.

Birden çok IoT Edge cihaz sağlamak istiyorsanız, bir sonraki bölümdeki adımları izleyin, [BIR DPS grup kaydı oluşturun](#create-a-dps-group-enrollment).

DPS 'de bir kayıt oluşturduğunuzda, bir **Ilk cihaz Ikizi durumu**bildirme fırsatına sahip olursunuz. Device ikizi 'da, bir cihaz için gereken bölge, ortam, konum veya cihaz türü gibi herhangi bir ölçüme cihazları gruplamak için Etiketler ayarlayabilirsiniz. Bu Etiketler [otomatik dağıtımlar](how-to-deploy-at-scale.md)oluşturmak için kullanılır.

Cihaz sağlama hizmetindeki kayıtlar hakkında daha fazla bilgi için bkz. cihaz kayıtlarını [yönetme](../iot-dps/how-to-manage-enrollments.md).

   > [!TIP]
   > Azure CLı 'da bir [kayıt](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/dps/enrollment) veya [kayıt grubu](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/dps/enrollment-group) oluşturabilir ve **kenar özellikli** bayrağını kullanarak bir cihazın veya cihaz grubunun IoT Edge bir cihaz olduğunu belirtebilirsiniz.

1. [Azure Portal](https://portal.azure.com), IoT Hub cihaz sağlama hizmeti örneğinize gidin.

1. **Ayarlar**altında kayıtları **Yönet**' i seçin.

1. **Bireysel kayıt Ekle** ' yi seçin, ardından kaydı yapılandırmak için aşağıdaki adımları izleyin:  

   * **Mekanizma**: **X. 509.440**öğesini seçin.

   * **Birincil sertifika. pek veya. cer dosyası**: ortak dosyayı cihaz kimliği sertifikasından karşıya yükleyin. Bir test sertifikası oluşturmak için betikleri kullandıysanız aşağıdaki dosyayı seçin:

      `<WRKDIR>/certs/iot-edge-device-identity-<name>.cert.pem`

   * **IoT Hub CIHAZ kimliği**: isterseniz cihazınız IÇIN bir kimlik sağlayın. Modül dağıtımı için tek bir cihazı hedeflemek üzere cihaz kimliklerini kullanabilirsiniz. Bir cihaz KIMLIĞI sağlamazsanız, X. 509.440 sertifikası içindeki ortak ad (CN) kullanılır.

   * **IoT Edge cihaz**: kaydın IoT Edge bir cihaz için olduğunu bildirmek için **true** seçeneğini belirleyin.

   * **Bu cihazın atanabileceği IoT Hub 'Larını seçin**: Cihazınızı bağlamak Istediğiniz bağlantılı IoT Hub 'ını seçin. Birden çok hub seçebilirsiniz ve bu cihaz, seçilen ayırma ilkesine göre bu cihazdan birine atanır.

   * **Ilk cihaz Ikizi durumu**: isterseniz, cihaza ikizi eklenecek bir etiket değeri ekleyin. Otomatik dağıtım için cihaz gruplarını hedeflemek üzere etiketleri kullanabilirsiniz. Örnek:

      ```json
      {
         "tags": {
            "environment": "test"
         },
         "properties": {
            "desired": {}
         }
      }
      ```

1. **Kaydet**’i seçin.

Bu cihaz için bir kayıt mevcut olduğuna göre, IoT Edge çalışma zamanı cihazı yükleme sırasında otomatik olarak sağlayabilir. IoT Edge cihazınızı ayarlamak için [IoT Edge çalışma zamanı 'Nı yüklemeye](#install-the-iot-edge-runtime) devam edin.

## <a name="create-a-dps-group-enrollment"></a>Bir DPS grup kaydı oluşturma

Birden çok IoT Edge cihaz için DPS 'de bir grup kaydı oluşturmak için oluşturulan sertifikalarınızı ve anahtarlarınızı kullanın. Grup kayıtları, tek tek cihaz kimlik sertifikalarını oluşturmak için kullanılan sertifika güven zincirindeki bir ara veya kök CA sertifikası kullanır.

Bunun yerine tek bir IoT Edge cihaz sağlamak istiyorsanız, önceki bölümdeki adımları izleyin, [BIR DPS bireysel kaydı oluşturun](#create-a-dps-individual-enrollment).

DPS 'de bir kayıt oluşturduğunuzda, bir **Ilk cihaz Ikizi durumu**bildirme fırsatına sahip olursunuz. Device ikizi 'da, bir cihaz için gereken bölge, ortam, konum veya cihaz türü gibi herhangi bir ölçüme cihazları gruplamak için Etiketler ayarlayabilirsiniz. Bu Etiketler [otomatik dağıtımlar](how-to-deploy-at-scale.md)oluşturmak için kullanılır.

### <a name="verify-your-root-certificate"></a>Kök sertifikanızı doğrulama

Bir kayıt grubu oluşturduğunuzda, doğrulanmış bir sertifika kullanma seçeneğiniz vardır. Kök sertifikaya sahip olduğunuzu belirterek, bir sertifikayı DPS ile doğrulayabilirsiniz. Daha fazla bilgi için bkz. [X. 509.440 CA sertifikaları Için nasıl sahip olma](../iot-dps/how-to-verify-certificates.md).

1. [Azure Portal](https://portal.azure.com), IoT Hub cihaz sağlama hizmeti örneğinize gidin.

1. Sol taraftaki menüden **Sertifikalar** ' ı seçin.

1. Yeni bir sertifika eklemek için **Ekle** ' yi seçin.

1. Sertifikanız için bir kolay ad girin, sonra X. 509.440 sertifikanızın ortak bölümünü temsil eden. cer veya. ped dosyasına gidin.

   Tanıtım sertifikalarını kullanıyorsanız, sertifikayı karşıya yükleyin `<wrkdir>/certs/azure-iot-test-only.root.ca.cert.pem` .

1. **Kaydet**’i seçin.

1. Sertifikanızın artık **Sertifikalar** sayfasında listelenmesi gerekir. Sertifika ayrıntılarını açmak için seçin.

1. **Doğrulama kodu oluştur** ' u seçin, sonra oluşturulan kodu kopyalayın.

1. Kendi CA sertifikanızı mi getirdiğinizi, ister tanıtım sertifikalarını mı kullandığınızı bağımsız olarak, IoT Edge deposunda sunulan doğrulama aracını kullanarak sahip olma kanıtını doğrulayabilirsiniz. Doğrulama Aracı, konu adı olarak belirtilen doğrulama koduna sahip yeni bir sertifikayı imzalamak için CA sertifikanızı kullanır.

   * Windows:

     ```powershell
     New-CACertsVerificationCert "<verification code>"
     ```

   * Linux:

     ```bash
     ./certGen.sh create_verification_certificate <verification code>
     ```

1. Azure portal sertifika ayrıntıları sayfasında, yeni oluşturulan doğrulama sertifikasını karşıya yükleyin.

1. **Doğrula** seçeneğini belirleyin.

### <a name="create-enrollment-group"></a>Kayıt grubu oluştur

Cihaz sağlama hizmetindeki kayıtlar hakkında daha fazla bilgi için bkz. cihaz kayıtlarını [yönetme](../iot-dps/how-to-manage-enrollments.md).

1. [Azure Portal](https://portal.azure.com), IoT Hub cihaz sağlama hizmeti örneğinize gidin.

1. **Ayarlar**altında kayıtları **Yönet**' i seçin.

1. Kayıt **grubu Ekle** ' yi seçin, ardından kaydı yapılandırmak için aşağıdaki adımları izleyin:

   * **Grup adı**: Bu grup kaydı için hatırlayabileceğiniz bir ad sağlayın.

   * **Kanıtlama türü**: **sertifika**seçin.

   * **IoT Edge cihaz**: **doğru**öğesini seçin. Bir grup kaydı için tüm cihazların IoT Edge cihaz olması veya hiçbirinin olmaması gerekir.

   * **Sertifika türü**: yalnızca bu kayıt için yeni bir dosya yüklemek ISTIYORSANıZ, DPS ile depolanan DOĞRULANMıŞ bir CA sertifikanız varsa **CA Sertifikası** ' nı veya **Ara Sertifika** ' yı seçin.

   * **Birincil sertifika**: son bölümde CA sertifikası ' nı seçerseniz, açılan listeden sertifikanızı seçin. Ara sertifika ' yı seçerseniz, ortak dosyayı cihaz kimlik sertifikalarını oluşturmak için kullanılan sertifika güven zincirindeki bir CA sertifikasından karşıya yükleyin.

   * **Bu cihazın atanabileceği IoT Hub 'Larını seçin**: Cihazınızı bağlamak Istediğiniz bağlantılı IoT Hub 'ını seçin. Birden çok hub seçebilirsiniz ve bu cihaz, seçilen ayırma ilkesine göre bu cihazdan birine atanır.

   * **Ilk cihaz Ikizi durumu**: isterseniz, cihaza ikizi eklenecek bir etiket değeri ekleyin. Otomatik dağıtım için cihaz gruplarını hedeflemek üzere etiketleri kullanabilirsiniz. Örnek:

      ```json
      {
         "tags": {
            "environment": "test"
         },
         "properties": {
            "desired": {}
         }
      }
      ```

1. **Kaydet**’i seçin.

Bu cihaz için bir kayıt mevcut olduğuna göre, IoT Edge çalışma zamanı cihazı yükleme sırasında otomatik olarak sağlayabilir. IoT Edge cihazınızı ayarlamak için sonraki bölüme geçin.

## <a name="install-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yükler

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Bileşenleri kapsayıcılarda çalıştırılır ve kenarda kod çalıştırabilmeniz için cihaza ek kapsayıcılar dağıtmanıza izin verir.

DPS ile X. 509.440 sağlama yalnızca IoT Edge Version 1.0.9 veya daha yeni sürümlerde desteklenir.

Cihazınızı sağlarken aşağıdaki bilgilere sahip olmanız gerekir:

* DPS **kimlik kapsamı** değeri. Bu değeri, Azure portal DPS örneğinizin genel bakış sayfasından elde edebilirsiniz.
* Cihazdaki cihaz kimliği sertifika zinciri dosyası.
* Cihazdaki cihaz kimliği anahtar dosyası.
* İsteğe bağlı bir kayıt KIMLIĞI (sağlanmazsa, cihaz kimlik sertifikasındaki ortak ad üzerinden çekilir).

### <a name="linux-device"></a>Linux cihazı

Cihazınızın mimarisine uygun komutları kullanarak Azure IoT Edge çalışma zamanını cihazınıza yüklemek için aşağıdaki bağlantıyı kullanın. Güvenlik cini yapılandırma bölümüne geldiğinizde, X. 509.952 otomatik için IoT Edge çalışma zamanını, el ile değil, sağlama için yapılandırın. Bu makalenin önceki bölümlerini tamamladıktan sonra ihtiyacınız olan tüm bilgi ve sertifika dosyalarına sahip olmanız gerekir.

[Linux üzerinde Azure IoT Edge çalışma zamanını yükler](how-to-install-iot-edge-linux.md)

X. 509.952 sertifikasını ve anahtar bilgilerini config. YAML dosyasına eklediğinizde, yolların dosya URI 'Leri olarak sağlanması gerekir. Örnek:

* `file:///<path>/identity_certificate_chain.pem`
* `file:///<path>/identity_key.pem`

X. 509.440 otomatik sağlama yapılandırma dosyasındaki bölümü şuna benzer:

```yaml
# DPS X.509 provisioning configuration
provisioning:
  source: "dps"
  global_endpoint: "https://global.azure-devices-provisioning.net"
  scope_id: "<SCOPE_ID>"
  attestation:
    method: "x509"
#   registration_id: "<OPTIONAL REGISTRATION ID. LEAVE COMMENTED OUT TO REGISTER WITH CN OF identity_cert>"
    identity_cert: "<REQUIRED URI TO DEVICE IDENTITY CERTIFICATE>"
    identity_pk: "<REQUIRED URI TO DEVICE IDENTITY PRIVATE KEY>"
```

, İçin yer tutucu değerlerini `scope_id` , `identity_cert` `identity_pk` DPS örneğinizin kapsam kimliğiyle, ve URI 'leri ise sertifika zinciri ve anahtar dosya konumlarına göre değiştirin. `registration_id`İsterseniz cihaz için bir belirtin veya bu satırı, kimlik SERTIFIKASıNıN CN adı ile kaydetmek için bu satırı açıklama olarak bırakın.

Config. YAML dosyasını güncelleştirdikten sonra her zaman güvenlik arka plan programını yeniden başlatın.

```bash
sudo systemctl restart iotedge
```

### <a name="windows-device"></a>Windows cihazı

Kimlik sertifika zincirini ve kimlik anahtarını oluşturduğunuz cihaza IoT Edge çalışma zamanını yükleyebilirsiniz. IoT Edge çalışma zamanını otomatik, el ile değil, sağlama için yapılandıracaksınız.

Windows üzerinde IoT Edge yükleme hakkında daha ayrıntılı bilgi için IoT Edge, bkz. [Azure IoT Edge çalışma zamanını Windows 'A yükleme](how-to-install-iot-edge-windows.md).

1. Yönetici modunda bir PowerShell penceresi açın. PowerShell (x86) değil IoT Edge yüklerken PowerShell 'in AMD64 oturumunu kullandığınızdan emin olun.

1. **Deploy-ıotedge** komutu, Windows makinenizin desteklenen bir sürümde olup olmadığını denetler, kapsayıcılar özelliğini açar ve ardından Moby çalışma zamanını ve IoT Edge çalışma zamanını indirir. Komut varsayılan olarak Windows kapsayıcıları ' nı kullanmaktır.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

1. Bu noktada, IoT çekirdek cihazları otomatik olarak yeniden başlatılabilir. Diğer Windows 10 veya Windows Server cihazları yeniden başlatmanızı isteyebilir. Bu durumda cihazınızı şimdi yeniden başlatın. Cihazınız çalışmaya başladıktan sonra PowerShell 'i yönetici olarak yeniden çalıştırın.

1. **Initialize-ıotedge** komutu, makinenizde IoT Edge çalışma zamanını yapılandırır. Otomatik sağlamayı kullanmak için bayrağını kullanmadığınız takdirde, komut el ile sağlamayı varsayılan olarak sağlar `-Dps` .

   ,, Ve için yer tutucu değerlerini, `{scope_id}` `{identity cert chain path}` `{identity key path}` DPS örneğinizin ve cihazınızdaki dosya yollarından uygun değerlerle değiştirin. Kayıt KIMLIĞINI belirtmek istiyorsanız, `-RegistrationId {registration_id}` yer tutucuyu uygun şekilde değiştirin.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps -ScopeId {scope ID} -X509IdentityCertificate {identity cert chain path} -X509IdentityPrivateKey {identity key path}
   ```

   >[!TIP]
   >Config. YAML dosyası, sertifikanızı ve anahtar bilgilerinizi dosya URI 'Leri olarak depolar. Ancak, Initialize-ıotedge komutu sizin için bu biçimlendirme adımını işler, böylece cihazınızdaki sertifikaya ve anahtar dosyalarına mutlak yol sağlayabilirsiniz.

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulama

Çalışma zamanı başarıyla başlatıldıysa, IoT Hub ve cihazınıza IoT Edge modülleri dağıtmaya başlayabilirsiniz.

Cihaz sağlama hizmeti 'nde oluşturduğunuz bireysel kaydın kullanıldığını doğrulayabilirsiniz. Azure portal cihaz sağlama hizmeti örneğinize gidin. Oluşturduğunuz bireysel kayıt için kayıt ayrıntılarını açın. Kayıt durumunun **atandığını** ve cihaz kimliğinin listelendiğini unutmayın.

Çalışma zamanının başarıyla yüklendiğini ve başlatıldığını doğrulamak için cihazınızda aşağıdaki komutları kullanın.

### <a name="linux-device"></a>Linux cihazı

IoT Edge hizmetinin durumunu kontrol edin.

```cmd/sh
systemctl status iotedge
```

Hizmet günlüklerini inceleyin.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Çalışan modülleri listeleyin.

```cmd/sh
iotedge list
```

### <a name="windows-device"></a>Windows cihazı

IoT Edge hizmetinin durumunu kontrol edin.

```powershell
Get-Service iotedge
```

Hizmet günlüklerini inceleyin.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Çalışan modülleri listeleyin.

```powershell
iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Cihaz sağlama hizmeti kayıt işlemi, yeni cihazı sağladığınız anda cihaz KIMLIĞI ve cihaz ikizi etiketlerini ayarlamanıza olanak sağlar. Bu değerleri, otomatik cihaz yönetimi kullanarak ayrı cihazları veya cihaz gruplarını hedeflemek için kullanabilirsiniz. Azure portal veya [Azure CLI kullanarak](how-to-deploy-cli-at-scale.md) [IoT Edge modüllerini ölçekte nasıl dağıtacağınızı ve izleyeceğinizi](how-to-deploy-at-scale.md) öğrenin.
