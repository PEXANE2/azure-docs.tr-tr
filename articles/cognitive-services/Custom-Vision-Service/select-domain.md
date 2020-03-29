---
title: Özel Vizyon projesi için bir etki alanı seçin - Computer Vision
titleSuffix: Azure Cognitive Services
description: Bu makalede, Özel Vizyon Hizmeti'nde projeniz için bir etki alanı nasıl seçeceğiniz gösterilmektedir.
services: cognitive-services
author: shonohs
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: shono
ms.openlocfilehash: 1569b6081adad4cae0855f9adfb4e14e910bf819
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78899456"
---
# <a name="select-a-domain-for-a-custom-vision-project"></a>Özel Vizyon projesi için etki alanı seçme

Özel Vizyon projenizin ayarlar bıçağından, projeniz için bir etki alanı seçebilirsiniz. Senaryonuza en yakın etki alanını seçin.

## <a name="image-classification"></a>Resimleri Sınıflandırma

|Domain|Amaç|
|---|---|
|__Genel__| Çok çeşitli görüntü sınıflandırma görevleri için optimize edin. Diğer etki alanlarından hiçbiri uygun değilse veya hangi etki alanını seçeceğiniz den emin değilseniz, Genel etki alanını seçin.|
|__Gıda__|Bir restoran menüsünde görmek gibi yemeklerin fotoğrafları için optimize edin. Tek tek meyve veya sebzelerin fotoğraflarını sınıflandırmak istiyorsanız, Gıda alan adını kullanın.|
|__Yer işaretleri__|Hem doğal hem de yapay olarak tanınabilir yerler için optimize edin. Bu etki alanı en iyi, simge sel fotoğrafta açıkça görüldüğünde çalışır. Bu etki alanı, önündeki kişiler tarafından biraz engellenmiş olsa bile çalışır.|
|__Perakende__|Bir alışveriş kataloğunda veya alışveriş web sitesinde bulunan resimler için optimize edin. Elbiseler, pantolonlar ve gömlekler arasında yüksek hassasiyetle sınıflandırmak istiyorsanız, bu alanı kullanın.|
|__Kompakt etki alanları__| Kenar aygıtlarında gerçek zamanlı sınıflandırma kısıtlamaları için optimize edin.|

## <a name="object-detection"></a>Nesne Algılama

|Domain|Amaç|
|---|---|
|__Genel__| Çok çeşitli nesne algılama görevleri için optimize edin. Diğer etki alanlarından hiçbiri uygun değilse veya hangi etki alanını seçeceğiniz den emin değilseniz, Genel etki alanını seçin.|
|__Logo__|Görüntülerde marka logolarını bulmak için optimize edin.|
|__Raflarda Ürünler__|Raflarda ürünleri algılamak ve sınıflandırmak için optimize edin.|
|__Kompakt etki alanları__| Kenar aygıtlarında gerçek zamanlı nesne algılama kısıtlamaları için optimize edin.|

## <a name="compact-domains"></a>Kompakt etki alanları

Kompakt etki alanları tarafından oluşturulan modeller yerel olarak çalıştırmak için dışa aktarılabilir. Model performansı seçili etki alanına göre değişir. Aşağıdaki tabloda, Intel Masaüstü CPU ve NVidia GPU \[1\]model boyutu ve çıkarım süresi rapor. 

> [!NOTE]
> Bu sayılar ön işleme ve işlem sonrası zamanı içermez.

|Görev|Domain|Model Boyutu|CPU çıkarım süresi|GPU çıkarım süresi|
|---|---|---|---|---|
|Sınıflandırma|General (compact) (Genel (kompakt))|5 MB|13 ms|5 ms|
|Nesne Algılama|General (compact) (Genel (kompakt))|45 MB|35 ms|5 ms|
|Nesne Algılama|Genel (kompakt) [S1]|14 MB|27 ms|7 ms|

## <a name="vaidk-vision-ai-dev-kit"></a>VAIDK (Vision AI Dev Kiti)

Kompakt bir etki alanı seçildiğinde ,"Temel Platformlar" ile "Vision AI Dev Kit" arasında ayrım sağlayan "Dışa Aktarma Yetenekleri" seçeneği sağlanır.

_Dışa Aktarma Yetenekleri_ altında iki seçenek vardır:

- Temel platformlar (Tensorflow, CoreML, ONNX, vb.)
- Vizyon AI Dev Kit.

_Vision AI Dev Kit_ _Genel_seçildiğinde, _Yerler_, ve _Perakende_ ama _Gıda_ kompakt etki alanları Görüntü Sınıflandırma için kullanılabilir iken hem _Genel (kompakt)_ ve _Genel (kompakt) [S1]_ nesne algılama için kullanılabilir.

>[!NOTE]
>Nesne Algılama için __genel (kompakt)__ etki alanı özel postprocessing mantığı gerektirir. Ayrıntı için, lütfen dışa aktarılan zip paketinde bir örnek komut dosyasına bakın. Postprocessing mantığı olmayan bir modele ihtiyacınız varsa, __Genel (kompakt) [S1]__ kullanın.

>[!IMPORTANT]
>Dışa aktarılan modellerin buluttaki tahmin API'si ile tam olarak aynı sonucu verdiğini garanti etmez. Çalışan platformdaki küçük fark veya ön işleme uygulaması model çıktılarında daha büyük farklara neden olabilir. Ön işleme mantığının ayrıntıları için lütfen [bu belgeye](python-tutorial.md)bakın.

\[1\] Intel Xeon E5-2690 CPU ve NVIDIA Tesla M60
