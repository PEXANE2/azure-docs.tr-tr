---
title: 'Öğretici: özel bulutu ölçeklendirme'
description: Bu öğreticide, Azure VMware Solution (AVS) önizleme özel bulutunu ölçeklendirmek için Azure portal kullanırsınız.
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: 70d8c1c555badd6102f4b2547492bdea54e55783
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82740293"
---
# <a name="tutorial-scale-an-azure-vmware-solution-avs-preview-private-cloud"></a>Öğretici: Azure VMware çözümünü (AVS) önizleme özel bulutu ölçeklendirme

AVS önizlemesinin özel bulut deneyiminizden en iyi şekilde yararlanmak için kümeleri ve Konakları, planlı iş yükleri için gerekenleri yansıtacak şekilde ölçeklendirin. AVS, önizleme sırasında şirket içi vCenter 'ı desteklemediğinden, Azure portal aracılığıyla zaten oluşturduğunuz öğeleri kullanmanız gerekir.

Uygulama iş yükünüz için gereken küme sayısını ve özel buluttaki ana bilgisayar sayısını ölçeklendirebilirsiniz. Belirli hizmetlere yönelik performans ve kullanılabilirlik sınırlamaları, AVS önizleme bulut ortamınızdan büyük bir şekilde bir servis talebi ile ilgilenilmesi gerekir. Özel buluttaki küme ve konak sınırları [özel bulut kavramı makalesinde](concepts-private-clouds-clusters.md)sunulmaktadır.

Bu öğreticide, Azure portal için kullanın:

> [!div class="checklist"]
> * Mevcut bir özel buluta küme ekleme
> * Mevcut bir kümeye konaklar ekleme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için özel bir buluta ihtiyacınız vardır. Henüz özel bir bulut oluşturmadıysanız, özel bulut oluşturmak için [özel bulut Oluşturma öğreticisini](tutorial-create-private-cloud.md) kullanın ve gerekli sanal ağı ayarlamak için Azure 'daki VMware özel bulutunuz için ağ iletişimini yapılandırın.

## <a name="add-a-new-cluster"></a>Yeni küme ekleme

Mevcut bir özel bulutun Genel Bakış sayfasında, **Yönet**altında, **özel bulutu Ölçeklendir**' i seçin. Ardından **+ küme Ekle**' yi seçin.

:::image type="content" source="./media/tutorial-scale-private-cloud/ss2-select-add-cluster.png" alt-text="Küme Ekle ' yi seçin" border="true":::

**Küme Ekle** sayfasında, ana bilgisayar sayısını seçmek için kaydırıcıyı kullanın. **Kaydet**’i seçin.

:::image type="content" source="./media/tutorial-scale-private-cloud/ss3-configure-new-cluster.png" alt-text="Yeni bir özel bulut kümesi yapılandırma" border="true":::

Yeni kümenin dağıtımı başlar.

## <a name="scale-a-cluster"></a>Kümeyi ölçeklendirme 

Mevcut bir özel bulutun Genel Bakış sayfasında, **özel bulutu Ölçeklendir** ' i seçin ve kümeyi düzenlemek için kurşun kalem simgesini seçin.

:::image type="content" source="./media/tutorial-scale-private-cloud/ss4-select-scale-private-cloud-2.png" alt-text="Genel görünümde ölçek özel bulutu seçin" border="true":::

**Kümeyi Düzenle** sayfasında, ana bilgisayar sayısını seçmek için kaydırıcıyı kullanın. **Kaydet**’i seçin.

:::image type="content" source="./media/tutorial-scale-private-cloud/ss5-scale-cluster.png" alt-text="Yeni bir özel bulut kümesi yapılandırma" border="true":::

Ana bilgisayarların kümeye eklenmesi başlayacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Başka bir AVS özel buluta ihtiyacınız varsa, aynı ağ önkoşullarını, küme ve konak limitlerini takip eden [başka bir özel bulut oluşturun](tutorial-create-private-cloud.md)...

<!-- LINKS - external-->

<!-- LINKS - internal -->
