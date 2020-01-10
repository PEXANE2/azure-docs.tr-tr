---
title: Azure Stream Analytics işleri kopyalama veya yedekleme
description: Bu makalede bir Azure Stream Analytics işinin nasıl kopyalanacağı veya yedekleneceği açıklanır.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 5c8f770855dd8d19a9d313f1b79f9bf8da4b2393
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75771504"
---
# <a name="copy-or-back-up-azure-stream-analytics-jobs"></a>Azure Stream Analytics işleri kopyalama veya yedekleme

Visual Studio Code veya Visual Studio kullanarak dağıtılan Azure Stream Analytics işlerinizi kopyalayabilir veya yedekleyebilirsiniz. 

## <a name="before-you-begin"></a>Başlamadan önce
* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

* [Azure Portal](https://portal.azure.com/)’ında oturum açın.

* [Visual Studio için Visual Studio Code veya Azure Stream Analytics araçları](quick-create-vs-code.md#install-the-azure-stream-analytics-tools-extension) [için Azure Stream Analytics uzantısını](quick-create-vs-code.md#install-the-azure-stream-analytics-tools-extension) yükler.  

## <a name="visual-studio-code"></a>Visual Studio Code

1. Visual Studio Code etkinlik çubuğundaki **Azure** simgesine tıklayın ve ardından **Stream Analytics** düğümünü genişletin. İşleriniz aboneliklerinizin altında görünmelidir.

   ![Stream Analytics Gezginini aç](./media/vscode-explore-jobs/open-explorer.png)

2. Bir işi yerel bir projeye aktarmak için, Visual Studio Code **Stream Analytics Gezgininde** dışarı aktarmak istediğiniz işi bulun. Ardından projeniz için bir klasör seçin.

    ![Visual Studio Code ASA işini dışarı aktarma](./media/vscode-explore-jobs/export-job.png)

    Proje seçtiğiniz klasöre aktarılmışsa ve geçerli çalışma alanınıza eklemiş olursunuz.

    ![Visual Studio Code ASA işini dışarı aktarma](./media/stream-analytics-manage-job/copy-backup-stream-analytics-jobs.png)

3. Farklı bir ad kullanarak işi başka bir bölgeye veya yedeklemeye yayımlamak için, sorgu Düzenleyicisi 'nde (\*. aşama QL) **yayımlamak üzere aboneliklerinizden Seç** ' i seçin ve yönergeleri izleyin.

    ![Visual Studio Code Azure 'da yayımlayın](./media/quick-create-vs-code/submit-job.png)

## <a name="visual-studio"></a>Visual Studio

1. [Dağıtılan Azure Stream Analytics işini bir proje yönergelerine dışarı aktarma](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-vs-tools#export-jobs-to-a-project)işlemini izleyin.

2. Sorgu Düzenleyicisi 'nde \*. asaql dosyasını açın, betik düzenleyicisinde **Azure 'A gönder** ' i seçin ve yeni bir ad kullanarak işi başka bir bölgeye veya yedeklemeye yayımlamak için yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Visual Studio Code kullanarak Stream Analytics işi oluşturma](quick-create-vs-code.md)
* [Hızlı başlangıç: Visual Studio 'Yu kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Azure Pipelines kullanarak CI/CD ile Azure Stream Analytics işi dağıtma](stream-analytics-tools-visual-studio-cicd-vsts.md)
