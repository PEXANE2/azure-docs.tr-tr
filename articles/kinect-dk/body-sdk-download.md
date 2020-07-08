---
title: Azure Kinect gövde Izleme SDK indirmesi
description: Windows veya Linux 'ta Azure Kinect algılayıcı SDK 'sının her bir sürümünün nasıl indirileceği hakkında bilgi edinin.
author: qm13
ms.author: quentinm
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Azure, Kinect, SDK, indirme güncelleştirme, en son, kullanılabilir, yükleme, gövde, izleme
ms.openlocfilehash: e3f8233d208e2a45c1af9a52a76b6064b15bfe4b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85277728"
---
# <a name="download-azure-kinect-body-tracking-sdk"></a>Azure Kinect gövde Izleme SDK 'sını indirin

Bu belge, Azure Kinect gövde Izleme SDK 'sının her bir sürümünü yüklemek için bağlantılar sağlar.

## <a name="azure-kinect-body-tracking-sdk-contents"></a>Azure Kinect gövdesi Izleme SDK içeriği

- Azure Kinect DK kullanarak bir gövde izleme uygulaması derlemek için üst bilgiler ve kitaplıklar.
- Azure Kinect DK kullanan gövde izleme uygulamaları için yeniden dağıtılabilir DLL 'Ler gereklidir.
- Örnek gövde izleme uygulamaları.

## <a name="windows-download-links"></a>Windows indirme bağlantıları

