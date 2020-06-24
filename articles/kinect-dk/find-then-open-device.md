---
title: Azure ınect cihazını bulun ve açın
description: Azure Kinect Senor SDK 'sını kullanarak Azure Kinect cihazını bulmayı ve açmayı öğrenin.
author: cedmonds
ms.author: cedmonds
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, algılayıcı, SDK, derinlik, RGB, cihaz, bul, aç
ms.openlocfilehash: 67fc93b924d5d663bb43098969c54d1975bd5895
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "85277655"
---
# <a name="find-then-open-the-azure-kinect-device"></a>Azure ınect cihazını bulun ve açın

Bu makalede, Azure Kinect DK 'nizi nasıl bulabileceğiniz açıklanır. Makalesinde, makinenize bağlı birden çok cihazın bulunduğu durumun nasıl işleneceği açıklanır.

Ayrıca, bu makaledeki işlevlerin nasıl kullanılacağını gösteren [SDK listeleme örneğine](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/enumerate) de başvurabilirsiniz.

Aşağıdaki işlevler ele alınmıştır:
 * [`k4a_device_get_installed_count()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf7d19df0f73f8e4dfaa21e1b4b719ecc.html#gaf7d19df0f73f8e4dfaa21e1b4b719ecc)
 * [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113)
 * [`k4a_device_get_serialnum()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga798489af207ff1c99f2285ff6b08bc22.html#ga798489af207ff1c99f2285ff6b08bc22)
 * [`k4a_device_close()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7a3931d9a690b3971caaac83b43f9423.html#ga7a3931d9a690b3971caaac83b43f9423)

## <a name="discover-the-number-of-connected-devices"></a>Bağlı cihazların sayısını bulma

İlk olarak, şu anda bağlı olan Azure Kinect cihazlarının sayısını kullanarak alın [`k4a_device_get_installed_count()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf7d19df0f73f8e4dfaa21e1b4b719ecc.html#gaf7d19df0f73f8e4dfaa21e1b4b719ecc) .

```C
uint32_t device_count = k4a_device_get_installed_count();

printf("Found %d connected devices:\n", device_count);
```

## <a name="open-a-device"></a>Bir cihaz açın

Bir cihaz hakkında bilgi almak ya da verileri buradan okumak için, önce kullanarak cihaza bir tanıtıcı açmanız gerekir [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113) .

```C
k4a_device_t device = NULL;

for (uint8_t deviceIndex = 0; deviceIndex < device_count; deviceIndex++)
{
    if (K4A_RESULT_SUCCEEDED != k4a_device_open(deviceIndex, &device))
    {
        printf("%d: Failed to open device\n", deviceIndex);
        continue;
    }

    ...

    k4a_device_close(device);
}
```

`index`Parametresi, birden [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113) fazla bağlı varsa hangi cihazın açılacağını gösterir. Yalnızca tek bir cihazın bağlanmasını düşünüyorsanız, `K4A_DEVICE_DEFAULT` ilk aygıtı göstermek için veya 0 bağımsız değişkenini geçirebilirsiniz.

Tanıtıcıyı kullanarak işiniz bittiğinde çağırmanız gereken her zaman bir cihaz Açarsınız [`k4a_device_close()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7a3931d9a690b3971caaac83b43f9423.html#ga7a3931d9a690b3971caaac83b43f9423) . Tanıtıcıyı kapatıncaya kadar aynı cihaza başka bir tanıtıcı açılamaz.

## <a name="identify-a-specific-device"></a>Belirli bir cihazı tanımla

Cihazların dizine göre numaralandırılacağı sıralama, cihazlar iliştirilene veya ayrılana kadar değişmez. Fiziksel bir cihazı tanımlamak için cihazın seri numarasını kullanmanız gerekir.

Cihazdan seri numarasını okumak için, [`k4a_device_get_serialnum()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga798489af207ff1c99f2285ff6b08bc22.html#ga798489af207ff1c99f2285ff6b08bc22) bir tanıtıcıyı açtıktan sonra işlevini kullanın.

Bu örnek, seri numarasını depolamak için doğru bellek miktarının nasıl ayrılacağını gösterir.

```C
char *serial_number = NULL;
size_t serial_number_length = 0;

if (K4A_BUFFER_RESULT_TOO_SMALL != k4a_device_get_serialnum(device, NULL, &serial_number_length))
{
    printf("%d: Failed to get serial number length\n", deviceIndex);
    k4a_device_close(device);
    device = NULL;
    continue;
}

serial_number = malloc(serial_number_length);
if (serial_number == NULL)
{
    printf("%d: Failed to allocate memory for serial number (%zu bytes)\n", deviceIndex, serial_number_length);
    k4a_device_close(device);
    device = NULL;
    continue;
}

if (K4A_BUFFER_RESULT_SUCCEEDED != k4a_device_get_serialnum(device, serial_number, &serial_number_length))
{
    printf("%d: Failed to get serial number\n", deviceIndex);
    free(serial_number);
    serial_number = NULL;
    k4a_device_close(device);
    device = NULL;
    continue;
}

printf("%d: Device \"%s\"\n", deviceIndex, serial_number);
```

## <a name="open-the-default-device"></a>Varsayılan cihazı aç

Çoğu uygulamada, aynı bilgisayara yalnızca tek bir Azure Kinect DK eklenmiş olur. Yalnızca tek bir beklenen cihaza bağlanmanız gerekiyorsa, [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113) `index` `K4A_DEVICE_DEFAULT` ilk cihazı açmak için ile ile arama yapabilirsiniz.

```C
k4a_device_t device = NULL;
uint32_t device_count = k4a_device_get_installed_count();

if (device_count != 1)
{
    printf("Unexpected number of devices found (%d)\n", device_count);
    goto Exit;
}

if (K4A_RESULT_SUCCEEDED != k4a_device_open(K4A_DEVICE_DEFAULT, &device))
{
    printf("Failed to open device\n");
    goto Exit;
}
```

## <a name="next-steps"></a>Sonraki adımlar

>[!div class="nextstepaction"]
>[Görüntüleri al](retrieve-images.md)

>[!div class="nextstepaction"]
>[IMU örneklerini alma](retrieve-imu-samples.md)

