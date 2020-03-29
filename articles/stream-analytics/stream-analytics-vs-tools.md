---
title: Visual Studio'da Azure Akış Analizi işlerini görüntüleyin
description: Visual Studio'yı kullanarak Azure Akışı Analizi işlerinizi görüntülemeyi, başlatmayı ve durdurmayı, bağlantıları nasıl test edin, sonuçları kontrol edin ve dışa aktarın öğrenin.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/10/2018
ms.openlocfilehash: ad35ed342dfd40b98d61919749479ec5612cdd4f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75369635"
---
# <a name="use-visual-studio-to-view-azure-stream-analytics-jobs"></a>Azure Akış Analizi işlerini görüntülemek için Visual Studio'yı kullanın

Visual Studio için Azure Akış Analizi araçları, geliştiricilerin Akış Analizi işlerini doğrudan IDE'den yönetmelerini kolaylaştırır. Azure Akış Analizi araçları yla şunları yapabilirsiniz:
- [Yeni işler oluşturma](stream-analytics-quick-create-vs.md)
- İşleri başlatma, durdurma ve [izleme](stream-analytics-monitor-jobs-use-vs.md)
- İş sonuçlarını denetleme
- Varolan işleri bir projeye aktarma
- Giriş ve çıkış bağlantılarını sınama
- [Sorguları yerel olarak çalıştırma](stream-analytics-vs-tools-local-run.md)

Visual Studio için Azure Akışı Analizi araçlarını nasıl [yükleydiğinizi](stream-analytics-tools-for-visual-studio-install.md)öğrenin.

## <a name="explore-the-job-view"></a>İş görünümünü keşfedin

İş görünümünü Visual Studio'daki Azure Akış Analizi işleriyle etkileşimkurmak için kullanabilirsiniz.

### <a name="open-the-job-view"></a>İş görünümünü açma

1. **Sunucu Gezgini'nde,** Akış **Analizi işlerini** seçin ve ardından **Yenile'yi**seçin. İşiniz Stream **Analytics işlerinin**altında görünmelidir.

    ![Stream Analytics sunucu gezgini listesi](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-list-jobs-01.png)

2. İş düğümünüzü genişletin ve iş görünümünü açmak için **İş Görünümü** düğümüne çift tıklayın.
    
   ![Genişletilmiş iş düğümü](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-job-view-01.png)

### <a name="start-and-stop-jobs"></a>İşleri başlatma ve durdurma

Azure Stream Analytics işleri Visual Studio'daki iş görünümünden tam olarak yönetilebilir. Bir işi başlatmak, durdurmak veya silmek için denetimleri kullanın.
    
   ![Stream Analytics iş kontrolleri](./media/stream-analytics-vs-tools/azure-stream-analytics-job-view-controls.png)

## <a name="check-job-results"></a>İş sonuçlarını denetleme

Visual Studio için Akış Analizi araçları şu anda Azure Veri Gölü Depolama ve blob depolama için çıktı önizlemesini destekler. Sonucu görüntülemek **için, İş Görünümü'nde** iş diyagramının çıktı düğümini çift tıklatın ve uygun kimlik bilgilerini girin.

   ![Stream Analytics iş blob çıktısı](./media/stream-analytics-vs-tools/stream-analytics-blob-preview.png)

## <a name="export-jobs-to-a-project"></a>İşleri projeye aktarma

Varolan bir işi bir projeye dışa aktarmanın iki yolu vardır.

1. **Sunucu Gezgini'nde**, Akış Analizi İşler düğümü altında, iş düğümüne sağ tıklayın. **Yeni Akış Analizi Projesine Dışa Aktar'ı**seçin.
    
   ![Projeye iş verme](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-01.png)
    
    Oluşturulan proje Çözüm **Gezgini'nde**görünür.
    
   ![Çözüm gezgini](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-02.png)

2. İş görünümünde, **Project Oluştur'u**seçin.
    
   ![İş görünümünden proje oluşturma](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="test-connections"></a>Test bağlantıları

Giriş ve çıktı bağlantıları, **Test Bağlantısı** açılır düşüşünden bir seçenek seçilerek **İş Görünümü'nden** sınanabilir.

   ![Test Bağlantısı açılır](./media/stream-analytics-vs-tools/stream-analytics-test-connection-dropdown.png)

**Test Bağlantısı** sonuçları **Çıktı** penceresinde görüntülenir.

   ![Test Bağlantısı sonuçları](./media/stream-analytics-vs-tools/stream-analytics-test-connection-results.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio'u kullanarak Azure Akış Analizi işlerini izleyin ve yönetin](stream-analytics-monitor-jobs-use-vs.md)
* [Quickstart: Visual Studio'u kullanarak Bir Akış Analizi işi oluşturun](stream-analytics-quick-create-vs.md)
* [Öğretici: Azure Pipelines kullanarak CI/CD ile Azure Stream Analytics işi dağıtma](stream-analytics-tools-visual-studio-cicd-vsts.md)
* [Stream Analytics araçlarıyla sürekli tümleştirme ve geliştirme](stream-analytics-tools-for-visual-studio-cicd.md)
