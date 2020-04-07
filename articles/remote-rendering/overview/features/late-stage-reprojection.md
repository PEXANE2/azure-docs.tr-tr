---
title: Geç aşama yeniden projeksiyonu
description: Geç Aşama Yeniden Projeksiyonu ve nasıl kullanılacağı hakkında bilgi.
author: sebastianpick
ms.author: sepick
ms.date: 02/04/2020
ms.topic: article
ms.openlocfilehash: 4aa1148e544ff3451aa1cb956bc4a5fb932b9611
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680992"
---
# <a name="late-stage-reprojection"></a>Geç aşama yeniden projeksiyonu

*Geç Aşama Yeniden Projeksiyonu* (LSR), kullanıcı hareket ettiğinde hologramların dengelenmesine yardımcı olan bir donanım özelliğidir.

Statik modellerin, hareket ettiğinizde konumlarını görsel olarak korumaları beklenir. Kararsız görünüyorlarsa, bu davranış LSR sorunlarına işaret edebilir. Animasyonlar veya patlama görünümleri gibi ek dinamik dönüşümlerin bu davranışı maskeleyebileceğini unutmayın.

İki farklı LSR modu arasında seçim yapabilirsiniz, yani **Düzlemsel LSR** veya **Derinlik LSR.** Hangisinin etkin olduğu, istemci başvurusunun bir derinlik arabelleği gönderip göndermediğine göre belirlenir.

Her iki LSR modu da hologram kararlılığını artırır, ancak farklı sınırlamaları vardır. Çoğu durumda daha iyi sonuçlar verdiği için Depth LSR'yi deneyerek başlayın.

## <a name="choose-lsr-mode-in-unity"></a>Unity'de LSR modunu seçin

Unity düzenleyicisinde, *Yapı Ayarları > Dosya'ya*gidin. Sol alttaki *Oyuncu Ayarlarını* seçin, ardından *Player > XR Ayarları* > Windows Mixed Reality > Derinlik **Arabelleği Paylaşımını Etkinleştir'in** denetlenip denetlenmediğini kontrol edin:

![Derinlik Arabellek Paylaşımı Etkin bayrak](./media/unity-depth-buffer-sharing-enabled.png)

Eğer varsa, uygulamanız Depth LSR kullanır, aksi takdirde Düzlemsel LSR kullanır.

## <a name="depth-lsr"></a>Derinlik LSR

Derinlik LSR çalışması için istemci uygulaması, LSR sırasında göz önünde bulundurulması gereken tüm ilgili geometriyi içeren geçerli bir derinlik arabelleği sağlaması gerekir.

Derinlik LSR, sağlanan derinlik arabelleği içeriğine göre video çerçevesini sabitlemeye çalışır. Sonuç olarak, saydam nesneler gibi ona işlenmemiş içerik LSR tarafından ayarlanamaz ve kararsızlık ve yeniden yansıtma yapıları gösterebilir.

## <a name="planar-lsr"></a>Düzlemsel LSR

Düzlemsel LSR, Derinlik LSR'ı gibi piksel başına derinlik bilgisine sahip değildir. Bunun yerine, her kareyi sağlamanız gereken bir düzleme göre tüm içeriği yeniden projelendirin.

Düzlemsel LSR, sağlanan düzleme en yakın olan nesneleri yeniden yeniden düzenler. Bir nesne ne kadar uzaksa, o kadar kararsız görünür. Derinlik LSR farklı derinliklerde nesneleri yeniden projeleme daha iyi olsa da, Düzlemsel LSR bir düzlem ile iyi hizalama içeriği için daha iyi çalışabilir.

### <a name="configure-planar-lsr-in-unity"></a>Düzlemsel LSR'yi Birlik Içinde Yapılandır

Düzlem parametreleri, her çerçeveyi sağlamanız gereken *odak noktası* `UnityEngine.XR.WSA.HolographicSettings.SetFocusPointForFrame`olarak adlandırılan bir noktadan türetilmiştir. Ayrıntılar için [Unity Focus Point API'ye](https://docs.microsoft.com/windows/mixed-reality/focus-point-in-unity) bakın. Bir odak noktası belirlemezseniz, sizin için bir geri dönüş seçilecektir. Ancak bu otomatik geri dönüş genellikle suboptimal sonuçlara yol açar.

Odak noktasını kendiniz hesaplayabilirsiniz, ancak uzaktan render ana bilgisayarı tarafından hesaplanan noktaya dayandırmak mantıklı olabilir. Bunu `RemoteManagerUnity.CurrentSession.GraphicsBinding.GetRemoteFocusPoint` elde etmek için ara. Odak noktasını ifade etmek için bir koordinat çerçevesi sağlamanız istenir. Çoğu durumda, sadece `UnityEngine.XR.WSA.WorldManager.GetNativeISpatialCoordinateSystemPtr` buradan sonucu sağlamak isteyeceksiniz.

Genellikle hem istemci hem de ana bilgisayar, istemcideki Kullanıcı Arabirimi öğeleri gibi diğer tarafın farkında olmadığı içeriği işler. Bu nedenle, uzak odak noktası ile yerel olarak hesaplanan bir birleştirme mantıklı olabilir.

Ardışık iki karede hesaplanan odak noktaları oldukça farklı olabilir. Sadece onları olduğu gibi kullanmak hologramların etrafta zıplamaya yol açabilir. Bu davranışı önlemek için, önceki ve geçerli odak noktaları arasında enterpolasyon tavsiye edilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Sunucu tarafı performans sorguları](performance-queries.md)
