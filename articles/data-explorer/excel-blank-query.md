---
title: Microsoft Excel'e aktarılan Azure Veri Gezgini Kusto sorgusukullanarak verileri görselleştirin
description: Bu makalede, Microsoft Excel'e Azure Veri Gezgini Kusto sorgusu alma mayı öğreneceksiniz.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: 4999000e2084922b43b8085034f545d4b5c644a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74849097"
---
# <a name="visualize-data-using-an-azure-data-explorer-kusto-query-imported-into-microsoft-excel"></a>Microsoft Excel'e aktarılan Azure Veri Gezgini Kusto sorgusukullanarak verileri görselleştirin

Azure Veri Gezgini, Excel'deki verilere bağlanmak için iki seçenek sunar: yerel bağlayıcıyı kullanın veya Azure Veri Gezgini'nden bir sorgu içe aktarma. Bu makalede, verileri görselleştirmek için Azure Veri Gezgini'nden Excel'e nasıl sorgu aktarılabildiğiniz gösterilmektedir. Veriler üzerinde ek hesaplamalar veya görselleştirmeler yapmak için Bir Excel veri kaynağı olarak Kusto sorgusu ekleyin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* Azure Etkin dizininin üyesi olan kurumsal bir e-posta hesabı, böylece [Azure Veri Gezgini yardım kümesine](https://dataexplorer.azure.com/clusters/help/databases/Samples) bağlanabilirsiniz 
<br>or</br>
* [Bir test kümesi ve veritabanı](create-cluster-database-portal.md) oluşturun ve Azure Veri [Gezgini Web UI uygulamasında](https://dataexplorer.azure.com/)oturum açın.

## <a name="define-kusto-query-as-an-excel-data-source"></a>Kusto sorgusunun Excel veri kaynağı olarak tanımlanması

1. [Azure Veri Gezgini Web UI'sinde](https://dataexplorer.azure.com/clusters/help/databases/Samples)sorguyu çalıştırın ve sonuçları denetleyin.

1. **Paylaş** sekmesini seçin ve **Power BI'ye Sorgu'yu**seçin.

    ![Power BI web ui sorgusu](media/excel-blank-query/web-ui-query-to-powerbi.png)

1. Aşağıdaki bildirimle birlikte bir pencere görüntülenir:

    ![sorguyı panoya aktarma](media/excel-blank-query/query-exported-to-clipboard.png)

1. **Microsoft Excel'i**açın.

1. **Veri** sekmesinde,**Diğer Kaynaklardan** >  **Veri** > Al**Boş Sorgu'yu**seçin.

    ![Veri alın ve boş sorgu seçin](media/excel-blank-query/get-data-blank-query.png)

1. **Güç Sorgusu Düzenleyicisi** penceresi açılır. Pencerede Gelişmiş **Düzenleyici'yi**seçin.

    ![Güç sorgusu düzenleyici penceresi](media/excel-blank-query/power-query-editor.png)

1. Gelişmiş **Düzenleyici** penceresinde, dışa aktarılan sorguyu panoya yapıştırın ve **Bitti'yi**seçin.

    ![Gelişmiş editör sorgusu](media/excel-blank-query/advanced-editor-query.png)    

1. Kimlik doğrulaması yapmak için **Kimlik Bilgilerini Edit'i**seçin.

    ![Kimlik bilgilerini düzenle](media/excel-blank-query/edit-credentials.png)

1. **Kuruluş hesabını** seçin ve **oturum açın.** Oturum açma işlemini tamamlayın ve sonra **Bağlan'ı**seçin.

    ![Tam oturum açma](media/excel-blank-query/complete-sign-in.png)

    Daha fazla sorgu eklemek için önceki adımları yineleyin. Sorguları daha anlamlı adlarla yeniden adlandırabilirsiniz.

1. Verilerinizi Excel'e sokmak için **& Yükle'yi kapat** düğmesini seçin.

    ![Kapat ve yükle'yi seçin](media/excel-blank-query/close-and-load.png)

1. Artık verileriniz Excel'de. Sorguyu yenilemek için **Yenile** düğmesini seçin.

    ![Excel'de verileri görüntüleme](media/excel-blank-query/data-in-excel.png)

## <a name="next-steps"></a>Sonraki adımlar

[Excel için Azure Veri Gezgini bağlayıcısını kullanarak verileri görselleştirin](excel-connector.md)