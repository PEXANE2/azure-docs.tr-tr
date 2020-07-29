---
title: Azure IoT Central panonuza kutucuk ekleme | Microsoft Docs
description: Oluşturucu olarak, varsayılan Azure IoT Central uygulama panosunu kutucuklar ile yapılandırmayı öğrenin.
author: Haley-Rowland
ms.author: harowl
ms.date: 05/27/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 892bdcc08bd19b92c8b3d32d2954583f80005e87
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84022921"
---
# <a name="configure-the-application-dashboard"></a>Uygulama panosunu yapılandırma

**Pano** , bir IoT Central uygulamasına bağlandığınızda gördüğünüz ilk sayfasıdır. Uygulamanızı sektör odaklı [uygulama şablonlarından](./concepts-app-templates.md)biri ile oluşturursanız, uygulamanızda başlamak için önceden tanımlanmış bir pano vardır. Uygulamanızı özel bir [uygulama şablonundan](./concepts-app-templates.md)oluşturursanız, panonuz başlamak için bazı ipuçları gösterir.

> [!TIP]
> Kullanıcılar, varsayılan uygulama panosuna ek olarak [birden çok Pano oluşturabilir](howto-create-personal-dashboards.md) . Bu panolar yalnızca Kullanıcı tarafından kişisel olabilir veya uygulamanın tüm kullanıcıları arasında paylaşılabilir.  

## <a name="add-tiles"></a>Kutucuk ekle

Aşağıdaki ekran görüntüsünde, Pano **özel uygulama** şablonundan oluşturulan bir uygulamada gösterilmektedir. Geçerli panoyu özelleştirmek için **Düzenle**' yi seçin, özel bir kişisel veya paylaşılan Pano eklemek için **Yeni**' yi seçin:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/dashboard-sample-contoso.png" alt-text="Özel uygulama şablonunu temel alan uygulamalar için Pano":::

**Düzenle** veya **Yeni**seçeneğini belirledikten sonra Pano *düzenleme* modundadır. Panoya kutucuk eklemek ve panodaki kutucukları özelleştirmek ve kaldırmak için **Pano düzenleme** bölmesindeki araçları kullanabilirsiniz. Örneğin, bir veya daha fazla cihaz tarafından bildirilen geçerli sıcaklığın gösterilmesi için bir **telemetri** kutucuğu eklemek için:

1. **Panoyu Düzenle** panelinde bir **cihaz grubu**seçin.
1. Kutucukta göstermek için **cihazlar** açılan menüsünde bir veya daha fazla cihaz seçin. Artık cihazlarda kullanılabilir telemetri, Özellikler ve komutları görürsünüz.
1. Telemetri bölümünde **sıcaklık** ' ı seçin ve ardından **kutucuk Ekle**' yi seçin. Kutucuk artık, görselleştirmeyi değiştirebileceğiniz, kutucuğu yeniden boyutlandırmanın ve yapılandırabileceğiniz Pano üzerinde görüntülenir:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/device-details.png" alt-text="Panoya bir sıcaklık telemetri kutucuğu ekleme":::

Panoya kutucuk ekleme ve özelleştirmeyi bitirdiğinizde **Kaydet**' i seçin.

## <a name="customize-tiles"></a>Kutucukları Özelleştir

