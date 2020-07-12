---
title: Visual Power BI Azure Maps ile çalışmaya başlama | Microsoft Azure haritaları
description: Bu makalede, Power BI için Microsoft Azure eşlemelerini nasıl kullanacağınızı öğreneceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 06/26/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: e840517934e77c96f1c19c3243ad2884d7ccce66
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86262126"
---
# <a name="getting-started-with-the-azure-maps-power-bi-visual"></a>Azure Maps Power BI Visual ile çalışmaya başlama

<Token>**Uygulama hedefi:** ![ ](media/power-bi-visual/yes.png) ***müşteriler*** için Evet Power BI hizmeti tasarımcılar için evet ![ ](media/power-bi-visual/yes.png) Power BI hizmeti tasarımcı & geliştiriciler ![ Evet ](media/power-bi-visual/yes.png) Power BI Desktop ![ Hayır ](media/power-bi-visual/no.png) Pro veya Premium lisansı gerektirir</Token>

Bu makalede, Power BI için Microsoft Azure harita görsellerinin nasıl kullanılacağı gösterilmektedir.

> [!NOTE]
> Bu görsel hem Power BI Desktop hem de Power BI hizmeti içinde oluşturulabilir ve görüntülenebilir. Bu makaledeki adımlar ve gösterimler Power BI Desktop’tan alınmıştır.

Power BI için Azure Maps Visual, bir haritanın en üstündeki uzamsal veriler için zengin bir veri görselleştirmesi kümesi sağlar. İş verilerinin %80 ' ün üzerinde bir konum bağlamı olduğunu tahmin edilmektedir. Azure haritalar görseli, bu konum bağlamının iş verilerini nasıl ele aldığı hakkında öngörüler elde etmek için kullanılabilir.

![Azure haritalar ile masaüstü Power BI iş verilerini görüntüleme](media/power-bi-visual/azure-maps-visual-hero.png)

## <a name="what-is-sent-to-azure"></a>Azure 'a ne gönderilir?

Harita görselleştirmesini oluşturmak için kullanılan harita görüntüleri ve koordinatları gibi konum verilerini almak için Azure Maps görseli, Azure 'da barındırılan bulut hizmetine bağlanır. 

-   Haritanın odaklandığı alanla ilgili ayrıntılar, harita tuvali (eşleme kutucukları olarak da bilinir) işlemek için gereken görüntüleri almak üzere Azure 'a gönderilir. 
-   Konum, enlem ve Boylam demetlerindeki veriler, harita koordinatlarını (coğrafi kodlama adlı bir işlem) almak için Azure 'a gönderilebilir. 

Siz veya yöneticiniz, aşağıdaki URL 'YI kullanan Azure haritalar platformuna erişime izin vermek için güvenlik duvarınızı güncelleştirmeniz gerekebilir.

> `https://atlas.microsoft.com`

