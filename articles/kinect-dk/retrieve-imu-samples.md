---
title: Azure Kinect ıMU örneklerini alma
description: Azure Kinect 'ı kullanarak Azure Kinect ıMU örneklerini almayı öğrenin.
author: qm13
ms.author: cedmonds
ms.date: 06/26/2019
ms.prod: kinect-dk
ms.topic: conceptual
keywords: Kinect, Azure, yapılandırma, derinlik, renk, RBG, kamera, algılayıcı, SDK, IMU, hareket algılayıcısı, hareket, jroscope, JRO, ivometer, FPS
ms.openlocfilehash: 649dd5b9de62d43d59e74d53adff1ec7de8dfd32
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "85277663"
---
# <a name="retrieve-azure-kinect-imu-samples"></a>Azure Kinect ıMU örneklerini alma

Azure Kinect cihazı, hem ivometer hem de cayroscope türleri dahil olmak üzere Inertial Motion birimlerine (Imus) erişim sağlar. Imus örneklerine erişmek için, önce cihazınızı açıp yapılandırmanız ve ardından ıMU verilerini yakalamanız gerekir. Daha fazla bilgi için bkz. [cihaz bulma ve açma](find-then-open-device.md).

IMU örnekleri görüntülerden çok daha yüksek bir sıklıkta oluşturulmuştur. Örnekler, örneklenden daha düşük bir hızda konağa bildirilir. Bir ıMU örneği beklenirken, birden çok örnek genellikle aynı anda kullanılabilir hale gelir.

IMU raporlama oranı hakkındaki ayrıntılar için bkz. Azure Kinect DK [donanım belirtimi](hardware-specification.md) .

## <a name="configure-and-start-cameras"></a>Kameraları yapılandırma ve başlatma

> [!NOTE]
> IMU algılayıcıları yalnızca renk ve/veya derinlik kameraları çalışırken çalışır. IMU sensörleri tek başına çalışmaz.

Kameraları başlatmak için [k4a_device_start_cameras ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaad7a85e1e5471810262442fc4a8e217a.html#gaad7a85e1e5471810262442fc4a8e217a)kullanın.

```C
k4a_device_configuration_t config = K4A_DEVICE_CONFIG_INIT_DISABLE_ALL;
config.camera_fps = K4A_FRAMES_PER_SECOND_30;
config.color_format = K4A_IMAGE_FORMAT_COLOR_MJPG;
config.color_resolution = K4A_COLOR_RESOLUTION_2160P;

if (K4A_RESULT_SUCCEEDED != k4a_device_start_cameras(device, &config))
{
    printf("Failed to start cameras\n");
    goto Exit;
}

if (K4A_RESULT_SUCCEEDED != k4a_device_start_imu(device))
{
    printf("Failed to start imu\n");
    goto Exit;
}
```

## <a name="access-imu-samples"></a>IMU örneklerine erişin

 Her [k4a_imu_sample_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__imu__sample__t.html#details) , neredeyse aynı anda yakalanan bir ivme ölçer ve jroscope okuma içerir.

IMU örneklerini görüntü yakalamalarını aldığınız aynı iş parçacığında ya da ayrı iş parçacıklarında alabilirsiniz.

IMU örneklerini kullanılabilir duruma geldiğinde almak için [`k4a_device_get_imu_sample()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga8e5913b3bb94a453c7143bbd6e399a0e.html#ga8e5913b3bb94a453c7143bbd6e399a0e) kendi iş parçacığında çağırmak isteyebilirsiniz. Ayrıca API, her görüntü yakalama döndürülmeden sonra örnekleri kontrol etmeniz için yeterli dahili sıraya sahip olur.

Bazı ıMU örneklerini iç sıraya alma nedeniyle, verileri bırakmadan aşağıdaki kalıbı kullanabilirsiniz:

1. Herhangi bir kare ücretine göre yakalama için bekleyin.
2. Yakalamayı işleyin.
3. Sıraya alınan tüm ıMU örneklerini alın.
4. Sonraki yakalama için yinelemeyi tekrarlayın.

Şu anda sıraya alınan tüm ıMU örneklerini almak için, [`k4a_device_get_imu_sample()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga8e5913b3bb94a453c7143bbd6e399a0e.html#ga8e5913b3bb94a453c7143bbd6e399a0e) `timeout_in_ms` işlev dönene kadar döngüyle 0 ile çağrı yapabilirsiniz `K4A_WAIT_RESULT_TIMEOUT` . `K4A_WAIT_RESULT_TIMEOUT`sıraya alınmış bir örnek olmadığını ve belirtilen zaman aşımından sonra hiçbirinin ulaştığını gösterir.

## <a name="usage-example"></a>Kullanım örneği

```C
k4a_imu_sample_t imu_sample;

// Capture a imu sample
switch (k4a_device_get_imu_sample(device, &imu_sample, TIMEOUT_IN_MS))
{
case K4A_WAIT_RESULT_SUCCEEDED:
    break;
case K4A_WAIT_RESULT_TIMEOUT:
    printf("Timed out waiting for a imu sample\n");
    continue;
    break;
case K4A_WAIT_RESULT_FAILED:
    printf("Failed to read a imu sample\n");
    goto Exit;
}

// Access the accelerometer readings
if (imu_sample != NULL)
{
    printf(" | Accelerometer temperature:%.2f x:%.4f y:%.4f z: %.4f\n",
            imu_sample.temperature,
            imu_sample.acc_sample.xyz.x,
            imu_sample.acc_sample.xyz.y,
            imu_sample.acc_sample.xyz.z);
}

```

## <a name="next-steps"></a>Sonraki adımlar

Artık ıMU örnekleri ile nasıl çalışabileceğinizi öğrenirsiniz, ayrıca
>[!div class="nextstepaction"]
>[Mikrofon giriş verilerine erişim](access-mics.md)
