---
title: Power BI içeri aktarılan bir sorguyla Azure Veri Gezgini verileri görselleştirme
description: 'Bu makalede, Power BI verileri görselleştirmeye yönelik üç seçenekten birini kullanmayı öğrenirsiniz: Azure Veri Gezgini bir sorgu içeri aktarılıyor.'
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: ff156ab3fe74115bce8f7d6bdd3ba47b514f5ff5
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562488"
---
# <a name="visualize-data-using-a-query-imported-into-power-bi"></a>Power BI içeri aktarılan bir sorgu kullanarak verileri görselleştirin

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Power BI, verilerinizi görselleştirmenizi ve sonuçları kuruluşunuzda paylaşmanızı sağlayan bir iş analizi çözümüdür.

Azure Veri Gezgini, Power BI verilere bağlanmak için üç seçenek sunar: yerleşik bağlayıcıyı kullanın, Azure Veri Gezgini bir sorgu içeri aktarın veya bir SQL sorgusu kullanın. Bu makalede, verileri alabilmeniz ve bir Power BI raporunda görselleştirmek için bir sorguyu içeri aktarma işlemi gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlayabilmeniz için aşağıdakiler gerekir:

* Azure Active Directory 'nin üyesi olan bir kurumsal e-posta hesabı, [azure Veri Gezgini yardım kümesine](https://dataexplorer.azure.com/clusters/help/databases/samples)bağlanabilirsiniz.

* [Power BI Desktop](https://powerbi.microsoft.com/get-started/) ( **indirmeyi ücretsiz**Seç)

* [Azure Veri Gezgini masaüstü uygulaması](/azure/kusto/tools/kusto-explorer)

## <a name="get-data-from-azure-data-explorer"></a>Azure Veri Gezgini veri al

İlk olarak, Azure Veri Gezgini masaüstü uygulamasında bir sorgu oluşturur ve Power BI kullanım için dışa aktarabilirsiniz. Ardından, Azure Veri Gezgini yardım kümesine bağlanırsınız ve *Stormevents* tablosundan verilerin bir alt kümesini getirebilirsiniz. [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

1. Bir tarayıcıda, Azure Veri Gezgini masaüstü uygulamasını başlatmak için [https://help.kusto.windows.net/](https://help.kusto.windows.net/) ' a gidin.

1. Masaüstü uygulamasında, aşağıdaki sorguyu sağ üst sorgu penceresine kopyalayın ve çalıştırın.

    ```Kusto
    StormEvents
    | sort by DamageCrops desc
    | take 1000
    ```

    Sonuç kümesinin ilk birkaç satırı aşağıdaki görüntüye benzer görünmelidir.

    ![Sorgu sonuçları](media/power-bi-imported-query/query-results.png)

1. **Araçlar** sekmesinde Sorgula ' yı seçerek **Power BI** **Tamam**' ı seçin.

    ![Sorguyu dışarı aktar](media/power-bi-imported-query/export-query.png)

1. Power BI Desktop, **giriş** sekmesinde **veri al** sonra **boş sorgu**' yı seçin.

    ![Verileri alma](media/power-bi-imported-query/get-data.png)

1. Power Query düzenleyicisinde, **giriş** sekmesinde **Gelişmiş Düzenleyici**' yi seçin.

1. **Gelişmiş Düzenleyici** penceresinde, verdiğiniz sorguyu yapıştırın ve **bitti**' yi seçin.

    ![Sorguyu Yapıştır](media/power-bi-imported-query/paste-query.png)

1. Ana Power Query Düzenleyicisi penceresinde **kimlik bilgilerini düzenle**' yi seçin. **Kuruluş hesabı**' nı seçin, oturum açın ve sonra **Bağlan**' ı seçin.

    ![Kimlik bilgilerini düzenle](media/power-bi-imported-query/edit-credentials.png)

1. **Giriş** sekmesinde **Kapat & Uygula**' yı seçin.

    ![Kapat ve uygula](media/power-bi-imported-query/close-apply.png)

## <a name="visualize-data-in-a-report"></a>Rapordaki verileri görselleştirme

[!INCLUDE [data-explorer-power-bi-visualize-basic](../../includes/data-explorer-power-bi-visualize-basic.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makale için oluşturduğunuz rapora artık ihtiyacınız yoksa Power BI Desktop (. pbix) dosyasını silin.

## <a name="next-steps"></a>Sonraki adımlar

[Power BI için Azure Veri Gezgini bağlayıcısını kullanarak verileri görselleştirin](power-bi-connector.md)