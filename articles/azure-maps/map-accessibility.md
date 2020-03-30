---
title: Azure Haritalar ile erişilebilir bir harita uygulaması oluşturun | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar'ı kullanarak erişilebilirlik özelliklerine sahip bir uygulamayı nasıl oluşturabileceğinizi öğreneceksiniz.
services: azure-maps
author: rbrundritt
ms.author: richbrun
ms.date: 12/10/2019
ms.topic: conceptual
ms.service: azure-maps
manager: cpendleton
ms.openlocfilehash: 5fa9e159fa0ac76ce8c585936059fb7f3151c7c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473328"
---
# <a name="building-an-accessible-application"></a>Erişilebilir bir uygulama oluşturma

İnternet kullanıcılarının %20'den fazlasının erişilebilir web uygulamalarına ihtiyacı vardır. Bu nedenle, uygulamanızın herhangi bir kullanıcının kolayca kullanabileceği şekilde tasarlandıklarından emin olmak önemlidir. Erişilebilirliği tamamlanması gereken bir görev kümesi olarak düşünmek yerine, bunu genel kullanıcı deneyiminizin bir parçası olarak düşünün. Uygulamanız ne kadar erişilebilirse, uygulamayı o kadar çok kişi kullanabilir. 

Harita gibi zengin etkileşimli içeriğe gelince, bazı yaygın erişilebilirlik konuları şunlardır:
- Web uygulamasını görmekte güçlük çeken kullanıcılar için ekran okuyucuyu destekleyin.
- Fare, dokunma ve klavye gibi web uygulamasıyla etkileşim kurmak ve gezinmek için birden çok yönteme sahip olmak.
- Renk kontrastı, renklerin birbirine karışmadığından ve birbirinden ayırt etmek zor laşır. 

Azure Haritalar Web SDK, aşağıdakiler gibi birçok erişilebilirlik özelliğiyle önceden oluşturulmuş olarak sunulur:
- Ekran okuyucu, harita hareket ettiğinde ve kullanıcı bir denetime veya açılır pencereye odaklandığında açıklamalar sunar.
- Fare, dokunma ve klavye desteği.
- Yol haritası stilinde erişilebilir renk kontrastı desteği.

