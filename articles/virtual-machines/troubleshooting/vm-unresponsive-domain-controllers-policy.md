---
title: Varsayılan etki alanı denetleyicisi ilkesi uygulanırken VM yanıt vermiyor
titlesuffix: Azure Virtual Machines
description: Bu makalede, varsayılan etki alanı denetleyicileri Ilkesinin bir Azure VM 'nin önyüklemesini önlediği sorunları gidermeye yönelik adımlar sağlanmaktadır.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 34e6b765-3496-46a1-b7d7-6def00884394
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/08/2020
ms.author: v-miegge
ms.openlocfilehash: 4f181a6dee7ed182150bd0cad2b51690b6c77d7a
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91963134"
---
# <a name="vm-is-unresponsive-while-applying-default-domain-controllers-policy"></a>Varsayılan etki alanı denetleyicisi ilkesi uygulanırken VM yanıt vermiyor

Bu makalede, varsayılan etki alanı denetleyicileri Ilkesinin bir Azure sanal makinesinin (VM) önyüklemesini önlediği sorunları gidermeye yönelik adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](./boot-diagnostics.md) kullandığınızda, ekran görüntüsünün **varsayılan etki alanı denetleyicileri ilkesiyle**önyükleme sırasında işletim sisteminin yanıt vermemeye karşı bir şekilde görüntülediğini görürsünüz.

  ![Şekil 1 ' varsayılan etki alanı denetleyicileri Ilkesi ' iletisiyle takılmış işletim sistemini görüntüler](./media/vm-unresponsive-domain-controllers-policy/1-default-domain-controllers-policy.png)

## <a name="cause"></a>Nedeni

Bu sorun, varsayılan etki alanı denetleyicileri Ilkesinde yapılan son değişikliklerden kaynaklanıyor olabilir. Aksi takdirde, kök nedenin belirlenmesi için bir bellek dökümü dosya analizinin gerçekleştirilmesi gerekir.

## <a name="solution"></a>Çözüm

En son olarak varsayılan etki alanı denetleyicileri Ilkesinde değişiklikler yaptıysanız, sorunu giderecek değişiklikleri geri almak isteyebilirsiniz. Soruna neyin neden olduğunu bilmiyorsanız, bir bellek dökümü toplayın ve ardından bir destek bileti gönderin.

### <a name="collect-the-memory-dump-file"></a>Bellek dökümü dosyasını toplayın

Bu sorunu çözmek için öncelikle kilitlenme için bellek dökümü dosyasını toplamalı ve ardından bellek dökümü dosyası ile desteğe başvurmanız gerekir. Döküm dosyasını toplamak için aşağıdaki adımları izleyin:

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>İşletim sistemi diskini yeni bir onarım VM 'sine iliştirme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) 1-3 adımlarını kullanın.

1. Uzak Masaüstü Bağlantısı kullanarak, onarım VM 'ye bağlanın.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Döküm dosyasını bulma ve destek bileti gönderme

1. VM 'yi Onar sayfasında, bağlı işletim sistemi diskinde Windows klasörüne gidin. Eklenen işletim sistemi diskine atanmış olan sürücü harfi `F` ise, `F:\Windows` konumuna gitmelisiniz.

1. Memory. dmp dosyasını bulun ve ardından bellek dökümü dosyası ile [bir destek bileti gönderebilirsiniz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

1. Memory. dmp dosyasını bulmada sorun yaşıyorsanız, bunun yerine, [seri konsolundaki maskelenemeyen kesme (NMI) çağrılarını](./serial-console-windows.md#use-the-serial-console-for-nmi-calls) kullanmak isteyebilirsiniz. [NMI çağrılarını kullanarak kilitlenme bilgi döküm dosyası oluşturma](/windows/client-management/generate-kernel-or-complete-crash-dump)kılavuzunu izleyin.