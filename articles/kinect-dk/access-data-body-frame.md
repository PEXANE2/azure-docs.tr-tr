---
title: Bir gövde çerçevesindeki Azure Kinect DK verilerine erişme
description: Bir gövde çerçevesindeki veriler hakkında bilgi edinin ve bu verilere Azure Kinect DK 'de erişmek için işlevleri öğrenin.
author: qm13
ms.author: quentinm
ms.reviewer: yijwan
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: how-to
keywords: gövde, çerçeve, Azure, Kinect, gövde, izleme, ipuçları
ms.openlocfilehash: be44f59cb84e99129bf526575293eee69ca64598
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "85277716"
---
# <a name="access-data-in-body-frame"></a>Gövde çerçevesindeki verilere erişme

Bu makalede, bir gövde çerçevesinde bulunan veriler ve bu verilere erişmek için işlevler açıklanmaktadır.

Aşağıdaki işlevler ele alınmıştır:

- [k4abt_frame_get_body_id ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga1d612404d133a279af847974e9359a92.html#ga1d612404d133a279af847974e9359a92)
- [k4abt_frame_get_body_index_map ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga0e4f2d0d7e330d444de7070fb1fee4f6.html#ga0e4f2d0d7e330d444de7070fb1fee4f6)
- [k4abt_frame_get_body_skeleton ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gac7032ab06268253538556750775064fb.html#gac7032ab06268253538556750775064fb)
- [k4abt_frame_get_capture ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gad9eef11f6496bbfe997536c374217d9a.html#gad9eef11f6496bbfe997536c374217d9a)
- [k4abt_frame_get_num_bodies ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga29ab088b1a0d1a246bdb5542e21aa3c3.html#ga29ab088b1a0d1a246bdb5542e21aa3c3)
- [k4abt_frame_get_device_timestamp_usec ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga04be7b814b40296cd6b97044ed7283e4.html#ga04be7b814b40296cd6b97044ed7283e4)

## <a name="key-components-of-a-body-frame"></a>Gövde çerçevesinin anahtar bileşenleri

Her gövde çerçevesi, bir gövde yapıları, bir 2B gövde dizin Haritası ve bu sonucu oluşturan giriş yakalama bir koleksiyonunu içerir.

![Gövde çerçevesi bileşenleri](./media/how-to-guides/body-frame.png)

## <a name="access-the-collection-of-body-structs"></a>Gövde yapıları koleksiyonuna erişin

Tek bir yakalama içinde birden çok gövde algılanabilir. [K4abt_frame_get_num_bodies ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga29ab088b1a0d1a246bdb5542e21aa3c3.html#ga29ab088b1a0d1a246bdb5542e21aa3c3) işlevini çağırarak gövdeler sayısını sorgulayabilirsiniz.

```C
size_t num_bodies = k4abt_frame_get_num_bodies(body_frame);
```

Gövde KIMLIĞI ve Birleşik konum/yönlendirme bilgilerini bulmak için her bir gövde dizininde yinelemek üzere [k4abt_frame_get_body_id ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga1d612404d133a279af847974e9359a92.html#ga1d612404d133a279af847974e9359a92) ve [k4abt_frame_get_body_skeleton ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gac7032ab06268253538556750775064fb.html#gac7032ab06268253538556750775064fb) işlevlerini kullanırsınız.

```C
for (size_t i = 0; i < num_bodies; i++)
{
    k4abt_skeleton_t skeleton;
    k4abt_frame_get_body_skeleton(body_frame, i, &skeleton);
    uint32_t id = k4abt_frame_get_body_id(body_frame, i);
}
```

## <a name="access-the-body-index-map"></a>Gövde dizin eşlemesine erişin

Body Dizin eşlemesine erişmek için [k4abt_frame_get_body_index_map ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga0e4f2d0d7e330d444de7070fb1fee4f6.html#ga0e4f2d0d7e330d444de7070fb1fee4f6) işlevini kullanın. Gövde dizini eşlemesinin ayrıntılı açıklaması için [gövde dizini haritasına](body-index-map.md) bakın. Artık gerekli olmadığında gövde dizin eşlemesini serbest bıraktığınızdan emin olun.

```C
k4a_image_t body_index_map = k4abt_frame_get_body_index_map(body_frame);
...  // Do your work with the body index map
k4a_image_release(body_index_map);
```

## <a name="access-the-input-capture"></a>Giriş yakalamaya erişin

Gövde İzleyicisi zaman uyumsuz bir API 'dir. Özgün yakalama, sonuç ortaya çıkan zaman tarafından zaten yayınlanmış olabilir. Bu gövde izleme sonucunu oluşturmak için kullanılan giriş yakalamayı sorgulamak için [k4abt_frame_get_capture ()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gad9eef11f6496bbfe997536c374217d9a.html#gad9eef11f6496bbfe997536c374217d9a) işlevini kullanın. Bu işlevin her çağrılışında k4a_capture_t için başvuru sayısı artar. Yakalamaya artık gerek kalmadığında [k4a_capture_release ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga0ed6f74ec403c3eac1b8ef3afb32cee6.html#ga0ed6f74ec403c3eac1b8ef3afb32cee6) işlevini kullanın.

```C
k4a_capture_t input_capture = k4abt_frame_get_capture(body_frame);
... // Do your work with the input capture
k4a_capture_release(input_capture);
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Azure Kinect gövde Izleme SDK 'Sı](https://microsoft.github.io/Azure-Kinect-Body-Tracking/)
