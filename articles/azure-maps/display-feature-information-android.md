---
title: Azure Haritalar Android SDK'da özellik bilgilerini görüntüleyin | Microsoft Azure Haritaları
description: Bu makalede, Özellik bilgilerinin bir haritada Microsoft Azure Haritalar Android SDK'sını kullanarak nasıl görüntüleneceksiniz öğrenebilirsiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 26f41a7fd88a3c2018592e89ae95e3b962c1a9e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75911686"
---
# <a name="display-feature-information"></a>Özellik bilgilerini görüntüleme

Uzamsal veriler genellikle noktalar, çizgiler ve çokgenler kullanılarak temsil edilir. Bu veriler genellikle onunla ilişkili meta veri bilgilerine sahiptir. Örneğin, bir nokta bir mağazanın konumunu temsil edebilir ve o restoranla ilgili meta veriler hizmet verdiği yemeğin adı, adresi ve türü olabilir. Bu meta veriler, bu özelliklerin özellikleri `JsonObject`olarak eklenebilir. Aşağıdaki kod , "Hello World!" `title` değerine sahip bir özellik ile basit bir nokta özelliği oluşturur.

```java
//Create a data source and add it to the map.
DataSource dataSource = new DataSource();
map.sources.add(dataSource);

//Create a point feature with some properties and add it to the data source.
JsonObject properties = new JsonObject();
properties.addProperty("title", "Hello World!");

//Create a point feature, pass in the metadata properties, and add it to the data source.
dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64), properties));
```

Bir kullanıcı haritadaki bir özellik ile etkileşimde olduğunda, olaylar bu eylemlere tepki vermek için kullanılabilir. Yaygın bir senaryo, kullanıcının etkileşimde olduğu bir özelliğin meta veri özelliklerinden oluşan bir iletiyi görüntülemektir. Olay, `OnFeatureClick` kullanıcının haritadaki bir özelliği ne zaman kullandığını algılamak için kullanılan ana olaydır. Bir de `OnLongFeatureClick` olay var. `OnFeatureClick` Olayı eşmeye eklerken, sınırlamak için bir katmanın kimliğini geçirerek tek bir katmanla sınırlandırılabilir. Katman kimliği geçirilirse, haritadaki herhangi bir özelliğe dokunmak, hangi katmanda olursa olsun, bu olayı ateşler. Aşağıdaki kod, haritadaki nokta verilerini işlemek için bir sembol `OnFeatureClick` katmanı oluşturur, ardından bir olay ekler ve bu sembol katmanıyla sınırlar.

```java
//Create a symbol and add it to the map.
SymbolLayer symbolLayer = new SymbolLayer(dataSource);
map.layers.add(symbolLayer);

//Add a feature click event to the map.
map.events.add((OnFeatureClick) (features) -> {
    //Retrieve the title property of the feature as a string.
    String msg = features.get(0).properties().get("title").getAsString();

    //Do something with the message.
}, symbolLayer.getId());    //Limit this event to the symbol layer.
```

## <a name="display-a-toast-message"></a>Tost iletisi görüntüleme

Tost mesajı, bilgileri kullanıcıya görüntülemenin en kolay yollarından biridir ve Android'in tüm sürümlerinde mevcuttur. Herhangi bir kullanıcı girişi türünü desteklemez ve yalnızca kısa bir süre için görüntülenir. Kullanıcıya ne leri dinledikleri hakkında hızlı bir şekilde bilgi vermek istiyorsanız, tost iletisi iyi bir seçenek olabilir. Aşağıdaki kod, bir tost iletisinin `OnFeatureClick` olayla nasıl kullanılabileceğini gösterir.

```java
//Add a feature click event to the map.
map.events.add((OnFeatureClick) (features) -> {
    //Retrieve the title property of the feature as a string.
    String msg = features.get(0).getStringProperty("title");

    //Display a toast message with the title information.
    Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
}, symbolLayer.getId());    //Limit this event to the symbol layer.
```

<center>

![Dinlenen bir özelliğin animasyonu ve tost iletisi görüntüleniyor](./media/display-feature-information-android/symbol-layer-click-toast-message.gif)</center>

Tost iletilerine ek olarak, bir özelliğin meta veri özelliklerini sunmanın başka yolları da vardır:

- [Snakbar widget](https://developer.android.com/training/snackbar/showing.html) - Snackbars bir işlem hakkında hafif geribildirim sağlar. Bunlar, ekranın alt kısmında mobil ve daha büyük cihazlarda sol alt kısmında kısa bir mesaj gösterir. Snackbars ekranda tüm diğer öğelerin üzerinde görünür ve sadece bir seferde görüntülenebilir.
- [İletişim Bilgileri](https://developer.android.com/guide/topics/ui/dialogs) - İletişim kutusu, kullanıcıdan karar vermesini veya ek bilgi girmesini gerektiren küçük bir penceredir. Bir iletişim ekranı doldurmaz ve normalde kullanıcıların devam etmeden önce bir eylemde bulunmalarını gerektiren modal olaylar için kullanılır.
- Geçerli faaliyete bir [Parça](https://developer.android.com/guide/components/fragments) ekleyin.
- Başka bir etkinlik veya görünüme gidin.

## <a name="next-steps"></a>Sonraki adımlar

Haritanıza daha fazla veri eklemek için:

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Android haritasına şekil ekleme](how-to-add-shapes-to-android-map.md)
