---
title: Haritaüzerinde çizim araçları etkileşim türleri ve klavye kısayolları | Microsoft Azure Haritaları
description: Microsoft Azure Haritalar Web SDK'da fare, dokunmatik ekran veya klavye kullanarak şekil çizme ve bunları nasıl edinilir?
author: rbrundritt
ms.author: richbrun
ms.date: 12/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: fb8a1e1a8c29086553500bdad2e4604d1e1ef471
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77198301"
---
# <a name="interaction-types-and-keyboard-shortcuts-in-the-drawing-tools-module"></a>Çizim araçları modülünde etkileşim türleri ve klavye kısayolları

Bu makalede, fare, dokunmatik ekran veya klavye kısayollarını kullanarak şekil çizmeve biçimleri oluşturmanın tüm farklı yolları özetlenmiştir.

Çizim yöneticisi, şekil çizmek için haritayla etkileşimkurmanın üç farklı yolunu destekler.

* `click`- Fare veya dokunma tıklatıldığında koordinatlar eklenir.
* `freehand `- Haritada fare veya dokunma sürüklendiğinde koordinatlar eklenir.
* `hybrid`- Fare veya dokunma tıklatıldığında veya sürüklendiğinde koordinatlar eklenir.

## <a name="how-to-draw-shapes"></a>Şekiller nasıl çizilir?

 Herhangi bir şekil çizilmeden `drawingMode` önce, çizim yöneticisinin seçeneğini desteklenen çizim ayarına ayarlayın. Bu ayar, araç çubuğundaki çizim düğmelerinden birine basılarak programlanabilir veya çağrılabilir. Çizim modu, bir şekil çizildikten sonra bile etkin kalır ve bu da aynı türden ek şekiller çizmeyi kolaylaştırır. Çizim modunu programlı olarak boşta duruma ayarlayın. Veya araç çubuğundaki geçerli çizim modları düğmesini tıklatarak boşta duruma geçin.

Sonraki bölümlerde, şekillerin haritada çizilebildiği tüm farklı yollar sıralanabilir.

### <a name="how-to-draw-a-point"></a>Bir nokta nasıl çizilir?

Çizim yöneticisi `draw-point` çizim modundayken, haritaüzerinde puan çizmek için aşağıdaki işlemler yapılabilir. Bu yöntemler tüm etkileşim modları ile çalışır.

**Çizime başla**
 - Sol fare düğmesini tıklatın veya haritaya bir nokta eklemek için haritaya dokunun. 
 - Fare haritanın üzerindeyse, tuşa `F` basın ve fare işaretçisinin koordinatına bir nokta eklenir. Bu yöntem, haritaya bir nokta eklemek için daha yüksek doğruluk sağlar. Sol fare düğmesinin basma hareketi nedeniyle fareüzerinde daha az hareket olacaktır.
 - Haritaya daha fazla nokta `F` eklemek için tıklatmaya, dokunmaya veya bastırmaya devam edin.
 
**Çizimi bitir**
 - Çizim araç çubuğundaki herhangi bir düğmeye tıklayın. 
 - Çizim modunu programlı olarak ayarlayın. 
 - Tuşa `C` basın.

**Çizimi iptal etme**
 - Tuşa `Escape` basın.

### <a name="how-to-draw-a-line"></a>Nasıl bir çizgi çizmek için

Çizim yöneticisi moddayken, `draw-line` etkileşim moduna bağlı olarak haritaüzerinde puan çizmek için aşağıdaki işlemler yapılabilir.

**Çizime başla**
 - Tıklama modu
   * Sol fare düğmesini tıklatın veya haritadaki bir satırın her noktasını eklemek için haritaya dokunun. Her tıklama veya dokunma için satıra bir koordinat eklenir. 
   * Fare haritanın üzerindeyse, tuşa `F` basın ve fare işaretçisinin koordinatına bir nokta eklenir. Bu yöntem, haritaya bir nokta eklemek için daha yüksek doğruluk sağlar. Sol fare düğmesinin basma hareketi nedeniyle fareüzerinde daha az hareket olacaktır.
   * Satıra istenen tüm noktalar eklenene kadar tıklatmaya devam edin.
 - Serbest el modu
   * Sol fare düğmesine basın veya haritaya dokunup fareyi sürükleyin veya dokunma noktasına dönün. Fare veya dokunma noktası harita nın etrafında hareket ettikçe koordinatlar çizgiye eklenir. Fare veya rötuş olayı tetiklenir tetiklenmez çizim tamamlanır. Koordinatların eklenme sıklığı çizim yöneticileri `freehandInterval` seçeneği ile tanımlanır.
 - Karma mod
   * Tek bir çizgi çizerken, istediğiniz gibi tıklatma ve serbest vuruş yöntemleri arasında geçiş. Örneğin, birkaç noktayı tıklatın, ardından bir demet nokta eklemek için fareyi basılı tutun ve sürükleyin, ardından birkaç noktayı tıklatın. 

**Çizimi bitir**
 - Karma/Tıklatma modu
   * Son noktada haritayı çift tıklatın. 
   * Çizim araç çubuğundaki herhangi bir düğmeye tıklayın. 
   * Çizim modunu programlı olarak ayarlayın. 
 - Serbest el modu
   * Fare düğmesini veya dokunma noktasını bırakın.
 - Tuşa `C` basın.

