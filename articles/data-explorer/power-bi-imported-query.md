---
title: Power BI içe aktarılan sorguyla Azure Veri Gezgini'nden gelen verileri görselleştirin
description: "Bu makalede, Power BI'de verileri görselleştirmek için üç seçenekten birini nasıl kullanacağınızı öğreneceksiniz: Azure Veri Gezgini'nden sorgu alma."
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: ff156ab3fe74115bce8f7d6bdd3ba47b514f5ff5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562488"
---
# <a name="visualize-data-using-a-query-imported-into-power-bi"></a>Power BI'ye aktarılan bir sorgukullanarak verileri görselleştirin

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Power BI, verilerinizi görselleştirmenizi ve sonuçları kuruluşunuzda paylaşmanızı sağlayan bir iş analizi çözümüdür.

Azure Veri Gezgini, Power BI'deki verilere bağlanmak için üç seçenek sunar: yerleşik bağlayıcıyı kullanın, Azure Veri Gezgini'nden sorgu alma veya SQL sorgusu kullanın. Bu makalede, verileri almak ve bir Power BI raporunda görselleştirmek için bir sorgu alma nasıl gösterir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için aşağıdakilere ihtiyacınız var:

* Azure Etkin dizininin üyesi olan kurumsal bir e-posta hesabı, böylece [Azure Veri Gezgini yardım kümesine](https://dataexplorer.azure.com/clusters/help/databases/samples)bağlanabilirsiniz.

* [Power BI Masaüstü](https://powerbi.microsoft.com/get-started/) **(ÜCRETSIZ İndir'i**seçin )

* [Azure Veri Gezgini masaüstü uygulaması](/azure/kusto/tools/kusto-explorer)

## <a name="get-data-from-azure-data-explorer"></a>Azure Veri Gezgini'nden veri alma

İlk olarak, Azure Veri Gezgini masaüstü uygulamasında bir sorgu oluşturun ve Power BI'de kullanılmak üzere dışa aktarın. Ardından, Azure Veri Gezgini yardım kümesine bağlanır ve *StormEvents* tablosundaki verilerin bir alt kümesini getirirsiniz. [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

1. Bir tarayıcıda Azure [https://help.kusto.windows.net/](https://help.kusto.windows.net/) Veri Gezgini masaüstü uygulamasını başlatmak için gidin.

1. Masaüstü uygulamasında, aşağıdaki sorguyu sağ üst sorgu penceresine kopyalayın ve ardından çalıştırın.

    ```Kusto
    StormEvents
    | sort by DamageCrops desc
    | take 1000
    ```

    Sonuç kümesinin ilk birkaç satırı aşağıdaki görüntüye benzer olmalıdır.

    ![Sorgu sonuçları](media/power-bi-imported-query/query-results.png)

1. **Araçlar** sekmesinde, **Güç BI'ye** Sorgula'yı ve **Tamam'ı**seçin.

    ![Dışa aktarma sorgusu](media/power-bi-imported-query/export-query.png)

1. Power BI Desktop'da, **Ana Sayfa** sekmesinde, Ver ve **Boş sorgu** **al'ı** seçin.

    ![Verileri alma](media/power-bi-imported-query/get-data.png)

1. Power Query Editor'da, **Ana sayfa** sekmesinde Gelişmiş **düzenleyiciyi**seçin.

1. Gelişmiş **düzenleyici** penceresinde, dışa aktardığınız sorguyu yapıştırın ve **ardından Bitti'yi**seçin.

    ![Sorguyapıştır](media/power-bi-imported-query/paste-query.png)

1. Ana Güç Sorgusu Düzenleyicisi penceresinde kimlik **bilgilerini edit'i**seçin. **Kuruluş hesabını**seçin, oturum açın, ardından **Bağlan'ı**seçin.

    ![Kimlik bilgilerini düzenle](media/power-bi-imported-query/edit-credentials.png)

1. **Giriş** sekmesinde **Uygula&** kapat'ı'nı seçin.

    ![Kapat ve uygula](media/power-bi-imported-query/close-apply.png)

## <a name="visualize-data-in-a-report"></a>Rapordaki verileri görselleştirme

[!INCLUDE [data-explorer-power-bi-visualize-basic](../../includes/data-explorer-power-bi-visualize-basic.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makale için oluşturduğunuz rapora artık ihtiyacınız yoksa, Power BI Desktop (.pbix) dosyasını silin.

## <a name="next-steps"></a>Sonraki adımlar

[Power BI için Azure Veri Gezgini bağlayıcısını kullanarak verileri görselleştirin](power-bi-connector.md)