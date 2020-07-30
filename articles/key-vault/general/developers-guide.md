---
title: Azure Key Vault Geliştirici Kılavuzu
description: Geliştiriciler, Microsoft Azure ortamındaki şifreleme anahtarlarını yönetmek için Azure Key Vault kullanabilir.
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: mbaldwin
ms.openlocfilehash: 4c28299758150f56e3f47156382d8a6245a0cf52
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87386223"
---
# <a name="azure-key-vault-developers-guide"></a>Azure Key Vault Geliştirici Kılavuzu

Key Vault, uygulamalarınızın içinden hassas bilgilere güvenli bir şekilde erişmenizi sağlar:

- Anahtarlar ve gizlilikler, kodu kendiniz yazmak zorunda kalmadan korunur ve bunları uygulamalarınızdan kolayca kullanabilirsiniz.
- Müşterilerinizin temel yazılım özelliklerini sağlamaya odaklanabilmeniz için kendi anahtarlarını sahip ve bunları yönetebileceksiniz. Bu şekilde, uygulamalarınız müşterilerinizin kiracı anahtarları ve gizli dizileri için sorumluluğa ya da potansiyel sorumluluğa sahip olmayacaktır.
- Uygulamanız imzalama ve şifreleme için anahtarlar kullanabilir, ancak çözümünüz coğrafi olarak dağıtılmış bir uygulama olarak uygun olacak şekilde uygulamanızda önemli yönetim sağlar.
- Key Vault sertifikalarını yönetin. Daha fazla bilgi için bkz. [Sertifikalar](../certificates/about-certificates.md)

Azure Key Vault hakkında daha fazla genel bilgi için bkz. [Key Vault nedir](overview.md)).

## <a name="public-previews"></a>Genel önizlemeler

Düzenli olarak, yeni bir Key Vault özelliğinin genel önizlemesini yayınlarız. Bunları deneyin ve azurekeyvault@microsoft.com geri bildirim e-posta adresi ile düşüncelerinizi öğrenmemizi sağlayın.

## <a name="creating-and-managing-key-vaults"></a>Anahtar kasaları oluşturma ve yönetme

Azure Key Vault kimlik bilgilerini ve diğer anahtarlarla gizli dizileri güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. Azure kaynaklarına yönelik yönetilen kimlikler, Azure hizmetlerine Azure Active Directory (Azure AD) içinde otomatik olarak yönetilen bir kimlik vererek bu sorunu daha kolay bir şekilde çözmenizi sağlar. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz. 

Azure kaynakları için Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [yönetilen kimliklere genel bakış](../../active-directory/managed-identities-azure-resources/overview.md). Azure AD ile çalışma hakkında daha fazla bilgi için bkz. [uygulamaları Azure Active Directory tümleştirme](../../active-directory/develop/active-directory-integrating-applications.md).

Anahtar kasaınızdaki anahtarlar, gizlilikler veya sertifikalarla çalışmadan önce, aşağıdaki makalelerde açıklandığı gibi CLı, PowerShell, Kaynak Yöneticisi şablonları veya REST aracılığıyla anahtar kasanızı oluşturup yönetirsiniz:

- [CLı ile Anahtar Kasası oluşturma ve yönetme](../secrets/quick-create-cli.md)
- [PowerShell ile Anahtar Kasası oluşturma ve yönetme](../secrets/quick-create-powershell.md)
- [Azure portal ile anahtar kasaları oluşturma ve yönetme](../secrets/quick-create-portal.md)
- [Python ile anahtar kasaları oluşturma ve yönetme](../secrets/quick-create-python.md)
- [Java ile anahtar kasaları oluşturma ve yönetme](../secrets/quick-create-java.md)
- [Node.jsanahtarla anahtar kasaları oluşturma ve yönetme](../secrets/quick-create-node.md)
- [.NET ile anahtar kasaları oluşturma ve yönetme (v4 SDK)](../secrets/quick-create-net.md)
- [Bir Anahtar Kasası oluşturma ve bir Azure Resource Manager şablonu aracılığıyla gizli dizi ekleme](../secrets/quick-create-template.md)
- [REST ile anahtar kasaları oluşturma ve yönetme](/rest/api/keyvault/)


## <a name="coding-with-key-vault"></a>Key Vault kodlama

Programcılar için Key Vault yönetim sistemi çeşitli arabirimlerden oluşur. Bu bölüm, bazı dillerin ve bazı kod örneklerinin bağlantılarını içerir. 

### <a name="supported-programming-and-scripting-languages"></a>Desteklenen programlama ve betik oluşturma dilleri

#### <a name="rest"></a>REST

Key Vault kaynaklarınızın tamamına REST arabiriminden erişilebilir. Kasalar, anahtarlar, gizlilikler vb. 

