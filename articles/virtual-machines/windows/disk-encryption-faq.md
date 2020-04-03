---
title: SSS - Windows VM'leri için Azure Disk Şifrelemesi
description: Bu makalede, Windows IaaS VM'ler için Microsoft Azure Disk Şifrelemesi hakkında sık sorulan soruların yanıtları sağlanmaktadır.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 11/01/2019
ms.custom: seodec18
ms.openlocfilehash: 5cc6a3d8d736209f762959bca6f8ad379c14203f
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80582706"
---
# <a name="azure-disk-encryption-for-windows-vms-faq"></a>Windows VMS SSS için Azure Disk Şifrelemesi

Bu makalede, Windows VM'ler için Azure Disk Şifrelemesi hakkında sık sorulan soruların (SSS) yanıtları verilmektedir. Bu hizmet hakkında daha fazla bilgi için [Azure Disk Şifrelemegenel bakış](disk-encryption-overview.md)bilgisine bakın.

## <a name="where-is-azure-disk-encryption-in-general-availability-ga"></a>Azure Disk Şifreleme si nerede genel kullanılabilirlik (GA)?

Azure Disk Şifrelemesi, tüm Azure ortak bölgelerinde genel kullanılabilirlik içindedir.

## <a name="what-user-experiences-are-available-with-azure-disk-encryption"></a>Azure Disk Şifreleme ile hangi kullanıcı deneyimleri kullanılabilir?

Azure Disk Şifreleme GA, Azure Kaynak Yöneticisi şablonlarını, Azure PowerShell'i ve Azure CLI'yi destekler. Farklı kullanıcı deneyimleri size esneklik sağlar. VM'leriniz için disk şifrelemeyi etkinleştirmek için üç farklı seçeneğiniz vardır. Azure Disk Şifreleme'de bulunan kullanıcı deneyimi ve adım adım kılavuzluk hakkında daha fazla bilgi [için Windows için Azure Disk Şifreleme senaryolarına](disk-encryption-windows.md)bakın.

## <a name="how-much-does-azure-disk-encryption-cost"></a>Azure Disk Şifreleme'nin maliyeti nedir?

VM diskleri Azure Disk Şifreleme ile şifrelemek ücretsizdir, ancak Azure Key Vault kullanımıyla ilişkili ücretler vardır. Azure Key Vault maliyetleri hakkında daha fazla bilgi için [Key Vault fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/) sayfasına bakın.

## <a name="how-can-i-start-using-azure-disk-encryption"></a>Azure Disk Şifreleme'yi kullanmaya nasıl başlayabilirim?

