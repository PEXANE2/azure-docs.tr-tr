---
title: Microsoft Azure Site Kurtarma Sağlayıcısı'nın sorun giderme yükseltmesi
description: Microsoft Azure Site Kurtarma sağlayıcısını yükseltirken ortaya çıkan sık karşılaşılan sorunları giderin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 11/10/2019
ms.author: raynew
ms.openlocfilehash: b59f933fedd5f1d3ed3f7972b1a1fe653df31be2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75893908"
---
# <a name="troubleshoot-microsoft-azure-site-recovery-provider-upgrade-failures"></a>Microsoft Azure Site Recovery Sağlayıcısını yükseltme hatalarını giderme

Bu makale, Microsoft Azure Site Kurtarma Sağlayıcısı yükseltmesi sırasında hatalara neden olabilecek sorunları çözmenize yardımcı olur.

## <a name="the-upgrade-fails-reporting-that-the-latest-site-recovery-provider-is-already-installed"></a>Yükseltme, en son Site Kurtarma Sağlayıcısı'nın zaten yüklü olduğunu bildirmede başarısız oldu

Microsoft Azure Site Kurtarma Sağlayıcısı'nı (DRA) yükseltirken, Birleşik Kurulum yükseltmesi başarısız olur ve hata iletisini yayınlar:

Yükseltme, yazılımın daha yüksek bir sürümü zaten yüklendiğinden desteklenmez.

Yükseltmek için aşağıdaki adımları kullanın:

1. Microsoft Azure Sitesi Kurtarma Birleşik Kurulum'u indirin:
   1. [Azure Site Kurtarma makalesinde Hizmet güncelleştirmelerinin](service-updates-how-to.md#links-to-currently-supported-update-rollups) "Desteklenen Güncelleştirme toplama bağlantıları" bölümünde, yükseltme yaptığınız sağlayıcıyı seçin.
   2. Toplama sayfasında, **Bilgileri Güncelleştir** bölümünü bulun ve Microsoft Azure Sitesi Kurtarma Tümlatİf Kurulumu için Güncelleştirme Rollup'ını indirin.

2. Bir komut istemi açın ve Birleşik Kurulum dosyasını indirdiğiniz klasöre gidin. Aşağıdaki komutu kullanarak indirme kurulum dosyalarını ayıklayın, MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:&lt;çıkarılan dosyalar&gt;için klasör yolu .
    
    Örnek komut:

    MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted

3. Komut isteminde, dosyaları ayıkladığınız klasöre gidin ve aşağıdaki yükleme komutlarını çalıştırın:
   
    CX_THIRDPARTY_SETUP. EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART UCX_SERVER_SETUP. EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART /UPGRADE

1. Birleşik Kurulum'u indirdiğiniz klasöre dönün ve yükseltmeyi tamamlamak için MicrosoftAzureSiteRecoveryUnifiedSetup.exe çalıştırın. 

## <a name="upgrade-failure-due-to-the-3rd-party-folder-being-renamed"></a>Üçüncü taraf klasörün yeniden adlandırılması nedeniyle yükseltme hatası

Yükseltmenin başarılı olabilmesi için üçüncü taraf klasörüne yeniden ad verilmemelidir.

Sorunu çözmek için.

1. Kayıt Defteri Düzenleyicisi'ni (regedit.exe) başlatın ve HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\InMage Systems\Installed Products\10 şubesini açın.
1. `Build_Version` Anahtar değerini inceleyin. En son sürüme ayarlanmışsa, sürüm numarasını azaltın. Örneğin, en son sürüm 9,22 ise. \* ve `Build_Version` bu değere ayarlanan anahtar, sonra 9,21'e düşürün. \*.
1. En son Microsoft Azure Site Kurtarma Birleşik Kurulum'u indirin:
   1. [Azure Site Kurtarma makalesinde Hizmet güncelleştirmelerinin](service-updates-how-to.md#links-to-currently-supported-update-rollups) "Desteklenen Güncelleştirme toplama bağlantıları" bölümünde, yükseltme yaptığınız sağlayıcıyı seçin.
   2. Toplama sayfasında, **Bilgileri Güncelleştir** bölümünü bulun ve Microsoft Azure Sitesi Kurtarma Tümlatİf Kurulumu için Güncelleştirme Rollup'ını indirin.
1. Bir komut istemi açın ve Birleşik Kurulum dosyasını indirdiğiniz klasöre gidin ve aşağıdaki komutu kullanarak indirmeden kurulum dosyalarını ayıklayın,&lt;MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x: çıkarılan dosyalar&gt;için klasör yolu .

    Örnek komut:

    MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted

1. Komut isteminde, dosyaları ayıkladığınız klasöre gidin ve aşağıdaki yükleme komutlarını çalıştırın:
   
    CX_THIRDPARTY_SETUP. EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART

1. Yüklemenin ilerlemesini izlemek için görev yöneticisini kullanın. Ne zaman CX_THIRDPARTY_SETUP için süreç. EXE artık görev yöneticisi görünür, bir sonraki adıma geçin.
1. C:\thirdparty'nin var olduğunu ve klasörün RRD kitaplıklarını içerdiğini doğrulayın.
1. Birleşik Kurulum'u indirdiğiniz klasöre dönün ve yükseltmeyi tamamlamak için MicrosoftAzureSiteRecoveryUnifiedSetup.exe çalıştırın. 