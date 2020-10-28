---
title: Öğretici-özel bulutu ölçeklendirme
description: Bu öğreticide, Azure VMware çözümü özel bulutunu ölçeklendirmek için Azure portal kullanırsınız.
ms.topic: tutorial
ms.date: 09/21/2020
ms.openlocfilehash: d49d973cc6d97280dc0c7ea6681f2602b871e1ba
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92791248"
---
# <a name="tutorial-scale-an-azure-vmware-solution-private-cloud"></a>Öğretici: Azure VMware çözümünü özel bulutu ölçeklendirme

Azure VMware Çözüm özel bulut deneyiminizden en iyi şekilde yararlanmak için kümeleri ve Konakları, planlı iş yükleri için gerekenleri yansıtacak şekilde ölçeklendirin. Özel bir buluttaki kümeleri ve Konakları, uygulama iş yükünüz için gereken şekilde ölçeklendirebilirsiniz. Belirli hizmetler için performans ve kullanılabilirlik sınırlamaları, büyük/küçük harfe göre değinilmesi gerekir. Küme ve konak sınırları, [özel bulut kavramı](concepts-private-clouds-clusters.md) makalesinde sunulmaktadır.

Bu öğreticide, Azure portal için kullanacaksınız:

> [!div class="checklist"]
> * Mevcut bir özel buluta küme ekleme
> * Mevcut bir kümeye konaklar ekleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamaya yönelik özel bir bulut. Özel bir bulut oluşturmadıysanız, bir [özel bulut Oluşturma öğreticisini](tutorial-create-private-cloud.md) kullanarak bir tane oluşturun. Azure 'da gerekli sanal ağı ayarlamak için VMware özel bulutunuz için ağ iletişimini yapılandırın.

## <a name="add-a-new-cluster"></a>Yeni küme ekleme

1. Mevcut bir özel bulutun Genel Bakış sayfasında, **Yönet** altında, **özel bulutu Ölçeklendir** ' i seçin. Ardından **+ küme Ekle** ' yi seçin.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss2-select-add-cluster.png" alt-text="Küme Ekle ' yi seçin" border="true":::

1. **Küme Ekle** sayfasında, ana bilgisayar sayısını seçmek için kaydırıcıyı kullanın. **Kaydet** ’i seçin.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss3-configure-new-cluster.png" alt-text="Küme Ekle ' yi seçin" border="true":::

   Yeni kümenin dağıtımı başlar.

## <a name="scale-a-cluster"></a>Kümeyi ölçeklendirme 

1. Mevcut bir özel bulutun Genel Bakış sayfasında, **özel bulutu Ölçeklendir** ' i seçin ve kümeyi düzenlemek için kurşun kalem simgesini seçin.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss4-select-scale-private-cloud-2.png" alt-text="Küme Ekle ' yi seçin" border="true":::

1. **Kümeyi Düzenle** sayfasında, ana bilgisayar sayısını seçmek için kaydırıcıyı kullanın. **Kaydet** ’i seçin.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss5-scale-cluster.png" alt-text="Küme Ekle ' yi seçin" border="true":::

   Ana bilgisayarların kümeye eklenmesi başlayacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Başka bir Azure VMware çözümü özel buluta ihtiyacınız varsa, aynı ağ önkoşullarını, kümeyi ve konak sınırlarını izleyerek [başka bir özel bulut oluşturun](tutorial-create-private-cloud.md).

<!-- LINKS - external-->

<!-- LINKS - internal -->
