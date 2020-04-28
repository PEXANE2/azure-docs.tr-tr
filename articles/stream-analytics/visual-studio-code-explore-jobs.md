---
title: Visual Studio Code Azure Stream Analytics işleri keşfet
description: Bu makalede bir Azure Stream Analytics işini yerel bir projeye aktarma, işleri listeleme ve iş varlıklarını görüntüleme işlemleri gösterilmektedir.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/15/2019
ms.topic: conceptual
ms.openlocfilehash: 1d3a02d3778f9b4113767c5f755d675aeadd901b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75479247"
---
# <a name="explore-azure-stream-analytics-with-visual-studio-code-preview"></a>Visual Studio Code ile Azure Stream Analytics araştırma (Önizleme)

Visual Studio Code uzantısının Azure Stream Analytics geliştiricilere Stream Analytics işlerini yönetmek için hafif bir deneyim sunar. Windows, Mac ve Linux 'ta kullanılabilir. Azure Stream Analytics uzantısıyla şunları yapabilirsiniz:

- İşleri [oluşturma](quick-create-vs-code.md), başlatma ve durdurma
- Mevcut işleri yerel bir projeye aktar
- İşleri listeleme ve iş varlıklarını görüntüleme

## <a name="export-a-job-to-a-local-project"></a>Yerel bir projeye bir işi dışarı aktarma

Bir işi yerel bir projeye aktarmak için, Visual Studio Code **Stream Analytics Gezgininde** dışarı aktarmak istediğiniz işi bulun. Ardından projeniz için bir klasör seçin. Proje, seçtiğiniz klasöre aktarıldığından Visual Studio Code işi yönetmeye devam edebilirsiniz. Stream Analytics işleri yönetmek için Visual Studio Code kullanma hakkında daha fazla bilgi için Visual Studio Code [hızlı başlangıç](quick-create-vs-code.md)başlığına bakın.

![Visual Studio Code ASA işini dışarı aktarma](./media/vscode-explore-jobs/export-job.png)

## <a name="list-job-and-view-job-entities"></a>İşi listeleme ve iş varlıklarını görüntüleme

Visual Studio 'dan Azure Stream Analytics işlerle etkileşim kurmak için iş görünümünü kullanabilirsiniz.


1. Visual Studio Code etkinlik çubuğundaki **Azure** simgesine tıklayın ve ardından **Stream Analytics düğümünü**genişletin. İşleriniz aboneliklerinizin altında görünmelidir.

   ![Stream Analytics Gezginini aç](./media/vscode-explore-jobs/open-explorer.png)

2. İş düğümünü genişletin, iş sorgusunu, yapılandırmayı, girişleri, çıkışları ve işlevleri açabilir ve görüntüleyebilirsiniz. 

3. İş düğümünüz üzerinde sağ tıklayın ve iş görünümünü Azure portal açmak için **Portal düğümünde Iş görünümünü aç** ' ı seçin.

   ![Portalda iş görünümünü aç](./media/vscode-explore-jobs/open-job-view.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code Azure Stream Analytics bulut işi oluşturma (Önizleme)](quick-create-vs-code.md)
