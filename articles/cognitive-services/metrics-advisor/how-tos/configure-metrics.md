---
title: Web portalını kullanarak ölçüm Danışmanı örneğinizi yapılandırma
titleSuffix: Azure Cognitive Services
description: Ölçüm Danışmanı örneğinizi yapılandırma ve anomali algılama sonuçlarında ince ayar yapma.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: mbullwin
ms.openlocfilehash: 7923dad3d47122c0ceb04d1240736e2b66a0dd64
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92048262"
---
# <a name="how-to-configure-metrics-and-fine-tune-detecting-configuration"></a>Nasıl yapılır: ölçümleri yapılandırma ve yapılandırmayı algılama ince ayar

Web portalını kullanarak ölçüm Danışmanı örneğinizi yapılandırmaya başlamak için bu makaleyi kullanın. Belirli bir veri akışı için ölçümlere gitmek üzere, **veri akışları** sayfasına gidin ve akışların birini seçin. Bu, bununla ilişkili ölçümlerin bir listesini görüntüler.

:::image type="content" source="../media/metrics/select-metric.png" alt-text="Ölçüm seçin" lightbox="../media/metrics/select-metric.png":::

Ayrıntılarını görmek için ölçüm adlarından birine tıklayın. Bu ayrıntılı görünümde, ekranın sağ üst köşesindeki açılan listeyi kullanarak aynı veri akışındaki başka bir ölçüme geçebilirsiniz.

Ölçüm ayrıntılarını ilk kez görüntülediğinizde, ölçüm Danışmanı 'Nın sizin için bir tane seçmesine izin vererek veya her bir boyut için dahil edilecek değerleri belirterek bir zaman serisi yükleyebilirsiniz. 

Ayrıca zaman aralıklarını seçebilir ve sayfanın yerleşimini değiştirebilirsiniz.

> [!NOTE]
> - Başlangıç saati dahil değildir.
> - Bitiş saati özeldir. 

