---
title: IoT Hub C SDK kullanan Kısıtlı Aygıtlar için Azure IoT Hub'ı Geliştir
description: Geliştirici kılavuzu - kısıtlı aygıtlar için Azure IoT SDK'larını kullanmanın nasıl geliştirilenene ilişkin kılavuzu.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: robinsh
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 9010ff582f05e81e17e280e20f180ceccf0e746f
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733196"
---
# <a name="develop-for-constrained-devices-using-azure-iot-c-sdk"></a>Azure IoT C SDK kullanan kısıtlı aygıtlar için geliştirme

Azure IoT Hub C SDK, ANSI C (C99) ile yazılmıştır ve bu da küçük disk ve bellek ayak izine sahip çeşitli platformları çalıştırmak için uygundur. Önerilen RAM en az 64 KB, ancak tam bellek ayak izi kullanılan protokole bağlıdır, açılan bağlantı sayısı, hem de hedeflenen platform.
> [!NOTE]
> * Azure IoT C SDK, geliştirmeye yardımcı olmak için düzenli olarak kaynak tüketim bilgilerini yayınlar.  Lütfen [GitHub depomuzu](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/c_sdk_resource_information.md) ziyaret edin ve en son kıyaslama yı gözden geçirin.
>

C SDK, apt-get, NuGet ve MBED'den paket halinde mevcuttur. Kısıtlı aygıtları hedeflemek için, Hedef platformunuz için SDK'yı yerel olarak oluşturmak isteyebilirsiniz. Bu dokümantasyon [cmake](https://cmake.org/)kullanarak C SDK ayak izini küçültmek için bazı özellikleri kaldırmak için nasıl gösterir. Ayrıca, bu dokümantasyon, kısıtlı aygıtlarla çalışmak için en iyi uygulama programlama modellerini tartışır.

## <a name="building-the-c-sdk-for-constrained-devices"></a>Kısıtlı cihazlar için C SDK'nın oluşturulması

Kısıtlı aygıtlar için C SDK'yı oluşturun.

### <a name="prerequisites"></a>Ön koşullar

C SDK'yı oluşturmak için geliştirme ortamınızı hazırlamak için bu [C SDK kurulum kılavuzunu](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) izleyin. Cmake ile oluşturma adımını almadan önce, kullanılmayan özellikleri kaldırmak için cmake bayraklarını çağırabilirsiniz.

### <a name="remove-additional-protocol-libraries"></a>Ek iletişim kuralı kitaplıklarını kaldırma

C SDK bugün beş protokolü destekler: MQTT, WebSocket üzerinden MQTT, AMQPs, WebSocket üzerinden AMQP ve HTTPS. Çoğu senaryo istemciüzerinde çalışan bir veya iki protokol gerektirir, bu nedenle kullanmadığınız iletişim kuralı kitaplığını SDK'dan kaldırabilirsiniz. Senaryonuz için uygun iletişim protokolünü seçme yle ilgili ek bilgileri [Bir IoT Hub iletişim protokolü seçin'de](iot-hub-devguide-protocols.md)bulabilirsiniz. Örneğin, MQTT genellikle kısıtlı aygıtlar için daha uygun olan hafif bir protokoldür.

Aşağıdaki cmake komutunu kullanarak AMQP ve HTTP kitaplıklarını kaldırabilirsiniz:

```
cmake -Duse_amqp=OFF -Duse_http=OFF <Path_to_cmake>
```

### <a name="remove-sdk-logging-capability"></a>SDK günlüğe kaydetme özelliğini kaldırma

C SDK hata ayıklama ile yardımcı olmak için boyunca kapsamlı günlük sağlar. Üretim aygıtları için günlük özelliğini aşağıdaki cmake komutunu kullanarak kaldırabilirsiniz:

```
cmake -Dno_logging=OFF <Path_to_cmake>
```

### <a name="remove-upload-to-blob-capability"></a>Blob özelliğine yüklemeyi kaldırma

SDK'daki yerleşik özelliği kullanarak azure depolamaya büyük dosyalar yükleyebilirsiniz. Azure IoT Hub, ilişkili bir Azure Depolama hesabına gönderici görevi görür. Bu özelliği, ortam dosyalarını, büyük telemetri toplu işlerini ve günlükleri göndermek için kullanabilirsiniz. [IoT Hub ile dosya yükleme](iot-hub-devguide-file-upload.md)konusunda daha fazla bilgi alabilirsiniz. Uygulamanız bu işlevselliği gerektirmiyorsa, aşağıdaki cmake komutunu kullanarak bu özelliği kaldırabilirsiniz:

```
cmake -Ddont_use_uploadtoblob=ON <Path_to_cmake>
```

### <a name="running-strip-on-linux-environment"></a>Linux ortamında şerit çalıştırma

İkilileriniz Linux sisteminde çalıştırılırsa, derledikten sonra son uygulamanın boyutunu küçültmek için [şerit komutundan](https://en.wikipedia.org/wiki/Strip_(Unix)) yararlanabilirsiniz.

```
strip -s <Path_to_executable>
```

## <a name="programming-models-for-constrained-devices"></a>Kısıtlı aygıtlar için programlama modelleri

Ardından, kısıtlı aygıtlar için programlama modellerine bakın.

### <a name="avoid-using-the-serializer"></a>Serializer kullanmaktan kaçının

C SDK, yöntemleri ve aygıt ikiz özelliklerini tanımlamak için bildirimsel eşleme tabloları kullanmanıza olanak tanıyan isteğe bağlı bir [C SDK serializer'a](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer)sahiptir. Serializer geliştirmeyi kolaylaştırmak için tasarlanmıştır, ancak kısıtlı aygıtlar için en uygun olmayan ek yükü ekler. Bu durumda, ilkel istemci API'leri kullanmayı düşünün ve [parson](https://github.com/kgabis/parson)gibi hafif bir ayrıştırıcı kullanarak JSON ayrıştırıcı.

### <a name="use-the-lower-layer-_ll_"></a>Alt katmanı kullanın (_LL_)

C SDK iki programlama modelini destekler. Bir set, alt katman anlamına gelen _LL_ ekine sahip API'ler vardır. Bu API kümesi daha hafiftir ve alt iş parçacıklarını döndürmez, bu da kullanıcının zamanlamayı el ile denetlemesi gerektiği anlamına gelir. Örneğin, aygıt istemcisi için _LL_ API'leri bu [üstbilgi dosyasında](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client_ll.h)bulunabilir. 

_LL_ dizini olmayan başka bir API kümesi, bir alt iş parçacığının otomatik olarak döndürüldüğü kolaylık katmanı olarak adlandırılır. Örneğin, aygıt istemcisi için kolaylık katmanı API'leri bu [IoT Aygıt İstemci üstbilgi dosyasında](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client.h)bulunabilir. Her ek iş parçacığının sistem kaynaklarının önemli bir yüzdesini alabildiği kısıtlı aygıtlar için _LL_ API'leri kullanmayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT C SDK mimarisi hakkında daha fazla bilgi edinmek için:
-    [Azure IoT C SDK kaynak kodu](https://github.com/Azure/azure-iot-sdk-c/)
-    [C giriş için Azure IoT cihazı SDK](iot-hub-device-sdk-c-intro.md)
