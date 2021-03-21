---
title: Azure Percept DK 'yi USB bağlantısı üzerinden güncelleştirme
description: Azure Percept DK 'yi USB bağlantısı üzerinden güncelleştirme hakkında bilgi edinin
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: 7f5e5e4da9fea671fc85a55fc8cc191fa14b720f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "101663853"
---
# <a name="how-to-update-azure-percept-dk-over-a-usb-connection"></a>Azure Percept DK 'yi USB bağlantısı üzerinden güncelleştirme

Azure Percept DK 'nin taşıyıcı panosuna yönelik bir USB güncelleştirmesi gerçekleştirmeyi öğrenmek için bu kılavuzu izleyin.

## <a name="prerequisites"></a>Önkoşullar
- Kullanılabilir bir USB-C veya USB-bir bağlantı noktası olan bilgisayar.
- Azure Percept DK (Dev Kit) taşıyıcı panosu ve USB-C ' y i USB-C kablosu ile sağlanan. Ana bilgisayarınızda USB-A bağlantı noktası varsa ancak USB-C bağlantı noktası yoksa, USB-C ' y i USB-A kablosu (ayrı olarak satılır) kullanabilirsiniz.
- [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) 'yi (yönetici erişimi gerekir) yükle.
- NXP UUU aracını yükler. [En son sürüm](https://github.com/NXPmicro/mfgtools/releases) uuu.exe dosyasını (Windows için) veya uuu dosyasını (Linux Için) varlıklar sekmesi altında indirin.
- [7-zip ' i yükler](https://www.7-zip.org/). Bu yazılım, Ham görüntü dosyasının XZ sıkıştırılmış dosyasından ayıklanması için kullanılacaktır. Uygun. exe dosyasını indirin ve yükleyin.

## <a name="steps"></a>Adımlar
1.  Aşağıdaki [üç USB güncelleştirme dosyasını](https://go.microsoft.com/fwlink/?linkid=2155734)indirin:
    - pe101-UEFI-***&lt; sürüm numarası &gt;***. RAW. XZ
    - emmc_full.txt
    - Fast-HAB-fw. RAW
 
1. _Sıkıştırılmış pe101-UEFI_**&lt; &gt;** * _&lt; sürüm numarası &gt;_* *. RAW. XZ dosyasından pe101-UEFI-* sürüm numarası. RAW konumuna ayıklayın. Nasıl ayıklanabildiğinizden emin değil misiniz? 7-zip indirin ve yükleyin, ardından **. XZ** görüntü dosyasına sağ tıklayın ve burada 7-Zip ayıkla ' yı seçin &gt; .

1. Aşağıdaki üç dosyayı UUU aracını içeren klasöre kopyalayın:
    - Ayıklanan pe101-UEFI-***&lt; sürüm numarası &gt;***. RAW dosyası (adım 2 ' den).
    - emmc_full.txt (1. adım).
    - Fast-HAB-fw. RAW (1. adım).
 
1. Geliştirme setinde güç.
1. [SSH üzerinden Dev Kit 'e bağlanma](./how-to-ssh-into-percept-dk.md)
1. Bir Windows komut istemi (Start &gt; cmd) veya Linux terminali açın ve güncelleştirme dosyalarının depolandığı klasöre gidin. Güncelleştirmeyi başlatmak için aşağıdaki komutu çalıştırın:
    - Windows: ```uuu -b emmc_full.txt fast-hab-fw.raw pe101-uefi-<version number>.raw```
    - Linux: ```sudo ./uuu -b emmc_full.txt fast-hab-fw.raw pe101-uefi-<version number>.raw```
    
Bu komutları çalıştırdıktan sonra, "cihaz bekleniyor..." iletisini bildiren bir ileti görebilirsiniz. komut isteminde. Bu beklenen bir sonraki adıma ilerlemeniz gerekir.
    
1. Geliştirme Seti taşıyıcısı panosunu, USB kablosu üzerinden ana bilgisayara bağlayın. Hangi bağlantı noktalarının kullanılabilir olduğuna bağlı olarak, her zaman taşıyıcı panolar USB-C bağlantı noktasından ana bilgisayarın USB-C veya USB-A bağlantı noktasına (USB-C ' y e, ayrı satılan bir kablo) bağlanın. 
 
1. SSH/PuTTY terminalinde, Dev Kit 'i USB moduna ayarlamak için aşağıdaki komutları girin ve ardından Dev Kit 'i yeniden başlatın.
    - ```flagutil    -wBfRequestUsbFlash    -v1```
    - ```reboot -f```
 
1. Ana bilgisayarın cihazı tanıdığını ve güncelleştirme işleminin otomatik olarak başlatılacağını belirten bir bildirim alabilirsiniz. Durumu görmek için komut istemine geri gidin. İşlem on dakikaya kadar sürer ve güncelleştirme başarılı olduğunda "başarı 1 hatası 0" belirten bir ileti görürsünüz
 
1. Güncelleştirme tamamlandıktan sonra, taşıyıcı panosunu kapatın. USB kablosunu BILGISAYARDAN çıkarın.  Azure Percept Vision modülünü USB kablosunu kullanarak taşıyıcı panosuna takın.

1. Taşıyıcı panonun yeniden gücünü yapın.

## <a name="next-steps"></a>Sonraki adımlar

Geliştirme Seti artık başarıyla güncelleştirildi. Devkit ile geliştirme ve işleme devam edebilirsiniz.