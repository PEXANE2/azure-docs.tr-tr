---
title: Haritalar oluşturma
description: Bu makalede, Azure FarmBeats'te haritaların nasıl üretilenanlatılmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 92228c691c323bc0b9621dfc7413d86c5c2669e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271778"
---
# <a name="generate-maps"></a>Haritalar oluşturma

Azure FarmBeats'i kullanarak uydu görüntüleri ve sensör veri girişlerini kullanarak aşağıdaki haritaları oluşturabilirsiniz. Haritalar, çiftliğinizin coğrafi konumunu görmenize ve aygıtlarınız için uygun konumu belirlemenize yardımcı olur.

  - **Sensör Yerleştirme haritası**: Kaç sensörün kullanılacağı ve bir çiftlikte nereye yerleştirilen öneriler sağlar.
  - **Uydu Endeksleri haritası**: Bir çiftliğin bitki indeksini ve su indeksini gösterir.
  - **Toprak Nem ısı haritası**: Uydu verilerini ve sensör verilerini eriterek toprak nem dağılımını gösterir.

## <a name="sensor-placement-map"></a>Sensör Yerleştirme haritası

FarmBeats Sensör Yerleştirme haritası, toprak nem sensörlerinin yerleştirilmesinde size yardımcı olur. Harita çıkışı, sensör dağıtımı için koordinatların bir listesini içerir. Bu sensörlerden gelen girdiler, Toprak Nem ısı haritasını oluşturmak için uydu görüntüleriyle birlikte kullanılır.

Bu harita, yıl boyunca birden çok tarih boyunca görüldüğü gibi gölgelik bölümleme tarafından türetilmiştir. Çıplak toprak ve binalar bile gölgeliğin bir parçasıdır. Konumda gerekli olmayan sensörleri kaldırabilirsiniz. Bu harita rehberlik içindir ve konumunu ve sayılarını özel bilginize göre biraz değiştirebilirsiniz. Sensörlerin eklenmesi toprak nem ısı haritası sonuçlarını gerilemez, ancak sensör numarası azaltılırsa ısı haritası doğruluğunda bozulma olasılığı vardır.

## <a name="before-you-begin"></a>Başlamadan önce

Sensör Yerleştirme haritası oluşturmayı denemeden önce aşağıdaki ön koşulları karşılayın:

- Çiftlik büyüklüğü bir dönümden fazla olmalıdır.
- Bulutsuz Sentinel sahnelerinin sayısı, seçilen tarih aralığı için altıdan fazla olmalıdır.
- En az altı bulutsuz Sentinel sahnesi, 0,3'ten daha büyük veya eşit bir Normalleştirilmiş Fark Bitki Örtüsü İndeksi (NDVI) olmalıdır.

    > [!NOTE]
    > Sentinel, kullanıcı başına yalnızca iki eşzamanlı iş parçacığısağlar. Sonuç olarak, işler sıraya alınır ve tamamlanması daha uzun sürebilir.

### <a name="dependencies-on-sentinel"></a>Sentinel'e Bağımlılıklar

Aşağıdaki bağımlılıklar Sentinel ile ilgilidir:

