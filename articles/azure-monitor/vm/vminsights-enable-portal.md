---
title: Azure portal tek bir sanal makine veya sanal makine ölçek kümesi için Azure Izleyicisini etkinleştirin
description: Tek bir Azure sanal makinesinde veya Azure portal kullanarak sanal makine ölçek kümesinde VM öngörülerini etkinleştirmeyi öğrenin.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: 076fcab7b0747a7993407edd65f9d08efc27309f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102035594"
---
# <a name="enable-azure-monitor-for-single-virtual-machine-or-virtual-machine-scale-set-in-the-azure-portal"></a>Azure portal tek bir sanal makine veya sanal makine ölçek kümesi için Azure Izleyicisini etkinleştirin
Bu makalede, Azure portal kullanarak bir sanal makine veya sanal makine ölçek kümesi için VM öngörülerinin nasıl etkinleştirileceği açıklanır. Bu yordam aşağıdakiler için kullanılabilir:

- Azure sanal makine
- Azure sanal makine ölçek kümesi
- Azure Arc ile bağlantılı karma sanal makine

## <a name="prerequisites"></a>Önkoşullar

- [Log Analytics çalışma alanı oluşturun ve yapılandırın](./vminsights-configure-workspace.md). Alternatif olarak, bu işlem sırasında yeni bir çalışma alanı oluşturabilirsiniz.
- Etkinleştirmiş olduğunuz sanal makine veya sanal makine ölçek kümesinin işletim sisteminin desteklendiğinden emin olmak için [desteklenen işletim sistemleri](./vminsights-enable-overview.md#supported-operating-systems) bölümüne bakın. 

## <a name="enable-vm-insights"></a>VM öngörülerini etkinleştir

Azure portal **sanal makineler**, **Sanal Makine Ölçek Kümeleri** veya **sunucular-Azure yay** ' i seçin ve listeden bir kaynak seçin. Menünün **izleme** bölümünde **Öngörüler** ' i ve ardından **Etkinleştir**' i seçin. Aşağıdaki örnekte bir Azure sanal makinesi gösterilmektedir, ancak menü Azure sanal makine ölçek kümesi veya Azure Arc için benzerdir.

![VM için VM öngörülerini etkinleştirme](media/vminsights-enable-portal/enable-vminsights-vm-portal.png)

Sanal makine zaten bir Log Analytics çalışma alanına bağlı değilse, bir tane seçmeniz istenir. Daha önce [bir çalışma alanı oluşturmadıysanız](../logs/quick-create-workspace.md), sanal makine veya sanal makine ölçek kümesinin abonelikte dağıtıldığı konum için varsayılan seçeneği belirleyebilirsiniz. Bu çalışma alanı, zaten mevcut değilse oluşturulacak ve yapılandırılacaktır. Mevcut bir çalışma alanını seçerseniz VM öngörüleri henüz yoksa yapılandırılır.

> [!NOTE]
> Daha önce VM öngörüleri için yapılandırılmayan bir çalışma alanı seçerseniz, bu çalışma alanına *Vminsıghts* yönetim paketi eklenecektir. Bu, sanal makine öngörüleri için etkin olup olmadığına bakılmaksızın, çalışma alanına zaten bağlı olan herhangi bir aracıya uygulanır. Performans verileri bu sanal makinelerden toplanacak ve *ınsightsölçümlerini* tablosunda depolanacaktır.

![Çalışma alanını seçme](media/vminsights-enable-portal/select-workspace.png)

Yapılandırma gerçekleştirilirken durum mesajları alacaksınız.

>[!NOTE]
>Sanal makine ölçek kümesi için el ile yükseltme modeli kullanıyorsanız, kurulumu tamamladıktan sonra örnekleri yükseltin. **Ayarlar** bölümünde, **örnekler** sayfasından yükseltmeleri başlatabilirsiniz.

![VM öngörüleri izleme dağıtım işlemeyi etkinleştir](media/vminsights-enable-portal/onboard-vminsights-vm-portal-status.png)



## <a name="next-steps"></a>Sonraki adımlar

* Bulunan uygulama bağımlılıklarını görüntülemek için bkz. [VM Insights haritasını kullanma](vminsights-maps.md) . 
* Performans sorunlarını, genel kullanımı ve sanal makinenizin performansını belirlemek için bkz. [Azure VM performansını görüntüleme](vminsights-performance.md) .
