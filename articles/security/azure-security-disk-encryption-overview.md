---
title: Azure disk şifrelemesi nedir?
description: Bu makalede, Azure disk şifrelemesi 'ne genel bakış sunulmaktadır
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 07/29/2019
ms.custom: seodec18
ms.openlocfilehash: c5e568dd073376295e4865994fba8ae5b5ac59a0
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68641000"
---
# <a name="azure-disk-encryption-overview"></a>Azure disk şifrelemeye genel bakış

Azure disk şifrelemesi, kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamak için verilerinizi korumanıza ve korumaya yardımcı olur. Azure sanal makinelerinin (VM 'Ler) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) özelliğini ve Linux 'un [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) özelliğini kullanır. Ayrıca, disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize yardımcı olmak için [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) ile TÜMLEŞIKTIR ve VM disklerindeki tüm verilerin Azure Storage 'da Rest 'te şifrelenmesini sağlar. Windows ve Linux sanal makineleri için Azure disk şifrelemesi, Azure Premium Depolama ile standart sanal makineler ve sanal makineler için tüm Azure genel bölgelerinde ve Azure Kamu bölgelerinde genel kullanıma açıktır. 

Azure Güvenlik Merkezi kullanırsanız, şifreli olmayan VM'ler varsa uyarı. Uyarılar yüksek önem derecesine sahip olarak gösterir ve bu VM'lerin şifrelemek için önerilir.

![Azure Güvenlik Merkezi disk Şifreleme Uyarısı](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Bazı öneriler, veri, ağ veya hesaplama kaynak kullanımı ve sonucunda ek lisans ya da abonelik maliyetlerinizi artırabilir.


## <a name="encryption-scenarios"></a>Şifreleme senaryoları

Azure disk şifrelemesi sayesinde, sektör standardı şifreleme teknolojisini kullanarak Azure VM 'lerinizi Rest 'ten güvenli hale getirerek kurumsal güvenlik ve uyumluluk gereksinimlerini ele alabilirsiniz. Ayrıca, VM 'Leri müşteri denetimli anahtarlar ve ilkeler (BYOK) altında önyüklenecek şekilde yapılandırabilir ve bu anahtarların anahtar kasasında kullanımını kontrol edebilirsiniz.

Azure disk şifrelemesi aşağıdaki müşteri senaryolarını destekler:

* Desteklenen Azure Galeri görüntülerinden oluşturulan yeni VM 'lerde şifrelemeyi etkinleştirme ve devre dışı bırakma.
* Azure 'da çalışan mevcut VM 'lerde şifrelemeyi etkinleştirme ve devre dışı bırakma.
* Önceden şifrelenen VHD ve şifreleme anahtarlarından oluşturulan yeni Windows VM 'lerinde şifrelemeyi etkinleştirme ve devre dışı bırakma.
* Windows sanal makine ölçek kümelerinde şifrelemeyi etkinleştirme ve devre dışı bırakma.
* Linux sanal makine ölçek kümeleri için veri sürücülerinde şifrelemeyi etkinleştirme ve devre dışı bırakma.
* Yönetilen disk VM 'lerinin şifrelemesini etkinleştirme ve devre dışı bırakma.
* Mevcut bir şifrelenmiş Premium ve Premium olmayan depolama VM 'sinin şifreleme ayarları güncelleştiriliyor.
* Şifrelenmiş VM 'Leri yedekleme ve geri yükleme.
* Kendi şifrelemesini getirin (BYOE) ve kendi anahtar (BYOK) senaryolarınızı getirin ve müşterilerin kendi şifreleme anahtarlarını kullanır ve bunları bir Azure Anahtar Kasası 'nda depolar.

Ayrıca, Microsoft Azure ' de etkinleştirildiklerinde VM 'Ler için aşağıdaki senaryoları destekler:

* Azure anahtar kasası ile tümleştirme.
* [Minimum bellek gereksinimini](azure-security-disk-encryption-prerequisites.md#supported-vm-sizes)karşılayan [Standart katman VM 'leri](https://azure.microsoft.com/pricing/details/virtual-machines/) . 
* Desteklenen Azure Galeri görüntülerinden Windows ve Linux VM 'lerinde, yönetilen diskte ve ölçek kümesi VM 'lerinde şifrelemeyi etkinleştirme.
* Windows VM 'leri, ölçek kümesi VM 'leri ve yönetilen disk VM 'Leri için işletim sistemi ve veri sürücülerinde şifrelemeyi devre dışı bırakma.
* Linux VM 'Leri, ölçek kümesi VM 'leri ve yönetilen disk VM 'Leri için veri sürücülerinde şifrelemeyi devre dışı bırakma.
* Windows Istemci işletim sistemini çalıştıran VM 'lerde şifrelemeyi etkinleştirme.
* Bağlama yollarındaki birimlerde şifrelemeyi etkinleştirme.
* Mdaddm kullanılarak disk şeridi (RAID) ile yapılandırılmış Linux VM 'lerinde şifrelemeyi etkinleştirme.
* Veri diskleri için LVM kullanan Linux sanal makinelerinde şifrelemeyi etkinleştirme.
* Linux VM işletim sistemi ve veri disklerinde şifrelemeyi etkinleştirme.

   > [!NOTE]
   > Bazı Linux dağıtımlarında için işletim sistemi Sürücü Şifrelemesi desteklenmez. Daha fazla bilgi için bkz [. Azure Disk Şifrelemesi tarafından desteklenen işletim sistemleri: Linux](azure-security-disk-encryption-prerequisites.md#linux).
   
* Windows Server 2016 ' de başlayan Windows depolama alanları ile yapılandırılmış VM 'lerde şifrelemeyi etkinleştirme. Depolama Alanları Doğrudan (S2D) henüz desteklenmiyor.
* Hem anahtar şifreleme anahtarı (KEK) hem de KEK olmayan senaryolar için şifrelenmiş VM 'Leri yedekleme ve geri yükleme.

Azure disk şifrelemesi, aşağıdaki senaryolar, Özellikler ve teknolojiler için çalışmaz:

* Klasik VM oluşturma yöntemiyle oluşturulan temel katman VM veya VM 'Leri şifreleme.
* IŞLETIM sistemi sürücüsü şifrelendiğinde bir işletim sistemi sürücüsünde veya Linux sanal makinesinin veri sürücüsünde şifrelemeyi devre dışı bırakma.
* Linux sanal makine ölçek kümeleri için işletim sistemi sürücüsü şifreleme.
* Yazılım tabanlı RAID sistemleriyle yapılandırılmış Windows VM 'Leri şifreleme.
* Linux VM 'lerde özel görüntüleri şifreleme.
* Şirket içi anahtar yönetim sistemiyle tümleştirme.
* Azure dosyaları (paylaşılan dosya sistemi).
* Ağ dosya sistemi (NFS).
* Dinamik birimler.
* Kısa ömürlü işletim sistemi diskleri.

## <a name="encryption-features"></a>Şifreleme özellikleri

Azure sanal makineleri için Azure disk şifrelemesini etkinleştirip dağıttığınızda, aşağıdaki özellikleri etkinleştirilecek şekilde yapılandırabilirsiniz:

* Depolama biriminizdeki geri yükleme birimini korumak için işletim sistemi birimini şifreleme.
* Depolama ortamınızdaki bekleyen veri birimlerini korumak için veri birimlerini şifreleme.
* Windows VM 'Leri için işletim sistemi ve veri sürücülerinde şifrelemeyi devre dışı bırakma.
* Linux VM 'Leri için veri sürücülerinde şifrelemeyi devre dışı bırakma (yalnızca işletim sistemi sürücüsü şifrelenmediğinde).
* Azure Key Vault aboneliğinizdeki şifreleme anahtarlarını ve gizli dizileri koruma altına al.
* Şifrelenmiş VM 'nin şifreleme durumunu raporlama.
* VM 'den disk şifrelemesi yapılandırma ayarları kaldırılıyor.
* Azure Backup hizmetini kullanarak şifrelenmiş VM 'Leri yedekleme ve geri yükleme.

Windows ve Linux için sanal makineler için Azure disk şifrelemesi şunları içerir:

* [Windows için disk şifreleme uzantısı](../virtual-machines/extensions/azure-disk-enc-windows.md).
* [Linux için disk şifreleme uzantısı](../virtual-machines/extensions/azure-disk-enc-linux.md).
* [PowerShell disk şifrelemesi cmdlet 'leri](/powershell/module/az.compute/set-azvmdiskencryptionextension?view=azps-2.2.0).
* [Azure CLI disk şifrelemesi cmdlet 'leri](/cli/azure/vm/encryption?view=azure-cli-latest).
* [Azure Resource Manager disk şifreleme şablonları](azure-security-disk-encryption-appendix.md#resource-manager-templates).

> [!NOTE]
> Azure Disk şifrelemesi ile VM disklerini şifrelemek için ek ücret yoktur. Standart [anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/) şifreleme anahtarları depolamak için kullanılan anahtar kasası için geçerlidir. 

## <a name="encryption-workflow"></a>Şifreleme iş akışı

Windows ve Linux Vm'leri için disk şifrelemeyi etkinleştirmek için aşağıdaki adımları uygulayın:

1. Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'leri veya Azure CLI üzerinden disk şifrelemeyi etkinleştirmek için katılım ve şifreleme yapılandırması belirtin.

   * Müşteri şifrelenmiş VHD senaryosu için depolama hesabınızı ve şifreleme anahtar malzemesi anahtar kasanıza şifrelenmiş VHD yükleyin. Ardından, yeni bir VM 'de şifrelemeyi etkinleştirmek için şifreleme yapılandırmasını sağlayın.
   * Desteklenen Galeri görüntülerinden oluşturulan yeni VM 'Ler ve Azure 'da zaten çalışan mevcut VM 'Ler için VM 'de şifrelemeyi etkinleştirmek üzere şifreleme yapılandırması sağlayın.

1. VM 'de şifrelemeyi etkinleştirmek için şifreleme anahtarı malzemesini (Windows sistemleri için BitLocker şifreleme anahtarları ve Linux için parola) okumak üzere Azure platformuna erişim izni verin.

1. Azure VM hizmet modeli şifrelemesi ile key vault yapılandırması güncelleştirir ve, şifrelenmiş sanal Makineyi ayarlar.

   ![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Şifre çözme iş akışı
VM 'Ler için disk şifrelemeyi devre dışı bırakmak için aşağıdaki üst düzey adımları izleyin:

1. Azure 'da çalışan bir VM 'de şifrelemeyi (şifre çözme) devre dışı bırakmayı seçin ve şifre çözme yapılandırmasını belirtin. Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'leri ve Azure CLI devre dışı bırakabilirsiniz.

   Bu adım işletim sistemi veya veri birimi şifrelemesini devre dışı bırakır ve her ikisi de çalışan Windows VM üzerinde. Önceki bölümde belirtildiği gibi Linux işletim sistemi disk şifreleme devre dışı bırakma özelliği desteklenmiyor. İşletim sistemi diski şifreli değil sürece şifre çözme adım yalnızca Linux vm'lerinde veri sürücülerine izin verilir.

1. Azure, VM hizmeti modelini güncelleştirir ve VM, şifresi çözülmüş olarak işaretlenir. VM içeriğini artık bekleme durumundayken şifrelenir.

   > [!NOTE]
   > Devre dışı bırakma şifreleme işlemi, anahtar kasanıza ve şifreleme anahtar malzemesi (BitLocker şifreleme anahtarlarını Windows sistemleri için) veya Linux için parola silmez.
   >
   > Linux için işletim sistemi disk şifreleme devre dışı bırakma desteklenmiyor. Şifre çözme adım, yalnızca Linux vm'lerinde veri sürücüleri için izin verilir.
   >
   > Linux için disk şifrelemeyi devre dışı bırakma, işletim sistemi sürücüsünü şifrelendiyse desteklenmez.


## <a name="encryption-workflow-previous-release"></a>Şifreleme iş akışı (önceki sürüm)

Azure Disk Şifrelemesi'nın yeni sürümüne VM disk şifrelemeyi etkinleştirmek için bir Azure Active Directory (Azure AD) uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümle birlikte, artık etkinleştirme şifreleme adımı sırasında bir Azure AD kimlik bilgisi sağlayın isteniyor. Yeni yayın kullandığınızda, tüm yeni Vm'lere Azure AD uygulama parametresiz şifrelenmelidir. Zaten Azure AD uygulama parametreleri ile şifrelenmiş VM'ler hala desteklenmektedir ve Azure AD sözdizimi ile yönetilmeye devam. Windows ve Linux Vm'leri (önceki sürüm) için disk şifrelemeyi etkinleştirmek için aşağıdaki adımları uygulayın:

1. Bir şifreleme senaryo listelenen senaryolar arasından seçim [şifreleme senaryolarının](#encryption-scenarios) bölümü.

1. Azure Disk şifrelemesi Resource Manager şablonu, PowerShell cmdlet'leri veya Azure CLI üzerinden disk şifrelemeyi etkinleştirmek için katılım ve şifreleme yapılandırması belirtin.

   * Müşteri şifrelenmiş VHD senaryosu için depolama hesabınızı ve şifreleme anahtar malzemesi anahtar kasanıza şifrelenmiş VHD yükleyin. Ardından, yeni bir VM 'de şifrelemeyi etkinleştirmek için şifreleme yapılandırmasını sağlayın.
   * Market 'ten ve Azure 'da zaten çalışan mevcut VM 'lerden oluşturulan yeni VM 'Ler için, VM 'de şifrelemeyi etkinleştirmek üzere şifreleme yapılandırmasını sağlayın.

1. VM 'de şifrelemeyi etkinleştirmek için şifreleme anahtarı malzemesini (Windows sistemleri için BitLocker şifreleme anahtarları ve Linux için parola) okumak üzere Azure platformuna erişim izni verin.

1. Anahtar kasanızı malzeme şifreleme anahtarını yazmak için Azure AD uygulama kimliği sağlayın. Bu adım 2. adımda bahsedilen senaryolar için VM 'de şifrelemeyi mümkün bir şekilde sunar.

1. Azure VM hizmet modeli şifrelemesi ile key vault yapılandırması güncelleştirir ve, şifrelenmiş sanal Makineyi ayarlar.


## <a name="terminology"></a>Terminoloji
Aşağıdaki tabloda, Azure disk şifrelemesi belgelerinde kullanılan bazı yaygın terimler tanımlanmaktadır:

| Terminoloji | Tanım |
| --- | --- |
| Azure AD | Bir [Azure AD'ye](https://azure.microsoft.com/documentation/services/active-directory/) hesap kimlik doğrulaması, depolama ve gizli dizileri anahtar kasasından almak için kullanılır. |
| Azure Key Vault | Key Vault, Federal Bilgi işleme standartları (FIPS) üzerinde doğrulanmış donanım güvenlik modülleri dayanıyor bir şifreleme ve anahtar yönetim hizmetidir. Bu standartlar, şifreleme anahtarlarını ve gizli gizli anahtarlarınızı yardımcı olur. Daha fazla bilgi için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) belgeleri. |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) , Windows VM 'lerinde disk şifrelemeyi etkinleştirmek için kullanılan sektör tarafından tanınan bir Windows birimi şifreleme teknolojisidir. |
| BEK | BitLocker şifreleme anahtarları (BEK), veri hacimleri ve işletim sistemi önyükleme birimi şifrelemek için kullanılır. BEKs anahtar kasasında gizli dizi olarak korunur. |
| Azure CLI | [Azure CLI'yı](/cli/azure/install-azure-cli) Azure kaynaklarını komut satırından yönetmek ve için optimize edilmiştir.|
| DM-Crypt |[Dm-crypt](https://gitlab.com/cryptsetup/cryptsetup/wikis/DMCrypt) , Linux sanal makinelerinde disk şifrelemeyi etkinleştirmek Için kullanılan Linux tabanlı, saydam disk şifreleme alt sistemidir. |
| Anahtar şifreleme anahtarı (KEK) | Gizli anahtarı korumak veya kaydırmak için kullanabileceğiniz asimetrik anahtar (RSA 2048). Bir donanım güvenlik modülü (HSM) sağladığınız-korumalı bir anahtar veya yazılım korumalı anahtarı. Daha fazla bilgi için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) belgeleri. |
| PowerShell cmdlet'leri | Daha fazla bilgi için [Azure PowerShell cmdlet'lerini](/powershell/azure/overview). |

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için bkz. [Azure disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md).

