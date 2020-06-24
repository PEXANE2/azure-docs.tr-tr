---
title: Azure Kinect DK derinlik Kamerası
description: Azure Kinect DK 'de derinlik kameranın çalışma ilkelerini ve temel özelliklerini anlayın.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, algılayıcı, SDK, derinlik Kamerası, TOF, ilkeler, performans, ınvalidation
ms.openlocfilehash: 22f04b983ed7c6a2ab19a5c1c709621655ee31c0
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "85277667"
---
# <a name="azure-kinect-dk-depth-camera"></a>Azure Kinect DK derinlik Kamerası

Bu sayfada, Azure Kinect DK 'de derinlik kamerasının nasıl kullanılacağı ele alınmaktadır. Derinlik Kamerası, iki kamerasının ikincisi olur. Önceki bölümlerde de belirtildiği gibi, diğer kamera ise RGB kameradır.  

## <a name="operating-principles"></a>İşletim ilkeleri

Azure Kinect DK derinliği Kamerası, genliği modüle edilmiş sürekli dalga (AMCW) uçuş zamanı (ToF) ilkesini uygular. Kamera, neredeyse IR (NıR) spekine, sahneye göre modüle edilmiş aydınlatmayı yayınlar. Daha sonra, ışığın kameradan sahneye ve geriye doğru ilerleme zamanının dolaylı bir ölçümünü kaydeder.

Bu ölçümler, bir derinlik eşlemesi oluşturmak için işlenir. Derinlik eşleme, her piksel için, milimetre cinsinden ölçülen Z koordinatı değerleri kümesidir.

Bir derinlik eşlemesiyle birlikte, temizleme IR okuma adlı bir de elde ediyoruz. Temiz IR okuma içindeki piksellerin değeri, sahneye döndürülen ışık miktarıyla orantılıdır. Görüntü, normal bir IR görüntüsüne benzer şekilde görünür. Aşağıdaki şekilde bir örnek derinlik Haritası (sol) ve buna karşılık gelen temiz IR görüntüsü (sağ) gösterilmektedir.

![Derinlik ve IR yan yana](./media/concepts/depth-camera-depth-ir.png)

## <a name="key-features"></a>Önemli özellikler

Derinlik kameranın teknik özellikleri şunlardır:

- daha yüksek modülasyonu ve derinlik hassasiyetini sağlayan gelişmiş piksel teknolojisine sahip 1 megapel ToF görüntüleme yongası.
- Neredeyse ve geniş bir görünüm alanı (FoV) derinliği modlarını etkinleştiren iki NıR lazer dides.
- Dünyanın en küçük ToF pikseli, 3,5 μm ile 3,5 μm.
- Piksel başına otomatik seçeneği, yakın ve uzak nesnelerin düzgün yakalanmasına izin veren büyük dinamik aralığı etkinleştirir.
- Güneş ışığı içinde iyileştirilmiş performans sağlayan küresel perde.
- Yonga, lazer ve güç kaynağı varyasyonuna bile sağlam doğruluk sağlayan çok aşamalı derinlik hesaplama yöntemi.
- Düşük Systematik ve rastgele hatalar.

![Derinlik modülü](./media/concepts/depth-camera-depth-module.jpg)

Derinlik Kamerası, ham modüle edilmiş IR görüntülerini ana BILGISAYARA iletir. BILGISAYARDA, GPU hızlandırılmış derinlik altyapısı yazılımı, Ham sinyali derinlik haritalarına dönüştürür.Derinlik Kamerası, çeşitli modları destekler. **Görünüm (FoV) modlarının dar alanı** X ve Y boyutlarındaki daha küçük kapsamlar, ancak Z boyutunda daha büyük kapsamlar içeren sahneler için idealdir. Sahnenin büyük X ve Y kapsamları varsa, ancak daha küçük bir Z aralığı varsa, **geniş FoV modları** daha uygundur.

Derinlik Kamerası, uygun **olmayan modlar**Ile Karşılaştırılacak Z aralığını genişletmek için **2x2 özellikli modlarını** destekler. Atma işlemi, görüntü çözünürlüğünü düşürmek için yapılır. Tüm modlar, en fazla 15 fps kare hızında çalışan 1 megapel (MP) modu dışında, saniyede 30 ' a kadar kare (fps) ile çalıştırılabilir. Derinlik Kamerası Ayrıca **Pasif BIR IR modu**sağlar. Bu modda, kameradaki aydınlatma etkin değildir ve yalnızca çevresel aydınlatma izlenir.

## <a name="camera-performance"></a>Kamera performansı

Kameranın performansı Systematik ve rastgele hatalar olarak ölçülür.

### <a name="systematic-error"></a>Sistematik hata

Sistematik hata, gürültü kaldırma sonrasında ölçülen derinlik ve doğru (taban gerçeği) derinliği arasındaki fark olarak tanımlanır. Derinlik gürültüsünü olabildiğince ortadan kaldırmak için bir statik sahnenin birçok karede zamana bağlı ortalamayı hesapladık. Daha kesin olarak, sistematik hata şu şekilde tanımlanır:

![Derinlik sistematik hata](./media/concepts/depth-camera-systematic-error.png)

Burada *d<sub>t</sub> * , *t*zamanında ölçü derinliğini, *N* ise ortalama yordamda kullanılan çerçeve sayısını ve *d<sub>gt</sub> * 'nin taban bir derinlik olduğunu gösterir.

