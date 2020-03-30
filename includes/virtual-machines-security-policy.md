---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 14a6703b3e256d33ab3b18e1821587cc3eb293db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79382108"
---
Çalıştırdığınız uygulamalar için sanal makinenizi (VM) güvende tutmanız önemlidir. Sanal Cihazlarınızın güvenliğini sağlamak, Sanal Cihazlarınıza güvenli erişimi ve verilerinizin güvenli bir şekilde depolanmasını kapsayan bir veya daha fazla Azure hizmeti ve özelliği içerebilir. Bu makalede, VM ve uygulamaları güvenli tutmanızı sağlayan bilgiler sağlar.

## <a name="antimalware"></a>Antimalware

Bulut ortamları için modern tehdit ortamı dinamiktir ve uyumluluk ve güvenlik gereksinimlerini karşılamak için etkin koruma yı koruma baskısını artırır. [Azure için Microsoft Kötü Amaçlı Yazılımdan Koruma,](../articles/security/fundamentals/antimalware.md) virüsleri, casus yazılımları ve diğer kötü amaçlı yazılımları tanımlamaya ve kaldırmaya yardımcı olan ücretsiz bir gerçek zamanlı koruma özelliğidir. Uyarılar, kötü amaçlı veya istenmeyen yazılımlar kendisini yüklemeye veya VM'nizde çalıştırmaya çalıştığında sizi bilgilendirecek şekilde yapılandırılabilir. Linux veya Windows Server 2008 çalıştıran VM'lerde desteklenmez.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi,](../articles/security-center/security-center-intro.md) VM'lerinize yönelik tehditleri önlemenize, algılamanıza ve yanıt vermenize yardımcı olur. Güvenlik Merkezi, Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde fark edilmeyecek tehditleri algılamaya yardımcı olur ve geniş bir güvenlik çözümleri ekosistemiyle çalışır.

Güvenlik Merkezi'nin tam zamanında erişimi, Azure VM'lerinize gelen trafiği kilitlemek ve gerektiğinde VM'lere bağlanmak için kolay erişim sağlarken saldırılara maruz kalmamayı azaltmak için VM dağıtımınızda uygulanabilir. Tam zamanında etkinleştirildiğinde ve bir kullanıcı VM'ye erişim istediğinde, Güvenlik Merkezi kullanıcının VM için hangi izinlere sahip olduğunu denetler. Doğru izinlere sahiplerse, istek onaylanır ve Güvenlik Merkezi, seçilen bağlantı noktalarına gelen trafiğin sınırlı bir süre için girmesine izin verecek şekilde Ağ Güvenlik Grupları'nı (NSGs) otomatik olarak yapılandırır. Süre dolduktan sonra, Güvenlik Merkezi NSG'leri önceki eyaletlerine geri yükler. 

## <a name="encryption"></a>Şifreleme

Yönetilen diskler için iki şifreleme yöntemi sunulur. Azure Disk Şifrelemesi olan Işletim Sistemi düzeyinde şifreleme ve sunucu tarafı şifrelemesi olan platform düzeyinde şifreleme.

### <a name="server-side-encryption"></a>Sunucu tarafı şifrelemesi

Azure yönetilen diskler, verilerinizi bulutta kalıcı hale getirirken varsayılan olarak verilerinizi otomatik olarak şifreler. Sunucu tarafı şifreleme verilerinizi korur ve kuruluş güvenliği ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur. Azure yönetilen disklerde veriler, mevcut en güçlü blok şifrelerinden biri olan 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanılarak saydam olarak şifrelenir ve FIPS 140-2 uyumludur.

Şifreleme yönetilen disklerin performansını etkilemez. Şifreleme için ek bir maliyet yoktur.

Yönetilen diskinizin şifrelemesi için platform tarafından yönetilen anahtarlara güvenebilir veya kendi anahtarlarınızı kullanarak şifrelemeyi yönetebilirsiniz. Şifrelemeyi kendi anahtarlarınızla yönetmeyi seçerseniz, yönetilen disklerde tüm verileri şifrelemek ve çözmek için kullanılacak *müşteri tarafından yönetilen* bir anahtar belirtebilirsiniz. 

Sunucu tarafı şifreleme hakkında daha fazla bilgi edinmek için [Windows](../articles/virtual-machines/windows/disk-encryption.md) veya [Linux](../articles/virtual-machines/linux/disk-encryption.md)makalelerine bakın.

### <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi

Gelişmiş [Windows VM](../articles/virtual-machines/windows/encrypt-disks.md) ve [Linux VM](../articles/virtual-machines/linux/disk-encryption-overview.md) güvenliği ve uyumluluğu için Azure'daki sanal diskler şifrelenebilir. Windows VM'deki sanal diskler BitLocker kullanılarak istirahatte şifrelenir. Linux VM'lerde sanal diskler dm-crypt kullanılarak istirahatte şifrelenir. 

Azure'da sanal diskleri şifrelemek ücretsizdir. Şifreleme anahtarları yazılım koruması kullanılarak Azure Key Vault'ta depolanır veya fips 140-2 seviye 2 standartlarına göre onaylı Donanım Güvenlik Modülleri'nde (HSM) anahtarlarınızı içe aktarabilir veya oluşturabilirsiniz. Bu şifreleme anahtarları, VM'nize bağlı sanal diskleri şifrelemek ve şifresini çözmek için kullanılır. Bu şifreleme anahtarlarının denetimini siz elde elabilirsiniz ve bunların kullanımını denetleyebilirsiniz. Azure Etkin Dizin hizmeti ilkesi, VM'ler açık ve kapalı güç ve çalıştırılırken bu şifreleme anahtarlarını vermek için güvenli bir mekanizma sağlar.

