---
title: Azure Kamu'da bir Azure Geçiş cihazı ayarlama
description: Azure Kamu'da bir Azure Geçiş cihazını nasıl ayarlayamaize ceretleri öğrenin
ms.topic: article
ms.date: 04/16/2020
ms.openlocfilehash: fd97161ffa075a6165ea963ef80bfabf8904576e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726741"
---
# <a name="set-up-an-appliance-in-azure-government"></a>Azure Kamu'da bir cihaz ayarlama 

VMware VM'ler, Hyper-V VM'ler ve fiziksel sunucular için bir Azure Devlet bulutuna [bir Azure Geçiş cihazı](deploy-appliance.md) dağıtmak için bu makaleyi izleyin. Cihazı oluşturmak için bir komut dosyası çalıştırın ve Azure'a bağlanabildiğinizi doğrularsınız. Genel bulutta bir cihaz kurmak istiyorsanız, [bu makaleyi](deploy-appliance-script.md)izleyin.


> [!NOTE]
> Bir cihazı şablon kullanarak dağıtma seçeneği (VMware VM'ler ve Hyper-V VM'ler için) Azure Kamu'da desteklenmez.


## <a name="prerequisites"></a>Ön koşullar

Komut dosyası, Azure Geçir cihazını varolan bir fiziksel makinede veya VM'de ayarlar.

