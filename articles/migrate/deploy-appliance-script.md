---
title: Bir komut dosyası ile Azure geçişi gereci ayarlama
description: Bir komut dosyası ile Azure geçişi gereci ayarlamayı öğrenin
ms.topic: article
ms.date: 04/16/2020
ms.openlocfilehash: 47b6b35e62d484b4d7a33f6a53796c59e01817fe
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86102455"
---
# <a name="set-up-an-appliance-with-a-script"></a>Komut dosyası ile gereç ayarlama

VMware VM 'Leri ve Hyper-V VM 'lerinin değerlendirmesi/geçirilmesi için bir [Azure geçiş](./migrate-appliance-architecture.md) gereci oluşturmak için bu makaleyi izleyin. Bir gereç oluşturmak için bir komut dosyası çalıştırın ve Azure 'a bağlanabildiğini doğrulayın. 

VMware ve Hyper-V VM 'Leri için bir betik kullanarak veya Azure portal indirtiğiniz bir şablon kullanarak gereci dağıtabilirsiniz. İndirilen şablonu kullanarak bir VM oluştursanız, betik kullanmak faydalıdır.

- Bir şablon kullanmak için [VMware](tutorial-prepare-vmware.md) veya [Hyper-V](tutorial-prepare-hyper-v.md)öğreticilerini izleyin.
- Fiziksel sunucular için bir gereç ayarlamak üzere yalnızca bir komut dosyası kullanabilirsiniz. [Bu makaleyi](how-to-set-up-appliance-physical.md)izleyin.
- Azure Kamu bulutunda bir gereç ayarlamak için [Bu makaleyi](deploy-appliance-script-government.md)izleyin.

## <a name="prerequisites"></a>Önkoşullar

Betik, mevcut bir fiziksel makineye veya VM 'ye Azure geçişi gereci ayarlar.

- Gereç görevi görecek makinenin aşağıdaki donanım ve işletim sistemi gereksinimlerini karşılaması gerekir:

Senaryo | Gereksinimler
--- | ---
VMware | Windows Server 2016, 32 GB bellek, sekiz vCPU, yaklaşık 80 GB disk depolaması
Hyper-V | 16 GB bellek, 8 GB disk depolaması 80 etrafında sekiz vCPU ile Windows Server 2016
- Makinenin aynı zamanda bir dış sanal anahtar olması gerekir. Statik veya dinamik bir IP adresi ve internet erişimi gerektirir.
- Gereci dağıtmadan önce, [VMware VM](migrate-appliance.md#appliance---vmware)'leri, [Hyper-V VM 'leri](migrate-appliance.md#appliance---hyper-v)için ayrıntılı gereç gereksinimlerini gözden geçirin.
- Betiği mevcut bir Azure geçiş gereci üzerinde çalıştırmayın.

## <a name="set-up-the-appliance-for-vmware"></a>VMware için gereci ayarlama

VMware için gereci ayarlamak için, AzureMigrateInstaller.zip adlı daraltılmış dosyayı [buradan](https://go.microsoft.com/fwlink/?linkid=2105112)indirin ve içeriği ayıklayın. Gereç Web uygulamasını başlatmak için PowerShell betiğini çalıştırın. Gereci ayarlayın ve ilk kez yapılandırın. Ardından, gereci Azure geçişi projesi ile kaydedersiniz.


### <a name="verify-file-security"></a>Dosya güvenliğini doğrula

Dağıtmadan önce daraltılmış dosyanın güvenli olduğunu denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. Daraltılmış dosyanın karmasını oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256```
3. Azure genel bulutu için en son gereç sürümünü ve betiğini doğrulayın:

    **Algoritma** | **İndir** | **SHA256**
    --- | --- | ---
    VMware (63,1 MB) | [En son sürüm](https://go.microsoft.com/fwlink/?linkid=2105112) | 0a27adf13cc5755e4b23df0c05732c6ac08d1fe8850567cb57c9906fbc3b85a0



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
4. Komut dosyası **AzureMigrateInstaller.ps1**aşağıdaki gibi çalıştırın:

    ``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 -scenario VMware ```
   
5. Betik başarıyla çalıştıktan sonra gereci ayarlayabilmeniz için gereç Web uygulamasını başlatır. Herhangi bir sorunla karşılaşırsanız, C:\ProgramData\Microsoft Azure\Logs\ AzureMigrateScenarioInstaller_<em>timestamp</em>. log konumundaki betik günlüklerini gözden geçirin.

### <a name="verify-access"></a>Erişimi doğrulama

Gerecin [genel](migrate-appliance.md#public-cloud-urls) bulut Için Azure URL 'lerine bağlanabildiğinizden emin olun.

## <a name="set-up-the-appliance-for-hyper-v"></a>Hyper-V için gereci ayarlama

Hyper-V için gereci ayarlamak için, AzureMigrateInstaller.zip adlı daraltılmış dosyayı [buradan](https://go.microsoft.com/fwlink/?linkid=2105112)indirin ve içeriği ayıklayın. Gereç Web uygulamasını başlatmak için PowerShell betiğini çalıştırın. Gereci ayarlayın ve ilk kez yapılandırın. Ardından, gereci Azure geçişi projesi ile kaydedersiniz.


### <a name="verify-file-security"></a>Dosya güvenliğini doğrula

Dağıtmadan önce daraltılmış dosyanın güvenli olduğunu denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. Daraltılmış dosyanın karmasını oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256```

3. Azure genel bulutu için en son gereç sürümünü ve betiğini doğrulayın:

    **Senaryo** | **İndir** | **SHA256**
    --- | --- | ---
    Hyper-V (63,1 MB) | [En son sürüm](https://go.microsoft.com/fwlink/?linkid=2105112) |  572be425ea0aca69a9aa8658c950bc319b2bdbeb93b440577264500091c846a1

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
4. Komut dosyası **AzureMigrateInstaller.ps1**aşağıdaki gibi çalıştırın:``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 -scenario Hyperv ```
   
5. Betik başarıyla çalıştıktan sonra gereci ayarlayabilmeniz için gereç Web uygulamasını başlatır. Herhangi bir sorunla karşılaşırsanız, C:\ProgramData\Microsoft Azure\Logs\ AzureMigrateScenarioInstaller_<em>timestamp</em>. log konumundaki betik günlüklerini gözden geçirin.

### <a name="verify-access"></a>Erişimi doğrulama

Gerecin [genel](migrate-appliance.md#public-cloud-urls) bulut Için Azure URL 'lerine bağlanabildiğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Gereci dağıttıktan sonra, ilk kez yapılandırmanız ve Azure geçişi projesine kaydetmeniz gerekir.

- [VMware](how-to-set-up-appliance-vmware.md#configure-the-appliance)için gereç ayarlayın.
- [Hyper-V](how-to-set-up-appliance-hyper-v.md#configure-the-appliance)için gereci ayarlayın.
