---
title: Linux VM'leri için Azure Disk Şifrelemesi'ni etkinleştirme
description: Bu makalede, Linux VM'ler için Microsoft Azure Disk Şifrelemesi etkinleştirme ile ilgili yönergeler verilmektedir.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: d058ff5f9863642f73725db3472c942161447f25
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548445"
---
# <a name="azure-disk-encryption-for-linux-vms"></a>Linux VM'leri için Azure Disk Şifrelemesi 

Azure Disk Şifrelemesi verilerinizi koruyarak kurumsal güvenlik ve uyumluluk taahhütlerinizi yerine getirmenize yardımcı olur. Azure sanal makinelerinin (VM) işletim sistemi ve veri diskleri için birim şifreleme sağlamak için Linux'un [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) özelliğini kullanır ve disk şifreleme anahtarlarını ve sırlarını kontrol ve yönetmenize yardımcı olmak için [Azure Key Vault](../../key-vault/index.yml) ile tümleştirilir. 

[Azure Güvenlik Merkezi](../../security-center/index.yml)kullanıyorsanız, şifrelenmemiş VM'leriniz varsa uyarılanırsınız. Uyarılar Yüksek Önem derecesi olarak gösterir ve öneri bu VM'leri şifrelemektir.

![Azure Güvenlik Merkezi disk şifreleme uyarısı](media/disk-encryption/security-center-disk-encryption-fig1.png)

