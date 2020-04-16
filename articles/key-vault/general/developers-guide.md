---
title: Azure Key Vault Geliştirici Kılavuzu
description: Geliştiriciler, Microsoft Azure ortamında şifreleme anahtarlarını yönetmek için Azure Key Vault'u kullanabilir.
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: mbaldwin
ms.openlocfilehash: 18e1ab1d01394d585cfb06ced8c5fbac04114177
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431962"
---
# <a name="azure-key-vault-developers-guide"></a>Azure Key Vault Geliştirici Kılavuzu

Key Vault, hassas bilgilere uygulamalarınız içinden güvenli bir şekilde erişmenizi sağlar:

- Anahtarlar ve sırlar kodu kendiniz yazmak zorunda kalmadan korunur ve uygulamalarınızdan kolayca kullanabilirsiniz.
- Temel yazılım özelliklerini sağlamaya konsantre olmak için müşterilerinizin kendi anahtarlarını sahip olabilir ve yönetebilirsiniz. Bu şekilde, uygulamalarınız müşterilerinizin kiracı anahtarları ve sırları için sorumluluk veya potansiyel sorumluluk sahibi olmayacaktır.
- Uygulamanız, imzalama ve şifreleme için anahtarları kullanabilir, ancak anahtar yönetimini uygulamanızdan harici tartmaya devam ederek çözümünüzün coğrafi olarak dağıtılmış bir uygulama olarak uygun olmasını sağlar.
- Anahtar Kasa sertifikalarını yönetin. Daha fazla bilgi için [Bkz. Sertifikalar](../certificates/about-certificates.md)

