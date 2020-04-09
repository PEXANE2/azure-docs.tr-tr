---
title: SSS - Linux VM'leri için Azure Disk Şifrelemesi
description: Bu makalede, Linux IaaS VM'ler için Microsoft Azure Disk Şifrelemesi hakkında sık sorulan soruların yanıtları verilmektedir.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 06/05/2019
ms.custom: seodec18
ms.openlocfilehash: ae3743530440c9df9094a0b9784922d2d6a3dfdf
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985414"
---
# <a name="azure-disk-encryption-for-linux-virtual-machines-faq"></a>Linux sanal makineler için Azure Disk Şifreleme SSS

Bu makalede, Linux sanal makineleri (VM) için Azure Disk Şifrelemesi hakkında sık sorulan soruların (SSS) yanıtları verilmektedir. Bu hizmet hakkında daha fazla bilgi için [Azure Disk Şifrelemegenel bakış](disk-encryption-overview.md)bilgisine bakın.

## <a name="what-is-azure-disk-encryption-for-linux-vms"></a>Linux VM'leri için Azure Disk Şifreleme nedir?

Linux VM'leri için Azure Disk Şifreleme, işletim sistemi diski* ve veri disklerinin tam disk şifrelemesini sağlamak için Linux'un dm-crypt özelliğini kullanır. Ayrıca, [EncryptFormatAll özelliğini](disk-encryption-linux.md#use-encryptformatall-feature-for-data-disks-on-linux-vms)kullanırken geçici kaynak diskinin şifrelemesini sağlar. İçerik VM'den Depolama arka ucuna şifrelenmiş olarak akar. Bu nedenle, müşteri tarafından yönetilen bir anahtarla uçdan uca şifreleme sağlar.
 
Bkz. [Desteklenen VM'ler ve işletim sistemleri.](disk-encryption-overview.md#supported-vms-and-operating-systems)

## <a name="where-is-azure-disk-encryption-in-general-availability-ga"></a>Azure Disk Şifreleme si nerede genel kullanılabilirlik (GA)?

Linux VM'leri için Azure Disk Şifrelemesi, tüm Azure ortak bölgelerinde genel olarak kullanılabilirdurumdadır.

## <a name="what-user-experiences-are-available-with-azure-disk-encryption"></a>Azure Disk Şifreleme ile hangi kullanıcı deneyimleri kullanılabilir?

Azure Disk Şifreleme GA, Azure Kaynak Yöneticisi şablonlarını, Azure PowerShell'i ve Azure CLI'yi destekler. Farklı kullanıcı deneyimleri size esneklik sağlar. VM'leriniz için disk şifrelemeyi etkinleştirmek için üç farklı seçeneğiniz vardır. Azure Disk Şifreleme'de bulunan kullanıcı deneyimi ve adım adım kılavuzluk hakkında daha fazla bilgi [için Linux için Azure Disk Şifreleme senaryolarına](disk-encryption-linux.md)bakın.

## <a name="how-much-does-azure-disk-encryption-cost"></a>Azure Disk Şifreleme'nin maliyeti nedir?

VM diskleri Azure Disk Şifreleme ile şifrelemek ücretsizdir, ancak Azure Key Vault kullanımıyla ilişkili ücretler vardır. Azure Key Vault maliyetleri hakkında daha fazla bilgi için [Key Vault fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/) sayfasına bakın.

## <a name="how-can-i-start-using-azure-disk-encryption"></a>Azure Disk Şifreleme'yi kullanmaya nasıl başlayabilirim?

Başlamak için Azure [Disk Şifrelemesi'ne genel bakışı](disk-encryption-overview.md)okuyun.

## <a name="what-vm-sizes-and-operating-systems-support-azure-disk-encryption"></a>Hangi VM boyutları ve işletim sistemleri Azure Disk Şifreleme'yi destekler?

[Azure Disk Şifreleme'ye genel bakış](disk-encryption-overview.md) makalesinde Azure Disk Şifrelemesini destekleyen [VM boyutları](disk-encryption-overview.md#supported-vms) ve [VM işletim sistemleri](disk-encryption-overview.md#supported-operating-systems) listelemektedir.

## <a name="can-i-encrypt-both-boot-and-data-volumes-with-azure-disk-encryption"></a>Azure Disk Şifreleme ile hem önyükleme hem de veri birimlerini şifreleyebilir miyim?

Evet, hem önyükleme hem de veri birimlerini şifreleyebilir veya önce işletim sistemi hacmini şifrelemek zorunda kalmadan veri hacmini şifreleyebilirsiniz. 

İşletim sistemi birimini şifreledikten sonra, işletim sistemi birimindeki şifrelemenin devre dışı bırakılması desteklenmez. Bir ölçek kümesindeki Linux VM'leri için yalnızca veri hacmi şifrelenebilir.

## <a name="can-i-encrypt-an-unmounted-volume-with-azure-disk-encryption"></a>Azure Disk Şifreleme ile monte edilmemiş bir birimi şifreleyebilir miyim?

Hayır, Azure Disk Şifreleme yalnızca monte edilmiş birimleri şifreler.

## <a name="what-is-storage-server-side-encryption"></a>Depolama sunucu tarafı şifreleme nedir?

Depolama sunucu tarafındaki şifreleme, Azure Depolama'da Azure yönetilen diskleri şifreler. Yönetilen diskler varsayılan olarak sunucu tarafı şifrelemesi ile platform tarafından yönetilen bir anahtarla şifrelenir (10 Haziran 2017 itibariyle). Yönetilen disklerin şifrelemesini müşteri tarafından yönetilen bir anahtar belirterek kendi anahtarlarınızla yönetebilirsiniz. Daha fazla bilgi için bkz: [Azure yönetilen disklerin sunucu tarafı şifrelemesi.](disk-encryption.md)
 
## <a name="how-is-azure-disk-encryption-different-from-storage-server-side-encryption-with-customer-managed-key-and-when-should-i-use-each-solution"></a>Azure Disk Şifrelemesi, müşteri tarafından yönetilen anahtarla Depolama sunucusu tarafındaki şifrelemeden nasıl farklıdır ve her çözümü ne zaman kullanmalıyım?

Azure Disk Şifreleme, işletim sistemi diski, veri diskleri ve müşteri tarafından yönetilen bir anahtara sahip kısa ömürlü kaynak diski için uçlardan uca şifreleme sağlar.
- Gereksinimleriniz yukarıdaki ve uçtan uca şifrelemeyi içeriyorsa, Azure Disk Şifreleme'yi kullanın. 
- Gereksinimleriniz yalnızca verileri müşteri tarafından yönetilen anahtarla şifrelemeyi içeriyorsa, [sunucu tarafı şifrelemesini müşteri tarafından yönetilen anahtarlarla](disk-encryption.md)kullanın. Bir diski hem Azure Disk Şifrelemesi hem de Depolama sunucu tarafı şifrelemesiyle müşteri tarafından yönetilen anahtarlarla şifreleyemezsiniz. 
- Linux dağıtım Sisteminiz [Azure Disk Şifreleme için desteklenen işletim sistemleri](disk-encryption-overview.md#supported-operating-systems) altında listelenmiyorsa veya Windows için [desteklenmeyen senaryolarda](disk-encryption-linux.md#unsupported-scenarios)çağrılan bir senaryo kullanıyorsanız, [müşteri tarafından yönetilen anahtarlarla Sunucu tarafı şifrelemeyi](disk-encryption.md)düşünün.
- Kuruluşunuzun ilkesi, azure tarafından yönetilen bir anahtarla içeriği istirahatte şifrelemenize izin veriyorsa, herhangi bir işlem gerekmez - içerik varsayılan olarak şifrelenir. Yönetilen diskler için depolama içindeki içerik varsayılan olarak platform tarafından yönetilen anahtarla Sunucu tarafı şifrelemesiyle şifrelenir. Anahtar Azure Depolama hizmeti tarafından yönetilir. 



## <a name="how-do-i-rotate-secrets-or-encryption-keys"></a>Sırları veya şifreleme anahtarlarını nasıl döndürebilirim?

Sırları döndürmek için, farklı bir Anahtar Kasası belirterek disk şifrelemesini etkinleştirmek için başlangıçta kullandığınız komutu aramanız gereken komutu kullanmanız gerekir. Anahtar şifreleme anahtarını döndürmek için, yeni anahtar şifrelemesini belirterek disk şifrelemeyi etkinleştirmek için başlangıçta kullandığınız komutu arayın. 

>[!WARNING]
> - Bu VM'yi şifrelemek için Azure AD kimlik bilgilerini belirterek Azure AD uygulamasıyla azure [disk şifrelemesini](disk-encryption-linux-aad.md) daha önce kullandıysanız, VM'nizi şifrelemek için bu seçeneği kullanmaya devam etiz. Desteklenen bir senaryo olmadığı için bu şifreli VM'de Azure Disk Şifreleme'yi kullanamazsınız, yani bu şifreli VM için AAD uygulamasından uzaklaşma henüz desteklenmez.

## <a name="how-do-i-add-or-remove-a-key-encryption-key-if-i-didnt-originally-use-one"></a>İlk kullanmadımsam bir anahtar şifreleme anahtarını nasıl ekleyebilirim veya kaldırırım?

Anahtar şifreleme anahtarı eklemek için, anahtar şifreleme anahtarı parametresini geçerek etkinleştir komutunu yeniden arayın. Anahtar şifreleme anahtarını kaldırmak için, anahtar şifreleme anahtarı parametresi olmadan etkinleştir komutunu yeniden arayın.

## <a name="does-azure-disk-encryption-allow-you-to-bring-your-own-key-byok"></a>Azure Disk Şifrelemesi kendi anahtarınızı (BYOK) getirmenize izin veriyor mu?

Evet, kendi anahtar şifreleme anahtarlarınızı sağlayabilirsiniz. Bu anahtarlar, Azure Disk Şifreleme'nin anahtar deposu olan Azure Key Vault'ta korunur. Anahtar şifreleme anahtarları destek senaryoları hakkında daha fazla bilgi için [bkz.](disk-encryption-key-vault.md)

## <a name="can-i-use-an-azure-created-key-encryption-key"></a>Azure tarafından oluşturulan bir anahtar şifreleme anahtarı kullanabilir miyim?

Evet, Azure disk şifreleme kullanımı için bir anahtar şifreleme anahtarı oluşturmak için Azure Key Vault'u kullanabilirsiniz. Bu anahtarlar, Azure Disk Şifreleme'nin anahtar deposu olan Azure Key Vault'ta korunur. Anahtar şifreleme anahtarı hakkında daha fazla bilgi için Azure [Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma](disk-encryption-key-vault.md)ya da yapılandırma bilgisine bakın.

## <a name="can-i-use-an-on-premises-key-management-service-or-hsm-to-safeguard-the-encryption-keys"></a>Şifreleme anahtarlarını korumak için şirket içi anahtar yönetimi hizmetini veya HSM'yi kullanabilir miyim?

Azure Disk Şifreleme ile şifreleme anahtarlarını korumak için şirket içi anahtar yönetimi hizmetini veya HSM'yi kullanamazsınız. Azure Key Vault hizmetini yalnızca şifreleme anahtarlarını korumak için kullanabilirsiniz. Anahtar şifreleme anahtarı destek senaryoları hakkında daha fazla bilgi için [bkz.](disk-encryption-key-vault.md)

## <a name="what-are-the-prerequisites-to-configure-azure-disk-encryption"></a>Azure Disk Şifrelemesini yapılandırmanın ön koşulları nelerdir?

Azure Disk Şifreleme için ön koşullar vardır. Yeni [bir](disk-encryption-key-vault.md) anahtar kasası oluşturmak için Azure Disk Şifreleme makalesi için önemli bir kasa oluşturma ve yapılandırma veya şifrelemeyi etkinleştirmek ve sırları ve anahtarları korumak için disk şifreleme erişimi için varolan bir anahtar kasası ayarlamaya bakın. Anahtar şifreleme anahtarı destek senaryoları hakkında daha fazla bilgi için [bkz.](disk-encryption-key-vault.md)

## <a name="what-are-the-prerequisites-to-configure-azure-disk-encryption-with-an-azure-ad-app-previous-release"></a>Azure Disk Şifrelemesini bir Azure AD uygulamasıyla (önceki sürüm) yapılandırmanın ön koşulları nelerdir?

Azure Disk Şifreleme için ön koşullar vardır. Azure Active Directory uygulaması oluşturmak, yeni bir anahtar kasası oluşturmak veya şifrelemeyi etkinleştirmek ve sırları ve anahtarları korumak için disk şifreleme erişimi için varolan bir anahtar kasası kurmak için [Azure AD içeriğiyle Azure Disk](disk-encryption-linux-aad.md) Şifrelemesi'ne bakın. Anahtar şifreleme anahtarı destek senaryoları hakkında daha fazla bilgi için bkz. Azure [AD ile Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma.](disk-encryption-key-vault-aad.md)

## <a name="is-azure-disk-encryption-using-an-azure-ad-app-previous-release-still-supported"></a>Azure Ad uygulamasını (önceki sürüm) kullanan Azure Disk Şifrelemesi hala destekleniyor mu?
Evet. Azure AD uygulamasını kullanarak disk şifreleme hala desteklenir. Ancak, yeni VM'leri şifrelerken, bir Azure AD uygulamasıyla şifrelemek yerine yeni yöntemi kullanmanız önerilir. 

## <a name="can-i-migrate-vms-that-were-encrypted-with-an-azure-ad-app-to-encryption-without-an-azure-ad-app"></a>Azure AD uygulamasıyla şifrelenmiş VM'leri Azure AD uygulaması olmadan şifrelemeye geçirebilir miyim?
  Şu anda, Azure AD uygulamasıyla şifrelenmiş makineler için azure AD uygulaması olmadan şifrelemeye doğrudan geçiş yolu yoktur. Ayrıca, Azure AD uygulaması olmadan şifrelemeden bir AD uygulamasıyla şifrelemeye doğrudan bir yol yoktur. 

## <a name="what-version-of-azure-powershell-does-azure-disk-encryption-support"></a>Azure Disk Şifreleme hangi sürümünü destekler?

Azure Disk Şifrelemesini yapılandırmak için Azure PowerShell SDK'nın en son sürümünü kullanın. [Azure PowerShell'in](https://github.com/Azure/azure-powershell/releases)en son sürümünü indirin. Azure Disk Şifreleme, Azure SDK sürüm 1.1.0 tarafından *desteklenmez.*

> [!NOTE]
> Linux Azure disk şifreleme önizleme uzantısı "Microsoft.OSTCExtension.AzureDiskEncryptionForLinux" amortismana tabidir. Bu uzantı, Azure disk şifreleme önizleme sürümü için yayımlandı. Deneme veya üretim dağıtımınızda uzantının önizleme sürümünü kullanmamalısınız.

> Linux IaaS VM'nizde şifreleme sağlamak için Linux VM için Azure disk şifreleme uzantısıdağıtmanız gereken Azure Kaynak Yöneticisi (ARM) gibi dağıtım senaryolarında, Azure disk şifreleme satımı destekli "Microsoft.Azure.Security.AzureDiskEncryptionForLinux" uzantısını kullanmanız gerekir.

## <a name="can-i-apply-azure-disk-encryption-on-my-custom-linux-image"></a>Azure Disk Şifreleme'yi özel Linux resmime uygulayabilir miyim?

Azure Disk Şifreleme'yi özel Linux resminize uygulayamazsınız. Desteklenen dağıtımlar için yalnızca galeri Linux görüntüleri daha önce desteklenir. Özel Linux görüntüleri şu anda desteklenmez.

## <a name="can-i-apply-updates-to-a-linux-red-hat-vm-that-uses-the-yum-update"></a>Yum güncellemesini kullanan bir Linux Red Hat VM'e güncelleme uygulayabilir miyim?

Evet, Red Hat Linux VM'de yum güncellemesi yapabilirsiniz.  Daha fazla bilgi [için, yalıtılmış bir ağda Azure Disk Şifreleme'ye](disk-encryption-isolated-network.md)bakın.

## <a name="what-is-the-recommended-azure-disk-encryption-workflow-for-linux"></a>Linux için önerilen Azure disk şifreleme iş akışı nedir?

Aşağıdaki iş akışınılinux üzerinde en iyi sonuçlara sahip olması için önerilir:
* Gerekli işletim sistemi dağıtım ve sürümüne karşılık gelen değiştirilmemiş stok galerisi görüntüsünden başlayın
* Şifrelenecek tüm monte edilmiş sürücüleri yedekleyin.  Bu yedekleme, bir hata varsa( örneğin şifreleme tamamlanmadan önce VM yeniden başlatılırsa) kurtarma sağlar.
* Şifreleme (VM özelliklerine ve ekli veri disklerinin boyutuna bağlı olarak birkaç saat, hatta günler sürebilir)
* Görüntüyü gerektiği gibi özelleştirin ve görüntüye yazılım ekleyin.

Bu iş akışı mümkün değilse, platform depolama hesabı katmanındaki [Depolama Hizmeti Şifrelemesine](../../storage/common/storage-service-encryption.md) (SSE) güvenmek, dm-crypt kullanarak tam disk şifrelemesine alternatif olabilir.

## <a name="what-is-the-disk-bek-volume-or-mntazure_bek_disk"></a>"Bek Volume" veya "/mnt/azure_bek_disk" diski nedir?

"Bek birimi", Şifrelenmiş Azure VM'lerinin şifreleme anahtarlarını güvenli bir şekilde depolayan yerel bir veri hacmidir.
> [!NOTE]
> Bu diskteki içeriği silmeyin veya düzenlemayın. IaaS VM'deki tüm şifreleme işlemleri için şifreleme anahtarı varlığı gerektiğinden diski sökmeyin.


## <a name="what-encryption-method-does-azure-disk-encryption-use"></a>Azure Disk Şifreleme hangi şifreleme yöntemini kullanır?

Azure Disk Şifreleme, 256 bit lik hacim ana anahtarıyla aes-xts-plain64'ün şifre çözme varsayılanını kullanır.

## <a name="if-i-use-encryptformatall-and-specify-all-volume-types-will-it-erase-the-data-on-the-data-drives-that-we-already-encrypted"></a>EncryptFormatAll kullanır ve tüm birim türlerini belirtirsem, zaten şifrelediğimiz veri sürücülerindeki verileri siler mi?
Hayır, veriler Azure Disk Şifrelemesi kullanılarak zaten şifrelenmiş veri sürücülerinden silinmez. EncryptFormatAll'ın işletim sistemi sürücüsünün yeniden şifrelenmemiş olmasına benzer şekilde, zaten şifrelenmiş olan veri sürücüsünüksetmez. Daha fazla bilgi için [EncryptFormatAll ölçütleri'ne](disk-encryption-linux.md#use-encryptformatall-feature-for-data-disks-on-linux-vms)bakın.        

## <a name="is-xfs-filesystem-supported"></a>XFS dosya sistemi desteklendi mi?
XFS birimleri yalnızca EncryptFormatAll ile veri diski şifrelemesi için desteklenir. Bu, daha önce orada herhangi bir veri silen, hacim de yeniden olacaktır. Daha fazla bilgi için [EncryptFormatAll ölçütleri'ne](disk-encryption-linux.md#use-encryptformatall-feature-for-data-disks-on-linux-vms)bakın.

## <a name="can-i-backup-and-restore-an-encrypted-vm"></a>Şifreli bir VM'yi yedekleyebilir ve geri yükleyebilir miyim? 

Azure Yedekleme, aynı abonelik ve bölge içinde şifrelenmiş VM'leri yedeklemek ve geri yüklemek için bir mekanizma sağlar.  Talimatlar için lütfen [Yedekleme'ye bakın ve Azure Yedekleme ile şifrelenmiş sanal makineleri geri yükleyin.](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption)  Şifreli bir VM'yi farklı bir bölgeye geri getirmek şu anda desteklenmez.  

## <a name="where-can-i-go-to-ask-questions-or-provide-feedback"></a>Soru sormak veya geri bildirim sağlamak için nereye gidebilirim?

[Azure Disk Şifreleme forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDiskEncryption)hakkında soru sorabilir veya geri bildirimde bulunabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Disk Şifrelemeile ilgili en sık sorulan sorular hakkında daha fazla bilgi edinebilirsiniz. Bu hizmet hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Disk Şifrelemesine Genel Bakış](disk-encryption-overview.md)
- [Azure Güvenlik Merkezi'nde disk şifreleme uygulayın](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Azure veri şifrelemesi istirahatte](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)
