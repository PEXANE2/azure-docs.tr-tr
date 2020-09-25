---
title: Öğretici-özel bulutu ölçeklendirme
description: Bu öğreticide, Azure VMware çözümü özel bulutunu ölçeklendirmek için Azure portal kullanırsınız.
ms.topic: tutorial
ms.date: 09/21/2020
ms.openlocfilehash: e1226eb98607a34869bda1f998c7cecea2e50919
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91254423"
---
# <a name="tutorial-scale-an-azure-vmware-solution-private-cloud"></a>Öğretici: Azure VMware çözümünü özel bulutu ölçeklendirme

Azure VMware Çözüm özel bulut deneyiminizden en iyi şekilde yararlanmak için kümeleri ve Konakları, planlı iş yükleri için gerekenleri yansıtacak şekilde ölçeklendirin. Uygulama iş yükünüz için gereken küme sayısını ve özel buluttaki ana bilgisayar sayısını ölçeklendirebilirsiniz. Belirli hizmetler için performans ve kullanılabilirlik sınırlamaları, Azure VMware çözümünüz özel bulut ortamınız dahilinde bir servis talebiyle aynı şekilde değinilmesi gerekir. Küme ve konak sınırları, [özel bulut kavramı](concepts-private-clouds-clusters.md) makalesinde sunulmaktadır.

Bu öğreticide, Azure portal için kullanacaksınız:

> [!div class="checklist"]
> * Mevcut bir özel buluta küme ekleme
> * Mevcut bir kümeye konaklar ekleme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için özel bir buluta ihtiyacınız vardır. Henüz özel bir bulut oluşturmadıysanız, gerekli sanal ağı kurmak üzere Azure 'daki VMware özel bulutunuz için bir [özel bulut Oluşturma öğreticisini](tutorial-create-private-cloud.md) kullanarak bir tane oluşturun.

## <a name="add-a-new-cluster"></a>Yeni küme ekleme

1. Mevcut bir özel bulutun Genel Bakış sayfasında, **Yönet**altında, **özel bulutu Ölçeklendir**' i seçin. Ardından **+ küme Ekle**' yi seçin.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss2-select-add-cluster.png" alt-text="Küme Ekle ' yi seçin" border="true":::

1. **Küme Ekle** sayfasında, ana bilgisayar sayısını seçmek için kaydırıcıyı kullanın. **Kaydet**’i seçin.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss3-configure-new-cluster.png" alt-text="Küme Ekle sayfasında, ana bilgisayar sayısını seçmek için kaydırıcıyı kullanın. Kaydet ' i seçin." border="true":::

   Yeni kümenin dağıtımı başlar.

## <a name="scale-a-cluster"></a>Kümeyi ölçeklendirme 

1. Mevcut bir özel bulutun Genel Bakış sayfasında, **özel bulutu Ölçeklendir** ' i seçin ve kümeyi düzenlemek için kurşun kalem simgesini seçin.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss4-select-scale-private-cloud-2.png" alt-text="Genel görünümde ölçek özel bulutu seçin" border="true":::

1. **Kümeyi Düzenle** sayfasında, ana bilgisayar sayısını seçmek için kaydırıcıyı kullanın. **Kaydet**’i seçin.

   :::image type="content" source="./media/tutorial-scale-private-cloud/ss5-scale-cluster.png" alt-text="Kümeyi Düzenle sayfasında, ana bilgisayar sayısını seçmek için kaydırıcıyı kullanın. Kaydet ' i seçin." border="true":::

   Ana bilgisayarların kümeye eklenmesi başlayacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Başka bir Azure VMware çözümü özel bulutuna ihtiyacınız varsa, aynı ağ önkoşullarını, küme ve konak limitlerini takip eden [başka bir özel bulut oluşturun](tutorial-create-private-cloud.md).

<!-- LINKS - external-->

<!-- LINKS - internal -->
