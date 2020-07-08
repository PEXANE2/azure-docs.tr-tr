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
ms.openlocfilehash: daefaca45adb061295928c64b6a0e328a12d8a3e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85269122"
---
# <a name="os-start-up--computer-restarted-unexpectedly-or-encountered-an-unexpected-error"></a>İşletim sistemi başlatma – bilgisayar beklenmedik şekilde yeniden başlatıldı veya beklenmeyen bir hatayla karşılaşıldı

Bu makalede, Windows yüklenirken sanal makinenin (VM) beklenmedik bir yeniden başlatma veya hata ile karşılaştığı sorunları çözmek için adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekran görüntüsünün Windows yüklemesinin başarısız olduğunu aşağıdaki hata ile görüntülediğini görürsünüz:

**Bilgisayar beklenmedik şekilde yeniden başlatıldı veya beklenmedik bir hatayla karşılaştı. Windows yüklemesi devam edemiyor. Windows 'u yüklemek için "Tamam" a tıklayarak bilgisayarı yeniden başlatın ve yüklemeyi yeniden başlatın.**

![Windows yüklemesi devam ederken hata: bilgisayar beklenmedik şekilde yeniden başlatıldı veya beklenmeyen bir hatayla karşılaştı. Windows yüklemesi devam edemiyor. Windows 'u yüklemek için "Tamam" a tıklayarak bilgisayarı yeniden başlatın ve yüklemeyi yeniden başlatın.](./media/unexpected-restart-error-during-vm-boot/1.png)
 
![Windows yükleme kurulumu hizmetleri başlatırken hata: bilgisayar beklenmedik şekilde yeniden başlatıldı veya beklenmeyen bir hatayla karşılaştı. Windows yüklemesi devam edemiyor. Windows 'u yüklemek için "Tamam" a tıklayarak bilgisayarı yeniden başlatın ve yüklemeyi yeniden başlatın.](./media/unexpected-restart-error-during-vm-boot/2.png)

## <a name="cause"></a>Nedeni

Makine [genelleştirilmiş bir görüntünün](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation)ilk önyüklemesini gerçekleştirmeye çalışıyor, ancak işlenmekte olan özel yanıt dosyası (unattend.xml) nedeniyle sorunlarla karşılaşıyor. Özel yanıt dosyaları Azure'da desteklenmez. 

Yanıt dosyası, bir Windows Server işletim sistemi yüklemesi sırasında otomatikleştirmek istediğiniz yapılandırma ayarları için tanımları ve değerleri içeren özel bir XML dosyasıdır. Yapılandırma seçenekleri, disklerin nasıl bölümleyeceğinize, yüklenecek Windows görüntüsünün nerede bulunacağını, uygulanacak ürün anahtarlarını ve çalıştırmak istediğiniz diğer komutları içerir.

Azure 'da özel yanıt dosyaları desteklenmez. Seçeneğini kullanarak özel bir **Unattend.xml** dosyası belirttiyseniz `sysprep /unattend:<your file’s name>` Bu hata oluşabilir.

Azure 'da, Unattend.xml dosyası yerine **Sysprep.exe**veya kullanın ' daki **sistem içi çalıştırma deneyimi (OOBE)** seçeneğini kullanın `sysprep /oobe` .

Bu sorun, en sık, genelleştirilmiş bir VM 'yi Azure 'a yüklemek için şirket içi VM ile **Sysprep.exe** kullanırken oluşturulur. Bu durumda, genelleştirilmiş bir VM 'yi düzgün bir şekilde karşıya yükleme hakkında da ilgileniyor olabilirsiniz.

## <a name="solution"></a>Çözüm

### <a name="replace-unattended-answer-file-option"></a>Katılımsız yanıt dosyası seçeneğini değiştir

Bu durum, bir görüntü Azure 'da kullanıma hazır olduğunda, ancak Azure 'da desteklenmeyen özel bir yanıt dosyası kullanmışsa ve **Sysprep** 'i aşağıdaki komuta benzer bir bayrağıyla kullandıysanız, bu durum oluşur:

`sysprep /oobe /generalize /unattend:<NameOfYourAnswerFile.XML> /shutdown`

- Önceki komutta, değerini `<NameOfYourAnswerFile.XML>` dosyanızın adıyla değiştirin.

Bu sorunu giderecek [bir görüntü hazırlama/yakalama](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed) ve yeni bir Genelleştirilmiş görüntü hazırlama hakkında Azure kılavuzunu izleyin. Sysprep sırasında `/unattend:<answerfile>` bayrak kullanmayın. Bunun yerine yalnızca aşağıdaki bayrakları kullanın:

`sysprep /oobe /generalize /shutdown`

- **Kullanıma hazır deneyim** (OOBE), Azure VM 'leri için desteklenen ayardır.

Aşağıda gösterilen seçenekleri belirleyerek, yukarıdaki komutla aynı görevi gerçekleştirmek için **Sistem Hazırlama Aracı GUI** 'sini de kullanabilirsiniz:

- Kullanıma hazır deneyim girin
- Genelleştir
- Kapat
 
![OOBE, generalize ve kapalı seçenekleri seçili olan Sistem Hazırlama aracı penceresi.](./media/unexpected-restart-error-during-vm-boot/3.png)