**Çizimi iptal etme**
 - Tuşa `Escape` basın.

### <a name="how-to-draw-a-polygon"></a>Çokgen nasıl çizilir?

Çizim yöneticisi moddayken, `draw-polygon` etkileşim moduna bağlı olarak haritaüzerinde puan çizmek için aşağıdaki işlemler yapılabilir.

**Çizime başla**
 - Tıklama modu
   * Sol fare düğmesini tıklatın veya haritaya çokgenin her noktasını eklemek için haritaya dokunun. Her tıklama veya dokunma için çokgen için bir koordinat eklenir. 
   * Fare haritanın üzerindeyse, tuşa `F` basın ve fare işaretçisinin koordinatına bir nokta eklenir. Bu yöntem, haritaya bir nokta eklemek için daha yüksek doğruluk sağlar. Sol fare düğmesinin basma hareketi nedeniyle fareüzerinde daha az hareket olacaktır.
   * Çokgenin tüm istenilen noktaları eklenene kadar tıklatmaya devam edin.
 - Serbest el modu
   * Sol fare düğmesine basın veya haritaya dokunup fareyi sürükleyin veya dokunma noktasına dönün. Fare veya dokunma noktası harita nın etrafında hareket ettikçe koordinatlar çokgenin yanına eklenir. Fare veya rötuş olayı tetiklenir tetiklenmez çizim tamamlanır. Koordinatların eklenme sıklığı çizim yöneticileri `freehandInterval` seçeneği ile tanımlanır.
 - Karma mod
   * Tek bir çokgen çizerken, istediğiniz gibi, tıklama ve serbest yöntemler arasında geçiş. Örneğin, birkaç noktayı tıklatın, ardından bir demet nokta eklemek için fareyi basılı tutun ve sürükleyin, ardından birkaç noktayı tıklatın. 

**Çizimi bitir**
 - Karma/Tıklatma modu
   * Son noktada haritayı çift tıklatın. 
   * Çokgenin ilk noktasına tıklayın.
   * Çizim araç çubuğundaki herhangi bir düğmeye tıklayın. 
   * Çizim modunu programlı olarak ayarlayın. 
 - Serbest el modu
   * Fare düğmesini veya dokunma noktasını bırakın.
 - Tuşa `C` basın.

**Çizimi iptal etme**
 - Tuşa `Escape` basın.

### <a name="how-to-draw-a-rectangle"></a>Dikdörtgen nasıl çizilir?

Çizim yöneticisi moddayken, `draw-rectangle` etkileşim moduna bağlı olarak haritaüzerinde puan çizmek için aşağıdaki işlemler yapılabilir. Oluşturulan şekil [dikdörtgenler için genişletilmiş GeoJSON belirtimi](extend-geojson.md#rectangle)takip edecektir.

**Çizime başla**
 - Dikdörtgenin ilk köşesini eklemek için sol fare düğmesine veya haritaya dokunarak aşağı doğru bastırın ve dikdörtgeni oluşturmak için sürükleyin. 

**Çizimi bitir**
 - Fare düğmesini veya dokunma noktasını bırakın.
 - Çizim modunu programlı olarak ayarlayın. 
 - Tuşa `C` basın.

**Çizimi iptal etme**
 - Tuşa `Escape` basın.

### <a name="how-to-draw-a-circle"></a>Nasıl bir daire çizmek için

Çizim yöneticisi moddayken, `draw-circle` etkileşim moduna bağlı olarak haritaüzerinde puan çizmek için aşağıdaki işlemler yapılabilir. Oluşturulan şekil [çevreler için genişletilmiş GeoJSON belirtimi](extend-geojson.md#circle)takip edecektir.

**Çizime başla**
 - Dairenin merkezini eklemek ve daireleri sürüklemek için sol fare düğmesine veya haritaya dokunarak aşağı doğru bastırın. 

**Çizimi bitir**
 - Fare düğmesini veya dokunma noktasını bırakın.
 - Çizim modunu programlı olarak ayarlayın. 
 - Tuşa `C` basın.

**Çizimi iptal etme**
 - Tuşa `Escape` basın.

## <a name="keyboard-shortcuts"></a>Klavye kısayolları

Çizim araçları klavye kısayollarını destekler. Bu klavye kısayolları, harita odaklandığında işlevseldir.

| Anahtar      | Eylem                            |
|----------|-----------------------------------|
| `C` | Devam eden herhangi bir çizimi tamamlar ve çizim modunu boşta ayarlar. Odak üst düzey harita öğesine taşınır.  |
| `Escape` | Devam eden herhangi bir çizimi iptal eder ve çizim modunu boşta ayarlar. Odak üst düzey harita öğesine taşınır.  |
| `F` | Fare haritanın üzerindeyse bir noktaya, çizgiye veya çokgenlere koordinat ekler. Tıklatma veya karma modda haritayı tıklatma eşdeğer eylem. Bu kısayol daha hassas ve daha hızlı çizimler sağlar. Fareyi konumlandırmak için bir elinizi ve diğerini tuşuna basma hareketinden hareket ettirmeden düğmeye basmak için kullanabilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar

Çizim araçları modülündeki sınıflar hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Çizim yöneticisi](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Çizim araç çubuğu](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)
