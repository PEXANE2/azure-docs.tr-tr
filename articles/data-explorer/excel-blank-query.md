---
title: Microsoft Excel 'e aktarılan bir Azure Veri Gezgini kusto sorgusu kullanarak verileri görselleştirme
description: Bu makalede, bir Azure Veri Gezgini kusto sorgusunun Microsoft Excel 'e nasıl içeri aktarılacağını öğreneceksiniz.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: 4999000e2084922b43b8085034f545d4b5c644a9
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74849097"
---
# <a name="visualize-data-using-an-azure-data-explorer-kusto-query-imported-into-microsoft-excel"></a>Microsoft Excel 'e aktarılan bir Azure Veri Gezgini kusto sorgusu kullanarak verileri görselleştirme

Azure Veri Gezgini, Excel 'deki verilere bağlanmak için iki seçenek sunar: yerel bağlayıcıyı kullanın veya Azure Veri Gezgini bir sorgu içeri aktarın. Bu makalede, verileri görselleştirmek için Azure Veri Gezgini 'ten Excel 'e nasıl bir sorgu içeri aktarabileceğiniz gösterilmektedir. Veriler üzerinde ek hesaplamalar veya görselleştirmeler yapmak için Excel veri kaynağı olarak kusto sorgusu ekleyin.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* Azure Active Directory 'nin üyesi olan bir kurumsal e-posta hesabı, [azure Veri Gezgini yardım kümesine](https://dataexplorer.azure.com/clusters/help/databases/Samples) bağlanabilirsiniz 
<br>or</br>
* [Bir test kümesi ve veritabanı](create-cluster-database-portal.md) oluşturun ve [Azure Veri Gezgini Web UI uygulamasında](https://dataexplorer.azure.com/)oturum açın.

## <a name="define-kusto-query-as-an-excel-data-source"></a>Kusto sorgusunu Excel veri kaynağı olarak tanımlama

1. [Azure Veri Gezgini Web Kullanıcı arabiriminde](https://dataexplorer.azure.com/clusters/help/databases/Samples), sorguyu çalıştırın ve sonuçları denetleyin.

1. **Paylaşma** sekmesini seçin ve **Power BI sorgula**' yı seçin.

    ![Power BI Web Kullanıcı arabirimi sorgusu](media/excel-blank-query/web-ui-query-to-powerbi.png)

1. Aşağıdaki bildirimle bir pencere görünür:

    ![sorguyu Panoya Aktar](media/excel-blank-query/query-exported-to-clipboard.png)

1. **Microsoft Excel**'i açın.

1. **Veri** sekmesinde, **boş sorgu** > **diğer kaynaklardan** **veri > al** ' ı seçin.

    ![Veri al ve boş sorgu Seç](media/excel-blank-query/get-data-blank-query.png)

1. **Power Query Düzenleyicisi** penceresi açılır. Penceresinde **Gelişmiş Düzenleyici**' yi seçin.

    ![Power Query Düzenleyicisi penceresi](media/excel-blank-query/power-query-editor.png)

1. **Gelişmiş Düzenleyici** penceresinde, panoya verdiğiniz sorguyu yapıştırın ve **bitti**' yi seçin.

    ![Gelişmiş Düzenleyici sorgusu](media/excel-blank-query/advanced-editor-query.png)    

1. Kimlik doğrulaması için **kimlik bilgilerini düzenle**' yi seçin.

    ![Kimlik bilgilerini düzenle](media/excel-blank-query/edit-credentials.png)

1. **Kuruluş hesabı** ' nı seçin ve **oturum açın**. Oturum açma işlemini tamamlayıp **Bağlan**' ı seçin.

    ![Oturum açmayı tamamlayacak](media/excel-blank-query/complete-sign-in.png)

    Daha fazla sorgu eklemek için önceki adımları tekrarlayın. Sorguları daha anlamlı adlarla yeniden adlandırabilirsiniz.

1. Verilerinizi Excel 'e almak için **& yükle** ' yi Kapat düğmesini seçin.

    ![Kapat ve yükle seçeneğini belirleyin](media/excel-blank-query/close-and-load.png)

1. Artık Verileriniz Excel 'de. Sorguyu yenilemek için **Yenile** düğmesini seçin.

    ![Excel 'de verileri görüntüleme](media/excel-blank-query/data-in-excel.png)

## <a name="next-steps"></a>Sonraki adımlar

[Excel için Azure Veri Gezgini bağlayıcısını kullanarak verileri görselleştirme](excel-connector.md)