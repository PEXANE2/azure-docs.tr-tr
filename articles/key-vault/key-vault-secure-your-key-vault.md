---
title: Önemli bir kasaya güvenli erişim - Azure Key Vault | Microsoft Dokümanlar
description: Azure Key Vault, anahtarlar ve sırlar için erişim izinlerini yönetin. Key Vault için kimlik doğrulama ve yetkilendirme modelini ve anahtar kasanızın nasıl güvenli hale alınabildiğini kapsar.
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: eac3850cfa0684bd1751cf7b88b4ff8e92667293
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284388"
---
# <a name="secure-access-to-a-key-vault"></a>Önemli bir kasaya güvenli erişim

Azure Key Vault, sertifikalar, bağlantı dizeleri ve parolalar gibi şifreleme anahtarlarını ve sırlarını koruyan bir bulut hizmetidir. Bu veriler hassas ve iş açısından kritik olduğundan, yalnızca yetkili uygulamalara ve kullanıcılara izin vererek anahtar kasalarınıza erişimi güvence altına almanız gerekir. Bu makalede, Key Vault erişim modeline genel bir bakış sağlar. Kimlik doğrulamayı ve yetkilendirmeyi açıklar ve anahtar kasalarınıza nasıl güvenli erişimi sağlayacağınızı açıklar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="access-model-overview"></a>Erişim modeline genel bakış

Anahtar kasasına erişim iki arayüz üzerinden kontrol edilir: **yönetim düzlemi** ve **veri düzlemi.** Yönetim düzlemi Key Vault'u yönettiğiniz yerdir. Bu düzlemdeki işlemler arasında anahtar kasaları oluşturma ve silme, Key Vault özelliklerini alma ve erişim ilkelerini güncelleştirme yer alıyor. Veri düzlemi, anahtar kasasında depolanan verilerle çalıştığınız yerdir. Anahtarları, sırları ve sertifikaları ekleyebilir, silebilir ve değiştirebilirsiniz.

Her iki düzlemde de anahtar kasasına erişmek için tüm arayanların (kullanıcılar veya uygulamalar) uygun kimlik doğrulama ve yetkilendirmeye sahip olması gerekir. Kimlik doğrulaması, arayanın kimliğini belirler. Yetkilendirme, arayanın yürütebileceği işlemleri belirler.

Her iki düzlem de kimlik doğrulaması için Azure Active Directory (Azure AD) kullanır. Yetkilendirme için yönetim düzlemi rol tabanlı erişim denetimi (RBAC) ve veri düzlemi bir Key Vault erişim ilkesi kullanır.

## <a name="active-directory-authentication"></a>Etkin Dizin kimlik doğrulaması

Bir Azure aboneliğinde önemli bir kasa oluşturduğunuzda, bu kasa otomatik olarak aboneliğin Azure AD kiracısıyla ilişkilendirlenir. Her iki düzlemdeki tüm arayanların bu kiracıya kaydolması ve anahtar kasasına erişmek için kimlik doğrulaması yapması gerekir. Her iki durumda da, uygulamalar Key Vault'a iki şekilde erişebilir:

- **Kullanıcı artı uygulama erişimi**: Uygulama, oturum açmış bir kullanıcı adına Key Vault'a erişir. Bu erişim türüne örnek olarak Azure PowerShell ve Azure portalı verilebilir. Kullanıcı erişimi iki şekilde verilir. Kullanıcılar Key Vault'a herhangi bir uygulamadan erişebilir veya belirli bir uygulamayı _(bileşik kimlik_olarak adlandırılır) kullanmaları gerekir.
- **Yalnızca uygulama erişimi**: Uygulama daemon hizmeti veya arka plan işi olarak çalışır. Başvuru kimliği anahtar kasasına erişim hakkı verilir.

Her iki erişim türü için de uygulama Azure AD ile kimlik doğrulaması olur. Uygulama, uygulama türüne göre desteklenen herhangi bir [kimlik doğrulama yöntemi](../active-directory/develop/authentication-scenarios.md) kullanır. Uygulama, erişim sağlamak için düzlemdeki bir kaynak için bir belirteç edinir. Kaynak, Azure ortamını temel alan yönetim veya veri düzleminde bir uç noktadır. Uygulama belirteci kullanır ve Key Vault bir REST API isteği gönderir. Daha fazla bilgi edinmek için, tüm kimlik doğrulama akışını gözden [geçirin.](../active-directory/develop/v2-oauth2-auth-code-flow.md)

