---
title: Azure portal VM'ler için Azure İzleyici etkinleştirme
description: Tek bir Azure sanal makinesinde veya bir sanal makine ölçek kümesinde VM'ler için Azure İzleyici değerlendirmeyi öğrenin.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/25/2020
ms.openlocfilehash: 4cdb9390b3146df74f2cbe8eba7b170a5d11fb2c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85507067"
---
# <a name="enable-azure-monitor-for-single-vm-or-vmss-in-the-azure-portal"></a>Azure portal tek VM veya VMSS için Azure Izleyicisini etkinleştirin
Bu makalede, Azure portal kullanılarak tek bir sanal makine veya sanal makine ölçek kümesi için VM'ler için Azure İzleyici nasıl etkinleştirileceği açıklanır. Bu yordam aşağıdakiler için kullanılabilir:

- Azure sanal makine
- Azure sanal makine ölçek kümesi
- Azure Arc makinesi

Başlamadan önce [önkoşulları](vminsights-enable-overview.md) gözden geçirin ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığından emin olun.  

## <a name="enable-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici etkinleştir

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. **Sanal makineler**, **Sanal Makine Ölçek Kümeleri**veya **makineler-Azure Arc**' ı seçin.

1. Listeden bir kaynak seçin.

1. Menünün **izleme** bölümünde **Öngörüler** ' i ve ardından **Etkinleştir**' i seçin. Aşağıdaki örnekte bir Azure sanal makinesi gösterilmektedir, ancak menü Azure VMSS veya Azure Arc için benzerdir.

    ![VM için VM'ler için Azure İzleyici etkinleştirme](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png)

1. VM zaten bir Log Analytics çalışma alanına bağlı değilse, bir tane seçmeniz istenir. Daha önce [bir çalışma alanı oluşturmadıysanız](../../azure-monitor/learn/quick-create-workspace.md), VM veya VMSS 'nin abonelikte dağıtıldığı konum için varsayılan değeri seçebilirsiniz. Bu çalışma alanı, zaten mevcut değilse oluşturulacak ve yapılandırılacaktır.

2. Yapılandırma gerçekleştirilirken durum mesajları alacaksınız.

    >[!NOTE]
    >Ölçek kümesi için el ile yükseltme modeli kullanıyorsanız, kurulumu tamamladıktan sonra örnekleri yükseltin. **Ayarlar** bölümünde, **örnekler** sayfasından yükseltmeleri başlatabilirsiniz.

    ![Dağıtım işlemeyi VM'ler için Azure İzleyici izlemeyi etkinleştir](media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)



## <a name="next-steps"></a>Sonraki adımlar

* Bulunan uygulama bağımlılıklarını görüntülemek için bkz. [VM'ler için Azure izleyici haritasını kullanma](vminsights-maps.md). 
* Performans sorunlarını, genel kullanımı ve sanal makinenizin performansını belirlemek için bkz. [Azure VM performansını görüntüleme](vminsights-performance.md).
