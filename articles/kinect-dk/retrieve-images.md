---
title: Azure Kinect görüntü verilerini alma
description: Kinect algılayıcı SDK 'sını kullanarak Azure Kinect görüntü verilerini almayı öğrenin.
author: cdedmonds
ms.author: cedmonds
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, alma, algılayıcı, kamera, SDK, derinlik, RGB, görüntüler, renk, yakalama, çözümleme, arabellek
ms.openlocfilehash: fed5c7340d287d9103ba35f0fd3d80c0fff6e3ec
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87538923"
---
# <a name="retrieve-azure-kinect-image-data"></a>Azure Kinect görüntü verilerini alma

Bu sayfa, Azure Kinect 'tan görüntülerin nasıl alınacağını gösteren ayrıntıları sağlar. Makalede, cihazın rengi ve derinliği arasında koordine edilen görüntülerin nasıl yakalanacağı ve erişebileceği gösterilmektedir. Görüntülere erişmek için önce cihazı açmanız ve yapılandırmanız gerekir, ardından görüntüleri yakalayabilirsiniz.
Bir görüntüyü yapılandırmadan ve yakalamadan önce [cihazı bulup açmanız](find-then-open-device.md)gerekir.

Ayrıca, bu makaledeki işlevlerin nasıl kullanılacağını gösteren [SDK akış örneğine](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/streaming) de başvurabilirsiniz.

Aşağıdaki işlevler ele alınmıştır:

