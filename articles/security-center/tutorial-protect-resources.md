---
title: Erişim & uygulama denetimleri öğreticisi-Azure Güvenlik Merkezi
description: Bu öğreticide, tam zamanında VM erişim ilkesinin ve uygulama denetim ilkesinin nasıl yapılandırılacağı gösterilir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/03/2018
ms.author: memildin
ms.openlocfilehash: 56dd74fba46aa8b79c94b1460996bb6edb1ff93f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90904598"
---
# <a name="tutorial-protect-your-resources-with-azure-security-center"></a>Öğretici: Kaynaklarınızı Azure Güvenlik Merkezi ile koruma
Güvenlik Merkezi, kötü amaçlı etkinliği engellemek için erişim ve uygulama denetimlerini kullanarak tehditlere maruz kalma riskinizi sınırlar. Tam zamanında (JıT) sanal makine (VM) erişimi, VM 'lere kalıcı erişimi reddetmenizi sağlayarak saldırılara maruz kalmayı azaltır. Bunun yerine, VM'ler için yalnızca gerektiğinde denetimli ve denetlenen erişim sağlamış olursunuz. Uyarlamalı uygulama denetimleri hangi uygulamaların VM'leriniz üzerinde çalışabileceğini denetleyerek kötü amaçlı yazılımlara karşı VM'lerin sağlamlaştırılmasına yardımcı olur. Güvenlik Merkezi, VM 'de çalışan işlemi çözümlemek için makine öğrenimini kullanır ve bu zekası kullanarak liste kurallarına izin ver kuralları uygulamanıza yardımcı olur.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Tam zamanında VM erişimi ilkesi yapılandırma
> * Uygulama denetim ilkesi yapılandırma

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticide ele alınan özellikler arasında ilerlemek için Azure Defender 'ın etkinleştirilmiş olması gerekir. Azure Defender 'ı ücretsiz olarak deneyebilirsiniz. Daha fazla bilgi için bkz. [Azure Defender 'ı deneyin](security-center-pricing.md).

## <a name="manage-vm-access"></a>VM erişimini yönetme
JıT VM erişimi, Azure VM 'lerinize gelen trafiği kilitlemek ve gerektiğinde VM 'lere bağlanmak için kolay erişim sağlarken saldırılara maruz kalma olasılığını azaltmak için kullanılabilir.

Yönetim bağlantı noktalarının her zaman açık olması gerekmez. Bunların yalnızca VM’ye bağlı olduğunuzda (örneğin, yönetim veya bakım görevleri gerçekleştirmek için) açık olması gerekir. Tam zamanında etkinleştirildiğinde Güvenlik Merkezi, ağ güvenlik grubu (NSG) kurallarını kullanır ve bu sayede, saldırganlar tarafından hedeflenememesi için yönetim bağlantı noktalarına erişimi kısıtlar.

[Tam zamanında erişimli yönetim bağlantı noktalarınızı güvenli hale getirme](security-center-just-in-time.md)kılavuzunu izleyin.

## <a name="harden-vms-against-malware"></a>VM’leri kötü amaçlı yazılımlara karşı sağlamlaştırma
Uyarlamalı uygulama denetimleri, diğer avantajlarının yanı sıra VM’lerinizin kötü amaçlı yazılımlara karşı sağlamlaştırılmasına yardımcı olan yapılandırılmış kaynak grupları üzerinde çalışmasına izin verilen uygulamalar tanımlamanıza yardımcı olur. Güvenlik Merkezi, VM 'de çalışan işlemi çözümlemek için makine öğrenimini kullanır ve bu zekası kullanarak liste kurallarına izin ver kuralları uygulamanıza yardımcı olur.

[Makinelerinizin saldırı yüzeylerini azaltmak için uyarlamalı uygulama denetimlerini kullanma](security-center-adaptive-application.md)bölümündeki yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, aşağıdaki işlemleri gerçekleştirerek, tehditlere maruz kalma riskinizi nasıl sınırlayabileceğinizi öğrendiniz:

> [!div class="checklist"]
> * Yalnızca gerektiğinde VM 'lere denetimli ve denetlenen erişim sağlamak için tam zamanında VM erişim ilkesi yapılandırma
> * VM’lerinizde hangi uygulamaların çalışabileceğini denetlemek için uyarlamalı uygulama denetimleri ilkesi yapılandırma

Güvenlik olaylarına yanıt verme hakkında bilgi edinmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Öğretici: Güvenlik olaylarına yanıt verme](tutorial-security-incident.md)

<!--Image references-->
[1]: ./media/tutorial-protect-resources/just-in-time-vm-access.png
[2]: ./media/tutorial-protect-resources/add-port.png
[3]: ./media/tutorial-protect-resources/adaptive-application-control-options.png
[4]: ./media/tutorial-protect-resources/recommended-resource-groups.png
