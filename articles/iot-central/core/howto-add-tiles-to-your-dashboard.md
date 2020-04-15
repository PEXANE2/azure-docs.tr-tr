---
title: Panonuza kutucuk ekleme | Microsoft Dokümanlar
description: Oluşturucu olarak, varsayılan Azure IoT Merkezi uygulama panosunu nasıl yapılandırıştırmayı öğrenin.
author: mavoge
ms.author: mavoge
ms.date: 10/17/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: f75e5a28f7ec56750432e74ee48ba68491a5e481
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81310292"
---
# <a name="configure-the-application-dashboard"></a>Uygulama panosunu yapılandırma

**Pano,** uygulamaya erişimi olan kullanıcılar uygulamanın URL'sine gittiğinde yüklenen sayfadır. **Uygulamaşablonlarından**birinden uygulamanızı oluşturduysanız, uygulamanızın başlatmak için önceden tanımlanmış bir panosu olacaktır. Uygulamanızı Eski uygulama **uygulama** şablonundan oluşturduysanız, panonuz başlamak için boş olur.

> [!NOTE]
> Kullanıcılar varsayılan uygulama panosuna ek olarak [birden çok pano oluşturabilir.](howto-create-personal-dashboards.md) Bu panolar yalnızca kullanıcıya özel olabilir veya uygulamanın tüm kullanıcıları arasında paylaşılabilir. 

## <a name="add-tiles"></a>Kutucuk ekle

Aşağıdaki ekran görüntüsü, **Özel Uygulama** şablonundan oluşturulan bir uygulamada panoyu gösterir. Uygulamanız için varsayılan panoyu özelleştirmek için sayfanın sol üst **kısmındaki Edit'i** seçin.

> [!div class="mx-imgBorder"]
> !["Örnek Contoso" şablonuna dayalı uygulamalar için pano](media/howto-add-tiles-to-your-dashboard/dashboard-sample-contoso.png)

**Edit'i** seçmek pano kitaplık panelini açar. Kitaplık, panoyu özelleştirmek için kullanabileceğiniz kutucukları ve pano ilkellerini içerir.

> [!div class="mx-imgBorder"]
> ![Pano kitaplığı](media/howto-add-tiles-to-your-dashboard/dashboard-library.png)

Örneğin, aygıtın geçerli sıcaklığı için bir **Telemetri** döşemesi ekleyebilirsiniz. Bunu yapmak için:
1. Aygıt **Şablonu** Seçin
1. Pano döşemesinde görmek istediğiniz aygıt için aygıt **örneği** seçin. Daha sonra, döşemede kullanılabilecek aygıtın özelliklerinin bir listesini görürsünüz.
1. Panodaki döşemeyi oluşturmak için **Sıcaklık'ı** tıklatın ve pano alanına sürükleyin. Ayrıca **Sıcaklık'ın** yanındaki onay kutusunu tıklatAbilir ve **Birleştir'i**tıklatın. Aşağıdaki ekran görüntüsü, bir Aygıt Şablonu ve Aygıt Örneği'ni seçip panoda sıcaklık telemetrisi döşemesi oluşturmayı gösterir.
1. Döşemeyi panoya kaydetmek için sol üstteki **Kaydet'i** seçin.

> [!div class="mx-imgBorder"]
> ![Ayarlar ve özellikler için ayrıntılarla birlikte "Aygıt Ayrıntılarını Yapılandırma" formu](media/howto-add-tiles-to-your-dashboard/device-details.png)

Şimdi bir operatör varsayılan uygulama panosunu görüntülediğinde, aygıtın **Sıcaklığı** ile yeni döşemeyi görür. Her döşeme, döşeme oluşturulduğunda görüntülenecek önceden seçilmiş bir grafik, grafik vb. vardır. Ancak, kullanıcılar bu görselleştirmeyi ve değiştirmeyi seçebilir. 

> [!div class="mx-imgBorder"]
> ![Döşeme için görüntülenen ayarları ve özellikleri ile "Pano" sekmesi](media/howto-add-tiles-to-your-dashboard/settings-and-properties.png)

## <a name="edit-tiles"></a>Kutucukları edin

Panodaki bir kutucuğu yeniden güncellemek için, önce sayfanın sol üst kısmındaki **ve** pano ve tüm kutucuklar için edit modunu açacak olan Edit'i tıklatın. 

> [!div class="mx-imgBorder"]
> ![Seçili bir döşeme için etkinleştirilen edit moduna sahip pano ekranı](media/howto-add-tiles-to-your-dashboard/edit-mode.png)