Daha fazla bilgi edinmek için Azure Maps ile ilgili gizlilik ve kullanım koşulları hakkında [Microsoft Azure yasal bilgiler](https://azure.microsoft.com/support/legal/)bölümüne bakın.

## <a name="preview-behavior-and-requirements"></a>Önizleme davranışı ve gereksinimleri

**Azure haritalar**'ın bu önizleme sürümü için bazı önemli noktalar ve gereksinimler vardır:

-   **Azure haritalar** görseli önizleme aşamasındadır ve Power BI Desktop ' de etkinleştirilmesi gerekir. **Azure haritalar**'ı etkinleştirmek için **Dosya** &gt; **seçenekleri ve ayarlar** &gt; **Seçenekler** &gt; **Önizleme özellikleri**' ni seçin ve ardından **Azure haritalar görsel** onay kutusunu seçin. Bunu yaptıktan sonra Azure Maps görseli yoksa, yönetim portalındaki bir kiracı yönetici anahtarının etkinleştirilmesi olasıdır.
-   Veri kümesi, **Enlem** ve **boylam** bilgilerini içeren alanlara sahip olmalıdır. Konum alanlarının coğrafi kodlama gelecekteki bir güncelleştirmeye eklenecektir.
-   Power BI için yerleşik gösterge denetimi şu anda bu önizlemede görünmüyor. Bu, gelecekteki bir güncelleştirmeye eklenecektir.

## <a name="use-the-azure-maps-visual"></a>Azure haritalar görselini kullanma

**Azure haritalar** görseli etkinleştirildikten sonra **görsel öğeler** bölmesinden **Azure haritalar** simgesini seçin.

![Görsel öğeler bölmesindeki Azure haritalar görsel düğmesi](media/power-bi-visual/azure-maps-in-visualizations-pane.png)

Power BI boş bir Azure Maps görsel tasarım tuvali oluşturur. Önizleme aşamasında ek bir vazgeçme belgesi görüntülenir.

![Azure haritalar görselindeki masaüstü Power BI ilk durumuna göre yüklendi](media/power-bi-visual/visual-initial-load.png)

Azure haritalar görselini yüklemek için aşağıdaki adımları uygulayın:

1.  **Alanlar** bölmesinde, enlem ve boylam koordinat bilgilerini içeren veri alanlarını **Enlem** ve/veya **boylam** demetlerine sürükleyin. Bu, Azure Maps görselini yüklemek için gereken en az veri.
    
    > [!div class="mx-imgBorder"]
    > ![Azure haritalar, enlem ve boylam alanları sağlandıktan sonra haritada kabarcıklar halinde görsel görüntüleme noktaları](media/power-bi-visual/bubble-layer.png)

2.  Kategoriye göre verileri renklendirmek için bir kategorik alanını **alanlar** bölmesinin **gösterge** demetine sürükleyin. Bu örnekte, **Admindistrict** sütununu kullanıyoruz (eyalet veya il olarak da bilinir).  
    
    > [!div class="mx-imgBorder"]
    > ![Azure haritalar, göstergeyi açıklama alanı sağlandığında haritada renkli kabarcıklar olarak görüntüler](media/power-bi-visual/bubble-layer-with-legend-color.png)

    > [!NOTE]
    > Power BI için yerleşik gösterge denetimi şu anda bu önizlemede görünmüyor. Bu, gelecekteki bir güncelleştirmeye eklenecektir.

3.  Verileri görece ölçeklendirmek için, bir ölçüyü **alanlar** bölmesinin **Boyut** demetine sürükleyin. Bu örnekte **Sales** sütununu kullanıyoruz.  
    
    > [!div class="mx-imgBorder"]
    > ![Azure, boyutları belirtilen boyut alanı oluşturulduktan sonra harita üzerinde renkli ve ölçeklendirilmiş kabarcıklar olarak görüntüler.](media/power-bi-visual/bubble-layer-with-legend-color-and-size.png)

4.  Verilerin nasıl işleneceğini özelleştirmek için **Biçim** bölmesindeki seçenekleri kullanın. Aşağıdaki görüntü, yukarıdaki haritada aynı eşlemedir, ancak kabarcık katmanları dolgusu saydamlığı seçeneği %50 olarak ve yüksek karşıtlıklı ana hat seçeneği etkin olarak ayarlanmıştır.  
    
    > [!div class="mx-imgBorder"]
    > ![Azure haritalar, özel bir stille haritada kabarcık olarak noktaları görüntüler.](media/power-bi-visual/bubble-layer-styled.png)

## <a name="fields-pane-buckets"></a>Alan bölmesi demetleri

Aşağıdaki veri demetleri, Azure Maps görselin **alanlar** bölmesinde bulunur.

| Alan     | Açıklama  |
|-----------|--------------|
| Enlem  | Veri noktalarının Enlem değerini belirtmek için kullanılan alan. Enlem değerleri-90 ile 90 ondalık derece biçiminde olmalıdır.  |
| Boylam | Veri noktalarının Boylam değerini belirtmek için kullanılan alan. Boylam değerleri-180 ve 180 ondalık derece biçiminde olmalıdır.  |
| Gösterge    | Her bir kategorideki veri noktaları için verileri kategorilere ayırmak ve benzersiz bir renk atamak için kullanılan alan. Bu demet dolduğunda, renkler üzerinde ayarlamalar yapılmasına izin veren **Biçim** bölmesinde bir **veri renkleri** bölümü görüntülenir. |
| Boyut      | Haritadaki veri noktalarının göreli boyutlandırması için kullanılan ölçü.   |
| Araç İpuçları  | Şekillerin üzerine gelindiğinde araç ipuçlarında görüntülenen ek veri alanları. |

## <a name="map-settings"></a>Harita ayarları

Biçim bölmesinin **harita ayarları** bölümü, haritanın nasıl görüntülendiğini özelleştirmek ve güncelleştirmelerin yeniden nasıl davranması için seçenekler sağlar.

| Ayar             | Açıklama  |
|---------------------|--------------|
| Otomatik yakınlaştırma           | Haritayı, görselin **alanlar** bölmesi aracılığıyla yüklenen verilere otomatik olarak büyütür. Veriler değiştikçe, eşleme konumunu uygun şekilde güncelleştirir. Kaydırıcı **kapalı** konumdayken, varsayılan harita görünümü için ek harita görünümü ayarları görüntülenir. |
| Dünya sarması          | Kullanıcının Haritayı yatay olarak sonsuz şekilde kaydırmasına olanak tanır. |
| Stil Seçici        | Haritaya rapor okuyucularının haritanın stilini değiştirmesine izin veren bir düğme ekler. |
| Gezinti denetimleri | Rapor okuyucularının haritanın boyutunu yakınlaştırmasına, döndürmesine ve değiştirmesine izin vermek için başka bir yöntem olarak haritaya düğme ekler. Daha fazla bilgi için, kullanıcıların haritada gezinebilecekleri farklı yollarla ilgili ayrıntılar için [haritada gezinirken](map-accessibility.md#navigating-the-map) bu belgeye bakın. |
| Harita stili           | Haritanın stili. Daha fazla bilgi için, [desteklenen harita stilleri](supported-map-styles.md)hakkında daha fazla bilgi için bu belgeye bakın. |

### <a name="map-view-settings"></a>Harita görünümü ayarları

**Otomatik yakınlaştırma** kaydırıcı **kapalı** konumdayken, aşağıdaki ayarlar görüntülenir ve kullanıcının varsayılan harita görünümü bilgilerini belirtmesini sağlar.

| Ayar          | Açıklama   |
|------------------|---------------|
| Zoom             | Haritanın varsayılan yakınlaştırma düzeyi. 0 ile 22 arasında bir sayı olabilir. |
| Orta Enlem  | Haritanın merkezinde varsayılan enlem. |
| Orta Boylam | Haritanın merkezinde varsayılan boylam. |
| Başlığıyla          | Haritanın derece cinsinden varsayılan yönü; burada 0 Kuzey, 90 Doğu, 180, Güney ve 270 Batı. 0 ile 360 arasında herhangi bir sayı olabilir. |
| Perde            | Haritanın, 0 ve 60 arasındaki derece cinsinden varsayılan eğimi, 0 ile arasında (0 ' ın, haritada normal şekilde bakıyor). |

## <a name="considerations-and-limitations"></a>Önemli Noktalar ve Sınırlamalar

Azure haritalar görseli aşağıdaki hizmet ve uygulamalarda kullanılabilir:

| Hizmet/Uygulama                              | Kullanılabilirlik |
|------------------------------------------|--------------|
| Power BI Desktop                         | Evet          |
| Power BI hizmeti (app.powerbi.com)       | Evet          |
| Power BI mobil uygulamaları             | Evet          |
| Power BI - web'de yayımlama                  | Hayır           |
| Power BI Embedded                        | Hayır           |
| Power BI hizmeti - ekleme (PowerBI.com) | Evet          |

Ek Power BI Hizmetleri/uygulamaları için destek, gelecekteki güncelleştirmelere eklenecektir.

**Azure haritalar nerede kullanılabilir?**

Şu anda Azure Maps Şu anda tüm ülkelerde ve bölgelerde aşağıdakiler hariç sunulmaktadır:

- Çin
- Güney Kore

Bu görseli destekleyen farklı Azure haritalar Hizmetleri için kapsam ayrıntıları için, [coğrafi kapsam bilgileri](geographic-coverage.md) belgesine bakın.

**Azure haritalar görseli hangi web tarayıcıları tarafından destekleniyor?**

[Azure Haritalar Web SDK 'sı tarafından desteklenen tarayıcılar](supported-browsers.md)hakkında bilgi için bu belgelere bakın.

**Kaç veri noktası görselleştirebilirim?**

Bu görselde en fazla 30.000 veri noktası desteklenir.

**Bu görselde adresler veya diğer konum dizeleri kullanılabilir mi?**

Bu görselin ilk önizlemesi yalnızca Enlem ve boylam değerlerini ondalık derecelerde destekler. Gelecekteki bir güncelleştirme, adresler ve diğer konum dizeleri için destek ekler.

## <a name="next-steps"></a>Sonraki adımlar

Azure haritalar Power BI görsel hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Azure haritalar Power BI görselindeki katmanları anlama](power-bi-visual-understanding-layers.md)

> [!div class="nextstepaction"]
> [Kuruluşunuzun içindeki Azure haritalar görselini yönetme](power-bi-visual-manage-access.md)

Görseli özelleştirin:

> [!div class="nextstepaction"]
> [Power BI'da renk biçimlendirmesiyle ilgili ipuçları ve püf noktaları](https://docs.microsoft.com/power-bi/visuals/service-tips-and-tricks-for-color-formatting)

> [!div class="nextstepaction"]
> [Görselleştirme başlıklarını, arka planlarını ve göstergelerini özelleştirme](https://docs.microsoft.com/power-bi/visuals/power-bi-visualization-customize-title-background-and-legend)
