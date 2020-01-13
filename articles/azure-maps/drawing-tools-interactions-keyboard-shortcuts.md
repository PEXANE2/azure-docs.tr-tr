---
title: Harita araçları etkileşim türleri ve klavye kısayolları Microsoft Azure haritaları
description: Microsoft Azure Maps web SDK 'sında fare, dokunmatik ekran veya klavye kullanarak şekil çizme ve düzenleme
author: rbrundritt
ms.author: richbrun
ms.date: 12/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 267579f7b4bbfe026f3aa01b00f01f3b872cf4a6
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911648"
---
# <a name="interaction-types-and-keyboard-shortcuts-in-the-drawing-tools-module"></a>Çizim araçları modülündeki etkileşim türleri ve klavye kısayolları

Bu makalede, fare, dokunmatik ekran veya klavye kısayolları kullanarak haritada şekil çizme ve düzenleme için kullanabileceğiniz farklı yollar özetlenmektedir.

Çizim Yöneticisi, şekil çizmek için harita ile etkileşimde bulunmanın üç farklı yolunu destekler.

* `click` koordinatları, fare veya dokunma tıklandığında eklenir.
* `freehand ` koordinatları, fare veya dokunma haritada sürüklendiğinde eklenir. 
* `hybrid` koordinatları, fare veya dokunma tıklandığında veya sürüklendiğinde eklenir.

## <a name="how-to-draw-shapes"></a>Şekil çizme

Aşağıda şekillerin haritada çizileceği farklı yolları özetlenmektedir. Herhangi bir şekil çizilebilmeniz için, çizim yöneticisinin `drawingMode` seçeneğinin desteklenen bir çizim ayarına ayarlanması gerekir. Bu, programlı bir şekilde veya araç çubuğundaki çizim düğmelerinden birine basılarak yapılabilir. Çizim modu, bir şekil çizildikten sonra bile etkin kalır, ancak aynı türde ek şekiller çizmeyi kolaylaştırır. Çizim modu, programlı olarak bir boş duruma veya araç çubuğundaki geçerli çizim modları düğmesine tıklanarak yerleştirilebilir. 

### <a name="how-to-draw-a-point"></a>Nokta çizme

Çizim Yöneticisi `draw-point` çizim modundayken, haritada noktaları çizmek için aşağıdaki eylemler yapılabilir. Bu yöntemler tüm etkileşim modlarıyla çalışır.

**Çizimi Başlat**
 - Haritaya bir nokta eklemek için sol fare düğmesine tıklayın veya haritaya dokunun. 
 - Fare haritanın üzerindeyken, `F` tuşuna basın ve fare işaretçisinin nerede olduğu koordinat kullanılarak bir nokta eklenir. Bu, sol fare düğmesinin düğmesine basılması nedeniyle fare üzerinde daha az hareket olacağı için haritaya nokta eklemek için daha yüksek doğruluk yöntemi sağlar.
 - Haritaya daha fazla işaret eklemek için `F` tıklamaya, dokunmaya veya basmaya devam edin.
 
**Çizimi tamamlama**
 - Çizim araç çubuğunda herhangi bir düğmeye tıklayın. 
 - Çizim modunu programlı olarak ayarlayın. 
 - `C` tuşuna basın.

**Çizimi iptal et**
 - `Escape` tuşuna basın.

### <a name="how-to-draw-a-line"></a>Çizgi çizme

Çizim Yöneticisi `draw-line` modundayken, etkileşim modunun ne şekilde ayarlandığına bağlı olarak haritada noktaları çizmek için aşağıdaki eylemler yapılabilir.

**Çizimi Başlat**
 - Tıklama modu
   * Haritada bir çizginin her noktasını eklemek için sol fare düğmesine tıklayın veya haritaya dokunun. Her tıklama/dokunmatik için satıra bir koordinat eklenir. 
   * Fare haritanın üzerindeyken, `F` tuşuna basın ve fare işaretçisinin nerede olduğu koordinat kullanılarak bir nokta eklenir. Bu, sol fare düğmesinin düğmesine basılması nedeniyle fare üzerinde daha az hareket olacağı için haritaya nokta eklemek için daha yüksek doğruluk yöntemi sağlar.
   * İstenen tüm noktaların satıra eklenene kadar tıklamasına devam edin.
 - FreeHand modu
   * Sol fare düğmesine basın veya haritada aşağı dokunun, fare veya dokunma noktasını etrafında sürükleyin. Fare veya dokunma noktası haritanın etrafında hareket ederken, koordinatlar çizgiye eklenir. Fare veya dokunma olayı tetiklendikten hemen sonra çizim tamamlanır. Koordinatların Eklenme sıklığı, çizim yöneticileri `freehandInterval` seçeneği tarafından tanımlanır.
 - Karma mod
   * Tek bir çizgi çizerken istediğiniz gibi tıklama ve FreeHand yöntemleri arasında alternatif. Örneğin, birkaç noktaya tıklayın, sonra da bir dizi noktayı eklemek için fareyi basılı tutup sürükleyin ve daha sonra birkaç kez tıklayın. 

**Çizimi tamamlama**
 - Karma/tıklama modu
   * Son noktada haritaya çift tıklayın. 
   * Çizim araç çubuğunda herhangi bir düğmeye tıklayın. 
   * Çizim modunu programlı olarak ayarlayın. 
 - FreeHand modu
   * Fare düğmesini veya dokunma noktasını serbest bırakın.
 - `C` tuşuna basın.

