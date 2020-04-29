---
title: Azure portal VM'ler için Azure İzleyici etkinleştirme
description: Tek bir Azure sanal makinesinde veya bir sanal makine ölçek kümesinde VM'ler için Azure İzleyici değerlendirmeyi öğrenin.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 45bc8f16a547d4a95820f9dcd02132844b3be83c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79480717"
---
# <a name="enable-azure-monitor-for-vms-in-the-azure-portal"></a>Azure portal VM'ler için Azure İzleyici etkinleştirme

Bu makalede, Azure portal kullanarak az sayıda Azure sanal makinesi (VM) üzerinde VM'ler için Azure İzleyici nasıl etkinleştirileceği açıklanır. Amacınız, sanal makinelerinizi izlemek ve tüm performans veya kullanılabilirlik sorunlarını bulmaktır. 

Başlamadan önce [önkoşulları](vminsights-enable-overview.md) gözden geçirin ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığından emin olun.  

## <a name="enable-monitoring-for-a-single-azure-vm"></a>Tek bir Azure VM için izlemeyi etkinleştirme
Azure VM 'nizi izlemeyi etkinleştirmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Sanal makineleri**seçin.

1. Listeden bir VM seçin.

1. VM sayfasında, **izleme** bölümünde **Öngörüler** ' i ve ardından **Etkinleştir**' i seçin.

    ![VM için VM'ler için Azure İzleyici etkinleştirme](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png)

1. **Azure Izleyici öngörüleri ekleme** sayfasında, aynı abonelikte mevcut bir Log Analytics çalışma alanınız varsa, açılır listeden seçin.  

    Liste, varsayılan çalışma alanını ve VM 'nin abonelikte dağıtıldığı konumu önceden seçer. 

    >[!NOTE]
    >İzleme verilerini VM 'den depolamak üzere yeni bir Log Analytics çalışma alanı oluşturmak için, bkz. [Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md). Log Analytics çalışma alanınız [desteklenen bölgelerden](vminsights-enable-overview.md#log-analytics)birine ait olmalıdır.

6. Yapılandırma gerçekleştirilirken durum mesajları alacaksınız.

    ![Dağıtım işlemeyi VM'ler için Azure İzleyici izlemeyi etkinleştir](media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="enable-monitoring-for-a-single-virtual-machine-scale-set"></a>Tek bir sanal makine ölçek kümesi için izlemeyi etkinleştir

Azure sanal makine ölçek kümesini izlemeyi etkinleştirmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Sanal makine ölçek kümelerini**seçin.

3. Listeden bir sanal makine ölçek kümesi seçin.

4. Sanal makine ölçek kümesi sayfasında, **izleme** bölümünde **Öngörüler** ' i ve ardından **Etkinleştir**' i seçin.

5. **Öngörüler** sayfasında, mevcut bir Log Analytics çalışma alanını kullanmak istiyorsanız, açılır listeden seçin.

    Liste, varsayılan çalışma alanını ve VM 'nin abonelikte dağıtıldığı konumu önceden seçer. 

    ![Bir sanal makine ölçek kümesi için VM'ler için Azure İzleyici etkinleştir](media/vminsights-enable-single-vm/enable-vminsights-vmss-portal.png)

    >[!NOTE]
    >Sanal makine ölçek kümesinden izleme verilerini depolamak üzere yeni bir Log Analytics çalışma alanı oluşturmak için, bkz. [Log Analytics çalışma alanı oluşturma](../learn/quick-create-workspace.md). Log Analytics çalışma alanınız [desteklenen bölgelerden](vminsights-enable-overview.md#log-analytics)birine ait olmalıdır.

6. Yapılandırma gerçekleştirilirken durum mesajları alacaksınız.

    >[!NOTE]
    >Ölçek kümesi için el ile yükseltme modeli kullanıyorsanız, kurulumu tamamladıktan sonra örnekleri yükseltin. **Ayarlar** bölümünde, **örnekler** sayfasından yükseltmeleri başlatabilirsiniz.
    
    ![Dağıtım işlemeyi VM'ler için Azure İzleyici izlemeyi etkinleştir](media/vminsights-enable-single-vm/onboard-vminsights-vmss-portal-status.png)

VM 'niz veya sanal makine ölçek kümesi için izlemeyi etkinleştirmiş olduğunuza göre, izleme bilgileri VM'ler için Azure İzleyici analiz için kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar

* Bulunan uygulama bağımlılıklarını görüntülemek için bkz. [VM'ler için Azure izleyici haritasını kullanma](vminsights-maps.md). 
* Performans sorunlarını, genel kullanımı ve sanal makinenizin performansını belirlemek için bkz. [Azure VM performansını görüntüleme](vminsights-performance.md).
