---
title: Azure Akış Analizi işlerini kopyalama veya yedekleme
description: Bu makalede, bir Azure Akış Analizi işinin nasıl kopyalanır veya yedeklenir.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 5c8f770855dd8d19a9d313f1b79f9bf8da4b2393
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75771504"
---
# <a name="copy-or-back-up-azure-stream-analytics-jobs"></a>Azure Akış Analizi işlerini kopyalama veya yedekleme

Visual Studio Code veya Visual Studio'u kullanarak dağıtılan Azure Akış Analizi işlerinizi kopyalayabilir veya yedekleyebilirsiniz. 

## <a name="before-you-begin"></a>Başlamadan önce
* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/)oluşturun.

* [Azure portalında](https://portal.azure.com/)oturum açın.

* Visual Studio Kodu veya Visual Studio için Azure Akış Analizi araçları [için Azure Akış Analizi uzantısını](quick-create-vs-code.md#install-the-azure-stream-analytics-tools-extension) [yükleyin.](quick-create-vs-code.md#install-the-azure-stream-analytics-tools-extension)  

## <a name="visual-studio-code"></a>Visual Studio Code

1. Visual Studio Code Activity Bar'daki **Azure** simgesini tıklatın ve ardından **Akış Analizi** düğümlerini genişletin. İşleriniz aboneliklerinizin altında görünmelidir.

   ![Açık Akış Analizi Explorer](./media/vscode-explore-jobs/open-explorer.png)

2. Yerel bir projeye iş aktarmak için, Dışa aktarmak istediğiniz işi Visual Studio Code'daki **Akış Analizi Gezgini'nde** bulun. Ardından projeniz için bir klasör seçin.

    ![Visual Studio Code'da ASA işini İhraç edin](./media/vscode-explore-jobs/export-job.png)

    Proje seçtiğiniz klasöre dışa aktarılır ve geçerli çalışma alanınıza eklenir.

    ![Visual Studio Code'da ASA işini İhraç edin](./media/stream-analytics-manage-job/copy-backup-stream-analytics-jobs.png)

3. İşi başka bir ad kullanarak başka bir bölgeye veya yedeklemeye yayımlamak için, sorgu düzenleyicisinde (.asaql)\* **yayımlamak için aboneliklerinizden Seç'i** seçin ve yönergeleri izleyin.

    ![Visual Studio Code'da Azure'da yayımlama](./media/quick-create-vs-code/submit-job.png)

## <a name="visual-studio"></a>Visual Studio

1. [Dağıtılmış bir Azure Akışı Analizi işini proje yönergelerine dışa](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-vs-tools#export-jobs-to-a-project)aktarma'yı izleyin.

2. Sorgu \*Düzenleyicisi'nde .asaql dosyasını açın, komut dosyası düzenleyicisinde **Azure'a Gönder'i** seçin ve işi başka bir bölgeye veya yeni bir ad kullanarak yedeklemeye yayımlamak için yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Quickstart: Visual Studio Code kullanarak bir Stream Analytics iş oluşturun](quick-create-vs-code.md)
* [Quickstart: Visual Studio'u kullanarak Bir Akış Analizi işi oluşturun](stream-analytics-quick-create-vs.md)
* [Azure Pipelines kullanarak CI/CD ile Azure Stream Analytics işi dağıtma](stream-analytics-tools-visual-studio-cicd-vsts.md)