- Cihaz olarak hareket edecek makine, 32 GB bellek, sekiz vCPUs, yaklaşık 80 GB disk depolama ve harici sanal anahtar ile Windows Server 2016'yı çalıştırıyor olmalıdır. Statik veya dinamik bir IP adresi ve internet erişimi gerektirir.
- Cihazı dağıtmadan [önce, VMware VM'ler,](migrate-appliance.md#appliance---vmware) [Hyper-V VM'ler](migrate-appliance.md#appliance---hyper-v)ve [fiziksel sunucular](migrate-appliance.md#appliance---physical)için ayrıntılı cihaz gereksinimlerini gözden geçirin.
- Komut dosyasını varolan bir Azure Geçiş cihazında çalıştırmayın.

## <a name="set-up-the-appliance-for-vmware"></a>Cihazı VMware için ayarlama

VMware için cihazı ayarlamak için Azure portalından sıkıştırılmış bir dosya indirir ve içeriğini ayıklarsınız. Cihaz web uygulamasını başlatmak için PowerShell komut dosyasını çalıştırın. Cihazı ayarlayın ve ilk kez yapılandırın. Ardından, cihazı Azure Geçiş projesine kaydettirin.

### <a name="download-the-script"></a>Komut dosyasını indirin

1.  **Geçiş Hedefleri** > **Sunucularında** > **Azure Geçir: Sunucu Değerlendirmesi**, **Keşfet'i**tıklatın.
2.  **Discover makinelerde** > **makineleri sanallaştırılmış mı?**, **Evet, VMWare vSphere hypervisor ile**seçin.
3.  Sıkıştırılmış dosyayı indirmek için **İndir'i**tıklatın. 


### <a name="verify-file-security"></a>Dosya güvenliğini doğrulama

Dağıtmadan önce sıkıştırılabilen dosyanın güvenli olup olmadığını kontrol edin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. Sıkıştırılanması gereken dosya için karma oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-VMWare-USGov.zip MD5```

3. Oluşturulan karma değerleri doğrulayın. En son cihaz sürümü için:

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | 6316bcc8bc932204295bfe33f4be3949
          

### <a name="run-the-script"></a>Betiği çalıştırın

Komut dosyasının yaptığı şey şu:

- Aracıları ve bir web uygulamasını yükler.
- Windows Etkinleştirme Hizmeti, IIS ve PowerShell ISE dahil olmak üzere Windows rollerini yükler.
- IIS yeniden yazılabilir bir modül indirir ve yükler. [Daha fazla bilgi edinin](https://www.microsoft.com/download/details.aspx?id=7435).
- Azure Geçiş için kalıcı ayarlarla bir kayıt defteri anahtarını (HKLM) güncelleştirir.
- Aşağıdaki gibi günlük ve yapılandırma dosyaları oluşturur:
    - **Config Files**: %ProgramData%\Microsoft Azure\Config
    - **Günlük Dosyaları**: %ProgramData%\Microsoft Azure\Günlükler

Betiği çalıştırmak için:

1. Sıkıştırılmış dosyayı makinede cihazı barındıracak bir klasöre ayıklayın. Komut dosyasını varolan bir Azure Geçir cihazında bir makinede çalıştırmadığınızdan emin olun.
2. PowerShell'i yönetici (yüksek) ayrıcalıklarla makinede başlatın.
3. PowerShell dizinini indirilen sıkıştırılmış dosyadan çıkarılan içeriği içeren klasörle değiştirin.
4. **AzureMigrateInstaller.ps1**komut dosyasını aşağıdaki gibi çalıştırın:``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-VMWare-USGov>AzureMigrateInstaller.ps1 ```
5. Komut dosyası başarılı bir şekilde çalıştırdıktan sonra, cihazı kurabilmeniz için cihaz web uygulamasını başlatir. Herhangi bir sorunla karşılaşırsanız, C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log adresindeki komut dosyası günlüklerini gözden geçirin.

### <a name="verify-access"></a>Erişimi doğrulama

Cihazın [devlet bulutları](migrate-appliance.md#government-cloud-urls)için Azure URL'lerine bağlanabileceğinden emin olun.


## <a name="set-up-the-appliance-for-hyper-v"></a>Cihazı Hyper-V için ayarlayın

Hyper-V cihazını ayarlamak için Azure portalından sıkıştırılmış bir dosya indirir ve içeriğini ayıklarsınız. Cihaz web uygulamasını başlatmak için PowerShell komut dosyasını çalıştırın. Cihazı ayarlayın ve ilk kez yapılandırın. Ardından, cihazı Azure Geçiş projesine kaydettirin.

### <a name="download-the-script"></a>Komut dosyasını indirin

1.  **Geçiş Hedefleri** > **Sunucularında** > **Azure Geçir: Sunucu Değerlendirmesi**, **Keşfet'i**tıklatın.
2.  **Discover makinelerinde** >  **Yes, with Hyper-V****makineleriniz sanallaştırıldı mı?**
3.  Sıkıştırılmış dosyayı indirmek için **İndir'i**tıklatın. 


### <a name="verify-file-security"></a>Dosya güvenliğini doğrulama

Dağıtmadan önce sıkıştırılabilen dosyanın güvenli olup olmadığını kontrol edin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. Sıkıştırılanması gereken dosya için karma oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-HyperV-USGov.zip MD5```

3. Oluşturulan karma değerleri doğrulayın. En son cihaz sürümü için:

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | 717f8b9185f565006b5aff0215ecadac
          

### <a name="run-the-script"></a>Betiği çalıştırın

Komut dosyasının yaptığı şey şu:

- Aracıları ve bir web uygulamasını yükler.
- Windows Etkinleştirme Hizmeti, IIS ve PowerShell ISE dahil olmak üzere Windows rollerini yükler.
- IIS yeniden yazılabilir bir modül indirir ve yükler. [Daha fazla bilgi edinin](https://www.microsoft.com/download/details.aspx?id=7435).
- Azure Geçiş için kalıcı ayarlarla bir kayıt defteri anahtarını (HKLM) güncelleştirir.
- Aşağıdaki gibi günlük ve yapılandırma dosyaları oluşturur:
    - **Config Files**: %ProgramData%\Microsoft Azure\Config
    - **Günlük Dosyaları**: %ProgramData%\Microsoft Azure\Günlükler

Betiği çalıştırmak için:

1. Sıkıştırılmış dosyayı makinede cihazı barındıracak bir klasöre ayıklayın. Komut dosyasını varolan bir Azure Geçir cihazında bir makinede çalıştırmadığınızdan emin olun.
2. PowerShell'i yönetici (yüksek) ayrıcalıklarla makinede başlatın.
3. PowerShell dizinini indirilen sıkıştırılmış dosyadan çıkarılan içeriği içeren klasörle değiştirin.
4. **AzureMigrateInstaller.ps1**komut dosyasını aşağıdaki gibi çalıştırın:``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-HyperV-USGov>AzureMigrateInstaller.ps1 ``` 
5. Komut dosyası başarılı bir şekilde çalıştırdıktan sonra, cihazı kurabilmeniz için cihaz web uygulamasını başlatir. Herhangi bir sorunla karşılaşırsanız, C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log adresindeki komut dosyası günlüklerini gözden geçirin.

### <a name="verify-access"></a>Erişimi doğrulama

Cihazın [devlet bulutları](migrate-appliance.md#government-cloud-urls)için Azure URL'lerine bağlanabileceğinden emin olun.


## <a name="set-up-the-appliance-for-physical-servers"></a>Cihazı fiziksel sunucular için ayarlama

VMware için cihazı ayarlamak için Azure portalından sıkıştırılmış bir dosya indirir ve içeriğini ayıklarsınız. Cihaz web uygulamasını başlatmak için PowerShell komut dosyasını çalıştırın. Cihazı ayarlayın ve ilk kez yapılandırın. Ardından, cihazı Azure Geçiş projesine kaydettirin.

### <a name="download-the-script"></a>Komut dosyasını indirin

1.  **Geçiş Hedefleri** > **Sunucularında** > **Azure Geçir: Sunucu Değerlendirmesi**, **Keşfet'i**tıklatın.
2.  **Discover makinelerinde** >  **Not virtualized/Other****makineleriniz sanallaştırıldı mı?**
3.  Sıkıştırılmış dosyayı indirmek için **İndir'i**tıklatın. 


### <a name="verify-file-security"></a>Dosya güvenliğini doğrulama

Dağıtmadan önce sıkıştırılabilen dosyanın güvenli olup olmadığını kontrol edin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. Sıkıştırılanması gereken dosya için karma oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-USGov.zip MD5```

3. Oluşturulan karma değerleri doğrulayın. En son cihaz sürümü için:

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | f81c155fc4a1409901caea948713913f
          

### <a name="run-the-script"></a>Betiği çalıştırın

Komut dosyasının yaptığı şey şu:

- Aracıları ve bir web uygulamasını yükler.
- Windows Etkinleştirme Hizmeti, IIS ve PowerShell ISE dahil olmak üzere Windows rollerini yükler.
- IIS yeniden yazılabilir bir modül indirir ve yükler. [Daha fazla bilgi edinin](https://www.microsoft.com/download/details.aspx?id=7435).
- Azure Geçiş için kalıcı ayarlarla bir kayıt defteri anahtarını (HKLM) güncelleştirir.
- Aşağıdaki gibi günlük ve yapılandırma dosyaları oluşturur:
    - **Config Files**: %ProgramData%\Microsoft Azure\Config
    - **Günlük Dosyaları**: %ProgramData%\Microsoft Azure\Günlükler

Betiği çalıştırmak için:

1. Sıkıştırılmış dosyayı makinede cihazı barındıracak bir klasöre ayıklayın. Komut dosyasını varolan bir Azure Geçir cihazında bir makinede çalıştırmadığınızdan emin olun.
2. PowerShell'i yönetici (yüksek) ayrıcalıklarla makinede başlatın.
3. PowerShell dizinini indirilen sıkıştırılmış dosyadan çıkarılan içeriği içeren klasörle değiştirin.
4. **AzureMigrateInstaller.ps1**komut dosyasını aşağıdaki gibi çalıştırın:``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-Server-USGov>AzureMigrateInstaller.ps1 ```
5. Komut dosyası başarılı bir şekilde çalıştırdıktan sonra, cihazı kurabilmeniz için cihaz web uygulamasını başlatir. Herhangi bir sorunla karşılaşırsanız, C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log adresindeki komut dosyası günlüklerini gözden geçirin.

### <a name="verify-access"></a>Erişimi doğrulama

Cihazın [devlet bulutları](migrate-appliance.md#government-cloud-urls)için Azure URL'lerine bağlanabileceğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Cihazı dağıttıktan sonra, cihazı ilk kez yapılandırmanız ve Azure Geçiş projesine kaydetmeniz gerekir.

- [Cihazı VMware](how-to-set-up-appliance-vmware.md#configure-the-appliance)için ayarlayın.
- [Hyper-V](how-to-set-up-appliance-hyper-v.md#configure-the-appliance)için cihazı ayarlayın.
- Cihazı [fiziksel sunucular](how-to-set-up-appliance-physical.md)için ayarlayın.