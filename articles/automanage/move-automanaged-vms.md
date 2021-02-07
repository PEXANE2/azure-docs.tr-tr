---
title: Bir Azure sanal makinesini bölgeler arasında taşıma
description: Bir oto tarafından yönetilen sanal makineyi bölgeler arasında taşımayı öğrenin
author: asinn826
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: how-to
ms.date: 02/05/2021
ms.author: alsin
ms.custom: subject-moving-resources
ms.openlocfilehash: 7e880992143bf79a5a99cc9830957f83167b6b46
ms.sourcegitcommit: 8245325f9170371e08bbc66da7a6c292bbbd94cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2021
ms.locfileid: "99808411"
---
# <a name="move-an-azure-automanage-virtual-machine-to-a-different-region"></a>Bir Azure oto sanal makinesini farklı bir bölgeye taşıma
Bu makalede, farklı bir bölgeye taşıdığınızda, bir sanal makinede (VM), oto yönetimi 'nin etkin tutulması açıklanmaktadır. Birçok nedenden dolayı sanal makinelerinizi başka bir bölgeye taşımak isteyebilirsiniz. Örneğin, yeni bir Azure bölgesinden yararlanarak iç ilke ve idare gereksinimlerini veya kapasite planlama gereksinimlerine yanıt olarak tanışın. Taşıdığınız bu VM 'Ler Şu anda yeniden yönetilebilir ve taşıma işleminden sonra, bu sanal makinelerin tekrar yönetilmesi gerekebilir.

## <a name="prerequisites"></a>Önkoşullar
* Hedef bölgenizin, [oto tarafından desteklendiğinden](./automanage-virtual-machines.md#prerequisites)emin olun.
* Log Analytics çalışma alanı bölgeniz, Otomasyon hesabı bölgeniz ve hedef bölgenizin [bölge eşlemeleri tarafından](https://docs.microsoft.com/azure/automation/how-to/region-mappings)desteklenen tüm bölgeler olduğundan emin olun.

## <a name="prepare-your-automanaged-vms-for-moving"></a>Zamanlanan sanal makinelerinizi taşımaya hazırlama
Oto tarafından yönetilen sanal makinelerinizdeki oto yönetimini devre dışı bırakın. Bunu, oto Yönet dikey penceresinde sanal makinelerinizi seçip, oto yönetimi dikey penceresinde, oto **yönetimini devre dışı bırak** ' a tıklayarak yapabilirsiniz.

## <a name="move-your-automanaged-vms-and-re-enable-automanage"></a>Oto tarafından yönetilen sanal makinelerinizi taşıyın ve tekrar Yönet 'i yeniden etkinleştirin
VM 'lerinizi taşıma hakkında daha fazla bilgi için bu [makaleye](https://docs.microsoft.com/azure/resource-mover/tutorial-move-region-virtual-machines)bakın.

VM 'lerinizi bölgeler arasında taşındıktan sonra, tekrar tekrar yönetmeyi yeniden etkinleştirebilirsiniz. Ayrıntılar [burada](./automanage-virtual-machines.md#enabling-automanage-for-vms-in-azure-portal)bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure oto yönetimi hakkında daha fazla bilgi edinin](./automanage-virtual-machines.md)
* [Azure oto yönetimi hakkında sık sorulan soruları görüntüleyin](./faq.md)