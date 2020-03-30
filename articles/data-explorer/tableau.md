---
title: Tableau verilerini görselleştirmek için Azure Veri Gezgini ODBC bağlayıcısını kullanma
description: Bu makalede, Tableau ile verileri görselleştirmek için Azure Veri Gezgini bağlantısına açık veritabanı bağlantısı (ODBC) bağlantısını nasıl kullanacağınızı öğreneceksiniz.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 903daf450800a7f060899d736c2b31920c1b51f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562454"
---
# <a name="visualize-data-from-azure-data-explorer-in-tableau"></a>Tableau'da Azure Veri Gezgini'nden gelen verileri görselleştirin

 [Tableau,](https://www.tableau.com/) iş zekası için görsel bir analiz platformudur. Tableau'dan Azure Veri Gezgini'ne bağlanmak ve örnek bir kümeden veri getirmek için SQL Server Open Database Connectivity (ODBC) sürücüsünü kullanın. 

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için aşağıdakilere ihtiyacınız var:

* Sql Server ODBC sürücüsünü kullanarak [ODBC ile Azure Veri Gezgini'ne bağlanarak](connect-odbc.md) Tableau'dan Azure Veri Gezgini'ne bağlanın. 

* Tableau Masaüstü, tam veya [deneme](https://www.tableau.com/products/desktop/download) sürümü.

* StormEvents örnek verilerini içeren bir küme. Daha fazla bilgi için [bkz.](create-cluster-database-portal.md) [Ingest sample data into Azure Data Explorer](ingest-sample-data.md)

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="visualize-data-in-tableau"></a>Tableau'da verileri görselleştir 

ODBC yapılandırmayı tamamladıktan sonra, örnek verileri Tableau'ya getirebilirsiniz.

1. Tableau Desktop'da, sol menüde **Diğer Veritabanları 'nı (ODBC)** seçin.

    ![ODBC ile bağlanma](media/tableau/connect-odbc.png)

1. **DSN**için, ODBC için oluşturduğunuz veri kaynağını seçin ve ardından **Oturum Aç'ı**seçin.

    ![ODBC oturum açma](media/tableau/odbc-sign-in.png)

1. **Veritabanı**için, *TestDatabase*gibi örnek kümenizdeki veritabanını seçin. **Şema**için, *dbo'yu*seçin ve **Tablo**için *StormEvents* örnek tablosunu seçin.

    ![Veritabanı ve tablo seçin](media/tableau/select-database-table.png)

1. Tableau şimdi örnek verilerin şema gösterir. Verileri Tableau'ya getirmek için **Şimdi Güncelleştir'i** seçin.

    ![Verileri güncelleştirme](media/tableau/update-data.png)

    Veriler içe aktarıldığında, Tableau aşağıdaki resme benzer veri satırları gösterir.

    ![Sonuç kümesi](media/tableau/result-set.png)

1. Artık Azure Veri Gezgini'nden getirdiğiniz verilere dayanarak Tableau'da görselleştirmeler oluşturabilirsiniz. Daha fazla bilgi için [Tableau Learning'e](https://www.tableau.com/learn)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini için sorgu yazma](write-queries.md)