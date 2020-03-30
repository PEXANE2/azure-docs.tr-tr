---
title: Power BI için Azure Veri Gezgini bağlayıcısı ile verileri görselleştirin
description: "Bu makalede, Power BI'deki verileri görselleştirmek için üç seçenekten birini nasıl kullanacağınızı öğreneceksiniz: Azure Veri Gezgini için Power BI bağlayıcısı."
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: a95d45481bed17e46429e3a22dff4b8cc62354a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77560499"
---
# <a name="visualize-data-using-the-azure-data-explorer-connector-for-power-bi"></a>Power BI için Azure Veri Gezgini bağlayıcısını kullanarak verileri görselleştirin

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Power BI, verilerinizi görselleştirmenizi ve sonuçları kuruluşunuzda paylaşmanızı sağlayan bir iş analizi çözümüdür. Azure Veri Gezgini, Power BI'deki verilere bağlanmak için üç seçenek sunar: yerleşik bağlayıcıyı kullanın, Azure Veri Gezgini'nden sorgu alma veya SQL sorgusu kullanın. Bu makalede, veri almak ve bir Power BI raporunda görselleştirmek için yerleşik bağlayıcınasıl kullanılacağını gösterir. Power BI panoları oluşturmak için Azure Veri Gezgini yerel bağlayıcısını kullanmak kolaydır. Power BI bağlayıcısı [Alma ve Doğrudan Sorgu bağlantı modlarını](https://docs.microsoft.com/power-bi/desktop-directquery-about)destekler. Senaryoya, **ölçeklendirmeye** ve performans gereksinimlerine bağlı olarak Alma veya **DirectQuery** modunu kullanarak panolar oluşturabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için aşağıdakilere ihtiyacınız var:

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* Azure Etkin dizininin üyesi olan kurumsal bir e-posta hesabı, böylece [Azure Veri Gezgini yardım kümesine](https://dataexplorer.azure.com/clusters/help/databases/samples)bağlanabilirsiniz.
* [Power BI Masaüstü](https://powerbi.microsoft.com/get-started/) **(ÜCRETSIZ İndir'i**seçin )

## <a name="get-data-from-azure-data-explorer"></a>Azure Veri Gezgini'nden veri alma

Önce Azure Veri Gezgini yardım kümesine bağlanırsınız, ardından *StormEvents* tablosundan verilerin bir alt kümesini getirirsiniz. [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

1. Power BI Desktop'da, **Ana Sayfa** sekmesinde, Daha **Fazla** **Veri Al'ı** seçin.

    ![Verileri alma](media/power-bi-connector/get-data-more.png)

1. Azure *Veri Gezgini'ni*arayın, **Azure Veri Gezgini'ni** seçin ve **bağlanın.**

    ![Arama ve veri alma](media/power-bi-connector/search-get-data.png)

1. Azure **Veri Gezgini (Kusto)** ekranında, formu aşağıdaki bilgilerle doldurun.

    ![Küme, veritabanı, tablo seçenekleri](media/power-bi-connector/cluster-database-table.png)

    **Ayar** | **Değer** | **Alan açıklaması**
    |---|---|---|
    | Küme | *https://help.kusto.windows.net* | Yardım kümesinin URL'si. Diğer kümeler için URL *ClusterName\<\>https:// şeklindedir.\< Bölge\>.kusto.windows.net*. |
    | Database | Boş bırakın | Bağlandığınız kümede barındırılan bir veritabanı. Bunu daha sonraki bir adımda seçeceğiz. |
    | Tablo adı | Boş bırakın | Veritabanındaki tablolardan biri veya <code>StormEvents \| take 1000</code>. Bunu daha sonraki bir adımda seçeceğiz. |
    | Gelişmiş seçenekler | Boş bırakın | Sonuç kümesi boyutu gibi sorgularınız için seçenekler. |
    | Veri bağlantısı modu | *DirectQuery* | Power BI'nin verileri içeri aktarıp aktarmadığını veya doğrudan veri kaynağına bağlanıp bağlanmayacağını belirler. Bu bağlayıcıile her iki seçeneği de kullanabilirsiniz. |
    | | | |
    
    > [!NOTE]
    > **Alma** modunda, veriler Power BI'ye taşınır. **DirectQuery** modunda veriler doğrudan Azure Veri Gezgini kümenizden sorgulanır.
    >
    > Şu anda **Alma** modunu kullanın:
    > * Veri setiniz küçük.
    > * Yakın gerçek zamanlı verilere ihtiyacınız yoktur. 
    > * Verileriniz zaten toplanmış veya [Kusto'da toplama](/azure/kusto/query/summarizeoperator#list-of-aggregation-functions) gerçekleştirmişsiniz    
    >
    > **DirectQuery** modunu şu zaman kullanın:
    > * Veri setiniz çok büyük. 
    > * Neredeyse gerçek zamanlı verilere ihtiyacın var.   

1. Yardım kümesiyle zaten bağlantınız yoksa oturum açın. Kuruluş hesabıyla oturum açın, ardından **Bağlan'ı**seçin.

    ![Oturum aç](media/power-bi-connector/sign-in.png)

1. **Navigator** ekranında, **Örnekler** veritabanını genişletin, **StormEvents'i** seçin ve **ardından Edit.**

    ![Tablo seçin](media/power-bi-connector/select-table.png)

    Tablo, Power Query Düzenleyicisi'nde açılır ve burada verileri içeri aktarmadan önce satırları ve sütunları düzenleyebilirsiniz.

1. Güç Sorgusu Düzenleyicisi'nde **DamageCrops** sütununyanındaki oku seçin ve **azalan sırala'** yı seçin.

    ![Hasar Eksponer'in azalan sıralaması](media/power-bi-connector/sort-descending.png)

1. Ana **Sayfa** sekmesinde, **Satırları Tut'u** ve **Üst Satırları Tut'u**seçin. Sıralanmış tablonun en iyi 1000 satırını getirmek için *1000* değeri girin.

    ![Üst satırları tutma](media/power-bi-connector/keep-top-rows.png)

1. **Giriş** sekmesinde **Uygula&** kapat'ı'nı seçin.

    ![Kapat ve uygula](media/power-bi-connector/close-apply.png)

## <a name="visualize-data-in-a-report"></a>Rapordaki verileri görselleştirme

[!INCLUDE [data-explorer-power-bi-visualize-basic](../../includes/data-explorer-power-bi-visualize-basic.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makale için oluşturduğunuz rapora artık ihtiyacınız yoksa, Power BI Desktop (.pbix) dosyasını silin.

## <a name="next-steps"></a>Sonraki adımlar

[Verileri sorgulamak için Power BI için Azure Veri Gezgini bağlayıcısını kullanma ipuçları](power-bi-best-practices.md#tips-for-using-the-azure-data-explorer-connector-for-power-bi-to-query-data)