Her iki düzlemde kimlik doğrulama için tek bir mekanizma modelinin birkaç faydası vardır:

- Kuruluşlar, kuruluşlarındaki tüm önemli kasalara erişimi merkezi olarak denetleyebilir.
- Bir kullanıcı ayrılırsa, kuruluştaki tüm önemli kasalara anında erişim kaybeder.
- Kuruluşlar, azure AD'deki ek güvenlik için çok faktörlü kimlik doğrulamayı etkinleştirmek gibi seçenekleri kullanarak kimlik doğrulamayı özelleştirebilir.

## <a name="resource-endpoints"></a>Kaynak uç noktaları

Uygulamalar uç noktalardan düzlemlere erişir. İki düzlemiçin erişim denetimleri bağımsız çalışır. Bir başvuruya anahtar kasasında ki anahtarları kullanmak için erişim sağlamak için, Key Vault erişim ilkesini kullanarak veri düzlemi erişimi sağlarsınız. Bir kullanıcıya Key Vault özelliklerine ve etiketlerine okuma erişimi sağlamak, ancak verilere (anahtarlar, sırlar veya sertifikalar) erişmemek için, RBAC ile yönetim düzlemi erişimi sağlarsınız.

Aşağıdaki tablo, yönetim ve veri düzlemleri için uç noktaları gösterir.

| Erişim&nbsp;düzlemi | Erişim uç noktaları | İşlemler | Erişim&nbsp;kontrol mekanizması |
| --- | --- | --- | --- |
| Yönetim düzlemi | **Küresel:**<br> management.azure.com:443<br><br> **Azure Çin 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure ABD Hükümeti:**<br> management.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> management.microsoftazure.de:443 | Anahtar kasalarını oluşturma, okuma, güncelleme ve silme<br><br>Anahtar Kasası erişim ilkelerini ayarlama<br><br>Anahtar Kasa etiketlerini ayarla | Azure Kaynak Yöneticisi RBAC |
| Veri düzlemi | **Küresel:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure Çin 21Vianet:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure ABD Hükümeti:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 | Anahtarlar: şifreyi çözmek, şifrelemek,<br> açmak, sarmak, doğrulamak, imzalamak,<br> almak, listele, güncellemek, oluşturmak,<br> alma, silme, yedekleme, geri yükleme<br><br> Sırlar: almak, listele, ayarlamak, silmek | Key Vault erişim ilkesi |

## <a name="management-plane-and-rbac"></a>Yönetim düzlemi ve RBAC

