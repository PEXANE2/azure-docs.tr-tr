---
title: StorSimple 8000 serisi cihazda DATA 0 ayarlarını değiştirin | Microsoft Dokümanlar
description: StorSimple cihazınızdaki DATA 0 ağ arabirimini yeniden yapılandırmak için StorSimple için Windows PowerShell'i nasıl kullanacağınızı öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: 3cf136c5ddec8f4998d15c597914e1f806453945
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60631592"
---
# <a name="modify-the-data-0-network-interface-settings-on-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınızdaki DATA 0 ağ arabirimi ayarlarını değiştirme

## <a name="overview"></a>Genel Bakış

Microsoft Azure StorSimple cihazınızda DATA 0'dan DATA 5'e kadar altı ağ arabirimi bulunur. DATA 0 arabirimi her zaman Windows PowerShell arabirimi veya seri konsol aracılığıyla yapılandırılır ve otomatik olarak bulut etkindir. AZURE portalı üzerinden DATA 0 ağ arabirimini yapılandıramayacağınızı unutmayın.

DATA 0 arabirimi ilk olarak StorSimple aygıtının ilk dağıtımı sırasında kurulum sihirbazı aracılığıyla yapılandırılır. Aygıt bir işletim modundayken, DATA 0 ayarlarını yeniden yapılandırmanız gerekebilir. Bu öğretici, StorSimple için Windows PowerShell aracılığıyla DATA 0 ağ ayarlarını değiştirmek için iki yöntem sağlar.

Bu öğretici okuduktan sonra, mümkün olacak:

* Kurulum sihirbazı aracılığıyla DATA 0 ağ ayarını değiştirme
* CMDLET üzerinden DATA `Set-HcsNetInterface` 0 ağ ayarlarını değiştirin

## <a name="modify-data-0-network-settings-through-setup-wizard"></a>Kurulum sihirbazı aracılığıyla DATA 0 ağ ayarlarını değiştirme
StorSimple cihazınızın Windows PowerShell arabirimine bağlanarak ve bir kurulum sihirbazı oturumu başlatarak DATA 0 ağ ayarlarını yeniden yapılandırabilirsiniz. DATA 0 ayarlarını değiştirmek için aşağıdaki adımları gerçekleştirin:

#### <a name="to-modify-data-0-network-settings-through-setup-wizard"></a>Kurulum sihirbazı aracılığıyla DATA 0 ağ ayarlarını değiştirmek için
1. Seri konsol menüsünde, seçenek 1 seçin, **tam erişim ile giriş yapın.** İstendiğinde **aygıt yöneticisi parolasını**sağlayın. Varsayılan `Password1`parola.
2. Komut istemine şunları yazın:
   
    `Invoke-HcsSetupWizard`
3. Bir kurulum sihirbazı, cihazınızın DATA 0 arabiriminin yapılandırılmasına yardımcı olur. IP adresi, ağ geçidi ve netmaske için yeni değerler sağlayın.

> [!NOTE]
> Sabit denetleyiciIP'lerin Azure portalındaki StorSimple cihazının **Ağ ayarları** bıçağı aracılığıyla yeniden yapılandırılması gerekir. Daha fazla bilgi için [ağ arabirimlerini değiştir'e](storsimple-8000-modify-device-config.md#modify-network-interfaces)gidin.

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>SET-HcsNetInterface cmdlet ile DATA 0 ağ ayarlarını değiştirin
DATA 0 ağ arabirimini yeniden yapılandırmanın alternatif `Set-HcsNetInterface` bir yolu cmdlet kullanımından geçer. Cmdlet, StorSimple cihazınızın Windows PowerShell arabiriminden çalıştırılır. Bu yordamı kullanırken, denetleyici sabit IP'ler de burada yapılandırılabilir. DATA 0 ayarlarını değiştirmek için aşağıdaki adımları gerçekleştirin: 

#### <a name="to-modify-data-0-network-settings-through-the-set-hcsnetinterface-cmdlet"></a>Set-HcsNetInterface cmdlet üzerinden DATA 0 ağ ayarlarını değiştirmek için
1. Seri konsol menüsünde, seçenek 1 seçin, **tam erişim ile giriş yapın.** İstendiğinde aygıt yöneticisi parolasını sağlayın. Varsayılan `Password1`parola.
2. Komut istemine şunları yazın:
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    Açılı paranteziçinde, DATA 0 için aşağıdaki değerleri yazın:
   
   * IPv4 adresi
   * IPv4 ağ geçidi
   * IPv4 alt ağ maskesi
   * Denetleyici 0 için IPv4 adresi düzeltildi
   * Denetleyici 1 için IPv4 adresi düzeltildi
     
     Bu cmdlet kullanımı hakkında daha fazla bilgi [için, StorSimple cmdlet referans için Windows PowerShell](https://technet.microsoft.com/library/dn688161.aspx)gidin.

## <a name="next-steps"></a>Sonraki adımlar
* DATA 0 dışındaki ağ arabirimlerini yapılandırmak için [Azure portalındaki Yapılandır ağ ayarlarını](storsimple-8000-modify-device-config.md)kullanabilirsiniz. 
* Ağ arabirimlerinizi yapılandırırken herhangi bir sorunla karşılaşırsanız, [dağıtım sorunlarını giderin.](storsimple-troubleshoot-deployment.md)

