---
title: Yönetilen HSM Azure Key Vault yönetilen HSM 'ye güvenli erişim
description: Azure RBAC ve yerel olarak yönetilen HSM RBAC kullanarak yönetilen HSM 'ye erişimi güvenli hale getirme hakkında bilgi edinin
services: key-vault
author: amitbapat
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: tutorial
ms.date: 09/15/2020
ms.author: ambapat
ms.openlocfilehash: 18ffa0f878effda8888200c13ab312851aaebdcd
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "91000920"
---
# <a name="secure-access-to-your-managed-hsms"></a>Yönetilen HSM 'lerinizde güvenli erişim

Azure Key Vault yönetilen HSM, şifreleme anahtarlarını koruyan bir bulut hizmetidir. Bu veriler hassas ve iş açısından kritik olduğundan, yalnızca yetkili uygulamalara ve kullanıcılara erişmesine izin vererek yönetilen HSM 'lerinizdeki erişimi güvenli hale getirmeniz gerekir. Bu makalede, yönetilen HSM erişim denetimi modeline bir genel bakış sunulmaktadır. Kimlik doğrulama ve yetkilendirmeyi açıklar ve yönetilen HSM 'lerinizde erişimin güvenliğini nasıl sağlayabileceğinizi açıklar.

Bu öğreticide, Azure RBAC ve yerel olarak yönetilen HSM RBAC kullanarak görev ve erişim denetimi ile ilgili nasıl ayrım yapılacağını gösteren basit bir örnek gösterilmektedir. Yönetilen HSM erişim denetimi modeli hakkında bilgi edinmek için bkz. [YÖNETILEN HSM erişim denetimi](access-control.md) .

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için aşağıdaki öğelere sahip olmanız gerekir:

* Bir Microsoft Azure aboneliği. Hesabınız yoksa, [ücretsiz deneme için kaydolabilirsiniz](https://azure.microsoft.com/pricing/free-trial).
* Azure CLı sürüm 2.12.0 veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).
* Aboneliğinizde yönetilen bir HSM. Bkz. [hızlı başlangıç: Azure CLI kullanarak](quick-create-cli.md) yönetilen bir HSM sağlama ve etkinleştirme.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

CLı kullanarak Azure 'da oturum açmak için şunu yazabilirsiniz:

```azurecli
az login
```

CLı aracılığıyla oturum açma seçenekleri hakkında daha fazla bilgi için bkz. [Azure CLI ile oturum açma](/cli/azure/authenticate-azure-cli?view=azure-cli-latest&preserve-view=true)

## <a name="example"></a>Örnek

Bu örnekte, oturum açma işlemleri için RSA 2.048 bit anahtarı kullanan bir uygulama geliştiriyoruz. Uygulamamız [yönetilen kimliğe](../../active-directory/managed-identities-azure-resources/overview.md)sahip bir Azure sanal MAKINESINDE (VM) çalışır. İmzalama için kullanılan RSA anahtarı, yönetilen HSM 'imizde depolanır.

Uygulamamızı yöneten, dağıtan ve denetlayan aşağıdaki rolleri belirledik:

- **Güvenlik ekibi**: CSO (Güvenlik Müdürü) veya benzer katkıda bulunanlar ofisindeki BT personeli. Güvenlik ekibi, anahtarların doğru safekinden ping işlemi yapmaktan sorumludur. Oturum açmak için RSA veya EC anahtarları, veri şifreleme için RSA veya AES anahtarları.
- **Geliştiriciler ve işleçler**: uygulamayı geliştiren ve Azure 'da dağıtan personel. Bu ekibin üyeleri güvenlik personelinin bir parçası değildir. Bunlar RSA anahtarları gibi hassas verilere erişemez. Yalnızca dağıttıkları uygulamanın bu hassas verilere erişimi olmalıdır.
- **Denetçiler**: Bu rol, geliştirme veya genel BT personelinin üyesi olmayan katkıda bulunanlar içindir. Güvenlik standartlarıyla uyumluluğu sağlamak için sertifikaların, anahtarların ve parolaların kullanımını ve bakımını gözden geçirir.

