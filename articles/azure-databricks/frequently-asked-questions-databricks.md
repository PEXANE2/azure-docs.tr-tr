---
title: 'Azure Databricks: Sık sorulan sorular ve yardım'
description: Azure Veri Tuğlaları hakkındaki sık sorulan soruların ve sorun giderme bilgilerinin yanıtlarını alın.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/25/2018
ms.openlocfilehash: 8d7aab43641c6c594ff60368ccb3810e0c060dd7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671571"
---
# <a name="frequently-asked-questions-about-azure-databricks"></a>Azure Databricks hakkında sık sorulan sorular

Bu makalede, Azure Veri Tuğlaları ile ilgili olabilecek en sık sorular listelenebilmiştir. Ayrıca, Databricks kullanırken bazı sık karşılaşılan sorunları listeler. Daha fazla bilgi için Azure [Databricks nedir'e](what-is-azure-databricks.md)bakın. 

## <a name="can-i-use-azure-key-vault-to-store-keyssecrets-to-be-used-in-azure-databricks"></a>Azure Veri Tuğlaları'nda kullanılacak anahtarları/sırları depolamak için Azure Key Vault'u kullanabilir miyim?
Evet. Azure Veri Tuğlaları ile kullanmak üzere anahtarları/sırları depolamak için Azure Key Vault'u kullanabilirsiniz. Daha fazla bilgi için Azure [Anahtar Kasası destekli kapsamlara](/azure/databricks/security/secrets/secret-scopes)bakın.


## <a name="can-i-use-azure-virtual-networks-with-databricks"></a>Veri tuğlalı Azure Sanal Ağları kullanabilir miyim?
Evet. Azure Veri Tuğlaları ile bir Azure Sanal Ağı (VNET) kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure Sanal Ağınızda Azure Veri Tuğlaları Dağıtma.](/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

## <a name="how-do-i-access-azure-data-lake-storage-from-a-notebook"></a>Azure Veri Gölü Depolamasına not defterinden nasıl erişebilirim? 

Şu adımları uygulayın:
1. Azure Etkin Dizini'nde (Azure AD), bir hizmet ilkesi ni sağatın ve anahtarını kaydedin.
1. Veri Gölü Depolama'daki servis ilkesine gerekli izinleri atayın.
1. Veri Gölü Depolama'daki bir dosyaya erişmek için Not Defteri'ndeki hizmet temel kimlik bilgilerini kullanın.

Daha fazla bilgi için bkz. Azure [Veri Alanı Depolamayı Azure Veri Tuğlalarıyla Kullan.](/azure/databricks/data/data-sources/azure/azure-datalake)

## <a name="fix-common-problems"></a>Sık karşılaşılan sorunları giderme

Databricks ile karşılaşabileceğiniz birkaç sorun aşağıda verebilirsiniz.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Sorun: Bu abonelik 'Microsoft.Databricks' ad alanını kullanmak için kayıtlı değil

#### <a name="error-message"></a>Hata iletisi

"Bu abonelik ad alanını 'Microsoft.Databricks' kullanmak için kayıtlı değildir. https://aka.ms/rps-not-found Abonelikleri nasıl kaydedin. (Kod: Eksik Abonelik Kaydı)"

#### <a name="solution"></a>Çözüm

1. [Azure portalına](https://portal.azure.com)gidin.
1. **Abonelikleri,** kullandığınız aboneliği ve ardından **Kaynak sağlayıcılarını**seçin. 
1. Kaynak sağlayıcılar listesinde, **Microsoft.Databricks'e**karşı **Register'ı**seçin. Kaynak sağlayıcısını kaydetmek için abonelikte katkıda bulunan veya sahip rolünün olması gerekir.


### <a name="issue-your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Sorun: Hesabınızda {e-posta} Azure portalındaki Databricks çalışma alanı kaynağında sahip veya katılımcı rolü yok

#### <a name="error-message"></a>Hata iletisi

"Hesabınız {e-posta} Azure portalındaki Databricks çalışma alanı kaynağında Sahip veya Katılımcı rolüne sahip değildir. Kiracıda konuk kullanıcıysanız, bu hata da oluşabilir. Yöneticinizden size erişim izni vermesini veya sizi doğrudan Databricks çalışma alanına kullanıcı olarak eklemesini isteyin." 

#### <a name="solution"></a>Çözüm

Bu soruna birkaç çözüm şunlardır:

* Kiracıyı başlatmanız için, konuk kullanıcı olarak değil, kiracının normal kullanıcısı olarak oturum açmış olmalısınız. Databricks çalışma alanı kaynağında da katkıda bulunan bir rol olmalısınız. Azure portalındaki Databricks çalışma alanınızdaki **Access denetimi (IAM)** sekmesinden kullanıcıerişimi verebilirsiniz.

* E-posta etki alanı adınız Azure AD'deki birden çok dizinlere atanmışsa, bu hata da oluşabilir. Bu sorunu çözmek için, Databricks çalışma alanınız ile aboneliği içeren dizinde yeni bir kullanıcı oluşturun.

    a. Azure portalında Azure AD'ye gidin. Seç **Kullanıcılar ve Gruplar** > **Kullanıcı ekleyin.**

    b. E-posta yerine `@<tenant_name>.onmicrosoft.com` e-posta içeren bir kullanıcı ekleyin. `@<your_domain>` Bu seçeneği Azure portalında Azure AD altında **Özel Etki Alanları'nda**bulabilirsiniz.
    
    c. Bu yeni kullanıcıya Databricks çalışma alanı kaynağında **Katılımcı** rolünü verin.
    
    d. Yeni kullanıcıyla Azure portalında oturum açın ve Databricks çalışma alanını bulun.
    
    e. Bu kullanıcı olarak Databricks çalışma alanını başlatın.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Sorun: Hesabınız {e-posta} Databricks'te kayıtlı değil 

#### <a name="solution"></a>Çözüm

Çalışma alanını oluşturmadıysanız ve kullanıcı olarak eklendiyseniz, çalışma alanını oluşturan kişiyle iletişim kurun. Azure Databricks Yönetici Konsolu'nu kullanarak bu kişinin sizi eklemesini sorun. Yönergeler için [bkz.](/azure/databricks/administration-guide/users-groups/users) Çalışma alanını oluşturduysanız ve yine de bu hatayı aldıysanız, Azure portalından **Yeniden Başlangıç Çalışması Alanını** seçmeyi deneyin.

### <a name="issue-cloud-provider-launch-failure-while-setting-up-the-cluster-publicipcountlimitreached"></a>Sorun: Kümeyi kurarken bulut sağlayıcısı başlatma hatası (PublicIPCountLimitReached)

#### <a name="error-message"></a>Hata iletisi

"Bulut Sağlayıcısı Başlatma Hatası: Kümeyi kurarken bir bulut sağlayıcısı hatasıyla karşılaşıldı. Daha fazla bilgi için Databricks kılavuzuna bakın. Azure hata kodu: PublicIPCountLimitReached. Azure hata iletisi: Bu bölgede bu abonelik için 10'dan fazla genel IP adresi oluşturulamaz."

#### <a name="background"></a>Arka plan

Databricks kümeleri düğüm başına bir genel IP adresi kullanır (sürücü düğümü dahil). Azure abonelikleri, bölge başına [genel IP adresi sınırlarına](/azure/azure-resource-manager/management/azure-subscription-service-limits#publicip-address) sahiptir. Bu nedenle, küme oluşturma ve ölçeklendirme işlemleri, o bölgedeki aboneye ayrılan ortak IP adreslerinin sayısının sınırı aşması durumunda başarısız olabilir. Bu sınır, özel kullanıcı tanımlı VM'ler gibi Veri tuğlası olmayan kullanımiçin ayrılan genel IP adreslerini de içerir.

Genel olarak, kümeler yalnızca etkinken genel IP adreslerini tüketir. Ancak, `PublicIPCountLimitReached` diğer kümeler sonlandırıldıktan sonra bile hatalar kısa bir süre için oluşmaya devam edebilir. Bunun nedeni, bir küme sonlandırıldığında Databricks'in Azure kaynaklarını geçici olarak önbelleğe aldığıdır. Birçok yaygın senaryoda küme başlatma ve otomatik küçültme gecikmesüresini önemli ölçüde azalttığından, kaynak önbelleğe alma tasarım gereğidir.

#### <a name="solution"></a>Çözüm

Aboneliğiniz belirli bir bölge için genel IP adresi sınırına zaten ulaşmışsa, aşağıdakilerden birini veya diğerini yapmalısınız.

- Farklı bir Databricks çalışma alanında yeni kümeler oluşturun. Diğer çalışma alanı, aboneliğinizin genel IP adresi sınırına ulaşmadığınız bir bölgede bulunmalıdır.
- [Genel IP adresi sınırınızı artırma isteği.](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) **Sorun Türü**olarak **Kota** seçin ve **Ağ:** **Kota Türü**olarak ARM . **Ayrıntılar'da,** Genel IP Adresi kota sı kontenjanı artışı isteyin. Örneğin, sınırınız şu anda 60 ise ve 100 düğümlü bir küme oluşturmak istiyorsanız, 160'a bir sınır artışı isteyin.

### <a name="issue-a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster-missingsubscriptionregistration"></a>Sorun: Küme (MissingSubscriptionRegistration) kurulurken ikinci bir tür bulut sağlayıcısı başlatma hatası

#### <a name="error-message"></a>Hata iletisi

"Bulut Sağlayıcısı Başlatma Hatası: Kümeyi kurarken bir bulut sağlayıcısı hatasıyla karşılaşıldı. Daha fazla bilgi için Databricks kılavuzuna bakın.
Azure hata kodu: Eksik AbonelikKaydı Azure hata iletisi: Abonelik ad alanı 'Microsoft.Compute' kullanmak için kayıtlı değildir. https://aka.ms/rps-not-found Abonelikleri nasıl kaydedin."

#### <a name="solution"></a>Çözüm

1. [Azure portalına](https://portal.azure.com)gidin.
1. **Abonelikleri,** kullandığınız aboneliği ve ardından **Kaynak sağlayıcılarını**seçin. 
1. Kaynak sağlayıcılar listesinde, **Microsoft.Compute'a**karşı **Kaydol'u**seçin. Kaynak sağlayıcısını kaydetmek için abonelikte katkıda bulunan veya sahip rolünün olması gerekir.

Daha ayrıntılı yönergeler için [Kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md)bkz.

### <a name="issue-azure-databricks-needs-permissions-to-access-resources-in-your-organization-that-only-an-admin-can-grant"></a>Sorun: Azure Databricks'in kuruluşunuzdaki kaynaklara erişmek için yalnızca bir yöneticinin verebileceği izinlere ihtiyacı vardır.

#### <a name="background"></a>Arka plan

Azure Databricks, Azure Active Directory ile tümleşiktir. Azure AD'deki kullanıcıları belirterek izinleri Azure Databricks içinde (örneğin, not defterlerinde veya kümelerde) ayarlayabilirsiniz. Azure Veri Tuğlaları'nın Azure REKLAMınızdaki kullanıcıların adlarını listeleyebilmeleri için, bu bilgilerin okunması ve verilmesine izin verilmesi gerekir. Onay zaten kullanılamıyorsa, hatayı görürsünüz.

#### <a name="solution"></a>Çözüm

Azure portalında global yönetici olarak oturum açın. Azure Etkin Dizini **için, Kullanıcı Ayarları** sekmesine gidin ve **Kullanıcıların kendi adlarına şirket verilerine erişen uygulamalara onay veremediğinden** emin **olun, evet**olarak ayarlanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç: Azure Databricks ile başlayın](quickstart-create-databricks-workspace-portal.md)
- [Azure Databricks nedir?](what-is-azure-databricks.md)
