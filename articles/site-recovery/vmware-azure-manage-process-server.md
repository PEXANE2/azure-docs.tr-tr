---
title: Azure Site Kurtarma'da VMware VM'ler/fiziksel sunucu olağanüstü durum kurtarma için bir işlem sunucusu yönetme
description: Bu makalede, Azure Site Kurtarma kullanarak VMware VM/fiziksel sunucuların olağanüstü kurtarma için bir işlem sunucusu yönetmek açıklanmaktadır.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: ef16e3b75ca8e051b1b7abb1a92843279884c697
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257257"
---
# <a name="manage-process-servers"></a>İşlem sunucularını yönetme

Bu makalede, Site Kurtarma işlem sunucusunu yönetmek için ortak görevler açıklanmaktadır.

İşlem sunucusu, çoğaltma verilerini Almak, optimize etmek ve Azure'a göndermek için kullanılır. Ayrıca, çoğaltmak istediğiniz VMware VM'lerde ve fiziksel sunucularda Mobilite hizmetinin itme yüklemesini gerçekleştirir ve şirket içi makinelerin otomatik olarak keşfini gerçekleştirir. Şirket içi VMware VM'leri veya fiziksel sunucuları Azure'a çoğaltmak için işlem sunucusu yapılandırma sunucusu makinesine varsayılan olarak yüklenir. 

- Büyük dağıtımlar için kapasiteyi ölçeklendirmek için ek şirket içi işlem sunucularına ihtiyacınız olabilir.
- Azure'dan şirket içi başarısız olmak için Azure'da geçici bir işlem sunucusu ayarlamanız gerekir. Geri dönüş yapıldığında bu VM'yi silebilirsiniz. 

İşlem sunucusu hakkında daha fazla bilgi edinin.


## <a name="upgrade-a-process-server"></a>İşlem sunucusuyükseltme

Bir işlem sunucusunu şirket içinde veya başarısız geri dönüş için Azure VM olarak dağıttığınızda, işlem sunucusunun en son sürümü yüklenir. Site Kurtarma ekipleri düzeltmeleri ve geliştirmeleri düzenli olarak yayımlar ve işlem sunucularını güncel tutmanızı öneririz. Bir işlem sunucusunun yükseltmesi aşağıdaki gibi yapabilirsiniz:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]


## <a name="move-vms-to-balance-the-process-server-load"></a>İşlem sunucusu yükünü dengelemek için VM'leri taşıma

VM'leri iki işlem sunucusu arasında taşıyarak yükü aşağıdaki gibi dengeleyin:

1. Kasada, **Site Kurtarma Altyapısını** **Yönet'in** altında. **VMware & Fiziksel makineler için**Configuration **Servers'ı**tıklatın.
2. İşlem sunucularının kayıtlı olduğu yapılandırma sunucusuna tıklayın.
3. Bakiye trafiğini yüklemek istediğiniz işlem sunucusuna tıklayın.

    ![Yük Dengesi](media/vmware-azure-manage-process-server/LoadBalance.png)

4. **Yük bakiyesini**tıklatın, makineleri taşımak istediğiniz hedef işlem sunucusunu seçin. Ardından **Tamam'ı** tıklatın

    ![LoadPS](media/vmware-azure-manage-process-server/LoadPS.PNG)

2. **Makineleri Seç'i**tıklatın ve geçerliden hedef işlem sunucusuna taşımak istediğiniz makineleri seçin. Ortalama veri değişiminin ayrıntıları her sanal makineye karşı görüntülenir. Ardından **Tamam**'a tıklayın. 
3. Kasada, **İzleme** > **Sitesi Kurtarma işleri**altında işin ilerlemesini izleyin.