Uygulamamızın kapsamı dışında başka bir rol var: abonelik (veya kaynak grubu) Yöneticisi. Abonelik Yöneticisi güvenlik ekibi için ilk erişim izinlerini ayarlar. Uygulama için gerekli kaynaklara sahip bir kaynak grubunu kullanarak güvenlik ekibine erişim izni verir.

Rollerimiz için aşağıdaki işlemleri yetkilendirmemiz gerekir:

**Güvenlik ekibi**
- Yönetilen HSM 'yi oluşturun.
- Yönetilen HSM güvenlik etki alanını (olağanüstü durum kurtarma için) indirin
- Günlüğe kaydetmeyi açın.
- Anahtar oluşturma veya içeri aktarma
- Olağanüstü durum kurtarma için yönetilen HSM yedeklemeleri oluşturun.
- Belirli işlemler için kullanıcılara ve uygulamalara izin vermek üzere yönetilen HSM yerel RBAC 'yi ayarlayın.
- Anahtarları düzenli olarak alın.

**Geliştiriciler ve operatörler**
- Güvenlik ekibinin imza için kullanılan RSA anahtarı olan başvuru (anahtar URI) alın.
- Program aracılığıyla anahtara erişen uygulamayı geliştirin ve dağıtın.

**Denetçiler**
- Anahtarların güncel olduğundan emin olmak için anahtarların süre sonu tarihlerini gözden geçirin
- Anahtarlara yalnızca yetkili kullanıcılar/uygulamalar tarafından erişilebilmesini sağlamak için rol atamalarını izleyin
- Anahtarların doğru kullanımını, veri güvenliği standartları ile uyumlu olarak doğrulamak için yönetilen HSM günlüklerini gözden geçirin.

Aşağıdaki tabloda, yönetilen HSM 'ye erişmek üzere takımlar ve kaynaklar için rol atamaları özetlenmektedir.

| Rol | Yönetim düzlemi rolü | Veri düzlemi rolü |
| --- | --- | --- |
| Güvenlik ekibi | Yönetilen HSM Katılımcısı | Yönetilen HSM Yöneticisi |
| Geliştiriciler ve operatörler | Hiçbiri | Hiçbiri |
| Denetçiler | Hiçbiri | Yönetilen HSM şifre denetleyicisi |
| Uygulama tarafından kullanılan VM 'nin yönetilen tanımlaması| Hiçbiri | Yönetilen HSM şifre kullanıcısı |
| Uygulama tarafından kullanılan depolama hesabının yönetilen kimliği| Hiçbiri| Yönetilen HSM hizmeti şifrelemesi |

Üç takım rolünün, yönetilen HSM izinleriyle birlikte diğer kaynaklara erişmesi gerekir. VM 'Leri (veya Azure App Service Web Apps özelliğini) dağıtmak için, geliştiricilerin ve işleçlerin `Contributor` Bu kaynak türlerine erişmesi gerekir. Denetçilerin, yönetilen HSM günlüklerinin depolandığı depolama hesabına okuma erişimi olması gerekir.

Yönetim düzlemi rolleri (Azure RBAC) atamak için Azure portal veya Azure CLı veya Azure PowerShell gibi diğer yönetim arabirimlerinden birini kullanabilirsiniz. Yönetilen HSM veri düzlemi rolleri atamak için Azure CLı kullanmanız gerekir.

Bu bölümdeki Azure CLı kod parçacıkları aşağıdaki varsayımlar ile oluşturulmuştur:

- Azure Active Directory Yöneticisi, üç rolü temsil eden güvenlik grupları oluşturdu: contoso güvenlik ekibi, contoso uygulama DevOps ve contoso uygulama denetçileri. Yönetici kullanıcıları ilgili gruplarına ekledi.
- Tüm kaynaklar **Contosoapprg** kaynak grubunda bulunur.
- Yönetilen HSM günlükleri **contosologstorage** depolama hesabında depolanır.
- **Contosomhsm** Managed HSM ve **contosologstorage** Storage hesabı aynı Azure konumudur.

Abonelik Yöneticisi, `Managed HSM Contributor` rolü güvenlik ekibine atar. Bu rol, güvenlik ekibinin mevcut yönetilen HSM 'leri yönetmesine ve yenilerini oluşturmalarına olanak tanır. Yönetilen HSM 'ler varsa, bunları yönetebilmek için "yönetilen HSM Yöneticisi" rolü atanması gerekir.

