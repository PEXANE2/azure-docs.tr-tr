---
title: Azure haritalar Power BI görselindeki katmanları anlama | Microsoft Azure haritaları
description: Bu makalede, Power BI için Microsoft Azure harita görsellerinde bulunan farklı katmanlar hakkında bilgi edineceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 06/26/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: a5c7296a0e7b7f6ea33c1f4a669675efd90f9e9a
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86262102"
---
# <a name="understanding-layers-in-the-azure-maps-power-bi-visual"></a>Azure haritalar Power BI görselindeki katmanları anlama

Azure haritalar görselindeki iki tür katman mevcuttur. İlk tür, görselin **alanlar** bölmesine geçirilen verileri işlemeye odaklanır ve aşağıdaki katmanlardan oluşur, bu veri işleme katmanlarını çağıralim.

:::row:::
    :::column span="":::
        **Kabarcık katmanı**

        Noktaları haritada ölçeklendirilen daireler olarak işler.

        ![Haritada kabarcık katmanı](media/power-bi-visual/bubble-layer-thumb.png)
    :::column-end:::
    :::column span="":::
        **Çubuk grafik katmanı**

        Noktaları haritada 3B çubuklar olarak işler.
        
        ![Harita üzerinde çubuk grafik katmanı](media/power-bi-visual/bar-chart-layer-thumb.png)
    :::column-end:::
:::row-end:::

İkinci katman türü, daha fazla bağlam sağlamak ve aşağıdaki katmanlardan oluşan, eşlenecek dış veri kaynaklarına ek olarak bağlanır.

:::row:::
    :::column span="":::
        **Başvuru katmanı**

        Karşıya yüklenen bir GeoJSON dosyasını haritanın üzerine kaplaın.

        ![Haritada başvuru katmanı](media/power-bi-visual/reference-layer-thumb.png)
    :::column-end:::
    :::column span="":::
        **Döşeme katmanı**

        Haritanın üstünde özel döşeme katmanını kaplama.
        
        ![Haritada döşeme katmanı](media/power-bi-visual/tile-layer-thumb.png)
    :::column-end:::
    :::column span="":::
        **Trafik katmanı**

        Haritadaki gerçek zamanlı trafik bilgilerini kaplama.
        
        ![Haritadaki trafik katmanı](media/power-bi-visual/traffic-layer-thumb.png)
    :::column-end:::
:::row-end:::

Tüm veri işleme katmanları ve **döşeme katmanı**, yakınlaştırma düzeyi aralığını belirtmek için kullanılan minimum ve maksimum yakınlaştırma düzeylerine yönelik seçeneklere sahiptir ve bu katmanların ' de gösterilmesi gerekir. Bu, bir tür işleme katmanının tek bir yakınlaştırma düzeyinde kullanılmasına ve başka bir işleme katmanına başka bir yakınlaştırma düzeyinde geçiş yapmasına olanak tanır.

Bu katmanlarda Ayrıca haritadaki diğer katmanlara göre konumlandırılmış bir seçenek de vardır. Birden çok veri işleme katmanı kullanıldığında, haritada eklendiği sıra, aynı **Katman konumu** değerine sahip olduklarında göreli katmanlama sırasını belirler.

## <a name="general-layer-settings"></a>Genel katman ayarları

**Biçim** bölmesinin genel katman bölümü, **alanlar** bölmesinde (kabarcık katmanı, çubuk grafik) Power BI veri kümesine bağlı olan katmanlara uygulanan ortak ayarlardır.

| Ayar     | Açıklama   |
|-------------|---------------|
| Seçilmemiş saydamlık | Bir veya daha fazla şekil seçildiğinde, seçili olmayan şekillerin saydamlığı.  |
| Sıfırları göster              | Boyut değeri sıfır olan noktaların en düşük yarıçap kullanılarak haritada gösterilip gösterilmeyeceğini belirtir. |
| Negatifleri göster          | Negatif boyut değerlerinin mutlak değerinin çizilmek gerekip gerekmediğini belirtir.   |
| Min veri değeri          | Ölçeklendirilecek giriş verilerinin en küçük değeri. Kırpma aykırı değerleri için iyi.  |
| En fazla veri değeri          | Ölçeklendirilecek giriş verilerinin en büyük değeri. Kırpma aykırı değerleri için iyi.  |

## <a name="next-steps"></a>Sonraki adımlar

Haritada verinin nasıl görüntülendiğini değiştirin:

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](power-bi-visual-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Çubuk grafik katmanı ekleme](power-bi-visual-add-bar-chart-layer.md)

Haritaya daha fazla bağlam ekleyin:

> [!div class="nextstepaction"]
> [Başvuru katmanı ekleme](power-bi-visual-add-reference-layer.md)

> [!div class="nextstepaction"]
> [Kutucuk katmanı ekleme](power-bi-visual-add-tile-layer.md)

> [!div class="nextstepaction"]
> [Gerçek zamanlı trafiği göster](power-bi-visual-show-real-time-traffic.md)
