---
title: Azure Site Kurtarma ile VMware VM'lerinin ve fiziksel sunucuların olağanüstü kurtarma sırasında ölçeklendirilen bir işlem sunucusu ayarlama | Microsoft Dokümanlar'
description: Bu makalede, VMware VM'lerin ve fiziksel sunucuların olağanüstü kurtarma sırasında ölçeklendirme işlem sunucusunun nasıl ayarlanacağı açıklanmaktadır.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 4/23/2019
ms.author: ramamill
ms.openlocfilehash: 1b6084b4e93f3dc17f633f1b8496f9c26e7f576f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257153"
---
# <a name="scale-with-additional-process-servers"></a>Ek işlem sunucularıyla ölçeklendirme

Varsayılan olarak, [Site Kurtarma'yı](site-recovery-overview.md)kullanarak VMware VM'lerini veya fiziksel sunucularını Azure'a kopyalarken yapılandırma sunucusu makinesine bir işlem sunucusu yüklenir ve Site Kurtarma ile şirket içi altyapınız arasındaki veri aktarımını koordine etmek için kullanılır. Kapasiteyi artırmak ve çoğaltma dağıtımınızı ölçeklendirmek için ek bağımsız işlem sunucuları ekleyebilirsiniz. Bu makalede, ölçeklendirme işlem sunucusu nasıl ayarlanılır.

## <a name="before-you-start"></a>Başlamadan önce

### <a name="capacity-planning"></a>Kapasite planlaması

VMware çoğaltma için [kapasite planlaması](site-recovery-plan-capacity-vmware.md) yaptığınızdan emin olun. Bu, ek işlem sunucularını nasıl ve ne zaman dağıtmanız gerektiğini belirlemenize yardımcı olur.

9.24 sürümünden itibaren, yeni çoğaltmalar için işlem sunucusu seçimi sırasında kılavuz eklenir. İşlem sunucusu belirli ölçütlere göre Sağlıklı, Uyarı ve Kritik olarak işaretlenir. İşlem sunucusunun durumunu etkileyebilen farklı senaryoları anlamak [için, işlem sunucusu uyarılarını](vmware-physical-azure-monitor-process-server.md#process-server-alerts)gözden geçirin.

> [!NOTE]
> Klonlanmış Process Server bileşeninin kullanımı desteklenmez. Her PS ölçeklendirmesi için bu makaledeki adımları izleyin.

### <a name="sizing-requirements"></a>Boyutlandırma gereksinimleri 

Tabloda özetlenen boyutlandırma gereksinimlerini doğrulayın. Genel olarak, dağıtımınızı 200'den fazla kaynak makineye ölçeklendirmeniz gerekiyorsa veya toplam günlük çalkalama oranınız 2 TB'den fazlaysa, trafik hacmini işlemek için ek işlem sunucularına ihtiyacınız vardır.

| **Ek işlem sunucusu** | **Önbellek disk boyutu** | **Veri değişim oranı** | **Korumalı makineler** |
| --- | --- | --- | --- |
|4 vCPUs (2 soket \@ * 2 çekirdek 2,5 GHz), 8 GB bellek |300 GB |250 GB veya daha az |85 veya daha az makine çoğaltın. |
|8 vCPUs (2 soket \@ * 4 çekirdek 2,5 GHz), 12 GB bellek |600 GB |250 GB - 1 TB |85-150 makineleri arasında çoğaltmak. |
|12 vCPUs (2 soket \@ * 6 çekirdek 2,5 GHz) 24 GB bellek |1 TB |1 TB - 2 TB |150-225 makineleri arasında çoğaltmak. |

Her korunan kaynak makinenin her biri 100 GB'lık 3 diskle yapılandırıldığı yer.

### <a name="prerequisites"></a>Ön koşullar

Ek işlem sunucusu nun ön koşulları aşağıdaki tabloda özetlenmiştir.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="download-installation-file"></a>Yükleme dosyasını karşıdan yükleme

İşlem sunucusu için yükleme dosyasını aşağıdaki gibi indirin:

1. Azure portalında oturum açın ve Kurtarma Hizmetleri Vault'unuza göz atın.
2. Açık **Site Kurtarma Altyapısı** > **VMWare ve Fiziksel Makineler** > **Yapılandırma Sunucuları** (VMware & Fiziksel Makineler için altında).
3. Sunucu ayrıntılarını ayrıntılı olarak incelemek için yapılandırma sunucusunu seçin. Ardından **+ İşlem Sunucusu'na**tıklayın.
4. **İşlem Ekle sunucusunda** >  **İşlem sunucunuzu dağıtmak istediğiniz yeri seçin**, şirket içinde **Ölçeklendirme İşlem Sunucusu'nı dağıt'ı**seçin.

   ![Sunucu Lar Sayfası Ekle](./media/vmware-azure-set-up-process-server-scale/add-process-server.png)
1. **Microsoft Azure Sitesi Kurtarma Tümlatay Kurulum'u Karşıdan Yükleyin'i**tıklatın. Bu, yükleme dosyasının en son sürümünü karşıdan yükler.

   > [!WARNING]
   > İşlem sunucusu yükleme sürümü, çalıştırdığınız yapılandırma sunucusu sürümüyle aynı veya daha önce olmalıdır. Sürüm uyumluluğunu sağlamanın basit bir yolu, yapılandırma sunucunuzu yüklemek veya güncelleştirmek için en son kullandığınız aynı yükleyiciyi kullanmaktır.

## <a name="install-from-the-ui"></a>UI'den yükleme

Aşağıdaki gibi yükleyin. Sunucuyu ayarladıktan sonra, kullanmak için kaynak makineleri geçirin.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="install-from-the-command-line"></a>Komut satırından yükleme

Aşağıdaki komutu çalıştırarak yükleyin:

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

Komut satırı parametrelerinin aşağıdaki gibi olduğu durumlarda:

[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]

Örnek:

```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```
### <a name="create-a-proxy-settings-file"></a>Proxy ayarları dosyası oluşturma

Proxy ayarlamanız gerekiyorsa, ProxySettingsFilePath parametresi bir dosyayı giriş olarak alır. Dosyayı aşağıdaki gibi oluşturabilir ve proxySettingsFilePath parametresi olarak geçirebilirsiniz.

```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```

## <a name="next-steps"></a>Sonraki adımlar
İşlem [sunucusu ayarlarını yönetme](vmware-azure-manage-process-server.md) hakkında bilgi edinin
