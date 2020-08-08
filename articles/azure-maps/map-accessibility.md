---
title: Azure haritalar ile erişilebilir harita uygulaması oluşturma | Microsoft Azure haritaları
description: Azure haritalar 'da erişilebilirlik konuları hakkında bilgi edinin. Harita uygulamaları erişilebilir hale getirmek için hangi özelliklerin kullanılabildiğini görün ve erişilebilirlik ipuçlarını görüntüleyin.
services: azure-maps
author: rbrundritt
ms.author: richbrun
ms.date: 12/10/2019
ms.topic: conceptual
ms.service: azure-maps
manager: cpendleton
ms.openlocfilehash: ec88437a0fad3a6bd94a67a5ef5c75b3e506f9e8
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88006222"
---
# <a name="building-an-accessible-application"></a>Erişilebilir bir uygulama oluşturma

Internet kullanıcılarının %20 ' si için, erişilebilir Web uygulamalarına ihtiyacı vardır. Bu nedenle, uygulamanızın herhangi bir kullanıcının kolayca kullanabileceği şekilde tasarlandığından emin olmak önemlidir. Tamamlanacak görevler kümesi olarak erişilebilirliği düşünmek yerine, bunu genel Kullanıcı deneyiminizin bir parçası olarak düşünün. Uygulamanız daha fazla erişilebilir olduğunda, daha fazla kişi tarafından kullanılabilir. 

Harita gibi zengin etkileşimli içeriğe geldiğinde bazı yaygın erişilebilirlik konuları şunlardır:
- Web uygulamasını görmekte güçlük çeken kullanıcılar için ekran okuyucuyu destekler.
- , Fare, dokunmatik ve klavye gibi Web uygulamasıyla etkileşim kurmak ve gezinmek için birden çok yöntem vardır.
- Renk karşıtlığın, renklerin birlikte karıştırılmadığından ve birbirinden ayırt edilebilmesi için oldukça zor olduğundan emin olun. 

Azure Haritalar Web SDK 'Sı, şu gibi birçok erişilebilirlik özelliği ile önceden oluşturulmuş olarak sunulur:
- Harita ne zaman taşındığını ve Kullanıcı bir denetime veya açılan pencereye odaklanmışken ekran okuyucu açıklamaları.
- Fare, dokunmatik ve klavye desteği.
- Yol haritası stilinde erişilebilir renk karşıtlığı desteği.
- Yüksek Karşıtlık desteği.