Değişikliklerin portala yansıtılması yaklaşık 15 dakika sürer. Daha hızlı bir etki için [yapılandırma sunucusunu yenileyin.](vmware-azure-manage-configuration-server.md#refresh-configuration-server)

## <a name="switch-an-entire-workload-to-another-process-server"></a>İş yükünün tamamını başka bir işlem sunucusuna değiştirme

Bir işlem sunucusu tarafından işlenen tüm iş yükünü aşağıdaki gibi farklı bir işlem sunucusuna taşıyın:

1. Kasada, **Site Kurtarma Altyapısını** **Yönet'in** altında. **VMware & Fiziksel makineler için**Configuration **Servers'ı**tıklatın.
2. İşlem sunucularının kayıtlı olduğu yapılandırma sunucusuna tıklayın.
3. İş yükünü değiştirmek istediğiniz işlem sunucusuna tıklayın.
4. **Anahtarla'yı**tıklatın, iş yükünü taşımak istediğiniz hedef işlem sunucusunu seçin. Ardından **Tamam'ı** tıklatın

    ![Anahtar](media/vmware-azure-manage-process-server/Switch.PNG)

5. Kasada, **İzleme** > **Sitesi Kurtarma işleri**altında işin ilerlemesini izleyin.

Değişikliklerin portala yansıtılması yaklaşık 15 dakika sürer. Daha hızlı bir etki için [yapılandırma sunucusunu yenileyin.](vmware-azure-manage-configuration-server.md#refresh-configuration-server)

## <a name="register-a-master-target-server"></a>Ana hedef sunucu kaydetme

Ana hedef sunucu yapılandırma sunucusunda ve ölçeklendirme işlem sunucularında bulunur. Yapılandırma sunucusuna kaydedilmelidir. Bu kayıtta bir hata olması durumunda, korunan öğelerin durumunu etkileyebilir. Ana hedef sunucuyu yapılandırma sunucusuna kaydetmek için, kaydın gerekli olduğu belirli yapılandırma sunucusu/ölçeklendirme işlem sunucusuna giriş yapın. **%PROGRAMDATA%\ASR\Agent**klasörüne gidin ve yönetici komut isteminde aşağıdakileri çalıştırın.

   ```
   cmd
   cdpcli.exe --registermt

   net stop obengine

   net start obengine

   exit
   ```

## <a name="reregister-a-process-server"></a>İşlem sunucusunun yeniden kaydedilmesi

Yapılandırma sunucusuyla şirket içinde veya Azure VM'de çalışan bir işlem sunucusunu aşağıdaki gibi yeniden kaydedin:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Ayarları kaydettikten sonra aşağıdakileri yapın:

1. İşlem sunucusunda bir yönetici komut istemi açın.
2. **%PROGRAMDATA%\ASR\Agent**klasörüne göz atın ve komutu çalıştırın:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Şirket içi işlem sunucusu için proxy ayarlarını değiştirme

Şirket içi bir işlem sunucusu Azure'a bağlanmak için proxy kullanıyorsa, proxy ayarlarını aşağıdaki gibi değiştirebilirsiniz:

1. İşlem sunucusu makinesinde oturum açın. 
2. Bir Yönetici PowerShell komut penceresini açın ve aşağıdaki komutu çalıştırın:
   ```powershell
   $pwd = ConvertTo-SecureString -String MyProxyUserPassword
   Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
   net stop obengine
   net start obengine
   ```
2. **%PROGRAMDATA%\ASR\Agent**klasörüne göz atın ve şu komutu çalıştırın:
   ```
   cmd
   cdpcli.exe --registermt

   net stop obengine

   net start obengine

   exit
   ```

## <a name="remove-a-process-server"></a>İşlem sunucusukaldırma

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="exclude-folders-from-anti-virus-software"></a>Klasörleri virüsten koruma yazılımından hariç tutma

Anti-virüs yazılımı ölçeklendirme işlem sunucusunda (veya ana hedef sunucuda) çalışıyorsa, aşağıdaki klasörleri virüsten koruma işlemlerinden hariç tutar:


- C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Kurtarma
- Sunucu yükleme dizini işleme. Örneğin: C:\Program Files (x86)\Microsoft Azure Site Kurtarma