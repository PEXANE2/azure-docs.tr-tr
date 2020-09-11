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
ms.openlocfilehash: 363f2934bbeec266c16711572620e03e69785f94
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "90007205"
---
# <a name="analyze-data-with-sql-pools"></a>SQL havuzlarıyla verileri çözümleme

Azure SYNAPSE Analytics, SQL havuzu ile verileri çözümleme yeteneği sağlar. Bu öğreticide, SQL havuzunun analitik yeteneklerini araştırmak için NYC TAXI örnek verilerini kullanacaksınız.

## <a name="link-the-nyc-taxi-sample-data-into-the-sqldb1-database"></a>NYC TAXI örnek verilerini SQLDB1 veritabanına bağlama

1. SYNAPSE Studio 'da sol taraftaki **veri** hub 'ına gidin.
1. ' **+** A ve ardından **örneklere gözatamazsınız**' ı seçin. Bu işlem, **örnek merkezini** açar ve **veri kümeleri** sekmesini açar.
1. **NYC taxı & Limousine Commission-sarı TAXI seyahat kayıtları**' nı seçin. Bu veri kümesi 1.500.000.000 satırdan fazla satır içerir.
1. **Veri kümesi Ekle** 'ye tıklayın
1. **Bağlı** **veri** merkezinde, **Azure Blob depolama > örnek veri kümeleri >** yeni bir veri kümesi görürsünüz nyc_tlc_yellow   
1. **Sorgu örnek verisi**etiketli kartta, **SQLDB1**adlı SQL havuzunu seçin.


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