Ardından, düzeltmek istediğiniz döşemenin sağ üst köşesindeki **Dişli** simgesine tıklayın. Burada başlığı, görselleştirme, toplama, vb dahil olmak üzere kiremit yönlerini edebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Döşeme toplama ayarları için açılır bırakma](media/howto-add-tiles-to-your-dashboard/aggregation-settings.png)

Döşemeüzerindeki **Cetvel** simgesine tıklayarak grafik görselleştirmesini de değiştirebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Döşeme görselleştirme ayarları için açılır bırakma](media/howto-add-tiles-to-your-dashboard/visualization-settings.png)

## <a name="tile-types"></a>Kiremit türleri

Aşağıdaki tablo, Azure IoT Central'daki kutucukların kullanımını özetley:
 
| Kutucuk | Pano | Açıklama
| ----------- | ------- | ------- |
| İçerik | Uygulama ve cihaz seti panoları |İşaretle destekli döşemeler, başlık ve açıklama metnini görüntüleyen tıklanabilir kutucuklardır. Bu döşemeyi, kullanıcının uygulamanızla ilgili bir URL'de gezinmesini sağlamak için bağlantı döşemesi olarak da kullanabilirsiniz.|
| Görüntü | Uygulama ve cihaz seti panoları |Resim döşemeleri özel bir resim görüntüler ve tıklanabilir. Panoya grafik eklemek ve isteğe bağlı olarak bir kullanıcının uygulamanızla alakalı bir URL'de gezinmesini sağlamak için resim döşemesini kullanın.|
| Etiketle | Uygulama panoları |Etiket döşemeleri panoda özel metin görüntüler. Metnin boyutunu seçebilirsiniz. Panoya bu tür açıklamalar, iletişim bilgileri veya yardım la ilgili bilgileri eklemek için etiket döşemesi kullanın.|
| Eşleme | Uygulama ve cihaz panoları |Harita kutucukları bir aygıtın konumunu haritada görüntüler. Ayrıca, bir aygıtın konum geçmişinin en fazla 100 noktasını görüntüleyebilirsiniz. Örneğin, bir aygıtın son bir hafta içinde bulunduğu yerin örneklenmiş bir rotasını görüntüleyebilirsiniz.|
| Çizgi Grafiği | Uygulama ve cihaz panoları |Çizgi grafiği döşemeleri, bir aygıt için belirli bir süre için toplam ölçüm grafiği görüntüler. Örneğin, bir aygıtın son bir saatteki ortalama sıcaklığını ve basıncını gösteren bir çizgi grafiği görüntüleyebilirsiniz.|
| Çubuk Grafiği | Uygulama ve cihaz panoları |Çubuk grafik döşemeleri, bir aygıt için belirli bir süre için toplam ölçümlerin grafiğini görüntüler. Örneğin, bir aygıtın son bir saatteki ortalama sıcaklığını ve basıncını gösteren bir çubuk grafiği görüntüleyebilirsiniz.|
| Pasta Grafiği | Uygulama ve cihaz seti panoları |Pasta grafiği döşemeleri, bir aygıt için belirli bir süre için toplam ölçümlerin grafiğini görüntüler.|
| Isı Haritası | Uygulama ve cihaz seti panoları |Isı Haritası döşemeleri, renk olarak temsil edilen cihaz kümesi hakkındaki bilgileri görüntüler.|
| Etkinlik Geçmişi | Uygulama ve cihaz panoları |Olay Geçmişi kutucukları bir aygıt için olayları bir zaman dilimi içinde görüntüler. Örneğin, son bir saat içinde bir aygıtın tüm sıcaklık değişikliklerini göstermek için kullanabilirsiniz.|
| Devlet Tarihi | Uygulama ve cihaz panoları |Durum geçmişi kutucukları bir döneme ait ölçüm değerlerini görüntüler. Örneğin, son saat içinde bir aygıtın sıcaklık değerlerini göstermek için kullanabilirsiniz.|
| KPI | Uygulama ve cihaz panoları | KPI karoları bir süre için toplu bir telemetri veya olay ölçümü görüntüler. Örneğin, son bir saat içinde bir aygıt için ulaşılan maksimum sıcaklığı göstermek için kullanabilirsiniz.|
| Bilinen Son Değer | Uygulama ve cihaz panoları |Bilinen son değer döşemeleri, bir telemetri veya durum ölçümü için en son değeri görüntüler. Örneğin, bu döşemeyi bir aygıtın en son sıcaklık, basınç ve nem ölçümlerini görüntülemek için kullanabilirsiniz.|

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Merkezi varsayılan uygulama panonuzu yapılandırmayı öğrendiğinize göre, [kişisel bir pano oluşturmayı öğrenebilirsiniz.](howto-create-personal-dashboards.md)
