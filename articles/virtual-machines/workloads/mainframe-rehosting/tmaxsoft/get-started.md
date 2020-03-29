---
title: Azure Sanal Makinelerde TmaxSoft OpenFrame ile başlayın
description: Azure Sanal Makinelerde (VM) TmaxSoft OpenFrame ortamını kullanarak IBM z/OS ana bilgisayar iş yüklerinizi yeniden barındırın.
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.openlocfilehash: 408e0166e52af9efd3d4c64f1b29bddcfc1cca4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61485534"
---
# <a name="get-started-with-tmaxsoft-openframe-on-azure"></a>Azure'da TmaxSoft OpenFrame ile başlayın

Varolan ana bilgisayar varlıklarınızı tmaxSoft OpenFrame kullanarak Microsoft Azure'a taşıyın. Bu popüler yeniden barındırma çözümü, Azure'da bir öykünme ortamı oluşturarak uygulamaları hızla geçirmenizi sağlar. Yeniden biçimlendirme gerekmez.

## <a name="openframe-rehosting-environment"></a>OpenFrame yeniden barındırma ortamı

Geliştirme, demolar, testler veya üretim iş yükleri için Azure'da bir OpenFrame ortamı ayarlayın. Aşağıdaki şekilde ngösterdiği gibi, OpenFrame Azure'da ana bilgisayar öykünme ortamını oluşturan birden çok bileşen içerir. Örneğin, OpenFrame çevrimiçi hizmetleri IBM Müşteri Bilgi Kontrol Sistemi (CICS) gibi ana bilgisayar ara yazılımlarının yerini alır. OpenFrame Toplu, TJES bileşeni ile IBM ana bilgisayarının İş Giriş Alt Sistemi'nin (JES) yerini alır. 

![OpenFrame yeniden barındırma işlemi](media/openframe-01.png)

> [!NOTE]
> Azure'da OpenFrame ortamını çalıştırmak için TmaxSoft'tan geçerli bir ürün lisansınız veya deneme lisansınolması gerekir.

## <a name="openframe-components"></a>OpenFrame bileşenleri

Aşağıdaki bileşenler Azure'daki OpenFrame ortamının bir parçasıdır:

- Ana bilgisayar varlıklarını analiz eden ve bunları Azure'a geçiren bir çözüm olan OFMiner gibi **geçiş araçları.**
- **Derleyiciler**, OFCOBOL, ana bilgisayarın COBOL programlarını yorumlayan bir derleyici; Ana bilgisayarın PL/I programlarını yorumlayan OFPLI; ve OFASM, ana bilgisayarın assembler programlarını yorumlayan bir derleyici.
- Java Enterprise User Solution (JEUS), Java Enterprise Edition 6.OFGW için onaylı bir web uygulama sunucusu ve 3270 dinleyici sağlayan OpenFrame ağ geçidi bileşeni dahil olmak üzere **ön uç** bileşenleri.
- **Uygulama** ortamı. OpenFrame Base, tüm sistemi yöneten ara yazılımdır. OpenFrame Server Type C (OSC), ana bilgisayarın ara bilgisayarının ve IBM CICS'nin yerini alır.
- Tibero (gösterilen), Oracle Database, Microsoft SQL Server, IBM Db2 veya MySQL gibi **ilişkisel veritabanı.** OpenFrame uygulamaları veritabanı ile iletişim kurmak için Açık Veritabanı Bağlantısı (ODBC) iletişim kuralını kullanır.
- Tacf, sistem ve kaynaklara kullanıcı erişimini kontrol eden bir hizmet modülü üzerinden **güvenlik.** 
- **OFManager,** OpenFrame'in web ortamında ki çalışma ve yönetim işlevlerini sağlayan bir çözümdür.

![OpenFrame mimarisi](media/openframe-02.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da TmaxSoft OpenFrame'i yükleme](./install-openframe-azure.md)
