---
title: StorSimple 8000 serisi cihazda disk sürücüsüdeğiştirme | Microsoft Dokümanlar
description: StorSimple birincil kasaveya EBOD muhafazası üzerindeki disk sürücüsünasıl değiştirilmeye başverir.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 8/25/2017
ms.author: alkohli
ms.openlocfilehash: 3d6ef22e4df36996d68194589f43ea0f57def22c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267917"
---
# <a name="replace-a-disk-drive-on-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınızın disk sürücüsünü değiştirme

## <a name="overview"></a>Genel Bakış
Bu öğretici, Microsoft Azure StorSimple aygıtındaki arızalı veya arızalı bir sabit disk sürücüsümü nasıl kaldırabileceğinizi ve değiştirebileceğinizi açıklar. Bir disk sürücüsüni değiştirmek için şunları yapmanız gerekir:

* Antitamper kilidi devre dışı
* Disk sürücüsünün kaldırma
* Yedek disk sürücüsüne yükleme

> [!IMPORTANT]
> Bir disk sürücüsünün sökülüp değiştirilmesinden önce [StorSimple donanım bileşeni değiştirmede](storsimple-8000-hardware-component-replacement.md)güvenlik bilgilerini gözden geçirin.
 

## <a name="disengage-the-antitamper-lock"></a>Antitamper kilidi devre dışı
Bu yordam, disk sürücülerini değiştirdiğinizde StorSimple aygıtınızdaki antitamper kilitlerin nasıl devre dışı kalınabileceğini veya devre dışı kalınabileceğini açıklar. Antitamper kilitleri sürücü taşıyıcı kolları na takılır ve kolu mandal bölümünde küçük bir diyafram ile erişilir. Sürücüler kilitli konuma ayarlanmış kilitlerle birlikte verilir.

#### <a name="to-unlock-the-antitamper-lock"></a>Antitamper kilidi kilidini açmak için
1. Kilit anahtarını (Microsoft'un sağladığı "kurcalamaya dayanıklı" T10 tornavida) saptaki diyafram açıklığına ve soketine dikkatlice takın. 
   
   Antitamper kilidi etkinleştirilirse, kırmızı gösterge diyafram görünür.
  
    ![Kilitli disk sürücüsü](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    **Şekil 1** Anti-kurcaper kilidi devreye girdi
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |Gösterge diyafram açıklığı |
   | 2 |Antitamper kilidi |
2. Kırmızı gösterge anahtarın üzerindeki diyafram açıklığında görünmeyene kadar anahtarı saat yönünün tersine döndürün.
3. Anahtarı çıkarın.
   
    ![Kilidi açılan disk sürücüsü](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    **Şekil 2** Kilidi açılan disk sürücüsü
4. Disk sürücüsü artık kaldırılabilir.

Kilidi devreye sokmak için adımları ters ten uygulayın.

## <a name="remove-the-disk-drive"></a>Disk sürücüsünün kaldırma
StorSimple aygıtınız RAID 10 benzeri depolama alanları yapılandırmasını destekler. Bu, bir başarısız disk, katı hal sürücüsü (SSD) veya sabit disk sürücüsü (HDD) ile normal olarak çalışabileceği anlamına gelir.

> [!IMPORTANT]
> * Sisteminizde birden fazla başarısız disk varsa, herhangi bir zamanda birden fazla SSD veya HDD'yi sistemden çıkarmayın. Bunu yapmak veri kaybına neden olabilir.
> * Daha önce SSD içeren bir yuvaya yeni bir SSD yerleştirdiğinizden emin olun. Benzer şekilde, daha önce hdd içeren bir yuvaya yedek bir HDD yerleştirin.
> * Azure portalında yuvalar 0 - 11 arasında numaralandırılır. Bu nedenle, portal yuva 2'deki bir diskin başarısız olduğunu gösterirse, aygıtta başarısız olan diski sol üstten üçüncü yuvada arayın.
> 
> 

Sistem çalışırken sürücüler kaldırılabilir ve değiştirilebilir.

#### <a name="to-remove-a-drive"></a>Sürücüyü kaldırmak için
1. Azure portalında başarısız diski tanımlamak için cihazınızın **Ayarlar > Donanım health'e**gidin. Bir disk birincil kasada ve/veya EBOD kasasında (8600 model kullanıyorsanız) başarısız olabileceğinden, **Paylaşılan bileşenlerin** altındaki ve **EBOD paylaşılan bileşenlerin**altındaki disklerin durumuna bakın. Her iki muhafazadaki başarısız bir disk kırmızı bir durumla gösterilir.
2. Sürücüleri birincil kasanın veya EBOD kasasının önündeki yerini bulun. 
3. Diskin kilidi açıldıysa, bir sonraki adıma geçin. Disk kilitliyse, [antitamper kilidi devre dışı işlemini](#disengage-the-antitamper-lock)izleyerek kilidi açın.
4. Sürücü taşıyıcı modülündeki siyah mandalı basın ve sürücü taşıyıcı kolunu şasinin önünden dışarı ve uzağa çekin.
   
    ![Disk sürücü tutamacını serbest bırakma](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    **Şekil 3** Sürücü tutamacını serbest bırakma
5. Sürücü taşıyıcı kolu tamamen uzadığında, sürücü taşıyıcısını şasinin dışına kaydırın. 
   
    ![Disksürücüsünden kayan disk](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    **Şekil 4** Disk sürücüsünün taşıyıcıdan kaydırılmasını

## <a name="install-the-replacement-disk-drive"></a>Yedek disk sürücüsüne yükleme
StorSimple aygıtınızda bir sürücü arızalı ve siz bunu kaldırdıktan sonra, yeni bir sürücüyle değiştirmek için bu yordamı uygulayın.

#### <a name="to-insert-a-drive"></a>Sürücü eklemek için
1. Aşağıdaki resimde gösterildiği gibi sürücü taşıyıcı tutamacının tamamen uzadığından emin olun.
   
    ![Tutamacı genişletilmiş disk sürücüsü](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    **Şekil 5** Kolu uzatılmış sürücü
2. Sürücü taşıyıcısını şasiye kadar kaydırın.
   
    ![Diski disk sürücü taşıyıcısına kaydırma](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    **Şekil 6**  Sürücü taşıyıcısını şasiye kaydırma
3. Sürücü taşıyıcısı takılıyken, sürücü taşıyıcısını şasiye itmeye devam ederken sürücü taşıyıcı tutamacını kapatın, ta ki sürücü taşıyıcısı kilitlenmiş bir konuma gelene kadar.
4. Kilit vidasını saat yönünde bir çeyrek dönüş çevirerek taşıyıcı kolunu yerine sabitlemek için Microsoft (kurcalamayadayanıklı Torx tornavida) tarafından sağlanan kilit anahtarını kullanın.
5. Değiştirmenin başarılı olduğunu ve sürücünün çalışır durumda olduğunu doğrulayın. Azure portalına erişin ve **Aygıt ayarları** > **Donanım sağlığına**gidin. **Paylaşılan bileşenler** veya **EBOD paylaşılan bileşenler**altında, sürücü durumu sağlıklı olduğunu belirten yeşil olmalıdır.

   
   > [!NOTE]
   > Değiştirmeden sonra disk durumunun yeşile dönmesi birkaç saat sürebilir.
  
## <a name="next-steps"></a>Sonraki adımlar
[StorSimple donanım bileşen değiştirme](storsimple-8000-hardware-component-replacement.md)hakkında daha fazla bilgi edinin.

