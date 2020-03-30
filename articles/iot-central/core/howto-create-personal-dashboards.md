---
title: Azure IoT Merkezi kişisel panoları oluşturun | Microsoft Dokümanlar
description: Bir kullanıcı olarak, kişisel panolarınızı nasıl oluşturup yöneteceklerinizi öğrenin.
author: mavoge
ms.author: mavoge
ms.date: 10/17/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: db886006ff5b9adf3de0932951f6cce4958e8ebd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158138"
---
# <a name="create-and-manage-multiple-dashboards"></a>Birden çok pano oluşturma ve yönetme

**Pano,** uygulamanıza ilk gittiğınızda yüklenen sayfadır. Uygulamanızdaki **bir oluşturucu,** tüm kullanıcılar için varsayılan uygulama panosunu tanımlar. Bu varsayılan panoyu kendi kişiselleştirilmiş uygulama panonuzla değiştirebilirsiniz. Farklı verileri görüntüleyen ve aralarında geçiş yapan birkaç panonuz olabilir. 

Uygulamanın **admin** yöneticisiyseniz, uygulamanın diğer kullanıcılarıyla paylaşmak üzere en fazla 10 uygulama düzeyi panosu oluşturabilirsiniz. Yalnızca **yöneticiler** uygulama düzeyi panoları oluşturma, düzenle ve silme yeteneğine sahiptir. 

## <a name="create-dashboard"></a>Pano oluşturma

Aşağıdaki ekran görüntüsü, **Özel Uygulama** şablonundan oluşturulan bir uygulamada panoyu gösterir. Varsayılan uygulama panosunu kişisel bir panoyla veya yöneticiyseniz, başka bir uygulama düzeyi panosuyla değiştirebilirsiniz. Bunu yapmak için sayfanın sol üst kısmında **+ Yeni'yi** seçin.
 
> [!div class="mx-imgBorder"]
> !["Özel Uygulama" şablonuna dayalı uygulamalar için pano](media/howto-create-personal-dashboards/dashboard-custom-app.png)

+ **Yeni'yi** seçmek pano düzenleyicisini açar. Düzenleyicide, panonuza bir ad verebilir ve kitaplıktaki öğeleri seçebilirsiniz. Kitaplık, panoyu özelleştirmek için kullanabileceğiniz kutucukları ve pano ilkellerini içerir.

> [!div class="mx-imgBorder"]
> ![Pano kitaplığı](media/howto-create-personal-dashboards/dashboard-library.png)

Uygulamanın bir **yöneticisiyseniz,** kişisel düzey panosu veya uygulama düzeyi panosu oluşturmak istiyorsanız geçiş seçeneği verilir. Kişisel düzeyde bir pano oluşturursanız, bunu yalnızca siz görebilirsiniz. Bir uygulama düzeyi panosu oluşturursanız, uygulamanın her kullanıcısı bunu görebilir. Bir başlık girdikten ve oluşturmak istediğiniz pano türünü seçtikten sonra, daha sonra kaydedebilir ve kutucuk ekleyebilirsiniz. Veya şimdi hazırsanız ve bir aygıt şablonu ve aygıt örneği eklediyseniz, devam edebilir ve ilk döşemenizi oluşturabilirsiniz. 

> [!div class="mx-imgBorder"]
> ![Cihaz Ayrıntılarını Yapılandır" formu, Sıcaklık için ayrıntılarla birlikte](media/howto-create-personal-dashboards/device-details.png)

Örneğin, aygıtın geçerli sıcaklığı için bir **Telemetri** döşemesi ekleyebilirsiniz. Bunu yapmak için:
1. Aygıt **Şablonu** Seçin
1. Pano döşemesinde görmek istediğiniz aygıt için aygıt **örneği** seçin. Daha sonra, döşemede kullanılabilecek aygıtın özelliklerinin bir listesini görürsünüz.
1. Panodaki döşemeyi oluşturmak için **Sıcaklık'ı** tıklatın ve pano alanına sürükleyin. Ayrıca **Sıcaklık'ın** yanındaki onay kutusunu tıklatAbilir ve **Birleştir'i**tıklatın. Aşağıdaki ekran görüntüsü, bir Aygıt Şablonu ve Aygıt Örneği'ni seçip panoda sıcaklık telemetrisi döşemesi oluşturmayı gösterir.
1. Döşemeyi panoya kaydetmek için sol üstteki **Kaydet'i** seçin.

> [!div class="mx-imgBorder"]
> ![Sıcaklık döşemesi için ayrıntıları içeren pano" sekmesi](media/howto-create-personal-dashboards/temperature-tile-edit.png)

Artık kişisel panonuzu görüntülediğinizde, aygıtın **Sıcaklık** ayarı olan yeni döşemeyi görürsünüz:

> [!div class="mx-imgBorder"]
> ![Sıcaklık döşemesi için ayrıntıları içeren pano" sekmesi](media/howto-create-personal-dashboards/temperature-tile-complete.png)

Kişisel panolarınızı nasıl daha da özelleştirebileceğinizi keşfetmek için kitaplıktaki diğer döşeme türlerini keşfedebilirsiniz.

Azure IoT Central'da kutucukların nasıl kullanılacağı hakkında daha fazla bilgi edinmek [için panonuza Kutucuk Ekle'ye](howto-add-tiles-to-your-dashboard.md)bakın.

## <a name="manage-dashboards"></a>Panoları yönetme

Birkaç kişisel panoya sahip olabilir ve aralarında geçiş yapabilir veya varsayılan uygulama panolarından birini seçebilirsiniz:

> [!div class="mx-imgBorder"]
> ![Panolar arasında geçiş](media/howto-create-personal-dashboards/switch-dashboards.png)

Kişisel panolarınızı düzenleyebilir ve artık ihtiyacınız olmayan panoları silebilirsiniz. Yöneticiyseniz, **admin**uygulama düzeyi panolarını da düzenleyebilme veya silme olanağınız vardır.

> [!div class="mx-imgBorder"]
> ![Panoları silme](media/howto-create-personal-dashboards/delete-dashboards.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık kişisel panoları oluşturmayı ve yönetmeyi öğrendiğinize [göre, uygulama tercihlerinizi nasıl yönetebileceğinizi öğrenebilirsiniz](howto-manage-preferences.md)