## <a name="key-vault-and-ssh-keys"></a>Anahtar Kasası ve SSH Tuşları

Sırlar ve sertifikalar kaynak olarak modellenebilir ve [Key Vault](../articles/key-vault/key-vault-whatis.md)tarafından sağlanabilir. [Windows VM'ler](../articles/virtual-machines/windows/key-vault-setup.md) için anahtar kasaları oluşturmak için Azure PowerShell'i ve [Linux VM'leri](../articles/virtual-machines/linux/key-vault-setup.md)için Azure CLI'yi kullanabilirsiniz. Şifreleme için anahtarlar da oluşturabilirsiniz.

Anahtar kasa erişim ilkeleri, anahtarlara, sırlara ve sertifikalara ayrı ayrı izin verir. Örneğin, bir kullanıcıya parolalar için herhangi bir izin vermeden yalnızca anahtarlar için erişim verebilirsiniz. Ancak, anahtar, parola veya sertifikalara erişim izni kasa düzeyinde verilir. Başka bir deyişle, [anahtar kasa erişim ilkesi](../articles/key-vault/key-vault-secure-your-key-vault.md) nesne düzeyi izinlerini desteklemez.

VM'lere bağlandığınızda, oturum açmanın daha güvenli bir yolunu sağlamak için ortak anahtar şifrelemesini kullanmanız gerekir. Bu işlem, kullanıcı adı ve parola yerine kendinizi doğrulamak için güvenli kabuk (SSH) komutunu kullanan ortak ve özel anahtar alışverişini içerir. Parolalar, özellikle web sunucuları gibi Internet'e bakan VM'lerde kaba kuvvet saldırılarına karşı savunmasızdır. Güvenli bir kabuk (SSH) anahtar çifti yle, kimlik doğrulama için SSH tuşlarını kullanan bir [Linux VM](../articles/virtual-machines/linux/mac-create-ssh-keys.md) oluşturarak oturum açma parolalarına olan gereksinimi ortadan kaldırabilirsiniz. Windows VM'den Linux [VM'ye](../articles/virtual-machines/linux/ssh-from-windows.md) bağlanmak için SSH tuşlarını da kullanabilirsiniz.

## <a name="managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikler

Bulut uygulamaları oluştururken yaygın olarak karşılaşılan bir zorluk, bulut hizmetlerinde kimlik doğrulaması yapmak için kodunuzda bulunan kimlik bilgilerinin yönetimidir. Kimlik bilgilerinin güvenlik altında tutulması önemli bir görevdir. İdeal olan kimlik bilgilerinin geliştirici iş istasyonlarında asla gösterilmemesi ve kaynak denetimine kaydedilmemesidir. Azure Key Vault kimlik bilgilerini, gizli dizileri ve diğer anahtarları güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. 

Azure Active Directory (Azure AD) hizmetindeki Azure kaynakları yönetilen hizmetleri bu sorunu çözer. Bu özellik, Azure hizmetlerine Azure AD üzerinde otomatik olarak yönetilen bir kimlik sağlar. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri olmadan kimlik doğrulaması yapabilirsiniz.  VM üzerinde çalışan kodunuz, yalnızca VM içinden erişilebilen iki uç noktadan bir belirteç isteyebilir. Bu hizmet hakkında daha ayrıntılı bilgi için [Azure kaynaklarına](../articles/active-directory/managed-identities-azure-resources/overview.md) genel bakış sayfası için yönetilen kimlikleri inceleyin.   

## <a name="policies"></a>İlkeler

[Azure ilkeleri,](../articles/azure-policy/azure-policy-introduction.md) kuruluşunuzun [Windows VM'leri](../articles/virtual-machines/windows/policy.md) ve [Linux VM'leri](../articles/virtual-machines/linux/policy.md)için istenen davranışı tanımlamak için kullanılabilir. İlkeleri kullanarak, bir kuruluş kuruluş kuruluş genelinde çeşitli kuralları ve kuralları uygulayabilir. İstenilen davranışın uygulanması, kuruluşun başarısına katkıda bulunmakla birlikte riski azaltmaya yardımcı olabilir.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

[Rol tabanlı erişim denetimini (RBAC)](../articles/role-based-access-control/overview.md)kullanarak, ekibinizin görevlerini ayırabilir ve yalnızca VM'inizdeki kullanıcılara işlerini gerçekleştirmek için gereken erişim miktarını verebilirsiniz. VM'de herkese sınırsız izin vermek yerine, yalnızca belirli eylemlere izin verebilirsiniz. [Azure CLI](https://docs.microsoft.com/cli/azure/role)veya[Azure PowerShell'i](../articles/role-based-access-control/role-assignments-powershell.md)kullanarak [Azure portalındaki](../articles/role-based-access-control/role-assignments-portal.md)VM için erişim denetimini yapılandırabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
- [Linux](../articles/security/fundamentals/overview.md) veya [Windows](../articles/virtual-machines/windows/tutorial-azure-security.md)için Azure Güvenlik Merkezi'ni kullanarak sanal makine güvenliğini izlemek için adımları gözden geçirin.