[Key Vault REST API başvurusu](/rest/api/keyvault/).

#### <a name="net"></a>.NET

[Key Vault için .NET API başvurusu](/dotnet/api/overview/azure/key-vault?view=azure-dotnet).

.NET SDK 'sının 2. x sürümü hakkında daha fazla bilgi için [sürüm notlarına](dotnet2api-release-notes.md)bakın.

#### <a name="java"></a>Java

[Key Vault için Java SDK](/java/api/overview/azure/keyvault)

#### <a name="nodejs"></a>Node.js

Node.js, Key Vault yönetim API 'SI ve Key Vault nesne API 'SI ayrıdır. Aşağıdaki genel bakış makalesi, her ikisine de erişmenizi sağlar. 

[Node.jsiçin Azure Key Vault modüller](https://docs.microsoft.com/javascript/api/overview/azure/key-vault-index?view=azure-node-latest)

#### <a name="python"></a>Python

[Python için Azure Key Vault kitaplıkları](https://docs.microsoft.com/python/api/overview/azure/key-vault-index?view=azure-python)

#### <a name="azure-cli"></a>Azure CLI

[Key Vault için Azure CLı](/cli/azure/keyvault?view=azure-cli-latest)

#### <a name="azure-powershell"></a>Azure PowerShell 

[Key Vault için Azure PowerShell](/powershell/module/az.keyvault/?view=azps-3.6.1#key_vault)

### <a name="code-examples"></a>Kod örnekleri

Uygulamalarınızla Key Vault kullanan tüm örnekler için bkz.:

- [Kod örneklerini Azure Key Vault](https://azure.microsoft.com/resources/samples/?service=key-vault) -Azure Key Vault Için kod örnekleri. 
- [Bir Web uygulamasından Azure Key Vault kullanın](../secrets/quick-create-net.md) -Azure 'da bir web uygulamasından Azure Key Vault kullanmayı öğrenmenize yardımcı olmak için öğreticiden yararlanabilirsiniz. 

## <a name="how-tos"></a>Nasıl yapılır belgeleri

Aşağıdaki makaleler ve senaryolar Azure Key Vault çalışmak için göreve özgü rehberlik sağlar:

- [Anahtar Kasası KIRACı kimliğini değiştirme abonelik taşıma sonrasında](move-subscription.md) , Azure aboneliğinizi A Kiracısından b kiracısına taşıdığınızda mevcut anahtar kasalarınız, b kiracısındaki sorumlular (kullanıcılar ve uygulamalar) tarafından erişilemez. bu kılavuzu kullanarak bunu düzeltin.
- [Güvenlik duvarının arkasındaki Key Vault erişme](access-behind-firewall.md) -anahtar kasasına erişmek için Anahtar Kasası istemci uygulamanızın çeşitli işlevlere ilişkin birden çok uç noktaya erişebilmesi gerekir.
- [Azure Key Vault IÇIN HSM korumalı anahtarlar oluşturma ve aktarma](../keys/hsm-protected-keys.md) -bu, Azure Key Vault ile kullanmak IÇIN kendi HSM korumalı anahtarlarınızı planlayıp oluşturmanıza yardımcı olur.
- [Dağıtım sırasında güvenli değerleri (parolalar](../../azure-resource-manager/templates/key-vault-parameter.md) gibi) geçirme-dağıtım sırasında bir parametre olarak güvenli bir değer (parola gibi) iletmeniz gerektiğinde, bu değeri bir Azure Key Vault gizli olarak saklayabilir ve diğer kaynak yöneticisi şablonlarındaki değere başvurabilirsiniz.
- [SQL Server ile Genişletilebilir anahtar yönetimi için Key Vault kullanma](https://msdn.microsoft.com/library/dn198405.aspx) -Azure Key Vault için SQL Server Bağlayıcısı, SQL Server ve SQL-ın-a-VM ' y i, uygulamalar için şifreleme anahtarlarını korumak Için Genişletilebilir anahtar YÖNETIMI (ekm) sağlayıcısı olarak Azure Key Vault hizmetinden faydalanabilir. Saydam Veri Şifrelemesi, yedekleme şifrelemesi ve sütun düzeyinde şifreleme.
- [Key Vault 'Ten sanal makinelere sertifika dağıtma](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) -Azure 'da VM 'de çalışan bir bulut uygulaması, bir sertifikaya ihtiyaç duyuyor. Bu sertifikayı bugün bu sanal makineye nasıl alırsınız?
- [Key Vault aracılığıyla Azure Web App sertifikası dağıtmak]( https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) , [App Service sertifikası](https://azure.microsoft.com/blog/internals-of-app-service-certificate/) teklifin bir parçası olarak Key Vault depolanan sertifikaları dağıtmaya yönelik adım adım yönergeler sağlar.
- [Birçok uygulamaya bir anahtar kasasına erişmek için Izin verme](group-permissions-for-apps.md) Key Vault Access Control Policy en fazla 1024 girişi destekler. Ancak, Azure Active Directory bir güvenlik grubu oluşturabilirsiniz. Tüm ilişkili hizmet sorumlularını bu güvenlik grubuna ekleyin ve Key Vault için bu güvenlik grubuna erişim izni verin.
- Anahtar kasalarını Azure ile tümleştirmeyle ve kullanmaya yönelik daha fazla göreve özgü rehberlik için bkz. [Ryan Jones ' Azure Resource Manager şablon örnekleri Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
- [CLI ile geçici silme Key Vault kullanımı](soft-delete-cli.md) , anahtar kasasının kullanımı ve yaşam döngüsü boyunca ve geçici silme etkin olan çeşitli Anahtar Kasası nesnelerinde size rehberlik eder.
- [PowerShell ile geçici silme Key Vault](soft-delete-powershell.md) , bir anahtar kasasının kullanımı ve yaşam döngüsü ve yumuşak silme etkin olan çeşitli Anahtar Kasası nesneleri boyunca size rehberlik eder.

## <a name="integrated-with-key-vault"></a>Key Vault ile tümleşik

Bu makaleler, Key Vault kullanan veya ile tümleştirilebilen diğer senaryolar ve hizmetlerle ilgilidir.

- [Azure disk şifrelemesi](../../security/fundamentals/encryption-overview.md) , işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un sektör standardı [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) özelliğini ve Linux 'un [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) özelliğini kullanır. Çözüm, Anahtar Kasası aboneliğinizdeki disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize yardımcı olmak üzere Azure Key Vault ile tümleşiktir. böylece, sanal makine disklerindeki tüm verilerin Azure depolamadaki geri kalanında şifrelenmesini sağlayabilirsiniz.
- [Azure Data Lake Store](../../data-lake-store/data-lake-store-get-started-portal.md) hesapta depolanan verilerin şifrelenmesi için seçenek sağlar. Anahtar yönetimi için Data Lake Store, ana şifreleme anahtarlarınızı (MEKs) yönetmek için, Data Lake Store depolanan verilerin şifresini çözmek için gereken iki mod sağlar. Sizin için, MEKs 'leri Data Lake Store yönetebilir veya Azure Key Vault hesabınızı kullanarak MEKs 'in sahipliğini tutmayı seçebilirsiniz. Data Lake Store bir hesap oluştururken anahtar yönetiminin modunu belirtirsiniz.
- [Azure Information Protection](/azure/information-protection/plan-implement-tenant-key) kendi kiracı anahtarınızı yöneticinize etmenizi sağlar. Örneğin, kiracı anahtarınızı Microsoft 'un yönetmesi yerine (varsayılan), kuruluşunuz için uygun olan belirli düzenlemelere uymak üzere kendi kiracı anahtarınızı yönetebilirsiniz. Kendi kiracı anahtarınızın yönetilmesi, kendi anahtarını getir (BYOK) olarak da bilinir.

## <a name="key-vault-overviews-and-concepts"></a>Key Vault genel bakış ve kavramlar

- [Key Vault geçici silme davranışı](soft-delete-overview.md)) silme işleminin yanlışlıkla mi yoksa bilerek mi olduğunu belirten silinen nesnelerin kurtarılmasına izin veren bir özelliği açıklar.
- [Key Vault istemci kısıtlama](overview-throttling.md) , sizi daraltma temel kavramlara yönlendirir ve uygulamanız için bir yaklaşım sunar.
- [Key Vault depolama hesabı anahtarlarına genel bakış](../secrets/overview-storage-keys.md)) Key Vault tümleştirme Azure depolama hesapları anahtarlarını açıklar.
- [Key Vault güvenliği](overview-security-worlds.md) , bölgeler ve güvenlik alanları arasındaki ilişkileri açıklar.

## <a name="social"></a>Sosyal

- [Key Vault blogu](https://aka.ms/kvblog)
- [Key Vault Forumu](https://aka.ms/kvforum)

## <a name="supporting-libraries"></a>Destekleyici kitaplıklar

- [Microsoft Azure Key Vault Çekirdek Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) , tanımlayıcılardan anahtar bulmaya ve anahtarlarla işlem gerçekleştirmeye yönelik **Ikey** ve **ıkeyresolver** arabirimlerini sağlar.
- [Microsoft Azure Key Vault uzantıları](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) Azure Key Vault için genişletilmiş yetenekler sağlar.
