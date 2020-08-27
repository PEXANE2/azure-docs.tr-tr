---
title: IoT Tak ve Kullan cihazlarını onaylama | Microsoft Docs
description: Bir cihaz Oluşturucu olarak, testlerin nasıl çalıştırılacağını ve sertifika için bir cihaz nasıl göndereceğini öğrenin
author: konichi3
ms.author: koichih
ms.date: 08/21/2020
ms.topic: how-to
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: bdb6bf166e84bb9134bbd14454899bcefbf0a887
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88949907"
---
# <a name="how-to-certify-iot-plug-and-play-devices"></a>IoT Tak ve Kullan cihazlarını onaylama

IoT Tak ve Kullan cihaz Sertifikası programı, bir cihazın IoT Tak ve Kullan sertifika gereksinimlerini karşılayıp karşılamadığını denetlemek için araçlar içerir. Bu araçlar, kuruluşların IoT Tak ve Kullan cihazlarının kullanılabilirliğini de kullanmalarına yardımcı olur. Bu sertifikalı cihazlar IoT çözümleri için tasarlanmıştır ve pazara ulaşma süresini azaltmaya yardımcı olur.

Bu makale, şunları nasıl yapacağınızı gösterir:

- Azure CLı için Azure IoT komut satırı aracı uzantısını yükler
- Geliştirme aşamasında cihaz uygulamanızı doğrulamak için IoT Tak ve Kullan testlerini çalıştırın  
- Cihaz uygulamasını doğrulamak için Azure Sertifikalı cihaz portalını kullanma

## <a name="prepare-your-device"></a>Cihazınızı hazırlama

IoT Tak ve Kullan çalışan uygulama kodu şunları içermelidir:

- [Cihaz sağlama hizmeti 'ni (DPS)](../iot-dps/about-iot-dps.md)kullanarak Azure IoT Hub 'a bağlanın.
- Telemetri, özellik ve komutları uygulamak için [IoT eklentisi yürütme kurallarını](concepts-convention.md) izleyin.

Uygulama, işletim sisteminden ayrı olarak yüklenen veya cihaza düzolan bir bellenim görüntüsündeki işletim sistemiyle birlikte paketlenmiş bir yazılımdır.

Sertifika işlemi, cihaz uygulamasının, [dijital TWINS tanım dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl) cihaz modelinde tanımlanan telemetri, Özellikler ve komutlarla eşleşip eşleşmediğini ve modelin [Azure IoT ortak modeli deposunda](concepts-model-repository.md)kullanılabildiğini doğrulayarak IoT Tak ve Kullan uyumlu olduğunu doğrular.

Sertifika gereksinimlerini karşılamak için cihazınızın şunları yapmanız gerekir:

- [DPS](../iot-dps/about-iot-dps.md)kullanarak Azure IoT Hub 'e bağlanır.
- IoT Tak ve Kullan kuralını takip eden telemetri, özellik veya komutların uygulanması.
- [Dtdl v2](https://aka.ms/dtdl) modeliyle cihaz etkileşimlerini açıklama.
- [Azure IoT ortak model deposundaki](https://devicemodels.azureiotsolutions.com/) modeli ve gerekli tüm arabirimleri yayımlayın
- DPS sağlama yükünde, [DPS kaydı](concepts-developer-guide.md#dps-payload) SıRASıNDA model kimliğini gönderin.
- [MQTT bağlantısı](concepts-developer-guide.md#model-id-announcement)SıRASıNDA model kimliğini duyurur.

## <a name="test-with-the-azure-iot-extension-cli"></a>Azure IoT uzantısı CLı ile test etme

[Azure ıOT CLI uzantısı](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/product?view=azure-cli-latest) , cihazı Azure Sertifikalı cihaz portalı aracılığıyla sertifika için göndermeden önce cihaz uygulamasının modelle eşleştiğini doğrulamanızı sağlar.

Aşağıdaki adımlarda, CLı kullanarak sertifika testlerinin nasıl hazırlanacağı ve çalıştırılacağı gösterilmektedir:

### <a name="install-the-azure-iot-extension-for-the-azure-cli"></a>Azure CLı için Azure IoT uzantısını yükler

Ortamınızdaki [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) 'yı ayarlamak için yükleme yönergelerine bakın.

Azure IoT uzantısını yüklemek için şu komutu çalıştırın:

```azurecli
az extension add --name azure-iot
```

Daha fazla bilgi için bkz. [Azure IoT Için Azure CLI](https://docs.microsoft.com/cli/azure/azure-cli-reference-for-iot?view=azure-cli-latest).

### <a name="create-a-new-product-test"></a>Yeni bir ürün testi oluşturun

Aşağıdaki komut, simetrik anahtar kanıtlama yöntemiyle DPS kullanarak bir test oluşturur:

- Test etmek için yeni bir ürün oluşturur ve bir test yapılandırması oluşturur. Çıktı, cihazın sağlama için kullanması gereken DPS bilgilerini görüntüler: birincil anahtar, cihaz KIMLIĞI ve KIMLIK kapsamı.
- Modelinizi tanımlayan DTDL dosyalarını içeren klasörü belirtir.

```azurecli
az iot product test create --badge-type Pnp --at SymmetricKey --device-type FinishedProduct --models {local folder name}
```

> [!NOTE]
> CLı kullandığınızda aboneliğinizde [oturum açmanız](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) gerekir.

Komuttan gelen JSON çıktısı, `primaryKey` `registrationId` cihazınızı bağladığınızda kullanılacak, ve öğesini içerir `scopeID` .

Beklenen çıktı:

```json
"deviceType": "FinishedProduct",
"id": "d45d53d9-656d-4be7-9bbf-140bc87e98dc",
"provisioningConfiguration": {
  "symmetricKeyEnrollmentInformation": {
    "primaryKey":"Ci/Ghpqp0n4m8FD5PTicr6dEikIfM3AtVWzAhObU7yc=",
    "registrationId": "d45d53d9-656d-4be7-9bbf-140bc87e98dc",
    "scopeId": "0ne000FFA42"
  }
}
```

### <a name="connect-your-device"></a>Cihazınızı bağlama

Cihazınızı test IoT Hub örneğine bağlamak için önceki komutla karşı DPS bilgi çıktısını kullanın.

### <a name="manage-and-configure-the-product-tests"></a>Ürün testlerini yönetme ve yapılandırma

Cihaz bağlanıp IoT Hub ile etkileşime geçmeye hazırsanız, bir ürün testi yapılandırma dosyası oluşturun. Dosyayı oluşturmak için:

- `id`Önceki komutun çıktısından testi kullanın.
- `--wait`Test çalışmasını almak için parametresini kullanın.

```azurecli
az iot product test task create --type GenerateTestCases --test-id d45d53d9-656d-4be7-9bbf-140bc87e98dc --wait
```

Beklenen çıktı:

```json
{
  "deviceTestId": "d45d53d9-656d-4be7-9bbf-140bc87e98dc",
  "error": null,
  "id": "526da38e-91fc-4e20-a761-4f04b392c42b",
  "resultLink": "/deviceTests/d45d53d9-656d-4be7-9bbf-140bc87e98dc/TestCases",
  "status": "Completed",
  "type": "GenerateTestCases"
}
```

`az iot product test case update`Test yapılandırma dosyasını değiştirmek için komutunu kullanabilirsiniz.

### <a name="run-the-tests"></a>Testleri çalıştırma

Test yapılandırması oluşturduktan sonra, bir sonraki adım testleri çalıştırmak için kullanılır. `devicetestId`Testleri çalıştırmak için önceki komutlardan parametre olarak aynısını kullanın. Tüm testlerin bir durumu olduğundan emin olmak için test sonuçlarını denetleyin `Passed` .

```azurecli
az iot product test task create --type QueueTestRun --test-id d45d53d9-656d-4be7-9bbf-140bc87e98dc --wait
```

Örnek test çalıştırması çıkışı

```json
      "validationTasks": [
        {
          "componentName": "Default component",
          "endTime": "2020-08-25T05:18:49.5224772+00:00",
          "interfaceId": "dtmi:com:example:TemperatureController;1",
          "logs": [
            {
              "message": "Waiting for telemetry from the device",
              "time": "2020-08-25T05:18:37.3862586+00:00"
            },
            {
              "message": "Validating PnP properties",
              "time": "2020-08-25T05:18:37.3875168+00:00"
            },
            {
              "message": "Validating PnP commands",
              "time": "2020-08-25T05:18:37.3894343+00:00"
            },
            {
              "message": "{\"propertyName\":\"serialNumber\",\"expectedSchemaType\":null,\"actualSchemaType\":null,\"message\":\"Property is successfully validated\",\"passed\":true,\"time\":\"2020-08-25T05:18:37.4205985+00:00\"}",
              "time": "2020-08-25T05:18:37.4205985+00:00"
            },
            {
              "message": "PnP interface properties validation passed",
              "time": "2020-08-25T05:18:37.4206964+00:00"
            },
```

## <a name="test-using-the-azure-certified-device-portal"></a>Azure Sertifikalı cihaz portalını kullanarak test etme

Aşağıdaki adımlarda, [Azure Sertifikalı cihaz portalı](https://aka.ms/acdp) 'nı eklemek, ürün ayrıntılarını kaydetmek, Başlangıç Kılavuzu göndermek ve sertifika testlerini çalıştırmak için nasıl kullanılacağı gösterilmektedir.

> [!NOTE]
> Yazma sırasında Portal, [Azure IoT cihaz kataloğunda sertifikalı](https://aka.ms/devicecatalog)yayımlamayı desteklemez.

### <a name="onboarding"></a>Ekleme

[Sertifika portalını](https://aka.ms/acdp)kullanmak için iş veya okul kiracınızdan bir Azure Active Directory kullanmanız gerekir.

Modelleri Azure IoT ortak modeli deposuna yayımlamak için hesabınızın [Microsoft iş ortağı ağı](https://partner.microsoft.com)bir üyesi olması gerekir. Sistem, Microsoft İş Ortağı Ağı KIMLIĞININ mevcut olduğunu ve hesabın cihaz kataloğuna yayımlamadan önce tam olarak güvenilir olup olmadığını denetler.

### <a name="company-profile"></a>Şirket profili

Şirket Profilinizi sol gezinti menüsünden yönetebilirsiniz. Şirket Profili Şirket URL 'SI, e-posta adresi ve şirket logosu içerir. Herhangi bir sertifika işlemini çalıştırmadan önce program sözleşmesinin bu sayfada kabul edilmesi gerekir.

Şirket profili bilgileri, cihaz kataloğunda gösterilen cihaz açıklamasında kullanılır.

### <a name="create-new-project"></a>Yeni proje oluşturma

Bir cihazı onaylamak için, önce yeni bir proje oluşturmanız gerekir.

[Sertifika portalına](https://aka.ms/acdp)gidin. **Projeler** sayfasında *+ Yeni proje oluştur*' u seçin. Ardından proje için bir ad, cihaz adı girin ve bir cihaz sınıfı seçin.

Sertifika işlemi sırasında verdiğiniz ürün bilgileri dört kategoriye ayrılır:

- Cihaz bilgileri. Cihaz hakkındaki adı, açıklaması, sertifikaları ve işletim sistemi gibi bilgileri toplar.
- **Başlarken** Kılavuzu. Cihazı yayımlamadan önce, Kılavuzu Sistem Yöneticisi tarafından onaylanmak üzere bir PDF belgesi olarak göndermeniz gerekir.
- Pazarlama ayrıntıları. Cihazınız için müşteriye hazırlamış pazarlama bilgilerini sağlayın. Pazarlama bilgileri, açıklama, fotoğraf ve dağıtıcılar içerir.
- Ek sektör sertifikaları. Bu isteğe bağlı bölüm, cihazın aldığı diğer sertifikalar hakkında ek bilgiler sağlamanıza olanak tanır.

### <a name="connect-and-test"></a>Bağlan ve sına

Bağlan ve test adımı, cihazınızın IoT Tak ve Kullan sertifika gereksinimlerini karşıladığını denetler.

Tamamlanması gereken üç adım vardır:

1. Arabirimleri bağlayın ve bulun. Cihaz, DPS aracılığıyla Azure IoT sertifika hizmetine bağlanmalıdır. Kullanılacak kimlik doğrulama yöntemini (X. 509.952 sertifikası, simetrik anahtarlar veya Güvenilir Platform Modülü) seçin ve cihaz uygulamasını DPS bilgileriyle güncelleştirin.
1. Arabirimleri gözden geçirin. Arabirimi gözden geçirin ve her birinin test için anlamlı olan yük girdilerine sahip olduğundan emin olun.
1. Sınamanız. Sistem, modelde açıklanan telemetri, özellik ve komutların IoT Tak ve Kullan kurallarını izleyip izlediğinden emin olmak için her bir cihaz modelini sınar. Test tamamlandığında, cihazdan Telemetriyi ve IoT Hub Device ikizi özelliklerine gönderilen ham verileri görmek için **günlükleri görüntüle** bağlantısını seçin.

## <a name="next-steps"></a>Sonraki adımlar

Cihaz gönderimi tamamlandı, [iotcert@microsoft.com](mailto:iotcert@microsoft.com) Microsoft iş ortağı ağı üyelik doğrulamasını ve Başlarken kılavuzlarını gözden geçirmeyi içeren sonraki adımlara devam etmek için ' de cihaz sertifikası ekibine başvurabilirsiniz. Tüm gereksinimler karşılandığınızda, cihazınızın [Azure IoT cihaz kataloğuna yönelik sertifikalı](https://aka.ms/devicecatalog)olmasını seçebilirsiniz.