Panodaki bir kutucuğu özelleştirmek için panonun Düzenleme modunda olması gerekir. Kullanılabilir özelleştirme seçenekleri [kutucuk türüne](#tile-types)bağlıdır:

* Bir kutucukta cetvel simgesi görselleştirmeyi değiştirmenize olanak sağlar. Görsel öğeler çizgi grafiklerini, bilinen son değerleri ve ısı haritalarını içerir.

* Kare simgesi kutucuğu yeniden boyutlandırmanızı sağlar.

* Dişli simgesi görselleştirmeyi yapılandırmanızı sağlar. Örneğin, bir çizgi grafik görselleştirmesi için, göstergeyi ve eksenleri göstermeyi seçebilir ve çizilmesi için zaman aralığını seçebilirsiniz.

## <a name="tile-types"></a>Döşeme türleri

Aşağıdaki tabloda, bir panoya ekleyebileceğiniz farklı kutucuk türleri açıklanmaktadır:

| Kutucuk             | Açıklama |
| ---------------- | ----------- |
| Markdown         | Markın kutucukları, markaşağı kullanarak biçimlendirilen bir başlık ve açıklama metnini görüntüleyen tıklatılabilir kutucuklardır. URL, uygulamadaki başka bir sayfanın göreli bağlantısı veya dış siteye mutlak bir bağlantı olabilir.|
| Görüntü            | Görüntü kutucukları özel bir görüntü görüntüler ve tıklatılabilir olabilir. URL, uygulamadaki başka bir sayfanın göreli bağlantısı veya dış siteye mutlak bir bağlantı olabilir.|
| Etiketle            | Etiket kutucukları, bir panoda özel metin görüntüler. Metnin boyutunu seçebilirsiniz. İlgili bilgileri panoya, iletişim ayrıntılarına veya yardım 'a eklemek için bir etiket kutucuğu kullanın.|
| Sayı            | Sayı kutucukları bir cihaz grubundaki cihaz sayısını görüntüler.|
| Harita              | Harita kutucukları bir haritadaki bir veya daha fazla cihazın konumunu görüntüler. Ayrıca, bir cihazın konum geçmişinin en fazla 100 noktasını görüntüleyebilirsiniz. Örneğin, bir cihazın geçen hafta içinde bulunduğu yerin örneklendiği yolunu görüntüleyebilirsiniz.|
| KPI              |  KPI kutucukları, bir veya daha fazla cihazın bir zaman diliminde toplam telemetri değerlerini görüntüler. Örneğin, son bir saat içinde bir veya daha fazla cihaz için en fazla sıcaklık ve basınç sınırına ulaşılmayı göstermek için bunu kullanabilirsiniz.|
| Çizgi grafik       | Çizgi grafik kutucukları bir veya daha fazla cihaz için bir zaman dilimi için bir veya daha fazla toplama telemetri değeri çizdir. Örneğin, son bir saat için bir veya daha fazla cihazın ortalama sıcaklığını ve basıncını çizmek üzere bir çizgi grafik görüntüleyebilirsiniz.|
| Çubuk Grafik        | Çubuk grafik kutucukları bir veya daha fazla cihaz için bir zaman dilimi için bir veya daha fazla toplama telemetri değeri çizdir. Örneğin, son bir saat içindeki bir veya daha fazla cihazın ortalama sıcaklığını ve basıncını göstermek için bir çubuk grafik görüntüleyebilirsiniz.|
| Pasta grafiği        | Pasta grafik kutucukları bir veya daha fazla cihaz için bir zaman dilimi için bir veya daha fazla toplama telemetri değeri görüntüler.|
| Isı haritası         | Isı haritası kutucukları, renk olarak gösterilen bir veya daha fazla cihaz hakkındaki bilgileri görüntüler.|
| Bilinen son değer | Son bilinen değer kutucukları bir veya daha fazla cihaz için en son telemetri değerlerini görüntüler. Örneğin, bir veya daha fazla cihaz için en son sıcaklık, basınç ve nem değerlerini göstermek üzere bu kutucuğu kullanabilirsiniz. |
| Olay geçmişi    | Olay geçmişi kutucukları bir zaman dönemi içinde bir cihaz için olayları görüntüler. Örneğin, son bir saat içinde bir veya daha fazla cihaz için tüm Vana açık ve kapatma olaylarını göstermek üzere onu kullanabilirsiniz.|
| Özellik         |  Özellik kutucukları, bir veya daha fazla cihazın Özellikler ve bulut özellikleri için geçerli değeri görüntüler. Örneğin, bu kutucuğu, bir cihazın üretici veya bellenim sürümü gibi cihaz özelliklerini göstermek için kullanabilirsiniz. |

Şu anda birden çok cihazı destekleyen kutucuklara en fazla 10 cihaz ekleyebilirsiniz.

### <a name="customizing-visualizations"></a>Görselleştirmeleri özelleştirme

Toplam değerleri görüntüleyen kutucuklar için, toplamayı seçmek üzere **grafiği Yapılandır** panelinde telemetri türünün yanındaki dişli simgesini seçin. Ortalama, toplam, maksimum, en düşük ve sayı seçeneklerinden birini belirleyebilirsiniz.

Çizgi grafikler, çubuk grafikler ve pasta grafikleri için farklı telemetri değerlerinin rengini özelleştirebilirsiniz. Özelleştirmek istediğiniz telemetrinin yanındaki palet simgesini seçin:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/color-customization.png" alt-text="Telemetri değerinin rengini değiştirme":::

Dize özelliklerini veya telemetri değerlerini gösteren kutucuklar için metnin nasıl görüntüleneceğini seçebilirsiniz. Örneğin, cihaz bir URL 'YI dize özelliğinde depoluyorsa, bunu tıklatılabilir bir bağlantı olarak gösterebilirsiniz. URL bir resme başvuruyorsa, görüntüyü bilinen son bir değer veya özellik kutucuğunda işleyebilirsiniz. Bir dizenin nasıl görüntüleneceğini değiştirmek için, kutucuk yapılandırmasında telemetri türü veya özelliğin yanındaki dişli simgesini seçin:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/string-customization.png" alt-text="Bir dizenin bir kutucukta nasıl görüntüleneceğini chanz":::

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central varsayılan uygulama panonuzun nasıl yapılandırılacağını öğrendiğinize göre, [Kişisel Pano oluşturmayı öğrenin](howto-create-personal-dashboards.md).
