---
title: Azure IoT Central cihaz bağlantısı | Microsoft Docs
description: Bu makalede, Azure IoT Central cihaz bağlantısıyla ilgili temel kavramlar tanıtılmaktadır
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: ddbb1c6fd705e658867c0d594981e87bc8cd6afe
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82930497"
---
# <a name="get-connected-to-azure-iot-central"></a>Azure IoT Central 'e bağlanın

*Bu makale işleçler ve cihaz geliştiricileri için geçerlidir.*

Bu makalede, cihazlarınızı bir Azure IoT Central uygulamasına bağlama seçenekleri açıklanmaktadır.

Genellikle, bağlanabilmek için uygulamanıza bir cihaz kaydetmeniz gerekir. Ancak IoT Central, [cihazların önce kaydolmadan bağlanabildiği](#connect-without-registering-devices)senaryolara destek verebilir.

IoT Central, bağlantı işlemini yönetmek için [Azure IoT Hub cihaz sağlama hizmeti 'ni (DPS)](../../iot-dps/about-iot-dps.md) kullanır. Bir cihaz, uygulamanıza bağlanması gereken bilgileri almak için önce bir DPS uç noktasına bağlanır. Dahili olarak, IoT Central uygulamanız cihaz bağlantısını işlemek için bir IoT Hub 'ı kullanır. DPS kullanılması şunları sunar:

- Cihazları ölçeklendirmeye ekleme ve bağlamayı desteklemek için IoT Central.
- Cihazları IoT Central kullanıcı arabirimi aracılığıyla kaydetmeden cihaz kimlik bilgilerini oluşturup cihazları çevrimdışı olarak yapılandırabilirsiniz.
- IoT Central cihazları kaydetmek için kendi cihaz kimliklerinizi kullanmanız gerekir. Kendi cihaz kimliklerinizin kullanılması, mevcut arka ofis sistemleriyle tümleştirmeyi basitleştirir.
- Cihazları IoT Central bağlamak için tek ve tutarlı bir yol.

Bir cihaz ve uygulamanız arasındaki iletişimin güvenliğini sağlamak için IoT Central hem paylaşılan erişim imzalarını (SAS) hem de X. 509.440 sertifikalarını destekler. X. 509.440 sertifikaları üretim ortamlarında önerilir.

Bu makalede aşağıdaki kullanım durumları açıklanmaktadır:

- [SAS kullanarak tek bir cihazı bağlama](#connect-a-single-device)
- [SAS kullanarak cihazları ölçeklendirmeye bağlama](#connect-devices-at-scale-using-sas)
- [X. 509.440 sertifikaları kullanarak cihazları ölçeklendirmeye bağlama](#connect-devices-using-x509-certificates) -üretim ortamları için önerilen yaklaşım.
- [Cihazları önce kaydetmeden bağlayın](#connect-without-registering-devices)
- [DPS bireysel kayıtları kullanan cihazları bağlama](#individual-enrollment-based-device-connectivity)
- [Cihazı bir cihaz şablonuyla otomatik olarak ilişkilendir](#automatically-associate-with-a-device-template)

## <a name="connect-a-single-device"></a>Tek bir cihazı bağlama

Bu yaklaşım, IoT Central veya test cihazlarıyla denemeler yaparken faydalıdır. Bir cihazı IoT Central uygulamanıza bağlamak için IoT Central uygulamanızdan cihaz bağlantısı SAS anahtarlarını kullanabilirsiniz. Kayıtlı bir cihazın bağlantı bilgilerini _CIHAZ SAS anahtarı_ ' nı kopyalayın:

![Tek bir cihaz için SAS anahtarları](./media/concepts-get-connected/single-device-sas.png)

Daha fazla bilgi edinmek için bkz. [Node. js istemci uygulaması oluşturma ve Azure IoT Central uygulamanızın](./tutorial-connect-device-nodejs.md) öğreticisine bağlama öğreticisine bakın.

## <a name="connect-devices-at-scale-using-sas"></a>SAS kullanarak cihazları ölçeklendirmeye bağlama

Cihazları SAS anahtarları kullanarak ölçeklendirerek IoT Central bağlamak için, cihazları kaydetmeniz ve sonra ayarlamanız gerekir:

### <a name="register-devices-in-bulk"></a>Cihazları toplu olarak kaydetme

IoT Central uygulamanızla çok sayıda cihazı kaydetmek için, [cihaz kimliklerini ve cihaz adlarını içeri aktarmak](howto-manage-devices.md#import-devices)üzere bir CSV dosyası kullanın.

İçeri aktarılan cihazların bağlantı bilgilerini almak için [IoT Central uygulamanızdan BIR CSV dosyası dışarı aktarın](howto-manage-devices.md#export-devices). İçe aktarılmış CSV dosyası, cihaz kimliklerini ve SAS anahtarlarını içerir.

### <a name="set-up-your-devices"></a>Cihazlarınızı ayarlama

Cihazlarınızı IoT Central uygulamanıza bağlamak ve IoT 'e veri göndermek üzere cihaz kodunuzda dışarı aktarma dosyasından bağlantı bilgilerini kullanın. Uygulamanız için DPS **kimlik kapsamı** da gerekir. Bu değeri, **yönetim > cihaz bağlantısı**'nda bulabilirsiniz.

> [!NOTE]
> Cihazları öncelikle IoT Central kaydetmeden nasıl bağlayabileceğinizi öğrenmek için, bkz. [önce cihazları kaydetmeden Bağlan](#connect-without-registering-devices).

## <a name="connect-devices-using-x509-certificates"></a>X. 509.440 sertifikalarını kullanarak cihazları bağlama

Bir üretim ortamında, X. 509.440 sertifikalarının kullanılması, IoT Central için önerilen cihaz kimlik doğrulama mekanizmasıdır. Daha fazla bilgi için bkz. [X. 509.440 CA sertifikalarını kullanarak cihaz kimlik doğrulaması](../../iot-hub/iot-hub-x509ca-overview.md).

Bir cihaza X. 509.952 sertifikası bağlanmadan önce, uygulamanızda bir ara veya kök X. 509.440 sertifikası ekleyin ve doğrulayın. Cihazların kök veya ara sertifikadan oluşturulan yaprak X. 509.440 sertifikalarını kullanması gerekir.

### <a name="add-and-verify-a-root-or-intermediate-certificate"></a>Kök veya ara sertifika ekleme ve doğrulama

**Yönetim > cihaz bağlantısı ' na gidin > birincil sertifikayı yönetin** ve cihaz sertifikalarını oluşturmak Için kullandığınız X. 509.440 kökünü veya ara sertifikasını ekleyin.

![Bağlantı ayarları](media/concepts-get-connected/manage-x509-certificate.png)

Sertifika sahipliğinin doğrulanması, sertifikayı karşıya yükleyen kişinin sertifikanın özel anahtarına sahip olmasını sağlar. Sertifikayı doğrulamak için:

  1. Kod oluşturmak için **doğrulama kodu** ' nun yanındaki düğmeyi seçin.
  1. Önceki adımda oluşturduğunuz doğrulama koduyla bir X. 509.952 doğrulama sertifikası oluşturun. Sertifikayı bir. cer dosyası olarak kaydedin.
  1. İmzalı doğrulama sertifikasını karşıya yükleyin ve **Doğrula**' yı seçin. Doğrulama başarılı olduğunda sertifika **doğrulanmış** olarak işaretlenir.

Bir güvenlik ihlali varsa veya birincil sertifikanız süresi dolacak şekilde ayarlandıysa, kapalı kalma süresini azaltmak için ikincil sertifikayı kullanın. Birincil sertifikayı güncelleştirdiğinizde ikincil sertifikayı kullanarak cihaz sağlamaya devam edebilirsiniz.

### <a name="register-and-connect-devices"></a>Cihazları kaydetme ve bağlama

X. 509.440 sertifikalarını kullanarak cihazları toplu bağlamak için, önce [cihaz kimliklerini ve cihaz adlarını içeri aktarmak](howto-manage-devices.md#import-devices)üzere bir CSV dosyası kullanarak uygulamanıza cihazları kaydedin. Cihaz kimliklerinin hepsi küçük olmalıdır.

Karşıya yüklenen kök veya ara sertifikayı kullanarak cihazlarınız için X. 509.440 yaprak sertifikaları oluşturun. Yaprak sertifikalarındaki `CNAME` değer olarak **cihaz kimliğini** kullanın. Cihaz kodunuz, uygulamanız için **kimlik kapsamı** değeri, **cihaz kimliği**ve karşılık gelen cihaz sertifikası gerektirir.

#### <a name="sample-device-code"></a>Örnek cihaz kodu

[Azure IoT Node. js SDK 'sının](https://github.com/Azure/azure-iot-sdk-node/blob/master/provisioning/device/samples/register_x509.js) aşağıdaki örneği, bir Node. js cihaz istemcisinin bir IoT Central uygulamasına kaydolmak Için bir X. 509.952 yaprak SERTIFIKASı ve DPS nasıl kullandığını gösterir:

:::code language="nodejs" source="~/azure-iot-sdk-node/provisioning/device/samples/register_x509.js":::

Eşdeğer bir C örneği için bkz. [Azure IoT C sağlama aygıtı istemci SDK 'sında](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md) [prov_dev_client_sample. C](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/samples/prov_dev_client_sample/prov_dev_client_sample.c) .

### <a name="for-testing-purposes-only"></a>Yalnızca test amaçlı

Yalnızca test için, kök, ara ve cihaz sertifikaları oluşturmak üzere aşağıdaki yardımcı programları kullanabilirsiniz:

- [Azure IoT cihaz sağlama cihaz SDK 'sı Için Araçlar](https://github.com/Azure/azure-iot-sdk-node/blob/master/provisioning/tools/readme.md): X. 509.440 sertifikalarını ve anahtarlarını oluşturmak ve doğrulamak için kullanabileceğiniz bir Node. js araçları koleksiyonu.
- Bir DevKit cihazı kullanıyorsanız, bu [komut satırı aracı](https://aka.ms/iotcentral-docs-dicetool) , sertifikaları doğrulamak için IoT Central uygulamanıza EKLEYEBILECEĞINIZ bir CA sertifikası oluşturur.
- [Örnek ve öğreticiler için test CA sertifikalarını yönetme](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md): bir PowerShell ve Bash betikleri koleksiyonu:
  - Bir sertifika zinciri oluşturun.
  - IoT Central uygulamanıza yüklemek için sertifikaları. cer dosyası olarak kaydedin.
  - Doğrulama sertifikasını oluşturmak için IoT Central uygulamasındaki doğrulama kodunu kullanın.
  - Cihaz kimliklerinizi araç için bir parametre olarak kullanarak cihazlarınız için yaprak sertifikaları oluşturun.

## <a name="connect-without-registering-devices"></a>Cihazları kaydettirmeden Bağlan

Daha önce açıklanan senaryolar, bağlanmadan önce uygulamanızdaki cihazları kaydetmenizi gerektirir. IoT Central Ayrıca, OEM 'Lerin, önce kaydolmadan bağlanabilecek cihazları toplu olarak üretmesine de olanak sağlar. OEM, uygun cihaz kimlik bilgilerini oluşturur ve fabrikadaki cihazları yapılandırır. Bir müşteri bir cihazı ilk kez açtığında, DPS 'e bağlanır ve ardından cihazı otomatik olarak doğru IoT Central uygulamasına bağlar. Bir IoT Central işleci, uygulamaya veri göndermeye başlamadan önce cihazı onaylamalıdır.

Akış, cihazların SAS belirteçlerini veya X. 509.440 sertifikalarını kullanmasına bağlı olarak biraz farklıdır:

### <a name="connect-devices-that-use-sas-tokens-without-registering"></a>Kayıt olmadan SAS belirteçleri kullanan cihazları bağlama

1. IoT Central uygulamasının grup birincil anahtarını kopyalayın:

    ![Uygulama grubu birincil SAS anahtarı](media/concepts-get-connected/group-sas-keys.png)

1. Cihaz SAS anahtarlarını oluşturmak için [DPS-keygen](https://www.npmjs.com/package/dps-keygen) aracını kullanın. Önceki adımda grup birincil anahtarını kullanın. Cihaz kimlikleri küçük harf olmalıdır:

    ```cmd
    dps-keygen -mk:<group primary key> -di:<device ID>
    ```

1. OEM, cihaz KIMLIĞI, oluşturulan cihaz SAS anahtarı ve uygulama **kimliği kapsam** değeri olan her bir cihazı yanıp sönmez.

1. Bir cihaza geçtiğinizde, ilk olarak IoT Central kayıt bilgilerini almak için DPS 'e bağlanır.

    Cihaz başlangıçta **cihazlar** sayfasında **ilişkilendirilmemiş** bir cihaz durumuna sahiptir ve bir cihaz şablonuna atanmaz. **Cihazlar** sayfasında, cihazı uygun cihaz şablonuna **geçirin** . Cihaz sağlama artık tamamlanmıştır, cihaz durumu artık **sağlanıyor**ve cihaz veri göndermeye başlayabilir.

    **Yönetim > cihaz bağlantısı** sayfasında, **otomatik onaylama** seçeneği, cihazı veri göndermeye başlayabilmesi için el ile onaylamanız gerekip gerekmediğini denetler.

    > [!NOTE]
    > Bir cihazı bir cihaz şablonuyla otomatik olarak ilişkilendirme hakkında bilgi edinmek için bkz. cihazı [otomatik olarak cihaz şablonuyla ilişkilendirme](#automatically-associate-with-a-device-template).

### <a name="connect-devices-that-use-x509-certificates-without-registering"></a>Kayıt olmadan X. 509.440 sertifikalarını kullanan cihazları bağlama

1. IoT Central uygulamanıza [bir kök veya ara X. 509.440 sertifikası ekleyin ve doğrulayın](#connect-devices-using-x509-certificates) .

1. IoT Central uygulamanıza eklediğiniz kök veya ara sertifikayı kullanarak cihazlarınız için yaprak sertifikaları oluşturun. Yaprak sertifikalarda, `CNAME` büyük/küçük harf cihaz kimliklerini kullanın.

1. OEM, cihaz KIMLIĞI, oluşturulan yaprak X. 509.440 sertifikası ve uygulama **kimliği kapsam** değeri olan her bir cihazı yanıp sönmez.

1. Bir cihaza geçtiğinizde, ilk olarak IoT Central kayıt bilgilerini almak için DPS 'e bağlanır.

    Cihaz başlangıçta **cihazlar** sayfasında **ilişkilendirilmemiş** bir cihaz durumuna sahiptir ve bir cihaz şablonuna atanmaz. **Cihazlar** sayfasında, cihazı uygun cihaz şablonuna **geçirin** . Cihaz sağlama artık tamamlanmıştır, cihaz durumu artık **sağlanıyor**ve cihaz veri göndermeye başlayabilir.

    **Yönetim > cihaz bağlantısı** sayfasında, **otomatik onaylama** seçeneği, cihazı veri göndermeye başlayabilmesi için el ile onaylamanız gerekip gerekmediğini denetler.

    > [!NOTE]
    > Bir cihazı bir cihaz şablonuyla otomatik olarak ilişkilendirme hakkında bilgi edinmek için bkz. cihazı [otomatik olarak cihaz şablonuyla ilişkilendirme](#automatically-associate-with-a-device-template).

## <a name="individual-enrollment-based-device-connectivity"></a>Ayrı kayıt tabanlı cihaz bağlantısı

Her biri kendi kimlik doğrulama kimlik bilgilerine sahip olan cihazları bağlayan müşteriler için bireysel kayıtları kullanın. Tek bir kayıt, bağlanmasına izin verilen tek bir cihaz için bir giriştir. Bireysel kayıtlar, bir X. 509.440 yaprak sertifikası veya SAS belirteçleri (fiziksel veya sanal Güvenilir Platform modülünden) kanıtlama mekanizmaları olarak kullanabilir. Tek bir kayıtta bulunan cihaz KIMLIĞI (kayıt KIMLIĞI olarak da bilinir) alfasayısal, küçük harf ve kısa çizgi içerebilir. Daha fazla bilgi için bkz. [DPS bireysel kayıt](https://docs.microsoft.com/azure/iot-dps/concepts-service#individual-enrollment).

> [!NOTE]
> Bir cihaz için tek bir kayıt oluşturduğunuzda, IoT Central uygulamanızdaki varsayılan grup kayıt seçeneklerine göre önceliklidir.

### <a name="create-individual-enrollments"></a>Bireysel kayıtlar oluşturma

IoT Central, bireysel kayıtlar için aşağıdaki kanıtlama mekanizmalarını destekler:

- **Simetrik anahtar kanıtlama:** Simetrik anahtar kanıtlama, bir cihazın DPS örneğine kimlik doğrulaması için basit bir yaklaşımdır. Simetrik anahtarlar kullanan tek bir kayıt oluşturmak için, **cihaz bağlantısı** sayfasını açın, bağlantı yöntemi olarak **bireysel kaydı** ve mekanizma olarak **paylaşılan erişim imzasını (SAS)** seçin. Base64 kodlamalı birincil ve ikincil anahtarlar girin ve değişikliklerinizi kaydedin. Cihazınızı bağlamak için **kimlik kapsamını**, **cihaz kimliğini**ve birincil ya da ikincil anahtarı kullanın.

    > [!TIP]
    > Sınama için, **OpenSSL** kullanarak Base64 kodlamalı anahtarlar oluşturabilirsiniz:`openssl rand -base64 64`

- **X. 509.440 sertifikaları:** X. 509.440 sertifikalarıyla tek bir kayıt oluşturmak için, **cihaz bağlantısı** sayfasını açın, bağlantı yöntemi olarak **bireysel kayıt** ' ı ve **Sertifikalar (X. 509.440)** öğesini seçin. Tek bir kayıt girişiyle kullanılan cihaz sertifikalarının, veren ve Subject CN 'nin cihaz KIMLIĞINE ayarlandığı bir gereksinimi vardır.

    > [!TIP]
    > Sınama için, otomatik olarak imzalanan bir sertifika oluşturmak için [Node. js Için Azure IoT cihaz sağlama cihaz SDK 'sı Için araçları](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/tools) kullanabilirsiniz:`node create_test_cert.js device "mytestdevice"`

- **Güvenilir Platform Modülü (TPM) kanıtlama:** [TPM](https://docs.microsoft.com/azure/iot-dps/concepts-tpm-attestation) , bir tür donanım güvenlik modülüdür. TPM kullanmak, bir cihazı bağlamak için en güvenli yöntemlerle biridir. Bu makalede ayrı, bellenim veya tümleşik TPM kullandığınız varsayılır. Yazılım öykünmesi, prototip oluşturma veya test etme için idealdir, ancak ayrık, bellenim veya tümleşik TPMs ile aynı güvenlik düzeyini sağlamalardır. Üretimde yazılım TPM 'Leri kullanmayın. TPM kullanan tek bir kayıt oluşturmak için, **cihaz bağlantısı** sayfasını açın, bağlantı yöntemi olarak **bireysel kayıt** ' ı ve, mekanizma olarak **TPM 'yi** seçin. TPM onay anahtarını girin ve cihaz bağlantı bilgilerini kaydedin.

## <a name="automatically-associate-with-a-device-template"></a>Otomatik olarak bir cihaz şablonuyla ilişkilendir

IoT Central temel özelliklerinden biri cihaz bağlantısıyla cihaz şablonlarını otomatik olarak ilişkilendirebilme özelliğidir. Cihazlar, cihaz kimlik bilgileri ile birlikte cihaz kayıt çağrısının bir parçası olarak bir **Capabilitymodelıd** gönderebilir. **Capabilitymodelıd** , cihazın uyguladığı yetenek modelini TANıMLAYAN bir urn 'dir. IoT Central uygulaması, kullanılacak cihaz şablonunu belirlemek için **Capabilitymodelıd** kullanabilir ve sonra cihazı cihaz şablonuyla otomatik olarak ilişkilendirir. Bulma işlemi aşağıdaki gibi kullanılabilir:

1. Cihaz şablonu IoT Central uygulamada zaten yayımlanıyorsa cihaz, cihaz şablonuyla ilişkilendirilir.
1. Önceden sertifikalı IoT Tak ve Kullan cihazlarında, cihaz şablonu IoT Central uygulamada zaten yayımlanmamışsa, cihaz şablonu genel depodan alınır.

Aşağıdaki kod parçacıkları, otomatik ilişkilendirmenin çalışması için DPS kayıt çağrısı sırasında cihazın gönderilmesi gereken ek yükün biçimini gösterir.

Bu, IoT Tak ve Kullan desteklemeyen, genel olarak kullanılabilir cihaz SDK 'sını kullanan cihazların biçimidir:

```javascript
    iotcModelId: '< this is the URN for the capability model>';
```

Bu, IoT Tak ve Kullan destekleyen genel önizleme cihazı SDK 'sını kullanan cihazların biçimidir:

```javascript
'__iot:interfaces': {
    CapabilityModelId: <this is the URN for the capability model>
}
```

> [!NOTE]
> Cihazların otomatik olarak bağlanması, cihaz şablonunu bulması ve veri göndermeye başlaması için **yönetim > cihaz bağlantısı** 'ndaki **otomatik onaylama** seçeneği etkinleştirilmelidir.

## <a name="device-status-values"></a>Cihaz durumu değerleri

Gerçek bir cihaz IoT Central uygulamasına bağlanırsa, cihaz durumu aşağıdaki gibi değişir:

1. Cihaz durumu ilk olarak **kaydedilir**. Bu durum, cihazın IoT Central oluşturulduğu ve bir cihaz KIMLIĞINE sahip olduğu anlamına gelir. Bir cihaz şu durumlarda kaydedilir:
    - **Cihazlar** sayfasına yeni bir gerçek cihaz eklenir.
    - **Cihazlar sayfasında** **içeri aktar** kullanılarak bir cihaz kümesi eklenir.

1. Geçerli kimlik bilgileriyle IoT Central uygulamanıza bağlı olan cihaz sağlama adımını tamamladığında cihaz durumu **sağlandı** olarak değişir. Bu adımda cihaz, IoT Central uygulamanız tarafından kullanılan IoT Hub bir bağlantı dizesini otomatik olarak almak için DPS kullanır. Cihaz artık IoT Central bağlanabilir ve veri göndermeye başlayabilir.

1. Bir işleç, bir cihazı engelleyebilir. Bir cihaz engellendiğinde, IoT Central uygulamanıza veri gönderemeyecektir. Engellenen cihazların durumu **engellendi**. Bir operatör, verileri göndermeye başlamadan önce cihazı sıfırlamalıdır. Bir operatör bir cihazı engellerse, durum önceki değerine döner, **kaydedilir** **veya sağlanır**.

1. Cihaz durumu **onay bekliyor**Ise, **otomatik onaylama** seçeneğinin devre dışı olduğu anlamına gelir. Bir işlecin, verileri göndermeye başlamadan önce açıkça bir cihaz onaylaması gerekir. **Cihazlar sayfasında el** ile kayıtlı değil, ancak geçerli kimlik bilgileriyle bağlantılı olarak cihaz durumu **onay bekliyor**olacaktır. İşleçler, **Onayla** düğmesini kullanarak bu cihazları **aygıtlar** sayfasından onaylayabilir.

1. Cihaz durumu **ilişkilendirilmemiş**ise, IoT Central bağlanan cihazın ilişkili bir cihaz şablonu olmadığı anlamına gelir. Bu durum genellikle aşağıdaki senaryolarda meydana gelir:

    - Cihaz şablonunu belirtmeden **cihazlar** sayfasında **içeri aktar** kullanılarak bir cihaz kümesi eklenir.
    - Cihaz şablonunu belirtmeden **cihazlar** sayfasında el ile bir cihaz kaydedildi. Ardından cihaz geçerli kimlik bilgileriyle bağlanır.  

    Işleci, **geçirme** düğmesini kullanarak **cihazları cihazlar** sayfasından bir cihaz şablonuyla ilişkilendirebilir.

## <a name="best-practices"></a>En iyi uygulamalar

Cihazı ilk kez bağladığınızda, DPS tarafından döndürülen cihaz bağlantı dizesini kalıcı veya önbelleğe alma. Bir cihazı yeniden bağlamak için, doğru cihaz bağlantı dizesini almak üzere standart cihaz kayıt akışı ' na gidin. Cihaz bağlantı dizesini önbelleğe alıyorsa, cihaz yazılımı, kullandığı temeldeki Azure IoT Hub 'ını güncelleştirse IoT Central eski bir bağlantı dizesine sahip olma riskiyle çalışır.

## <a name="sdk-support"></a>SDK desteği

Azure cihaz SDK 'Ları, cihaz kodunuzu uygulamanız için en kolay yolu sunar. Aşağıdaki cihaz SDK 'Ları kullanılabilir:

- [C için Azure IoT SDK'sı](https://github.com/azure/azure-iot-sdk-c)
- [Python için Azure IoT SDK'sı](https://github.com/azure/azure-iot-sdk-python)
- [Node.js için Azure IoT SDK'sı](https://github.com/azure/azure-iot-sdk-node)
- [Java için Azure IoT SDK'sı](https://github.com/azure/azure-iot-sdk-java)
- [.NET için Azure IoT SDK'sı](https://github.com/azure/azure-iot-sdk-csharp)

### <a name="sdk-features-and-iot-hub-connectivity"></a>SDK özellikleri ve IoT Hub bağlantısı

IoT Hub ile tüm cihaz iletişimi aşağıdaki IoT Hub bağlantı seçeneklerini kullanır:

- [Cihazdan buluta mesajlaşma](../../iot-hub/iot-hub-devguide-messages-d2c.md)
- [Cihaz ikikesi](../../iot-hub/iot-hub-devguide-device-twins.md)

Aşağıdaki tabloda Azure IoT Central cihaz özelliklerinin IoT Hub özellikleriyle nasıl eşleme yapılacağı özetlenmektedir:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Telemetri | Cihazdan buluta mesajlaşma |
| Özellik | Cihaz ikizi bildirilen özellikler |
| Özellik (yazılabilir) | Cihaz ikizi istenen ve bildirilen özellikler |
| Komut | Doğrudan yöntemler |

Cihaz SDK 'larını kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure IoT Central uygulamanıza bir DevDiv Kit cihazını bağlama](howto-connect-devkit.md) örnek kodu.

### <a name="protocols"></a>Protokoller

Cihaz SDK 'Ları, bir IoT Hub 'ına bağlanmak için aşağıdaki ağ protokollerini destekler:

- MQTT
- AMQP
- HTTPS

Bu fark protokolleri ve bir seçim hakkında rehberlik hakkında daha fazla bilgi için bkz. [iletişim protokolü seçme](../../iot-hub/iot-hub-devguide-protocols.md).

Cihazınız desteklenen protokollerden herhangi birini kullanamıyorum, protokol dönüştürmesi yapmak için Azure IoT Edge kullanabilirsiniz. IoT Edge, Azure IoT Central uygulamasından kenara işlem yükünü devretmek için diğer uç zekası senaryolarını destekler.

## <a name="security"></a>Güvenlik

Cihazlar ve Azure IoT Central arasında değiş tokuş edilen tüm veriler şifrelenir. IoT Hub, cihaza yönelik IoT Hub uç noktalarına bağlanan bir cihazdan her isteğin kimliğini doğrular. Bir cihaz, kimlik bilgilerinin bağlantı üzerinden değiş tokuşu yapmasını önlemek için imzalı belirteçleri kullanır. Daha fazla bilgi için bkz. [IoT Hub erişimi denetleme](../../iot-hub/iot-hub-devguide-security.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir cihaz geliştiricisiyseniz, önerilen bazı sonraki adımlar şunlardır:

- [Azure CLI kullanarak cihaz bağlantısını izlemeyi](./howto-monitor-devices-azure-cli.md) öğrenin
- [Azure IoT Central uygulamanızda yeni bir IoT cihaz türü tanımlama](./howto-set-up-template.md) hakkında bilgi edinin
- [Azure IoT Edge cihazlar ve Azure IoT Central](./concepts-iot-edge.md) hakkında bilgi edinin
