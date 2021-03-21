---
title: Çalışma öğesi tümleştirmesi (Önizleme)-Application Insights
description: GitHub 'da veya Azure DevOps 'da ekli Application Insights verilerle iş öğeleri oluşturmayı öğrenin.
ms.topic: conceptual
ms.date: 02/9/2021
ms.openlocfilehash: ba0a67bad3ba47191414d6b406ab6cb4e6b7da78
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101731927"
---
# <a name="work-item-integration-preview"></a>Çalışma öğesi tümleştirmesi (Önizleme)

Çalışma öğesi tümleştirme işlevselliği, GitHub veya Azure DevOps 'da, bunlara eklenmiş ilgili Application Insights verileri içeren iş öğelerini kolayca oluşturmanızı sağlar.

> [!IMPORTANT]
> İş öğesi tümleştirmesi Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="create-and-configure-a-work-item-template"></a>İş öğesi şablonu oluşturma ve yapılandırma

1. Bir iş öğesi şablonu oluşturmak için Application Insights kaynağına gidin ve sol tarafta Select **Iş öğelerini** *Yapılandır* ' ın altında, en üstteki **Yeni Şablon Oluştur** ' u seçin.

    :::image type="content" source="./media/work-item-integration/create-work-item-template.png" alt-text=" Yeni Şablon Oluştur seçiliyken Iş öğeleri sekmesinin ekran görüntüsü." lightbox="./media/work-item-integration/create-work-item-template.png":::

    Geçerli bir şablon yoksa, uçtan uca işlem ayrıntıları sekmesinden bir iş öğesi şablonu da oluşturabilirsiniz. Bir olay seçin ve sağ tarafta **bir iş öğesi oluştur**' u seçin ve ardından **bir çalışma kitabı şablonuyla başlayın**.

    :::image type="content" source="./media/work-item-integration/create-template-from-transaction-details.png" alt-text=" İş öğesi oluştur ' u kullanarak uçtan uca işlem ayrıntıları sekmesinin ekran görüntüsü seçili bir çalışma kitabı şablonuyla başlayın." lightbox="./media/work-item-integration/create-template-from-transaction-details.png":::

2. **Yeni Şablon Oluştur**' u seçtikten sonra, izleme sistemlerinizi seçebilir, çalışma kitabınızı verebilir, seçtiğiniz izleme sistemine bağlanabilir ve şablonu depolamak için bir bölge seçebilirsiniz (varsayılan değer Application Insights kaynağınızın bulunduğu bölgedir). URL parametreleri, deponuzun varsayılan URL 'sidir, örneğin `https://github.com/myusername/reponame` veya `https://mydevops.visualstudio.com/myproject` .

    :::image type="content" source="./media/work-item-integration/create-workbook.png" alt-text=" Yeni iş öğesi çalışma kitabı şablonu oluştur ekran görüntüsü.":::

## <a name="create-a-work-item"></a>Çalışma öğesi oluşturma

 Yeni şablonunuza, performans, başarısızlık, kullanılabilirlik veya diğer sekmelerin erişebileceği herhangi bir uçtan uca işlem ayrıntısından erişebilirsiniz.

1. Bir iş öğesi oluşturmak için uçtan uca işlem ayrıntılarına gidin, bir olay seçin, sonra **iş öğesi oluştur** ' u seçin ve iş öğesi şablonunuzu seçin.

    :::image type="content" source="./media/work-item-integration/create-work-item.png" alt-text=" Çalışma öğesi Oluştur seçiliyken uçtan uca işlem ayrıntıları sekmesinin ekran görüntüsü." lightbox="./media/work-item-integration/create-work-item.png":::

1. Tarayıcınızda yeni bir sekme, seçim izleme sisteminize açılır. Azure DevOps 'da bir hata veya görev oluşturabilir ve GitHub 'da deponuzda yeni bir sorun oluşturabilirsiniz. Yeni bir iş öğesi, Application Insights tarafından belirtilen bağlamsal bilgilerle otomatik olarak oluşturulur.

    :::image type="content" source="./media/work-item-integration/github-work-item.png" alt-text=" Otomatik olarak oluşturulan GitHub sorununun ekran görüntüsü" lightbox="./media/work-item-integration/github-work-item.png":::

    :::image type="content" source="./media/work-item-integration/azure-devops-work-item.png" alt-text=" Azure DevOps 'da otomatik olarak hata oluştur ekran görüntüsü." lightbox="./media/work-item-integration/azure-devops-work-item.png":::

## <a name="edit-a-template"></a>Bir şablonu düzenleme

Şablonunuzu düzenlemek için, *Yapılandır* altında **iş öğeleri** sekmesine gidin ve güncelleştirmek istediğiniz çalışma kitabının yanındaki kurşun kalem simgesini seçin.

:::image type="content" source="./media/work-item-integration/edit-template.png" alt-text=" Kalem simgesini Düzenle simgesi seçili olan iş öğesi sekmesinin ekran görüntüsü.":::

![ ](./media/work-item-integration/edit-icon.png) Şablonu düzenlemeye başlamak için üstteki düzenleme Düzenle simgesini seçin. İş öğesi şablonları, [Azure Izleyici çalışma kitaplarını](../visualize/workbooks-overview.md)temel alır. İş öğesi bilgileri anahtar sözcük sorgu dili kullanılarak oluşturulur. Ekibinize daha fazla bağlam eklemek için sorguları değiştirebilirsiniz. Düzenlemeden sonra, ![ üstteki araç çubuğunda Kaydet simgesini Kaydet simgesini seçerek çalışma kitabını kaydedin ](./media/work-item-integration/save-icon.png) .

:::image type="content" source="./media/work-item-integration/edit-workbook.png" alt-text=" Düzenleme modundaki çalışma öğesi şablonu çalışma kitabının ekran görüntüsü." lightbox="./media/work-item-integration/edit-workbook.png":::

Birden fazla iş öğesi yapılandırması oluşturabilir ve her senaryoyu karşılamak için özel bir çalışma kitabına sahip olabilirsiniz. Çalışma kitapları, ortamlarınızda standart uygulamalar sağlamak Azure Resource Manager tarafından da dağıtılabilir.