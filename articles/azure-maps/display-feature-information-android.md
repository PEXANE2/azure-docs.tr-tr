---
title: Android haritalar 'da Özellik bilgilerini görüntüleme | Microsoft Azure haritaları
description: Kullanıcılar harita özellikleriyle etkileşim kurarken bilgileri görüntülemeyi öğrenin. Bildirim iletilerini ve diğer ileti türlerini göstermek için Azure Haritalar Android SDK kullanın.
author: rbrundritt
ms.author: richbrun
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 37b5ab1c144ed81d995da40b87edeaccdcad7253
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97680002"
---
# <a name="display-feature-information"></a>Özellik bilgilerini görüntüleme

Uzamsal veriler genellikle noktaları, çizgileri ve çokgenler kullanılarak temsil edilir. Bu verilere genellikle ilişkili meta veri bilgileri sahip olur. Örneğin, bir nokta, Restoran konumunu temsil edebilir ve bu restoran hakkında, hizmet verdiği yiyecek 'nin adı, adresi ve veri türü olabilir. Bu meta veriler coğrafi JSON 'ın özellikleri olarak eklenebilir `Feature` . Aşağıdaki kod, `title` "Merhaba Dünya!" değerine sahip bir özelliği olan basit bir nokta özelliği oluşturur

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a point feature.
Feature feature = Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64));

//Add a property to the feature.
feature.addStringProperty("title", "Hello World!");

//Create a point feature, pass in the metadata properties, and add it to the data source.
source.add(feature);
```

Verileri oluşturma ve haritaya ekleme yolları için [veri kaynağı oluşturma](create-data-source-android-sdk.md) belgelerine bakın.

Kullanıcı haritada bir özellik ile etkileşime geçtiğinde, olaylar bu eylemlere tepki vermek için kullanılabilir. Yaygın bir senaryo, Kullanıcı tarafından etkileşim kurulan bir özelliğin meta veri özelliklerinden oluşan bir ileti görüntülemektir. `OnFeatureClick`Olay, kullanıcının haritadaki bir özelliği dokunduğunda tespit etmek için kullanılan ana olaydır. Ayrıca bir olay da vardır `OnLongFeatureClick` . `OnFeatureClick`Olayı haritaya eklerken, bir katmanın kimliğini ile sınırlamak için bir katman kimliğine geçerek tek bir katmanla sınırlı olabilir. Katman KIMLIĞI geçirilmemişse, hangi katmanda olduğuna bakılmaksızın haritadaki herhangi bir özelliğe dokunarak bu olayı tetiklersiniz. Aşağıdaki kod, haritada nokta verisi işlemek için bir sembol katmanı oluşturur, sonra bir `OnFeatureClick` olay ekler ve bu sembol katmanına kısıtlar.

```java
//Create a symbol and add it to the map.
SymbolLayer layer = new SymbolLayer(source);
map.layers.add(layer);

//Add a feature click event to the map.
map.events.add((OnFeatureClick) (features) -> {
    //Retrieve the title property of the feature as a string.
    String msg = features.get(0).getStringProperty("title");

    //Do something with the message.
}, layer.getId());    //Limit this event to the symbol layer.
```

## <a name="display-a-toast-message"></a>Bildirim iletisi görüntüle

Bildirim iletisi, kullanıcıya bilgi görüntülemenin en kolay yollarından biridir ve Android 'in tüm sürümlerinde kullanılabilir. Her tür Kullanıcı girişini desteklemez ve yalnızca kısa bir süre için görüntülenir. Kullanıcının ne dokundukları hakkında bir şey bilmesini hızlıca sağlamak istiyorsanız, bir bildirim iletisi iyi bir seçenek olabilir. Aşağıdaki kod, bir bildirim iletisinin olayla nasıl kullanılabileceğini gösterir `OnFeatureClick` .

```java
//Add a feature click event to the map.
map.events.add((OnFeatureClick) (features) -> {
    //Retrieve the title property of the feature as a string.
    String msg = features.get(0).getStringProperty("title");

    //Display a toast message with the title information.
    Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
}, layer.getId());    //Limit this event to the symbol layer.
```

![Dokunulmakta olan bir özelliğin animasyonu ve bir bildirim iletisi görüntüleniyor](./media/display-feature-information-android/symbol-layer-click-toast-message.gif)

Mesajların yanı sıra, bir özelliğin meta veri özelliklerini sunmak için birçok farklı yol vardır:

- [Snackbar pencere öğesi](https://developer.android.com/training/snackbar/showing.html)  -  `Snackbars` bir işlem hakkında hafif geri bildirim sağlayın. Bu kişiler, ekranın alt kısmında mobil ve daha büyük cihazlarda sol alt kısımdaki kısa bir ileti gösterir. `Snackbars` Tüm diğer öğelerin ekranda görünmesini ve tek seferde yalnızca bir tane görüntülenmesini sağlayabilirsiniz.
- İletişim [kutuları](https://developer.android.com/guide/topics/ui/dialogs) -bir iletişim kutusu, kullanıcıdan karar vermesini veya ek bilgiler girmesini isteyen küçük bir penceredir. Bir iletişim kutusu, ekranı doldurmaz ve normalde, kullanıcıların devam etmeden önce bir eylemde bulunması gereken kalıcı olaylar için kullanılır.
- Geçerli etkinliğe bir [parça](https://developer.android.com/guide/components/fragments) ekleyin.
- Başka bir etkinliğe veya görünüme gidin.

## <a name="next-steps"></a>Sonraki adımlar

Haritanızda daha fazla veri eklemek için:

> [!div class="nextstepaction"]
> [Eşleme olaylarına tepki verme](android-map-events.md)

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](how-to-add-shapes-to-android-map.md)
