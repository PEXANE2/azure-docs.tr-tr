---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/05/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 293f0f459e1f1e464fdec16b76eaf08336c92e93
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91376670"
---
- Bir disk erişimi nesnesine yalnızca bir sanal ağ bağlanabilir.
- Sanal ağınızın bağlamak için disk erişim nesneniz ile aynı abonelikte olması gerekir.
- Aynı disk erişimi nesnesiyle 10 ' a kadar disk veya anlık görüntü içeri aktarılabilir veya aynı anda aktarılabilir.
- Bir sanal ağı bir disk erişim nesnesine bağlamak için el ile onay isteyemezsiniz.
- Artımlı anlık görüntüler, bir disk erişim nesnesiyle ilişkilendirildiğinde verilemez.
- Bir depolama hesabına özel bağlantılar aracılığıyla güvenliği sağlanmış bir disk/anlık görüntünün VHD 'sini indirmek için AzCopy kullanamazsınız. Ancak, sanal makinelerinize VHD 'yi indirmek için AzCopy kullanabilirsiniz.
