---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/27/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e96d205ef1a8f94baa3a0cfe6c5127b6cf570e5a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80420969"
---
Bu makalede, bir VHD 'YI yerel makinenizden Azure yönetilen diskine yükleme veya AzCopy kullanarak yönetilen bir diski başka bir bölgeye kopyalama işlemleri açıklanmaktadır. Bu işlem, doğrudan karşıya yükleme, bir VHD 'yi doğrudan yönetilen diske boyutundaki 32 TiB 'ye kadar karşıya yüklemenizi de sağlar. Şu anda, standart HDD, standart SSD ve Premium SSD tarafından yönetilen diskler için doğrudan karşıya yükleme desteklenir. Henüz Ultra diskler için desteklenmez.

Azure 'da IaaS sanal makineleri için bir yedekleme çözümü sağlıyorsanız, müşteri yedeklemelerini yönetilen disklere geri yüklemek için doğrudan karşıya yüklemeyi kullanmanızı öneririz. Bir VHD 'den Azure 'a bir VHD 'yi karşıya yüklerken, hızlarınızın yerel bant genişliğine bağlı olması gerekir. Bir Azure VM 'sinden karşıya yükleme veya kopyalama yaparken, bant genişliğiniz standart HDD 'Ler ile aynı olacaktır.