---
title: Azure Veri Gölü Analizini İzle - Azure portalı
description: Bu makalede, Azure Veri Gölü Analizi işlerini gidermek için Azure portalının nasıl kullanılacağı açıklanmaktadır.
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 7fb85f179f1e1e1ac873fc7d6d937b34c1d48ada
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71316599"
---
# <a name="monitor-jobs-in-azure-data-lake-analytics-using-the-azure-portal"></a>Azure Portalını kullanarak Azure Data Lake Analytics işlerini izleme

**Tüm işleri görmek için**

1. Azure portalından sol üst köşede **Microsoft Azure'u** tıklatın.
2. Data Lake Analytics hesap adınızı içeren kutucuğa tıklayın.  İş özeti **İş Yönetimi** döşemesi üzerinde gösterilir.

    ![Azure Veri Gölü Analizi iş yönetimi](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    İş Yönetimi size iş durumu hakkında bir bakış sunar. Başarısız bir iş olduğuna dikkat edin.
3. İşleri görmek için **İş Yönetimi** döşemesini tıklatın. İşler **Çalışma,** **Sıraya ve** **Sona Eren'de**sınıflandırılır. Başarısız işinizi **Sona Erdi** bölümünde göreceksiniz. Bu listede ilk olacak. Çok fazla işin olduğunda, işleri bulmanıza yardımcı olmak için **Filtre'yi** tıklatabilirsiniz.

    ![Azure Veri Gölü Analizi filtre işleri](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. İş ayrıntılarını açmak için listeden başarısız olan işi tıklatın:

    ![Azure Veri Gölü Analizi başarısız iş](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Yeniden Gönder düğmesine dikkat **edin.** Sorunu çözdükten sonra, işi yeniden gönderebilirsiniz.
5. Hata ayrıntılarını açmak için önceki ekran görüntüsünden vurgulanan bölümü tıklatın.  Şöyle bir şey göreceksiniz:

    ![Azure Veri Gölü Analizi iş ayrıntılarını başarısız lığa uğrattı](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Kaynak klasörün bulunmadığını söyler.
6. **Yinelenen Komut Dosyası'nı**tıklatın.
7. **FROM** yolunu şu şekilde güncelleyin:

    "/Örnekler/Veriler/SearchLog.tsv"
8. **İşi Gönder**'e tıklayın.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Veri Gölü Analizine genel bakış](data-lake-analytics-overview.md)
* [Azure PowerShell'i kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [Azure portalLarını kullanarak Azure Veri Gölü Analizini yönetme](data-lake-analytics-manage-use-portal.md)