Başlamak için Azure [Disk Şifrelemesi'ne genel bakışı](disk-encryption-overview.md)okuyun.

## <a name="what-vm-sizes-and-operating-systems-support-azure-disk-encryption"></a>Hangi VM boyutları ve işletim sistemleri Azure Disk Şifreleme'yi destekler?

[Azure Disk Şifreleme'ye genel bakış](disk-encryption-overview.md) makalesinde Azure Disk Şifrelemesini destekleyen [VM boyutları](disk-encryption-overview.md#supported-vms) ve [VM işletim sistemleri](disk-encryption-overview.md#supported-operating-systems) listelemektedir.

## <a name="can-i-encrypt-both-boot-and-data-volumes-with-azure-disk-encryption"></a>Azure Disk Şifreleme ile hem önyükleme hem de veri birimlerini şifreleyebilir miyim?

Hem önyükleme hem de veri birimlerini şifreleyebilirsiniz, ancak işletim sistemi birimini şifrelemeden verileri şifreleyebilirsiniz.

İşletim sistemi birimini şifreledikten sonra, işletim sistemi birimindeki şifrelemenin devre dışı bırakılması desteklenmez.

## <a name="can-i-encrypt-an-unmounted-volume-with-azure-disk-encryption"></a>Azure Disk Şifreleme ile monte edilmemiş bir birimi şifreleyebilir miyim?

Hayır, Azure Disk Şifreleme yalnızca monte edilmiş birimleri şifreler.

## <a name="how-do-i-rotate-secrets-or-encryption-keys"></a>Sırları veya şifreleme anahtarlarını nasıl döndürebilirim?

Sırları döndürmek için, farklı bir Anahtar Kasası belirterek disk şifrelemesini etkinleştirmek için başlangıçta kullandığınız komutu aramanız gereken komutu kullanmanız gerekir. Anahtar şifreleme anahtarını döndürmek için, yeni anahtar şifrelemesini belirterek disk şifrelemeyi etkinleştirmek için başlangıçta kullandığınız komutu arayın. 

>[!WARNING]
> - Bu VM'yi şifrelemek için Azure AD kimlik bilgilerini belirterek Azure AD uygulamasıyla azure [disk şifrelemesini](disk-encryption-windows-aad.md) daha önce kullandıysanız, VM'nizi şifrelemek için bu seçeneği kullanmaya devam etiz. Desteklenen bir senaryo olmadığı için bu şifreli VM'de Azure Disk Şifreleme'yi kullanamazsınız, yani bu şifreli VM için AAD uygulamasından uzaklaşma henüz desteklenmez.

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

Azure Disk Şifreleme için ön koşullar vardır. Azure Active Directory uygulaması oluşturmak, yeni bir anahtar kasası oluşturmak veya şifrelemeyi etkinleştirmek ve sırları ve anahtarları korumak için disk şifreleme erişimi için varolan bir anahtar kasası kurmak için [Azure AD içeriğiyle Azure Disk](disk-encryption-windows-aad.md) Şifrelemesi'ne bakın. Anahtar şifreleme anahtarı destek senaryoları hakkında daha fazla bilgi için bkz. Azure [AD ile Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma.](disk-encryption-key-vault-aad.md)

## <a name="is-azure-disk-encryption-using-an-azure-ad-app-previous-release-still-supported"></a>Azure Ad uygulamasını (önceki sürüm) kullanan Azure Disk Şifrelemesi hala destekleniyor mu?
Evet. Azure AD uygulamasını kullanarak disk şifreleme hala desteklenir. Ancak, yeni VM'leri şifrelerken, bir Azure AD uygulamasıyla şifrelemek yerine yeni yöntemi kullanmanız önerilir. 

## <a name="can-i-migrate-vms-that-were-encrypted-with-an-azure-ad-app-to-encryption-without-an-azure-ad-app"></a>Azure AD uygulamasıyla şifrelenmiş VM'leri Azure AD uygulaması olmadan şifrelemeye geçirebilir miyim?
  Şu anda, Azure AD uygulamasıyla şifrelenmiş makineler için azure AD uygulaması olmadan şifrelemeye doğrudan geçiş yolu yoktur. Ayrıca, Azure AD uygulaması olmadan şifrelemeden bir AD uygulamasıyla şifrelemeye doğrudan bir yol yoktur. 

## <a name="what-version-of-azure-powershell-does-azure-disk-encryption-support"></a>Azure Disk Şifreleme hangi sürümünü destekler?

Azure Disk Şifrelemesini yapılandırmak için Azure PowerShell SDK'nın en son sürümünü kullanın. [Azure PowerShell'in](https://github.com/Azure/azure-powershell/releases)en son sürümünü indirin. Azure Disk Şifreleme, Azure SDK sürüm 1.1.0 tarafından *desteklenmez.*

## <a name="what-is-the-disk-bek-volume-or-mntazure_bek_disk"></a>"Bek Volume" veya "/mnt/azure_bek_disk" diski nedir?

"Bek birimi", Şifrelenmiş Azure VM'lerinin şifreleme anahtarlarını güvenli bir şekilde depolayan yerel bir veri hacmidir.

> [!NOTE]
> Bu diskteki içeriği silmeyin veya düzenlemayın. IaaS VM'deki tüm şifreleme işlemleri için şifreleme anahtarı varlığı gerektiğinden diski sökmeyin.

## <a name="what-encryption-method-does-azure-disk-encryption-use"></a>Azure Disk Şifreleme hangi şifreleme yöntemini kullanır?

Azure Disk Şifreleme, Windows'un sürümünü temel alan BitLocker'daki şifreleme yöntemini aşağıdaki gibi seçer:

| Windows Sürümleri                 | Sürüm | Şifreleme Yöntemi        |
|----------------------------------|--------|--------------------------|
| Windows Server 2012, Windows 10 veya daha büyük  | >=1511 |XTS-AES 256 bit           |
| Windows Server 2012, Windows 8, 8.1, 10 | < 1511 |AES 256 bit *              |
| Windows Server 2008R2            |        |Difüzör lü AES 256 bit |

\*Difüzör lü AES 256 bit, Windows 2012 ve sonraki yıllarda desteklenmez.

Windows OS sürümünü belirlemek için sanal makinenizde 'winver' aracını çalıştırın.

## <a name="if-i-use-encryptformatall-and-specify-all-volume-types-will-it-erase-the-data-on-the-data-drives-that-we-already-encrypted"></a>EncryptFormatAll kullanır ve tüm birim türlerini belirtirsem, zaten şifrelediğimiz veri sürücülerindeki verileri siler mi?
Hayır, veriler Azure Disk Şifrelemesi kullanılarak zaten şifrelenmiş veri sürücülerinden silinmez. EncryptFormatAll'ın işletim sistemi sürücüsünün yeniden şifrelenmemiş olmasına benzer şekilde, zaten şifrelenmiş olan veri sürücüsünüksetmez. 

## <a name="can-i-backup-and-restore-an-encrypted-vm"></a>Şifreli bir VM'yi yedekleyebilir ve geri yükleyebilir miyim? 

Azure Yedekleme, aynı abonelik ve bölge içinde şifrelenmiş VM'leri yedeklemek ve geri yüklemek için bir mekanizma sağlar.  Talimatlar için lütfen [Yedekleme'ye bakın ve Azure Yedekleme ile şifrelenmiş sanal makineleri geri yükleyin.](../../backup/backup-azure-vms-encryption.md)  Şifreli bir VM'yi farklı bir bölgeye geri getirmek şu anda desteklenmez.  

## <a name="where-can-i-go-to-ask-questions-or-provide-feedback"></a>Soru sormak veya geri bildirim sağlamak için nereye gidebilirim?

[Azure Disk Şifreleme forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDiskEncryption)hakkında soru sorabilir veya geri bildirimde bulunabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Disk Şifrelemeile ilgili en sık sorulan sorular hakkında daha fazla bilgi edinebilirsiniz. Bu hizmet hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Disk Şifrelemesine Genel Bakış](disk-encryption-overview.md)
- [Azure Güvenlik Merkezi'nde disk şifreleme uygulayın](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Azure veri şifrelemesi istirahatte](../../security/fundamentals/encryption-atrest.md)