Olayları görüntülemek ve [Olay Hub 'ına](diagnose-incident.md)bir bağlantı bulmak için **Olaylar** sekmesine tıklayabilirsiniz.

## <a name="tune-the-detecting-configuration"></a>Algılama yapılandırmasını ayarlama

Bir ölçüm, bir veya daha fazla algılama yapılandırması uygulayabilir. Her ölçüm için, izleme gereksinimlerinize göre düzenleyebileceğiniz veya ekleyebileceğiniz varsayılan bir yapılandırma vardır.

### <a name="tune-the-configuration-for-all-series-in-current-metric"></a>Geçerli ölçümdeki tüm seriler için yapılandırmayı ayarla

Bu yapılandırma, ayrı bir yapılandırmaya sahip olanlar hariç, bu ölçümdeki tüm seriye uygulanır. Ölçüm düzeyi yapılandırması, veriler eklendi olduğunda varsayılan olarak uygulanır ve sol bölmede gösterilir. Kullanıcılar ölçüm düzeyi yapılandırmasını ölçüm sayfasında doğrudan düzenleyebilir. 

**Yön**ve yapılandırmayı daha fazla ayarlamak Için kullanılabilecek **geçerli anomali** gibi ek parametreler vardır. Farklı algılama yöntemlerini de birleştirebilirsiniz. 

:::image type="content" source="../media/configuration-combination.png" alt-text="Ölçüm seçin" lightbox="../media/configuration-combination.png":::

### <a name="tune-the-configuration-for-a-specific-series-or-group"></a>Belirli bir serinin veya grubun yapılandırmasını ayarlama

Grup düzeyi yapılandırmasını görmek için ölçüm düzeyi yapılandırma seçeneklerinin altındaki **Gelişmiş yapılandırma** ' ya tıklayın. Bu penceredeki simgeye tıklayarak tek bir seri için veya seri grubuna yönelik bir yapılandırma ekleyebilirsiniz **+** . Parametreler, ölçüm düzeyi yapılandırma parametrelerine benzerdir, ancak bir dizi grubunu tanımlamak için grup düzeyi yapılandırma için en az bir boyut değeri belirtmeniz gerekebilir. Ve belirli bir seriyi tanımlamak için seri düzeyi yapılandırma için tüm boyut değerlerini belirtin. 

Bu yapılandırma, ölçüm düzeyi yapılandırması yerine serinin veya belirli bir serinin grubuna uygulanır. Bu grubun koşullarını ayarladıktan sonra kaydedin.

:::image type="content" source="../media/advanced-configuration.png" alt-text="Ölçüm seçin" lightbox="../media/advanced-configuration.png":::

### <a name="anomaly-detection-methods"></a>Anomali algılama yöntemleri

Ölçüm Danışmanı birden çok anomali algılama yöntemi sunar. Bir tane kullanabilirsiniz veya düğmeye tıklayarak mantıksal işleçler kullanarak bunları birleştirebilirsiniz **+** . 

**Akıllı algılama** 

Akıllı algılama, geçmiş verilerden desenleri öğrenerek bunları daha sonra algılamak için kullanan makine öğrenimi tarafından desteklenir. Bu yöntemi kullanırken **duyarlık** , algılama sonuçlarının ayarlanması için en önemli parametredir. Sayfanın sağ tarafındaki görselleştirmeyi etkilemek için, daha küçük veya daha büyük bir değere sürükleyebilirsiniz. Verilerinize uyan bir tane seçin ve kaydedin. 


Akıllı algılama modunda, anomali algılama sonucunu ayarlamak için duyarlılık ve sınır sürümü parametreleri kullanılır.

Duyarlılık, her bir noktanın beklenen değer aralığının genişliğini etkileyebilir. Artış arttıkça, beklenen değer aralığı daha sıkı olacaktır ve daha fazla bilgi bildirilir:

:::image type="content" source="../media/metrics/smart-detection-high-sensitivity.png" alt-text="Ölçüm seçin":::

Duyarlılık kapatıldığında, beklenen değer aralığı daha geniş olur ve daha az anomali bildirilir:

:::image type="content" source="../media/metrics/smart-detection-low-sensitivity.png" alt-text="Ölçüm seçin":::

**Değişiklik eşiği** 

Ölçüm verileri genellikle belirli bir Aralık etrafında kaldığında değişiklik eşiği genellikle kullanılır. Eşik, **değişim yüzdesine**göre ayarlanır. **Değişiklik eşiği** modu, senaryolardaki anormallikleri tespit edebilir:

* Verileriniz normalde kararlı ve sorunsuz olur. Dalgalanmalar olduğunda size bildirilmesini isteyebilirsiniz.
* Verileriniz normalde oldukça kararsız ve çok fazla dalgalanmaktadır. Çok kararlı veya düz hale geldiğinde size bildirilmesini isteyebilirsiniz.

Bu modu kullanmak için aşağıdaki adımları kullanın:

1. Ölçümleriniz veya zaman seriniz için anomali algılama yapılandırmasını ayarladığınızda, anomali algılama yönteminiz için **eşiği Değiştir** ' i seçin.
    
    :::image type="content" source="../media/metrics/change-threshold.png" alt-text="Ölçüm seçin":::

2. Senaryonuza bağlı olarak **aralığın dışına** veya **Aralık** parametresini seçin.

    Dalgalanmaları saptamak istiyorsanız **aralığın dışında**' yı seçin. Örneğin, aşağıdaki ayarlarla, önceki bir ile karşılaştırıldığında %10 ' dan fazla değişiklik yapan tüm veri noktaları, bir aykırı değer olarak algılanır.
    :::image type="content" source="../media/metrics/out-of-the-range.png" alt-text="Ölçüm seçin":::

    Verilerinizde düz çizgileri algılamak isterseniz, **Aralık içinde**öğesini seçin. Örneğin, aşağıdaki ayarlarla, Öncekiyle karşılaştırıldığında% 0,01 ' de değişen herhangi bir veri noktası, bir aykırı değer olarak algılanır. Eşik bu kadar küçük (% 0,01%) olduğundan, verilerde düz çizgileri mantıksal olarak algılar.

    :::image type="content" source="../media/metrics/in-the-range.png" alt-text="Ölçüm seçin":::

3. Bir anomali olarak sayılacak değişiklik yüzdesini ayarlayın ve daha önce yakalanan veri noktalarının karşılaştırma için kullanılacağı süreyi belirleyin. Bu karşılaştırma her zaman geçerli veri noktası arasında ve öncesinde tek bir veri noktası N puntodur.
    
    **Yön** yalnızca **Aralık modundan birini** kullanıyorsanız geçerlidir:
    
    * , Algılamayı yalnızca (geçerli veri noktası) (karşılaştırma veri noktası) > eşik yüzdesi olan anormallikleri algılayacak **şekilde yapılandırır** **+** .
    * **Aşağı** , algılamayı yalnızca (geçerli veri noktası)-(veri noktası karşılaştırması) < eşik yüzdesi olan anormallikleri algılayacak şekilde yapılandırır **-** .
 
**Sabit eşik**

 Hard Threshold, anomali algılama için temel bir yöntemdir. Beklenen değer aralığını belirleyebilmek için bir üst ve/veya alt sınır belirleyebilirsiniz. Sınırın dışında kalan noktaları bir anomali olarak tanımlanır. 


## <a name="preset-events"></a>Önceden ayarlanmış olaylar

Bazen beklenen olaylar ve oluşum (tatiller gibi) anormal veri üretebilir. Önceden ayarlanmış olayları kullanarak, anomali algılama çıktısına belirtilen zamanlarda bayrak ekleyebilirsiniz. Bu özellik, veri akışınız eklendi sonra yapılandırılmalıdır. Her ölçüm yalnızca bir önceden ayarlanmış olay yapılandırmasına sahip olabilir.

> [!Note]
> Önceden ayarlanmış olay yapılandırması, anomali algılama sırasında tatiller göz önüne alınır ve sonuçlarınızı değiştirebilir. Bu, yapılandırmayı kaydettikten sonra gelen veri noktalarına uygulanır. 

Her ölçüm ayrıntıları sayfasındaki ölçümler açılan listesinin yanındaki **önceden ayarlanmış olayı Yapılandır** düğmesine tıklayın.
 
:::image type="content" source="../media/metrics/preset-event-button.png" alt-text="Ölçüm seçin":::

Görüntülenen pencerede, seçenekleri kullanımınıza göre yapılandırın. Yapılandırmayı kullanmak için **tatil olayını etkinleştir** ' in seçildiğinden emin olun. 

**Tatil olayı** bölümü tatiller sırasında algılanan gereksiz anormallikleri göstermenize yardımcı olur. **Strateji** seçeneği için uygulayabileceğiniz iki seçenek vardır:

* **Tatili bastır**: Tatil dönemi boyunca anomali algılama sonuçlarındaki tüm anormalları ve uyarıları bastırır.
* **Hafta sonu olarak tatil**: tatilden önce birkaç karşılık gelen hafta sonu için beklenen ortalama değeri hesaplar ve anomali durumunu bu değerlerden dayandırır.

Yapılandırabileceğiniz birkaç farklı değer vardır:

|Seçenek  |Açıklama  |
|---------|---------|
|**Ülke olarak bir boyut seçin**     | Ülke bilgilerini içeren bir boyut seçin. Örneğin bir ülke kodu.         |
|**Ülke kodu eşleme**     | Standart [ülke kodu](https://wikipedia.org/wiki/ISO_3166-1_alpha-2)ve Seçilen boyutun ülke verileri arasındaki eşleme.        |
|**Tatil seçenekleri**    | Tüm tatiller mi yoksa yalnızca PTO (ücretli zaman) tatiller mi yoksa yalnızca POF tatiller mi hesaba alınmayacağını belirtir.         |
|**Genişletilecek günler**    |  Bir tatilden önce ve sonra etkilenen günler.        |


Veri içindeki Döngüsel desenleri kullanarak gereksiz uyarıların azaltılmasına yardımcı olması için, **Cycle olayı** bölümü bazı senaryolarda kullanılabilir. Örnek: 

- Hem haftalık hem de aylık desen gibi birden çok desen veya döngü içeren ölçümler. 
- Açık bir düzene sahip olmayan ölçümler, ancak veriler yıl içinde (YoY), aya göre ay (MoM), hafta üzerinden hafta (WoW) veya gün içinde (DoD) arasında karşılaştırılabilir.
 
Her ayrıntı düzeyi için tüm seçenekler seçilebilir değildir. Ayrıntı düzeyi başına kullanılabilen seçenekler aşağıda verilmiştir:

| Ayrıntı düzeyi | YoY | An | WoW | DoD |
|:-|:-|:-|:-|:-|
| Yıllık | X | X | X | X |
| Aylık olarak | X | X | X | X |
| Haftalık | ✔ | X | X | X |
| Günlük | ✔ | ✔ | ✔ | X |
| Saatlik | ✔ | ✔ | ✔ | ✔ |
| Dakikada | X | X | X | X |
| İkincisi | X | X | X | X |
| Özel | ✔ | ✔ | ✔ | ✔ |

X-kullanılamıyor.  
✔ Kullanılabilir.
  
\* Saniye cinsinden özel bir ayrıntı düzeyi kullanırken, yalnızca ölçüm bir saatten uzun ve bir günden az olduğunda kullanılabilir.

Cycle olayı, döngüsel bir düzende takip ettikleri anormallikleri azaltmak için kullanılır, ancak birden fazla veri noktası bu kalıbı izlerse anomali bildirir. **Katı mod** , tek bir veri noktası, modele izmese bile anomali Raporlamayı etkinleştirmek için kullanılır. 

:::image type="content" source="../media/metrics/preset-events.png" alt-text="Ölçüm seçin":::

## <a name="view-recent-incidents"></a>Son olayları görüntüle

Ölçüm Danışmanı, tüm zaman serisi verileriniz üzerinde olduğu gibi, anormallikleri algılar. Bununla birlikte, büyük bir etkiye sahip olmadıkları için tüm anormalilerin ilerlememesi gerekmez. Toplama, ilgili olanları olaylar halinde gruplamak için yapılır. Bu olayları, ölçüm ayrıntıları sayfasındaki **olay** sekmesinden görüntüleyebilirsiniz. 

Olayla ilgili daha fazla ayrıntı görebileceğiniz **Olaylar Analizi** sayfasına gitmek için bir olaya tıklayın. Belirli bir ölçüm altındaki tüm olayları bulabileceğiniz [Olay Hub](diagnose-incident.md) 'ını bulmak Için **Yeni Olay Hub 'ında olayları Yönet**' e tıklayın. 

## <a name="subscribe-anomalies-for-notification"></a>Bildirim için anomali abone

Bir anomali algılandığında bildirim almak isterseniz, bir kanca kullanarak ölçüm için Uyarılara abone olabilirsiniz. Daha fazla bilgi için bkz. [Uyarıları yapılandırma ve kanca kullanarak bildirim edinme](alerts.md) .


## <a name="next-steps"></a>Sonraki adımlar 
- [Kanca kullanarak uyarıları yapılandırma ve bildirimler alma](alerts.md)
- [Geri bildirimi kullanarak anomali algılamayı ayarlama](anomaly-feedback.md)
- [Bir olayı tanılayın](diagnose-incident.md).

