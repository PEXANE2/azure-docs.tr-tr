---
title: Özel Görüntü İşleme projesi için bir etki alanı seçin-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Bu makalede, Özel Görüntü İşleme Hizmeti projeniz için bir etki alanı seçme gösterilmektedir.
services: cognitive-services
author: shonohs
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: shono
ms.openlocfilehash: 87b9e4a3ca7151b3666928b00add175eddeea050
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94409391"
---
# <a name="select-a-domain-for-a-custom-vision-project"></a>Özel Görüntü İşleme projesi için bir etki alanı seçin

Özel Görüntü İşleme projenizin ayarlar dikey penceresinde, projeniz için bir etki alanı seçebilirsiniz. Senaryonuza en yakın etki alanını seçin.

## <a name="image-classification"></a>Resimleri Sınıflandırma

|Etki alanı|Amaç|
|---|---|
|__Genel__| Çok çeşitli resim Sınıflandırma görevleri için iyileştirilmiştir. Diğer etki alanlarından hiçbiri uygun değilse veya hangi etki alanının seçeceğinizden emin değilseniz, genel etki alanını seçin.|
|__Yemek__|Bir restoran menüsünde gördüğünüz gibi yemeklerin fotoğrafları için iyileştirildi. Bireysel meyve veya vegetables fotoğraflarını sınıflandırmak istiyorsanız, yiyecek etki alanını kullanın.|
|__Bölümler__|Hem doğal hem de yapay olan tanınabilir yer işaretleri için iyileştirilmiştir. Bu etki alanı, yer işareti fotoğrafta açık bir şekilde görünür olduğunda en iyi şekilde çalışıyor. Bu etki alanı, yer işareti, önündeki kişiler tarafından biraz engelde olsa bile çalışıyor.|
|__Perakende__|Bir alışveriş kataloğunda veya alışveriş web sitesinde bulunan görüntüler için iyileştirilmiştir. Dresler, Pants ve Shirts arasında yüksek duyarlıklı sınıflandırmaları istiyorsanız bu etki alanını kullanın.|
|__Compact etki alanları__| Edge cihazlarında gerçek zamanlı sınıflandırmanın kısıtlamaları için iyileştirildi.|

## <a name="object-detection"></a>Nesne Algılama

|Etki alanı|Amaç|
|---|---|
|__Genel__| Çok çeşitli nesne algılama görevleri için iyileştirilmiştir. Diğer etki alanlarından hiçbiri uygun değilse veya hangi etki alanının seçeceğinizden emin değilseniz, genel etki alanını seçin.|
|__Logo__|Görüntülerde marka logolarını bulmak için iyileştirildi.|
|__Rafların ürünleri__|Rafların ürünlerini algılamak ve sınıflandırmak için iyileştirildi.|
|__Compact etki alanları__| Uç cihazlarda gerçek zamanlı nesne algılama kısıtlamaları için iyileştirilmiştir.|

## <a name="compact-domains"></a>Compact etki alanları

Compact etki alanları tarafından oluşturulan modeller yerel olarak çalışacak şekilde aktarılabilir. Özel Görüntü İşleme 3,4 genel önizleme API 'sinde, GetDomains API 'sini çağırarak, Compact etki alanları için dışarı aktarılabilir platformların bir listesini alabilirsiniz.

Model performansı seçilen etki alanına göre değişir. Aşağıdaki tabloda, Intel Masaüstü CPU ve NVIDIA GPU 1 ' deki model boyutunu ve çıkarım zamanını raporlarız \[ \] . Bu numaralar ön işleme ve anında işleme süresini içermez.

|Görev|Etki alanı|Model Boyutu|CPU çıkarımı süresi|GPU çıkarımı süresi|
|---|---|---|---|---|
|Sınıflandırma|General (compact) (Genel (kompakt))|5 MB|13 MS|5 MS|
|Nesne Algılama|General (compact) (Genel (kompakt))|45 MB|35 MS|5 MS|
|Nesne Algılama|Genel (Compact) [S1]|14 MB|27 MS|7 MS|

>[!NOTE]
>Nesne algılaması için __Genel (Compact)__ etki alanı özel bir özelleştirmediğiniz mantığı gerektirir. Ayrıntılı bilgi için lütfen bkz. bir örnek komut dosyası. Gönderme işlemi mantığı olmadan bir modele ihtiyacınız varsa __Genel (Compact) [S1]__ kullanın.

>[!IMPORTANT]
>Verme modellerinin, buluttaki tahmin API 'siyle tam olarak aynı sonucu vereceğiyle ilgili hiçbir garanti yoktur. Çalışan platformda veya ön işleme uygulamasındaki hafif fark, model çıkışları üzerinde daha büyük farka neden olabilir. Ön işleme mantığının ayrıntıları için lütfen [Bu belgeye](quickstarts/image-classification.md)bakın.

\[1 \] Intel Xeon E5-2690 CPU ve NVIDIA Tesla M60
