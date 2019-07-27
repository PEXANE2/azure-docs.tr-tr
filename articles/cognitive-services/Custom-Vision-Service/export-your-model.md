---
title: Modelinizi mobil Özel Görüntü İşleme Hizmeti dışa aktarma
titleSuffix: Azure Cognitive Services
description: Modelinizi mobil uygulamalar oluştururken kullanmak üzere nasıl dışarı aktaralabileceğinizi öğrenin.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: 554a392a7f815a6e646927f137b1e6c2856099bd
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68561080"
---
# <a name="export-your-model-for-use-with-mobile-devices"></a>Modelinizi mobil cihazlarla kullanılmak üzere dışarı aktarma

Özel Görüntü İşleme Hizmeti, sınıflandırıcıın çevrimdışı çalışacak şekilde verilmesini sağlar. Verdiğiniz sınıflandırmanızı bir uygulamaya ekleyebilir ve gerçek zamanlı sınıflandırma için bir cihazda yerel olarak çalıştırabilirsiniz.

Özel Görüntü İşleme Hizmeti aşağıdaki dışarı aktarmaları destekler:

* __Android__Için __TensorFlow__ .
* __İOS11__Için __coreml__ .
* __WINDOWS ml__Için __onnx__ .
* Bir Windows veya Linux __kapsayıcısı__. Kapsayıcı, Özel Görüntü İşleme Hizmeti API 'sini kullanmak için bir TensorFlow modeli ve hizmet kodu içerir. 

> [!IMPORTANT]
> Özel Görüntü İşleme Hizmeti yalnızca __Compact__ etki alanlarını dışarı aktarır. Compact etki alanları tarafından oluşturulan modeller, mobil cihazlardaki gerçek zamanlı sınıflandırmanın kısıtlamaları için iyileştirilmiştir. Kompakt bir etki alanıyla oluşturulan sınıflandırıcılar, aynı miktardaki eğitim verilerine sahip standart bir etki alanından biraz daha az doğru olabilir.
>
> Sınıflandırıcılarınızı iyileştirme hakkında daha fazla bilgi için bkz. [sınıflandırıcınızı geliştirme](getting-started-improving-your-classifier.md) belgesi.

## <a name="convert-to-a-compact-domain"></a>Compact etki alanına Dönüştür

> [!NOTE]
> Bu bölümdeki adımlar yalnızca, Compact Domain olarak ayarlanmamış mevcut bir sınıflandırıcı varsa geçerlidir.

Mevcut bir sınıflandırıcının etki alanını dönüştürmek için aşağıdaki adımları kullanın:

1. [Özel Vizyon sayfasında](https://customvision.ai), projelerinizin listesini görüntülemek için __giriş__ simgesini seçin. Projelerinizi görmek [https://customvision.ai/projects](https://customvision.ai/projects) için öğesini de kullanabilirsiniz.

    ![Giriş simgesinin ve Projeler listesinin görüntüsü](./media/export-your-model/projects-list.png)

2. Bir proje seçin ve sayfanın sağ üst kısmındaki __dişli__ simgesini seçin.

    ![Dişli simgesinin görüntüsü](./media/export-your-model/gear-icon.png)

3. __Etki alanları__ bölümünde, bir __Compact__ etki alanı seçin. Değişiklikleri kaydetmek için __Değişiklikleri Kaydet__ ' i seçin.

    ![Etki alanı seçiminin görüntüsü](./media/export-your-model/domains.png)

4. Sayfanın üst kısmından yeni etki alanını kullanarak yeniden eğitedenemek için __eğitme__ ' yi seçin.

## <a name="export-your-model"></a>Modelinizi dışarı aktarma

Yeniden öğreticduktan sonra modeli dışarı aktarmak için aşağıdaki adımları kullanın:

1. **Performans** sekmesine gidin ve __dışarı aktar__' ı seçin. 

    ![Dışa aktarma simgesinin görüntüsü](./media/export-your-model/export.png)

    > [!TIP]
    > __Dışarı aktarma__ girdisi kullanılamıyorsa, seçilen yineleme bir Compact etki alanı kullanmaz. Sıkıştırılmış etki alanı kullanan bir yinelemeyi seçmek için bu sayfanın __yinelemeler__ bölümünü kullanın ve ardından __dışarı aktar__' ı seçin.

2. Dışarı aktarma biçimini seçin ve ardından modeli indirmek için __dışarı aktar__ ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerden veya örneklerden birini inceleyerek, verdiğiniz modeli bir uygulamayla tümleştirin:

* [Python ile TensorFlow modelinizi kullanma](export-model-python.md)
* [Windows Machine Learning ile ONNX modelinizi kullanın](custom-vision-onnx-windows-ml.md)
* Swift ile gerçek zamanlı görüntü sınıflandırması için [bir iOS uygulamasındaki Coreml modeli](https://go.microsoft.com/fwlink/?linkid=857726) örneğine bakın.
* Android 'de gerçek zamanlı görüntü sınıflandırması için [Android uygulamasındaki TensorFlow modeli](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) örneğine bakın.
* Xamarin iOS uygulamasındaki gerçek zamanlı görüntü sınıflandırması için [Xamarin Ile Coreml modeli](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) örneğine bakın.
