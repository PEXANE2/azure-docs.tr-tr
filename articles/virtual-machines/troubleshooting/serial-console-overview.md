---
title: Azure seri konsol | Microsoft Docs
description: Azure seri konsolu, SSH veya RDP kullanılabilir olmadığında sanal makinenize bağlanmanızı sağlar.
services: virtual-machines
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 02/10/2020
ms.author: alsin
ms.openlocfilehash: 779bb88d15ea6c52f4399f17223b89916e22653d
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78381700"
---
# <a name="azure-serial-console"></a>Azure seri konsol

Azure portal seri konsol, sanal makineler (VM) ve Linux ya da Windows çalıştıran sanal makine ölçek kümesi örnekleri için metin tabanlı bir konsola erişim sağlar. Bu seri bağlantı, ağ veya işletim sistemi durumundan bağımsız olarak erişim sağlayan VM veya sanal makine ölçek kümesi örneğinin ttyS0 veya COM1 seri bağlantı noktasına bağlanır. Seri konsoluna yalnızca Azure portal kullanılarak erişilebilir ve yalnızca VM veya sanal makine ölçek kümesine katkıda bulunan veya daha yüksek bir erişim rolüne sahip olan kullanıcılar için izin verilir.

Seri konsol, VM 'Ler ve sanal makine ölçek kümesi örnekleri için aynı şekilde çalışmaktadır. Bu belgede, aksi belirtilmediği takdirde VM 'lerdeki tüm bahsetmeler, sanal makine ölçek kümesi örneklerini örtülü olarak içerir.

> [!NOTE]
> Seri konsol Genel Azure bölgelerinde ve Azure Kamu 'da genel önizlemede kullanılabilir. Henüz Azure Çin bulutu 'nda mevcut değildir.

## <a name="prerequisites-to-access-the-azure-serial-console"></a>Azure seri konsoluna erişim önkoşulları
VM veya sanal makine ölçek kümesi örneğindeki seri konsoluna erişmek için şunlar gerekir:

- VM için önyükleme tanılamaları etkinleştirilmelidir
- Parola kimlik doğrulaması kullanan bir kullanıcı hesabının VM içinde mevcut olması gerekir. VM erişimi uzantısının [parola sıfırlama](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) işlevini kullanarak parola tabanlı bir kullanıcı oluşturabilirsiniz. **Destek + sorun giderme** bölümünde **Parolayı Sıfırla** ' yı seçin.
- Seri konsoluna erişen Azure hesabının hem VM hem de [önyükleme tanılama](boot-diagnostics.md) depolama hesabı Için [sanal makine katılımcısı rolü](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) olmalıdır

> [!NOTE]
> Klasik dağıtımlar desteklenmez. VM 'niz veya sanal makine ölçek kümesi örneğinizin Azure Resource Manager dağıtım modelini kullanması gerekir.

## <a name="get-started-with-the-serial-console"></a>Seri konsol ile çalışmaya başlama
VM 'Lerin ve sanal makine ölçek kümesinin seri konsoluna yalnızca Azure portal aracılığıyla erişilebilir:

### <a name="serial-console-for-virtual-machines"></a>Sanal makineler için seri konsol
VM 'Ler için seri konsol, Azure portal **destek + sorun giderme** bölümündeki **seri konsol** tıklamakla açıktır.
  1. [Azure portalını](https://portal.azure.com) açın.

  1. **Tüm kaynaklara** gidin ve bir sanal makine seçin. VM 'nin genel bakış sayfası açılır.

  1. **Destek + sorun giderme** bölümüne gidin ve **seri konsol**' yi seçin. Seri konsolu ile yeni bir bölme açılır ve bağlantısını başlatır.

     ![Linux seri konsol penceresi](./media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)

### <a name="serial-console-for-virtual-machine-scale-sets"></a>Sanal Makine Ölçek Kümeleri için seri konsol
Seri konsol, ölçek kümesi içindeki her bir örnek üzerinde erişilebilir olan sanal makine ölçek kümeleri için kullanılabilir. **Seri konsol** düğmesini görmeden önce bir sanal makine ölçek kümesinin tek örneğine gitmeniz gerekir. Sanal makine ölçek kümesinde önyükleme tanılaması etkinleştirilmemişse, sanal makine ölçek kümesi modelinizi önyükleme tanılamayı etkinleştirecek şekilde güncelleştirdiğinizden emin olun ve ardından seri konsoluna erişmek için tüm örnekleri yeni modele yükseltin.
  1. [Azure portalını](https://portal.azure.com) açın.

  1. **Tüm kaynaklara** gidin ve bir sanal makine ölçek kümesi seçin. Sanal makine ölçek kümesi için genel bakış sayfası açılır.

  1. **Örneklere** git

  1. Bir sanal makine ölçek kümesi örneği seçin

  1. **Destek + sorun giderme** bölümünde **seri konsol**' yi seçin. Seri konsolu ile yeni bir bölme açılır ve bağlantısını başlatır.

     ![Linux sanal makine ölçek kümesi seri konsolu](./media/virtual-machines-serial-console/vmss-start-console.gif)

## <a name="advanced-uses-for-serial-console"></a>Seri konsol için gelişmiş kullanımlar
Sanal makinenize konsol erişiminin yanı sıra aşağıdakiler için de Azure seri konsolu 'nu kullanabilirsiniz:
* [Sanal makinenize bir sistem isteği komutu](./serial-console-nmi-sysrq.md) gönderme
* [Sanal makinenize maskelenemeyen bir kesme](./serial-console-nmi-sysrq.md) gönderiliyor
* [VM 'nizi düzgün bir şekilde yeniden başlatma veya zorlayarak güç dönüşümü](./serial-console-power-options.md)


## <a name="next-steps"></a>Sonraki adımlar
Ek seri konsol belgeleri kenar çubuğunda kullanılabilir.
- [Linux VM 'leri Için seri konsol](./serial-console-linux.md)için daha fazla bilgi mevcuttur.
- [Windows VM 'lerine yönelik seri konsol](./serial-console-windows.md)için daha fazla bilgi bulunmaktadır.
