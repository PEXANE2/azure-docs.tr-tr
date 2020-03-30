---
title: Azure IoT Merkezi uygulamanızdaki aygıt verilerini analiz edin | Microsoft Dokümanlar
description: Azure IoT Central uygulamanızdaki aygıt verilerini analiz edin.
author: ankitscribbles
ms.author: ankitgup
ms.date: 11/27/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: 40460b58ede0ca0da8fe25475906bdbe41bfffe0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158291"
---
# <a name="how-to-use-analytics-to-analyze-device-data"></a>Cihaz verilerini analiz etmek için analitik nasıl kullanılır?

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*



Azure IoT Central, geçmiş eğilimleri analiz etmek ve aygıtlarınızdaki çeşitli telemeleri ilişkilendirmek için zengin analiz özellikleri sağlar. Başlamak için sol bölmedeki **Analytics'i** ziyaret edin.

## <a name="understanding-the-analytics-ui"></a>Analitik UI'yi Anlama
Analytics kullanıcı arabirimi üç ana bileşenden oluşur:
- **Veri yapılandırma paneli:** Yapılandırma panelinde, verileri çözümlemek istediğiniz aygıt grubunu seçerek başlayın. Ardından, çözümlemek istediğiniz telemetriyi seçin ve her telemetri için toplama yöntemini seçin. **Split By** denetimi, aygıt özelliklerini boyut olarak kullanarak verilerin gruplandırmasına yardımcı olur.

- **Zaman kontrolü:** Zaman denetimi, verileri çözümlemek istediğiniz süreyi seçmek için kullanılır. Zaman aralığını seçmek için zamanın her iki ucunu sürükleyebilirsiniz. Zaman denetimi, kovayı veya verileri toplamak için kullanılan aralık boyutunu kontrol eden bir **Interval boyutu** kaydırıcısı da vardır. 

- **Grafik denetimi:** Grafik denetimi verileri bir çizgi grafiği olarak görselleştirir. Grafik göstergesiyle etkileşimkurarak belirli satırların görünürlüğünü geçiş yapabilirsiniz. 


  ![Analitik UI Genel Bakış](media/howto-create-analytics/analyticsui.png)


## <a name="querying-your-data"></a>Verilerinizi sorgulama

Bir **aygıt grubu**ve analiz etmek istediğiniz telemetriyi seçerek başlamanız gerekir. İşinizi bitirdikten sonra, verilerinizi görselleştirmeye başlamak için **Analiz** et'i seçin.

- **Cihaz grubu:** [Aygıt grubu,](tutorial-use-device-groups.md) aygıtlarınızın kullanıcı tanımlı grubudur. Örneğin, Oakland tüm Buzdolapları, ya da Tüm sürüm 2.0 rüzgar türbinleri.

- **Telemetri:** Analiz etmek ve keşfetmek istediğiniz telemetriyi seçin. Birlikte çözümlemek için birden çok telemetries seçebilirsiniz. Varsayılan toplama yöntemi, sırasıyla sayısal için Ortalama ve dize veri türü için Sayma olarak ayarlanır. Sayısal veri türleri için desteklenen toplama yöntemleri Ortalama, Maksimum, Minimum, Sayım ve, Toplam'dır.  Dize veri türü için desteklenen toplama yöntemleri sayılır.

- **Bölün:** 'Bölüne' denetimi, aygıt özelliklerini boyut olarak kullanarak verilerin gruplandırmasına yardımcı olur. Aygıt ve bulut özelliklerinin değerleri, cihaz tarafından gönderildiği anda ve ne zaman gönderildiği yle birlikte birleştirilir. Bulut veya aygıt özelliği güncelleştirilmişse, grafikte farklı değerlere göre gruplanmış telemetriyi görürsünüz.

    > [!TIP]
    > Her aygıtın verilerini ayrı ayrı görüntülemek için 'Bölünerek' denetiminde Aygıt Kimliği'ni seçin.

## <a name="interacting-with-your-data"></a>Verilerinizle etkileşim kurma

Verilerinizi sorguladıktan sonra, çizgi grafiğinde görselleştirmeye başlayabilirsiniz. Telemetriyi gösterebilir/gizleyebilir, süreyi değiştirebilir, veri ızgarasında telemetrigörüntüleyebilirsiniz.