Sürüm       | İndir
--------------|----------
1.0.1 | [MSI](https://www.microsoft.com/en-us/download/details.aspx?id=100942) [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/1.0.1)
1.0.0 | [MSI](https://www.microsoft.com/en-us/download/details.aspx?id=100848) [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/1.0.0)
0.9.5 | [MSI](https://www.microsoft.com/en-us/download/details.aspx?id=100636) [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/0.9.5)
0.9.4 | [MSI](https://www.microsoft.com/en-us/download/details.aspx?id=100415) [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/0.9.4)
0.9.3 | [MSI](https://www.microsoft.com/en-us/download/details.aspx?id=100307) [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/0.9.3)
0.9.2 | [MSI](https://www.microsoft.com/en-us/download/details.aspx?id=100128) [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/0.9.2)
0.9.1 | [MSI](https://www.microsoft.com/en-us/download/details.aspx?id=100063) [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/0.9.1)
0.9.0 | [MSI](https://www.microsoft.com/en-us/download/details.aspx?id=58402) [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/0.9.0)

## <a name="linux-installation-instructions"></a>Linux yükleme yönergeleri

Şu anda desteklenen tek dağıtım Ubuntu 18,04 ' dir. Diğer dağıtımlar için destek istemek için [Bu sayfaya](https://aka.ms/azurekinectfeedback)bakın.

İlk olarak, [buradaki](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software)yönergeleri izleyerek [Microsoft 'un paket deposunu](https://packages.microsoft.com/)yapılandırmanız gerekecektir.

`libk4abt<major>.<minor>-dev`Paket, oluşturulacak üst bilgileri ve CMake dosyalarını içerir `libk4abt` .
`libk4abt<major>.<minor>`Paket, `libk4abt` örnek görüntüleyiciye ve ayrıca bağlı olan yürütülebilir dosyaları çalıştırmak için gereken paylaşılan nesneleri içerir.

Temel öğreticiler `libk4abt<major>.<minor>-dev` paketini gerektirir. Yüklemek için şunu çalıştırın

`sudo apt install libk4abt1.0-dev`

Komut başarılı olursa SDK kullanıma hazırlanmaya devam edilir.

> [!NOTE]
> SDK 'yı yüklerken, yüklediğiniz yolu unutmayın. Örneğin, "C:\Program Files\Azure Kinect Body Tracking SDK 1.0.0". Bu yoldaki makalelerde başvurulan örnekleri bulacaksınız.
> Gövde izleme örnekleri, Azure-Kinect-Samples deposundaki [Body-Tracking-Samples](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples) klasöründe bulunur. Burada makalelerde başvurulan örnekleri bulacaksınız.

## <a name="change-log"></a>Değişiklik günlüğü

### <a name="v101"></a>v 1.0.1
* [Hata düzeltildi] Windows Build 19025 veya sonraki sürümlerde yolundan onnxruntime.dll yüklüyorsanız SDK 'nın çöktüğü sorunu çözme: [bağlantı](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/932)

### <a name="v100"></a>v 1.0.0
* Özellik MSI yükleyicisine C# sarmalayıcı ekleyin.
* [Hata düzeltildi] Baş dönüşün doğru algılanamadığından sorunu çözme: [bağlantı](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/997)
* [Hata düzeltildi] CPU kullanımının Linux makinesinde %100 ' e varan bir sorunu giderme: [bağlantı](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1007)
* Lerinizi Örnek depoya iki örnek ekleyin. Örnek 1 ' de, gövde izleme sonuçlarının derinlik alanından renk alanı [bağlantısına](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples/camera_space_transform_sample)nasıl dönüştürülebileceğiniz gösterilmektedir; Örnek 2 ' de zemin düzlemi [bağlantısının](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples/floor_detector_sample) nasıl algılanması gösterilmektedir

### <a name="v095"></a>v 0.9.5
* Özellik C# desteği. C# sarmalayıcı, NuGet paketinde paketlenmiştir.
* Özellik Çoklu izleyici desteği. Birden çok trackingsize oluşturulmasına izin verilir. Artık Kullanıcı, farklı Azure Kinect cihazlarından gövdeler izlemek için birden çok izleme oluşturabilir.
* Özellik CPU modu için çoklu iş parçacığı işleme desteği. CPU modunda çalışırken, tüm çekirdekler hızı en üst düzeye çıkarmak için kullanılacaktır.
* Özellik `gpu_device_id`Struct öğesine ekleyin `k4abt_tracker_configuration_t` . Kullanıcıların gövde izleme algoritmasını çalıştırmak için varsayılan olandan başka bir GPU aygıtı belirtmesini sağlar.
* [Hata düzeltildi/Son değişiklik] Bir Birleşik adda yazım hatası 'ı düzeltir. Birleşik adı ' dan ' a değiştirin `K4ABT_JOINT_SPINE_NAVAL` `K4ABT_JOINT_SPINE_NAVEL` .

### <a name="v094"></a>v 0.9.4
* Özellik El ile destek ekleme. SDK her bir el için üç ek bilgi sağlar: el, el tıp, parmak izi.
* Özellik Algılanan her bir neşeli için tahmin güven düzeyi ekleyin.
* Özellik CPU modu desteği ekleyin. `cpu_only_mode`İçindeki değeri değiştirerek `k4abt_tracker_configuration_t` , artık SDK, kullanıcının güçlü bir grafik kartına sahip olmasını gerektirmeyen CPU modunda çalışabilir.

### <a name="v093"></a>v 0.9.3
* Özellik Büyük ölçüde gövde izlemenin sağlamlığını artıran yeni bir DNN modeli dnn_model_2_0. onnx yayımlayın.
* Özellik Varsayılan olarak zamana yumuşatmaya devre dışı bırakın. İzlenen eklemler daha hızlı yanıt verir.
* Özellik Gövde dizini eşlemesinin doğruluğunu geliştirir.
* [Hata düzeltildi] Algılayıcı yönü ayarının etkili olmadığı hatayı düzeltir.
* [Hata düzeltildi] Body_index_map türünü K4A_IMAGE_FORMAT_CUSTOM K4A_IMAGE_FORMAT_CUSTOM8 olarak değiştirin.
* [Bilinen sorun] İki yakın gövde tek örnekli kesimiyle birleştirilebilir.

### <a name="v092"></a>v 0.9.2
* [Son değişiklik] Güncelleştirme, en son Azure Kinect algılayıcı SDK 1.2.0 bağımlıdır.
* [API değişikliği] `k4abt_tracker_create` işlevi bir girişi alacak şekilde başlayacaktır `k4abt_tracker_configuration_t` . 
* [API değişikliği] `k4abt_frame_get_timestamp_usec`API 'yi `k4abt_frame_get_device_timestamp_usec` , algılayıcı SDK 1.2.0 ile daha belirli ve tutarlı olacak şekilde değiştirin.
* Özellik Farklı açılardan bağlama sırasında daha doğru gövde izleme sonuçları elde etmek üzere izleyici oluştururken kullanıcıların algılayıcı bağlama yönlendirmesini belirtmesini sağlar.
* Özellik `k4abt_tracker_set_temporal_smoothing`Kullanıcının uygulamak istediği zamana bağlı yumuşatma miktarını değiştirmek için yenı API sağlayın.
* Özellik C++ sarmalayıcı K4ABT. hpp ekleyin.
* Özellik Sürüm tanımı üst bilgisi k4abtversion. h ekleyin.
* [Hata düzeltildi] Son derece yüksek CPU kullanımına neden olan hatayı düzeltir.
* [Hata düzeltildi] Günlükçü kilitlenme hatasını düzeltir.

### <a name="v091"></a>v 0.9.1
* [Hata düzeltildi] İzleyici yok edilirken Bellek sızıntısını çözme
* [Hata düzeltildi] Eksik bağımlılıklara yönelik daha iyi hata iletileri
* [Hata düzeltildi] İkinci bir izleyici örneği oluşturulurken kilitlenme olmadan başarısız oldu
* [Hata düzeltildi] Günlükçü ortam değişkenleri artık düzgün çalışıyor
* Linux desteği

### <a name="v090"></a>v 0.9.0

* [Son değişiklik] SDK bağımlılığını CUDA 10,0 (CUDA 10,1) olarak düşürülemez. ONNX Runtime resmi yalnızca CUDA 10,0 ' i destekler.
* [Son değişiklik] TensorFlow çalışma zamanı yerine ONNX çalışma zamanına geçti. İlk çerçeveyi, zaman ve bellek kullanımını Başlatan şekilde azaltır. Ayrıca SDK ikili boyutunu da azaltır.
* [API değişikliği] Yeniden `k4abt_tracker_queue_capture()` adlandırıldı`k4abt_tracker_enqueue_capture()`
* [API değişikliği] `k4abt_frame_get_body()`İki ayrı işleve sahiptir: `k4abt_frame_get_body_skeleton()` ve `k4abt_frame_get_body_id()` . Artık tüm iskelet yapısını kopyalamaya gerek kalmadan gövde KIMLIĞINI sorgulayabilirsiniz.
* [API değişikliği] `k4abt_frame_get_timestamp_usec()`Kullanıcıların gövde çerçevesi zaman damgasını sorgulayabilecek adımları basitleştirmek için işlevi eklenmiştir.
* Gövde izleme algoritması doğruluğu ve izleme güvenilirliği daha da geliştirildi

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Kinect DK genel bakış](about-azure-kinect-dk.md)

- [Azure Kinect DK’yi kurma](set-up-azure-kinect-dk.md)

- [Azure Kinect gövdesi izlemeyi ayarlama](body-sdk-setup.md)
