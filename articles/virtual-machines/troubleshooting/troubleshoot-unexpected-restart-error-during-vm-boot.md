---
title: İşletim sistemi başlatma – bilgisayar beklenmedik şekilde yeniden başlatıldı veya beklenmeyen bir hatayla karşılaşıldı
description: Bu makalede, Windows yüklenirken VM 'nin beklenmeyen bir yeniden başlatma veya hata ile karşılaştığı sorunları çözmek için adımlar sağlanmaktadır.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 1d249a4e-71ba-475d-8461-31ff13e57811
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 06/22/2020
ms.author: v-mibufo
ms.openlocfilehash: d8d2ab2bb3f24e1faa4791ebdc1ce3852f6a790e
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632699"
---
# <a name="os-start-up--computer-restarted-unexpectedly-or-encountered-an-unexpected-error"></a>İşletim sistemi başlatma – bilgisayar beklenmedik şekilde yeniden başlatıldı veya beklenmeyen bir hatayla karşılaşıldı

Bu makalede, Windows yüklenirken sanal makinenin (VM) beklenmedik bir yeniden başlatma veya hata ile karşılaştığı sorunları çözmek için adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](./boot-diagnostics.md) kullandığınızda, ekran görüntüsünün Windows yüklemesinin başarısız olduğunu aşağıdaki hata ile görüntülediğini görürsünüz:

**Bilgisayar beklenmedik şekilde yeniden başlatıldı veya beklenmedik bir hatayla karşılaştı. Windows yüklemesi devam edemiyor. Windows 'u yüklemek için "Tamam" a tıklayarak bilgisayarı yeniden başlatın ve yüklemeyi yeniden başlatın.**

![Windows yüklemesi devam ederken hata: bilgisayar beklenmedik şekilde yeniden başlatıldı veya beklenmeyen bir hatayla karşılaştı. Windows yüklemesi devam edemiyor. Windows 'u yüklemek için "Tamam" a tıklayarak bilgisayarı yeniden başlatın ve yüklemeyi yeniden başlatın.](./media/unexpected-restart-error-during-vm-boot/1.png)
 
![Windows yükleme kurulumu hizmetleri başlatırken hata: bilgisayar beklenmedik şekilde yeniden başlatıldı veya beklenmeyen bir hatayla karşılaştı. Windows yüklemesi devam edemiyor. Windows 'u yüklemek için "Tamam" a tıklayarak bilgisayarı yeniden başlatın ve yüklemeyi yeniden başlatın.](./media/unexpected-restart-error-during-vm-boot/2.png)

## <a name="cause"></a>Nedeni

Makine [genelleştirilmiş bir görüntünün](/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation)ilk önyüklemesini gerçekleştirmeye çalışıyor, ancak işlenmekte olan özel yanıt dosyası (Unattend.xml) nedeniyle sorunlarla karşılaşıyor. **Özel yanıt dosyaları Azure 'da desteklenmez**. 

Yanıt dosyası, bir Windows Server işletim sistemi yüklemesi sırasında otomatikleştirmek istediğiniz yapılandırma ayarları için tanımları ve değerleri içeren özel bir XML dosyasıdır. Yapılandırma seçenekleri, disklerin nasıl bölümleyeceğinize, yüklenecek Windows görüntüsünün nerede bulunacağını, uygulanacak ürün anahtarlarını ve çalıştırmak istediğiniz diğer komutları içerir.

Bu durumda, özel yanıt dosyaları Azure 'da desteklenmez. Bu nedenle, bir görüntü Azure 'da kullanıma hazır olduğunda bu durum oluşur, ancak **Sysprep** kullanarak aşağıdaki komuta benzer bir bayrağıyla özel bir Unattend.xml dosyası belirttiniz:

`sysprep /oobe /generalize /unattend:<your file’s name> /shutdown`

Azure 'da, **sistem hazırlığı Aracı GUI**'sindeki **sistem çalıştırma deneyimi (OOBE) seçeneğini girin** veya `sysprep /oobe` Unattend.xml dosyası yerine kullanın.

Bu sorun, en sık, genelleştirilmiş bir VM 'yi Azure 'a yüklemek için bir şirket içi VM ile Sysprep kullanırken oluşturulmuştur. Bu durumda, genelleştirilmiş bir VM 'yi düzgün bir şekilde karşıya yükleme hakkında da ilgileniyor olabilirsiniz.

## <a name="solution"></a>Çözüm

### <a name="do-not-use-unattendxml"></a>Unattend.xml kullanma

> [!TIP]
> VM 'nin son yedeğine sahipseniz önyükleme sorununu çözmek için [VM 'yi yedekten geri yüklemeyi](../../backup/backup-azure-arm-restore-vms.md) deneyebilirsiniz.

Bu sorunu giderecek [bir görüntü hazırlama/yakalama](../windows/upload-generalized-managed.md) ve yeni bir Genelleştirilmiş görüntü hazırlama hakkında Azure kılavuzunu izleyin. Sysprep sırasında **`/unattend:<your file’s name>` bayrak kullanmayın**. Bunun yerine yalnızca aşağıdaki bayrakları kullanın:

`sysprep /oobe /generalize /shutdown`

- Kullanıma hazır deneyim (OOBE), Azure VM 'Leri için desteklenen ayardır.

Aşağıda gösterilen seçenekleri belirleyerek, yukarıdaki komutla aynı görevi gerçekleştirmek için **Sistem Hazırlama Aracı GUI** 'sini de kullanabilirsiniz:

- Kullanıma hazır deneyim girin
- Genelleştir
- Kapat
 
![OOBE, generalize ve kapalı seçenekleri seçili olan Sistem Hazırlama aracı penceresi.](./media/unexpected-restart-error-during-vm-boot/3.png)
