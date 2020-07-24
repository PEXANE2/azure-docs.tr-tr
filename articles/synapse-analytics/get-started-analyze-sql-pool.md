---
title: 'Öğretici: SQL havuzuyla verileri çözümlemeye başlama'
description: Bu öğreticide, SQL havuzunun analitik yeteneklerini araştırmak için NYC TAXI örnek verilerini kullanacaksınız.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 948de6968d336d16554fe0a0c2f7d0c5b7367f15
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87101839"
---
# <a name="analyze-data-with-sql-pools"></a>SQL havuzlarıyla verileri çözümleme

Azure SYNAPSE Analytics, SQL havuzu ile verileri çözümleme yeteneği sağlar. Bu öğreticide, SQL havuzunun analitik yeteneklerini araştırmak için NYC TAXI örnek verilerini kullanacaksınız.

## <a name="load-the-nyc-taxi-sample-data-into-the-sqldb1-database"></a>NYC TAXI örnek verilerini SQLDB1 veritabanına yükleme

1. SYNAPSE Studio 'da, en üstteki mavi menüsünde soru işareti (**?**) simgesini seçin.
1. Başlarken **Getting started**  >  **hub 'ını**seçin.
1. **Sorgu örnek verisi**etiketli kartta, **SQLDB1**adlı SQL havuzunu seçin.
1. **Sorgu verileri**' ni seçin. "Örnek verileri yükleme" bildirimi kısaca görüntülenir. SYNAPSE Studio 'nun en üstünde yer alan açık mavi bir durum çubuğu, verilerin SQLDB1 'e yüklendiğini gösterir.
1. Durum çubuğu yeşile dönüşdikten sonra kapatın.

## <a name="explore-the-nyc-taxi-data-in-the-sql-pool"></a>SQL havuzundaki NYC TAXI verilerini keşfet

1. SYNAPSE Studio 'da **veri** merkezine gidin.
1. **SQLDB1**  >  **Tables**bölümüne gidin. Birkaç tablo yüklendiğini görürsünüz.
1. Dbo öğesine sağ tıklayın **. Seyahat** tablosu ve **Yeni SQL betiği**Seç  >  **ilk 100 satır seçin**.
1. Yeni bir SQL betiği oluşturulup çalışırken bekleyin.
1. SQL **komut dosyasının en üstünde,** **SQLDB1**adlı SQL havuzuna otomatik olarak ayarlandığını unutmayın.
1. SQL komut dosyasının metnini bu kodla değiştirin ve çalıştırın.

    ```sql
    SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
    FROM  dbo.Trip
    WHERE TripDistanceMiles > 0 AND PassengerCount > 0
    GROUP BY PassengerCount
    ORDER BY PassengerCount
    ```

    Bu sorgu, toplam seyahat mesafeleri ve ortalama seyahat mesafesinin, pascların sayısıyla ilişkisini gösterir.
1. SQL komut dosyası sonucu penceresinde, sonuçların bir çizgi grafik olarak görselliğini görmek için **görünümü** **grafik** olarak değiştirin.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Spark kullanarak çözümleme](get-started-analyze-spark.md)