**Çizimi iptal et**
 - `Escape` tuşuna basın.

### <a name="how-to-draw-a-polygon"></a>Çokgen çizme

Çizim Yöneticisi `draw-polygon` modundayken, etkileşim modunun ne şekilde ayarlandığına bağlı olarak haritada noktaları çizmek için aşağıdaki eylemler yapılabilir.

**Çizimi Başlat**
 - Tıklama modu
   * Haritada bir çokgenin her noktasını eklemek için sol fare düğmesine tıklayın veya haritaya dokunun. Her tıklama/dokunmatik için çokgene bir koordinat eklenir. 
   * Fare haritanın üzerindeyken, `F` tuşuna basın ve fare işaretçisinin nerede olduğu koordinat kullanılarak bir nokta eklenir. Bu, sol fare düğmesinin düğmesine basılması nedeniyle fare üzerinde daha az hareket olacağı için haritaya nokta eklemek için daha yüksek doğruluk yöntemi sağlar.
   * Tüm istenen noktaları Çokgen 'e eklenene kadar tıklamasına devam edin.
 - FreeHand modu
   * Sol fare düğmesine basın veya haritada aşağı dokunun, fare veya dokunma noktasını etrafında sürükleyin. Fare veya dokunma noktası haritanın etrafında hareket ettirildiğinde, koordinatlar Çokgen 'e eklenir. Fare veya dokunma olayı tetiklendikten hemen sonra çizim tamamlanır. Koordinatların eklendiği sıklık 'ın, çizim yöneticileri `freehandInterval` seçeneği tarafından tanımlandığını unutmayın.
 - Karma mod
   * Tek bir çokgen çizerken istediğiniz gibi tıklama ve FreeHand yöntemleri arasında alternatif. Örneğin, birkaç noktaya tıklayın, sonra da bir dizi noktayı eklemek için fareyi basılı tutup sürükleyin ve daha sonra birkaç kez tıklayın. 

**Çizimi tamamlama**
 - Karma/tıklama modu
   * Son noktada haritaya çift tıklayın. 
   * Çokgendeki ilk noktaya tıklayın.
   * Çizim araç çubuğunda herhangi bir düğmeye tıklayın. 
   * Çizim modunu programlı olarak ayarlayın. 
 - FreeHand modu
   * Fare düğmesini veya dokunma noktasını serbest bırakın.
 - `C` tuşuna basın.

**Çizimi iptal et**
 - `Escape` tuşuna basın.

### <a name="how-to-draw-a-rectangle"></a>Dikdörtgen çizme

Çizim Yöneticisi `draw-rectangle` modundayken, etkileşim modunun ne şekilde ayarlandığına bağlı olarak haritada noktaları çizmek için aşağıdaki eylemler yapılabilir. Oluşturulan Şekil, [dikdörtgenler için genişletilmiş GeoJSON belirtimine](extend-geojson.md#rectangle)uyar.

**Çizimi Başlat**
 - Dikdörtgenin ilk köşesini eklemek için sol fare düğmesine veya haritada dokunma düğmesine basın ve dikdörtgeni oluşturmak için sürükleyin. 

**Çizimi tamamlama**
 - Fare düğmesini veya dokunma noktasını serbest bırakın.
 - Çizim modunu programlı olarak ayarlayın. 
 - `C` tuşuna basın.

**Çizimi iptal et**
 - `Escape` tuşuna basın.

### <a name="how-to-draw-a-circle"></a>Daire çizme

Çizim Yöneticisi `draw-circle` modundayken, etkileşim modunun ne şekilde ayarlandığına bağlı olarak haritada noktaları çizmek için aşağıdaki eylemler yapılabilir. Oluşturulan Şekil, [daireler için genişletilmiş GeoJSON belirtimine](extend-geojson.md#circle)uyar.

**Çizimi Başlat**
 - Dairenin merkezini eklemek için sol fare düğmesine veya haritada dokunma tuşlarına basın ve ardından daire bir yarıçap verir. 

**Çizimi tamamlama**
 - Fare düğmesini veya dokunma noktasını serbest bırakın.
 - Çizim modunu programlı olarak ayarlayın. 
 - `C` tuşuna basın.

**Çizimi iptal et**
 - `Escape` tuşuna basın.

## <a name="keyboard-shortcuts"></a>Klavye kısayolları

Çizim araçları, haritada şekil çizmeyi ve düzenlemeyi kolaylaştıran klavye kısayollarını destekler. Bu klavye kısayolları, eşleme odağa sahip olduğunda işlevseldir.

| Anahtar      | Eylem                            |
|----------|-----------------------------------|
| `C` | Sürmekte olan tüm çizimleri tamamlar ve çizim modunu boşta olarak ayarlar. Odak, en üst düzey harita öğesine taşınacak.  |
| `Escape` | Sürmekte olan tüm çizimleri iptal eder ve çizim modunu boşta olarak ayarlar. Odak, en üst düzey harita öğesine taşınacak.  |
| `F` | Fare haritanın üzerindeyken bir noktaya, çizgiye veya poligbir koordinat ekler. Tıklama veya karma moddaki haritaya tıklanması için denk eylem. Bu kısayol, fareyi ve diğerini konumlandırmak için bir yandan, fare tuşuna basarak fareyi basılı hareket etmeden düğmeye basarak daha kesin ve daha hızlı çizimler sağlar. |

## <a name="next-steps"></a>Sonraki adımlar

Çizim araçları modülündeki sınıflar hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Çizim Yöneticisi](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Çizim araç çubuğu](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)