- **Zaman editörü paneli:** Varsayılan olarak, son bir günden veri alacağız. Zaman süresini değiştirmek için zaman kaydırıcısının her iki ucunu sürükleyebilirsiniz. Takvim denetimini önceden tanımlanmış zaman kovalarından birini seçmek veya özel bir zaman aralığı seçmek için de kullanabilirsiniz. Zaman denetimi, kovayı veya verileri toplamak için kullanılan aralık boyutunu kontrol eden bir **Interval boyutu** kaydırıcısı da vardır.

    ![Zaman Editörü](media/howto-create-analytics/timeeditorpanel.png)

    - **İç tarih aralığı kaydırıcı aracı**: İki uç nokta denetimini istediğiniz zaman aralığında sürükleyerek kullanın. Bu iç tarih aralığı dış tarih aralığı kaydırıcı kontrolü ile sınırlandırılmıştır.
    
   
    - **Dış tarih aralığı kaydırıcı sıcası denetimi**: İç tarih aralığı denetiminiz için kullanılabilecek dış tarih aralığını seçmek için bitiş noktası denetimlerini kullanın.

    - **Tarih aralığı düğmelerini artırın ve azaltın**: İstediğiniz aralık için düğmelerden birini seçerek zaman aralığınızı artırın veya azaltın.

    - **Aralık boyutu kaydırıcı**: Aynı zaman aralığında aralıkları yakınlaştırmak ve uzaklaştırmak için kullanın. Bu eylem, büyük zaman dilimleri arasındaki hareketin daha hassas denetimini sağlar. Verilerinizin parçalı, yüksek çözünürlüklü görünümlerini milisaniyelere kadar bile olsa görmek için kullanabilirsiniz. Kaydırıcının varsayılan başlangıç noktası, çözünürlük, sorgu hızı ve parçalı lığı dengeleyen seçiminizdeki verilerin en uygun görünümü olarak ayarlanır.
    
    - **Tarih aralığı seçici**: Bu web denetimi ile istediğiniz tarih ve saat aralıklarını kolayca seçebilirsiniz. Denetimi farklı saat dilimleri arasında geçiş yapmak için de kullanabilirsiniz. Geçerli çalışma alanınıza uygulanacak değişiklikleri yaptıktan sonra Kaydet'i seçin.

    > [!TIP]
    > Aralık boyutu, seçilen zaman aralığına göre dinamik olarak belirlenir. Daha küçük zaman aralıkları, verilerin birkaç saniyeye kadar çok parçalı aralıklarla toplanmasını sağlar.


- **Grafik Efsanesi:** Grafik göstergesi, grafikte seçilen telemetriyi gösterir. Grafik üzerinde odak haline getirmek için gösterge üzerinde her öğe üzerinde gezinmek olabilir. 'By Böleni' kullanırken, telemetri seçili boyutun ilgili değerlerine göre gruplandırılır. Grup adını tıklatarak her bir telemetrinin veya tüm grubun görünürlüğünü geçiş yapabilirsiniz.  


- **Y ekseni biçim denetimi:** y ekseni modu kullanılabilir y ekseni görünüm seçenekleri ile döngüleri. Bu denetim yalnızca farklı telemetries görselleştiriliyor zaman kullanılabilir. Üç moddan birini seçerek y eksenini ayarlayabilirsiniz:

    - **Yığılmış:** Her telemetri için bir grafik istiflenir ve grafiklerin her birinin kendi y ekseni vardır. Bu mod varsayılan olarak ayarlanır.
    - **Paylaşılan:** Her telemetri için bir grafik aynı y eksenine karşı çizilir.
    - **Çakışma:** Seçilen satıra göre y ekseni verileri değişirken, aynı y ekseninde birden çok satırı yığınlamak için kullanın.

  ![Farklı görselleştirme modları ile verileri y ekseni boyunca düzenleme](media/howto-create-analytics/yaxiscontrol.png)

- **Yakınlaştırma denetimi:** Yakınlaştırma, verilerinize daha fazla girmenizi sağlar. Sonuç setinizin içinde odaklanmak istediğiniz bir zaman dilimi bulursanız, alanı kapmak için fare işaretçinizi kullanın ve ardından seçtiğiniz son noktaya sürükleyin. Ardından seçili alana sağ tıklayın ve Yakınlaştır'ı tıklatın.

  ![Verilere yakınlaştırma](media/howto-create-analytics/zoom.png)

Elipsaltında, verilerle etkileşim kurmak için daha fazla grafik denetimi vardır.

- **Görüntü ızgarası:** Sonuçlarınız, her veri noktası için belirli değeri görüntülemenizi sağlayan bir tablo biçiminde kullanılabilir.

- **Bir İşaretçi Bırak:** 'Drop Marker' denetimi, grafikteki belirli veri noktalarını sabitlemenin bir yolunu sağlar. Farklı zaman dilimleri arasında birden çok satır için verileri karşılaştırmaya çalışırken yararlıdır.

  ![Analitiğiniz için ızgara görünümünü gösterme](media/howto-create-analytics/additionalchartcontrols.png)