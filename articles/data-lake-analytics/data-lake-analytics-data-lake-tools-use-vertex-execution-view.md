---
title: Visual Studio için Veri Gölü Araçlarında Vertex Yürütme Görünümü
description: Bu makalede, Veri Gölü Analizi işlerini sınamak için Vertex Yürütme Görünümü nasıl kullanılacağı açıklanmaktadır.
services: data-lake-analytics
ms.service: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.topic: conceptual
ms.date: 10/13/2016
ms.openlocfilehash: f5adbb75e6852551976aa040a1a1c723d2e3f59b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71309722"
---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Visual Studio için Veri Gölü Araçlarında Vertex Yürütme Görünümünü Kullanın
Data Lake Analytics işlerini sınavda Vertex Yürütme Görünümü'nü nasıl kullanacağınızı öğrenin.


## <a name="open-the-vertex-execution-view"></a>Vertex Yürütme Görünümünü Aç
Visual Studio için Data Lake Tools'ta bir U-SQL işi açın. Sol alt köşede **Vertex Yürütme Görünümü'ne** tıklayın. Önce profilleri yüklemeniz istenebilir ve ağ bağlantınıza bağlı olarak biraz zaman alabilir.

![Veri Gölü Analizi Araçları Vertex Yürütme Görünümü](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Vertex Yürütme Görünümünü Anlayın
Vertex Yürütme Görünümü üç bölümden oluşur:

![Veri Gölü Analizi Araçları Vertex Yürütme Görünümü](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

Soldaki **Vertex seçici,** özelliklere göre vertices seçmenizi sağlar (en iyi 10 veri okuma gibi, ya da aşama seçin). En sık kullanılan filtrelerden biri **kritik yolda vertices**görmektir. **Kritik yol,** bir U-SQL işinin en uzun vertices zinciridir. Kritik yolu anlamak, hangi tepe noktasının en uzun zaman aldığını kontrol ederek işinizi optimize etmek için yararlıdır.
  
![Veri Gölü Analizi Araçları Vertex Yürütme Görünümü](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

Üst orta **bölmetüm vertices çalışan durumunu**gösterir.
  
![Veri Gölü Analizi Araçları Vertex Yürütme Görünümü](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

Alt orta bölme, her tepe noktası hakkında bilgi gösterir:
* İşlem Adı: Vertex örneğinin adı. It is composed of different parts in StageName| VertexName| VertexRunInstance. Örneğin, SV7_Split[62].v1 vertex, Aşama SV7_Split'daki 62 numaralı Vertex sayısının ikinci çalışma örneğini (.v1, 0'dan başlayarak dizin) anlamına gelir.
* Toplam Veri Okuma/Yazma: Veriler bu tepe tarafından okundu/yazıldı.
* Durum/Çıkış Durumu: Tepe noktası sona erdiğinde son durum.
* Çıkış Kodu/Hata Türü: Tepe noktası arızalandığında hata.
* Yaratılış Nedeni: Neden tepe noktası oluşturuldu.
* Kaynak Gecikmesüresi/İşlem Gecikmesi/PN Queue Gecikme süresi: vertex'in kaynakları beklemesi, verileri işlemesi ve kuyrukta kalması için geçen süre.
* Süreç/Oluşturucu GUID: Geçerli çalışan tepe noktası veya yaratıcısı için GUID.
* Sürüm: çalışan tepe noktasının N-th örneği (sistem birçok nedenden dolayı bir tepe noktasının yeni örneklerini zamanlayabilir, örneğin failover, hesaplama artıklığı, vb.)
* Sürüm Zaman oluşturuldu.
* İşlem Başlangıç Zamanı Oluşturma/İşlem Sıraya Sıralı Zaman/İşlem Başlangıç Zamanı/İşlem Tam Zamanı: vertex işlemi oluşturulmaya başladığında; vertex işlemi sıraya başladığında; belirli vertex işlemi başladığında; belirli tepe noktası tamamlandığında.

## <a name="next-steps"></a>Sonraki adımlar
* Tanılama bilgilerini günlüğe kaydetmek için bkz. [Azure Data Lake Analytics için tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md)
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* İş ayrıntılarını görüntülemek için Azure [Veri gölü Analizi işleri için İş Tarayıcısı ve İş Görünümü'nü kullan'a](data-lake-analytics-data-lake-tools-view-jobs.md) bakın
