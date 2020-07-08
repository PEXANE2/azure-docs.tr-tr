---
title: Haritalar oluşturma
description: Bu makalede, Azure Farmtts 'de eşlemelerin nasıl oluşturulacağı açıklanır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 92228c691c323bc0b9621dfc7413d86c5c2669e7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84709071"
---
# <a name="generate-maps"></a>Haritalar oluşturma

Azure Farmıts 'yi kullanarak, uydu Imagery ve algılayıcı veri girişlerini kullanarak aşağıdaki haritaları oluşturabilirsiniz. Haritalar, grubunuzun coğrafi konumunu görmenizi ve cihazlarınız için uygun konumu tanımlamanızı sağlar.

  - **Algılayıcı yerleştirme Haritası**: kaç sensörün ve bir gruba nereye yerleştirileceğini gösteren öneriler sağlar.
  - **Uydu dizinleri Haritası**: bir grup için vegetation dizinini ve su dizinini gösterir.
  - **SOIL nemi ısı haritasını**: uydu verilerini ve algılayıcı verilerini kullanarak SOIL nemi dağıtımını gösterir.

## <a name="sensor-placement-map"></a>Algılayıcı yerleştirme Haritası

Bir farmtörü algılayıcı yerleştirme Haritası, SOIL nemi algılayıcılarının yerleştirilmesi için size yardımcı olur. Harita çıktısı, algılayıcı dağıtımı için bir koordinat listesinden oluşur. Bu sensörlerden girişler, SOIL Moisture heatmap 'i oluşturmak için uydu canlandırın ile birlikte kullanılır.

Bu harita, yıl boyunca birden çok tarih üzerinde görüldüğü gibi kanopi 'yi parçalara ayırarak türetilir. Hatta çıplak ve binalar de Canopy 'nin bir parçasıdır. Konumda gerekli olmayan algılayıcıları kaldırabilirsiniz. Bu harita rehberlik içindir ve özel bilginiz temelinde konumu ve sayıları biraz daha değiştirebilirsiniz. Sensörlerde algılayıcısı eklemek SOIL nemi ısı haritasını sonucunu vermeyecek, ancak algılayıcı numarası azaltıldığında, ısı haritasını doğruluğu üzerinde ayrım yapma olasılığı vardır.

## <a name="before-you-begin"></a>Başlamadan önce

Bir algılayıcı yerleştirme eşlemesi oluşturmayı denemeden önce aşağıdaki önkoşulları karşılaın:

- Grup boyutu birden fazla Acre olmalıdır.
- Bulut içermeyen Sentinel sahneleri sayısı, seçilen tarih aralığı için altıdan büyük olmalıdır.
- En az altı bulut ücretsiz Sentinel sahneleri, 0,3 ' ten büyük veya buna eşit bir normalleştirilmiş fark vegetation dizinine (NDVı) sahip olmalıdır.

    > [!NOTE]
    > Sentinel Kullanıcı başına yalnızca iki eşzamanlı iş parçacığına izin verir. Sonuç olarak, işler kuyruğa alınır ve tamamlanması daha uzun sürebilir.

### <a name="dependencies-on-sentinel"></a>Sentinel üzerinde bağımlılıklar

Aşağıdaki bağımlılıklar Sentinel 'e aittir:

