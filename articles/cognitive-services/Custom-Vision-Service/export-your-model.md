---
title: Modelinizi mobil cihazlara aktarın - Özel Vizyon Hizmeti
titleSuffix: Azure Cognitive Services
description: Bu makalede, mobil uygulamalar oluştururken kullanılmak üzere modelinizi nasıl dışa aktaracağınızı veya gerçek zamanlı sınıflandırma için yerel olarak nasıl çalıştıracağınızı gösterecektir.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: f734f4f1a11f57b759615e7a9ce2cd2f7f8028fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73718958"
---
# <a name="export-your-model-for-use-with-mobile-devices"></a>Modelinizi mobil cihazlarla kullanmak üzere dışa aktarma

Özel Görme Hizmeti, sınıflandırıcıların çevrimdışı çalıştırmak için dışa aktarılmasını sağlar. Dışa aktarılan sınıflandırıcınızı bir uygulamaya katıştırabilir ve gerçek zamanlı sınıflandırma için bir aygıtta yerel olarak çalıştırabilirsiniz.

## <a name="export-options"></a>Dışa aktarma seçenekleri

Custom Vision Service aşağıdaki dışa aktarmayı destekler:

* __Android__için __Tensorflow__ .
* __iOS11__için __CoreML__ .
* __Windows ML__için __ONNX__ .
* __[Vizyon AI Geliştirici Kiti](https://azure.github.io/Vision-AI-DevKit-Pages/)__.
* Windows, Linux veya ARM mimarisi için docker __kapsayıcısı.__ Kapsayıcı, Özel Görüş API'sini kullanmak için bir Tensorflow modeli ve servis kodu içerir.

> [!IMPORTANT]
> Custom Vision Service yalnızca kompakt etki alanlarını __dışa__ aktarım. Kompakt etki alanları tarafından oluşturulan modeller, mobil cihazlarda gerçek zamanlı sınıflandırma kısıtlamaları için optimize edilebilmektedir. Kompakt bir etki alanıyla oluşturulmuş sınıflandırıcılar, aynı miktarda eğitim verisi olan standart bir etki alanından biraz daha az doğru olabilir.
>
> Sınıflandırıcılarınızı geliştirme hakkında bilgi için [sınıflandırıcı belgenizi geliştirme'ye](getting-started-improving-your-classifier.md) bakın.

## <a name="convert-to-a-compact-domain"></a>Kompakt bir etki alanına dönüştürme

> [!NOTE]
> Bu bölümdeki adımlar yalnızca etki alanını sıkıştıracak şekilde ayarlanmayan varolan bir modele sahipseniz geçerlidir.

Varolan bir modelin etki alanını dönüştürmek için aşağıdaki adımları izleyin:

1. Özel [görme web sitesinde,](https://customvision.ai)projelerinizin listesini görüntülemek için __Ana Sayfa__ simgesini seçin.

    ![Ev simgesi ve proje listesinin görüntüsü](./media/export-your-model/projects-list.png)

1. Bir proje seçin ve ardından sayfanın sağ üst kısmındaki __Dişli__ simgesini seçin.

    ![Dişli simgesinin görüntüsü](./media/export-your-model/gear-icon.png)

1. Etki __Alanları__ bölümünde, __kompakt__ etki alanlarından birini seçin. Değişiklikleri kaydetmek için __Değişiklikleri Kaydet'i__ seçin. 

    > [!NOTE]
    > Vision AI Dev Kit için, proje __Genel (Kompakt)__ etki alanı ile oluşturulmalıdır ve **Dışa Aktarma Yetenekleri** bölümü altında **Vision AI Dev Kit** seçeneğini belirtmeniz gerekir.

    ![Etki alanı seçiminin görüntüsü](./media/export-your-model/domains.png)

1. Sayfanın üst kısmından, yeni etki alanını kullanarak yeniden eğitmek için __Tren'i__ seçin.

## <a name="export-your-model"></a>Verilerinizi dışarı aktarma

Yeniden eğitimden sonra modeli dışa aktarmak için aşağıdaki adımları kullanın:

1. **Performans** sekmesine gidin ve __Dışa Aktar'ı__seçin. 

    ![Dışa aktarma simgesinin görüntüsü](./media/export-your-model/export.png)

    > [!TIP]
    > __Dışa Aktarma__ girişi kullanılamıyorsa, seçili yineleme kompakt bir etki alanı kullanmaz. Kompakt bir etki alanı kullanan bir yineleme seçmek için bu sayfanın __Yinelemeler__ bölümünü kullanın ve ardından __Dışa Aktar'ı__seçin.

1. İstediğiniz dışa aktarma biçimini seçin ve ardından modeli indirmek için __Dışa Aktar'ı__ seçin.

## <a name="next-steps"></a>Sonraki adımlar

Dışa aktarılan modelinizi aşağıdaki makalelerden veya örneklerden birini keşfederek bir uygulamaya entegre edin:

* [Python ile Tensorflow modelinizi kullanın](export-model-python.md)
* [Windows Machine Learning ile ONNX modelinizi kullanın](custom-vision-onnx-windows-ml.md)
* Swift ile gerçek zamanlı görüntü sınıflandırması için [bir iOS uygulamasında CoreML modeli](https://go.microsoft.com/fwlink/?linkid=857726) için örnek bakın.
* Android'de gerçek zamanlı görüntü sınıflandırması için [bir Android uygulamasında Tensorflow modeli](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) örneği için bkz.
* Bir Xamarin iOS uygulamasında gerçek zamanlı görüntü sınıflandırması için [Xamarin ile CoreML modeli](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) örneğine bakın.
