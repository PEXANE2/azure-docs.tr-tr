---
title: Azure portalında VM'ler için Azure Monitörünü etkinleştirme
description: Azure Monitor'u SANAL M'ler için tek bir Azure sanal makinede veya sanal makine ölçeği nde nasıl değerlendireceklerini öğrenin.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 45bc8f16a547d4a95820f9dcd02132844b3be83c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480717"
---
# <a name="enable-azure-monitor-for-vms-in-the-azure-portal"></a>Azure portalında VM'ler için Azure Monitörünü etkinleştirme

Bu makalede, Azure portalını kullanarak az sayıda Azure sanal makinede (VM) Azure Monitor'un VM'ler için nasıl etkinleştirilen bir şekilde etkinleştirilen anlatılmaktadır. Amacınız, VM'lerinizi izlemek ve performans veya kullanılabilirlik sorunlarını keşfetmektir. 

Başlamadan önce ön [koşulları](vminsights-enable-overview.md) gözden geçirin ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığından emin olun.  

## <a name="enable-monitoring-for-a-single-azure-vm"></a>Tek bir Azure VM için izlemeyi etkinleştirme
Azure VM'nizin izlenmesini etkinleştirmek için:

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. **Sanal Makineleri**seçin.

1. Listeden bir VM seçin.

1. VM sayfasında, **İzleme** **bölümünde, İstatistikler'i** seçin ve ardından **Etkinleştir'i etkinleştirin.**

    ![VM için VM'ler için Azure Monitörünü etkinleştirme](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png)

1. Azure **Monitor Öngörüleri Onboarding** sayfasında, aynı abonelikte mevcut bir Log Analytics çalışma alanınız varsa, açılan listede seçin.  

    Liste, varsayılan çalışma alanını ve VM'nin abonelikte dağıtıldığı konumu önceden seçer. 

    >[!NOTE]
    >VM'deki izleme verilerini depolamak için yeni bir Log [Analytics çalışma](../../azure-monitor/learn/quick-create-workspace.md)alanı oluşturmak için bkz. Log Analytics çalışma alanınız desteklenen [bölgelerden](vminsights-enable-overview.md#log-analytics)birine ait olmalıdır.

6. Yapılandırma gerçekleştirilirken durum iletileri alırsınız.

    ![VM izleme dağıtım işlemleri için Azure Monitörünü etkinleştirme](media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="enable-monitoring-for-a-single-virtual-machine-scale-set"></a>Tek bir sanal makine ölçeği kümesi için izlemeyi etkinleştirme

Azure sanal makine ölçek kümenizin izlenmesini etkinleştirmek için:

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. **Sanal Makine Ölçek Kümeleri'ni**seçin.

3. Listeden sanal makine ölçeği kümesini seçin.

4. Sanal makine ölçeği ayar sayfasında, **İzleme** **bölümünde, İstatistikler'i** seçin ve ardından **Etkinleştir'i etkinleştirin.**

5. **Öngörüler** sayfasında, varolan bir Log Analytics çalışma alanını kullanmak istiyorsanız, açılan listede seçin.

    Liste, VM'nin abonelikte dağıtılan varsayılan çalışma alanını ve konumunu önceden seçer. 

    ![Sanal makine ölçeği kümesi için SANAL M'ler için Azure Monitörünü etkinleştirme](media/vminsights-enable-single-vm/enable-vminsights-vmss-portal.png)

    >[!NOTE]
    >İzleme verilerini sanal makine ölçeği kümesinden depolamak için yeni bir Log [Analytics çalışma](../learn/quick-create-workspace.md)alanı oluşturmak için bkz. Log Analytics çalışma alanınız desteklenen [bölgelerden](vminsights-enable-overview.md#log-analytics)birine ait olmalıdır.

6. Yapılandırma gerçekleştirilirken durum iletileri alırsınız.

    >[!NOTE]
    >Ölçek kümeniz için el ile yükseltme modeli kullanıyorsanız, kurulumu tamamlamak için örnekleri yükseltin. Yükseltmeleri **Ayarlar** bölümünden **Örnekler** sayfasından başlatabilirsiniz.
    
    ![VM izleme dağıtım işlemleri için Azure Monitörünü etkinleştirme](media/vminsights-enable-single-vm/onboard-vminsights-vmss-portal-status.png)

VM veya sanal makine ölçeği kümeniz için izlemeyi etkinleştirdiğinize göre, izleme bilgileri VM'ler için Azure Monitor'da analiz edilebilir. 

## <a name="next-steps"></a>Sonraki adımlar

* Keşfedilen uygulama bağımlılıklarını görüntülemek [için VM Haritası için Azure Monitörünü Kullan'a](vminsights-maps.md)bakın. 
* Darboğazları, genel kullanımı ve VM'nizin performansını belirlemek için Bkz. [Azure VM performansını görüntüleyin.](vminsights-performance.md)
