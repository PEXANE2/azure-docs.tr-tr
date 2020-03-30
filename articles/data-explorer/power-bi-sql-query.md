---
title: Power BI SQL sorgusuyla Azure Veri Gezgini'nden gelen verileri görselleştirin
description: "Bu makalede, Power BI'de verileri görselleştirmek için üç seçenekten birini nasıl kullanacağınızı öğreneceksiniz: Azure Veri Gezgini kümesine karşı bir SQL sorgusu."
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: d402d4c1ee77d0f97d2a5c3bdf43d0cc62aac096
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77560482"
---
# <a name="visualize-data-from-azure-data-explorer-using-a-sql-query-in-power-bi"></a>Power BI'de BIR SQL sorgusu kullanarak Azure Veri Gezgini'nden gelen verileri görselleştirin

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Power BI, verilerinizi görselleştirmenizi ve sonuçları kuruluşunuzda paylaşmanızı sağlayan bir iş analizi çözümüdür.

Azure Veri Gezgini, Power BI'deki verilere bağlanmak için üç seçenek sunar: yerleşik bağlayıcıyı kullanın, Azure Veri Gezgini'nden sorgu alma veya SQL sorgusu kullanın. Bu makalede, veri almak ve bir Power BI raporunda görselleştirmek için bir SQL sorgusu nasıl kullanılacağını gösterir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için aşağıdakilere ihtiyacınız var:

* Azure Etkin dizininin üyesi olan kurumsal bir e-posta hesabı, böylece [Azure Veri Gezgini yardım kümesine](https://dataexplorer.azure.com/clusters/help/databases/samples)bağlanabilirsiniz.

* [Power BI Masaüstü](https://powerbi.microsoft.com/get-started/) **(ÜCRETSIZ İndir'i**seçin )

## <a name="get-data-from-azure-data-explorer"></a>Azure Veri Gezgini'nden veri alma

Önce Azure Veri Gezgini yardım kümesine bağlanırsınız, ardından *StormEvents* tablosundan verilerin bir alt kümesini getirirsiniz. [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

Genellikle Azure Veri Gezgini ile ana sorgu dilini kullanırsınız, ancak burada kullanacağınız SQL sorgularını da destekler. Azure Veri Gezgini, SQL sorgusunu sizin için yerel bir sorguya çevirir.

1. Power BI Desktop'da, **Ana Sayfa** sekmesinde, Daha **Fazla** **Veri Al'ı** seçin.

    ![Verileri alma](media/power-bi-sql-query/get-data-more.png)

1. Azure *SQL Veritabanı'nı*arayın, **Azure SQL Veritabanı'nı** seçin ve **ardından Bağlanın.**

    ![Arama ve veri alma](media/power-bi-sql-query/search-get-data.png)

1. SQL **Server veritabanı** ekranında formu aşağıdaki bilgilerle doldurun.

    ![Veritabanı, tablo, sorgu seçenekleri](media/power-bi-sql-query/database-table-query.png)

    **Ayar** | **Değer** | **Alan açıklaması**
    |---|---|---|
    | Sunucu | *help.kusto.windows.net* | Yardım kümesinin URL'si *(https://* olmadan). Diğer kümeler için URL * \<ClusterName\>biçimindedir.\< Bölge\>.kusto.windows.net*. |
    | Database | *Örnekler* | Bağlandığınız kümede barındırılan örnek veritabanı. |
    | Veri bağlantısı modu | *İçeri Aktarma* | Power BI'nin verileri içeri aktarıp aktarmadığını veya doğrudan veri kaynağına bağlanıp bağlanmayacağını belirler. Bu bağlayıcıile her iki seçeneği de kullanabilirsiniz. |
    | Komut zaman | Boş bırakın | Zaman aşımı hatası atmadan önce sorgunun ne kadar süre çalıştığı. |
    | SQL deyimi | Sorguyu bu tablonun altında kopyalama | Azure Veri Gezgini'nin yerel bir sorguya çevirdiği SQL deyimi. |
    | Diğer seçenekler | Varsayılan değerler olarak bırakma | Seçenekler Azure Veri Gezgini kümeleri için geçerli değildir. |
    | | | |

    ```SQL
    SELECT TOP 1000 *
    FROM StormEvents
    ORDER BY DamageCrops DESC
    ```

1. Yardım kümesiyle zaten bağlantınız yoksa oturum açın. Microsoft hesabıyla oturum açın ve **ardından Bağlan'ı**seçin.

    ![Oturum aç](media/power-bi-sql-query/sign-in.png)

1. **help.kusto.windows.net: Örnekler** ekranında **Yükle'yi**seçin.

    ![Veri yükleme](media/power-bi-sql-query/load-data.png)

    Tablo, örnek verilere dayalı raporlar oluşturabileceğiniz rapor görünümünde ana Power BI penceresinde açılır.

## <a name="visualize-data-in-a-report"></a>Rapordaki verileri görselleştirme

[!INCLUDE [data-explorer-power-bi-visualize-basic](../../includes/data-explorer-power-bi-visualize-basic.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makale için oluşturduğunuz rapora artık ihtiyacınız yoksa, Power BI Desktop (.pbix) dosyasını silin.

## <a name="next-steps"></a>Sonraki adımlar

[Power BI için Azure Veri Gezgini bağlayıcısını kullanarak verileri görselleştirin](power-bi-connector.md)