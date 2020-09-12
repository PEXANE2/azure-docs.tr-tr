---
title: Azure IoT Central uygulamalarınız için cihaz şablonu sürüm oluşturmayı anlama | Microsoft Docs
description: Yeni sürümler oluşturarak ve canlı bağlı cihazlarınızı etkilemeden cihaz şablonlarınızda yineleme yapın
author: sarahhubbard
ms.author: sahubbar
ms.date: 04/24/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: peterpr
ms.custom: device-developer
ms.openlocfilehash: 2025b127a428afa478cfe839c7619df2d7d688d3
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90015968"
---
# <a name="create-a-new-device-template-version"></a>Yeni bir cihaz şablonu sürümü oluştur

*Bu makale, çözüm oluşturucular ve cihaz geliştiricileri için geçerlidir.*

Bir cihaz şablonu, bir cihazın IoT Central nasıl etkileşime gireceğini tanımlayan bir şema içerir. Bu etkileşimler telemetri, Özellikler ve komutları içerir. Hem cihaz hem de IoT Central uygulaması, bilgi alışverişi için bu şemanın paylaşılan olarak anlaşılmasını kullanır. Şemayı bozmadan yalnızca şemada sınırlı değişiklikler yapabilirsiniz. Bu, en çok şema değişikliklerinin cihaz şablonunun yeni bir sürümünü gerektirmesinin nedeni budur. Cihaz şablonunun sürümü, daha eski cihazların anladıkları şema sürümü ile devam etmesine izin verir, daha yeni veya güncelleştirilmiş cihazlar daha sonraki bir şema sürümünü kullanır.

Bir cihaz şablonundaki şema, cihaz yetenek modeli (DCM) ve arabirimlerinde tanımlanmıştır. Cihaz şablonları, bulut özellikleri, görüntüleme özelleştirmeleri ve görünümleri gibi diğer bilgileri içerir. Cihaz şablonunun bu bölümlerinde, cihazın IoT Central verileri nasıl değiş tokuşunu tanımlamayan bir değişiklik yaparsanız, şablonun sürümünü yüklemeniz gerekmez.

Bir işlecin kullanabilmesi için bir sürüm güncelleştirmesi gerektirmeksizin, herhangi bir cihaz şablonu değişikliğini yayımlamanız gerekir. IoT Central, önce şablon sürümü oluşturmadan bir cihaz şablonuna yönelik son değişiklikleri yayımlamanızı engeller.

> [!NOTE]
> Cihaz şablonu oluşturma hakkında daha fazla bilgi edinmek için bkz. [cihaz şablonu ayarlama ve yönetme](howto-set-up-template.md)

## <a name="versioning-rules"></a>Sürüm oluşturma kuralları

Bu bölüm, cihaz şablonları için uygulanan sürüm oluşturma kurallarını özetler. Hem DCMs hem de arabirimlerin sürüm numaraları vardır. Aşağıdaki kod parçacığında bir ortam algılayıcı cihazı için DCM gösterilmektedir. DCM iki arabirime sahiptir: **Deviceınformation** ve **Environmentalalgılayıcı**. Alanların sonundaki sürüm numaralarını görebilirsiniz `@id` . Bu bilgileri IoT Central Kullanıcı arabiriminde görüntülemek için cihaz şablonu düzenleyicisinde **kimliği görüntüle** ' yi seçin.

```json
{
  "@id": "urn:contoso:sample_device:1",
  "@type": "CapabilityModel",
  "implements": [
    {
      "@id": "urn:contoso:sample_device:deviceinfo:1",
      "@type": "InterfaceInstance",
      "name": "deviceinfo",
      "schema": {
        "@id": "urn:azureiot:DeviceManagement:DeviceInformation:1",
        "@type": "Interface",
        "displayName": {
          "en": "Device Information"
        },
        "contents": [...
        ]
      }
    },
    {
      "@id": "urn:contoso:sample_device:sensor:1",
      "@type": "InterfaceInstance",
      "name": "sensor",
      "schema": {
        "@id": "urn:contoso:EnvironmentalSensor:2",
        "@type": "Interface",
        "displayName": {
          "en": "Environmental Sensor"
        },
        "contents": [...
        ]
      }
    }
  ],
  "displayName": {
    "en": "Environment Sensor Capability Model"
  },
  "@context": [
    "http://azureiot.com/v1/contexts/IoTModel.json"
  ]
}
```

* Bir DCM yayımlandıktan sonra, cihaz şablonunun yeni bir sürümünde bile tüm arabirimleri kaldıramazsınız.
* Bir DCM yayımlandıktan sonra, cihaz şablonunun yeni bir sürümünü oluşturursanız bir arabirim ekleyebilirsiniz.
* Bir DCM yayımlandıktan sonra, cihaz şablonunun yeni bir sürümünü oluşturursanız bir arabirimi daha yeni bir sürümle değiştirebilirsiniz. Örneğin, algılayıcı v1 cihaz şablonu Environmentalalgılayıcı v1 arabirimini kullanıyorsa, Environmentalalgılayıcı v2 arabirimini kullanan bir algılayıcı v2 cihaz şablonu oluşturabilirsiniz.
* Bir arabirim yayımlandıktan sonra, yeni bir cihaz şablonu sürümünde bile arabirim içeriklerinin hiçbirini kaldıramazsınız.
* Arabirim yayımlandıktan sonra, arabirim ve cihaz şablonunun yeni bir sürümünü oluşturursanız bir arabirimin içeriğine öğeler ekleyebilirsiniz. Arabirime ekleyebileceğiniz öğeler telemetri, Özellikler ve komutları içerir.
* Bir arabirim yayımlandıktan sonra, arabirimin ve cihaz şablonunun yeni bir sürümünü oluşturursanız, arabirimdeki varolan öğelerde şema dışı değişiklikler yapabilirsiniz. Bir arabirim öğesinin şema dışı kısımları, görünen adı ve anlam türünü içerir. Değiştiremiyoruz bir arabirim öğesinin şema bölümleri ad, yetenek türü ve şemadır.

