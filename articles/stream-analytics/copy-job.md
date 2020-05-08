---
title: Azure Stream Analytics işleri kopyalama veya yedekleme
description: Bu makalede bir Azure Stream Analytics işinin nasıl kopyalanacağı veya yedekleneceği açıklanır.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 258fc30f3e4a4ba374ada89c03b824ff5e27718e
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612274"
---
# <a name="copy-or-back-up-azure-stream-analytics-jobs"></a>Azure Stream Analytics işleri kopyalama veya yedekleme

Visual Studio Code veya Visual Studio kullanarak dağıtılan Azure Stream Analytics işlerinizi kopyalayabilir veya yedekleyebilirsiniz. Bir işi başka bir bölgeye kopyalamak, son çıkış süresini kopyalamaz. Bu nedenle, kopyalanmış işi başlatırken [**son durdurulduğu zaman**](https://docs.microsoft.com/azure/stream-analytics/start-job#start-options) seçeneğini kullanamazsınız.

## <a name="before-you-begin"></a>Başlamadan önce
* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)oluşturun.

* [Azure Portal](https://portal.azure.com/) oturum açın.

* [Visual Studio için Visual Studio Code veya Azure Stream Analytics araçları](quick-create-vs-code.md#install-the-azure-stream-analytics-tools-extension) [için Azure Stream Analytics uzantısını](quick-create-vs-code.md#install-the-azure-stream-analytics-tools-extension) yükler.  

## <a name="visual-studio-code"></a>Visual Studio Code

1. Visual Studio Code etkinlik çubuğundaki **Azure** simgesine tıklayın ve ardından **Stream Analytics** düğümünü genişletin. İşleriniz aboneliklerinizin altında görünmelidir.

   ![Stream Analytics Gezginini aç](./media/vscode-explore-jobs/open-explorer.png)

2. Bir işi yerel bir projeye aktarmak için, Visual Studio Code **Stream Analytics Gezgininde** dışarı aktarmak istediğiniz işi bulun. Ardından projeniz için bir klasör seçin.

    ![Visual Studio Code ASA işini dışarı aktarma](./media/vscode-explore-jobs/export-job.png)

    Proje seçtiğiniz klasöre aktarılmışsa ve geçerli çalışma alanınıza eklemiş olursunuz.

    ![Visual Studio Code ASA işini dışarı aktarma](./media/stream-analytics-manage-job/copy-backup-stream-analytics-jobs.png)

3. Farklı bir ad kullanarak işi başka bir bölgeye veya yedeklemeye yayımlamak için, sorgu Düzenleyicisi 'nde (\*. asaql) **yayımlamak üzere aboneliklerinizden Seç** ' i seçin ve yönergeleri izleyin.

    ![Visual Studio Code Azure 'da yayımlayın](./media/quick-create-vs-code/submit-job.png)

## <a name="visual-studio"></a>Visual Studio

1. [Dağıtılan Azure Stream Analytics işini bir proje yönergelerine dışarı aktarma](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-vs-tools#export-jobs-to-a-project)işlemini izleyin.

2. Sorgu Düzenleyicisi \*'nde. asaql dosyasını açın, betik düzenleyicisinde **Azure 'a gönder** ' i seçin ve yeni bir ad kullanarak işi başka bir bölgeye veya yedeklemeye yayımlamak için yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Visual Studio Code kullanarak Stream Analytics işi oluşturma](quick-create-vs-code.md)
* [Hızlı başlangıç: Visual Studio 'Yu kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Azure Pipelines kullanarak CI/CD ile Azure Stream Analytics işi dağıtma](stream-analytics-tools-visual-studio-cicd-vsts.md)
