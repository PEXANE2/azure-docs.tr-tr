---
title: 'Hızlı başlangıç: Azure Kinect gövde izleme uygulaması oluşturma'
description: İlk Azure Kinect gövde izleme uygulamanızı oluşturmak için adım adım yönergeler
author: qm13
ms.author: quentinm
ms.reviewer: yijwan
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: quickstart
keywords: Kinect, Azure, algılayıcı, SDK, gövde, izleme, birleşme, uygulama, ilk
ms.openlocfilehash: bdf8ee7a14bf59a151dfa316b11159830b4f63b8
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "85278002"
---
# <a name="quickstart-build-an-azure-kinect-body-tracking-application"></a>Hızlı başlangıç: Azure Kinect gövde izleme uygulaması oluşturma

Gövde Izleme SDK 'Sı ile çalışmaya başlama Bu hızlı başlangıç, gövde izlemeyle çalışmaya başlamanızı sağlayacak! Bu [Azure-Kinect-Sample](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples)deposunda daha fazla örnek bulabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- [Azure Kinect DK 'yi ayarlama](set-up-azure-kinect-dk.md)
- [Gövde Izleme SDK 'sını ayarlama](body-sdk-setup.md)
- [Ilk Azure Kinect uygulamanızı](build-first-app.md) hızlı başlangıç yapın.
- Aşağıdaki algılayıcı SDK işlevleri hakkında bilgi edinin:
  - [k4a_device_open ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113)
  - [k4a_device_start_cameras ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaad7a85e1e5471810262442fc4a8e217a.html#gaad7a85e1e5471810262442fc4a8e217a)
  - [k4a_device_stop_cameras ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4fa0e0a011a7105309ad97f081a5d6b8.html#ga4fa0e0a011a7105309ad97f081a5d6b8)
  - [k4a_device_close ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7a3931d9a690b3971caaac83b43f9423.html#ga7a3931d9a690b3971caaac83b43f9423)
- Aşağıdaki gövde Izleme SDK işlevleri hakkındaki belgeleri gözden geçirin:
  - [k4abt_tracker_create ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga1aa71481b8441def94de11b29e0e3cbc.html#ga1aa71481b8441def94de11b29e0e3cbc)
  - [k4abt_tracker_enqueue_capture ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga093becd9bb4a63f5f4d56f58097a7b1e.html#ga093becd9bb4a63f5f4d56f58097a7b1e)
  - [k4abt_tracker_pop_result ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gaaf446fb1579cbbe0b6af824ee0a7458b.html#gaaf446fb1579cbbe0b6af824ee0a7458b)
  - [k4abt_tracker_shutdown ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga94036969ef94cbc414c78b3f6d04bfa5.html#ga94036969ef94cbc414c78b3f6d04bfa5)
  - [k4abt_tracker_destroy ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gab2c9afca092130976cd66077c0557ed1.html#gab2c9afca092130976cd66077c0557ed1)

## <a name="headers"></a>Üst Bilgiler

Gövde izleme tek bir üst bilgi kullanır, `k4abt.h` . Buna ek olarak bu üstbilgiyi dahil edin `k4a.h` . Seçim derleyicisinin hem algılayıcı SDK hem de gövde Izleme SDK 'Sı ve klasörleri için ayarlandığından emin olun `lib` `include` . Ayrıca, ve dosyalarına bağlantı oluşturmanız `k4a.lib` gerekir `k4abt.lib` . Uygulamanın çalıştırılması,, `k4a.dll` `k4abt.dll` `onnxruntime.dll` ve `dnn_model.onnx` uygulamalarının yürütme yolunda olması gerekir.

```C
#include <k4a/k4a.h>
#include <k4abt.h>
```

## <a name="open-device-and-start-the-camera"></a>Cihazı açın ve kamerayı başlatın

İlk gövde izleme uygulamanız, BILGISAYARA bağlı tek bir Azure ınect cihazı olduğunu varsayar.

Gövde izleme, algılayıcı SDK 'sında oluşturulur. Gövde izlemeyi kullanmak için önce cihazı açmanız ve yapılandırmanız gerekir. Cihazı açmak için [k4a_device_open ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113) işlevini kullanın ve ardından bunu bir [k4a_device_configuration_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__device__configuration__t.html) nesnesiyle yapılandırın. En iyi sonuçlar için derinlik modunu veya olarak `K4A_DEPTH_MODE_NFOV_UNBINNED` ayarlayın `K4A_DEPTH_MODE_WFOV_2X2BINNED` . Derinlik modu veya olarak ayarlandıysa gövde İzleyicisi çalışmaz `K4A_DEPTH_MODE_OFF` `K4A_DEPTH_MODE_PASSIVE_IR` .

[Bu sayfada](find-then-open-device.md)cihazı bulma ve açma hakkında daha fazla bilgi edinebilirsiniz.

Bu sayfalarda Azure Kinect derinlik modları hakkında daha fazla bilgi edinebilirsiniz: [donanım belirtimi](hardware-specification.md) ve [k4a_depth_mode_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___enumerations_ga3507ee60c1ffe1909096e2080dd2a05d.html#ga3507ee60c1ffe1909096e2080dd2a05d) Numaralandırmalar.

```C
k4a_device_t device = NULL;
k4a_device_open(0, &device);

// Start camera. Make sure depth camera is enabled.
k4a_device_configuration_t deviceConfig = K4A_DEVICE_CONFIG_INIT_DISABLE_ALL;
deviceConfig.depth_mode = K4A_DEPTH_MODE_NFOV_UNBINNED;
deviceConfig.color_resolution = K4A_COLOR_RESOLUTION_OFF;
k4a_device_start_cameras(device, &deviceConfig);
```

## <a name="create-the-tracker"></a>İzleyici oluşturma

Gövde izleme sonuçlarını almak için ilk adım bir gövde izleyici oluşturmaktır. Bu, algılayıcı ayarlaması [k4a_calibration_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__calibration__t.html) yapısına ihtiyaç duyuyor. Algılayıcı ayarı [k4a_device_get_calibration ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4e43940d8d8db48da266c7a7842c8d78.html#ga4e43940d8d8db48da266c7a7842c8d78) işlevi kullanılarak sorgulanabilir.

```C
k4a_calibration_t sensor_calibration;
k4a_device_get_calibration(device, deviceConfig.depth_mode, deviceConfig.color_resolution, &sensor_calibration);

k4abt_tracker_t tracker = NULL;
k4abt_tracker_configuration_t tracker_config = K4ABT_TRACKER_CONFIG_DEFAULT;
k4abt_tracker_create(&sensor_calibration, tracker_config, &tracker);
```

## <a name="get-captures-from-the-azure-kinect-device"></a>Azure Kinect cihazından yakalanan yakalamaları al

Görüntü verilerini [Bu sayfada](retrieve-images.md)alma hakkında daha fazla bilgi edinebilirsiniz.

```C
// Capture a depth frame
k4a_device_get_capture(device, &capture, TIMEOUT_IN_MS);
```

## <a name="enqueue-the-capture-and-pop-the-results"></a>Yakalamayı sıraya al ve sonuçları aç

İzleyici, bir giriş kuyruğunu ve bir çıkış sırasını zaman uyumsuz olarak işlemek için Azure Kinect DK 'in daha verimli bir şekilde yakalamalarını sağlar. Sonraki adım, `k4abt_tracker_enqueue_capture()` giriş kuyruğuna yeni bir yakalama eklemek için işlevini kullanmaktır. `k4abt_tracker_pop_result()`Çıkış sırasından bir sonuç eklemek için işlevini kullanın. Zaman aşımı değeri uygulamaya bağımlıdır ve sıraya alma bekleme süresini denetler.

İlk gövde izleme uygulamanız gerçek zamanlı işleme modelini kullanır. Diğer desenlerin ayrıntılı açıklaması için [gövde izleme sonuçlarını alma](get-body-tracking-results.md) bölümüne bakın.

```C
k4a_wait_result_t queue_capture_result = k4abt_tracker_enqueue_capture(tracker, sensor_capture, K4A_WAIT_INFINITE);
k4a_capture_release(sensor_capture); // Remember to release the sensor capture once you finish using it
if (queue_capture_result == K4A_WAIT_RESULT_FAILED)
{
    printf("Error! Adding capture to tracker process queue failed!\n");
    break;
}

k4abt_frame_t body_frame = NULL;
k4a_wait_result_t pop_frame_result = k4abt_tracker_pop_result(tracker, &body_frame, K4A_WAIT_INFINITE);
if (pop_frame_result == K4A_WAIT_RESULT_SUCCEEDED)
{
    // Successfully popped the body tracking result. Start your processing
    ...

    k4abt_frame_release(body_frame); // Remember to release the body frame once you finish using it
}
```

## <a name="access-the-body-tracking-result-data"></a>Gövde izleme sonuç verilerine erişin

Her bir algılayıcı yakalamanın gövde izleme sonuçları, bir gövde çerçevesinin [k4abt_frame_t](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/structk4abt__frame__t.html) yapısında depolanır. Her gövde çerçevesi üç anahtar bileşeni içerir: gövde yapıları, 2B gövde dizini Haritası ve giriş yakalama.

İlk gövde izleme uygulamanız yalnızca algılanan gövdelerin sayısına erişir. Gövde çerçevesindeki verilerin ayrıntılı açıklaması için [gövde çerçevesindeki verilere erişim](access-data-body-frame.md) bölümüne bakın.

```C
size_t num_bodies = k4abt_frame_get_num_bodies(body_frame);
printf("%zu bodies are detected!\n", num_bodies);
```

## <a name="clean-up"></a>Temizleme

Son adım, gövde izleyicisini kapatmakta ve gövde izleme nesnesini yayınlamadır. Ayrıca, cihazı durdurup kapatmanız gerekir.

```C
k4abt_tracker_shutdown(tracker);
k4abt_tracker_destroy(tracker);
k4a_device_stop_cameras(device);
k4a_device_close(device);
```

## <a name="full-source"></a>Tam kaynak

```C
#include <stdio.h>
#include <stdlib.h>

#include <k4a/k4a.h>
#include <k4abt.h>

#define VERIFY(result, error)                                                                            \
    if(result != K4A_RESULT_SUCCEEDED)                                                                   \
    {                                                                                                    \
        printf("%s \n - (File: %s, Function: %s, Line: %d)\n", error, __FILE__, __FUNCTION__, __LINE__); \
        exit(1);                                                                                         \
    }                                                                                                    \

int main()
{
    k4a_device_t device = NULL;
    VERIFY(k4a_device_open(0, &device), "Open K4A Device failed!");

    // Start camera. Make sure depth camera is enabled.
    k4a_device_configuration_t deviceConfig = K4A_DEVICE_CONFIG_INIT_DISABLE_ALL;
    deviceConfig.depth_mode = K4A_DEPTH_MODE_NFOV_UNBINNED;
    deviceConfig.color_resolution = K4A_COLOR_RESOLUTION_OFF;
    VERIFY(k4a_device_start_cameras(device, &deviceConfig), "Start K4A cameras failed!");

    k4a_calibration_t sensor_calibration;
    VERIFY(k4a_device_get_calibration(device, deviceConfig.depth_mode, deviceConfig.color_resolution, &sensor_calibration),
        "Get depth camera calibration failed!");

    k4abt_tracker_t tracker = NULL;
    k4abt_tracker_configuration_t tracker_config = K4ABT_TRACKER_CONFIG_DEFAULT;
    VERIFY(k4abt_tracker_create(&sensor_calibration, tracker_config, &tracker), "Body tracker initialization failed!");

    int frame_count = 0;
    do
    {
        k4a_capture_t sensor_capture;
        k4a_wait_result_t get_capture_result = k4a_device_get_capture(device, &sensor_capture, K4A_WAIT_INFINITE);
        if (get_capture_result == K4A_WAIT_RESULT_SUCCEEDED)
        {
            frame_count++;
            k4a_wait_result_t queue_capture_result = k4abt_tracker_enqueue_capture(tracker, sensor_capture, K4A_WAIT_INFINITE);
            k4a_capture_release(sensor_capture); // Remember to release the sensor capture once you finish using it
            if (queue_capture_result == K4A_WAIT_RESULT_TIMEOUT)
            {
                // It should never hit timeout when K4A_WAIT_INFINITE is set.
                printf("Error! Add capture to tracker process queue timeout!\n");
                break;
            }
            else if (queue_capture_result == K4A_WAIT_RESULT_FAILED)
            {
                printf("Error! Add capture to tracker process queue failed!\n");
                break;
            }

            k4abt_frame_t body_frame = NULL;
            k4a_wait_result_t pop_frame_result = k4abt_tracker_pop_result(tracker, &body_frame, K4A_WAIT_INFINITE);
            if (pop_frame_result == K4A_WAIT_RESULT_SUCCEEDED)
            {
                // Successfully popped the body tracking result. Start your processing

                size_t num_bodies = k4abt_frame_get_num_bodies(body_frame);
                printf("%zu bodies are detected!\n", num_bodies);

                k4abt_frame_release(body_frame); // Remember to release the body frame once you finish using it
            }
            else if (pop_frame_result == K4A_WAIT_RESULT_TIMEOUT)
            {
                //  It should never hit timeout when K4A_WAIT_INFINITE is set.
                printf("Error! Pop body frame result timeout!\n");
                break;
            }
            else
            {
                printf("Pop body frame result failed!\n");
                break;
            }
        }
        else if (get_capture_result == K4A_WAIT_RESULT_TIMEOUT)
        {
            // It should never hit time out when K4A_WAIT_INFINITE is set.
            printf("Error! Get depth frame time out!\n");
            break;
        }
        else
        {
            printf("Get depth capture returned error: %d\n", get_capture_result);
            break;
        }

    } while (frame_count < 100);

    printf("Finished body tracking processing!\n");

    k4abt_tracker_shutdown(tracker);
    k4abt_tracker_destroy(tracker);
    k4a_device_stop_cameras(device);
    k4a_device_close(device);

    return 0;
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Gövde izleme sonuçlarını al](get-body-tracking-results.md)
