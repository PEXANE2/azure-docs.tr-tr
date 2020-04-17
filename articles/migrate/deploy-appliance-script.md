---
title: Komut dosyasıyla Azure Geçiş cihazı ayarlama
description: Komut dosyasıyla Azure Geçiş cihazını nasıl ayarlayamanızı öğrenin
ms.topic: article
ms.date: 04/16/2020
ms.openlocfilehash: faed7f96ea8c1850af5523d35f9f891011a48df8
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537721"
---
# <a name="set-up-an-appliance-with-a-script"></a>Komut dosyası olan bir cihaz ayarlama

Bu makalede, VMware VM'ler ve Hyper-VM'ler için PowerShell yükleyici komut dosyası kullanarak [Azure Geçir cihazının](deploy-appliance.md) nasıl ayarlanır. Cihazı fiziksel sunucular için kurmak istiyorsanız, [bu makaleyi inceleyin.](how-to-set-up-appliance-physical.md)


Cihazı birkaç yöntem kullanarak dağıtabilirsiniz:


- VMware VM 'ler (OVA) veya Hyper-V VM'ler (VHD) için şablon kullanma.
- Komut dosyası kullanarak. Bu, bu makalede açıklanan yöntemdir. Komut dosyası şunları sağlar:
    - VMware VM'lerin değerlendirilmesi ve aracısız geçişi için ova şablonu kullanarak cihazı kurmaya alternatif.
    - Hyper-V VM'lerin değerlendirilmesi ve geçişi için vhd şablonu kullanarak cihazı kurmaya alternatif.
    - Fiziksel sunucuların (veya fiziksel sunucu olarak geçirmek istediğiniz VM'lerin) değerlendirilmesi için komut dosyası, cihazı ayarlamak için tek yöntemdir.
    - Cihazı Azure Kamu'da dağıtmanın bir yolu.


Cihazı oluşturduktan sonra Azure Geçiş'e bağlanabildiğinizi doğrularsınız. Daha sonra cihazı ilk kez yapılandırın ve Azure Geçiş projesine kaydettirin.


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
    - Genel bulut için örnek kullanım:```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256```
    - Devlet bulutu için örnek kullanım:```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-VMWare-USGov.zip```

3. Oluşturulan karma değerleri doğrulayın:

    - Genel bulut için (en son cihaz sürümü için):

        **Algoritma** | **Karma değeri**
          --- | ---
          MD5 | 1e92ede3e87c03bd148e56a708cdd33f
          SHA256 | a3fa78edc8ff8aff9ab5ae66be1b64e6de7b9f475b6542beef114b20bfdac3c

    - Azure yönetimi için (en son cihaz sürümü için):

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
4. **AzureMigrateInstaller.ps1**komut dosyasını aşağıdaki gibi çalıştırın:
    - Genel bulut için:``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 -scenario VMware ```
    - Azure Kamu için:``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-VMWare-USGov>AzureMigrateInstaller.ps1 ```
   
5. Komut dosyası başarılı bir şekilde çalıştırdıktan sonra, cihazı kurabilmeniz için cihaz web uygulamasını başlatir. Herhangi bir sorunla karşılaşırsanız, C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log adresindeki komut dosyası günlüklerini gözden geçirin.

### <a name="verify-access"></a>Erişimi doğrulama

Cihazın [genel](migrate-appliance.md#public-cloud-urls) ve [devlet bulutları] için Azure URL'lerine bağlanabilmesini unutmayın (geçiş-appliance.md#government-cloud-urls).


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
    - Genel bulut için örnek kullanım:```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256```
    - Devlet bulutu için örnek kullanım:```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-HyperV-USGov.zip MD5```

3. Oluşturulan karma değerleri doğrulayın:

    - Genel bulut için (en son cihaz sürümü için):

        **Algoritma** | **Karma değeri**
          --- | ---
          MD5 | 1e92ede3e87c03bd148e56a708cdd33f
          SHA256 | a3fa78edc8ff8aff9ab5ae66be1b64e6de7b9f475b6542beef114b20bfdac3c

    - Azure yönetimi için (en son cihaz sürümü için):

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
4. **AzureMigrateInstaller.ps1**komut dosyasını aşağıdaki gibi çalıştırın:
    - Genel bulut için:``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 -scenario Hyperv ```
    - Azure Kamu için:``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-HyperV-USGov>AzureMigrateInstaller.ps1 ```
   
5. Komut dosyası başarılı bir şekilde çalıştırdıktan sonra, cihazı kurabilmeniz için cihaz web uygulamasını başlatir. Herhangi bir sorunla karşılaşırsanız, C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log adresindeki komut dosyası günlüklerini gözden geçirin.

### <a name="verify-access"></a>Erişimi doğrulama

Cihazın [genel](migrate-appliance.md#public-cloud-urls) ve [devlet bulutları] için Azure URL'lerine bağlanabilmesini unutmayın (geçiş-appliance.md#government-cloud-urls).



## <a name="next-steps"></a>Sonraki adımlar

Cihazı şablonla veya fiziksel sunucularla ayarlama hakkında daha fazla bilgi edinmek için şu makaleleri inceleyin:

- [Cihazı VMware](how-to-set-up-appliance-vmware.md#configure-the-appliance)için ayarlayın.
- [Hyper-V](how-to-set-up-appliance-hyper-v.md#configure-the-appliance)için cihazı ayarlayın.
- Cihazı [fiziksel sunucular](how-to-set-up-appliance-physical.md)için ayarlayın.