Aşağıdaki bölümlerde, IoT Central cihaz şablonlarının değiştirilmesine ilişkin bazı örnekler gösterilmektedir.

## <a name="customize-the-device-template-without-versioning"></a>Cihaz şablonunu sürüm oluşturma olmadan özelleştirme

Cihaz olanaklarınızın belirli öğeleri, cihaz şablonu ve arabirimlerinizin sürümü gerekmeden düzenlenebilir. Örneğin, bu alanlardan bazıları görünen adı, anlam türünü, minimum değeri, en büyük değer, ondalık basamak, renk, birim, görüntü birimi, açıklama ve açıklama içerir. Şu özelleştirmelere birini eklemek için:

1. **Cihaz şablonları** sayfasına gidin.
1. Özelleştirmek istediğiniz cihaz şablonunu seçin.
1. **Özelleştir** sekmesini seçin.
1. Cihaz yetenek modelinizde tanımlanan tüm yetenekler burada listelenmiştir. Cihaz şablonunuzu sürüm gerekmeden bu alanları düzenleyebilir, kaydedebilir ve kullanabilirsiniz. Salt okunurdur düzenlemek istediğiniz alanlar varsa, bunları değiştirmek için cihaz şablonunuzu sürümüne uygulamanız gerekir. Düzenlemek istediğiniz bir alanı seçin ve yeni değerleri girin.
1. **Kaydet**’e tıklayın. Artık bu değerler, ilk olarak cihaz şablonunuzda kaydedilen ve uygulama genelinde kullanılan her şeyi geçersiz kılar.

## <a name="version-a-device-template"></a>Bir cihaz şablonu sürümü

Cihaz şablonunuzun yeni bir sürümünü oluşturmak, cihaz yetenek modelinin düzenlenebildiği şablonun taslak bir sürümünü oluşturur. Yayımlanan tüm arabirimler, tek tek sürümleyene kadar yayımlanmaya devam eder. Yayınlanmış bir arabirimi değiştirmek için önce yeni bir cihaz şablonu sürümü oluşturun.

Yalnızca, cihaz özelliği modelinin özelleştirmeler bölümünde düzenleyebileceğiniz bir parçasını düzenlemeye çalışırken cihaz şablonunu sürüm olarak değiştirin.

Bir cihaz şablonunu sürüm için:

1. **Cihaz şablonları** sayfasına gidin.
1. Sürüm oluşturmaya çalıştığınız cihaz şablonunu seçin.
1. Sayfanın üst kısmındaki **Sürüm** düğmesine tıklayın ve şablona yeni bir ad verin. IoT Central, düzenleyebilmeniz için yeni bir ad önerir.
1. **Oluştur**’a tıklayın.
1. Artık cihaz şablonunuz taslak modunda. Arabirimlerinizin hala kilitli olduğunu görebilirsiniz. Değiştirmek istediğiniz arabirimlerin herhangi bir sürümünü kullanabilirsiniz.

## <a name="version-an-interface"></a>Bir arabirim sürümü

Bir arabirimin sürümü oluşturma, önceden oluşturduğunuz arabirim içindeki özellikleri eklemenize, güncelleştirmenize ve kaldırmanıza olanak sağlar.

Bir arabirimi sürüm için:

1. **Cihaz şablonları** sayfasına gidin.
1. Taslak modunda olan cihaz şablonunu seçin.
1. Sürüm ve düzenleme yapmak istediğiniz yayımlanmış modda olan arabirimi seçin.
1. Arabirim sayfasının en üstündeki **Sürüm** düğmesine tıklayın.
1. **Oluştur**’a tıklayın.
1. Artık arabiriminiz taslak modunda. Mevcut özelleştirmeleri ve görünümleri bozmadan arabiriminize özellikler ekleyebilir veya düzenleyebilirsiniz.

## <a name="migrate-a-device-across-versions"></a>Bir cihazı sürümler arasında geçirme

Cihaz şablonunun birden çok sürümünü oluşturabilirsiniz. Zaman içinde, bu cihaz şablonlarını kullanan birden fazla bağlı cihazınız olacaktır. Cihazları, cihaz şablonunuzun bir sürümünden başka bir sürümüne geçirebilirsiniz. Aşağıdaki adımlarda bir cihazın nasıl geçirileceği açıklanır:

1. **Device Explorer** sayfasına gidin.
1. Başka bir sürüme geçirmeniz gereken cihazı seçin.
1. **Geçir**' i seçin.
1. Cihazı geçirmek istediğiniz sürüm numarasına sahip cihaz şablonunu seçin ve **geçir**' i seçin.

![Cihaz geçirme](media/howto-version-device-template/pick-version.png)

## <a name="next-steps"></a>Sonraki adımlar

Operatör veya çözüm Oluşturucu kullanıyorsanız, [cihazlarınızın nasıl yönetileceğini](./howto-manage-devices.md)öğrenmek için bir sonraki adım önerilir.

Bir cihaz geliştiricisiyseniz, önerilen bir sonraki adım [Azure IoT Edge cihazları ve Azure IoT Central](./concepts-iot-edge.md)hakkında bilgi almanız gerekir.
