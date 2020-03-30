---
title: Komut dosyasıyla Azure Geçiş cihazı ayarlama
description: Komut dosyasıyla Azure Geçiş cihazını nasıl ayarlayamanızı öğrenin
ms.topic: article
ms.date: 03/23/2020
ms.openlocfilehash: bf8d7148f685d4ac6a5f33603020a0503b0c62e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337694"
---
# <a name="set-up-an-appliance-with-a-script"></a>Komut dosyası olan bir cihaz ayarlama

Bu makalede, PowerShell yükleyici komut dosyası kullanarak [Azure Geçir cihazının](deploy-appliance.md) nasıl ayarlanır.

Komut dosyası şunları sağlar:
- VMware VM'lerin değerlendirilmesi ve aracısız geçişi için ova şablonu kullanarak cihazı kurmaya alternatif.
- Hyper-V VM'lerin değerlendirilmesi ve geçişi için vhd şablonu kullanarak cihazı kurmaya alternatif.
- Fiziksel sunucuların (veya fiziksel sunucu olarak geçirmek istediğiniz VM'lerin) değerlendirilmesi için komut dosyası, cihazı ayarlamak için tek yöntemdir.

## <a name="prerequisites"></a>Ön koşullar

Komut dosyası, Azure Geçir cihazını varolan bir fiziksel makinede veya VM'de ayarlar.

- Cihaz olarak hareket edecek makine, 32 GB bellek, sekiz vCPUs, yaklaşık 80 GB disk depolama ve harici sanal anahtar ile Windows Server 2016'yı çalıştırıyor olmalıdır. Statik veya dinamik bir IP adresi ve internet erişimi gerektirir.
- Cihazı dağıtmadan [önce, VMware VM'ler,](migrate-appliance.md#appliance---vmware) [Hyper-V VM'ler](migrate-appliance.md#appliance---hyper-v)ve [fiziksel sunucular](migrate-appliance.md#appliance---physical)için ayrıntılı cihaz gereksinimlerini gözden geçirin.
- Komut dosyasını varolan bir Azure Geçiş cihazında çalıştırmayın.


## <a name="download-the-script"></a>Komut dosyasını indirin

1. Azure Geçir cihazı olarak hareket edecek makineyi/VM'yi bulun.
2. Makinede aşağıdakileri yapın:

    - Cihazı VMware VM veya Hyper-V VM'lerle kullanmak için yükleyici komut dosyasını ve MSI'ları içeren sıkıştırılmış klasörü [indirin.](https://go.microsoft.com/fwlink/?linkid=2105112)
    - Cihazı fiziksel sunucularla kullanmak için, bu öğreticide açıklandığı gibi komut dosyasını Azure Geçiş portalından [indirin.](tutorial-assess-physical.md#set-up-the-appliance)

## <a name="verify-file-security"></a>Dosya güvenliğini doğrulama

Dağıtmadan önce sıkıştırılabilen dosyanın güvenli olup olmadığını kontrol edin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. Sıkıştırılanması gereken dosya için karma oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256```

3. Oluşturulan karma değerlerinin bu ayarlarla eşleştin (en son cihaz sürümü için):

    **Algoritma** | **Karma değeri**
      --- | ---
      MD5 | 1e92ede3e87c03bd148e56a708cdd33f
      SHA256 | a3fa78edc8ff8aff9ab5ae66be1b64e6de7b9f475b6542beef114b20bfdac3c

## <a name="run-the-script"></a>Betiği çalıştırın

Komut dosyasının yaptığı şey şu:

- Aracıları ve bir web uygulamasını yükler.
- Windows Etkinleştirme Hizmeti, IIS ve PowerShell ISE dahil olmak üzere Windows rollerini yükler.
- IIS yeniden yazılabilir bir modül indirir ve yükler. [Daha fazla bilgi edinin](https://www.microsoft.com/download/details.aspx?id=7435).
- Azure Geçiş için kalıcı ayarlarla bir kayıt defteri anahtarını (HKLM) güncelleştirir.
- Aşağıdaki gibi günlük ve yapılandırma dosyaları oluşturur:
    - **Config Files**: %ProgramData%\Microsoft Azure\Config
    - **Günlük Dosyaları**: %ProgramData%\Microsoft Azure\Günlükler

Betiği çalıştırmak için:

1. Sıkıştırılmış dosyayı makinede cihazı barındıracak bir klasöre ayıklayın.
2. PowerShell'i yönetici (yüksek) ayrıcalıklarla makinede başlatın.
3. PowerShell dizinini indirilen sıkıştırılmış dosyadan çıkarılan içeriği içeren klasörle değiştirin.
4. **AzureMigrateInstaller.ps1** komut dosyasını aşağıdaki gibi çalıştırın:

    - VMware için: 
        ```
        PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 -scenario VMware
        ```
    - Hyper-V için:
        ```
        PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 -scenario Hyperv
        ```
    - Fiziksel sunucular için:
        ```
        PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1
        ```      
5. Komut dosyası başarılı bir şekilde çalıştırdıktan sonra, cihazı kurabilmeniz için cihaz web uygulamasını başlatir. Herhangi bir sorunla karşılaşırsanız, Komut dosyası günlüklerini C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log adresinde görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Komut dosyasını kullanarak cihazı kurduktan sonra aşağıdaki yönergeleri izleyin:

- [Cihazı VMware](how-to-set-up-appliance-vmware.md#configure-the-appliance)için ayarlayın.
- [Hyper-V](how-to-set-up-appliance-hyper-v.md#configure-the-appliance)için cihazı ayarlayın.
- Cihazı [fiziksel sunucular](how-to-set-up-appliance-physical.md)için ayarlayın.