---
title: Azure Veri Gezgini'ni Redash ile görselleştirin
description: Bu makalede, Redash yerel bağlayıcısıyla Azure Veri Gezgini'ndeki verileri nasıl görselleştirdiğinizi öğreneceksiniz.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 0380689ae6ca81e3f31a07f1e205c7773fdea8c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76773951"
---
# <a name="visualize-data-from-azure-data-explorer-in-redash"></a>Redash'te Azure Veri Gezgini'nden verileri görselleştirin

[Redash,](https://redash.io/) veri kaynaklarınızı bağlar ve sorgular, verileri görselleştirmek ve eşlerle paylaşmak için panolar oluşturur. Bu makalede, Azure Veri Gezgini'ni Redash için bir veri kaynağı olarak nasıl ayarlayabileceğinizi ve ardından verileri görselleştirdiğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

1. [Küme ve veritabanı oluşturun.](create-cluster-database-portal.md)
1. Örnek verileri Azure [Veri Gezgini'ne sindirirken](ingest-sample-data.md)açıklandığı gibi verileri alın. Daha fazla yutma seçeneği için, [yutma genel bakış](ingest-data-overview.md)bakın.

[!INCLUDE [data-explorer-configure-data-source](../../includes/data-explorer-configure-data-source.md)]

## <a name="create-azure-data-explorer-connector-in-redash"></a>Redash'te Azure Veri Gezgini Bağlayıcısı Oluşturma 

1. [Redash'e](https://www.redash.io/)kaydol. Hesap oluşturmak için **Başlat'ı** seçin.
1. **Başlayalım**altında , Bir **Veri Kaynağı Bağla'yı**seçin.

    ![Bir veri kaynağını bağlayın](media/redash/connect-data-source.png)

1. **Yeni Veri Kaynağı Oluştur** penceresinde Azure Veri **Gezgini 'ni (Kusto)** seçin ve ardından **Oluştur'u**seçin. 

    ![Azure Veri Gezgini veri kaynağını seçin](media/redash/select-adx-data-source.png)

1. **Azure Veri Gezgini (Kusto)** penceresinde, aşağıdaki formu doldurun ve **Oluştur'u**seçin.

    ![Azure Veri Gezgini (Kusto) ayarları penceresi](media/redash/adx-settings-window.png)

1. **Ayarlar** penceresinde, **Azure Veri Gezgini (Kusto)** veri kaynağı bağlantınızı test etmek için Bağlantıyı **Kaydet** ve **Test Et'i** seçin.

## <a name="create-queries-in-redash"></a>Redash'te sorgu oluşturma

1. Redash'in sol üst kısmında**Sorgu** **Oluştur'u** > seçin. Yeni **Sorgu'yu** tıklatın ve sorguyu yeniden adlandırın.

    ![Sorgu oluşturma](media/redash/create-query.png)

1. Sorgunuzu üst düzenleme bölmesine yazın ve **Kaydet** ve Yürüt'ün'u seçin. **Execute** Sorgunun ileride yayımlanmasını sağlamak için **Yayımla'yı** seçin.

    ![Sorguyı kaydetme ve yürütme](media/redash/save-and-execute-query.png)

    Sol bölmede, açılan menüde veri kaynağı bağlantı adını (akışımızdaki**Github bağlayıcısı)** ve seçili veritabanındaki tabloları görebilirsiniz. 

1. Sorgu sonuçlarını alt merkezi bölmede görüntüleyin. **Yeni Görselleştirme** düğmesini seçerek sorguyu yapmak için bir görselleştirme oluşturun.

    ![Yeni görselleştirme](media/redash/new-visualization.png)

1. Görselleştirme ekranında, **Görselleştirme Türünü** ve **X Sütunu** ve Y **Sütunu**gibi ilgili alanları seçin. Görselleştirmeyi **kaydedin.**

    ![Görselleştirmeyi yapılandırma ve kaydetme](media/redash/configure-visualization.png)

### <a name="create-a-query-using-a-parameter"></a>Parametre kullanarak sorgu oluşturma

1. **Create** > Yeni bir sorgu oluşturmak için**Sorgu'u** oluşturun. { }{}kıvırcık parantezkullanarak bir parametre ekleyin. **Parametre Ekle** penceresini açmak için **{}{ }'u** seçin. Ayrıca, varolan bir parametrenin özniteliklerini değiştirmek ve **<parameter_name>** penceresini açmak için ayarlar *simgesini* de seçebilirsiniz. 

    ![parametre ekle](media/redash/insert-parameter.png)

1. Parametrenizi adlandırın. **Tür :** Açılan menüden Sorgu Tabanlı **Açılır Liste'yi** seçin. **Tamam'ı** seçin

    ![sorgu tabanlı açılır liste](media/redash/query-based-dropdown-list.png)

    > [!NOTE]
    > Sorgu birden çok değer kullanır, bu nedenle `| where Type in ((split('{{Type}}', ',')))`aşağıdaki sözdizimini eklemeniz gerekir. Daha fazla bilgi için [operatöre](/azure/kusto/query/inoperator)bakın. Bu, [redash uygulamasında birden çok sorgu parametresi seçeneğiyle](https://redash.io/help/user-guide/querying/query-parameters#Serialized-Multi-Select-Query-Parametersredash.io) sonuçlanır

## <a name="create-a-dashboard-in-redash"></a>Redash'te pano oluşturma

1. Panonuzu oluşturmak için**Pano** **Oluşturun.** >  Alternatif olarak, varolan panoyu, **Panolar** > listeden bir pano seçin.

    ![Pano oluşturma](media/redash/create-dashboard.png)

1. **Yeni Pano** penceresinde, panonuzu adlandırın ve **Kaydet'i**seçin. >penceresinde **Dashboard_name<,** yeni bir widget oluşturmak için **Widget Ekle'yi** seçin. 

1. **Widget Ekle** penceresinde sorgu adını, **Görselleştirmeyi ve** **Parametreleri**seçin. **Panoya Ekle'yi** seçin

   ![Görselleştirmeleri seçin ve panoya ekleyin](media/redash/add-widget-window.png)

1. Pano oluşturmayı tamamlamak için **Bitti Düzenlemesi'ni** seçin.

1.  Pano edit modunda, önceden tanımlanan **Tür** parametresini kullanmak için **Pano Düzeyi Filtrelerini Kullan'ı** seçin.

    ![Pano oluşturmayı tamamlayın](media/redash/complete-dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini için sorgu yazma](write-queries.md)


