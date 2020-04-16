---
title: Azure Anahtar Kasası güvenliği | Microsoft Dokümanlar
description: Azure Key Vault, anahtarlar ve sırlar için erişim izinlerini yönetin. Key Vault için kimlik doğrulama ve yetkilendirme modelini ve anahtar kasanızın nasıl güvenli hale alınabildiğini kapsar.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: mbaldwin
Customer intent: As a key vault administrator, I want to learn the options available to secure my vaults
ms.openlocfilehash: dbe13b2d1291f212af7da9d1176bc3d90997978b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428972"
---
# <a name="azure-key-vault-security"></a>Azure Anahtar Kasası güvenliği

Azure Key Vault'u kullanmak için şifreleme anahtarlarını ve sertifikalar, bağlantı dizeleri ve parolalar gibi sırları bulutta korumanız gerekir. Hassas ve iş açısından kritik verileri depoladığınıziçin, kasalarınızın ve bunların depolanan verilerinin güvenliğini en üst düzeye çıkarmak için adımlar atmanız gerekir. Bu makale, Azure Anahtar Kasası güvenliğinizi tasarlarken göz önünde bulundurmanız gereken bazı kavramları kapsayacaktır.

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

Bir Azure aboneliğinde önemli bir kasa oluşturduğunuzda, bu kasa otomatik olarak aboneliğin Azure AD kiracısıyla ilişkilendirlenir. Bir kasadan içeriği yönetmeye veya almaya çalışan herkesin Azure AD tarafından kimlik doğrulaması gerekir.

- Kimlik doğrulaması, arayanın kimliğini belirler.
- Yetkilendirme, arayanın hangi işlemleri gerçekleştirebileceğini belirler. Key Vault'ta [Yetkilendirme, Role tabanlı erişim denetimi](../../role-based-access-control/overview.md) (RBAC) ve Azure Key Vault erişim ilkelerinin bir birleşimini kullanır.

### <a name="access-model-overview"></a>Erişim modeline genel bakış

Kasalara erişim iki arayüz veya düzlem üzerinden gerçekleşir. Bu uçaklar yönetim düzlemi ve veri düzlemidir.

- *Yönetim düzlemi* Key Vault'un kendisini yönettiğiniz yerdir ve kasaları oluşturmak ve silmek için kullanılan arayüzdür. Ayrıca anahtar kasa özelliklerini okuyabilir ve erişim ilkelerini yönetebilirsiniz.
- *Veri düzlemi,* anahtar kasasında depolanan verilerle çalışmanızı sağlar. Anahtarları, sırları ve sertifikaları ekleyebilir, silebilir ve değiştirebilirsiniz.

Her iki düzlemde de anahtar kasasına erişmek için tüm arayanların (kullanıcılar veya uygulamalar) kimlik doğrulaması ve yetkilendirilmesi gerekir. Her iki düzlem de kimlik doğrulaması için Azure Active Directory (Azure AD) kullanır. Yetkilendirme için yönetim düzlemi rol tabanlı erişim denetimi (RBAC) ve veri düzlemi bir Key Vault erişim ilkesi kullanır.

Her iki düzlemde kimlik doğrulama için tek bir mekanizma modelinin birkaç faydası vardır:

- Kuruluşlar, kuruluşlarındaki tüm önemli kasalara erişimi merkezi olarak denetleyebilir.
- Bir kullanıcı ayrılırsa, kuruluştaki tüm önemli kasalara anında erişim kaybeder.
- Kuruluşlar, azure AD'deki seçenekleri kullanarak kimlik doğrulamayı özelleştirebilir, örneğin ek güvenlik için çok faktörlü kimlik doğrulamayı etkinleştirebilirsiniz

### <a name="managing-administrative-access-to-key-vault"></a>Key Vault'a yönetim erişiminin yönetilmesi

Kaynak grubunda önemli bir kasa oluşturduğunuzda, Azure AD'yi kullanarak erişimi yönetirsiniz. Kullanıcılara veya gruplara bir kaynak grubundaki anahtar kasaları yönetme olanağı verirsiniz. Uygun RBAC rollerini atayarak belirli bir kapsam düzeyinde erişim izni verebilirsiniz. Anahtar kasalarını yönetmek için bir kullanıcıya erişim izni `key vault Contributor` vermek için, kullanıcıya belirli bir kapsamda önceden tanımlanmış bir rol atarsınız. Aşağıdaki kapsam düzeyleri bir RBAC rolüne atanabilir:

- **Abonelik**: Abonelik düzeyinde atanan bir RBAC rolü, bu abonelikteki tüm kaynak grupları ve kaynaklar için geçerlidir.
- **Kaynak grubu**: Kaynak grubu düzeyinde atanan bir RBAC rolü, bu kaynak grubundaki tüm kaynaklar için geçerlidir.
- **Belirli kaynak**: Belirli bir kaynak için atanan bir RBAC rolü bu kaynağa uygulanır. Bu durumda, kaynak belirli bir anahtar kasasıdır.

Önceden tanımlanmış birkaç rol vardır. Önceden tanımlanmış bir rol gereksinimlerinize uymuyorsa, kendi rolünüzü tanımlayabilirsiniz. Daha fazla bilgi için Bkz. [RBAC: Yerleşik roller.](../../role-based-access-control/built-in-roles.md)

