---
title: CloudSimple Eylül 2020 güncelleştirmesine göre Azure VMware çözümü
description: Bu makalede, bu bakım işlemi sırasında bekleneceğiniz ve özel buluttaki değişiklikler hakkında bilgi edinin.
author: dikamath
ms.author: dikamath
ms.date: 09/3/2020
ms.topic: article
ms.service: azure-vmware-cloudsimple
manager: dikamath
ms.openlocfilehash: ae9c1ba5259e95ed030d7099e5dafe2d4f7935b4
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89448027"
---
# <a name="azure-vmware-solution-by-cloudsimple-september-2020-update"></a>CloudSimple Eylül 2020 güncelleştirmesine göre Azure VMware çözümü

Eylül ayında Azure VMware çözüm hizmeti için önemli bir güncelleştirme gerçekleştirilecek. Bakım parçası olarak gönderilen bir e-posta bildirimi, bakımın zaman çizelgesini içerir. Bu makalede, bu bakım işlemi sırasında bekleneceğiniz ve özel buluttaki değişiklikler hakkında bilgi edineceksiniz.

> [!NOTE]
> Bu, kesintiye uğramayan bir yükseltmez. Yükseltme sırasında, yedekli bileşenlerden birini görebilirsiniz.

## <a name="vmware-infrastructure-upgrade"></a>VMware altyapı yükseltmesi

Özel bulutunuzun VMware altyapısı, daha yeni bir sürüme güncelleştirilecektir. Bu, özel bulutunuzun vCenter, ESXi, NSX-T ve karma bulut uzantısı (HCX) bileşenleri (dağıtılmışsa) için güncelleştirmeler içerir.

Yükseltme sırasında, bir düğüm yükseltme işlemi için bakım moduna alınmadan önce özel buluta yeni bir düğüm eklenecektir. Bu, yükseltme işlemi sırasında özel bulutunuzun ve özel bulutunuzun kullanım kapasitesinin korunmasını sağlar. VMware bileşenlerinin yükseltilmesi sırasında vCenter Web Kullanıcı arabiriminizdeki uyarıları görebilirsiniz. Alarmlar, hizmet işlemleri ekibi tarafından gerçekleştirilen bakım işlemlerinin bir parçasıdır.

**Bileşen sürümü**

- ESXi 6.7 U3
- vCenter 6.7 U3
- vSAN 6,7
- NSX veri merkezi 2.5.1
- HCX 3.5.2

## <a name="datacenter-updates"></a>Veri merkezi güncelleştirmeleri

Bu güncelleştirme, veri merkezi altyapısına yönelik güncelleştirmeleri içerir. Bakım süresince kesintiye uğramayan güncelleştirmeler gerçekleştirilecek. Güncelleştirme işlemi sırasında yedekliliği azaltılan fark edeceksiniz. Özel bulut VMware altyapınızda, ExpressRoute bağlantı hatlarınızda, GlobalReach bağlantılarında ve bağlantı kullanılabilirliği ile ilgili tüm siteden siteye VPN cihazlarında uyarılar oluşturulabilir. Bu, güncelleştirmenin bir parçası olarak bileşenler yeniden başlatıldığı için güncelleştirme sırasında normaldir.

-   Siteden siteye VPN tek bir örnek (HA dışı) olarak dağıtılırsa VPN bağlantısını yeniden oluşturmanız gerekebilir.

-   Noktadan siteye VPN bağlantısı kullanıyorsanız VPN bağlantısını yeniden kurmanız gerekir.

## <a name="post-update"></a>Güncelleştirme sonrası

Güncelleştirmeler tamamlandıktan sonra, VMware bileşenlerinin daha yeni sürümlerini görmeniz gerekir. Herhangi bir sorunla karşılaşırsanız veya herhangi bir sorunuz olursa [destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)açarak destek ekibimize başvurun.