Tüm Microsoft ürünleri için tam erişilebilirlik uygunluk ayrıntılarını [burada](https://cloudblogs.microsoft.com/industry-blog/government/2018/09/11/accessibility-conformance-reports/)bulabilirsiniz. Belgeyi özellikle Azure Haritalar Web SDK'sı için bulmak için "Azure Haritalar web"i arayın. 

## <a name="navigating-the-map"></a>Haritada gezinme

Haritanın yakınlaştırılabildiği, kaydırılabildiği, döndürülebileceği ve pitched edilebildiği birkaç farklı yol vardır. Aşağıdaki ayrıntıları harita gezinmek için tüm farklı yolları.

**Haritayı yakınlaştır**

- Fareyi kullanarak, bir seviyeyi yakınlaştırmak için haritayı çift tıklatın.
- Fareyi kullanarak, haritayı yakınlaştırmak için tekerleği kaydırın.
- Dokunmatik ekran kullanarak haritaya iki parmağınızla dokunun ve yakınlaştırmak için parmaklarınızı uzaklaştırmak veya yaymak için birbirine sıkıştırın.
- Dokunmatik ekran kullanarak, bir seviyeyi yakınlaştırmak için haritaya çift dokunun.
- Harita odaklanmışken, bir seviyeyi`+`yakınlaştırmak için`=`Artı işaretini ( ) veya Eşittir işaretini kullanın.
- Harita odaklanmışken, bir seviyeyi uzaklaştırmak`-`için Eksi`_`işareti, Hyphen ( veya Alt Puan ( ) işaretini kullanın.
- Fare, dokunma veya klavye sekmesi/enter tuşları ile yakınlaştırma denetimini kullanın.
- `Shift` Düğmeye basın ve basılı tutun ve harita üzerinde sol fare düğmesine basın ve harita içine yakınlaştırmak için bir alan çizmek için sürükleyin.

**Haritayı kaydır**

- Fareyi kullanarak, haritada sol fare tuşu ile aşağı tuşuna basın ve herhangi bir yöne sürükleyin.
- Dokunmatik ekran kullanarak haritaya dokunun ve herhangi bir yöne sürükleyin.
- Harita odaklanmışken, haritayı taşımak için ok tuşlarını kullanın.

**Haritayı döndürme**

- Fareyi kullanarak, haritada sağ fare düğmesiyle aşağı doğru bastırın ve sola veya sağa sürükleyin. 
- Dokunmatik ekran kullanarak haritaya iki parmağınızla dokunun ve döndürün.
- Harita odaklanmışken, shift tuşunu ve sol veya sağ ok tuşlarını kullanın.
- Döndürme denetimini fare, dokunma veya klavye sekmesi/enter tuşları ile kullanın.

**Haritayı pitch**

- Fareyi kullanarak, haritaüzerinde sağ fare düğmesi ile aşağı tuşuna basın ve yukarı veya aşağı sürükleyin. 
- Dokunmatik ekran kullanarak haritaya iki parmağınızla dokunun ve birlikte yukarı veya aşağı sürükleyin.
- Harita odaklanmışken, shift tuşunu ve yukarı veya aşağı ok tuşlarını kullanın. 
- Bir fare, dokunmatik veya klavye sekmesi/enter tuşları ile adım denetimini kullanın.

## <a name="change-the-map-style"></a>Harita Stilini Değiştir

Tüm geliştiriciler olası tüm harita stillerinin uygulamalarında kullanılabilir olmasını istemez. Geliştirici haritanın stil seçici denetimini görüntülerse, kullanıcı fareyi, dokunuşu veya sekmeyi veya enter tuşu yla klavyeyi kullanarak harita stilini değiştirebilir. Geliştirici, harita stili seçici denetiminde hangi harita stillerini kullanılabilir hale getirmek istediklerini belirtebilir. Ayrıca, geliştirici programlı olarak ayarlayabilir ve harita stilini değiştirebilir.

**Yüksek karşıtlığı kullan**

- Eşlemi denetimi yüklendiğinde, yüksek karşıtlık etkinleştirilip etkinleştirİlmeygösterilemesini denetler ve tarayıcı bunu destekler.
- Harita denetimi aygıtın yüksek kontrast lı modunu izlemez. Aygıt modu değişirse, harita değişmez. Bu nedenle, kullanıcının sayfayı yenileyerek haritayı yeniden yüklemesi gerekir.
- Yüksek kontrast algılandığında, harita stili otomatik olarak yüksek karşıtlıka geçer ve tüm yerleşik denetimler yüksek kontrast stili kullanır. Örneğin, ZoomControl, PitchControl, CompassControl, StyleControl ve diğer yerleşik denetimler yüksek kontrast stili kullanır.
- Yüksek kontrast, açık ve koyu iki türü vardır. Yüksek karşıtlık türü harita denetimleri tarafından algılanabiliyorsa, haritanın davranışı buna göre ayarlanır. Işık varsa, grayscale_light harita stili yüklenir. Tür algılanamıyorsa veya karanlıksa, high_contrast_dark stili yüklenir.
- Özel denetimler oluşturuyorsanız, yerleşik denetimlerin yüksek karşıtlık stili ni kullanıp kullanmamasını bilmek yararlıdır. Geliştiriciler kontrol etmek için harita kapsayıcı div bir css sınıfı ekleyebilirsiniz. Eklenecek css sınıfları `high-contrast-dark` ve `high-contrast-light`. JavaScript'i kullanarak kontrol etmek için şunları kullanın:

```javascript
map.getMapContainer().classList.contains("high-contrast-dark")
```

veya, kullanın:

```javascript
map.getMapContainer().classList.contains("high-contrast-light")
```

## <a name="keyboard-shortcuts"></a>Klavye kısayolları

Harita, haritayı kullanmayı kolaylaştıran bir dizi klavye kısayol özelliğine sahiptir. Bu klavye kısayolları, harita nın odak noktası olduğunda çalışır.

| Anahtar      | Eylem                            |
|----------|-----------------------------------|
| `Tab` | Haritadaki denetimler ve açılır pencereler arasında gezinin. |
| `ESC` | Odak noktasını haritadaki herhangi bir öğeden üst düzey eş öğe öğesine taşıyın. |
| `Ctrl` + `Shift` + `D` | Ekran okuyucu ayrıntı düzeyini geçiş.  |
| Sol ok tuşu | Haritayı 100 piksel sola kaydırma |
| Sağ ok tuşu | Haritayı sağa kaydırma 100 piksel |
| Aşağı ok tuşu | Haritayı 100 piksel aşağı kaydırma |
| Yukarı ok tuşu | Haritayı 100 piksele kaydırma |
| `Shift`+ yukarı ok | Harita adımlarını 10 derece artırın |
| `Shift`+ aşağı ok | Harita adımlarını 10 derece azaltın |
| `Shift`+ sağ ok | Haritayı saat yönünde 15 derece döndürme |
| `Shift`+ sol ok | Haritayı saat yönünün tersine 15 derece döndürme |
| Artı işareti`+`( <sup>*</sup>) veya`=`Eşittir işareti ( ) | Yakınlaştır |
| Eksi işareti, Tire`-`( <sup>*</sup>),`_`veya Alt Puan ( ) | Uzaklaştır | 
| `Shift`+ alan çizmek için haritaüzerinde + fare sürükleme | Alana yakınlaştırma |

<sup>*</sup>Bu tuş kısayolları genellikle klavyede aynı tuşu paylaşır. Bu kısayollar, kullanıcı deneyimini geliştirmek için eklendi. Kullanıcının bu kısayollar için shift tuşunu kullanıp kullanmadığı da önemli değildir.

## <a name="screen-reader-support"></a>Screen Reader desteği

Kullanıcılar klavyeyi kullanarak haritada gezinebilir. Ekran okuyucu çalışıyorsa, harita kullanıcıya durumundaki değişiklikleri bildirir. Örneğin, harita kaydırıldığında veya yakınlaştırıldığında kullanıcılara harita değişiklikleri bildirilir. Varsayılan olarak, harita, haritanın merkezinin yakınlaştırma düzeyini ve koordinatlarını dışlayan basitleştirilmiş açıklamalar sağlar. Kullanıcı klavye kısa `Ctrl`  +  `Shift`  +  `D`kese sini kullanarak bu açıklamaların ayrıntı düzeyini geçişyapabilir.

Temel haritaya yerleştirilen ek bilgiler, ekran okuyucu kullanıcıları için karşılık gelen metin bilgilerine sahip olmalıdır. Erişilebilir Zengin [İnternet Uygulamaları (ARIA),](https://www.w3.org/WAI/standards-guidelines/aria/)alt ve uygun olduğunda başlık öznitelikleri eklemek için emin olun. 

## <a name="make-popups-keyboard-accessible"></a>Pop-up klavyeyi erişilebilir hale getirin

İşaretçi veya sembol genellikle haritadaki bir konumu temsil etmek için kullanılır. Kullanıcı işaretçiyle etkileşimde bulunduğunda, konumla ilgili ek bilgiler genellikle açılır pencerede görüntülenir. Çoğu uygulamada, kullanıcı bir işaretçiyi tıklattığında veya dokunduğunda açılır pencereler görünür. Ancak, tıklatma ve dokunma, kullanıcının sırasıyla bir fare ve dokunmatik ekran kullanmasını gerektirir. İyi bir uygulama, açılır pencereleri klavye kullanırken erişilebilir hale getirmektir. Bu işlevsellik, her veri noktası için bir açılır pencere oluşturarak ve eşe ekleyerek elde edilebilir. 

Aşağıdaki örnek, bir sembol katmanı kullanarak haritadaki ilgi alanlarını yükler ve ilgi çekici her nokta için haritaya bir açılır pencere ekler. Her açılır pencereiçin bir başvuru her veri noktasının özelliklerinde depolanır. Bir işaretçi tıklatıldığında olduğu gibi bir işaretçi için de alınabilir. Haritaya odaklandığında, sekme tuşuna basıldığında kullanıcının haritadaki her açılır pencereden geçmesine olanak sağlar.

<br/>

<iframe height='500' scrolling='no' title='Erişilebilir bir uygulama yapma' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. Kalem <a href='https://codepen.io'>CodePen'de</a>Azure<a href='https://codepen.io/azuremaps'>@azuremaps</a>Haritalar tarafından erişilebilir bir uygulama <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>yapın</a> ( ) </iframe>

<br/>

## <a name="additional-accessibility-tips"></a>Ek erişilebilirlik ipuçları

Web eşleme uygulamanızı daha erişilebilir hale getirmek için bazı ek ipuçları aşağıda verilmiştir.

- Haritada çok sayıda etkileşimli nokta verisi görüntüleniyorsa, dağınıklığı azaltmayı düşünün ve kümelemeyi kullanın. 
- Metin/semboller ve arka plan renkleri arasındaki renk kontrast oranının 4,5:1 veya daha fazla olduğundan emin olun.
- Ekran okuyucunuzu (ARIA, alt ve başlık öznitelikleri) iletilerinizi kısa, açıklayıcı ve anlamlı tutun. Gereksiz jargon ve kısaltmalardan kaçının.
- Kullanıcının sindirmesi kolay kısa anlamlı bilgiler sağlamak için ekran okuyucuya gönderilen iletileri en iyi duruma getirmeye çalışın. Örneğin, ekran okuyucuyu haritanın hareket halinde olduğu gibi yüksek frekansta güncelleştirmek istiyorsanız, aşağıdaki noktaları yapmayı düşünün:
    - Ekran okuyucuyu güncelleştirmek için haritanın hareket etmesi bitene kadar bekleyin.
    - Güncelleştirmeleri birkaç saniyede bir daraltın. 
    - İletileri mantıksal bir şekilde birleştirin. 
- Bilgi iletmenin tek yolu olarak renk kullanmaktan kaçının. Rengi tamamlamak veya değiştirmek için metin, simge veya desen kullanın. Bazı hususlar:
    - Veri noktaları arasındaki göreli değeri göstermek için bir kabarcık katmanı kullanıyorsanız, her kabarcığın yarıçapını ölçeklendirmeyi, kabarcığı veya her ikisini birden renklendirmeyi düşünün. 
    - Üçgenler, yıldızlar ve kareler gibi farklı metrik kategoriler için farklı simgelere sahip bir sembol katmanı kullanmayı düşünün. Sembol katmanı da simgenin boyutunu ölçekleme destekler. Metin etiketi de görüntülenebilir.
    - Satır verileri görüntüleniyorsa, genişlik ağırlığı veya boyutu temsil etmek için kullanılabilir. Tire dizisi deseni, farklı satır kategorilerini temsil etmek için kullanılabilir. Sembol katmanı, satır boyunca simgeleri kaplamak için bir çizgiyle birlikte kullanılabilir. Bir ok simgesi kullanmak, çizginin akışını veya yönünü göstermek için yararlıdır.
    - Çokgen verileri görüntüleniyorsa, çizgiler gibi bir desen, renge alternatif olarak kullanılabilir. 
- Isı haritaları, döşeme katmanları ve görüntü katmanları gibi bazı görselleştirmelere görme bozukluğu olan kullanıcılar erişemez. Bazı hususlar:
    - Ekran okuyucunun haritaya eklendiğinde katmanın ne görüntülenecek olduğunu açıklamasını sağlar. Örneğin, bir hava radarı döşeme katmanı görüntüleniyorsa, ekran okuyucuya "Hava radarı verileri haritanın üzerine konmuştur" detirin.
- Fare gezinilmesi gereken işlevsellik miktarını sınırlayın. Bu işlevlere, uygulamanızla etkileşimde kalmak için klavye veya dokunmatik aygıt kullanan kullanıcılar erişilemez. Not, tıklanabilir simgeler, bağlantılar ve düğmeler gibi etkileşimli içerik için gezinme stiline sahip olmak yine de iyi bir uygulamadır.
- Klavyeyi kullanarak uygulamanızda gezinmeyi deneyin. Sekme sıralamanın mantıklı olduğundan emin olun.
- Klavye kısayolları oluşturuyorsanız, iki veya daha az tuşla sınırlamayı deneyin. 

## <a name="next-steps"></a>Sonraki adımlar

Web SDK modüllerinde erişilebilirlik hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Çizim araçları erişilebilirlik](drawing-tools-interactions-keyboard-shortcuts.md)

Microsoft Learn ile erişilebilir uygulamalar geliştirme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Action Dijital Rozet Öğrenme Yolunda Erişilebilirlik](https://ready.azurewebsites.net/learning/track/2940)

Şu yararlı erişilebilirlik araçlarına bir göz atın:
> [!div class="nextstepaction"]
> [Erişilebilir uygulamalar geliştirme](https://developer.microsoft.com/windows/accessible-apps)

> [!div class="nextstepaction"]
> [WAI-ARIA Genel Bakış](https://www.w3.org/WAI/standards-guidelines/aria/)

> [!div class="nextstepaction"]
> [Web Erişilebilirlik Değerlendirme Aracı (WAVE)](https://wave.webaim.org/)

> [!div class="nextstepaction"]
> [WebAim renk kontrast denetleyicisi](https://webaim.org/resources/contrastchecker/)

> [!div class="nextstepaction"]
> [No Coffee Vision Simulator](https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl?hl=en-US)
