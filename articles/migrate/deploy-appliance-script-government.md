---
title: Azure Kamu 'da Azure geçiş gereci ayarlama
description: Azure Kamu 'da Azure geçiş gereci ayarlamayı öğrenin
ms.topic: article
ms.date: 04/16/2020
ms.openlocfilehash: fd97161ffa075a6165ea963ef80bfabf8904576e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81726741"
---
# <a name="set-up-an-appliance-in-azure-government"></a>Azure Kamu 'da gereç ayarlama 

Bir Azure Kamu bulutundaki VMware VM 'Leri, Hyper-V VM 'Leri ve fiziksel sunucular için bir [Azure geçiş](deploy-appliance.md) gereci dağıtmak üzere bu makaleye uyun. Gereci oluşturmak için bir komut dosyası çalıştırın ve Azure 'a bağlanabildiğini doğrulayın. Genel bulutta bir gereç kurmak istiyorsanız [Bu makaleyi](deploy-appliance-script.md)izleyin.


> [!NOTE]
> Şablonu kullanarak bir gereci dağıtma seçeneği (VMware VM 'Leri ve Hyper-V VM 'Leri için) Azure Kamu 'da desteklenmez.


## <a name="prerequisites"></a>Ön koşullar

Betik, mevcut bir fiziksel makineye veya VM 'ye Azure geçişi gereci ayarlar.

