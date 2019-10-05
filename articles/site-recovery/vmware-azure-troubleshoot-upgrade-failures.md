---
title: Microsoft Azure Site Recovery sağlayıcısı yükseltme hatalarında sorun giderme | Microsoft Docs
description: Anlayın ve
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 02/05/2019
ms.author: raynew
ms.openlocfilehash: 6bb8263e7291506be6af7d5af7eda5786274d73c
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71972601"
---
# <a name="troubleshoot-microsoft-azure-site-recovery-provider-upgrade-failures"></a>Microsoft Azure Site Recovery Sağlayıcısını yükseltme hatalarını giderme

Bu makale, Microsoft Azure Site Recovery sağlayıcı yükseltmesi sırasında hatalara neden olabilecek sorunları çözmenize yardımcı olur.

## <a name="the-upgrade-fails-reporting-that-the-latest-site-recovery-provider-is-already-installed"></a>Yükseltme, en son Site Recovery sağlayıcının zaten yüklü olduğunu bildiriyor

Microsoft Azure Site Recovery sağlayıcısını (DRA) yükseltirken, Birleşik kurulum yükseltmesi başarısız olur ve hata iletisini yayınlar:

Yazılımın daha yüksek bir sürümü zaten yüklü olduğundan yükseltme desteklenmiyor.

Yükseltmek için aşağıdaki adımları kullanın:

1. Birleşik kurulum Site Recovery Microsoft Azure indirin:
   1. [Azure Site Recovery makalesindeki hizmet güncelleştirmelerinin](service-updates-how-to.md##links-to-currently-supported-update-rollups) "Şu anda desteklenen güncelleştirme paketlerinin bağlantıları" bölümünde, yükseltmekte olduğunuz sağlayıcıyı seçin.
   2. Toplama sayfasında, **güncelleştirme bilgileri** bölümünü bulun ve Site Recovery birleşik kurulum Microsoft Azure Için güncelleştirme paketini indirin.

2. Bir komut istemi açın ve Birleşik kurulum dosyasını indirdiğiniz klasöre gidin. Şu komutu kullanarak kurulum dosyalarını MicrosoftAzureSiteRecoveryUnifiedSetup. exe/q/x: &lt;folder yolunu ayıklayın (@ no__t-1 ayıklanan dosyalar için).
    
    Örnek komut:

    MicrosoftAzureSiteRecoveryUnifiedSetup. exe/q/x: C:\temp\ayıklanmış

3. Komut isteminde, dosyaları ayıkladığınız klasöre gidin ve aşağıdaki yükleme komutlarını çalıştırın:
   
    CX_THIRDPARTY_SETUP. EXE/VERYSILENT/SUPPRESSMSGKUTULAR/NORESTART UCX_SERVER_SETUP. EXE/VERYSILENT/SUPPRESSMSGKUTULAR/NORESTART/UPGRADE

1. Birleşik kurulumu indirdiğiniz klasöre dönün ve yükseltme işleminin ardından MicrosoftAzureSiteRecoveryUnifiedSetup. exe ' yi çalıştırın. 

## <a name="upgrade-failure-due-to-the-3rd-party-folder-being-renamed"></a>Yeniden adlandırılmakta olan üçüncü taraf klasör nedeniyle yükseltme hatası

Yükseltmenin başarılı olması için 3. taraf klasörü yeniden adlandırılmamalıdır.

Sorunu gidermek için.

1. Kayıt Defteri Düzenleyicisi 'Ni (Regedit. exe) başlatın ve HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\InMage Systems\yüklü Products\10 dalını açın.
1. @No__t-0 anahtar değerini inceleyin. En son sürüme ayarlanırsa sürüm numarasını küçültün. Örneğin, en son sürüm 9,22. \* ve `Build_Version` anahtarı bu değere ayarlanmışsa, 9,21. \* ' ye küçültün.
1. Birleşik kurulum Site Recovery en son Microsoft Azure indirin:
   1. [Azure Site Recovery makalesindeki hizmet güncelleştirmelerinin](service-updates-how-to.md##links-to-currently-supported-update-rollups) "Şu anda desteklenen güncelleştirme paketlerinin bağlantıları" bölümünde, yükseltmekte olduğunuz sağlayıcıyı seçin.
   2. Toplama sayfasında, **güncelleştirme bilgileri** bölümünü bulun ve Site Recovery birleşik kurulum Microsoft Azure Için güncelleştirme paketini indirin.
1. Bir komut istemi açın ve Birleşik kurulum dosyasını indirdiğiniz klasöre gidin ve aşağıdaki komutu kullanarak kurulum dosyalarını indirmeden ayıklayın: MicrosoftAzureSiteRecoveryUnifiedSetup. exe/q/x: &lt;klasör yolu ayıklanan dosyalar @ no__t-1.

    Örnek komut:

    MicrosoftAzureSiteRecoveryUnifiedSetup. exe/q/x: C:\temp\ayıklanmış

1. Komut isteminde, dosyaları ayıkladığınız klasöre gidin ve aşağıdaki yükleme komutlarını çalıştırın:
   
    CX_THIRDPARTY_SETUP. EXE/VERYSILENT/SUPPRESSMSGKUTULAR/NORESTART

1. Yüklemenin ilerlemesini izlemek için Görev Yöneticisi 'ni kullanın. CX_THIRDPARTY_SETUP için işlem. EXE artık görev yöneticisi 'nde görünmez, sonraki adıma geçin.
1. C:\üçüncü taraf olduğunu ve klasörün RRD kitaplıklarını içerdiğini doğrulayın.
1. Birleşik kurulumu indirdiğiniz klasöre dönün ve yükseltme işleminin ardından MicrosoftAzureSiteRecoveryUnifiedSetup. exe ' yi çalıştırın. 