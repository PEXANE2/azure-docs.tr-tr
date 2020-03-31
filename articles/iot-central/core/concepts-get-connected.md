---
title: Azure IoT Central'da aygıt bağlantısı | Microsoft Dokümanlar
description: Bu makalede, Azure IoT Central'da aygıt bağlantısıyla ilgili temel kavramlar
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 8178e585ecb7b1cdfd5e530f3d3406b7397f0968
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476058"
---
# <a name="get-connected-to-azure-iot-central"></a>Azure IoT Central'a bağlanın

Bu makalede, aygıtlarınızı bir Azure IoT Merkezi uygulamasına bağlama seçenekleri açıklanmaktadır.

Genellikle, bağlanabilmesi için bir aygıtı uygulamanızda kaydetmeniz gerekir. Ancak, IoT [Central, aygıtların ilk kaydedilmeden bağlanabileceği](#connect-without-registering-devices)senaryoları destekler.

IoT Central, bağlantı işlemini yönetmek için [Azure IoT Hub Aygıt Sağlama hizmetini (DPS)](../../iot-dps/about-iot-dps.md) kullanır. Bir aygıt, uygulamanıza bağlanması için gereken bilgileri almak için önce bir DPS bitiş noktasına bağlanır. Dahili olarak, IoT Central uygulamanız aygıt bağlantısını işlemek için bir IoT hub'ı kullanır. DPS'nin kullanılması şunları sağlar:

- IoT Central, ölçekte biniş ve bağlantı aygıtlarını desteklemek için.
- Aygıt kimlik bilgilerini oluşturmak ve aygıtları IoT Merkezi Web Aracı aracılığıyla kaydetmeden çevrimdışı olarak yapılandırmak için.
- IoT Central'daki cihazları kaydetmek için kendi cihaz lı lı lıları kullanmanız gerekir. Kendi aygıt ınızın kullanılması, varolan arka ofis sistemleriyle tümleştirmeyi kolaylaştırır.
- Aygıtları IoT Central'a bağlamanın tek ve tutarlı bir yolu.

Bir aygıt ve uygulamanız arasındaki iletişimi sağlamak için IoT Central hem paylaşılan erişim imzalarını (SAS) hem de X.509 sertifikalarını destekler. X.509 sertifikaları üretim ortamlarında önerilir.

Bu makalede, aşağıdaki kullanım örnekleri açıklanmaktadır:

- [SAS kullanarak tek bir cihazı bağlama](#connect-a-single-device)
- [SAS kullanarak cihazları ölçekte bağlama](#connect-devices-at-scale-using-sas)
- Üretim ortamları için önerilen yaklaşım olan [X.509 sertifikalarını kullanarak cihazları ölçekte bağlayın.](#connect-devices-using-x509-certificates)
- [Aygıtları ilk kaydettirmeden bağlama](#connect-without-registering-devices)
- [DPS tek tek kayıtları kullanan aygıtları bağlama](#individual-enrollment-based-device-connectivity)
- [IoT Tak ve Çalıştır (önizleme) özelliklerini kullanarak cihazları bağlama](#connect-devices-with-iot-plug-and-play-preview)

## <a name="connect-a-single-device"></a>Tek bir cihazı bağlama

Bu yaklaşım, IoT Merkezi veya test cihazlarıyla denemeler yaparken yararlıdır. Bir cihazı IoT Central uygulamanıza bağlamak için IoT Central uygulamanızdan sas anahtarlarını kullanabilirsiniz. Aygıt _SAS anahtarını_ kayıtlı bir aygıtın bağlantı bilgilerinden kopyalayın:

![Tek bir cihaz için SAS tuşları](./media/concepts-get-connected/single-device-sas.png)

Daha fazla bilgi edinmek için Create'e bakın [ve Bir Düğüm istemcisi uygulamasını Azure IoT Central uygulama öğreticinize bağlayın.](./tutorial-connect-device.md)

## <a name="connect-devices-at-scale-using-sas"></a>SAS kullanarak cihazları ölçekte bağlama

Aygıtları SAS tuşlarını kullanarak ölçekte IoT Central'a bağlamak için, aygıtları kaydettirmeniz ve sonra ayarlamanız gerekir:

### <a name="register-devices-in-bulk"></a>Aygıtları toplu olarak kaydetme

IoT Merkezi uygulamanızla çok sayıda aygıt kaydetmek için, aygıt [adlarını ve aygıt adlarını almak](howto-manage-devices.md#import-devices)için bir CSV dosyası kullanın.

Alınan aygıtların bağlantı bilgilerini almak [için, IoT Central uygulamanızdan bir CSV dosyası dışa aktarın.](howto-manage-devices.md#export-devices) Dışa aktarılan CSV dosyasında aygıt işleri ve SAS anahtarları yer alıyor.

### <a name="set-up-your-devices"></a>Cihazlarınızı ayarlama

Aygıtlarınızın IoT Merkezi uygulamanıza bağlanıp IoT'ye veri göndermesine olanak sağlamak için aygıt kodunuzdaki dışa aktarma dosyasındaki bağlantı bilgilerini kullanın. Ayrıca başvurunuz için DPS **Kimlik kapsamı** gerekir. Bu değeri Yönetim **> Aygıt bağlantısında**bulabilirsiniz.

> [!NOTE]
> Aygıtları IoT Central'a kaydetmeden nasıl bağlanabileceğinizi öğrenmek için, [aygıtları kaydetmeden Bağlan'a](#connect-without-registering-devices)bakın.

## <a name="connect-devices-using-x509-certificates"></a>X.509 sertifikalarını kullanarak cihazları bağlama

Üretim ortamında, X.509 sertifikaları kullanılarak IoT Central için önerilen aygıt kimlik doğrulama mekanizması dır. Daha fazla bilgi için [X.509 CA Sertifikalarını kullanarak Aygıt Kimlik Doğrulaması'na](../../iot-hub/iot-hub-x509ca-overview.md)bakın.

X.509 sertifikasına sahip bir aygıtı bağlamadan önce, uygulamanızda bir ara veya kök X.509 sertifikası ekleyin ve doğrulayın. Aygıtlar kök veya ara sertifikadan oluşturulan yaprak X.509 sertifikalarını kullanmalıdır.

### <a name="add-and-verify-a-root-or-intermediate-certificate"></a>Kök veya ara sertifika ekleme ve doğrulama

Yönetim **> Aygıt Bağlantısı > Birincil Sertifikayı Yönet'e** gidin ve aygıt sertifikalarını oluşturmak için kullandığınız X.509 kök veya ara sertifikasını ekleyin.

![Bağlantı ayarları](media/concepts-get-connected/manage-x509-certificate.png)

Sertifika sahipliğinin doğrulanması, sertifikayı yükleyen kişinin sertifikanın özel anahtarına sahip olmasını sağlar. Sertifikayı doğrulamak için:

  1. Kod oluşturmak için **Doğrulama Kodu'nun** yanındaki düğmeyi seçin.
  1. Önceki adımda oluşturduğunuz doğrulama koduyla bir X.509 doğrulama sertifikası oluşturun. Sertifikayı .cer dosyası olarak kaydedin.
  1. İmzalı doğrulama sertifikasını yükleyin ve **Doğrula'yı**seçin. Doğrulama başarılı olduğunda sertifika **Doğrulandı** olarak işaretlenir.

Bir güvenlik ihlaliniz varsa veya birincil sertifikanızın süresi dolacaksa, kapalı kalma süresini azaltmak için ikincil sertifikayı kullanın. Birincil sertifikayı güncellerken ikincil sertifikayı kullanarak aygıtları sağlamaya devam edebilirsiniz.

### <a name="register-and-connect-devices"></a>Cihazları kaydetme ve bağlama

X.509 sertifikalarını kullanarak aygıtları toplu olarak bağlamak için, aygıt [adlarını ve aygıt adlarını almak](howto-manage-devices.md#import-devices)için bir CSV dosyası kullanarak önce cihazları uygulamanızda kaydedin. Aygıt adlarının tümü küçük harfle olmalıdır.

Yüklenen kök veya ara sertifikayı kullanarak aygıtlarınız için X.509 yaprak sertifikaları oluşturun. **Yaprak** sertifikalarında `CNAME` değer olarak Aygıt Kimliğini kullanın. Cihaz kodunuz, uygulamanız, **aygıt kimliğiniz**ve ilgili cihaz sertifikası için **kimlik kapsamı** değerine ihtiyaç duyar.

### <a name="for-testing-purposes-only"></a>Yalnızca test amaçlı

Yalnızca sınama için, kök, ara ve aygıt sertifikaları oluşturmak için aşağıdaki yardımcı programları kullanabilirsiniz:

- [Azure IoT Aygıt Sağlama Aygıtı SDK için araçlar:](https://github.com/Azure/azure-iot-sdk-node/blob/master/provisioning/tools/readme.md)X.509 sertifikaları ve anahtarları oluşturmak ve doğrulamak için kullanabileceğiniz Node.js araçları koleksiyonu.
- DevKit aygıtı kullanıyorsanız, bu [komut satırı aracı,](https://aka.ms/iotcentral-docs-dicetool) sertifikaları doğrulamak için IoT Central uygulamanıza ekleyebileceğiniz bir CA sertifikası oluşturur.
- [Örnekler ve öğreticiler için test CA sertifikalarını yönetin:](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md)PowerShell ve Bash komut dosyaları nın bir koleksiyonu:
  - Sertifika zinciri oluşturun.
  - Sertifikaları IoT Central uygulamanıza yüklemek için .cer dosyaları olarak kaydedin.
  - Doğrulama sertifikasını oluşturmak için IoT Central uygulamasındaki doğrulama kodunu kullanın.
  - Aracı parametre olarak aygıt larınızı kullanarak cihazlarınız için yaprak sertifikalar oluşturun.

### <a name="further-reference"></a>Daha fazla başvuru

- [RaspberryPi için örnek uygulama](https://aka.ms/iotcentral-docs-Raspi-releases)
- [C'de örnek cihaz istemcisi](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md)

## <a name="connect-without-registering-devices"></a>Aygıtları kaydetmeden bağlanma

Daha önce açıklanan senaryoların tümü, aygıtları bağlanmadan önce uygulamanızda kaydetmenizi gerektirir. IoT Central, OEM'lerin ilk kaydedilmeden bağlanabilen toplu üretim aygıtlarına da olanak tanır. OEM uygun aygıt kimlik bilgilerini oluşturur ve fabrikadaki aygıtları yapılandırır. Bir müşteri bir aygıtı ilk kez çevirdiğinde, DPS'ye bağlanır ve ardından cihazı otomatik olarak doğru IoT Merkezi uygulamasına bağlar. Bir IoT Merkezi operatörü, uygulamaya veri göndermeye başlamadan önce aygıtı onaylamalıdır.

Akış, aygıtların SAS belirteçleri veya X.509 sertifikaları kullanıp kullanmadığına bağlı olarak biraz farklıdır:

### <a name="connect-devices-that-use-sas-tokens-without-registering"></a>Kayıt olmadan SAS belirteçleri kullanan aygıtları bağlama

1. IoT Central uygulamasının grup birincil anahtarını kopyalayın:

    ![Uygulama grubu birincil SAS anahtarı](media/concepts-get-connected/group-sas-keys.png)

1. Aygıt SAS tuşlarını oluşturmak için [dps-keygen](https://www.npmjs.com/package/dps-keygen) aracını kullanın. Önceki adımdaki grup birincil anahtarını kullanın. Aygıt işleri daha alt harfli olmalıdır:

    ```cmd
    dps-keygen -mk:<group primary key> -di:<device ID>
    ```

1. OEM her aygıtı bir aygıt kimliği, oluşturulan bir aygıt SAS anahtarı ve uygulama **kimliği kapsam** değeriyle yanıp söner.

1. Bir aygıtı açtığınızda, ioT Merkezi kayıt bilgilerini almak için önce DPS'ye bağlanır.

    Aygıt başlangıçta **Aygıtlar** sayfasında **İlişkisiz** bir aygıt durumuna sahiptir ve aygıt şablonuna atanmaz. **Aygıtlar** sayfasında, aygıtı uygun aygıt şablonuna **geçirin.** Aygıt sağlama artık tamamlandı, aygıt durumu artık **sağlanmıştır**ve aygıt veri göndermeye başlayabilir.

    **Aygıtın Yönetimi > Bağlantı** sayfasında, **Otomatik onay** seçeneği, veri göndermeye başlamadan önce aygıtı el ile onaylamanız gerekip gerekmediğini denetler.

    > [!NOTE]
    > Bir aygıtı bir aygıt şablonuyla nasıl otomatik olarak ilişkilendirecek lerini öğrenmek için, [ioT Tak ve Çalıştır (önizleme) ile Aygıtları Bağla'ya](#connect-devices-with-iot-plug-and-play-preview)bakın.

### <a name="connect-devices-that-use-x509-certificates-without-registering"></a>X.509 sertifikalarını kullanan cihazları kaydolmadan bağlama

1. IoT Central uygulamanıza [bir kök veya ara X.509 sertifikası ekleyin ve doğrulayın.](#connect-devices-using-x509-certificates) (#connect-cihazlar-using-x509-certificates)

1. IoT Central uygulamanıza eklediğiniz kök veya ara sertifikayı kullanarak aygıtlarınızın yaprak sertifikalarını oluşturun. Yaprak sertifikalarında küçük `CNAME` harfli aygıt adlarını kullanın.

1. OEM her aygıtı bir aygıt kimliği, oluşturulan sol x.509 sertifikası ve uygulama **kimliği kapsam** değeriyle yanıp söner.

1. Bir aygıtı açtığınızda, ioT Merkezi kayıt bilgilerini almak için önce DPS'ye bağlanır.

    Aygıt başlangıçta **Aygıtlar** sayfasında **İlişkisiz** bir aygıt durumuna sahiptir ve aygıt şablonuna atanmaz. **Aygıtlar** sayfasında, aygıtı uygun aygıt şablonuna **geçirin.** Aygıt sağlama artık tamamlandı, aygıt durumu artık **sağlanmıştır**ve aygıt veri göndermeye başlayabilir.

    **Aygıtın Yönetimi > Bağlantı** sayfasında, **Otomatik onay** seçeneği, veri göndermeye başlamadan önce aygıtı el ile onaylamanız gerekip gerekmediğini denetler.

    > [!NOTE]
    > Bir aygıtı bir aygıt şablonuyla nasıl otomatik olarak ilişkilendirecek lerini öğrenmek için, [ioT Tak ve Çalıştır (önizleme) ile Aygıtları Bağla'ya](#connect-devices-with-iot-plug-and-play-preview)bakın.

## <a name="individual-enrollment-based-device-connectivity"></a>Tek tek kayıt tabanlı aygıt bağlantısı

Her birinin kendi kimlik doğrulama kimlik bilgilerine sahip aygıtları bağlaması için tek tek kayıtları kullanın. Tek bir kayıt, bağlanmasına izin verilen tek bir aygıtın girişidir. Tek tek kayıtlar, x.509 yaprak sertifikalarını veya SAS belirteçlerini (fiziksel veya sanal olarak güvenilen platform modülünden) attestation mekanizmaları olarak kullanabilir. Tek bir kayıttaki aygıt kimliği (kayıt kimliği olarak da bilinir) alfasayısal, küçük ve tire içerebilir. Daha fazla bilgi için [DPS bireysel kaydına](https://docs.microsoft.com/azure/iot-dps/concepts-service#individual-enrollment)bakın.

> [!NOTE]
> Bir aygıt için tek tek bir kayıt oluşturduğunuzda, bu, IoT Merkezi uygulamanızdaki varsayılan grup kayıt seçeneklerinden önce gelir.

### <a name="creating-individual-enrollments"></a>Tek tek kayıt oluşturma

IoT Central, bireysel kayıtlar için aşağıdaki attestation mekanizmalarını destekler:

- **Simetrik anahtar attestation:** Simetrik anahtar attestation DPS örneği ile bir cihazın kimlik doğrulaması için basit bir yaklaşımdır. Simetrik anahtarlar kullanan tek bir kayıt oluşturmak için **Aygıt Bağlantısı** sayfasını açın, bağlantı yöntemi olarak **Bireysel kaydı** ve mekanizma olarak Paylaşılan erişim **imzası (SAS)'yi** seçin. Base64 kodlanmış birincil ve ikincil tuşları girin ve değişikliklerinizi kaydedin. **Kimliğinizi,** **Aygıt Kimliğini**ve cihazınızı bağlamak için birincil veya ikincil anahtarı kullanın.

    > [!TIP]
    > Test için base64 kodlanmış anahtarları oluşturmak için **OpenSSL'yi** kullanabilirsiniz:`openssl rand -base64 64`

- **X.509 sertifikaları:** X.509 sertifikalarına sahip tek bir kayıt oluşturmak için **Aygıt Bağlantısı** sayfasını açın, bağlantı yöntemi olarak **Bireysel kayıt** ve mekanizma olarak **Sertifikalar (X.509)** seçeneğini belirleyin. Tek tek kayıt girişiyle kullanılan aygıt sertifikalarının, verenin ve konu CN'sinin aygıt kimliğine ayarlanmış olması gerekir.

    > [!TIP]
    > Test etmek için, Kendi imzalanmış bir sertifika oluşturmak [için Node.js için Azure IoT Aygıt Sağlama Aygıtı SDK'sını](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/tools) kullanabilirsiniz:`node create_test_cert.js device "mytestdevice"`

- **Güvenilir Platform Modülü (TPM) attestation:** [TPM](https://docs.microsoft.com/azure/iot-dps/concepts-tpm-attestation) donanım güvenlik modülü türüdür. TPM kullanmak, bir aygıtı bağlamanın en güvenli yollarından biridir. Bu makalede, ayrı bir, firmware veya entegre TPM kullandığınızı varsayar. Yazılım taklit TPM'ler prototip oluşturma veya test etmek için uygundur, ancak ayrık, firmware veya entegre TPM'ler ile aynı güvenlik düzeyini sağlamaz. Üretimde yazılım TPM'leri kullanmayın. TPM kullanan tek bir kayıt oluşturmak için **Aygıt Bağlantısı** sayfasını açın, bağlantı yöntemi olarak **Bireysel kaydı** ve mekanizma olarak **TPM'yi** seçin. TPM onay anahtarını girin ve aygıt bağlantı bilgilerini kaydedin.

## <a name="connect-devices-with-iot-plug-and-play-preview"></a>IoT Tak ve Çalıştır ile cihazları bağlama (önizleme)

IoT Central ile IoT Tak ve Çalıştır'ın (önizleme) temel özelliklerinden biri, aygıt şablonlarını aygıt bağlantısında otomatik olarak ilişkilendirebilme yeteneğidir. Aygıt kimlik bilgileriyle birlikte, aygıtlar artık **CapabilityModelId'i** aygıt kayıt çağrısının bir parçası olarak gönderebilir. Bu özellik, IoT Central'ın aygıt şablonunu keşfetmesini ve aygıtla ilişkilendirmesini sağlar. Bulma işlemi aşağıdaki gibi çalışır:

1. IoT Central uygulamasında zaten yayınlanmışsa aygıt şablonuyla ilişkilendirilir.
1. Yayınlanmış ve onaylı yetenek modellerinin genel deposundan getirilir.

Aşağıda, DPS kayıt çağrısı sırasında aygıtın göndereceği ek yükün biçimi

```javascript
'__iot:interfaces': {
    CapabilityModelId: <this is the URN for the capability model>
}
```

> [!NOTE]
> Aygıtların otomatik olarak bağlanabilmesi, aygıt şablonunu keşfetmesi ve veri göndermeye başlaması için **Yönetim > Aygıt bağlantısındaki** **Otomatik onaylama** seçeneğinin etkinleştirilmesi gerektiğini unutmayın.

## <a name="device-status-values"></a>Cihaz durum değerleri

Gerçek bir aygıt IoT Merkezi uygulamanıza bağlandığında, aygıt durumu aşağıdaki gibi değişir:

1. Aygıt durumu ilk **Olarak Kaydedilir.** Bu durum, aygıtın IoT Central'da oluşturulduğu ve aygıt kimliğine sahip olduğu anlamına gelir. Bir aygıt şu anda kaydedilir:
    - **Cihazlar** sayfasına yeni bir gerçek aygıt eklenir.
    - **Aygıtlar** sayfasında Içe **Aktar** kullanılarak bir aygıt kümesi eklenir.

1. Aygıt durumu, IoT Central uygulamanıza geçerli kimlik bilgileriyle bağlanan aygıt sağlama adımını tamamladığında **VerilenE** göre değişir. Bu adımda, aygıt IoT Central uygulamanız tarafından kullanılan IoT Hub'ından otomatik olarak bir bağlantı dizesini almak için DPS kullanır. Cihaz artık IoT Central'a bağlanabilir ve veri göndermeye başlayabilir.

1. Bir operatör aygıtı engelleyebilir. Bir aygıt engellendiğinde, IoT Merkezi uygulamanıza veri gönderemez. Engellenen aygıtların **Engellendi**durumu vardır. Veri göndermeye devam edeabilmesi için bir işleç aygıtı sıfırlamalıdır. Bir işleç aygıtın engelini geri aldığında durum önceki değerine geri döner, **Kayıtlı** veya **Sağlanan**.

1. Aygıt durumu **Onay Bekliyorsa,** Otomatik **onaylama** seçeneği devre dışı bırakılır anlamına gelir. Bir işleç, veri göndermeye başlamadan önce aygıtı açıkça onaylamalıdır. **Aygıtlar** sayfasında el ile kaydedilmemiş, ancak geçerli kimlik bilgilerine bağlı aygıtlar **Onay Bekliyor**cihaz durumuna sahip olur. İşleçler bu aygıtları **Onayla** düğmesini kullanarak **Aygıtlar** sayfasından onaylayabilir.

1. Aygıt durumu **İlişkili değilse,** IoT Central'a bağlanan aygıtın ilişkili bir aygıt şablonu olmadığı anlamına gelir. Bu durum genellikle aşağıdaki senaryolarda olur:

    - Aygıt şablonu belirtilmeden **Aygıtlar** sayfasında **Içe Aktar** kullanılarak bir aygıt kümesi eklenir.
    - Aygıt şablonu belirtilmeden **aygıtlar** sayfasında el ile kaydedildi. Aygıt daha sonra geçerli kimlik bilgilerine bağlanır.  

    Operatör, **Geçir** düğmesini kullanarak **aygıtları Aygıtlar** sayfasından bir aygıt şablonuna bağlayabilir.

## <a name="best-practices"></a>En iyi uygulamalar

Cihazı ilk bağladığınızda DPS'nin döndürdeğindeki aygıt bağlantı dizesini kalıcı olarak sunmayın veya önbelleğe yüklemeyin. Bir aygıtı yeniden bağlamak için, doğru aygıt bağlantı dizesini almak için standart aygıt kayıt akışını gözden geçirin. Aygıt bağlantı dizesini önbelleğe alırsa, IoT Central kullandığı temel Azure IoT hub'ını güncelleştirirse, aygıt yazılımı eski bir bağlantı dizesi alma riskine girer.

## <a name="sdk-support"></a>SDK desteği

Azure Aygıt SDK'ları, aygıt kodunuzu uygulamanız için en kolay yolu sunar. Aşağıdaki aygıt SDK'ları kullanılabilir:

- [C için Azure IoT SDK'sı](https://github.com/azure/azure-iot-sdk-c)
- [Python için Azure IoT SDK'sı](https://github.com/azure/azure-iot-sdk-python)
- [Node.js için Azure IoT SDK'sı](https://github.com/azure/azure-iot-sdk-node)
- [Java için Azure IoT SDK'sı](https://github.com/azure/azure-iot-sdk-java)
- [.NET için Azure IoT SDK'sı](https://github.com/azure/azure-iot-sdk-csharp)

### <a name="sdk-features-and-iot-hub-connectivity"></a>SDK özellikleri ve IoT Hub bağlantısı

IoT Hub ile tüm aygıt iletişimi aşağıdaki IoT Hub bağlantı seçeneklerini kullanır:

- [Aygıt-bulut mesajlaşma](../../iot-hub/iot-hub-devguide-messages-d2c.md)
- [Cihaz ikizleri](../../iot-hub/iot-hub-devguide-device-twins.md)

Aşağıdaki tablo, Azure IoT Merkezi aygıtının IoT Hub'ın özellikleriyle nasıl eşleşiş özelliklerini özetleyerek şunları özetler:

| Azure IoT Central | Azure IoT Hub’ı |
| ----------- | ------- |
| Telemetri | Aygıt-bulut mesajlaşma |
| Özellik | Aygıt ikizi bildirilen özellikler |
| Özellik (yazılabilir) | Cihaz ikiz ivezisi istenen ve bildirilen özellikler |
| Komut | Doğrudan yöntemler |

Aygıt SDK'larını kullanma hakkında daha fazla bilgi edinmek için [bkz.](howto-connect-devkit.md)

### <a name="protocols"></a>Protokoller

Aygıt SDK'ları bir IoT hub'ına bağlanmak için aşağıdaki ağ protokollerini destekler:

- MQTT
- AMQP
- HTTPS

Bu fark protokolleri ve birini seçme kılavuzu hakkında bilgi için [bkz.](../../iot-hub/iot-hub-devguide-protocols.md)

Aygıtınız desteklenen protokollerden hiçbirini kullanamıyorsa, protokol dönüştürmesi yapmak için Azure IoT Edge'i kullanabilirsiniz. IoT Edge, Azure IoT Merkezi uygulamasından işlemeyi kenara boşaltmak için diğer kenardan ilgili bilgileri destekler.

## <a name="security"></a>Güvenlik

Aygıtlar ve Azure IoT Merkeziniz arasında değiş tokuş edilen tüm veriler şifrelenir. IoT Hub, aygıta bakan IoT Hub uç noktalarından herhangi biri için bağlanan bir aygıttan gelen her isteği doğrular. Bir aygıt, kimlik bilgilerinin kablo üzerinden değiş tokuş etmesini önlemek için kimlik doğrulama için imzalı belirteçler kullanır. Daha fazla bilgi için bkz: [IoT Hub'a erişimi denetle.](../../iot-hub/iot-hub-devguide-security.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central'da aygıt bağlantısı hakkında bilgi edindiğinize göre, önerilen sonraki adımlar şunlardır:

- [DevKit aygıtı hazırlama ve bağlama](howto-connect-devkit.md)
- [C SDK: Prove cihaz istemcisi SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md)