- Gereç işlevi görecek makinenin Windows Server 2016, 32 GB bellek, 5. disk depolaması ve bir dış sanal anahtar ile birlikte sekiz V80 CPU çalıştırması gerekir. Statik veya dinamik bir IP adresi ve internet erişimi gerektirir.
- Gereci dağıtmadan önce, [VMware VM 'leri](migrate-appliance.md#appliance---vmware), [Hyper-V VM 'leri](migrate-appliance.md#appliance---hyper-v)ve [fiziksel sunucular](migrate-appliance.md#appliance---physical)için ayrıntılı gereç gereksinimlerini gözden geçirin.
- Betiği mevcut bir Azure geçiş gereci üzerinde çalıştırmayın.

## <a name="set-up-the-appliance-for-vmware"></a>VMware için gereci ayarlama

VMware için gereci ayarlamak üzere Azure portal daraltılmış bir dosya indirir ve içeriği ayıklayabilir. Gereç Web uygulamasını başlatmak için PowerShell betiğini çalıştırın. Gereci ayarlayın ve ilk kez yapılandırın. Ardından, gereci Azure geçişi projesi ile kaydedersiniz.

### <a name="download-the-script"></a>Betiği indir

1.  **Geçiş hedefleri** > **sunucuları** > **Azure geçişi: Sunucu değerlendirmesi**' nde **keşfet**' e tıklayın.
2.   > Makinelerde **bulunan makineler****sanallaştırılmış mı?**, **VMware vSphere Hiper Yöneticisi ile Evet '** i seçin.
3.  Sıkıştırılmış dosyayı indirmek için **İndir**' e tıklayın. 


### <a name="verify-file-security"></a>Dosya güvenliğini doğrula

Dağıtmadan önce daraltılmış dosyanın güvenli olduğunu denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. Daraltılmış dosyanın karmasını oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-VMWare-USGov.zip MD5```

3. Oluşturulan karma değerleri doğrulayın. En son gereç sürümü için:

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | 6316bcc8bc932204295bfe33f4be3949
          

### <a name="run-the-script"></a>Betiği çalıştırın

Komut dosyası şu şekildedir:

- Aracıları ve bir Web uygulamasını kurar.
- Windows etkinleştirme hizmeti, IIS ve PowerShell ıSE dahil Windows rollerini kurar.
- Bir IIS yeniden yazılabilir modülünü indirir ve yükler. [Daha fazla bilgi edinin](https://www.microsoft.com/download/details.aspx?id=7435).
- Azure geçişi için kalıcı ayarlarla bir kayıt defteri anahtarını (HKLM) güncelleştirir.
- Günlük ve yapılandırma dosyalarını şu şekilde oluşturur:
    - **Yapılandırma dosyaları**:%ProgramData%\Microsoft Azure\Config
    - **Günlük dosyaları**:%ProgramData%\Microsoft Azure\Logs

Betiği çalıştırmak için:

1. Sıkıştırılmış dosyayı, gereci barındıracak makinedeki bir klasöre ayıklayın. Betiği mevcut bir Azure geçişi gereci üzerinde bir makinede çalıştırmayın emin olun.
2. Makinede, yönetici (yükseltilmiş) ayrıcalıklarla PowerShell 'i başlatın.
3. PowerShell dizinini, indirilen sıkıştırılmış dosyadan ayıklanan içerikleri içeren klasör olarak değiştirin.
4. **AzureMigrateInstaller. ps1**betiğini aşağıdaki gibi çalıştırın:``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-VMWare-USGov>AzureMigrateInstaller.ps1 ```
5. Betik başarıyla çalıştıktan sonra gereci ayarlayabilmeniz için gereç Web uygulamasını başlatır. Herhangi bir sorunla karşılaşırsanız, C:\ProgramData\Microsoft Azure\Logs\ AzureMigrateScenarioInstaller_<em>timestamp</em>. log konumundaki betik günlüklerini gözden geçirin.

### <a name="verify-access"></a>Erişimi doğrulama

Gerecin [kamu bulutları](migrate-appliance.md#government-cloud-urls)Için Azure URL 'lerine bağlanabildiğinizden emin olun.


## <a name="set-up-the-appliance-for-hyper-v"></a>Hyper-V için gereci ayarlama

Hyper-V için gereci ayarlamak üzere Azure portal daraltılmış bir dosya indirir ve içeriği ayıklayabilir. Gereç Web uygulamasını başlatmak için PowerShell betiğini çalıştırın. Gereci ayarlayın ve ilk kez yapılandırın. Ardından, gereci Azure geçişi projesi ile kaydedersiniz.

### <a name="download-the-script"></a>Betiği indir

1.  **Geçiş hedefleri** > **sunucuları** > **Azure geçişi: Sunucu değerlendirmesi**' nde **keşfet**' e tıklayın.
2.   > Makinelerde **bulunan makineler****sanallaştırılmış mı?**, **Hyper-V ile Evet '** i seçin.
3.  Sıkıştırılmış dosyayı indirmek için **İndir**' e tıklayın. 


### <a name="verify-file-security"></a>Dosya güvenliğini doğrula

Dağıtmadan önce daraltılmış dosyanın güvenli olduğunu denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. Daraltılmış dosyanın karmasını oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-HyperV-USGov.zip MD5```

3. Oluşturulan karma değerleri doğrulayın. En son gereç sürümü için:

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | 717f8b9185f565006b5af0215ecadac
          

### <a name="run-the-script"></a>Betiği çalıştırın

Komut dosyası şu şekildedir:

- Aracıları ve bir Web uygulamasını kurar.
- Windows etkinleştirme hizmeti, IIS ve PowerShell ıSE dahil Windows rollerini kurar.
- Bir IIS yeniden yazılabilir modülünü indirir ve yükler. [Daha fazla bilgi edinin](https://www.microsoft.com/download/details.aspx?id=7435).
- Azure geçişi için kalıcı ayarlarla bir kayıt defteri anahtarını (HKLM) güncelleştirir.
- Günlük ve yapılandırma dosyalarını şu şekilde oluşturur:
    - **Yapılandırma dosyaları**:%ProgramData%\Microsoft Azure\Config
    - **Günlük dosyaları**:%ProgramData%\Microsoft Azure\Logs

Betiği çalıştırmak için:

1. Sıkıştırılmış dosyayı, gereci barındıracak makinedeki bir klasöre ayıklayın. Betiği mevcut bir Azure geçişi gereci üzerinde bir makinede çalıştırmayın emin olun.
2. Makinede, yönetici (yükseltilmiş) ayrıcalıklarla PowerShell 'i başlatın.
3. PowerShell dizinini, indirilen sıkıştırılmış dosyadan ayıklanan içerikleri içeren klasör olarak değiştirin.
4. **AzureMigrateInstaller. ps1**betiğini aşağıdaki gibi çalıştırın:``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-HyperV-USGov>AzureMigrateInstaller.ps1 ``` 
5. Betik başarıyla çalıştıktan sonra gereci ayarlayabilmeniz için gereç Web uygulamasını başlatır. Herhangi bir sorunla karşılaşırsanız, C:\ProgramData\Microsoft Azure\Logs\ AzureMigrateScenarioInstaller_<em>timestamp</em>. log konumundaki betik günlüklerini gözden geçirin.

### <a name="verify-access"></a>Erişimi doğrulama

Gerecin [kamu bulutları](migrate-appliance.md#government-cloud-urls)Için Azure URL 'lerine bağlanabildiğinizden emin olun.


## <a name="set-up-the-appliance-for-physical-servers"></a>Fiziksel sunucular için gereci ayarlama

VMware için gereci ayarlamak üzere Azure portal daraltılmış bir dosya indirir ve içeriği ayıklayabilir. Gereç Web uygulamasını başlatmak için PowerShell betiğini çalıştırın. Gereci ayarlayın ve ilk kez yapılandırın. Ardından, gereci Azure geçişi projesi ile kaydedersiniz.

### <a name="download-the-script"></a>Betiği indir

1.  **Geçiş hedefleri** > **sunucuları** > **Azure geçişi: Sunucu değerlendirmesi**' nde **keşfet**' e tıklayın.
2.   > Makinelerde **bulunan makineler****sanallaştırılmış mi?**, **sanallaştırılmamış/diğer**' i seçin.
3.  Sıkıştırılmış dosyayı indirmek için **İndir**' e tıklayın. 


### <a name="verify-file-security"></a>Dosya güvenliğini doğrula

Dağıtmadan önce daraltılmış dosyanın güvenli olduğunu denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. Daraltılmış dosyanın karmasını oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-USGov.zip MD5```

3. Oluşturulan karma değerleri doğrulayın. En son gereç sürümü için:

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | f81c155fc4a1409901caea948713913f
          

### <a name="run-the-script"></a>Betiği çalıştırın

Komut dosyası şu şekildedir:

- Aracıları ve bir Web uygulamasını kurar.
- Windows etkinleştirme hizmeti, IIS ve PowerShell ıSE dahil Windows rollerini kurar.
- Bir IIS yeniden yazılabilir modülünü indirir ve yükler. [Daha fazla bilgi edinin](https://www.microsoft.com/download/details.aspx?id=7435).
- Azure geçişi için kalıcı ayarlarla bir kayıt defteri anahtarını (HKLM) güncelleştirir.
- Günlük ve yapılandırma dosyalarını şu şekilde oluşturur:
    - **Yapılandırma dosyaları**:%ProgramData%\Microsoft Azure\Config
    - **Günlük dosyaları**:%ProgramData%\Microsoft Azure\Logs

Betiği çalıştırmak için:

1. Sıkıştırılmış dosyayı, gereci barındıracak makinedeki bir klasöre ayıklayın. Betiği mevcut bir Azure geçişi gereci üzerinde bir makinede çalıştırmayın emin olun.
2. Makinede, yönetici (yükseltilmiş) ayrıcalıklarla PowerShell 'i başlatın.
3. PowerShell dizinini, indirilen sıkıştırılmış dosyadan ayıklanan içerikleri içeren klasör olarak değiştirin.
4. **AzureMigrateInstaller. ps1**betiğini aşağıdaki gibi çalıştırın:``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-Server-USGov>AzureMigrateInstaller.ps1 ```
5. Betik başarıyla çalıştıktan sonra gereci ayarlayabilmeniz için gereç Web uygulamasını başlatır. Herhangi bir sorunla karşılaşırsanız, C:\ProgramData\Microsoft Azure\Logs\ AzureMigrateScenarioInstaller_<em>timestamp</em>. log konumundaki betik günlüklerini gözden geçirin.

### <a name="verify-access"></a>Erişimi doğrulama

Gerecin [kamu bulutları](migrate-appliance.md#government-cloud-urls)Için Azure URL 'lerine bağlanabildiğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Gereci dağıttıktan sonra, ilk kez yapılandırmanız ve Azure geçişi projesine kaydetmeniz gerekir.

- [VMware](how-to-set-up-appliance-vmware.md#configure-the-appliance)için gereç ayarlayın.
- [Hyper-V](how-to-set-up-appliance-hyper-v.md#configure-the-appliance)için gereci ayarlayın.
- [Fiziksel sunucular](how-to-set-up-appliance-physical.md)için gereç ayarlayın.