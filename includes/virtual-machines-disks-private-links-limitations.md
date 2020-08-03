---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/05/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 138e7efd33ee36a77fd56deda155a29017432302
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87420303"
---
- Bir disk erişimi nesnesine yalnızca bir sanal ağ bağlanabilir.
- Sanal ağınızın bağlamak için disk erişim nesneniz ile aynı abonelikte olması gerekir.
- Aynı disk erişimi nesnesiyle 10 ' a kadar disk veya anlık görüntü içeri aktarılabilir veya aynı anda aktarılabilir.
- Bir sanal ağı bir disk erişim nesnesine bağlamak için el ile onay isteyemezsiniz.
- Bir disk erişim nesnesiyle ilişkili artımlı anlık görüntüler için fark özelliği desteklenmez.
- Bir depolama hesabına özel bağlantılar aracılığıyla güvenliği sağlanmış bir disk/anlık görüntünün VHD 'sini indirmek için AzCopy kullanamazsınız. Ancak, sanal makinelerinize VHD 'yi indirmek için AzCopy kullanabilirsiniz.
