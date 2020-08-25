---
title: 'Öğretici: özel bulutu ölçeklendirme'
description: Bu öğreticide, Azure VMware Çözüm önizleme özel bulutunu ölçeklendirmek için Azure portal kullanırsınız.
ms.topic: tutorial
ms.date: 08/21/2020
ms.openlocfilehash: dddfbddd57f3ad6b541d11c360aeadea4383044a
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750439"
---
# <a name="tutorial-scale-an-azure-vmware-solution-preview-private-cloud"></a>Öğretici: Azure VMware Çözüm önizlemesi özel bulutu ölçeklendirme

Azure VMware Çözüm önizleme özel bulut deneyiminizden en iyi şekilde yararlanmak için kümeleri ve Konakları, planlı iş yükleri için gerekenleri yansıtacak şekilde ölçeklendirin. Azure VMware çözümü önizleme sırasında şirket içi vCenter 'ı desteklemediğinden, Azure portal aracılığıyla zaten oluşturduğunuz öğeleri kullanmanız gerekir.

Uygulama iş yükünüz için gereken küme sayısını ve özel buluttaki ana bilgisayar sayısını ölçeklendirebilirsiniz. Belirli hizmetler için performans ve kullanılabilirlik sınırlamaları, Azure VMware Çözüm önizleme bulut ortamınız dahilinde bir servis talebiyle aynı şekilde değinilmesi gerekir. Özel buluttaki küme ve konak sınırları [özel bulut kavramı makalesinde](concepts-private-clouds-clusters.md)sunulmaktadır.

Bu öğreticide, Azure portal için kullanın:

> [!div class="checklist"]
> * Mevcut bir özel buluta küme ekleme
> * Mevcut bir kümeye konaklar ekleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlayabilmeniz için özel bir buluta ihtiyacınız vardır. Henüz özel bir bulut oluşturmadıysanız, özel bulut oluşturmak için [özel bulut Oluşturma öğreticisini](tutorial-create-private-cloud.md) kullanın ve gerekli sanal ağı ayarlamak için Azure 'daki VMware özel bulutunuz için ağ iletişimini yapılandırın.

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
