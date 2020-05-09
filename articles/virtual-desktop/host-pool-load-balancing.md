---
title: Windows sanal masaüstü konak havuzu yük dengelemesi-Azure
description: Windows sanal masaüstü ortamı için konak havuzu Yük Dengeleme yöntemleri.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 15d50033a1316601dd8c36bd5748c659f4397d66
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612053"
---
# <a name="host-pool-load-balancing-methods"></a>Ana bilgisayar havuzu yük dengeleme yöntemleri

>[!IMPORTANT]
>Bu içerik, Azure Resource Manager Windows sanal masaüstü nesneleriyle Spring 2020 güncelleştirmesine yöneliktir. Windows sanal masaüstü Fall 2019 sürümünü Azure Resource Manager nesneleri olmadan kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/host-pool-load-balancing-2019.md)bakın.
>
> Windows sanal masaüstü Spring 2020 güncelleştirmesi şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. 
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Windows sanal masaüstü iki yük dengeleme yöntemini destekler. Her yöntem, bir konak havuzundaki bir kaynağa bağlandıklarında bir kullanıcının oturumunu hangi oturum konağının barındıracağını belirler.

Aşağıdaki Yük Dengeleme yöntemleri Windows sanal masaüstü 'nde kullanılabilir:

- Enine ilk yük dengeleme, kullanıcı oturumlarını bir konak havuzundaki oturum ana bilgisayarları arasında eşit olarak dağıtmanızı sağlar.
- Derinlik-ilk yük dengeleme, bir konak havuzundaki Kullanıcı oturumlarıyla bir oturum konağını doygunluğu sağlar. İlk oturum, oturum sınırı eşiğine ulaştığında, yük dengeleyici, Yeni Kullanıcı bağlantılarını, sınırına ulaşana kadar ana bilgisayar havuzundaki bir sonraki oturum ana bilgisayarına yönlendirir ve bu şekilde devam eder.

Her konak havuzu, kendisine özgü bir tür yük dengelemeyi yalnızca bir tane yapılandırabilir. Ancak, her iki yük dengeleme yöntemi, hangi konak havuzunun nerede bulundukları, her ikisi de aşağıdaki davranışları paylaşır:

- Bir kullanıcı zaten konak havuzunda oturum varsa ve bu oturuma yeniden bağlanmışsa, yük dengeleyici bunları mevcut oturumlarıyla birlikte oturum ana bilgisayarına başarıyla yönlendirdirecektir. Bu davranış, oturum konağının AllowNewConnections özelliği false olarak ayarlanmış olsa bile geçerlidir.
- Bir kullanıcının konak havuzunda oturumu yoksa, yük dengeleyici, yük dengeleme sırasında AllowNewConnections özelliği false olarak ayarlanmış oturum konaklarına göz önünde bulundurmaz.

## <a name="breadth-first-load-balancing-method"></a>Enine-ilk yük dengeleme yöntemi

Enine ilk yük dengeleme yöntemi, bu senaryoya yönelik iyileştirmek için Kullanıcı bağlantılarını dağıtmanıza olanak tanır. Bu yöntem, havuza alınmış sanal masaüstü ortamlarına bağlanan kullanıcılar için en iyi deneyimi sağlamak isteyen kuruluşlar için idealdir.

Enine ilk yöntem, yeni bağlantılara izin veren oturum ana bilgisayarlarını ilk kez sorgular. Daha sonra yöntemi, en az oturum sayısı olan oturum konağını seçer. Bir bağ varsa, yöntem sorgudaki ilk oturum konağını seçer.

## <a name="depth-first-load-balancing-method"></a>Derinlik-ilk yük dengeleme yöntemi

Derinlik-ilk yük dengeleme yöntemi, bu senaryoyu iyileştirmek için tek seferde bir oturum ana bilgisayarının doygunluğunu görmenizi sağlar. Bu yöntem, bir konak havuzu için ayırdıkları sanal makine sayısı üzerinde daha ayrıntılı denetim yapmak isteyen maliyet açısından bilinçli kuruluşlar için idealdir.

İlk olarak kısıtlama yöntemi, yeni bağlantılara izin veren ve en fazla oturum sınırının üzerinde olmayan oturum konaklarını sorgular. Daha sonra yöntemi, en fazla oturum sayısı olan oturum konağını seçer. Bir bağ varsa, yöntem sorgudaki ilk oturum konağını seçer.