Azure Anahtar Kasası hakkında daha genel bilgi için [Key Vault nedir'e](overview.md)bakın).

## <a name="public-previews"></a>Genel Önizlemeler

Periyodik olarak, yeni bir Key Vault özelliğinin genel önizlemesini yayınlarız. Bunları deneyin ve geri bildirim e-posta adresimiz aracılığıyla azurekeyvault@microsoft.comne düşündüğünüzü bize bildirin.

## <a name="creating-and-managing-key-vaults"></a>Anahtar Kasaları Oluşturma ve Yönetme

Azure Key Vault kimlik bilgilerini ve diğer anahtarlarla gizli dizileri güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. Azure kaynakları için yönetilen kimlikler, Azure hizmetlerine Azure Etkin Dizin'de (Azure AD) otomatik olarak yönetilen bir kimlik vererek bu sorunu çözmeyi kolaylaştırır. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz. 

Azure kaynakları için yönetilen kimlikler hakkında daha fazla bilgi için [yönetilen kimliklere genel bakış'a](../../active-directory/managed-identities-azure-resources/overview.md)bakın. Azure AD ile çalışma hakkında daha fazla bilgi [için](../../active-directory/develop/active-directory-integrating-applications.md)bkz.

Anahtar kasanızda anahtarlar, sırlar veya sertifikalarla çalışmadan önce, aşağıdaki makalelerde açıklandığı gibi CLI, PowerShell, Resource Manager Şablonları veya REST aracılığıyla anahtar kasanızı oluşturur ve yönetirsiniz:

- [CLI ile Anahtar Kasaları oluşturma ve yönetme](../secrets/quick-create-cli.md)
- [PowerShell ile Anahtar Kasaları oluşturun ve yönetin](../secrets/quick-create-powershell.md)
- [Azure bağlantı noktası yla Anahtar Kasaları oluşturma ve yönetme](../secrets/quick-create-portal.md)
- [Python ile Anahtar Kasaları oluşturma ve yönetme](../secrets/quick-create-python.md)
- [Java ile Anahtar Kasaları oluşturma ve yönetme](../secrets/quick-create-java.md)
- [Node.js ile Anahtar Kasaları oluşturma ve yönetme](../secrets/quick-create-node.md)
- [.NET (v4 SDK) ile Anahtar Kasaları oluşturma ve yönetme](../secrets/quick-create-net.md)
- [Azure Kaynak Yöneticisi şablonu aracılığıyla önemli bir kasa oluşturun ve bir sır ekleyin](../secrets/quick-create-template.md)
- [REST ile Anahtar Kasaları oluşturma ve yönetme](/rest/api/keyvault/)


## <a name="coding-with-key-vault"></a>Anahtar Kasası ile Kodlama

Programcılar için Key Vault yönetim sistemi çeşitli arabirimlerden oluşur. Bu bölümde, tüm dillerin yanı sıra bazı kod örnekleri ne bağlantılar içerir. 

### <a name="supported-programming-and-scripting-languages"></a>Desteklenen programlama ve komut dosyası dilleri

#### <a name="rest"></a>REST

Tüm Key Vault kaynaklarınıza REST arabirimi üzerinden erişilebilir; kasalar, anahtarlar, sırlar, vb. 

[Anahtar Vault REST API Referans](/rest/api/keyvault/).

#### <a name="net"></a>.NET

[Key Vault için .NET API başvurusu.](/dotnet/api/overview/azure/key-vault?view=azure-dotnet)

.NET SDK'nın 2.x sürümü hakkında daha fazla bilgi için [Sürüm notlarına](dotnet2api-release-notes.md)bakın.

#### <a name="java"></a>Java

[Anahtar Vault için Java SDK](/java/api/overview/azure/keyvault)

#### <a name="nodejs"></a>Node.js

Düğüm.js'de, Key Vault yönetimi API'si ve Key Vault nesnesi API'si ayrıdır. Aşağıdaki genel bakış makalesi her ikisine de erişmenizi sağlar. 

[Node.js için Azure Anahtar Kasası modülleri](/javascript/api/overview/azure/key-vault?view=azure-node-latest)

#### <a name="python"></a>Python

[Python için Azure Key Vault kitaplıkları](/python/api/overview/azure/key-vault?view=azure-python)

#### <a name="azure-cli"></a>Azure CLI

[Anahtar Kasası için Azure CLI](/cli/azure/keyvault?view=azure-cli-latest)

#### <a name="azure-powershell"></a>Azure PowerShell 

[Anahtar Kasası için Azure PowerShell](/powershell/module/az.keyvault/?view=azps-3.6.1#key_vault)

### <a name="code-examples"></a>Kod örnekleri

Uygulamalarınızla birlikte Key Vault'u kullanarak tüm örnekler için bkz:

- [Azure Anahtar Kasası kod örnekleri](https://azure.microsoft.com/resources/samples/?service=key-vault) - Azure Anahtar Kasası için Kod Örnekleri. 
- [Azure'daki bir web uygulamasından](../secrets/quick-create-net.md) Azure Key Vault'u kullanmayı öğrenmenize yardımcı olmak için bir Web Uygulamasından Azure Key Vault 'u kullanın . 

## <a name="how-tos"></a>Nasıl yapılır belgeleri

Aşağıdaki makaleler ve senaryolar Azure Anahtar Atlama ile çalışmak için göreve özel kılavuz sağlar:

- [Abonelik taşındıktan sonra anahtar kasa kiracı kimliğini değiştirin](subscription-move-fix.md) - Azure aboneliğinizi kiracı A'dan kiracı B'ye taşıdığınızda, varolan anahtar kasalarınıza kiracı B'deki asıllar (kullanıcılar ve uygulamalar) erişemez.
- [Güvenlik duvarının arkasındaki Key Vault'a erişim](access-behind-firewall.md) - Anahtar kasa istemciuygulamasına erişmek için çeşitli işlevler için birden çok uç noktaya erişebilmesi gerekir.
- [Azure Anahtar Kasası için HSM Korumalı Anahtarları Oluşturma ve Aktarma](../keys/hsm-protected-keys.md) - Bu, Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı planlamanıza, oluşturmanıza ve aktarmanıza yardımcı olur.
- [Dağıtım sırasında güvenli değerler (parolalar gibi) nasıl geçirilir](../../azure-resource-manager/templates/key-vault-parameter.md) - Dağıtım sırasında parametre olarak güvenli bir değer (parola gibi) geçirmeniz gerektiğinde, bu değeri bir Azure Anahtar Kasası'nda gizli olarak depolayabilir ve diğer Kaynak Yöneticisi şablonlarında değere başvurun.
- SQL Server ile [genişletilebilir anahtar yönetimi için Key Vault nasıl kullanılır](https://msdn.microsoft.com/library/dn198405.aspx) - Azure Key Vault için SQL Server ve SQL-in-a-VM uygulamaları bağlantı için şifreleme anahtarlarını korumak için genişletilebilir Anahtar Yönetimi (EKM) sağlayıcısı olarak Azure Key Vault hizmeti kaldıraç sağlar; Saydam Veri Şifreleme, Yedekleme Şifreleme ve Sütun Düzeyi Şifreleme.
- [Key Vault'tan VM'lere Sertifikaları dağıtma -](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) Azure'da VM'de çalışan bir bulut uygulamasının bir sertifikaya ihtiyacı vardır. Bu sertifikayı bugün bu VM'ye nasıl sokabilirsin?
- [Key Vault'u uçtan uca anahtar döndürme ve denetleme yle nasıl ayarlaylayın](../secrets/key-rotation-log-monitoring.md) - Bu, Azure Key Vault ile anahtar döndürme ve denetlemenin nasıl ayarlanıştu.
- [Azure Web App Sertifikası'nı Key Vault üzerinden dağıtmak,]( https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) [Uygulama Hizmeti Sertifikası](https://azure.microsoft.com/blog/internals-of-app-service-certificate/) teklifinin bir parçası olarak Key Vault'ta depolanan sertifikaları dağıtmak için adım adım talimatlar sağlar.
- [Önemli bir kasaya erişmek için birçok başvuruya izin verme](group-permissions-for-apps.md) Key Vault erişim kontrol ilkesi 1024'e kadar girişi destekler. Ancak bir Azure Active Directory güvenlik grubu oluşturabilirsiniz. Bu güvenlik grubuna ilişkili tüm hizmet ilkelerini ekleyin ve ardından bu güvenlik grubuna Erişim izni ni Anahtar Kasası'na ekleyin.
- Azure ile Key Vaults'u tümleştirme ve kullanma konusunda daha fazla göreve özel kılavuz için, [Anahtar Kasası için Ryan Jones'un Azure Kaynak Yöneticisi şablonu örneklerine](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)bakın.
- [CLI ile Key Vault yumuşak silme nasıl kullanılacağı,](soft-delete-cli.md) bir anahtar kasasının ve yumuşak silme etkinleştirilmiş çeşitli anahtar tonoz nesnelerinin kullanım ve yaşam döngüsü boyunca size yol gösterin.
- [PowerShell ile Key Vault yumuşak silme nasıl kullanılır,](soft-delete-powershell.md) bir anahtar kasasının ve yumuşak silme etkinleştirilmiş çeşitli anahtar kasa nesnelerinin kullanım ve yaşam döngüsü boyunca size yol gösterin.

## <a name="integrated-with-key-vault"></a>Key Vault ile entegre

Bu makaleler, Key Vault'u kullanan veya bunlarla bütünleşen diğer senaryolar ve hizmetler hakkındadır.

- [Azure Disk Şifreleme,](../../security/fundamentals/encryption-overview.md) işletim sistemi ve veri diskleri için birim şifreleme sağlamak için Windows'un endüstri standardı [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) özelliğinden ve Linux'un [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) özelliğinden yararlanır. Çözüm, anahtar kasa aboneliğinizdeki disk şifreleme anahtarlarını ve sırlarını kontrol etmenize ve yönetmenize yardımcı olmak ve bunları yönetmenize yardımcı olmak ve sanal makine disklerinizdeki tüm verilerin Azure depolama alanınızda dinlenerek şifrelenmenizi sağlamak için Azure Key Vault ile entegre edilmiştir.
- [Azure Veri Gölü Deposu,](../../data-lake-store/data-lake-store-get-started-portal.md) hesapta depolanan verilerin şifrelemesi için seçenek sağlar. Anahtar yönetimi için, Veri Gölü Deposu Deposu'nda depolanan verilerin şifresini çözmek için gereken ana şifreleme anahtarlarınızı (MEKs) yönetmek için iki mod sağlar. Data Lake Store'un MEK'leri sizin için yönetmesine izin verebilir veya Azure Key Vault hesabınızı kullanarak MEK'lerin sahipliğini korumayı seçebilirsiniz. Veri Gölü Deposu hesabı oluştururken anahtar yönetim modunu belirtirsiniz.
- [Azure Bilgi Koruması,](/azure/information-protection/plan-implement-tenant-key) kendi kiracı anahtarınızı yönetemenizi sağlar. Örneğin, Microsoft'un kiracı anahtarınızı (varsayılan) yönetmesi yerine, kuruluşunuz için geçerli olan belirli düzenlemelere uymak için kendi kiracı anahtarınızı yönetebilirsiniz. Kendi kiracı anahtarınızın yönetilmesi, kendi anahtarını getir (BYOK) olarak da bilinir.

## <a name="key-vault-overviews-and-concepts"></a>Key Vault genel bakışları ve konseptleri

- [Anahtar Vault yumuşak silme davranışı](overview-soft-delete.md)) silinen nesnelerin kurtarılmasına izin veren bir özelliği açıklar, silme işlemi kazara veya kasıtlı olsun.
- [Key Vault istemcisi azaltma](overview-throttling.md) sizi temel azaltma kavramlarına yönlendirir ve uygulamanız için bir yaklaşım sunar.
- [Key Vault depolama hesap anahtarları na genel bakış](../secrets/overview-storage-keys.md)) Key Vault tümleştirmeSi Azure Depolama Hesapları anahtarlarını açıklar.
- [Key Vault güvenlik dünyaları](overview-security-worlds.md) bölgeler ve güvenlik alanları arasındaki ilişkileri açıklar.

## <a name="social"></a>Sosyal

- [Anahtar Vault Blog](https://aka.ms/kvblog)
- [Anahtar Vault Forumu](https://aka.ms/kvforum)

## <a name="supporting-libraries"></a>Destekleyici Kütüphaneler

- [Microsoft Azure Key Vault Core Library,](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) tanımlayıcılardan anahtarları bulmak ve tuşları kullanarak işlemleri gerçekleştirmek için **IKey** ve **IKeyResolver** arabirimleri sağlar.
- [Microsoft Azure Anahtar Vault Uzantıları,](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) Azure Key Vault için genişletilmiş özellikler sağlar.