Derinlik kameranın sistematik hata belirtimi çok yollu girişim 'yi (MPı) dışlanıyor. MPı, bir algılayıcı pikselin birden fazla nesne tarafından yansıtılan ışığı tümleştirtiği bir. MPı daha sonra daha sonra açıklayacağımızı derinlemesine bir şekilde daha yüksek modülasyon sıklıklarıyla birlikte derinlemesine bir şekilde azalmaktadır.

### <a name="random-error"></a>Rastgele hata

Kamerayı taşımadan aynı nesnenin 100 görüntüsünü ele alalım. Nesnenin derinliği, 100 görüntülerde her birinde biraz farklı olacaktır. Bu fark, görüntü paraziti nedeniyle oluşur. Görüntü paraziti, sensörin zaman içinde rastgele bir faktöre göre değişiklik gösterdiği photons sayısıdır. Bu rastgele hatayı, bir statik sahnede şu şekilde hesaplanan zaman içindeki derinliğin standart sapması olarak tanımladık:

![Derinlik rastgele hatası](./media/concepts/depth-camera-random-error.png)

Burada *N* , *derinlik ölçümlerinin sayısını belirtir. d<sub>t</sub>* *<sub>t</sub> * , *t* ve *d* tüm derinlik ölçümlerine göre hesaplanan ortalama değeri gösterir.

## <a name="invalidation"></a>Geçersiz kılma

Bazı durumlarda, derinlik kamerası bazı pikseller için doğru değerler sunmayabilir. Bu durumlarda derinlik pikselleri geçersiz kılınır. Geçersiz pikseller, Derinlik değeri 0 ' a eşit olarak belirtilir. Derinlik altyapısının doğru değerleri üretmesinin nedenleri şunlardır:

- Etkin IR aydınlatma maskesinin dışında
- Doygun IR sinyali
- Düşük IR sinyali
- Aykırı değerleri filtrele
- Çoklu yol engelleme

### <a name="illumination-mask"></a>Aydınlatma maskesi

Etkin IR aydınlatma maskesinin dışında olduklarında pikseller geçersiz kılınır. Bu tür piksellerin sinyal işlem derinliğine kullanılması önerilmez. Aşağıdaki şekilde, aydınlatmayla maske tarafından geçersiz kılma örneği gösterilmektedir. Geçersiz kılınan pikseller, geniş FoV modlarında (sol) dairenin dışındaki siyah renk piksellerdir ve dar FoV modlarında altıon (sağ).

![Aydınlatma maskesi dışında geçersiz kılma](./media/concepts/depth-camera-invalidation-illumination-mask.png)

### <a name="signal-strength"></a>Sinyal gücü

Doygun bir IR sinyali içerdiğinde piksel geçersiz kılınır. Pikseller doygun olduğunda, aşama bilgileri kaybolur. Aşağıdaki görüntüde, doygun bir IR sinyaliyle geçersiz kılma örneği gösterilmektedir. Hem derinlik hem de IR görüntülerinde örnek piksellere işaret eden oklara bakın.

![Geçersiz kılma doygunluğu](./media/concepts/depth-camera-invalidation-saturation.png)

IR sinyali, derinlik oluşturmak için yeterince güçlü olmadığında de geçersiz kılma yapılabilir. Aşağıdaki şekilde, düşük bir IR sinyaliyle geçersiz kılma örneği gösterilmektedir. Hem derinlik hem de IR görüntülerinde örnek piksellere işaret eden oklara bakın.

![Düşük sinyal ınvalidation](./media/concepts/depth-camera-invalidation-low-signal.png)

### <a name="ambiguous-depth"></a>Belirsiz derinlik

Ayrıca, sahnede birden fazla nesneden sinyal almış olmaları durumunda pikseller geçersiz kılınabilir. Bu tür bir geçersiz kılma, görünebileceği yaygın bir durumdur.  Sahne geometrisi nedeniyle, kameradan gelen IR ışıkları bir duvardan diğerine ve diğerine yansıtılmıştır. Yansıtılan ışık, pikselin ölçülen derinliğinde belirsizlik oluşmasına neden olur. Derinlik algoritmasındaki filtreler bu belirsiz sinyalleri algılar ve pikselleri geçersiz kılar.

Aşağıdaki şekiller, çok yollu algılama ile geçersiz kılma örneklerini gösterir. Ayrıca, bir kamera görünümünden (üst satır) geçersiz kılınan yüzey alanının farklı bir kamera görünümünden (alt satır) yeniden belirebilir. Bu görüntü, bir perspektiften geçersiz kılınan yüzeyler diğerinden görünebilir olduğunu gösterir.

![Invalidation çok yollu-köşe](./media/concepts/depth-camera-invalidation-multipath.png)

Çok yollu bir diğer yaygın durum, ön plan ve arka planda (nesne kenarları etrafında) karışık sinyal içeren pikseldir. Hızlı hareket sırasında kenarlar etrafında daha fazla geçersiz kılınan piksel görebilirsiniz. Ek geçersiz kılınan pikseller, ham derinlik yakalamanın etkilenme aralığı nedeniyle oluşur.

![Invalidation çok yollu kenarları](./media/concepts/depth-camera-invalidation-edge.png)

## <a name="next-steps"></a>Sonraki adımlar

[Koordinat sistemleri](coordinate-systems.md)
