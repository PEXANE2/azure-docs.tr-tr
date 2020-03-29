---
title: Azure Seri Konsol Güç Seçenekleri | Microsoft Dokümanlar
description: Azure Seri Konsolu içinde vm güç seçenekleri mevcuttur
services: virtual-machines
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 8/6/2019
ms.author: alsin
ms.openlocfilehash: 11c2549d7282bae5654ede1ac34d63a43cd0f059
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451193"
---
# <a name="power-options-available-from-the-azure-serial-console"></a>Azure Seri Konsolundan Güç Seçenekleri

Azure Seri Konsolu, VM veya sanal makine ölçeği setinizde güç yönetimi için birkaç güçlü araç sağlar. Bu güç yönetimi seçenekleri bazıları için kafa karıştırıcı olabilir, bu nedenle bu her aracın genel bakışı ve kullanım amacına yönelik tir.

Seri Konsol Özelliği | Açıklama | Kullanım Örneği
:----------------------|:------------|:---------
VM'yi yeniden başlatın | VM veya sanal makine ölçeği set örneğinizin zarif bir şekilde yeniden başlatılması. Bu işlem, Genel Bakış sayfasında bulunan yeniden başlatma özelliğini çağırmakla aynıdır. | Çoğu durumda, bu seçenek VM yeniden başlatmaya çalışırken ilk aracı olmalıdır. Seri Konsol bağlantınız kısa bir kesintiyle karşılaşır ve VM yeniden başlatılır başlamaz otomatik olarak devam edecektir.
VM Sıfırlama | Azure platformu tarafından ayarlanan VM veya sanal makine ölçeğinizin güçlü bir güç döngüsü. | Bu seçenek, işletim sisteminizi geçerli durumuna bakılmaksızın hemen yeniden başlatmak için kullanılır. Bu işlem zarif olmadığından, veri kaybı veya bozulma riski vardır. Seri Konsol bağlantısında herhangi bir kesinti yoktur ve bu komutları önyükleme süresinin erken saatlerinde göndermek için yararlı olabilir (örneğin, Windows VM'de Linux VM veya Güvenli Mod'da GRUB'a gitmek).
SysRq - Yeniden başlatma (b) | Bir konuk yeniden başlatmaya zorlamak için bir sistem isteği. | Bu özellik yalnızca Linux işletim sistemleri için geçerlidir ve [SysRq'un](./serial-console-nmi-sysrq.md#system-request-sysrq) işletim sisteminde etkin olmasını gerektirir. İşletim sistemi SysRq için düzgün şekilde yapılandırılırsa, bu komut işletim sisteminin yeniden başlatılmasına neden olur.
NMI (Maskesiz Kesme) | İşletim sistemine teslim edilecek bir kesme komutu | Bu işlem hem [Windows](./serial-console-windows.md#use-the-serial-console-for-nmi-calls) hem de [Linux](./serial-console-nmi-sysrq.md#non-maskable-interrupt-nmi) VM'leri için kullanılabilir ve NMI'nin etkinleştirilmesini gerektirir. NMI göndermek genellikle işletim sisteminizin çökmesine neden olur. İşletim sisteminizi bir döküm dosyası oluşturacak şekilde yapılandırabilir ve nmi aldıktan sonra yeniden başlatabilirsiniz, bu da düşük düzeyli hata ayıklamada yararlı olabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Linux VM'leri için Azure Seri Konsolu](./serial-console-linux.md) hakkında daha fazla bilgi edinin
* [Windows VM'ler için Azure Seri Konsolu](./serial-console-windows.md) hakkında daha fazla bilgi edinin