- Uydu görüntülerini indirmek için Sentinel performansına bağımlıdır. Sentinel performans durumunu ve bakım [etkinliklerini](https://scihub.copernicus.eu/twiki/do/view/SciHubNews/WebHome)denetleyin.
- Sentinel, Kullanıcı başına yalnızca iki eşzamanlı [indirme iş parçacığına](https://sentinels.copernicus.eu/web/sentinel/sentinel-data-access/typologies-and-services) izin verir.
- Duyarlık eşleme oluşturma işlemi [Sentinel kapsam ve geri ödeme sıklığından]( https://sentinel.esa.int/web/sentinel/user-guides/sentinel-2-msi/revisit-coverage)etkilenir.

## <a name="create-a-sensor-placement-map"></a>Algılayıcı yerleştirme eşlemesi oluşturma
Bu bölümde, algılayıcı yerleştirme haritaları oluşturma yordamları ayrıntılı olarak yer almamıştır.

> [!NOTE]
> **Haritalar** sayfasından veya **Grup ayrıntıları** sayfasındaki **duyarlık haritaları oluştur** açılır menüsünden bir algılayıcı yerleştirme Haritası başlatabilirsiniz.

Şu adımları izleyin.

1. Giriş sayfasında sol gezinti menüsünden **haritalar** ' a gidin.
2. **Haritalar oluştur**' u seçin ve açılan menüden **algılayıcı yerleşimi** ' nı seçin.

    ![Algılayıcı yerleşimini seçin](./media/get-sensor-data-from-sensor-partner/create-maps-drop-down-1.png)

3. **Algılayıcı yerleştirmesini**seçtikten sonra, **algılayıcı yerleşimi** penceresi görüntülenir.

    ![Algılayıcı yerleştirme penceresi](./media/get-sensor-data-from-sensor-partner/sensor-placement-1.png)

4. **Grup** açılan menüsünden bir grup seçin.
   Grubunuzu aramak ve seçmek için, açılan listede kaydırma yapabilir veya metin kutusuna grubun adını girebilirsiniz.
5. Son yıl için bir harita oluşturmak için **Önerilen**' ı seçin.
6. Özel bir tarih aralığı için bir harita oluşturmak için, **Tarih aralığını Seç**seçeneğini belirleyin. Algılayıcı yerleşimi eşlemesini oluşturmak istediğiniz başlangıç ve bitiş tarihini girin.
7. **Haritaları oluştur**' u seçin.
 İş ayrıntıları içeren bir onay iletisi görüntülenir.

  İş durumu hakkında bilgi için bkz. **Işleri görüntüleme**. İş durumu *başarısız*gösteriyorsa, *başarısız* durumundaki araç ipucunda ayrıntılı bir hata iletisi görüntülenir. Bu durumda, önceki adımları yineleyin ve yeniden deneyin.

  Sorun devam ederse, ilgili günlüklere yönelik destek için [sorun giderme](troubleshoot-azure-farmbeats.md) bölümüne bakın veya [Azure farmtempts forumuna](https://aka.ms/FarmBeatsMSDN) başvurun.

### <a name="view-and-download-a-sensor-placement-map"></a>Bir algılayıcı yerleşimi haritasını görüntüleyin ve indirin

Şu adımları izleyin.

1. Giriş sayfasında sol gezinti menüsünden **haritalar** ' a gidin.

    ![Sol gezinti menüsünden Haritalar ' ı seçin](./media/get-sensor-data-from-sensor-partner/view-download-sensor-placement-map-1.png)

2. Pencerenin sol gezinti bölmesinde **filtre** ' yi seçin.
  **Filtre** penceresi arama ölçütlerini görüntüler.

    ![Filtre penceresi](./media/get-sensor-data-from-sensor-partner/view-download-filter-1.png)

3. Açılır menülerden **tür**, **Tarih**ve **ad** değerlerini seçin. Ardından, görüntülemek istediğiniz Haritayı aramak için **Uygula** ' yı seçin.
  İşin oluşturulduğu tarih type_farmname_YYYY-AA-GG biçiminde gösterilir.
4. Sayfanın sonundaki gezinti çubuklarını kullanarak kullanılabilir haritalar listesinde gezinin.
5. Görüntülemek istediğiniz haritayı seçin. Bir açılır pencere, seçilen haritanın önizlemesini görüntüler.
6. **İndir**' i seçin ve sensör koordinatlarının geojson dosyasını indirin.

    ![Algılayıcı yerleştirme Haritası önizlemesi](./media/get-sensor-data-from-sensor-partner/download-sensor-placement-map-1.png)

## <a name="satellite-indices-map"></a>Uydu dizinleri Haritası

Aşağıdaki bölümlerde bir uydu dizinleri eşlemesi oluşturma ve görüntüleme ile ilgili yordamlar açıklanmaktadır.

> [!NOTE]
> **Haritalar** sayfasından veya **Grup ayrıntıları** sayfasında **duyarlık haritaları oluştur** açılır menüsünden bir uydu dizinleri Haritası başlatabilirsiniz.

Farmpts, gruplar için NDVı, gelişmiş vegetation dizini (EVI) ve normalleştirilmiş fark su dizini (NDWI) haritalarını oluşturma olanağı sağlar. Bu dizinler, kırpmadaki Şu anda büyümekte olan veya geçmişte büyümekte olan ya da zemin içindeki temsili su düzeylerini belirlemesine yardımcı olur.


> [!NOTE]
> Bir Sentinel görüntüsü, eşlemenin oluşturulduğu günler için gereklidir.


## <a name="create-a-satellite-indices-map"></a>Uydu dizinleri eşlemesi oluşturma

Şu adımları izleyin.

1. Giriş sayfasında sol gezinti menüsünden **haritalar** ' a gidin.
2. **Haritalar oluştur**' u seçin ve açılan menüden **uydu dizinleri** ' ni seçin.

    ![Açılan menüden uydu dizinleri ' ni seçin](./media/get-sensor-data-from-sensor-partner/create-maps-drop-down-satellite-indices-1.png)

3. **Uydu dizinleri**' ni seçtikten sonra **uydu dizinleri** penceresi görüntülenir.

    ![Uydu endeksleri penceresi](./media/get-sensor-data-from-sensor-partner/satellitte-indices-1.png)

4. Açılan menüden bir grup seçin.
   Grubunuzu aramak ve seçmek için, aşağı açılan listede kaydırma yapabilir veya grubun adını girebilirsiniz.   
5. Geçen hafta için bir harita oluşturmak üzere **Bu hafta**' yı seçin.
6. Özel bir tarih aralığı için bir harita oluşturmak için, **Tarih aralığını Seç**seçeneğini belirleyin. Uydu dizinleri eşlemesini oluşturmak istediğiniz başlangıç ve bitiş tarihini girin.
7. **Haritaları oluştur**' u seçin.
    İş ayrıntıları içeren bir onay iletisi görüntülenir.

    ![Uydu dizinleri eşleme onayı iletisi](./media/get-sensor-data-from-sensor-partner/successful-satellitte-indices-1.png)

    İş durumu hakkında bilgi için bkz. **Işleri görüntüleme**. İş durumu *başarısız*gösteriyorsa, *başarısız* durumundaki araç ipucunda ayrıntılı bir hata iletisi görüntülenir. Bu durumda, önceki adımları yineleyin ve yeniden deneyin.

    Sorun devam ederse, ilgili günlüklere yönelik destek için [sorun giderme](troubleshoot-azure-farmbeats.md) bölümüne bakın veya [Azure farmtempts forumuna](https://aka.ms/FarmBeatsMSDN) başvurun.

### <a name="view-and-download-a-map"></a>Harita görüntüleme ve indirme

Şu adımları izleyin.

1. Giriş sayfasında sol gezinti menüsünden **haritalar** ' a gidin.

    ![Haritaları Seç](./media/get-sensor-data-from-sensor-partner/view-download-sensor-placement-map-1.png)

2. Pencerenin sol gezinti bölmesinde **filtre** ' yi seçin. **Filtre** penceresi arama ölçütlerini görüntüler.

    ![Filtre penceresi arama ölçütlerini görüntüler](./media/get-sensor-data-from-sensor-partner/view-download-filter-1.png)

3. Açılır menülerden **tür**, **Tarih**ve **ad** değerlerini seçin. Ardından, görüntülemek istediğiniz Haritayı aramak için **Uygula** ' yı seçin.
  İşin oluşturulduğu tarih type_farmname_YYYY-AA-GG biçiminde gösterilir.

4. Sayfanın sonundaki gezinti çubuklarını kullanarak kullanılabilir haritalar listesinde gezinin.
5. Her **Grup adı** ve **tarihin**birleşimi için aşağıdaki üç eşleme kullanılabilir:
    - NDVı
    - EVI
    - NDWI
6. Görüntülemek istediğiniz haritayı seçin. Bir açılır pencere, seçilen haritanın önizlemesini görüntüler.
7. İndirme biçimini seçmek için açılan menüden **İndir** ' i seçin. Eşleme indirilir ve bilgisayarınızdaki yerel klasörünüzde depolanır.

    ![Seçili uydu dizinleri eşleme önizlemesi](./media/get-sensor-data-from-sensor-partner/download-satellite-indices-map-1.png)

## <a name="soil-moisture-heatmap"></a>SOIL Moisture ısı haritasını

SOIL nemi, SOIL parçacıkları arasındaki boşlukların tutulduğu su.SOIL nemi ısı haritasını, grubunuzun içinde yüksek çözünürlükte SOIL nemi verilerini herhangi bir derinlikte anlamanıza yardımcı olur. Doğru ve kullanılabilir bir SOIL Moisture heatmap oluşturmak için, tek bir algılayıcı dağıtımı gereklidir. Tüm sensörlerden aynı sağlayıcıdan olması gerekir. Farklı sağlayıcılarda, SOIL nemi 'in ayarlama farklılıklarıyla birlikte ölçüldüğü fark vardır. Isı haritasını, bu derinlikte dağıtılan sensörler kullanılarak belirli bir derinlik için oluşturulur.

### <a name="before-you-begin"></a>Başlamadan önce

SOIL Moisture heatmap oluşturmayı denemeden önce aşağıdaki önkoşulları karşılayın:

- En az üç Soil nemi sensörünün dağıtılması gerekir. Algılayıcılar dağıtılmadan ve grupla ilişkilendirilmeden önce SOIL Moisture ısı haritasını oluşturmayı denemeyin.
- SOIL Moisture ısı haritasını oluşturmak Sentinel 'in yol kapsamı, bulut kapağı ve bulut gölgesi tarafından etkilenir. En az bir adet bulut ücretsiz Sentinel görünümü, SOIL Moisture ısı haritasını 'in istendiği günden itibaren son 120 gün için kullanılabilir olmalıdır.
- Grupta dağıtılan sensörların en az yarısı çevrimiçi olmalı ve veri hub 'ına veri akışı olmalıdır.
- Isı haritasını 'in aynı sağlayıcıdan gelen algılayıcı ölçüleri kullanılarak oluşturulması gerekir.


## <a name="create-a-soil-moisture-heatmap"></a>SOIL Moisture ısı haritasını oluşturma

Şu adımları izleyin.

1. Giriş sayfasında, **haritalar** sayfasını görüntülemek için sol gezinti menüsünden **haritalar** ' a gidin.
2. **Haritalar oluştur**' u seçin ve açılan menüden **SOIL Moisture** ' i seçin.

    ![Açılan menüden Soil Moisture ' i seçin](./media/get-sensor-data-from-sensor-partner/create-maps-drop-down-soil-moisture-1.png)

3. **Soil Moisture**' i seçtikten sonra **Soil Moisture** penceresi görüntülenir.

    ![SOIL Moisture penceresi](./media/get-sensor-data-from-sensor-partner/soil-moisture-1.png)

4. **Grup** açılan menüsünden bir grup seçin.
   Grubunuzu aramak ve seçmek için, açılan listeden kaydırma yapabilir veya grup **Seç** açılan menüsünde grubun adını girebilirsiniz.
5. **SOIL nemi algılayıcı ölçüsünü Seç** açılan menüsünde, eşlemesini oluşturmak istediğiniz SOIL nemi algılayıcı ölçüsünü (derinlik) seçin.
Algılayıcı ölçüsünü bulmak için **sensörler**' e gidin ve herhangi bir SOIL nemi algılayıcısı seçin. Ardından, **algılayıcı özellikleri** bölümünde, **Ölçü adı**' nın değerini kullanın.
6. **Bugün** veya **Bu hafta**için bir harita oluşturmak üzere seçeneklerden birini seçin.
7. Özel bir tarih aralığı için bir harita oluşturmak için, **Tarih aralığını Seç**seçeneğini belirleyin. SOIL Moisture heatmap 'i oluşturmak istediğiniz başlangıç ve bitiş tarihini girin.
8. **Haritaları oluştur**' u seçin.
 İş ayrıntıları içeren bir onay iletisi görüntülenir.

   ![SOIL Moisture eşleme onayı iletisi](./media/get-sensor-data-from-sensor-partner/successful-soil-moisture-1.png)

    İş durumu hakkında bilgi için bkz. **Işleri görüntüleme**. İş durumu *başarısız*gösteriyorsa, *başarısız* durumundaki araç ipucunda ayrıntılı bir hata iletisi görüntülenir. Bu durumda, önceki adımları yineleyin ve yeniden deneyin.

    Sorun devam ederse, ilgili günlüklere yönelik destek için [sorun giderme](troubleshoot-azure-farmbeats.md) bölümüne bakın veya [Azure farmtempts forumuna](https://aka.ms/FarmBeatsMSDN) başvurun.

### <a name="view-and-download-a-map"></a>Harita görüntüleme ve indirme

Şu adımları izleyin.

1. Giriş sayfasında sol gezinti menüsünden **haritalar** ' a gidin.

    ![Haritalar 'a git](./media/get-sensor-data-from-sensor-partner/view-download-sensor-placement-map-1.png)

2. Pencerenin sol gezinti bölmesinde **filtre** ' yi seçin. **Filtre** penceresi haritaları arayınızdan görüntülenir.

    ![Sol gezinmede filtre Seç](./media/get-sensor-data-from-sensor-partner/view-download-filter-1.png)

3.  Açılır menülerden **tür**, **Tarih**ve **ad** değerlerini seçin. Ardından, görüntülemek istediğiniz Haritayı aramak için **Uygula** ' yı seçin. İşin oluşturulduğu tarih type_farmname_YYYY-AA-GG biçiminde gösterilir.
4. **Grup**, tarih, **Oluşturulma** **tarihi**, **iş kimliği**ve **Iş türüne**göre sıralamak için tablo üst bilgilerinin yanındaki **sıralama** simgesini seçin.
5. Sayfanın sonundaki gezinti düğmelerini kullanarak kullanılabilir haritalar listesinde gezinin.
6. Görüntülemek istediğiniz haritayı seçin. Bir açılır pencere, seçilen haritanın önizlemesini görüntüler.
7. İndirme biçimini seçmek için açılan menüden **İndir** ' i seçin. Eşleme indirilip belirtilen klasöre depolanır.

    ![SOIL Moisture ısı haritasını önizlemesi](./media/get-sensor-data-from-sensor-partner/download-soil-moisture-map-1.png)
