---
title: Azure Kinect algılayıcı SDK 'Sı görüntü dönüşümlerini kullanma
description: Azure Kinect algılayıcı SDK 'Sı resim dönüştürme işlevlerini nasıl kullanacağınızı öğrenin.
author: cdedmonds
ms.author: cedmonds
ms.prod: kinect-dk
ms.topic: conceptual
ms.date: 06/26/2019
keywords: Kinect, Azure, algılayıcı, SDK, koordinat sistemi, ayarlama, proje, unproje, dönüşüm, RGB-d, nokta bulutu
ms.openlocfilehash: df7f2aa13c0e9c0241494e96e720b30f3ff1d8a5
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "85277650"
---
# <a name="use-azure-kinect-sensor-sdk-image-transformations"></a>Azure Kinect algılayıcı SDK 'Sı görüntü dönüşümlerini kullanma

Azure Kinect DK 'de kullanılan kamera sistemleri arasında görüntüleri kullanmak ve dönüştürmek için belirli işlevleri izleyin.

## <a name="k4a_transformation-functions"></a>k4a_transformation işlevleri

 Tüm resimler üzerinde *k4a_transformation* ön eki olan tüm işlevler çalışır. [K4a_transformation_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__transformation__t.html) , [k4a_transformation_create ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga853a1a5b6d521bbdf523a69e890c4f10.html#ga853a1a5b6d521bbdf523a69e890c4f10) ile elde edilen dönüşüm tutamacı gerektirir ve [k4a_transformation_destroy ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7d3ecaae66f26c1a89da9042b1bc6d44.html#ga7d3ecaae66f26c1a89da9042b1bc6d44)ile ayrılmaları gerekir. Ayrıca, bu konudaki üç işlevin nasıl kullanılacağını gösteren SDK [dönüştürme örneğine](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/transformation) de başvurabilirsiniz.

Aşağıdaki işlevler ele alınmıştır:

* [`k4a_transformation_depth_image_to_color_camera()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514)
* [`k4a_transformation_depth_image_to_color_camera_custom()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gac00dd00e7612a86382e3d0a130f276bb.html#gac00dd00e7612a86382e3d0a130f276bb)
* [`k4a_transformation_color_image_to_depth_camera()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf3a941f07bb0185cd7a72699a648fc29.html#gaf3a941f07bb0185cd7a72699a648fc29)
* [`k4a_transformation_depth_image_to_point_cloud()`](
https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7385eb4beb9d8892e8a88cf4feb3be70.html#ga7385eb4beb9d8892e8a88cf4feb3be70)

### <a name="k4a_transformation_depth_image_to_color_camera"></a>k4a_transformation_depth_image_to_color_camera

#### <a name="overview"></a>Genel Bakış

 [K4a_transformation_depth_image_to_color_camera ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514) işlevi, derinlik kamerayı, renk kamerasının görüş açısından olan derinlik eşlemini dönüştürür. Bu işlev, RGB-D görüntüleri oluşturmak için tasarlanmıştır; burada D, derinlik değerini kaydeden ek bir görüntü kanalını temsil eder. Aşağıdaki şekilde görüldüğü gibi, [k4a_transformation_depth_image_to_color_camera ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514) rengi aynı bakış açısı 'ndan alınmış gibi görünür, yani renk kamerasının bakış açısı.

   ![Görüntü dönüştürme](./media/how-to-guides/image-transformation.png)

#### <a name="implementation"></a>Uygulama

 Bu dönüştürme işlevi, her piksel için [k4a_calibration_2d_to_2d ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3b6bf6dedbfe67468e2f895dcce68ed4.html#ga3b6bf6dedbfe67468e2f895dcce68ed4) çağırmanın daha karmaşıktır. Derinlik kamerasının geometrisi olan bir üçgen ağı, renk kamerasının geometrisine çarpıtma. Üçgen kafes, dönüştürülmüş derinlik görüntüsünde delik oluşturmayı önlemek için kullanılır. Bir Z arabelleği, occlus, doğru işlenmesini sağlar. Bu işlev için GPU hızlandırma varsayılan olarak etkinleştirilmiştir.

#### <a name="parameters"></a>Parametreler

 Giriş parametreleri, dönüştürme işleyicsahiptir ve bir derinlik görüntüsüdür. Derinlik görüntü çözümlemesi, ```depth_mode``` dönüştürme tanıtıcısının oluşturulması sırasında belirtilen ile eşleşmelidir. Örneğin, dönüşüm tanıtıcısı 1024x1024 modu kullanılarak oluşturulduysa ```K4A_DEPTH_MODE_WFOV_UNBINNED``` , derinlik resminin çözümlenmesi 1024x1024 piksel olmalıdır. Çıktı, [k4a_image_create ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga859554581bb97a620ff8e92a893e71ef.html#ga859554581bb97a620ff8e92a893e71ef)çağrısı yoluyla kullanıcı tarafından ayrılması gereken dönüştürülmüş bir derinlik görüntüsüdür. Dönüştürülmüş derinlik görüntüsünün çözümlenmesi, ```color_resolution``` dönüştürme tanıtıcısının oluşturulması sırasında belirtilen ile eşleşmelidir. Örneğin, renk çözünürlüğü olarak ayarlandıysa `K4A_COLOR_RESOLUTION_1080P` , çıkış görüntüsü çözünürlüğü 1920x1080 piksel olmalıdır. `width * sizeof(uint16_t)`Görüntü 16 bit derinlik değerlerini depoladığından, çıkış görüntüsü ilerlemesiyle olarak ayarlanır.

### <a name="k4a_transformation_depth_image_to_color_camera_custom"></a>k4a_transformation_depth_image_to_color_camera_custom

#### <a name="overview"></a>Genel Bakış

 [K4a_transformation_depth_image_to_color_camera_custom ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gac00dd00e7612a86382e3d0a130f276bb.html#gac00dd00e7612a86382e3d0a130f276bb) işlevi, derinlik haritasını ve bir özel görüntüyü, derinlik kameranın görüş açısından, renk kamerasının görüş açısından dönüştürür. [K4a_transformation_depth_image_to_color_camera ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514)uzantısı olarak, bu işlev her bir pikselin, dönüştürülmüş derinlik görüntüsüne ek olarak renk kameranın karşılık gelen piksel koordinatları ile eşleştiği ilgili özel bir görüntü oluşturmak için tasarlanmıştır.

#### <a name="implementation"></a>Uygulama

 Bu dönüşüm işlevi, dönüştürülmüş derinlik görüntüsünü [k4a_transformation_depth_image_to_color_camera ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514)ile aynı şekilde oluşturur. Özel görüntüyü dönüştürmek için, bu işlev doğrusal ilişkilendirmeyi veya en yakın komşu ilişkilendirmeyi kullanma seçeneklerini sağlar. Doğrusal ilişkilendirmeyi kullanmak, dönüştürülmüş özel görüntüde yeni değerler oluşturabilir. En yakın komşu ilişkilendirmeyi kullanmak, Orijinal görüntüde bulunmayan değerlerin çıkış görüntüsünde görünmesini engeller, ancak daha az düz görüntüye neden olur. Özel görüntü tek kanallı 8 bit veya 16 bit olmalıdır. Bu işlev için GPU hızlandırma varsayılan olarak etkinleştirilmiştir.

#### <a name="parameters"></a>Parametreler

 Giriş parametreleri, dönüştürme işleyic, bir derinlik görüntüsü, özel bir görüntü ve ilişkilendirme türüdür. Derinlik görüntüsü ve özel görüntü çözümlemesi, `depth_mode` dönüştürme tanıtıcısının oluşturulması sırasında belirtilen ile eşleşmelidir. Örneğin, dönüşüm tanıtıcısı 1024x1024 modu kullanılarak oluşturulduysa `K4A_DEPTH_MODE_WFOV_UNBINNED` , derinlik resminin ve özel görüntünün çözümlenmesi 1024x1024 piksel olmalıdır. `interpolation_type`Ya da olmalıdır `K4A_TRANSFORMATION_INTERPOLATION_TYPE_LINEAR` `K4A_TRANSFORMATION_INTERPOLATION_TYPE_NEAREST` . Çıktı, dönüştürülmüş bir derinlik görüntüsüdür ve [k4a_image_create ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga859554581bb97a620ff8e92a893e71ef.html#ga859554581bb97a620ff8e92a893e71ef)çağrısı yoluyla kullanıcı tarafından ayrılması gereken dönüştürülmüş bir özel görüntüdür. Dönüştürülmüş derinlik resminin ve dönüştürülmüş özel görüntünün çözümlenmesi, `color_resolution` dönüştürme tanıtıcısının oluşturulması sırasında belirtilen ile eşleşmelidir. Örneğin, renk çözünürlüğü olarak ayarlandıysa `K4A_COLOR_RESOLUTION_1080P` , çıkış görüntüsü çözünürlüğü 1920x1080 piksel olmalıdır. `width * sizeof(uint16_t)`Görüntü 16 bit derinlik değerlerini depoladığından, çıkış derinliği görüntü ilerlemesiyle olarak ayarlanır. Giriş özel görüntüsü ve dönüştürülmüş özel görüntü biçiminde olmalıdır `K4A_IMAGE_FORMAT_CUSTOM8` veya `K4A_IMAGE_FORMAT_CUSTOM16` buna karşılık gelen görüntü ilerlemesiyle aynı şekilde ayarlanmalıdır. 

### <a name="k4a_transformation_color_image_to_depth_camera"></a>k4a_transformation_color_image_to_depth_camera

#### <a name="overview"></a>Genel Bakış

 [K4a_transformation_color_image_to_depth_camera ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf3a941f07bb0185cd7a72699a648fc29.html#gaf3a941f07bb0185cd7a72699a648fc29) işlevi, Color kameranın görüş açısından renk görüntüsünü derinlik kamerasının görüş açısından dönüştürür (Yukarıdaki şekil konusuna bakın). Bu, RGB-D görüntüleri oluşturmak için kullanılabilir.

#### <a name="implementation"></a>Uygulama

 Derinlik eşlemesinin her pikseli için işlev, renk görüntüsünde karşılık gelen alt piksel koordinatını hesaplamak için pikselin derinlik değerini kullanır. Daha sonra renk görüntüsünde bu koordinat üzerindeki renk değerini arayacaktır. Bilinear enterpolasyon, renk görüntüsünde, alt piksel duyarlıkta renk değeri elde etmek için gerçekleştirilir. İlişkili derinlikli okuma olmayan bir piksel, çıkış görüntüsünde bir BGRA değerine atanır `[0,0,0,0]` . Bu işlev için GPU hızlandırma varsayılan olarak etkinleştirilmiştir. Bu yöntem, dönüştürülmüş renkli görüntüde boşluklar ürettiğinden ve occlustik oluşturmadığından, bunun yerine [k4a_transformation_depth_image_to_color_camera ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514) işlevini kullanmanızı öneririz.

#### <a name="parameters"></a>Parametreler

Giriş parametreleri, dönüştürme işleyicgidir, bir derinlik görüntüsüdür ve bir renk görüntüsüdür. Derinlik ve renk görüntülerinin çözümlerinin, dönüştürme tanıtıcısının oluşturulmasında belirtilen depth_mode ve color_resolution ile eşleşmesi gerekir. Çıktı, [k4a_image_create ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga859554581bb97a620ff8e92a893e71ef.html#ga859554581bb97a620ff8e92a893e71ef)çağrısı yoluyla kullanıcı tarafından ayrılması gereken dönüştürülmüş bir renk görüntüsüdür. Dönüştürülmüş renkli görüntünün çözümlenmesi, dönüştürme tanıtıcısının oluşturulması sırasında belirtilen depth_resolution eşleşmelidir. Çıktı görüntüsü, her piksel için BGRA 'yi temsil eden dört 8 bitlik değeri depolar. Bu nedenle, görüntünün ilerlemesiyle aynıdır ```width * 4 * sizeof(uint8_t)``` . Veri sırası piksel Aralanmış, yani mavi değer-piksel 0, yeşil değer-piksel 0, kırmızı değer-piksel 0, alfa değeri-piksel 0, mavi değer-piksel 1 vb..

### <a name="k4a_transformation_depth_image_to_point_cloud"></a>k4a_transformation_depth_image_to_point_cloud

#### <a name="overview"></a>Genel Bakış

[K4a_transformation_depth_image_to_point_cloud ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7385eb4beb9d8892e8a88cf4feb3be70.html#ga7385eb4beb9d8892e8a88cf4feb3be70) işlevi, kameranın bir 2B derinlik eşlemesini aynı kameranın koordinat sisteminde bir 3B nokta bulutuna dönüştürür. Kamera, bu sayede derinlik veya renkli kamera olabilir.

#### <a name="implementation"></a>Uygulama

 İşlevi, her piksel için [k4a_calibration_2d_to_2d ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3b6bf6dedbfe67468e2f895dcce68ed4.html#ga3b6bf6dedbfe67468e2f895dcce68ed4) çalıştırmaya denk sonuçlar verir, ancak hesaplama daha etkilidir. [K4a_transformation_create ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga853a1a5b6d521bbdf523a69e890c4f10.html#ga853a1a5b6d521bbdf523a69e890c4f10)çağrılırken, her resim pikseli için x ve y ölçekli faktörleri depolayan, yani adlı bir xy-arama tablosunu önceden hesapladık. [K4a_transformation_depth_image_to_point_cloud ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7385eb4beb9d8892e8a88cf4feb3be70.html#ga7385eb4beb9d8892e8a88cf4feb3be70)çağrılırken, pikselin x ölçeği faktörünü pikselin Z koordinatı ile çarparak pikselin 3B x koordinatını elde ediyoruz. Anormal olarak 3B Y koordinatı, Y ölçekli faktörle çarparak hesaplanır. SDK 'nın [hızlı nokta bulutu örneği](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/fastpointcloud) , XY tablosunun nasıl hesaplanacağını göstermektedir. Kullanıcılar, bu işlevin kendi sürümünü (örneğin, GPU işlem hattını hızlandırmak için) uygulamak üzere örnek kodu izleyebilir.

#### <a name="parameters"></a>Parametreler

 Giriş parametreleri, dönüştürme tutamacı, bir kamera belirleyicisi ve bir derinlik görüntüsüdür. Kamera belirleyicisi derinlik olarak ayarlandıysa, derinlik görüntüsünün çözümlenmesi, dönüştürme tanıtıcısının oluşturulması sırasında belirtilen depth_mode eşleşmelidir. Aksi takdirde, belirtici renk kameraya ayarlandıysa, çözüm seçilen color_resolution çözümlenmesinden eşleşmelidir. Output parametresi, [k4a_image_create ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga859554581bb97a620ff8e92a893e71ef.html#ga859554581bb97a620ff8e92a893e71ef)çağırarak kullanıcı tarafından ayrılması gereken bir xyz görüntüsüdür. XYZ görüntü çözünürlüğü, giriş derinliği eşlemesinin çözümüyle eşleşmelidir. Her piksel için milimetre cinsinden üç adet işaretli 16 bit koordinat değeri depoluyoruz. Bu nedenle XYZ görüntüsü ilerlemesiyle olarak ayarlanır `width * 3 * sizeof(int16_t)` . Veri sırası piksel Aralanmış, yani X koordinatı – pixel 0, Y-koordinatı – pixel 0, Z-koordinatı – pixel 0, X-koordinatı – Pixel 1 vb. olur. Bir piksel 3D olarak dönüştürülemiyorsa, işlev değerleri `[0,0,0]` piksele atar.

## <a name="samples"></a>Örnekler

[Dönüştürme örneği](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/transformation)

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Kinect algılayıcı SDK 'Sı resim dönüştürme işlevlerini nasıl kullanacağınızı öğrenirsiniz, ayrıca

>[!div class="nextstepaction"]
>[Azure Kinect algılayıcı SDK ayarlama işlevleri](use-calibration-functions.md)

Ayrıca şunları gözden geçirebilirsiniz

[Koordinat sistemleri](coordinate-systems.md)