```azurecli-interactive
# This role assignment allows Contoso Security Team to create new Managed HSMs
az role assignment create --assignee-object-id $(az ad group show -g 'Contoso Security Team' --query 'objectId' -o tsv) --assignee-principal-type Group --role "Managed HSM Contributor"

# This role assignment allows Contoso Security Team to become administrator of existing managed HSM
az keyvault role assignment create  --hsm-name ContosoMHSM --assignee $(az ad group show -g 'Contoso Security Team' --query 'objectId' -o tsv) --scope / --role "Managed HSM Administrator"
```

Güvenlik ekibi, günlüğü ayarlar ve Denetçilerin ve VM uygulamasına roller atar.

```azurecli-interactive
# Enable logging
hsmresource=$(az keyvault show --hsm-name ContosoMHSM --query id -o tsv)
storageresource=$(az storage account show --name contosologstorage --query id -o tsv)
az monitor diagnostic-settings create --name MHSM-Diagnostics --resource $hsmresource --logs    '[{"category": "AuditEvent","enabled": true}]' --storage-account $storageresource

# Assign the "Crypto Auditor" role to Contoso App Auditors group. It only allows them to read.
az keyvault role assignment create  --hsm-name ContosoMHSM --assignee $(az ad group show -g 'Contoso App Auditors' --query 'objectId' -o tsv) --scope / --role "Managed HSM Crypto Auditor"

# Grant the "Crypto User" role to the VM's managed identity. It allows to create and use keys. 
# However it cannot permanently delete (purge) keys
az keyvault role assignment create  --hsm-name ContosoMHSM --assignee $(az vm identity show --name "vmname" --resource-group "ContosoAppRG" --query objectId -o tsv) --scope / --role "Managed HSM Crypto Auditor"

# Assign "Managed HSM Crypto Service Encryption" role to the Storage account ID
storage_account_principal=$(az storage account show --id $storageresource --query identity.principalId -o tsv)
# (if no identity exists), then assign a new one
[ "$storage_account_principal" ] || storage_account_principal=$(az storage account update --assign-identity --id $storageresource)

az keyvault role assignment create --hsm-name ContosoMHSM --role "Managed HSM Crypto Service Encryption" --assignee $storage_account_principal
```

Bu öğretici yalnızca çoğu bölüm için erişim denetimiyle ilgili eylemleri gösterir. VM 'nize uygulama dağıtmaya ilişkin diğer eylemler ve işlemler, bir depolama hesabı için müşteri tarafından yönetilen anahtarla şifrelemeyi açmak üzere burada yönetilen HSM oluşturma, örneği erişim denetimi ve rol yönetimine odaklanmış tutmak için burada gösterilmez.

Örneğimizde basit bir senaryo açıklanmaktadır. Gerçek yaşam senaryoları daha karmaşık olabilir. Gereksinimlerinize göre anahtar kasanıza yönelik izinleri ayarlayabilirsiniz. Güvenlik ekibinin, uygulamalarında DevOps personeli tarafından kullanılan anahtar ve gizli başvuruları (URI 'Ler ve parmak izleri) sağladığını kabul ediyoruz. Geliştiricilere ve işleçlere herhangi bir veri düzlemi erişimi gerekmez. Anahtar kasanızın güvenliğini sağlama konusunda odaklandık. [VM 'lerinizi](https://azure.microsoft.com/services/virtual-machines/security/), [depolama hesaplarınızı](../../storage/blobs/security-recommendations.md)ve diğer Azure kaynaklarınızı güvenli hale getirmeye benzer bir göz kazandırın.

## <a name="resources"></a>Kaynaklar

- [Azure RBAC belgeleri](../../role-based-access-control/overview.md)
- [Azure RBAC: yerleşik roller](../../role-based-access-control/built-in-roles.md)
- [Azure RBAC 'yi Azure CLı ile yönetme](../../role-based-access-control/role-assignments-cli.md)

## <a name="next-steps"></a>Sonraki adımlar

Bir yöneticiye yönelik bir başlangıç öğreticisi için bkz. [YÖNETILEN HSM nedir?](overview.md).

Yönetilen HSM günlüğü için kullanım günlüğü hakkında daha fazla bilgi için bkz. [YÖNETILEN HSM günlüğü](logging.md).
