---
title: Hızlı başlangıç-ilk Azure Kinect uygulamanızı oluşturma
description: Bu hızlı başlangıçta, yeni bir uygulama oluşturma işlemi boyunca Azure ınect DK kullanıcısına kılavuzluk eder.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: quickstart
keywords: Kinect, Azure, algılayıcı, SDK, mikrofon, erişim mik, MIC verileri
ms.openlocfilehash: 3632145b3f3b63023e0c66e3cf99903231802edf
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "85277998"
---
# <a name="quickstart-build-your-first-azure-kinect-application"></a>Hızlı başlangıç: ilk Azure Kinect uygulamanızı oluşturma

Azure Kinect DK ile çalışmaya başlama Bu hızlı başlangıç, cihaz ile çalışmaya başlamanızı sağlayacak!

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Aşağıdaki işlevler ele alınmıştır:

- [`k4a_device_get_installed_count()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf7d19df0f73f8e4dfaa21e1b4b719ecc.html#gaf7d19df0f73f8e4dfaa21e1b4b719ecc)
- [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113)
- [`k4a_device_get_serialnum()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga798489af207ff1c99f2285ff6b08bc22.html#ga798489af207ff1c99f2285ff6b08bc22)
- [`k4a_device_start_cameras()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaad7a85e1e5471810262442fc4a8e217a.html#gaad7a85e1e5471810262442fc4a8e217a)
- [`k4a_device_stop_cameras()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4fa0e0a011a7105309ad97f081a5d6b8.html#ga4fa0e0a011a7105309ad97f081a5d6b8)
- [`k4a_device_close()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7a3931d9a690b3971caaac83b43f9423.html#ga7a3931d9a690b3971caaac83b43f9423)

## <a name="prerequisites"></a>Ön koşullar

1. [Azure Kinect dk cihazını ayarlayın](set-up-azure-kinect-dk.md).
2. Azure Kinect algılayıcı SDK 'sını [indirip](sensor-sdk-download.md) yükleyin.

## <a name="headers"></a>Üst Bilgiler

İhtiyacınız olacak yalnızca bir üst bilgi vardır ve bu `k4a.h` . Seçtiğiniz derleyicinin, SDK 'nın LIB ve Include klasörlerinde ayarlandığından emin olun. Ayrıca, `k4a.lib` ve `k4a.dll` dosyalarının bağlı olması gerekir. [Projenize Azure Kinect kitaplığı eklemek](add-library-to-project.md)için başvurmak isteyebilirsiniz.

```C
#include <k4a/k4a.h>
```

## <a name="finding-an-azure-kinect-dk-device"></a>Azure Kinect DK cihazı bulma

Bilgisayarınıza birden çok Azure Kinect DK cihazı bağlı olabilir. İlk olarak, işlevini kullanarak kaç tane, ne kadar bağlı olduğunu öğrenerek başlayacağız [`k4a_device_get_installed_count()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf7d19df0f73f8e4dfaa21e1b4b719ecc.html#gaf7d19df0f73f8e4dfaa21e1b4b719ecc) . Bu işlev, ek bir kurulum yapılmadan hemen çalışır.

```C
uint32_t count = k4a_device_get_installed_count();
```

Bilgisayara bağlı bir cihaz olduğunu belirledikten sonra, kullanarak açabilirsiniz [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113) . Açmak istediğiniz cihazın dizinini sağlayabilir veya yalnızca `K4A_DEVICE_DEFAULT` ilk bir tane için kullanabilirsiniz.

```C
// Open the first plugged in Kinect device
k4a_device_t device = NULL;
k4a_device_open(K4A_DEVICE_DEFAULT, &device);
```
Azure Kinect kitaplığı 'ndaki birçok konuda olduğu gibi, bir şeyi açtığınızda, ile işiniz bittiğinde de kapatmanız gerekir! Kapatılırken, için bir çağrı yapmayı unutmayın [`k4a_device_close()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7a3931d9a690b3971caaac83b43f9423.html#ga7a3931d9a690b3971caaac83b43f9423) .

```C
k4a_device_close(device);
```

Cihaz açıldıktan sonra çalıştığından emin olmak için bir test yapabiliriz. Şimdi cihazın seri numarasını okuyalim!

```C
// Get the size of the serial number
size_t serial_size = 0;
k4a_device_get_serialnum(device, NULL, &serial_size);

// Allocate memory for the serial, then acquire it
char *serial = (char*)(malloc(serial_size));
k4a_device_get_serialnum(device, serial, &serial_size);
printf("Opened device: %s\n", serial);
free(serial);
```

## <a name="starting-the-cameras"></a>Kameraları başlatma

Cihazı açtıktan sonra kamerayı bir nesne ile yapılandırmanız gerekir [`k4a_device_configuration_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__device__configuration__t.html) . Kamera yapılandırmasında çeşitli seçenekler vardır. Kendi senaryonuza en uygun ayarları seçin.

```C
// Configure a stream of 4096x3072 BRGA color data at 15 frames per second
k4a_device_configuration_t config = K4A_DEVICE_CONFIG_INIT_DISABLE_ALL;
config.camera_fps       = K4A_FRAMES_PER_SECOND_15;
config.color_format     = K4A_IMAGE_FORMAT_COLOR_BGRA32;
config.color_resolution = K4A_COLOR_RESOLUTION_3072P;

// Start the camera with the given configuration
k4a_device_start_cameras(device, &config);

// ...Camera capture and application specific code would go here...

// Shut down the camera when finished with application logic
k4a_device_stop_cameras(device);
```

## <a name="error-handling"></a>Hata işleme

Kısaltma ve açıklık açısından, bazı satır içi örneklerde hata işlemeyi göstermezsiniz. Ancak hata işleme her zaman önemlidir! Birçok işlev genel bir başarı/başarısızlık türü veya gibi [`k4a_result_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___enumerations_ga4b419a99aa2220b076a4520dc2afd1e5.html#ga4b419a99aa2220b076a4520dc2afd1e5) ayrıntılı bilgiler içeren daha belirli bir varyant döndürür [`k4a_wait_result_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___enumerations_ga44c7c0c1cfba7c879e9e2da1a869e4ee.html#ga44c7c0c1cfba7c879e9e2da1a869e4ee) . Görmeyi beklediğiniz hata iletilerini görmek için her bir işlev için docs veya IntelliSense 'i denetleyin!

[`K4A_SUCCEEDED`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___macros_ga8e5b48150bc243c6052793bd830c2fcd.html#ga8e5b48150bc243c6052793bd830c2fcd) [`K4A_FAILED`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___macros_ga7c2e32349135d008b6f836c571d434b4.html#ga7c2e32349135d008b6f836c571d434b4) Bir işlevin sonucunu denetlemek için ve makrolarını kullanabilirsiniz. Bu nedenle, yalnızca bir Azure Kinect DK cihazı açmak yerine işlev çağrısını şu şekilde korsunacağız:

```C
// Open the first plugged in Kinect device
k4a_device_t device = NULL;
if ( K4A_FAILED( k4a_device_open(K4A_DEVICE_DEFAULT, &device) ) )
{
    printf("Failed to open k4a device!\n");
    return;
}
```

## <a name="full-source"></a>Tam kaynak

```C
#pragma comment(lib, "k4a.lib")
#include <k4a/k4a.h>

#include <stdio.h>
#include <stdlib.h>

int main()
{
    uint32_t count = k4a_device_get_installed_count();
    if (count == 0)
    {
        printf("No k4a devices attached!\n");
        return 1;
    }

    // Open the first plugged in Kinect device
    k4a_device_t device = NULL;
    if (K4A_FAILED(k4a_device_open(K4A_DEVICE_DEFAULT, &device)))
    {
        printf("Failed to open k4a device!\n");
        return 1;
    }

    // Get the size of the serial number
    size_t serial_size = 0;
    k4a_device_get_serialnum(device, NULL, &serial_size);

    // Allocate memory for the serial, then acquire it
    char *serial = (char*)(malloc(serial_size));
    k4a_device_get_serialnum(device, serial, &serial_size);
    printf("Opened device: %s\n", serial);
    free(serial);

    // Configure a stream of 4096x3072 BRGA color data at 15 frames per second
    k4a_device_configuration_t config = K4A_DEVICE_CONFIG_INIT_DISABLE_ALL;
    config.camera_fps       = K4A_FRAMES_PER_SECOND_15;
    config.color_format     = K4A_IMAGE_FORMAT_COLOR_BGRA32;
    config.color_resolution = K4A_COLOR_RESOLUTION_3072P;

    // Start the camera with the given configuration
    if (K4A_FAILED(k4a_device_start_cameras(device, &config)))
    {
        printf("Failed to start cameras!\n");
        k4a_device_close(device);
        return 1;
    }

    // Camera capture and application specific code would go here

    // Shut down the camera when finished with application logic
    k4a_device_stop_cameras(device);
    k4a_device_close(device);

    return 0;
}

```

## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı SDK kullanarak bir Azure Kinect DK cihazını bulmayı ve açmayı öğrenin
> [!div class="nextstepaction"]
>[Bir cihaz bulun ve açın](find-then-open-device.md)
