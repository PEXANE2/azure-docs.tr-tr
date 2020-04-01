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
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420969"
---
Bu makalede, Yerel makinenizden Azure yönetilen bir diske vhd yükleme veya azcopy kullanarak yönetilen bir diski başka bir bölgeye kopyalama küçülme şekli açıklanmaktadır. Bu işlem, doğrudan yükleme, aynı zamanda doğrudan yönetilen bir diske boyutu 32 TiB kadar bir VHD yüklemenize olanak sağlar. Şu anda, doğrudan yükleme standart HDD, standart SSD ve premium SSD yönetilen diskler için desteklenir. Henüz ultra diskler için desteklenmez.

Azure'daki IaaS VM'ler için bir yedekleme çözümü sağlıyorsanız, müşteri yedeklemelerini yönetilen disklere geri yüklemek için doğrudan yükleme kullanmanızı öneririz. Bir kaynaktan Azure'a bir VHD yüklerken, hızlar yerel bant genişliğinize bağlıdır. Bir Azure VM'den yükleme veya kopyalama yaparken, bant genişliğiniz standart HDD'lerle aynı olacaktır.