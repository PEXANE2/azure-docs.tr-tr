---
title: Azure portal tek bir sanal makine veya sanal makine ölçek kümesi için Azure Izleyicisini etkinleştirin
description: Azure portal kullanarak tek bir Azure sanal makinesinde veya sanal makine ölçek kümesinde VM'ler için Azure İzleyici etkinleştirmeyi öğrenin.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: ba6ae9262fb0e55c53e1b1421c075e555fae8d98
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87328369"
---
# <a name="enable-azure-monitor-for-single-virtual-machine-or-virtual-machine-scale-set-in-the-azure-portal"></a>Azure portal tek bir sanal makine veya sanal makine ölçek kümesi için Azure Izleyicisini etkinleştirin
Bu makalede, Azure portal kullanarak bir sanal makine veya sanal makine ölçek kümesi için VM'ler için Azure İzleyici nasıl etkinleştirileceği açıklanır. Bu yordam aşağıdakiler için kullanılabilir:

- Azure sanal makine
- Azure sanal makine ölçek kümesi
- Azure Arc ile bağlantılı karma sanal makine

## <a name="prerequisites"></a>Önkoşullar

- [Log Analytics çalışma alanı oluşturun ve yapılandırın](vminsights-configure-workspace.md). Alternatif olarak, bu işlem sırasında yeni bir çalışma alanı oluşturabilirsiniz.
- Etkinleştirmiş olduğunuz sanal makine veya sanal makine ölçek kümesinin işletim sisteminin desteklendiğinden emin olmak için [desteklenen işletim sistemleri](vminsights-enable-overview.md#supported-operating-systems) bölümüne bakın. 

## <a name="enable-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici etkinleştir

Azure portal **sanal makineler**, **Sanal Makine Ölçek Kümeleri**veya **makineler-Azure yay** ' i seçin ve listeden bir kaynak seçin. Menünün **izleme** bölümünde **Öngörüler** ' i ve ardından **Etkinleştir**' i seçin. Aşağıdaki örnekte bir Azure sanal makinesi gösterilmektedir, ancak menü Azure sanal makine ölçek kümesi veya Azure Arc için benzerdir.

![VM için VM'ler için Azure İzleyici etkinleştirme](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png)

Sanal makine zaten bir Log Analytics çalışma alanına bağlı değilse, bir tane seçmeniz istenir. Daha önce [bir çalışma alanı oluşturmadıysanız](../../azure-monitor/learn/quick-create-workspace.md), sanal makine veya sanal makine ölçek kümesinin abonelikte dağıtıldığı konum için varsayılan seçeneği belirleyebilirsiniz. Bu çalışma alanı, zaten mevcut değilse oluşturulacak ve yapılandırılacaktır. Mevcut bir çalışma alanını seçerseniz, zaten yoksa VM'ler için Azure İzleyici için yapılandırılır.

> [!NOTE]
> Daha önce VM'ler için Azure İzleyici için yapılandırılmayan bir çalışma alanı seçerseniz, *Vminsights* yönetim paketi bu çalışma alanına eklenecektir. Bu, VM'ler için Azure İzleyici için etkin olup olmadığına bakılmaksızın, çalışma alanına zaten bağlı olan herhangi bir aracıya uygulanır. Performans verileri bu sanal makinelerden toplanacak ve *ınsightsölçümlerini* tablosunda depolanacaktır.

![Çalışma alanını seçme](media/vminsights-configure-workspace/select-workspace.png)

Yapılandırma gerçekleştirilirken durum mesajları alacaksınız.

>[!NOTE]
>Sanal makine ölçek kümesi için el ile yükseltme modeli kullanıyorsanız, kurulumu tamamladıktan sonra örnekleri yükseltin. **Ayarlar** bölümünde, **örnekler** sayfasından yükseltmeleri başlatabilirsiniz.

![Dağıtım işlemeyi VM'ler için Azure İzleyici izlemeyi etkinleştir](media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)



## <a name="next-steps"></a>Sonraki adımlar

* Bulunan uygulama bağımlılıklarını görüntülemek için bkz. [VM'ler için Azure izleyici haritasını kullanma](vminsights-maps.md) . 
* Performans sorunlarını, genel kullanımı ve sanal makinenizin performansını belirlemek için bkz. [Azure VM performansını görüntüleme](vminsights-performance.md) .