> [!WARNING]
> - VM'yi şifrelemek için Azure AD ile Azure Disk Şifreleme'yi daha önce kullandıysanız, VM'nizi şifrelemek için bu seçeneği kullanmaya devam etmelisiniz. Ayrıntılar için [Azure AD (önceki sürüm) ile Azure Disk](disk-encryption-overview-aad.md) Şifrelemesi'ne bakın. 
> - Bazı öneriler veri, ağ veya bilgi işlem kaynağı kullanımını artırarak ek lisans veya abonelik maliyetlerine neden olabilir. Desteklenen bölgelerde Azure'da kaynak oluşturmak için geçerli bir etkin Azure aboneliğiniz olmalıdır.
> - Şu anda Nesil 2 VM'ler Azure Disk Şifreleme'yi desteklemiyor. Ayrıntılar [için Azure'da Nesil 2 VM'ler için Destek'e](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) bakın.

Linux için Azure Disk Şifrelemesinin [temellerini, Azure CLI quickstart'ı](disk-encryption-cli-quickstart.md) ile Bir Linux VM Oluştur ile birkaç dakika içinde öğrenebilir ve Bir Linux [VM'yi Azure Powershell hızlı başlatma ile oluştur ve şifreleyebilirsiniz.](disk-encryption-powershell-quickstart.md)

## <a name="supported-vms-and-operating-systems"></a>Desteklenen VM'ler ve işletim sistemleri

### <a name="supported-vms"></a>Desteklenen VM'ler

Linux VM'ler [çeşitli boyutlarda](sizes.md)mevcuttur. Azure Disk Şifreleme, [Temel, A serisi SANAL Makinelerde](https://azure.microsoft.com/pricing/details/virtual-machines/series/)veya bu minimum bellek gereksinimlerini karşılamayan sanal makinelerde kullanılamaz:

| Sanal makine | Minimum bellek gereksinimi |
|--|--|
| Linux VM'leri yalnızca veri hacimlerini şifrelerken| 2 GB |
| Linux VM'leri hem veri hem de işletim sistemi birimlerini şifrelerken ve kök (/) dosya sistemi kullanımının 4 GB veya daha az olduğu durumlarda | 8 GB |
| Linux VM'leri hem veri hem de işletim sistemi birimlerini şifrelerken ve kök (/) dosya sistemi kullanımının 4 GB'dan büyük olduğu durumlarda | Kök dosya sistemi kullanımı * 2. Örneğin, 16 GB kök dosya sistemi kullanımı en az 32 GB RAM gerektirir |

Linux sanal makinelerde işletim sistemi disk şifreleme işlemi tamamlandıktan sonra, VM daha az bellekle çalışacak şekilde yapılandırılabilir. 

Azure Disk Şifrelemesi, birinci sınıf depolama alanına sahip VM'ler için de kullanılabilir.

Azure Disk [Şifreleme, Nesil 2 VM'lerde](generation-2.md#generation-1-vs-generation-2-capabilities)ve [Lsv2 serisi VM'lerde](../lsv2-series.md)kullanılamaz). Daha fazla özel durum için [bkz: Azure Disk Şifrelemesi: Desteklenmeyen senaryolar.](disk-encryption-linux.md#unsupported-scenarios)

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Azure Disk Şifreleme, azure [onaylı Linux dağıtımlarının](endorsed-distros.md)bir alt kümesinde desteklenir ve bu da tüm Linux sunucusu olası dağıtımlarının bir alt kümesidir.

![Azure Disk Şifrelemesini destekleyen Linux sunucu dağılımlarının Venn Diyagramı](./media/disk-encryption/ade-supported-distros.png)

Azure tarafından onaylanmayan Linux sunucu dağıtımları Azure Disk Şifrelemesini desteklemez; onaylananların yalnızca aşağıdaki dağıtımları ve sürümleri Azure Disk Şifrelemesini destekler:

| Linux dağıtımı | Sürüm | Şifreleme için desteklenen ses türü|
| --- | --- |--- |
| Ubuntu | 18.04| İşletim sistemi ve veri diski |
| Ubuntu | 16.04| İşletim sistemi ve veri diski |
| Ubuntu | 14.04.5</br>[Azure ayarlı çekirdek 4.15 veya daha sonra güncellendi](disk-encryption-troubleshooting.md) | İşletim sistemi ve veri diski |
| RHEL | 7.7 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL | 7.6 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL | 7,5 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL | 7.4 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL | 7.3 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL | 7.2 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL | 6.8 | Veri diski (aşağıdaki nota bakın) |
| RHEL | 6.7 | Veri diski (aşağıdaki nota bakın) |
| CentOS | 7.7 | İşletim sistemi ve veri diski |
| CentOS | 7.6 | İşletim sistemi ve veri diski |
| CentOS | 7,5 | İşletim sistemi ve veri diski |
| CentOS | 7.4 | İşletim sistemi ve veri diski |
| CentOS | 7.3 | İşletim sistemi ve veri diski |
| CentOS | 7.2n | İşletim sistemi ve veri diski |
| CentOS | 6.8 | Veri diski |
| openSUSE | 42.3 | Veri diski |
| SLES | 12-SP4 | Veri diski |
| SLES | 12-SP3 | Veri diski |

> [!NOTE]
> Yeni Azure Disk Şifreleme uygulaması, RHEL OS ve RHEL7 You-You-Go verileri için veri diski için desteklenir.  
>
> ADE ayrıca RHEL Bring-Your-Own-Subscription Gold Images için de desteklenir, ancak abonelik kaydedildikten **sonra.** Daha fazla bilgi için Bkz. [Red Hat Enterprise Linux Azure'da Kendi Aboneliğinizi Getir Altın Görüntüler](../workloads/redhat/byos.md#encrypt-red-hat-enterprise-linux-bring-your-own-subscription-gold-images)

## <a name="additional-vm-requirements"></a>Ek VM gereksinimleri

Azure Disk Şifreleme, dm-crypt ve vfat modüllerinin sistemde bulunmasını gerektirir. VFAT'ı varsayılan görüntüden kaldırmak veya devre dışı bırakmak, sistemin anahtar hacmini okumasını ve sonraki yeniden başlatmalarda disklerin kilidini açmak için gereken anahtarı almasını engeller. Vfat modüllerini sistemden kaldıran sistem sertleştirme adımları Azure Disk Şifrelemeile uyumlu değildir. 

Şifrelemeyi etkinleştirmeden önce, şifrelenecek veri disklerinin /etc/fstab'da düzgün bir şekilde listelenmiş olması gerekir. "/dev/sdX" biçimindeki aygıt adlarının, özellikle şifreleme uygulandıktan sonra, yeniden başlatmalarda aynı diskle ilişkilendirilmesine güvenilen bir aygıt adı olduğundan, bu giriş için kalıcı bir blok aygıt adı kullanın. Bu davranış hakkında daha fazla ayrıntı için bkz: [Sorun giderme Linux VM aygıt adı değişiklikleri](troubleshoot-device-names-problems.md)

/etc/fstab ayarlarının montaj için uygun şekilde yapılandırıldığından emin olun. Bu ayarları yapılandırmak için, mount -a komutunu çalıştırın veya VM'yi yeniden başlatın ve yeniden binme yi bu şekilde tetikle. Bu tamamlandığında, sürücünün hala takılı olup olmadığını doğrulamak için lsblk komutunun çıktısını kontrol edin. 
- /etc/fstab dosyası şifrelemeyi etkinleştirmeden önce sürücüyü düzgün monte etmiyorsa, Azure Disk Şifrelemesi sürücüyü düzgün monte edemez.
- Azure Disk Şifreleme işlemi, montaj bilgilerini /etc/fstab'dan çıkarıp şifreleme işleminin bir parçası olarak kendi yapılandırma dosyasına taşır. Veri sürücüsü şifrelemesi tamamlandıktan sonra /etc/fstab'tan girişin eksik olduğunu görmek için telaşlanmayın.
- Şifrelemeye başlamadan önce, monte edilmiş veri disklerine yazabilecek tüm hizmetleri ve işlemleri durdurup devre dışı bıraktığından emin olun, böylece yeniden başlatıldıktan sonra otomatik olarak yeniden başlatılmaz. Bunlar, bu bölümlerdeki dosyaları açık tutarak şifreleme yordamının yeniden monte etmesini önleyebilir ve şifrelemenin başarısızlığa neden olabilir. 
- Yeniden başlatıldıktan sonra, Azure Disk Şifreleme işleminin yeni şifrelenmiş diskleri takması zaman alacaktır. Yeniden başlatıldıktan sonra hemen kullanılamazlar. İşlemin, diğer işlemlerin erişebilmesi için kullanılabilir hale gelmeden önce şifrelemeli sürücüleri başlatmak, kilidini açmak ve sonra takmak için zamana ihtiyacı vardır. Bu işlem, sistem özelliklerine bağlı olarak yeniden başlatıldıktan sonra bir dakikadan fazla sürebilir.

Veri disklerini monte etmek ve gerekli /etc/fstab girişlerini oluşturmak için kullanılabilecek komutların bir örneği [Azure Disk Şifreleme ön koşulları CLI komut dosyasında](https://github.com/ejarvi/ade-cli-getting-started) (satır 244-248) ve Azure Disk Şifreleme [önkoşullar PowerShell komut dosyasında](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)bulunabilir. 

## <a name="networking-requirements"></a>Ağ gereksinimleri

Azure Disk Şifreleme özelliğini etkinleştirmek için Linux VM'lerinin aşağıdaki ağ bitiş noktası yapılandırma gereksinimlerini karşılaması gerekir:
  - Anahtar kasanıza bağlanmak için bir belirteç elde etmek için Linux VM'nin \[bir\]Azure Active Directory bitiş noktasına login.microsoftonline.com.
  - Anahtar kasanızın şifreleme anahtarlarını yazmak için Linux VM'nin anahtar kasa bitiş noktasına bağlanabilmesi gerekir.
  - Linux VM, Azure uzantı deposunu ve VHD dosyalarını barındıran bir Azure depolama hesabı barındıran bir Azure depolama bitiş noktasına bağlanabilmeli.
  -  Güvenlik ilkeniz Azure VM'lerinden Internet'e erişimi kısıtlıyorsa, önceki URI'yi çözebilir ve IP'lere giden bağlantıya izin verecek şekilde belirli bir kural yapılandırabilirsiniz. Daha fazla bilgi için [bir güvenlik duvarının arkasındaki Azure Anahtar Kasası'na](../../key-vault/key-vault-access-behind-firewall.md)bakın.  

## <a name="encryption-key-storage-requirements"></a>Şifreleme anahtarı depolama gereksinimleri  

Azure Disk Şifreleme, disk şifreleme anahtarlarını ve sırlarını denetlemek ve yönetmek için bir Azure Anahtar Kasası gerektirir. Anahtar kasanız ve VM'leriniz aynı Azure bölgesinde ve abonelikte ikamet etmelidir.

Ayrıntılar için Azure [Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma](disk-encryption-key-vault.md)ya da yapılandırma ya da

## <a name="terminology"></a>Terminoloji
Aşağıdaki tablo, Azure disk şifreleme belgelerinde kullanılan ortak terimlerden bazılarını tanımlar:

| Terminoloji | Tanım |
| --- | --- |
| Azure Key Vault | Key Vault, Federal Bilgi İşlem Standartları (FIPS) onaylı donanım güvenlik modüllerini temel alan bir şifreleme, anahtar yönetim hizmetidir. Bu standartlar, şifreleme anahtarlarınızın ve hassas sırlarınızın korunmasına yardımcı olur. Daha fazla bilgi için [Azure Anahtar Kasası](https://azure.microsoft.com/services/key-vault/) belgelerine ve [Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma](disk-encryption-key-vault.md)ya da yapılandırma'ya bakın. |
| Azure CLI | [Azure CLI,](/cli/azure/install-azure-cli) komut satırından Azure kaynaklarını yönetmek ve yönetmek için optimize edilebilmektedir.|
| DM-Crypt |[DM-Crypt,](https://gitlab.com/cryptsetup/cryptsetup/wikis/DMCrypt) Linux VM'lerinde disk şifrelemesini etkinleştirmek için kullanılan Linux tabanlı, saydam disk şifreleme alt sistemidir. |
| Anahtar şifreleme anahtarı (KEK) | Sırrı korumak veya sarmak için kullanabileceğiniz asimetrik anahtar (RSA 2048). Donanım güvenlik modülü (HSM) korumalı anahtar veya yazılım korumalı anahtar sağlayabilirsiniz. Daha fazla bilgi için [Azure Anahtar Kasası](https://azure.microsoft.com/services/key-vault/) belgelerine ve [Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma](disk-encryption-key-vault.md)ya da yapılandırma'ya bakın. |
| PowerShell cmdlet'leri | Daha fazla bilgi için Azure [PowerShell cmdlets'e](/powershell/azure/overview)bakın. |


## <a name="next-steps"></a>Sonraki adımlar

- [Quickstart - Azure CLI ile bir Linux VM oluşturun ve şifreleyin](disk-encryption-cli-quickstart.md)
- [Quickstart - Azure Powershell ile bir Linux VM oluşturun ve şifreleyin](disk-encryption-powershell-quickstart.md)
- [Linux VM'lerinde Azure Disk Şifrelemesi senaryoları](disk-encryption-linux.md)
- [Azure Disk Şifreleme önkoşullar CLI komut dosyası](https://github.com/ejarvi/ade-cli-getting-started)
- [Azure Disk Şifreleme PowerShell komut dosyası önkoşullar](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)
- [Azure Disk Şifrelemesi için anahtar kasası oluşturma ve yapılandırma](disk-encryption-key-vault.md)


