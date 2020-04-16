---
title: X.509 sertifikalarını kullanarak DPS'li cihazları otomatik olarak sağlama - Azure IoT Edge | Microsoft Dokümanlar
description: Azure IoT Edge için otomatik aygıt sağlamayı Aygıt Sağlama Hizmeti ile test etmek için X.509 sertifikalarını kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: kevindaw
ms.date: 04/09/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: d5e968e578428a16a0005149a409986015a1fc5c
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393747"
---
# <a name="create-and-provision-an-iot-edge-device-using-x509-certificates"></a>X.509 sertifikalarını kullanarak bir IoT Edge aygıtı oluşturma ve sağlama

Azure [IoT Hub Aygıt Sağlama Hizmeti (DPS)](../iot-dps/index.yml)ile X.509 sertifikalarını kullanarak IoT Edge aygıtlarını otomatik olarak sağlayabilirsiniz. Otomatik sağlama işlemine aşina değilseniz, devam etmeden önce [otomatik sağlama kavramlarını](../iot-dps/concepts-auto-provisioning.md) gözden geçirin.

Bu makalede, bir IoT Edge aygıtında X.509 sertifikalarını kullanarak aygıt sağlama hizmeti kaydının nasıl oluşturulabileceğinizi aşağıdaki adımları gösterilmektedir:

* Sertifikalar ve anahtarlar oluşturun.
* Aygıt için tek tek bir kayıt veya bir aygıt kümesi için grup kaydı oluşturun.
* IoT Edge çalışma süresini yükleyin ve aygıtı IoT Hub'a kaydedin.

