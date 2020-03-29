---
title: Visual Studio Code'ta Azure Akış Analizi işlerini keşfedin
description: Bu makalede, bir Azure Akış Analizi işini yerel bir projeye nasıl dışa aktarabileceğinizi, işleri listelediğinizi ve iş varlıklarını nasıl görüntüleyebilirsiniz gösterilmektedir.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/15/2019
ms.topic: conceptual
ms.openlocfilehash: 1d3a02d3778f9b4113767c5f755d675aeadd901b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75479247"
---
# <a name="explore-azure-stream-analytics-with-visual-studio-code-preview"></a>Visual Studio Code (Önizleme) ile Azure Akış Analizini Keşfedin

Visual Studio Code için Azure Akış Analizi uzantısı, geliştiricilere Stream Analytics işlerini yönetmeleri için hafif bir deneyim sunar. Windows, Mac ve Linux'ta kullanılabilir. Azure Akışı Analizi uzantısı ile şunları yapabilirsiniz:

- [İş oluşturma,](quick-create-vs-code.md)başlatma ve durdurma
- Varolan işleri yerel bir projeye aktarma
- İşleri listele ve iş varlıklarını görüntüleyin

## <a name="export-a-job-to-a-local-project"></a>Yerel bir projeye iş verme

Yerel bir projeye iş aktarmak için, Dışa aktarmak istediğiniz işi Visual Studio Code'daki **Akış Analizi Gezgini'nde** bulun. Ardından projeniz için bir klasör seçin. Proje seçtiğiniz klasöre dışa aktarılır ve işi Visual Studio Code'dan yönetmeye devam edebilirsiniz. Akış Analizi işlerini yönetmek için Visual Studio Code'u kullanma hakkında daha fazla bilgi için Visual Studio Code [quickstart](quick-create-vs-code.md)bölümüne bakın.

![Visual Studio Code'da ASA işini İhraç edin](./media/vscode-explore-jobs/export-job.png)

## <a name="list-job-and-view-job-entities"></a>İş ve görünüm iş varlıklarını listele

İş görünümünü Visual Studio'daki Azure Akış Analizi işleriyle etkileşimkurmak için kullanabilirsiniz.


1. Visual Studio Code Activity Bar'daki **Azure** simgesini tıklatın ve ardından **Akış Analizi düğüm'üne**genişletin. İşleriniz aboneliklerinizin altında görünmelidir.

   ![Açık Akış Analizi Explorer](./media/vscode-explore-jobs/open-explorer.png)

2. İş düğümünüzün genişletilmesini, iş sorgusunu, yapılandırmayı, girdileri, çıktıları ve işlevleri ni açıp görüntüleyebilirsiniz. 

3. İş düğümünüze sağ tıklayın ve Azure portalındaki iş görünümünü açmak için **Portal düğümünde İş Görünümü Aç'ı** seçin.

   ![Portalda iş görünümünü aç](./media/vscode-explore-jobs/open-job-view.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code'da Bir Azure Akışı Analizi bulut işi oluşturun (Önizleme)](quick-create-vs-code.md)