- [`k4a_device_start_cameras()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaad7a85e1e5471810262442fc4a8e217a.html#gaad7a85e1e5471810262442fc4a8e217a)
- [`k4a_device_get_capture()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4dac757a33657f4d3dbf1ae8b21c158a.html#ga4dac757a33657f4d3dbf1ae8b21c158a)
- [`k4a_capture_get_depth_image()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafa03513da96bf6b8c254fc11a04ee6d6.html#gafa03513da96bf6b8c254fc11a04ee6d6)
- [`k4a_image_get_buffer()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga2ef070cf4e543cd0f726478af332546e.html#ga2ef070cf4e543cd0f726478af332546e)
- [`k4a_image_release()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga27c81863b13fafc3934a32935a014e9f.html#ga27c81863b13fafc3934a32935a014e9f)
- [`k4a_capture_release()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga0ed6f74ec403c3eac1b8ef3afb32cee6.html#ga0ed6f74ec403c3eac1b8ef3afb32cee6)
- [`k4a_device_stop_cameras()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4fa0e0a011a7105309ad97f081a5d6b8.html#ga4fa0e0a011a7105309ad97f081a5d6b8)
  
## <a name="configure-and-start-the-device"></a>Cihazı yapılandırma ve başlatma

Kinect cihazınızda bulunan iki kamera birden çok modu, çözünürlüğü ve çıkış biçimini destekler. Tüm liste için Azure Kinect Geliştirme Seti [donanım belirtimlerine](hardware-specification.md)bakın.

Akış Yapılandırması, yapıdaki değerler kullanılarak ayarlanır [`k4a_device_configuration_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__device__configuration__t.html) .

```C
k4a_device_configuration_t config = K4A_DEVICE_CONFIG_INIT_DISABLE_ALL;
config.camera_fps = K4A_FRAMES_PER_SECOND_30;
config.color_format = K4A_IMAGE_FORMAT_COLOR_MJPG;
config.color_resolution = K4A_COLOR_RESOLUTION_2160P;
config.depth_mode = K4A_DEPTH_MODE_NFOV_UNBINNED;

if (K4A_RESULT_SUCCEEDED != k4a_device_start_cameras(device, &config))
{
    printf("Failed to start device\n");
    goto Exit;
}
```

Kameralar başlatıldıktan sonra, [`k4a_device_stop_cameras()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4fa0e0a011a7105309ad97f081a5d6b8.html#ga4fa0e0a011a7105309ad97f081a5d6b8) çağrılana veya cihaz kapatılana kadar verileri yakalamaya devam ederler.

## <a name="stabilization"></a>Sabitleme

Çoklu cihaz eşitleme özelliğini kullanarak cihazları başlatırken, bu, sabit bir pozlama ayarı kullanılarak yapılması önemle önerilir.
El ile Pozlandırma kümesi sayesinde, görüntülerden ve kare hızına kadar cihazdan önce sekiz yakalama yapılabilir. Otomatik pozlamaya göre, görüntülerden önce 20 ' ye kadar yakalama yapılabilir ve kare hızına kadar kararlı hale gelir.

## <a name="get-a-capture-from-the-device"></a>Cihazdan bir yakalama al

Görüntüler cihazdan bağıntılı bir şekilde yakalanır. Yakalanan her görüntü, bir derinlik görüntüsü, IR görüntüsü, bir renk görüntüsü veya görüntülerin bir birleşimini içerir.

Varsayılan olarak, API yalnızca akış modu için istenen tüm görüntüleri aldıktan sonra bir yakalama döndürür. ' In parametresi temizlenerek, API 'YI yalnızca derinlik veya renkli görüntülerle kısmi yakalamaları döndürecek şekilde yapılandırabilirsiniz [`synchronized_images_only`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__device__configuration__t_a8208974f05d89fc1362c6a0900bdef4d.html#a8208974f05d89fc1362c6a0900bdef4d) [`k4a_device_configuration_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__device__configuration__t.html) .

```C
// Capture a depth frame
switch (k4a_device_get_capture(device, &capture, TIMEOUT_IN_MS))
{
case K4A_WAIT_RESULT_SUCCEEDED:
    break;
case K4A_WAIT_RESULT_TIMEOUT:
    printf("Timed out waiting for a capture\n");
    continue;
    break;
case K4A_WAIT_RESULT_FAILED:
    printf("Failed to read a capture\n");
    goto Exit;
}
```

API başarıyla bir yakalama döndürtikten sonra, yakalama nesnesini kullanmayı tamamladığınızda öğesini çağırmanız gerekir [`k4a_capture_release()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga0ed6f74ec403c3eac1b8ef3afb32cee6.html#ga0ed6f74ec403c3eac1b8ef3afb32cee6) .

## <a name="get-an-image-from-the-capture"></a>Yakalamanın bir görüntüsünü alma

Yakalanan bir görüntüyü almak için, her bir görüntü türü için uygun işlevi çağırın. Aşağıdakilerden biri:

- [`k4a_capture_get_color_image()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga683e440b5f22215a2de58d7fa140488c.html#ga683e440b5f22215a2de58d7fa140488c)
- [`k4a_capture_get_depth_image()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafa03513da96bf6b8c254fc11a04ee6d6.html#gafa03513da96bf6b8c254fc11a04ee6d6)
- [`k4a_capture_get_ir_image()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga1531c3fa76a7c174b8f2eab24de91794.html#ga1531c3fa76a7c174b8f2eab24de91794)

[`k4a_image_release()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga27c81863b13fafc3934a32935a014e9f.html#ga27c81863b13fafc3934a32935a014e9f) [`k4a_image_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__image__t.html) Görüntüyü kullanarak işiniz bittiğinde, bu işlevler tarafından döndürülen herhangi bir tanıtıcıyı çağırmanız gerekir.

## <a name="access-image-buffers"></a>Erişim resmi arabellekleri

[`k4a_image_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__image__t.html)görüntünün özelliklerini almak için birçok erişimci işlevi vardır.

Görüntünün bellek arabelleğine erişmek için [k4a_image_get_buffer](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga2ef070cf4e543cd0f726478af332546e.html#ga2ef070cf4e543cd0f726478af332546e)kullanın.

Aşağıdaki örnekte, yakalanan bir derinlik görüntüsüne nasıl erişebileceğiniz gösterilmektedir. Aynı ilke diğer görüntü türleri için de geçerlidir. Ancak, görüntü türü değişkenini IR veya renk gibi doğru görüntü türüyle değiştirdiğinizden emin olun.

```C
// Access the depth16 image
k4a_image_t image = k4a_capture_get_depth_image(capture);
if (image != NULL)
{
    printf(" | Depth16 res:%4dx%4d stride:%5d\n",
            k4a_image_get_height_pixels(image),
            k4a_image_get_width_pixels(image),
            k4a_image_get_stride_bytes(image));

    // Release the image
    k4a_image_release(image);
}

// Release the capture
k4a_capture_release(capture);
```

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Kinect cihazınızı kullanarak, kamera görüntülerini, renk ve derinlik arasında nasıl yakalayıp koordine leyeceğinizi öğrenirsiniz. Şunları da yapabilirsiniz:

>[!div class="nextstepaction"]
>[IMU örneklerini alma](retrieve-imu-samples.md)

>[!div class="nextstepaction"]
>[Erişim mikrofonlar](access-mics.md)