Yönetim düzleminde, bir arayanın yürütebileceği işlemleri yetkilendirmek için RBAC(Role Based Access Control) kullanırsınız. RBAC modelinde, her Azure aboneliğinde Azure AD örneği vardır. Bu dizinden kullanıcılara, gruplara ve uygulamalara erişim izni verirsiniz. Azure Kaynak Yöneticisi dağıtım modelini kullanan Azure aboneliğindeki kaynakları yönetmek için erişim verilir. Erişim sağlamak için [Azure portalını](https://portal.azure.com/), [Azure CLI'yi, Azure](../cli-install-nodejs.md) [PowerShell'i](/powershell/azureps-cmdlets-docs)veya [Azure Kaynak Yöneticisi REST API'lerini](https://msdn.microsoft.com/library/azure/dn906885.aspx)kullanın.

Kaynak grubunda önemli bir kasa oluşturur ve Azure AD'yi kullanarak erişimi yönetirsiniz. Kullanıcılara veya gruplara bir kaynak grubundaki anahtar kasaları yönetme olanağı verirsiniz. Uygun RBAC rolleri atayarak belirli bir kapsam düzeyinde erişim verirsiniz. Anahtar kasalarını yönetmek için bir kullanıcıya erişim izni `key vault Contributor` vermek için, kullanıcıya belirli bir kapsamda önceden tanımlanmış bir rol atarsınız. Aşağıdaki kapsam düzeyleri bir RBAC rolüne atanabilir:

- **Abonelik**: Abonelik düzeyinde atanan bir RBAC rolü, bu abonelikteki tüm kaynak grupları ve kaynaklar için geçerlidir.
- **Kaynak grubu**: Kaynak grubu düzeyinde atanan bir RBAC rolü, bu kaynak grubundaki tüm kaynaklar için geçerlidir.
- **Belirli kaynak**: Belirli bir kaynak için atanan bir RBAC rolü bu kaynağa uygulanır. Bu durumda, kaynak belirli bir anahtar kasasıdır.

Önceden tanımlanmış birkaç rol vardır. Önceden tanımlanmış bir rol gereksinimlerinize uymuyorsa, kendi rolünüzü tanımlayabilirsiniz. Daha fazla bilgi için Bkz. [RBAC: Yerleşik roller.](../role-based-access-control/built-in-roles.md)

> [!IMPORTANT]
> Bir kullanıcının `Contributor` önemli bir kasa yönetim düzlemi için izinleri varsa, anahtar kasa erişim ilkesi ayarlayarak kullanıcı veri düzlemine erişim izni verebilir. Anahtar kasalarınıza kimin `Contributor` rol erişimi olduğunu sıkı bir şekilde kontrol edmelisiniz. Anahtar kasalarınıza, anahtarlarınıza, sırlarınıza ve sertifikalarınıza yalnızca yetkili kişilerin erişebilmesini ve yönetebilmesini sağlayın.
>

<a id="data-plane-access-control"></a>
## <a name="data-plane-and-access-policies"></a>Veri düzlemi ve erişim ilkeleri

Anahtar kasası için Key Vault erişim ilkelerini ayarlayarak veri düzlemi erişimi verirsiniz. Bu erişim ilkelerini ayarlamak için, bir kullanıcının, grubun veya uygulamanın bu anahtar kasası için yönetim düzlemi için `Contributor` izinleri olması gerekir.

Anahtar kasasında ki anahtarlar veya sırlar için belirli işlemleri yürütmek için bir kullanıcıya, gruba veya uygulamaya erişim verirsiniz. Key Vault, önemli bir kasa için en fazla 1.024 erişim ilkesi girişini destekler. Veri düzleminin birden çok kullanıcıya erişimini sağlamak için bir Azure AD güvenlik grubu oluşturun ve kullanıcıları bu gruba ekleyin.

<a id="key-vault-access-policies"></a>Key Vault erişim ilkeleri, anahtarlara, sırlara ve sertifikaya ayrı ayrı izin verir. Kullanıcıya yalnızca anahtarlara erişim izni verebilirsiniz, sırlara değil. Anahtarlar, sırlar ve sertifikalar için erişim izinleri kasa düzeyindedir. Key Vault erişim ilkeleri, belirli bir anahtar, gizli veya sertifika gibi parçalı, nesne düzeyinde izinleri desteklemez. Önemli bir kasa için erişim ilkeleri ayarlamak için [Azure portalını,](https://portal.azure.com/) [Azure CLI'yi, Azure](../cli-install-nodejs.md) [PowerShell'i](/powershell/azureps-cmdlets-docs)veya [Key Vault Management REST API'lerini](https://msdn.microsoft.com/library/azure/mt620024.aspx)kullanın.

> [!IMPORTANT]
> Anahtar Vault erişim ilkeleri kasa düzeyinde geçerlidir. Bir kullanıcıya anahtar oluşturma ve silme izni verildiğinde, bu işlemleri bu anahtar kasasındaki tüm anahtarlarda gerçekleştirebilir.
>

Azure Key Vault için [sanal ağ hizmeti bitiş noktalarını](key-vault-overview-vnet-service-endpoints.md)kullanarak veri düzlemi erişimini kısıtlayabilirsiniz. Ek bir güvenlik katmanı için [güvenlik duvarlarını ve sanal ağ kurallarını](key-vault-network-security.md) yapılandırabilirsiniz.

## <a name="example"></a>Örnek

Bu örnekte, verileri depolamak için TLS/SSL sertifikası, Azure Depolama ve imza işlemleri için RSA 2.048 bit anahtar kullanan bir uygulama geliştiriyoruz. Uygulamamız bir Azure sanal makinede (VM) (veya sanal makine ölçeği kümesi) çalışır. Uygulama sırlarını saklamak için anahtar kasası kullanabiliriz. Uygulama tarafından kullanılan bootstrap sertifikasını Azure AD ile kimlik doğrulaması yapmak için depolayabiliriz.

Aşağıdaki saklanan anahtarlara ve sırlara erişmemiz gerekir:
- **TLS/SSL sertifikası**: TLS/SSL için kullanılır.
- **Depolama anahtarı**: Depolama hesabına erişmek için kullanılır.
- **RSA 2.048 bit anahtar**: İşaret işlemleri için kullanılır.
- **Bootstrap sertifikası**: Azure AD ile kimlik doğrulaması yapmak için kullanılır. Erişim verildikten sonra, depolama anahtarını alabilir ve imzalamak için RSA anahtarını kullanabiliriz.

Uygulamamızı kimlerin yönetebileceğini, dağıtabileceğini ve denetlenebilenleri belirtmek için aşağıdaki rolleri tanımlamamız gerekir:
- **Güvenlik ekibi**: MSo (Güvenlik Şefi) veya benzeri katkıda bulunanların ofisinden BT personeli. Güvenlik ekibi sırların doğru tutulmasından sorumludur. Sırlar arasında TLS/SSL sertifikaları, imza için RSA anahtarları, bağlantı dizeleri ve depolama hesabı anahtarları yer alabilir.
- **Geliştiriciler ve operatörler**: Uygulamayı geliştiren ve Azure'da dağıtan personel. Bu ekibin üyeleri güvenlik personelinin bir parçası değil. TLS/SSL sertifikaları ve RSA anahtarları gibi hassas verilere erişememeleri gerekir. Yalnızca dağıttıkları uygulama nın hassas verilere erişimi olmalıdır.
- **Denetçiler**: Bu rol, geliştirme veya genel BT personeli nezdinde üyesi olmayan katkıda bulunanlar içindir. Güvenlik standartlarına uygunluğu sağlamak için sertifikaların, anahtarların ve sırların kullanımını ve bakımını gözden geçirin.

Uygulamamızın kapsamı dışında başka bir rol daha vardır: abonelik (veya kaynak grubu) yöneticisi. Abonelik yöneticisi, güvenlik ekibi için ilk erişim izinlerini ayarlar. Uygulamanın gerektirdiği kaynaklara sahip bir kaynak grubu kullanarak güvenlik ekibine erişim sağlarlar.

Rollerimiz için aşağıdaki işlemleri yetkilendirmemiz gerekir:

**Güvenlik ekibi**
- Anahtar kasaları oluşturun.
- Anahtar Kasa günlüğe kaydetmeyi açın.
- Anahtarları ve sırları ekleyin.
- Olağanüstü durum kurtarma için anahtarların yedeklerini oluşturun.
- Belirli işlemler için kullanıcılara ve uygulamalara izin vermek için Key Vault erişim ilkelerini ayarlayın.
- Anahtarları ve sırları periyodik olarak yuvarlayın.

**Geliştiriciler ve operatörler**
- İmzalama için bootstrap ve TLS/SSL sertifikaları (parmak izleri), depolama anahtarı (gizli URI) ve RSA anahtarı (anahtar URI) için güvenlik ekibinden referanslar alın.
- Tuşlara ve sırlara programlı olarak erişmek için uygulamayı geliştirin ve dağıtın.

**Denetçiler**
- Anahtarların ve sırların doğru kullanımını ve veri güvenliği standartlarına uygunluğu onaylamak için Key Vault günlüklerini gözden geçirin.

Aşağıdaki tabloda rollerimiz ve uygulamamız için erişim izinleri özetlenmiştir.

| Rol | Yönetim düzlemi izinleri | Veri düzlemi izinleri |
| --- | --- | --- |
| Güvenlik ekibi | Anahtar Vault Katılımcı | Anahtarlar: yedekleme, oluşturma, silme, alma, içeri aktarma, listeleme, geri yükleme<br>Sırlar: tüm işlemler |
| Geliştiriciler&nbsp;ve operatörler | Key Vault dağıtma izni<br><br> **Not**: Bu izin, dağıtılan VM'lerin önemli bir kasadan sırları getirmesine izin verir. | None |
| Denetçiler | None | Anahtarlar: listeleme<br>Parolalar: listeleme<br><br> **Not**: Bu izin, denetçilerin günlüklerde yayılmayan anahtarlar ve sırlar için öznitelikleri (etiketler, etkinleştirme tarihleri, son kullanma tarihleri) denetlemesine olanak tanır. |
| Uygulama | None | Anahtarlar: imzalama<br>Parolalar: imzalama |

Üç takım rolü, Key Vault izinleriyle birlikte diğer kaynaklara erişmeye ihtiyaç duyar. VM'leri (veya Azure Uygulama Hizmetinin Web Apps `Contributor` özelliğini) dağıtmak için geliştiricilerin ve operatörlerin bu kaynak türlerine erişmeleri gerekir. Denetçilerin, Anahtar Kasa günlüklerinin depolandığı Depolama hesabına okuma erişimi ne kadar olması gerekir.

Sertifikaların, erişim anahtarlarının ve sırların programlı olarak nasıl dağıtılacak tır.
- Müşteri tarafından yönetilen bir anahtar kasasından (blog gönderisi) [sertifikaları VM'lere](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) nasıl dağıtılayacaklarını öğrenin.
- Azure [Key Vault istemci örneklerini](https://www.microsoft.com/download/details.aspx?id=45343)indirin. Bu içerik, önemli bir kasaya erişmek için Azure AD'de kimlik doğrulaması yapmak için bir önyükleme sertifikasının nasıl kullanılacağını göstermektedir.

Azure portalını kullanarak erişim izinlerinin çoğunu verebilirsiniz. Parçalı izinler vermek için Azure PowerShell'i veya Azure CLI'yi kullanabilirsiniz.

Bu bölümdeki PowerShell parçacıkları aşağıdaki varsayımlarla oluşturulmuştur:
- Azure AD yöneticisi üç rolü temsil edecek güvenlik grupları oluşturdu: Contoso Security Team, Contoso App DevOps ve Contoso App Auditors. Yönetici, kullanıcıları kendi gruplarına ekledi.
- Tüm kaynaklar **ContosoAppRG** kaynak grubunda yer alır.
- Anahtar Kasa günlükleri **contosologdepolama** hesabında saklanır.
- **ContosoKeyVault** anahtar kasası ve **contosologdepolama** depolama hesabı aynı Azure konumundadır.

Abonelik yöneticisi güvenlik `key vault Contributor` ekibine ve `User Access Administrator` rolleri atar. Bu roller, güvenlik ekibinin **contosoAppRG** kaynak grubunda bulunan diğer kaynaklara ve önemli kasalara erişimi yönetmesine olanak sağlar.

```powershell
New-AzRoleAssignment -ObjectId (Get-AzADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzRoleAssignment -ObjectId (Get-AzADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Güvenlik ekibi önemli bir kasa oluşturur ve günlüğe kaydetme ve erişim izinleri ayarlar. Key Vault erişim ilkesi izinleri hakkında ayrıntılı bilgi için [Azure Anahtar Kasası anahtarları, sırları ve sertifikaları hakkında](about-keys-secrets-and-certificates.md)bilgi alabilirsiniz.

```powershell
# Create a key vault and enable logging
$sa = Get-AzStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzKeyVault -Name ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent

# Set up data plane permissions for the Contoso Security Team role
Set-AzKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge

# Set up management plane permissions for the Contoso App DevOps role
# Create the new role from an existing role
$devopsrole = Get-AzRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App DevOps"
$devopsrole.Description = "Can deploy VMs that need secrets from a key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permissions for the Contoso App DevOps role so members can deploy VMs with secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzRoleDefinition -Role $devopsrole

# Assign the new role to the Contoso App DevOps security group
New-AzRoleAssignment -ObjectId (Get-AzADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Set up data plane permissions for the Contoso App Auditors role
Set-AzKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

Tanımlanan özel rollerimiz yalnızca **ContosoAppRG** kaynak grubunun oluşturulduğu aboneye devredilebilir. Diğer aboneliklerde diğer projeler için özel bir rol kullanmak için, rolün kapsamına başka abonelikler ekleyin.

DevOps personelimiz için, anahtar kasa `deploy/action` izni için özel rol ataması kaynak grubuna yöneliktir. Yalnızca **ContosoAppRG** kaynak grubunda oluşturulan VM'lerin sırlara (TLS/SSL ve bootstrap sertifikaları) erişmesine izin verilir. DevOps üyesi tarafından diğer kaynak gruplarında oluşturulan VM'ler, VM'de gizli IU'lar olsa bile bu sırlara erişemez.

Örneğimiz basit bir senaryoyu açıklar. Gerçek hayat senaryoları daha karmaşık olabilir. İhtiyaçlarınıza göre izinleri anahtar kasanıza ayarlayabilirsiniz. Güvenlik ekibinin, DevOps personeli tarafından uygulamalarında kullanılan anahtar ve gizli referansları (URI'ler ve parmak izleri) sağladığını varsaydık. Geliştiriciler ve operatörler herhangi bir veri düzlemi erişimi gerekmez. Anahtar kasanızı nasıl güvenebileceğimize odaklandık. [VM'lerinizi,](https://azure.microsoft.com/services/virtual-machines/security/) [depolama hesaplarınızı](../storage/blobs/security-recommendations.md)ve diğer Azure kaynaklarınızı güvenli hale aldığınızda da benzer bir önem verin.

> [!NOTE]
> Bu örnek, Key Vault erişiminin üretimde nasıl kilitlenir olduğunu gösterir. Geliştiricilerin kasalarını, VM'lerini ve uygulamayı geliştirdikleri depolama hesabını yönetmek için tam izinlere sahip kendi abonelik veya kaynak grubuna sahip olmaları gerekir.

[Key Vault güvenlik duvarlarını ve sanal ağları yapılandırarak](key-vault-network-security.md)anahtar kasanıza ek güvenli erişim ayarlamanızı öneririz.

## <a name="resources"></a>Kaynaklar

* [Azure AD RBAC](../role-based-access-control/role-assignments-portal.md)

* [RBAC: Yerleşik roller](../role-based-access-control/built-in-roles.md)

* [Kaynak Yöneticisi dağıtımını ve klasik dağıtımı anlama](../azure-resource-manager/management/deployment-models.md)

* [Azure PowerShell ile RBAC'ı yönetin](../role-based-access-control/role-assignments-powershell.md)

* [REST API ile RBAC'ı yönetin](../role-based-access-control/role-assignments-rest.md)

* [Microsoft Azure için RBAC](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Bu 2015 Microsoft Ignite konferans videosu, Azure'daki erişim yönetimi ve raporlama özelliklerini tartışıyor. Ayrıca, Azure AD'yi kullanarak Azure aboneliklerine erişimi güvence altına almak için en iyi uygulamaları da inceler.

* [OAuth 2.0 ve Azure AD'yi kullanarak web uygulamalarına erişimi yetkilendirme](../active-directory/develop/v2-oauth2-auth-code-flow.md)

* [Anahtar Vault Yönetimi REST API'leri](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    KEY Vault erişim ilkesini ayarlama dahil olmak üzere, anahtar kasanızı programlı olarak yönetmek için REST API'leri için başvuru.

* [Anahtar Vault REST API'leri](https://msdn.microsoft.com/library/azure/dn903609.aspx)

* [Anahtar erişim denetimi](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)

* [Parola erişim denetimi](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)

* PowerShell'i kullanarak Key Vault erişim ilkesini [ayarlayın](/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy) ve [kaldırın.](/powershell/module/az.keyvault/Remove-azKeyVaultAccessPolicy)

## <a name="next-steps"></a>Sonraki adımlar

Key [Vault güvenlik duvarlarını ve sanal ağları](key-vault-network-security.md)yapılandırın.

Bir yönetici için başlangıç eğitimi için Azure [Anahtar Kasası nedir?](key-vault-overview.md)

Key Vault için kullanım günlüğü hakkında daha fazla bilgi için [Azure Key Vault günlüğe kaydin.](key-vault-logging.md)

Azure Key Vault ile tuşları ve sırları kullanma hakkında daha fazla bilgi [için, anahtarlar ve sırlar hakkında](https://msdn.microsoft.com/library/azure/dn903623.aspx)bilgi edin.

Key Vault hakkında sorularınız varsa, [forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)ziyaret edin.
