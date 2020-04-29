---
title: VMware VM 'Leri ve fiziksel sunucuları Azure Site Recovery ile olağanüstü durum kurtarma sırasında genişleme işlem sunucusu ayarlama | Microsoft Docs '
description: Bu makalede, VMware VM 'Leri ve fiziksel sunucular için olağanüstü durum kurtarma sırasında genişleme işlem sunucusu 'nun nasıl ayarlanacağı açıklanır.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 4/23/2019
ms.author: ramamill
ms.openlocfilehash: 1b6084b4e93f3dc17f633f1b8496f9c26e7f576f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79257153"
---
# <a name="scale-with-additional-process-servers"></a>Ek işlem sunucularıyla ölçeklendirme

Varsayılan olarak, [Site Recovery](site-recovery-overview.md)kullanarak VMware VM 'lerini veya fiziksel sunucuları Azure 'a çoğaltdığınızda, yapılandırma sunucusu makinesine bir işlem sunucusu yüklenir ve Site Recovery ile şirket içi altyapınız arasında veri aktarımını koordine etmek için kullanılır. Kapasiteyi artırmak ve çoğaltma dağıtımınızı ölçeklendirmek için, başka bir tek başına işlem sunucusu ekleyebilirsiniz. Bu makalede, bir genişleme işlem sunucusunun nasıl ayarlanacağı açıklanır.

## <a name="before-you-start"></a>Başlamadan önce

### <a name="capacity-planning"></a>Kapasite planlaması

VMware çoğaltması için [Kapasite planlaması](site-recovery-plan-capacity-vmware.md) gerçekleştirdiğinizden emin olun. Bu, ek işlem sunucularını nasıl ve ne zaman dağıtacağınızı belirlemenize yardımcı olur.

9,24 sürümünden, yeni çoğaltmalar için işlem sunucusu seçimi sırasında rehberlik eklenir. İşlem sunucusu, belirli ölçütlere göre sağlıklı, uyarı ve kritik olarak işaretlenir. İşlem sunucusu durumunu etkileyebilecek farklı senaryoları anlamak için, [işlem sunucusu uyarılarını](vmware-physical-azure-monitor-process-server.md#process-server-alerts)gözden geçirin.

> [!NOTE]
> Kopyalanmış Işlem sunucusu bileşeni kullanımı desteklenmiyor. Her PS ölçeği için bu makaledeki adımları izleyin.

### <a name="sizing-requirements"></a>Boyutlandırma gereksinimleri 

Tabloda özetlenen boyutlandırma gereksinimlerinin olduğunu doğrulayın. Genel olarak, dağıtımınızı 200 ' den fazla kaynak makineye ölçeklendirmeniz veya 2 TB 'den fazla toplam günlük dalgalanma oranı varsa, trafik birimini işlemek için ek işlem sunucularına ihtiyacınız vardır.

| **Ek işlem sunucusu** | **Önbellek diski boyutu** | **Veri değişim oranı** | **Korumalı makineler** |
| --- | --- | --- | --- |
|4 vCPU (2 yuva * 2 çekirdek \@ 2,5 GHz), 8 GB bellek |300 GB |250 GB veya daha az |85 veya daha az makineyi çoğaltın. |
|8 vCPU (2 yuva * 4 çekirdek \@ 2,5 GHz), 12 GB bellek |600 GB |250 GB ila 1 TB |85-150 makine arasında çoğaltın. |
|12 vCPU (2 yuva * 6 çekirdek \@ 2,5 GHz) 24 GB bellek |1 TB |1 TB-2 TB |150-225 makine arasında çoğaltın. |

Her korunan kaynak makinenin her biri 3 disk 100 GB ile yapılandırılır.

### <a name="prerequisites"></a>Ön koşullar

Ek işlem sunucusu Önkoşulları aşağıdaki tabloda özetlenmiştir.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="download-installation-file"></a>Yükleme dosyasını indir

İşlem sunucusu için yükleme dosyasını aşağıdaki gibi indirin:

1. Azure portal oturum açın ve kurtarma hizmetleri kasanıza gidin.
2. **Site Recovery altyapı** > **VMware ve fiziksel makineler** > **yapılandırma sunucularını** açın (VMware & fiziksel makineleri için altında).
3. Sunucu ayrıntılarının detayına gitmek için yapılandırma sunucusunu seçin. Ardından **+ Işlem sunucusu**' na tıklayın.
4. İşlem sunucusu **Ekle** >  ' de,**işlem sunucunuzu dağıtmak istediğiniz yeri seçin**' de, **Şirket içi genişleme işlem sunucusu dağıt**' ı seçin.

   ![Sunucu ekleme sayfası](./media/vmware-azure-set-up-process-server-scale/add-process-server.png)
1. **Birleşik kurulum Site Recovery Microsoft Azure indir**' e tıklayın. Bu, yükleme dosyasının en son sürümünü indirir.

   > [!WARNING]
   > İşlem sunucusu yükleme sürümü, çalıştırdığınız yapılandırma sunucusu sürümü ile aynı veya daha önceki bir sürüm olmalıdır. Sürüm uyumluluğunu sağlamanın basit bir yolu, yapılandırma sunucunuzu yüklemek veya güncelleştirmek için en son kullandığınız yükleyiciyi kullanmaktır.

## <a name="install-from-the-ui"></a>Kullanıcı arabiriminden yüklemesi

Uygulamasını aşağıdaki şekilde yapın. Sunucuyu ayarladıktan sonra, kaynak makineleri kullanmak üzere geçirolursunuz.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="install-from-the-command-line"></a>Komut satırından yükler

Aşağıdaki komutu çalıştırarak yüklemesini yapın:

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

Burada komut satırı parametreleri aşağıdaki gibidir:

[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]

Örneğin:

```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```
### <a name="create-a-proxy-settings-file"></a>Proxy ayarları dosyası oluşturma

Bir ara sunucu ayarlamanız gerekirse, ProxySettingsFilePath parametresi bir dosyayı giriş olarak alır. Dosyayı aşağıdaki gibi oluşturabilir ve giriş ProxySettingsFilePath parametresi olarak geçirebilirsiniz.

```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```

## <a name="next-steps"></a>Sonraki adımlar
[İşlem sunucusu ayarlarını yönetme](vmware-azure-manage-process-server.md) hakkında bilgi edinin
