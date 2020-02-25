---
title: Tableau verilerini görselleştirmek için Azure Veri Gezgini ODBC bağlayıcısını kullanma
description: Bu makalede, Tableau ile verileri görselleştirmek için Azure Veri Gezgini bağlantısına yönelik açık bir veritabanı bağlantısı (ODBC) bağlantısı kullanmayı öğreneceksiniz.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 903daf450800a7f060899d736c2b31920c1b51f6
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562454"
---
# <a name="visualize-data-from-azure-data-explorer-in-tableau"></a>Tableau 'de Azure Veri Gezgini verileri görselleştirme

 [Tableau](https://www.tableau.com/) , iş zekası için bir görsel analiz platformudur. Tableau adresinden Azure Veri Gezgini bağlanmak ve bir örnek kümeden veri getirmek için, açık veritabanı bağlantısı (ODBC) sürücüsünü SQL Server kullanın. 

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlayabilmeniz için aşağıdakiler gerekir:

* Tableau adresinden Azure Veri Gezgini 'e bağlanmak için SQL Server ODBC sürücüsünü kullanarak [azure Veri Gezgini 'A bağlanın](connect-odbc.md) . 

* Tableau Desktop, Full veya [deneme](https://www.tableau.com/products/desktop/download) sürümü.

* StormEvents örnek verilerini içeren bir küme. Daha fazla bilgi için bkz. Azure [Veri Gezgini kümesi ve veritabanı oluşturma](create-cluster-database-portal.md) ve [örnek verileri Azure Veri Gezgini](ingest-sample-data.md)ile alma.

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="visualize-data-in-tableau"></a>Tableau 'de verileri görselleştirme 

ODBC yapılandırmasını tamamladıktan sonra, örnek verileri Tableau 'ye taşıyabilirsiniz.

1. Tableau Desktop ' ta, sol menüde **diğer veritabanları ' nı (ODBC)** seçin.

    ![ODBC ile bağlanma](media/tableau/connect-odbc.png)

1. **DSN**IÇIN, ODBC için oluşturduğunuz veri kaynağını seçin ve **oturum aç**' ı seçin.

    ![ODBC oturum açma](media/tableau/odbc-sign-in.png)

1. **Veritabanı**için, örnek kümenizdeki *TestDatabase*gibi veritabanını seçin. **Şema**için *dbo*ve **tablo**için *stormevents* örnek tablosunu seçin.

    ![Veritabanı ve tablo Seç](media/tableau/select-database-table.png)

1. Tableau şimdi örnek verilerin şemasını gösterir. Verileri Tableau 'e getirmek için **Şimdi Güncelleştir** ' i seçin.

    ![Verileri güncelleştirme](media/tableau/update-data.png)

    Veriler içeri aktarıldığında, Tableau aşağıdaki görüntüye benzer veri satırlarını gösterir.

    ![Sonuç kümesi](media/tableau/result-set.png)

1. Artık Tableau 'de Azure Veri Gezgini aldığınız verileri temel alarak görselleştirmeler oluşturabilirsiniz. Daha fazla bilgi için bkz. [Tableau Learning](https://www.tableau.com/learn).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini için sorgu yazma](write-queries.md)