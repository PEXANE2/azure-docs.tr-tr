---
title: Toprak Nem Isı Haritası oluşturun
description: Azure FarmBeats'te Toprak Nem Isı Haritasınasıl üretilir açıklar
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: a2115e9c1601c86cce8857c10baf12b91cc2b997
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75482575"
---
# <a name="generate-soil-moisture-heatmap"></a>Toprak Nem Isı Haritası oluşturun

Toprak nemi, toprak parçacıkları arasındaki boşluklarda tutulan sudur.Toprak Nem Isı Haritası, çiftliklerinizdeki nem verilerini her derinlikte ve yüksek çözünürlükte anlamanıza yardımcı olur. Doğru ve kullanılabilir bir toprak nem ısı haritası oluşturmak için, aynı sağlayıcıdan sensörlerin tek tip dağıtım gereklidir. Farklı sağlayıcılar kalibrasyon farklılıkları ile birlikte toprak nem ölçülür şekilde farklılıklar olacaktır. Isı Haritası, bu derinlikte konuşlandırılan sensörler kullanılarak belirli bir derinlik için oluşturulur.

Bu makalede, Azure FarmBeats Hızlandırıcı'yı kullanarak çiftliğiniz için bir Toprak Nem Isı Haritası oluşturma işlemi açıklanmaktadır. Bu makalede, nasıl öğreneceksiniz:

- [Çiftlikler Oluştur](#create-a-farm)
- [Çiftliklere sensör atama](#get-soil-moisture-sensor-data-from-partner)
- [Toprak Nem Isı Haritası oluşturun](#generate-soil-moisture-heatmap)

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdakilerden emin olun:  

- Azure aboneliği.
- Azure FarmBeats'in çalışan bir örneği.
- Çiftlik için en az üç toprak nem sensörü mevcuttur.

## <a name="create-a-farm"></a>Çiftlik oluşturma

Bir çiftlik için bir toprak nem ısı haritası oluşturmak istediğiniz ilgi coğrafi bir alandır. [Farms API'yi](https://aka.ms/FarmBeatsDatahubSwagger) kullanarak veya [FarmsBeats Hızlandırıcı UI'de](manage-farms-in-azure-farmbeats.md#create-farms) bir çiftlik oluşturabilirsiniz

## <a name="deploy-sensors"></a>Sensörleri dağıtın

Çiftlikte ki toprak nem sensörlerini fiziksel olarak konuşlandırmanız gerekir. [Davis Instruments](https://www.davisinstruments.com/product/enviromonitor-gateway/) ve [Teralitik](https://teralytic.com/)- Bizim onaylı ortakların herhangi bir toprak nem sensörleri satın alabilirsiniz. Çiftliğinizdeki fiziksel kurulumu yapmak için sensör sağlayıcınızla işbirliği yapmalısınız.

## <a name="get-soil-moisture-sensor-data-from-partner"></a>İş ortağından toprak nem sensörü verilerini alın

Sensörler akış akışına, iş ortağı veri panosuna veri akışı na başladıkça, verileri Azure FarmBeats'e dönüştürür. Bu ortak uygulamadan yapılabilir.

Örneğin, Davis sensörleri satın aldıysanız, hava durumu bağlantı hesabınıza giriş yapacak ve verilerin Azure FarmBeats'e aktamasını sağlamak için gerekli kimlik bilgilerini sağlayacaksınız. Gerekli kimlik bilgilerini almak için [sensör verilerini al'dan](get-sensor-data-from-sensor-partner.md#get-sensor-data-from-sensor-partners)talimatları izleyin.

Kimlik bilgilerinizi girdikten ve iş ortağı uygulamasında **Gönder'i** seçtikten sonra, verilerin Azure FarmBeats'e akmasını sağlayabilirsiniz.

### <a name="assign-soil-moisture-sensors-to-the-farm"></a>Toprak nem sensörlerini çiftliğe atama

Sensör hesabınızı Azure FarmBeats'e bağladıktan sonra, toprak nem sensörlerini ilgi çekici çiftliğe atamanız gerekir.

1.  Ana sayfada, menüden **Çiftlikler'i** seçin, **Çiftlikler** liste sayfası görüntülenir.
2.  **MyFarm** > **Ekle Cihazlarını**seçin.
3.  **Aygıt Ekle** penceresi görüntülenir. Çiftliğiniz için toprak nem sensörlerine bağlı herhangi bir cihaz seçin.

    ![Proje Çiftlik Beats](./media/get-sensor-data-from-sensor-partner/add-devices-1.png)

4. **Aygıt Ekle'yi**seçin.     

## <a name="generate-soil-moisture-heatmap"></a>Toprak Nem Isı Haritası oluşturun

Bu adım bir iş ya da çiftlik için Toprak Nem Isı Haritası üretecek uzun süren bir operasyon oluşturmaktır.

1.  Ana sayfada, çiftlikler **Farms** sayfasını görüntülemek için soldaki gezinti menüsünden Çiftlikler'e gidin.
2.  **MyFarm'ı**seçin.
3.  Çiftlik **Ayrıntıları** sayfasında **Hassas Harita Oluştur'u**seçin.
4.  Açılan menüden **Toprak Nemi'ni**seçin.
5.  Toprak **Nem** penceresinde, **Bu Hafta**seçin.
6.  Toprak Nem Sensörü **Ölçümü'nü seçin,** **Sensor Measure**harita için kullanmak istediğiniz ölçüyünü girin.
    Sensör ölçüsünü bulmak **için, Sensörlerde,** herhangi bir toprak nem sensörünü seçin. **Sensör Özellikleri'nde** **Ölçü Adı** değerini kullanın.

    ![Proje Çiftlik Beats](./media/get-sensor-data-from-sensor-partner/soil-moisture-1.png)


7.  **Harita Oluştur'u**seçin.
    İş ayrıntılarını içeren bir onay iletisi görüntülenir. Daha fazla bilgi için İş İşlerinde İş Durumu'na bakın.

    >[!NOTE]
    > İşi tamamlaması yaklaşık üç ila dört saat sürer.

### <a name="download-the-soil-moisture-heatmap"></a>Toprak Nem Isı Haritası indir

Aşağıdaki adımları kullanın:

1. **İşler** sayfasında, son yordamda oluşturduğunuz iş için **İş Durumu'nu** denetleyin.
2. İş durumu **Başarılı'yı**gösterdiğinde, menüde **Haritalar'ı** seçin.
3. Haritayı, toprak moisture_MyFarm_YYYY-MM-DD> <biçiminde oluşturulduğu güne kadar arayın.
4. **Ad** sütununda bir harita seçin, seçili haritanın önizlemesini içeren açılır pencere görüntülenir.
5. **Download** (İndir) seçeneğini belirleyin. Harita indirilir ve bilgisayarınızın yerel klasörüne saklanır.

    ![Proje Çiftlik Beats](./media/get-sensor-data-from-sensor-partner/download-soil-moisture-map-1.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık başarılı bir Toprak Nem Isı Haritası oluşturduk, [sensör yerleştirme](generate-maps-in-azure-farmbeats.md#sensor-placement-map) oluşturmak ve [tarihsel telemetri verileri yutmak](ingest-historical-telemetry-data-in-azure-farmbeats.md)öğrenin. 