> [!IMPORTANT]
> Bir kullanıcının `Contributor` önemli bir kasa yönetim düzlemi için izinleri varsa, anahtar kasa erişim ilkesi ayarlayarak kullanıcı veri düzlemine erişim izni verebilir. Anahtar kasalarınıza kimin `Contributor` rol erişimi olduğunu sıkı bir şekilde kontrol edmelisiniz. Anahtar kasalarınıza, anahtarlarınıza, sırlarınıza ve sertifikalarınıza yalnızca yetkili kişilerin erişebilmesini ve yönetebilmesini sağlayın.

<a id="data-plane-access-control"></a>
### <a name="controlling-access-to-key-vault-data"></a>Key Vault verilerine erişimi denetleme

Key Vault erişim ilkeleri, anahtarlara, sırlara veya sertifikaya ayrı ayrı izin verir. Kullanıcıya yalnızca anahtarlara erişim izni verebilirsiniz, sırlara değil. Anahtarlar, sırlar ve sertifikalar için erişim izinleri kasa düzeyinde yönetilir.

> [!IMPORTANT]
> Key Vault erişim ilkeleri, belirli bir anahtar, gizli veya sertifika gibi parçalı, nesne düzeyinde izinleri desteklemez. Bir kullanıcıya anahtar oluşturma ve silme izni verildiğinde, bu işlemleri bu anahtar kasasındaki tüm anahtarlarda gerçekleştirebilir.

Önemli bir kasa için erişim ilkeleri ayarlamak için [Azure portalını,](https://portal.azure.com/) [Azure CLI'yi, Azure](/cli/azure/install-azure-cli?view=azure-cli-latest) [PowerShell'i](/powershell/azureps-cmdlets-docs)veya [Key Vault Management REST API'lerini](/rest/api/keyvault/)kullanın.

Azure Key Vault için [sanal ağ hizmeti bitiş noktalarını](overview-vnet-service-endpoints.md)kullanarak veri düzlemi erişimini kısıtlayabilirsiniz). Ek bir güvenlik katmanı için [güvenlik duvarlarını ve sanal ağ kurallarını](network-security.md) yapılandırabilirsiniz.

## <a name="network-access"></a>Ağ erişimi

Hangi IP adreslerinin bunlara erişimi olduğunu belirterek kasalarınızın maruz kalmasını azaltabilirsiniz. Azure Key Vault'un sanal ağ hizmeti bitiş noktaları, belirli bir sanal ağa erişimi kısıtlamanızı sağlar. Uç noktalar ayrıca IPv4 (internet protokolü sürüm 4) adres aralıkları listesine erişimi kısıtlamanızı sağlar. Bu kaynakların dışından anahtar kasanıza bağlanan herhangi bir kullanıcının erişimi engellenir.

Güvenlik duvarı kuralları yürürlüğe girdiğinde, kullanıcılar Yalnızca istekleri izin verilen sanal ağlardan veya IPv4 adres aralıklarından geldiğinde Key Vault'tan gelen verileri okuyabilir. Bu, Azure portalından Key Vault'a erişmek için de geçerlidir. Kullanıcılar Azure portalından önemli bir kasaya göz atabilseler de, istemci makineleri izin verilenler listesinde değilse anahtarları, sırları veya sertifikaları listelemeyebilir. Bu, diğer Azure hizmetleri tarafından Key Vault Picker'ı da etkiler. Güvenlik duvarı kuralları istemci makinelerini engelliyorsa, kullanıcılar anahtar kasalarının listesini görebilir, ancak liste anahtarlarını göremeyebilir.

Azure Key Vault ağ adresi hakkında daha fazla bilgi [için Azure Key Vault için Sanal ağ hizmeti bitiş noktalarını](overview-vnet-service-endpoints.md)gözden geçirin)

## <a name="monitoring"></a>İzleme

Key Vault günlüğü kasanızda gerçekleştirilen etkinlikler hakkında bilgi kaydeder. Anahtar Vault günlükleri:

- Başarısız istekler de dahil olmak üzere tüm kimlik doğrulaması REST API istekleri
  - Anahtar kasasının kendisi üzerinde operasyonlar. Bu işlemler, oluşturma, silme, erişim ilkeleri ayarlama ve etiketler gibi önemli kasa özniteliklerini güncelleştirmeyi içerir.
  - Anahtar kasasında ki anahtarlar ve sırlar üzerinde operasyonlar:
    - Bu anahtarları veya sırları oluşturma, değiştirme veya silme.
    - Anahtarları imzalama, doğrulama, şifreleme, şifre çözme, paketleme ve açma, sır alma ve anahtarları ve sırları (ve sürümlerini) listeleme.
- Bir 401 yanıtına neden olan kimliği doğrulanmamış istekler. Örnekler, taşıyıcı belirteci olmayan, hatalı biçimlendirilmiş veya süresi dolmuş veya geçersiz bir belirteci olan isteklerdir.

Günlük bilgilerine anahtar kasa işleminden sonra 10 dakika içinde ulaşılabilir. Günlüklerinizi depolama hesabınızda yönetmek size kalmış.

- Günlüklerinize erişebilecek kişileri kısıtlayarak güvenliklerini sağlamak için standart Azure erişim denetimi yöntemlerini kullanın.
- Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.

Depolama hesaplarını güvenli bir şekilde yönetme önerisi için [Azure Depolama güvenlik kılavuzunu](../../storage/blobs/security-recommendations.md) inceleyin

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Anahtar Kasası için sanal ağ hizmeti bitiş noktaları](overview-vnet-service-endpoints.md))
- [RBAC: Yerleşik roller](../../role-based-access-control/built-in-roles.md)
- [Azure Key Vault için sanal ağ hizmeti bitiş noktaları](overview-vnet-service-endpoints.md))