Tüm Microsoft ürünlerine yönelik tam erişilebilirlik uyumluluğu ayrıntıları [burada](https://cloudblogs.microsoft.com/industry-blog/government/2018/09/11/accessibility-conformance-reports/)bulunabilir. Azure Haritalar Web SDK 'Sı için belgeyi özel olarak bulmak üzere "Azure Maps web" araması yapın. 

## <a name="navigating-the-map"></a>Haritada gezinme

Haritanın Yakınlaştırılıp Yakınlaştırılıp döndürülmemiş, döndürülebileceği ve açık bir şekilde birçok farklı yolu vardır. Haritada gezinmek için kullanabileceğiniz farklı yollar aşağıda verilmiştir.

**Haritayı Yakınlaştır**

- Bir fare kullanarak, bir düzey yakınlaştırmak için Haritayı çift tıklayın.
- Fareyi kullanarak Haritayı yakınlaştırmak için tekerleği kaydırın.
- Dokunmatik ekran kullanarak parmakları yakınlaştırmak veya yaymak için iki parmağınızla ve Pinç ile Haritayı dokunarak bir araya geçirin.
- Dokunmatik ekran kullanarak, bir düzey yakınlaştırmak için haritaya çift dokunun.
- Eşleme odaklı `+` olduğunda, `=` bir düzey yakınlaştırmak için artı işaretini () veya eşittir işaretini () kullanın.
- Eşleme odaklı olduğunda, bir düzey uzaklaştırmak için eksi işaretini, kısa çizgi ( `-` ) veya alt çizgi ( `_` ) kullanın.
- Yakınlaştırma denetimini fare, dokunmatik veya klavye ile kullanma/ENTER tuşları.
- Düğmeye basın ve basılı tutun ve Haritayı `Shift` yakınlaştırmak için bir alan çizmek üzere, haritada sol fare düğmesine basın ve sürükleyin.
- Çok dokunmalı bir oyun kullanarak, yakınlaştırmak veya yakınlaştırmak için iki parmağınızı yukarı doğru sürükleyin.

**Haritayı kaydır**

- Fare kullanarak haritada sol fare düğmesine basın ve herhangi bir yöne sürükleyin.
- Dokunmatik ekran kullanarak Haritayı dokunarak istediğiniz yöne sürükleyin.
- Eşleme odaklı olduğunda, Haritayı taşımak için ok tuşlarını kullanın.

**Haritayı döndürme**

- Fare kullanarak haritada sağ fare düğmesiyle basın ve sol veya sağ sürükleyin. 
- Dokunmatik ekran kullanarak Haritayı iki parmağınızla dokunarak ve döndürün.
- Eşleme odaklı olduğunda SHIFT tuşunu ve sol veya sağ ok tuşlarını kullanın.
- Döndürme denetimini fare, dokunmatik veya klavye sekmesi/ENTER tuşlarıyla kullanma.

**Haritayı sıklık**

- Fareyi kullanarak haritada sağ fare düğmesiyle aşağı basın ve yukarı veya aşağı sürükleyin. 
- Dokunmatik ekran kullanarak haritada iki parmağınızla dokunarak dokunun ve bunları birlikte yukarı veya aşağı sürükleyin.
- Eşlemle odaklanan şekilde SHIFT tuşunu ve yukarı veya aşağı ok tuşlarını kullanın. 
- Sıklık denetimini fare, dokunmatik veya klavye sekmesi/ENTER tuşları ile kullanma.

## <a name="change-the-map-style"></a>Harita stilini değiştirme

Tüm geliştiriciler, tüm olası harita stillerinin uygulamalarıyla kullanılabilmesini ister. Geliştirici haritanın stil Seçicisi denetimini görüntülerse Kullanıcı, sekme veya ENTER tuşu ile fare, dokunmatik veya klavyeyi kullanarak harita stilini değiştirebilir. Geliştirici, harita stili seçici denetiminde kullanılabilir hale getirmek istedikleri eşleme stillerini belirtebilir. Ayrıca geliştirici, harita stilini programlı bir şekilde ayarlayıp değiştirebilir.

**Yüksek karşıtlığı kullan**

- Harita denetimi yüklendiğinde, yüksek karşıtlık özelliğinin etkinleştirilip etkinleştirilmediğini ve tarayıcının bunu destekleyip desteklemediğini denetler.
- Harita denetimi, cihazın yüksek karşıtlık modunu izlemez. Cihaz modu değişirse, eşleme olmayacaktır. Bu nedenle, kullanıcının sayfayı yenileyerek Haritayı yeniden yüklemesi gerekir.
- Yüksek karşıtlık algılandığında, harita stili otomatik olarak yüksek karşıtlığa geçiş yapar ve tüm yerleşik denetimler yüksek karşıtlık stilini kullanır. Örneğin, ZoomControl, Lıchcontrol, CompassControl, StyleControl ve diğer yerleşik denetimler, yüksek karşıtlık stili kullanacaktır.
- İki tür yüksek karşıtlık, açık ve koyu. Yüksek karşıtlık türü harita denetimleri tarafından algılanıyorsa, haritanın davranışı buna uygun şekilde ayarlanacaktır. Işık ise grayscale_light harita stili yüklenir. Tür algılanamadığından veya karanlık ise high_contrast_dark stili yüklenir.
- Özel denetimler oluşturuyorsanız, yerleşik denetimlerin yüksek karşıtlık stili kullanıp kullanmediğini bilmemiz yararlı olur. Geliştiriciler, denetlemek için harita kapsayıcısı div öğesine bir CSS sınıfı ekleyebilir. Eklenecek CSS sınıfları `high-contrast-dark` ve ' dir `high-contrast-light` . JavaScript kullanmayı denetlemek için şunu kullanın:

```javascript
map.getMapContainer().classList.contains("high-contrast-dark")
```

veya şunu kullanın:

```javascript
map.getMapContainer().classList.contains("high-contrast-light")
```

## <a name="keyboard-shortcuts"></a>Klavye kısayolları

Haritada, eşlemenin kullanımını kolaylaştıran yerleşik bir dizi klavye kısayolu vardır. Bu klavye kısayolları, eşleme odağa sahip olduğunda çalışır.

| Anahtar      | Eylem                            |
|----------|-----------------------------------|
| `Tab` | Haritadaki denetimler ve açılır pencereler arasında gezinme. |
| `ESC` | Haritadaki herhangi bir öğeden odağı en üst düzey harita öğesine taşıyın. |
| `Ctrl` + `Shift` + `D` | Ekran okuyucusu ayrıntı düzeyini değiştirin.  |
| Sol ok tuşu | Haritayı sola kaydır 100 piksel |
| Sağ ok tuşu | Haritayı sağa kaydır 100 piksel |
| Aşağı ok tuşu | Haritayı aşağı kaydırın 100 piksel |
| Yukarı ok tuşu | Haritayı 100 piksel yukarı kaydır |
| `Shift`+ yukarı ok | Eşleme aralığını 10 derece artır |
| `Shift`+ aşağı ok | Eşleme aralığını 10 derece azalt |
| `Shift`+ sağ ok | Haritayı saat yönünde 15 derece döndürün |
| `Shift`+ sol ok | Haritayı saatin tersi yönde 15 derece döndürün |
| Artı işareti ( `+` ) veya <sup>*</sup> eşittir işareti ( `=` ) | Yakınlaştır |
| Eksi işareti, kısa çizgi ( `-` ) veya <sup>*</sup> alt çizgi ( `_` ) | Uzaklaştır | 
| `Shift`çizim alanına haritada fare sürükleme | Alana Yakınlaştır |

<sup>*</sup>Bu anahtar kısayollar genellikle klavyede aynı anahtarı paylaşır. Bu kısayollar, Kullanıcı deneyimini geliştirmek için eklenmiştir. Ayrıca, kullanıcının bu kısayollar için değil, Shift tuşunu kullanıp kullanmadığını önemli değildir.

## <a name="screen-reader-support"></a>Ekran okuyucu desteği

Kullanıcılar, klavyeyi kullanarak haritada gezinebilirler. Bir ekran okuyucu çalışıyorsa, eşleme kullanıcıya durumunda değişiklikleri bilgilendirir. Örneğin, eşleme değiştirildiğinde veya yakınlaştırıldığında kullanıcılara eşleme değişiklikleri bildirilir. Eşleme, varsayılan olarak haritanın merkezinin yakınlaştırma düzeyini ve koordinatlarını hariç tutarak basit açıklamalar sağlar. Kullanıcı klavye kısa kesmeyi kullanarak bu açıklamaların ayrıntı düzeyini değiştirebilir `Ctrl`  +  `Shift`  +  `D` .

Temel haritaya yerleştirilmiş ek bilgiler, ekran okuyucu kullanıcıları için ilgili metin bilgilerine sahip olmalıdır. Uygun yerlerde, [erişilebilir zengin Internet uygulamaları (ARIA)](https://www.w3.org/WAI/standards-guidelines/aria/), alt ve başlık öznitelikleri eklediğinizden emin olun. 

## <a name="make-popups-keyboard-accessible"></a>Açılır klavyeden erişilebilir yap

Bir işaretleyici veya sembol genellikle haritadaki bir konumu temsil etmek için kullanılır. Bu konum hakkında ek bilgiler genellikle Kullanıcı işaretleyiciyle etkileşime geçtiğinde bir açılan pencerede görüntülenir. Çoğu uygulamada, bir Kullanıcı bir işaretçiye tıkladığında veya dokunduğunda açılır pencere görünür. Ancak, tıklatmak ve dokunmak, kullanıcının sırasıyla bir fare ve dokunmatik ekran kullanmasını gerektirir. Klavye kullanırken açılır pencerelere erişilebilir hale getirmek iyi bir uygulamadır. Bu işlevsellik, her bir veri noktası için bir açılan pencere oluşturup haritaya eklenerek elde edilebilir. 

Aşağıdaki örnek, bir sembol katmanını kullanarak haritada ilgi çekici noktaları yükler ve her bir ilgi noktası için haritaya bir açılan pencere ekler. Her bir açılan pencerede bir başvuru, her bir veri noktasının özelliklerinde saklanır. Ayrıca, bir işaretleyicinin ne zaman tıklandığı gibi bir işaretleyici için de alınabilir. Haritada odaklandığında Tab tuşuna basmak, kullanıcının haritadaki her açılan pencerede ilerlemenize izin verir.

<br/>

<iframe height='500' scrolling='no' title='Erişilebilir bir uygulama oluşturma' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. Azure <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>Make an accessible application</a> Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) Ile <a href='https://codepen.io'>codepen</a>'da erişilebilir bir uygulama oluşturma. </iframe>

<br/>

## <a name="additional-accessibility-tips"></a>Ek erişilebilirlik ipuçları

Web eşleme uygulamanızı daha erişilebilir hale getirmek için bazı ek ipuçları aşağıda verilmiştir.

- Haritada birçok etkileşimli nokta verisi görüntülüyorsanız, dağınıklığı azaltmayı ve kümeleme kullanmayı göz önünde bulundurun. 
- Metin/semboller ve arka plan renkleri arasındaki renk karşıtlığı oranının 4.5:1 veya daha fazla olduğundan emin olun.
- Ekran okuyucunuzu (ARıA, alt ve başlık öznitelikleri), kısa, açıklayıcı ve anlamlı bir şekilde saklayın. Gereksiz öğretmek ve kısaltmalardan kaçının.
- Kullanıcının özetlemenin kolay olduğu kısa anlamlı bilgiler sağlamak için ekran okuyucusuna gönderilen iletileri iyileştirmeye çalışın. Örneğin, harita hareketli olduğu gibi yüksek bir sıklıkta ekran okuyucuyu güncelleştirmek istiyorsanız aşağıdaki noktaları yapmayı göz önünde bulundurun:
    - Ekran okuyucuyu güncelleştirmek için haritanın taşınmasını bitirene kadar bekleyin.
    - Güncelleştirmeleri birkaç saniyede bir kez daha kısıtlama. 
    - İletileri bir mantıksal şekilde birleştirin. 
- Bilgi iletmenin tek yolu olarak renk kullanmaktan kaçının. Rengi tamamlamak veya değiştirmek için metin, simgeler veya desenler kullanın. Bazı hususlar:
    - Veri noktaları arasındaki göreli değeri göstermek için bir kabarcık katmanı kullanıyorsanız, her kabarcığun yarıçapını ölçeklendirin, kabarcığun renklendirilmesini veya her ikisini de göz önünde bulundurun. 
    - Üçgenler, yıldızlar ve kareler gibi farklı ölçüm kategorileri için farklı simgelere sahip bir sembol katmanı kullanmayı düşünün. Sembol katmanı Ayrıca simgenin boyutunu ölçeklendirmeyi destekler. Metin etiketi de görüntülenebilir.
    - Çizgi verileri görüntülenmiyorsa, genişlik veya boyutu temsil etmek için Genişlik kullanılabilir. Çizgi dizisi düzeni, farklı satır kategorilerini temsil etmek için kullanılabilir. Bir sembol katmanı, çizgi üzerinde simgelerin yer aldığı bir satırla birlikte kullanılabilir. Bir ok simgesi kullanmak çizginin akışını veya yönünü göstermek için yararlıdır.
    - Çokgen verileri görüntülenmiyorsa, renk için bir alternatif olarak şerit gibi bir kalıp kullanılabilir. 
- Heatmaps, döşeme katmanları ve görüntü katmanları gibi bazı görselleştirmeler, görme engelli kullanıcılar için erişilebilir değildir. Bazı hususlar:
    - Ekran okuyucunun, haritaya eklendiğinde katmanın ne şekilde görüntülediğine ilişkin açıklamayı görüntülemesini sağlayabilirsiniz. Örneğin, bir hava durumu radar kutucuk katmanı görüntüleniyorsa ekran okuyucunuzun "Hava durumu radar verileri haritada yer alır" deyin.
- Fare üzerine gelme gerektiren işlev miktarını sınırlayın. Bu işlevlere, uygulamanızla etkileşimde bulunmak için klavye veya dokunmatik cihaz kullanan kullanıcılar erişemez. Bu durumda, tıklatılabilir simgeler, bağlantılar ve düğmeler gibi etkileşimli içerik için bir vurgulama stili olması iyi bir uygulamadır.
- Klavyeyi kullanarak uygulamanıza gezinmenize çalışın. Sekme sıralamanın mantıklı olduğundan emin olun.
- Klavye kısayolları oluşturuyorsanız, iki anahtar veya daha kısa bir süre sonra sınırlamayı deneyin. 

## <a name="next-steps"></a>Sonraki adımlar

Web SDK modüllerinde erişilebilirlik hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Çizim araçları erişilebilirliği](drawing-tools-interactions-keyboard-shortcuts.md)

Microsoft Learn ile erişilebilir uygulamalar geliştirme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Işlem dijital rozet öğrenme yolunda erişilebilirlik](https://ready.azurewebsites.net/learning/track/2940)

Aşağıdaki yararlı erişilebilirlik araçlarına göz atın:
> [!div class="nextstepaction"]
> [Erişilebilir uygulamalar geliştirme](https://developer.microsoft.com/windows/accessible-apps)

> [!div class="nextstepaction"]
> [WAı-ARIA genel bakış](https://www.w3.org/WAI/standards-guidelines/aria/)

> [!div class="nextstepaction"]
> [Web Erişilebilirlik değerlendirme aracı (dalga)](https://wave.webaim.org/)

> [!div class="nextstepaction"]
> [WebAim renk karşıtlığı denetleyicisi](https://webaim.org/resources/contrastchecker/)

> [!div class="nextstepaction"]
> [Kafevision simülatörü yok](https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl?hl=en-US)
