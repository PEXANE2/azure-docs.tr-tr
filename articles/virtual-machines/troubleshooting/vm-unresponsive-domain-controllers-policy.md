---
title: Varsayılan etki alanı denetleyicileri ilkesi uygulanırken VM yanıt vermiyor
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
ms.openlocfilehash: 53e1daca47a2917a19cbc30db5348e4fcc06b325
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90039156"
---
# <a name="vm-is-unresponsive-while-applying-default-domain-controllers-policy"></a>Varsayılan etki alanı denetleyicileri ilkesi uygulanırken VM yanıt vermiyor

Bu makalede, varsayılan etki alanı denetleyicileri Ilkesinin bir Azure sanal makinesinin (VM) önyüklemesini önlediği sorunları gidermeye yönelik adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekran görüntüsünün **varsayılan etki alanı denetleyicileri ilkesiyle**önyükleme sırasında işletim sisteminin yanıt vermemeye karşı bir şekilde görüntülediğini görürsünüz.

  ![Şekil 1 ' varsayılan etki alanı denetleyicileri Ilkesi ' iletisiyle takılmış işletim sistemini görüntüler](./media/vm-unresponsive-domain-controllers-policy/1-default-domain-controllers-policy.png)

## <a name="cause"></a>Nedeni

Bu sorun, varsayılan etki alanı denetleyicileri Ilkesinde yapılan son değişikliklerden kaynaklanıyor olabilir. Aksi takdirde, kök nedenin belirlenmesi için bir bellek dökümü dosya analizinin gerçekleştirilmesi gerekir.

## <a name="solution"></a>Çözüm

En son olarak varsayılan etki alanı denetleyicileri Ilkesinde değişiklikler yaptıysanız, sorunu giderecek değişiklikleri geri almak isteyebilirsiniz. Soruna neyin neden olduğunu bilmiyorsanız, bir bellek dökümü toplayın ve ardından bir destek bileti gönderin.

### <a name="collect-the-memory-dump-file"></a>Bellek dökümü dosyasını toplayın

Bu sorunu çözmek için öncelikle kilitlenme için bellek dökümü dosyasını toplamalı ve ardından bellek dökümü dosyası ile desteğe başvurmanız gerekir. Döküm dosyasını toplamak için aşağıdaki adımları izleyin:

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>İşletim sistemi diskini yeni bir onarım VM 'sine iliştirme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) 1-3 adımlarını kullanın.

1. Uzak Masaüstü Bağlantısı kullanarak, onarım VM 'ye bağlanın.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Döküm dosyasını bulma ve destek bileti gönderme

1. VM 'yi Onar sayfasında, bağlı işletim sistemi diskinde Windows klasörüne gidin. Eklenen işletim sistemi diskine atanmış olan sürücü harfi `F` ise, `F:\Windows` konumuna gitmelisiniz.

1. Memory. dmp dosyasını bulun ve ardından bellek dökümü dosyası ile [bir destek bileti gönderebilirsiniz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

1. Memory. dmp dosyasını bulmada sorun yaşıyorsanız, bunun yerine, [seri konsolundaki maskelenemeyen kesme (NMI) çağrılarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows#use-the-serial-console-for-nmi-calls) kullanmak isteyebilirsiniz. [NMI çağrılarını kullanarak kilitlenme bilgi döküm dosyası oluşturma](https://docs.microsoft.com/windows/client-management/generate-kernel-or-complete-crash-dump)kılavuzunu izleyin.