- Uydu görüntülerini indirmek için Sentinel performansına güveniyoruz. Sentinel performans durumunu ve bakım faaliyetlerini kontrol [edin.](https://scihub.copernicus.eu/twiki/do/view/SciHubNews/WebHome)
- Sentinel, kullanıcı başına yalnızca iki eşzamanlı [indirme dizisine](https://sentinels.copernicus.eu/web/sentinel/sentinel-data-access/typologies-and-services) izin verir.
- Hassas harita oluşturma [Sentinel kapsama]( https://sentinel.esa.int/web/sentinel/user-guides/sentinel-2-msi/revisit-coverage)ve yeniden ziyaret sıklığı etkilenir.

## <a name="create-a-sensor-placement-map"></a>Sensör Yerleştirme haritası oluşturma
Bu bölümde Sensör Yerleştirme haritaları oluşturma yordamları ayrıntılı olarak anlatılıyor.

> [!NOTE]
> **Haritalar** sayfasından veya **Farm Details** sayfasındaki **Hassas Haritalar** oluşturma açılır menüsünden bir Sensör Yerleşimi haritası başlatabilirsiniz.

Şu adımları izleyin.

1. Ana sayfada, soldaki gezinti menüsünden **Haritalar'a** gidin.
2. **Haritalar Oluştur'u**seçin ve açılan menüden **Sensör Yerleşimi'ni** seçin.

    ![Sensör Yerleştirme'yi seçin](./media/get-sensor-data-from-sensor-partner/create-maps-drop-down-1.png)

3. **Sensör Yerleşimi'ni**seçtikten sonra **Sensör Yerleştirme** penceresi görüntülenir.

    ![Sensör Yerleştirme penceresi](./media/get-sensor-data-from-sensor-partner/sensor-placement-1.png)

4. **Çiftlik** açılır menüsünden bir çiftlik seçin.
   Çiftliğinizi aramak ve seçmek için açılan listede ilerleyebilir veya metin kutusuna çiftliğin adını girebilirsiniz.
5. Son bir yıla ait bir harita oluşturmak için **Önerilen'i**seçin.
6. Özel bir tarih aralığı için bir harita oluşturmak için **Tarih Aralığını Seç**seçeneğini seçin. Sensör Yerleşimi eşlemi oluşturmak istediğiniz başlangıç ve bitiş tarihini girin.
7. **Harita Oluştur'u**seçin.
 İş ayrıntılarını içeren bir onay iletisi görüntülenir.

  İş durumu hakkında bilgi için **İş İlanları Görüntüle**bölümüne bakın. İş durumu *Başarısız'ı*gösteriyorsa, *Başarısız* durumunun araç ucunda ayrıntılı bir hata iletisi görüntülenir. Bu durumda, önceki adımları yineleyin ve yeniden deneyin.

  Sorun devam ederse, [Sorun Giderme](troubleshoot-azure-farmbeats.md) bölümüne bakın veya ilgili günlüklerle [destek için Azure FarmBeats forumuna](https://aka.ms/FarmBeatsMSDN) başvurun.

### <a name="view-and-download-a-sensor-placement-map"></a>Sensör Yerleştirme haritasını görüntüleme ve indirme

Şu adımları izleyin.

1. Ana sayfada, soldaki gezinti menüsünden **Haritalar'a** gidin.

    ![Soldaki gezinme menüsünden Haritalar'ı seçin](./media/get-sensor-data-from-sensor-partner/view-download-sensor-placement-map-1.png)

2. Pencerenin sol daki navigasyonundan **Filtre'yi** seçin.
  **Filtre** penceresi arama ölçütlerini görüntüler.

    ![Filtre penceresi](./media/get-sensor-data-from-sensor-partner/view-download-filter-1.png)

3. Açılan menülerden **Tür**, **Tarih**ve **Ad** değerlerini seçin. Ardından görüntülemek istediğiniz haritayı aramak için **Uygula'yı** seçin.
  İşin oluşturulduğu tarih type_farmname_YYYY-MM-DD biçiminde gösterilir.
4. Sayfanın sonundaki gezinti çubuklarını kullanarak kullanılabilir haritalar listesinde ilerleyin.
5. Görüntülemek istediğiniz haritayı seçin. Açılır pencere, seçili haritanın önizlemesini görüntüler.
6. **İndir'i**seçin ve sensör koordinatlarının GeoJSON dosyasını indirin.

    ![Sensör Yerleştirme haritası önizlemesi](./media/get-sensor-data-from-sensor-partner/download-sensor-placement-map-1.png)

## <a name="satellite-indices-map"></a>Uydu Endeksleri haritası

Aşağıdaki bölümlerde Uydu Endeksleri haritasının oluşturulması ve görüntülenmesiyle ilgili prosedürler açıklayınız.

> [!NOTE]
> **Haritalar** sayfasından veya **Farm Details** sayfasındaki **Hassas Haritalar** oluşturma menüsünden bir Uydu Endeksleri haritası başlatabilirsiniz.

FarmBeats size çiftlikler için NDVI, Geliştirilmiş Bitki İndeksi (EVI) ve Normalleştirilmiş Fark Su Endeksi (NDWI) haritaları oluşturma olanağı sağlar. Bu endeksler, mahsulün şu anda nasıl büyüdüğünü veya geçmişte nasıl büyüdüğünü ve topraktaki temsili su seviyelerini belirlemeye yardımcı olur.


> [!NOTE]
> Haritanın oluşturulduğu günler için Sentinel görüntüsü gereklidir.


## <a name="create-a-satellite-indices-map"></a>Uydu Endeksleri haritası oluşturma

Şu adımları izleyin.

1. Ana sayfada, soldaki gezinti menüsünden **Haritalar'a** gidin.
2. **Haritalar Oluştur'u**seçin ve açılan menüden **Uydu Endeksleri'ni** seçin.

    ![Açılan menüden Uydu Endeksleri'ni seçin](./media/get-sensor-data-from-sensor-partner/create-maps-drop-down-satellite-indices-1.png)

3. **Uydu Endeksleri'ni**seçtikten sonra **Uydu Endeksleri** penceresi görüntülenir.

    ![Uydu Endeksleri penceresi](./media/get-sensor-data-from-sensor-partner/satellitte-indices-1.png)

4. Açılan menüden bir çiftlik seçin.
   Çiftliğinizi aramak ve seçmek için açılan listede gezinebilir veya çiftliğin adını girebilirsiniz.   
5. Son hafta için bir harita oluşturmak için **Bu Hafta'yı**seçin.
6. Özel bir tarih aralığı için bir harita oluşturmak için **Tarih Aralığını Seç**seçeneğini seçin. Uydu Endeksleri haritasını oluşturmak istediğiniz başlangıç ve bitiş tarihini girin.
7. **Harita Oluştur'u**seçin.
    İş ayrıntılarını içeren bir onay iletisi görüntülenir.

    ![Uydu Endeksleri harita onay mesajı](./media/get-sensor-data-from-sensor-partner/successful-satellitte-indices-1.png)

    İş durumu hakkında bilgi için **İş İlanları Görüntüle**bölümüne bakın. İş durumu *Başarısız'ı*gösteriyorsa, *Başarısız* durumunun araç ucunda ayrıntılı bir hata iletisi görüntülenir. Bu durumda, önceki adımları yineleyin ve yeniden deneyin.

    Sorun devam ederse, [Sorun Giderme](troubleshoot-azure-farmbeats.md) bölümüne bakın veya ilgili günlüklerle [destek için Azure FarmBeats forumuna](https://aka.ms/FarmBeatsMSDN) başvurun.

### <a name="view-and-download-a-map"></a>Haritayı görüntüleme ve indirme

Şu adımları izleyin.

1. Ana sayfada, soldaki gezinti menüsünden **Haritalar'a** gidin.

    ![Haritaları Seçin](./media/get-sensor-data-from-sensor-partner/view-download-sensor-placement-map-1.png)

2. Pencerenin sol daki navigasyonundan **Filtre'yi** seçin. **Filtre** penceresi arama ölçütlerini görüntüler.

    ![Filtre penceresi arama ölçütlerini görüntüler](./media/get-sensor-data-from-sensor-partner/view-download-filter-1.png)

3. Açılan menülerden **Tür**, **Tarih**ve **Ad** değerlerini seçin. Ardından görüntülemek istediğiniz haritayı aramak için **Uygula'yı** seçin.
  İşin oluşturulduğu tarih type_farmname_YYYY-MM-DD biçiminde gösterilir.

4. Sayfanın sonundaki gezinti çubuklarını kullanarak kullanılabilir haritalar listesinde ilerleyin.
5. **Çiftlik Adı** ve **Tarihi'nin**her birleşimi için aşağıdaki üç harita mevcuttur:
    - NDVI
    - EVİ
    - NDWI
6. Görüntülemek istediğiniz haritayı seçin. Açılır pencere, seçili haritanın önizlemesini görüntüler.
7. İndirme biçimini seçmek için açılır menüden **İndir'i** seçin. Harita indirilir ve bilgisayarınızdaki yerel klasörünüzde saklanır.

    ![Seçili Uydu Endeksleri harita önizlemesi](./media/get-sensor-data-from-sensor-partner/download-satellite-indices-map-1.png)

## <a name="soil-moisture-heatmap"></a>Toprak Nem ısı haritası

Toprak nemi, toprak parçacıkları arasındaki boşluklarda tutulan sudur.Toprak Nem ısı haritası, çiftlik içinde yüksek çözünürlükte, herhangi bir derinlikte toprak nem verilerini anlamanıza yardımcı olur. Doğru ve kullanılabilir bir Toprak Nem ısı haritası oluşturmak için, sensörlerin tek tip bir dağıtım gereklidir. Tüm sensörler aynı sağlayıcıdan olmalı. Farklı sağlayıcılar kalibrasyon farklılıkları ile birlikte toprak nem ölçülür şekilde farklılıklar var. Isı haritası, o derinlikte konuşlandırılan sensörler kullanılarak belirli bir derinlik için oluşturulur.

### <a name="before-you-begin"></a>Başlamadan önce

Toprak Nem ısı haritası oluşturmayı denemeden önce aşağıdaki ön koşulları karşıla:

- En az üç toprak nem sensörü yerleştirilmelidir. Sensörler dağıtılmadan ve çiftlikle ilişkilendirilmeden önce bir Toprak Nem ısı haritası oluşturmaya çalışmayın.
- Toprak Nem ısı haritası oluşturmak Sentinel'in yol kapsama alanı, bulut örtüsü ve bulut gölgesi tarafından etkilenir. Toprak Nem ısı haritasının istendiği günden itibaren son 120 gün boyunca en az bir bulutsuz Sentinel Sahnesi hazır olmalıdır.
- Çiftlikte dağıtılan sensörlerin en az yarısı çevrimiçi olmalı ve veri hub'ına veri akışına sahip olmalıdır.
- Isı haritası aynı sağlayıcıdan sensör önlemleri kullanılarak oluşturulmalıdır.


## <a name="create-a-soil-moisture-heatmap"></a>Toprak Nem ısı haritası oluşturun

Şu adımları izleyin.

1. Ana sayfada, **Haritalar** sayfasını görüntülemek için soldaki gezinti menüsünden **Haritalar'a** gidin.
2. **Harita Oluştur'u**seçin ve açılan menüden **Toprak Nemi'ni** seçin.

    ![Açılan menüden Toprak Nemi'ni seçin](./media/get-sensor-data-from-sensor-partner/create-maps-drop-down-soil-moisture-1.png)

3. **Toprak Nemi'ni**seçtikten **sonra, Toprak Nem** penceresi belirir.

    ![Toprak Nem penceresi](./media/get-sensor-data-from-sensor-partner/soil-moisture-1.png)

4. **Çiftlik** açılır menüsünden bir çiftlik seçin.
   Çiftliğinizi aramak ve seçmek için açılan listeden kaydırabilir veya **çiftliğin** açılır menüsüne çiftliğin adını girebilirsiniz.
5. Select **Soil Nem Sensörü Ölçümü** açılır menüsünde, haritayı oluşturmak istediğiniz toprak nem sensörü ölçüsünü (derinlik) seçin.
Sensör ölçüsünü bulmak için **Sensörler'e**gidin ve herhangi bir toprak nem sensörü seçin. Daha sonra, **Sensör Özellikleri** bölümünün altında, Ölçü **Adı'ndaki**değeri kullanın.
6. **Bugün** veya **Bu Hafta**için bir harita oluşturmak için seçeneklerden birini seçin.
7. Özel bir tarih aralığı için bir harita oluşturmak için **Tarih Aralığını Seç**seçeneğini seçin. Toprak Nem ısı haritasını oluşturmak istediğiniz başlangıç ve bitiş tarihini girin.
8. **Harita Oluştur'u**seçin.
 İş ayrıntılarını içeren bir onay iletisi görüntülenir.

   ![Toprak Nem haritası onay mesajı](./media/get-sensor-data-from-sensor-partner/successful-soil-moisture-1.png)

    İş durumu hakkında bilgi için **İş İlanları Görüntüle**bölümüne bakın. İş durumu *Başarısız'ı*gösteriyorsa, *Başarısız* durumunun araç ucunda ayrıntılı bir hata iletisi görüntülenir. Bu durumda, önceki adımları yineleyin ve yeniden deneyin.

    Sorun devam ederse, [Sorun Giderme](troubleshoot-azure-farmbeats.md) bölümüne bakın veya ilgili günlüklerle [destek için Azure FarmBeats forumuna](https://aka.ms/FarmBeatsMSDN) başvurun.

### <a name="view-and-download-a-map"></a>Haritayı görüntüleme ve indirme

Şu adımları izleyin.

1. Ana sayfada, soldaki gezinti menüsünden **Haritalar'a** gidin.

    ![Haritalara Git](./media/get-sensor-data-from-sensor-partner/view-download-sensor-placement-map-1.png)

2. Pencerenin sol daki navigasyonundan **Filtre'yi** seçin. **Filtre** penceresi, haritaları nerede arayabilirsiniz görüntüler.

    ![Sol navigasyondan Filtre'yi seçin](./media/get-sensor-data-from-sensor-partner/view-download-filter-1.png)

3.  Açılan menülerden **Tür**, **Tarih**ve **Ad** değerlerini seçin. Ardından görüntülemek istediğiniz haritayı aramak için **Uygula'yı** seçin. İşin oluşturulduğu tarih type_farmname_YYYY-MM-DD biçiminde gösterilir.
4. **Çiftliğe,** **Tarihe**, **Açık Oluşturulana,** **İş Kimliğine**ve **İş Türüne**göre sıralamak için tablo üsterlerin yanındaki **Sıralama** simgesini seçin.
5. Sayfanın sonundaki gezinti düğmelerini kullanarak kullanılabilir haritalar listesinde ilerleyin.
6. Görüntülemek istediğiniz haritayı seçin. Açılır pencere, seçili haritanın önizlemesini görüntüler.
7. İndirme biçimini seçmek için açılır menüden **İndir'i** seçin. Harita indirilir ve belirtilen klasörde saklanır.

    ![Toprak Nem ısı haritası önizleme](./media/get-sensor-data-from-sensor-partner/download-soil-moisture-map-1.png)