X.509 sertifikalarını bir attestation mekanizması olarak kullanmak, üretimi ölçeklendirmenin ve cihaz sağlamayı basitleştirmenin mükemmel bir yoludur. Tipik olarak, X.509 sertifikaları bir güven sertifikası zincirinde düzenlenir. Kendi imzalı veya güvenilir bir kök sertifikasıyla başlayarak, zincirdeki her sertifika bir sonraki alt sertifikayı imzalar. Bu desen, kök sertifikadan her ara sertifikaya ve aygıta yüklenen son "yaprak" sertifikasına devredilmiş bir güven zinciri oluşturur.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir IoT Hub'ı.
* IoT Edge aygıtı olması için fiziksel veya sanal bir aygıt.
* [Git'in](https://git-scm.com/download/) en son sürümü yüklendi.
* Azure'daki IoT Hub Aygıt Sağlama Hizmeti'nin bir örneği, IoT hub'ınıza bağlıdır.
  * Aygıt Sağlama Hizmeti örneğiniz yoksa, [IoT Hub DPS'yi ayarla'daki](../iot-dps/quick-setup-auto-provision.md)yönergeleri izleyin.
  * Aygıt Sağlama Hizmeti çalıştırdıktan **sonra, kimlik kapsamının** değerini genel bakış sayfasından kopyalayın. IoT Edge çalışma süresini yapılandırırken bu değeri kullanırsınız.

## <a name="generate-device-identity-certificates"></a>Aygıt kimlik sertifikaları oluşturma

Aygıt kimlik sertifikası, bir güven sertifikası zinciri aracılığıyla üst düzey X.509 sertifika yetkilisi (CA) sertifikasına bağlanan bir yaprak sertifikadır. Aygıt kimlik sertifikası, aygıtın IoT hub'ınızda olmasını istediğiniz aygıt kimliğine ortak ad (CN) ayarlamış olmalıdır.

Aygıt kimlik sertifikaları yalnızca IoT Edge aygıtını sağlama ve aygıtın kimliğini Azure IoT Hub ile doğrulamak için kullanılır. IoT Edge aygıtının doğrulama için modüllere veya yaprak aygıtlara sunduğu CA sertifikalarının aksine sertifikaları imzalamazlar. Daha fazla bilgi için [Azure IoT Edge sertifika kullanım ayrıntısına](iot-edge-certs.md)bakın.

Aygıt kimlik sertifikasını oluşturduktan sonra iki dosyanız olmalıdır: sertifikanın ortak bölümünü içeren bir .cer veya .pem dosyası ve sertifikanın özel anahtarına sahip bir .cer veya .pem dosyası. DPS'de grup kaydı kullanmayı planlıyorsanız, aynı güven sertifikası zincirinde ara veya kök CA sertifikasının ortak bölümüne de ihtiyacınız vardır.

X.509 ile otomatik sağlama ayarlamak için aşağıdaki dosyalara ihtiyacınız var:

* Aygıt kimlik sertifikası ve özel anahtar sertifikası. Tek bir kayıt oluşturursanız, aygıt kimlik sertifikası DPS'ye yüklenir. Özel anahtar IoT Edge çalışma zamanına geçirilir.
* En azından aygıt kimliğine ve içinde ara sertifikalara sahip olması gereken tam zincir sertifikası. Tam zincir sertifikası IoT Edge çalışma süresine geçirilir.
* Sertifika güven zincirinden bir ara veya kök CA sertifikası. Bir grup kaydı oluşturursanız, bu sertifika DPS'ye yüklenir.

### <a name="use-test-certificates"></a>Test sertifikalarını kullanma

Yeni kimlik sertifikaları oluşturmak için kullanılabilir bir sertifika yetkiniz yoksa ve bu senaryoyu denemek istiyorsanız, Azure IoT Edge git deposu test sertifikaları oluşturmak için kullanabileceğiniz komut dosyaları içerir. Bu sertifikalar yalnızca geliştirme testleri için tasarlanmıştır ve üretimde kullanılmamalıdır.

Test sertifikaları oluşturmak [için, IoT Edge aygıt özelliklerini test etmek için demo sertifikaları oluştur'daki](how-to-create-test-certificates.md)adımları izleyin. Sertifika oluşturma komut dosyalarını ayarlamak ve kök CA sertifikası oluşturmak için gerekli iki bölümü tamamlayın. Ardından, aygıt kimlik sertifikası oluşturmak için adımları izleyin. İşi bittiğinde, aşağıdaki sertifika zincirine ve anahtar çiftine sahip olmalısınız:

Linux:

* `<WRKDIR>/certs/iot-edge-device-identity-<name>-full-chain.cert.pem`
* `<WRKDIR>/private/iot-edge-device-identity-<name>.key.pem`

Windows:

* `<WRKDIR>\certs\iot-edge-device-identity-<name>-full-chain.cert.pem`
* `<WRKDIR>\private\iot-edge-device-identity-<name>.key.pem`

Bu sertifikaların her ikisine de IoT Edge aygıtında ihtiyacınız vardır. DPS'de tek tek kayıt kullanacaksanız,.cert.pem dosyasını yüklersiniz. DPS'de grup kaydı kullanacaksanız, yüklemek için aynı güven sertifikası zincirinde bir ara veya kök CA sertifikasına da ihtiyacınız vardır. Demo sertifikaları kullanıyorsanız, grup kaydı `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem` için sertifikayı kullanın.

## <a name="create-a-dps-individual-enrollment"></a>DPS tek tek kayıt oluşturma

Tek bir IoT Edge aygıtı için DPS'de tek tek bir kayıt oluşturmak için oluşturulan sertifikalarınızı ve anahtarlarınızı kullanın. Tek tek kayıtlar, aygıtın kimlik sertifikasının ortak bölümünü alır ve aygıttaki sertifikayla eşleşir.

Birden çok IoT Edge aygıtı sağlamak istiyorsanız, bir sonraki bölümdeki adımları izleyin, [DPS grup kaydı oluşturun.](#create-a-dps-group-enrollment)

DPS'de bir kayıt oluşturduğunuzda, Başlangıç **Aygıtı İkiz Durumu**bildirme fırsatınız vardır. Aygıt ikizinde, etiketleri aygıtları bölge, ortam, konum veya aygıt türü gibi çözümünüzde gereksinim duyduğunuz herhangi bir metrike göre gruplayabilir. Bu etiketler otomatik [dağıtımlar](how-to-deploy-monitor.md)oluşturmak için kullanılır.

Aygıt Sağlama Hizmeti'ndeki kayıtlar hakkında daha fazla bilgi için [aygıt kayıtlarını nasıl yönetebilirsiniz](../iot-dps/how-to-manage-enrollments.md)konusuna bakın.

   > [!TIP]
   > Azure CLI'de bir [kayıt](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/dps/enrollment) veya [kayıt grubu](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/dps/enrollment-group) oluşturabilir ve bir aygıtın veya aygıt grubunun bir IoT Edge aygıtı olduğunu belirtmek için **kenar özellikli** bayrağı kullanabilirsiniz.

1. Azure [portalında,](https://portal.azure.com)IoT Hub Aygıt Sağlama Hizmeti örneğinize gidin.

1. **Ayarlar** **altında, kayıtları yönet'i**seçin.

1. **Tek tek kayıt ekle'yi** seçin ve ardından kaydı yapılandırmak için aşağıdaki adımları tamamlayın:  

   * **Mekanizma**: **X.509'u**seçin.

   * **Birincil Sertifika .pem veya .cer dosyası**: Ortak dosyayı aygıt kimlik sertifikasından yükleyin. Bir test sertifikası oluşturmak için komut dosyalarını kullandıysanız, aşağıdaki dosyayı seçin:

      `<WRKDIR>/certs/iot-edge-device-identity-<name>.cert.pem`

   * **IoT Hub Aygıt Kimliği**: İsterseniz cihazınız için bir kimlik sağlayın. Modül dağıtımı için tek bir aygıtı hedeflemek için aygıt adlarını kullanabilirsiniz. Aygıt kimliği sağlamazsanız, X.509 sertifikasındaki ortak ad (CN) kullanılır.

   * **IoT Edge aygıt**: Kaydın bir IoT Edge aygıtı için olduğunu bildirmek için **True'yu** seçin.

   * **Bu aygıtın atanabileceği IoT hub'larını seçin**: Cihazınızı bağlamak istediğiniz bağlantılı IoT merkezini seçin. Birden çok hub seçebilirsiniz ve aygıt seçili ayırma ilkesine göre bunlardan birine atanır.

   * **İlk Aygıt İkiz Durumu**: İstersenize aygıt ikizine eklenecek etiket değeri ekleyin. Etiketleri, otomatik dağıtım için aygıt gruplarını hedeflemek için kullanabilirsiniz. Örneğin:

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

Bu aygıt için bir kayıt bulunduğuna göre, IoT Edge çalışma süresi yükleme sırasında aygıtı otomatik olarak sağlayabilir. IoT Edge aygıtınızı kurmak için [IoT Edge çalışma zamanı](#install-the-iot-edge-runtime) bölümünü yüklemeye devam edin.

## <a name="create-a-dps-group-enrollment"></a>DPS grup kaydı oluşturma

Birden çok IoT Edge aygıtı için DPS'de grup kaydı oluşturmak için oluşturulan sertifikalarınızı ve anahtarlarınızı kullanın. Grup kayıtları, tek tek aygıt kimlik sertifikaları oluşturmak için kullanılan güven sertifikası zincirinden bir ara veya kök CA sertifikası kullanır.

Bunun yerine tek bir IoT Edge aygıtı sağlamak istiyorsanız, önceki bölümdeki adımları izleyin, [DPS tek tek kayıt oluşturun.](#create-a-dps-individual-enrollment)

DPS'de bir kayıt oluşturduğunuzda, Başlangıç **Aygıtı İkiz Durumu**bildirme fırsatınız vardır. Aygıt ikizinde, etiketleri aygıtları bölge, ortam, konum veya aygıt türü gibi çözümünüzde gereksinim duyduğunuz herhangi bir metrike göre gruplayabilir. Bu etiketler otomatik [dağıtımlar](how-to-deploy-monitor.md)oluşturmak için kullanılır.

### <a name="verify-your-root-certificate"></a>Kök sertifikanızı doğrulama

Bir kayıt grubu oluşturduğunuzda, doğrulanmış bir sertifika kullanma seçeneğiniz olur. Kök sertifikanın mülkiyetine sahip olduğunuzu kanıtlayarak bir sertifikayı DPS ile doğrulayabilirsiniz. Daha fazla bilgi için, [X.509 CA sertifikaları için sahip kanıtı nın nasıl yapılacağını](../iot-dps/how-to-verify-certificates.md)görün.

1. Azure [portalında,](https://portal.azure.com)IoT Hub Aygıt Sağlama Hizmeti örneğinize gidin.

1. Sol menüden **Sertifikalar'ı** seçin.

1. Yeni bir sertifika eklemek için **Ekle'yi** seçin.

1. Sertifikanız için uygun bir ad girin ve Ardından X.509 sertifikanızın ortak kısmını temsil eden .cer veya .pem dosyasına göz atın.

   Demo sertifikalarını kullanıyorsanız, sertifikayı `<wrkdir>/certs/azure-iot-test-only.root.ca.cert.pem` yükleyin.

1. **Kaydet**’i seçin.

1. Sertifikanız artık **Sertifikalar** sayfasında listelenmelidir. Sertifika ayrıntılarını açmak için seçin.

1. **Doğrulama Kodu Oluştur'u** seçin ve ardından oluşturulan kodu kopyalayın.

1. İster kendi CA sertifikanızı getirdiolun, ister demo sertifikalarını kullanıyor olun, sahip olma kanıtını doğrulamak için IoT Edge deposunda sağlanan doğrulama aracını kullanabilirsiniz. Doğrulama aracı, konu adı olarak sağlanan doğrulama kodunu içeren yeni bir sertifikayı imzalamak için CA sertifikanızı kullanır.

   * Windows:

     ```powershell
     New-CACertsVerificationCert "<verification code>"
     ```

   * Linux:

     ```bash
     ./certGen.sh create_verification_certificate <verification code>
     ```

1. Azure portalındaki aynı sertifika ayrıntıları sayfasında, yeni oluşturulan doğrulama sertifikasını yükleyin.

1. **Doğrula** seçeneğini belirleyin.

### <a name="create-enrollment-group"></a>Kayıt grubu oluşturma

Aygıt Sağlama Hizmeti'ndeki kayıtlar hakkında daha fazla bilgi için [aygıt kayıtlarını nasıl yönetebilirsiniz](../iot-dps/how-to-manage-enrollments.md)konusuna bakın.

1. Azure [portalında,](https://portal.azure.com)IoT Hub Aygıt Sağlama Hizmeti örneğinize gidin.

1. **Ayarlar** **altında, kayıtları yönet'i**seçin.

1. **Kayıt grubu ekle'yi** seçin ve ardından kaydı yapılandırmak için aşağıdaki adımları tamamlayın:

   * **Grup adı**: Bu grup kaydı için unutulmaz bir ad sağlayın.

   * **Attestation Türü**: **Sertifika**seçin.

   * **IoT Edge cihazı**: **True'yı**seçin. Grup kaydı için tüm aygıtların IoT Edge aygıtları olması gerekir veya bunların hiçbiri olamaz.

   * **Sertifika Türü**: DPS ile depolanmış doğrulanmış bir CA sertifikanız varsa **CA Sertifikası'nı** veya yalnızca bu kayıt için yeni bir dosya yüklemek istiyorsanız **Ara Sertifika'yı** seçin.

   * **Birincil sertifika**: Son bölümde CA sertifikasını seçtiyseniz, açılan listeden sertifikanızı seçin. Ara sertifika seçtiyseniz, ortak dosyayı aygıt kimlik sertifikaları oluşturmak için kullanılan güven sertifikası zincirindeki ca sertifikasından yükleyin.

   * **Bu aygıtın atanabileceği IoT hub'larını seçin**: Cihazınızı bağlamak istediğiniz bağlantılı IoT merkezini seçin. Birden çok hub seçebilirsiniz ve aygıt seçili ayırma ilkesine göre bunlardan birine atanır.

   * **İlk Aygıt İkiz Durumu**: İstersenize aygıt ikizine eklenecek etiket değeri ekleyin. Etiketleri, otomatik dağıtım için aygıt gruplarını hedeflemek için kullanabilirsiniz. Örneğin:

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

Bu aygıt için bir kayıt bulunduğuna göre, IoT Edge çalışma süresi yükleme sırasında aygıtı otomatik olarak sağlayabilir. IoT Edge cihazınızı kurmak için sonraki bölüme devam edin.

## <a name="install-the-iot-edge-runtime"></a>IoT Edge çalışma süresini yükleme

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Bileşenleri kapsayıcılarda çalışır ve kenarda kod çalıştırabilmeniz için aygıta ek kapsayıcılar dağıtmanızı sağlar.

DPS ile X.509 sağlama yalnızca IoT Edge sürüm 1.0.9 veya daha yeni desteklenir.

Cihazınızı sağlarken aşağıdaki bilgilere ihtiyacınız olacak:

* DPS **Id Kapsam** değeri. Bu değeri Azure portalındaki DPS örneğinizin genel bakış sayfasından alabilirsiniz.
* Aygıttaki aygıt kimlik sertifikası zinciri dosyası.
* Aygıttaki aygıt kimlik anahtarı dosyası.
* İsteğe bağlı bir kayıt kimliği (sağlanmazsa aygıt kimlik sertifikasındaki ortak addan çekilir).

### <a name="linux-device"></a>Linux cihazı

Cihazınızın mimarisine uygun komutları kullanarak Azure IoT Edge çalışma süresini cihazınıza yüklemek için aşağıdaki bağlantıyı kullanın. Güvenlik daemon'una yapılandırış bölümüne girdiğinizde, IoT Edge çalışma süresini manuel değil, X.509 otomatik olarak yapılandırın. Bu makalenin önceki bölümlerini tamamladıktan sonra ihtiyacınız olan tüm bilgi ve sertifika dosyalarına sahip olmanız gerekir.

[Azure IoT Edge çalışma süresini Linux'a yükleme](how-to-install-iot-edge-linux.md)

X.509 sertifikasını ve anahtar bilgilerini config.yaml dosyasına eklediğinizde, yollar dosya URI'leri olarak sağlanmalıdır. Örneğin:

* `file:///<path>/identity_certificate_chain.pem`
* `file:///<path>/identity_key.pem`

X.509 otomatik sağlama yapılandırma dosyasındaki bölüm aşağıdaki gibi görünür:

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

Yer tutucu değerlerini `scope_id` `identity_cert`DPS `identity_pk` örneğinizin kapsam kimliğiyle ve URI'lerle cihazınızdaki sertifika zinciri ve anahtar dosya konumlarına değiştirin. İsterseniz `registration_id` aygıt için a sağlayın veya bu satırı bırakın, aygıtı kimlik sertifikasının CN adı ile kaydettirmek için yorumladı.

Config.yaml dosyasını güncelledikten sonra her zaman güvenlik daemon'una yeniden başlayın.

```bash
sudo systemctl restart iotedge
```

### <a name="windows-device"></a>Windows cihazı

Kimlik sertifikası zincirini ve kimlik anahtarını oluşturduğunuz aygıta IoT Edge çalışma saatini yükleyin. IoT Edge çalışma süresini manuel değil otomatik olarak yapılandıracaksınız.

Kapsayıcıları yönetme ve IoT Edge'i güncelleştirme gibi görevler için ön koşullar ve yönergeler de dahil olmak üzere Windows'a IoT [Edge](how-to-install-iot-edge-windows.md)yükleme hakkında daha ayrıntılı bilgi için bkz.

1. Yönetici modunda bir PowerShell penceresi açın. IoT Edge'i yüklerken AMD64 PowerShell oturumunu kullandığınızdan emin olun, PowerShell'i (x86) değil.

1. **Deploy-IoTEdge** komutu, Windows makinenizin desteklenen bir sürümde olup olmadığını denetler, kapsayıcılar özelliğini açar ve moby çalışma süresini ve IoT Edge çalışma süresini karşıdan yüklez. Komut, Windows kapsayıcılarını kullanmayı varsayılan olarak kullanır.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

1. Bu noktada, IoT Core aygıtları otomatik olarak yeniden başlatılabilir. Diğer Windows 10 veya Windows Server aygıtları yeniden başlatmanızı isteyebilir. Öyleyse, cihazınızı şimdi yeniden başlatın. Cihazınız hazır olduğunda PowerShell'i yeniden yönetici olarak çalıştırın.

1. **Initialize-IoTEdge komutu,** Makinenizdeki IoT Edge çalışma süresini yapılandırır. Komut, `-Dps` bayrağı otomatik sağlama kullanmak için kullanmadığınız sürece el ile sağlama için varsayılandır.

   DPS örneğinizin `{scope_id}`ve `{identity cert chain path}`cihazınızdaki dosya yollarının yer tutucu değerlerini ve `{identity key path}` DPS örneğinizin uygun değerleriyle değiştirin. Kayıt kimliğini belirtmek istiyorsanız, yer `-RegistrationId {registration_id}` tutucuyu uygun şekilde değiştirerek de ekleyin.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps -ScopeId {scope ID} -X509IdentityCertificate {identity cert chain path} -X509IdentityPrivateKey {identity key path}
   ```

   >[!TIP]
   >Config.yaml dosyası, sertifikanızı ve önemli bilgilerinizi dosya URI'leri olarak saklar. Ancak, Initialize-IoTEdge komutu bu biçimlendirme adımını sizin için işler, böylece aygıtınızdaki sertifikaya ve anahtar dosyalarına mutlak yolu sağlayabilirsiniz.

## <a name="verify-successful-installation"></a>Başarılı yüklemeyi doğrulama

Çalışma süresi başarıyla başladıysa, IoT Hub'ınıza gidip IoT Edge modüllerini cihazınıza dağıtmaya başlayabilirsiniz.

Aygıt Sağlama Hizmeti'nde oluşturduğunuz tek tek kaydın kullanıldığını doğrulayabilirsiniz. Azure portalındaki Aygıt Sağlama Hizmeti örneğinize gidin. Oluşturduğunuz tek tek kayıt için kayıt ayrıntılarını açın. Kaydın durumunun **atandığını** ve aygıt kimliğinin listelenmiş olduğuna dikkat edin.

Çalışma zamanının yüklü olduğunu ve başarılı bir şekilde başladığını doğrulamak için cihazınızdaki aşağıdaki komutları kullanın.

### <a name="linux-device"></a>Linux cihazı

IoT Edge hizmetinin durumunu kontrol edin.

```cmd/sh
systemctl status iotedge
```

Servis günlüklerini inceleyin.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Çalışan modülleri listele.

```cmd/sh
iotedge list
```

### <a name="windows-device"></a>Windows cihazı

IoT Edge hizmetinin durumunu kontrol edin.

```powershell
Get-Service iotedge
```

Servis günlüklerini inceleyin.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Çalışan modülleri listele.

```powershell
iotedge list
```

## <a name="next-steps"></a>Sonraki adımlar

Aygıt Sağlama Hizmeti kayıt işlemi, aygıt kimliğini ve aygıt ikiz etiketlerini yeni aygıtı sağlarken aynı anda ayarlamanızı sağlar. Bu değerleri, otomatik aygıt yönetimini kullanarak tek tek aygıtları veya aygıt gruplarını hedeflemek için kullanabilirsiniz. Azure portalını kullanarak veya [Azure CLI'yi kullanarak](how-to-deploy-monitor-cli.md) [IoT Edge modüllerini ölçekte nasıl dağıtıp izleyeceğinizi](how-to-deploy-monitor.md) öğrenin.
