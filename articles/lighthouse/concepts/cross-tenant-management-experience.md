---
title: Kiracılar arası yönetim deneyimleri
description: Azure temsilci kaynak yönetimi, kiracılar arası yönetim deneyimi sağlar.
ms.date: 03/12/2020
ms.topic: conceptual
ms.openlocfilehash: 0e55923e688d1062adc5838a88e8d3202864282a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218384"
---
# <a name="cross-tenant-management-experiences"></a>Kiracılar arası yönetim deneyimleri

Bir hizmet sağlayıcısı olarak, [Azure portalında](https://portal.azure.com)kendi kiracınızdan birden fazla müşteri için Azure kaynaklarını yönetmek için [Azure yetkin kaynak yönetimini](../concepts/azure-delegated-resource-management.md) kullanabilirsiniz. Görevlerin ve hizmetlerin çoğu, yönetilen kiracılar arasında devredilen Azure kaynaklarında gerçekleştirilebilir. Bu makalede, Azure temsilci kaynak yönetiminin etkili olabileceği gelişmiş senaryolardan bazıları açıklanmaktadır.

> [!NOTE]
> Azure temsilcikaynak yönetimi, kiracılar arası yönetimi basitleştirmek için [birden çok Azure AD kiracısına sahip bir kuruluşta](enterprise.md) da kullanılabilir.

## <a name="understanding-customer-tenants"></a>Müşteri kiracılarını anlama

Azure Etkin Dizin (Azure AD) kiracısı bir kuruluşun temsilidir. Azure, Microsoft 365 veya diğer hizmetlere kaydolarak Microsoft ile ilişki oluşturduğunda bir kuruluşun aldığı Azure AD özel bir örneğidir. Her Azure AD kiracısı diğer Azure AD kiracılarından farklıdır ve ayrıdır ve kendi kiracı kimliğine (GUID) sahiptir. Daha fazla bilgi için Azure [Etkin Dizin nedir?](../../active-directory/fundamentals/active-directory-whatis.md)

Genellikle, bir müşteri için Azure kaynaklarını yönetmek için, hizmet sağlayıcıların müşterinin kiracısıyla ilişkili bir hesabı kullanarak Azure portalında oturum açmaları gerekir ve müşterinin kiracısındaki bir yöneticinin kullanıcı hesapları oluşturması ve yönetmesi gerekir. servis sağlayıcı için.

Azure temsilcikaynak yönetimi yle, onboarding işlemi, hizmet sağlayıcısının kiracısında, müşterinin kiracısındaki aboneliklere, kaynak gruplarına ve kaynaklara erişebilecek ve yönetebilecek kullanıcıları belirtir. Bu kullanıcılar daha sonra kendi kimlik bilgilerini kullanarak Azure portalında oturum açabilir. Azure portalında, erişebildikleri tüm müşterilere ait kaynakları yönetebilirler. Bu işlem, Azure portalındaki [Müşterilerim](../how-to/view-manage-customers.md) sayfasını ziyaret ederek veya doğrudan azure portalında veya API'ler aracılığıyla müşterinin aboneliği bağlamında çalışarak yapılabilir.

Azure temsilci kaynak yönetimi, farklı kiracılardaki farklı hesaplarda oturum açmamak zorunda kalmadan birden çok müşterinin kaynaklarını yönetmesinde daha fazla esneklik sağlar. Örneğin, bir hizmet sağlayıcısının burada gösterildiği gibi farklı sorumlulukları ve erişim düzeyleri olan üç müşterisi olabilir:

![Servis sağlayıcının sorumluluklarını gösteren üç müşteri kiracısı](../media/azure-delegated-resource-management-customer-tenants.jpg)

Azure temsilci kaynak yönetimini kullanarak, yetkili kullanıcılar bu kaynaklara erişmek için hizmet sağlayıcısının kiracısında oturum açabilir:

![Tek bir hizmet sağlayıcı kiracı aracılığıyla yönetilen müşteri kaynakları](../media/azure-delegated-resource-management-service-provider-tenant.jpg)

## <a name="apis-and-management-tool-support"></a>API'ler ve yönetim aracı desteği

Yönetim görevlerini doğrudan portalda veya API'ler ve yönetim araçlarını (Azure CLI ve Azure PowerShell gibi) kullanarak devralan kaynaklarda gerçekleştirebilirsiniz. İşlevsellik kiracı lar arası yönetim için desteklendiğinde ve kullanıcı uygun izinlere sahip olduğu sürece, varolan tüm API'ler temsilci kaynaklarıyla çalışırken kullanılabilir.

Azure PowerShell [Get-AzSubscription cmdlet,](https://docs.microsoft.com/powershell/module/Az.Accounts/Get-AzSubscription?view=azps-3.5.0) iade edilen aboneliğin servis sağlayıcınız kiracınıza mı yoksa yönetilen bir müşteri kiracısına mı ait olduğunu belirlemenize olanak tanıyan her abonelik için **kiracı kimliğini** gösterir.

Benzer şekilde, [az hesap listesi](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-list) gibi Azure CLI komutları **homeTenantId** ve **managedByTenants** özniteliklerini gösterir.

> [!TIP]
> Azure CLI kullanırken bu değerleri görmüyorsanız, önbelleğinizi `az account clear` çalıştırarak `az login --identity`temizlemeyi deneyin.

Ayrıca, Azure temsilci kaynak yönetimi görevlerini gerçekleştirmeye özel API'ler de salıyoruz. Daha fazla bilgi için **Başvuru** bölümüne bakın.

## <a name="enhanced-services-and-scenarios"></a>Gelişmiş hizmetler ve senaryolar

Görevlerin ve hizmetlerin çoğu yönetilen kiracılar arasında devredilen kaynaklarda gerçekleştirilebilir. Aşağıda, kiracılar arası yönetimin etkili olabileceği bazı önemli senaryolar verilmiştir.

[Sunucular için Azure Arc (önizleme)](../../azure-arc/servers/overview.md):

- [Windows Server veya Linux makinelerini Azure dışındaki](../../azure-arc/servers/quickstart-onboard-portal.md) temsilcilere ve/veya Azure'daki kaynak gruplarına bağlayın
- Azure İlkesi ve etiketleme gibi Azure yapılarını kullanarak bağlı makineleri yönetme

[Azure Otomasyonu](../../automation/index.yml):

- Temsilci mizaninmüşteri kaynaklarına erişmek ve onlarla çalışmak için otomasyon hesaplarını kullanma

[Azure Yedekleme](../../backup/index.yml):

- Müşteri kiracılarında müşteri verilerini yedekleme ve geri yükleme
- Yedek öğelerin (henüz yedekleme için yapılandırılmamış Azure kaynakları dahil) operasyonel bilgilerini görüntülemeye yardımcı olmak ve devralınan abonelikler için bilgileri (iş ve uyarılar) izlemeye yardımcı olmak için [Yedekleme Gezgini'ni](../../backup/monitor-azure-backup-with-backup-explorer.md) kullanın. Yedekleme Gezgini şu anda yalnızca Azure VM verileri için kullanılabilir.
- Geçmiş eğilimleri izlemek, yedekleme depolama tüketimini analiz etmek ve yedeklemeleri denetlemek ve geri yüklemeleri denetlemek için devredilen abonelikler arasında [Yedekleme Raporları'nı](../../backup/configure-reports.md) kullanın.

[Azure Kubernetes Servisi (AKS)](../../aks/index.yml):

- Barındırılan Kubernetes ortamlarını yönetin ve konteyner uygulamaları müşteri kiracıları içinde dağıtma ve yönetme

[Azure Monitör:](../../azure-monitor/index.yml)

- Tüm aboneliklerde uyarıları görüntüleme özelliğiyle, devralınan abonelikler için uyarıları görüntüleme
- Devredilen abonelikler için etkinlik günlüğü ayrıntılarını görüntüleme
- Günlük analitiği: Birden çok kiracıda uzak müşteri çalışma alanlarından gelen verileri sorgula
- Webhooks aracılığıyla hizmet sağlayıcı kiracısında Azure Otomasyon runbook'ları veya Azure Fonksiyonları gibi otomasyonu tetikleyen müşteri kiracılarında uyarılar oluşturun

[Azure Politikası](../../governance/policy/index.yml):

- Uyumluluk anlık görüntüleri, devredilen abonelikler içinde atanan ilkelerin ayrıntılarını gösterir
- Devredilen abonelik tespisi tanımlarını oluşturma ve yeniden oluşturma
- Devredilen abonelik içinde müşteri tanımlı ilke tanımları atama
- Müşteriler, kendi kendilerine yazdığı ilkelerle birlikte servis sağlayıcı tarafından yazılmış ilkeleri de görür
- Dağıtımı düzeltebilir veya [müşteri kiracıiçindeki atamaları değiştirebilir veya atamaları değiştirebilir](../how-to/deploy-policy-remediation.md)

[Azure Kaynak Grafiği](../../governance/resource-graph/index.yml):

- Artık iade edilen sorgu sonuçlarında kiracı kimliğini içerir ve bir aboneliğin müşteri kiracısına veya hizmet sağlayıcısı kiracısına ait olup olmadığını belirlemenize olanak sağlar

[Azure Güvenlik Merkezi](../../security-center/index.yml):

- Kiracı lar arası görünürlük
  - Güvenlik ilkelerine uygunluğu izleyin ve tüm kiracıların kaynaklarında güvenlik kapsamı sağlayın
  - Tek bir görünümde birden fazla müşteri arasında sürekli mevzuata uygunluk izleme
  - Güvenli puan hesaplaması ile işlem yapılabilir güvenlik önerilerini izleme, önceliklendirme ve önceliklendirme
- Kiracılar arası güvenlik duruş yönetimi
  - Güvenlik ilkelerini yönetme
  - Eyleme geçirilebilir güvenlik önerilerine uymayan kaynaklar hakkında işlem gerçekleştirin
  - Güvenlikle ilgili verileri toplama ve depolama
- Kiracı lar arası tehdit algılama ve koruma
  - Kiracıların kaynakları yla ilgili tehditleri algılama
  - Tam zamanında (JIT) VM erişimi gibi gelişmiş tehdit koruma denetimleri uygulayın
  - Adaptive Network Hardening ile ağ güvenlik grubu yapılandırması sertleştirme
  - Sunucuların yalnızca uyarlamalı uygulama denetimleri ile olması gereken uygulamaları ve işlemleri çalıştırdığından emin olun
  - Dosya Bütünlüğü İzleme (FIM) ile önemli dosyalardaki ve kayıt defteri girişlerine yapılan değişiklikleri izleme

[Azure Sentinel](../../sentinel/multiple-tenants-service-providers.md):

- Azure Sentinel kaynaklarını [müşteri kiracılarında](../../sentinel/multiple-tenants-service-providers.md) yönetme
- [Saldırıları izleme ve birden çok müşteri kiracısı arasında güvenlik uyarılarını görüntüleme](https://techcommunity.microsoft.com/t5/azure-sentinel/using-azure-lighthouse-and-azure-sentinel-to-monitor-across/ba-p/1043899)

[Azure Hizmet Durumu](../../service-health/index.yml):

- Azure Kaynak Durumu ile müşteri kaynaklarının durumunu izleme
- Müşterileriniz tarafından kullanılan Azure hizmetlerinin sistem durumunu izleme

[Azure Site Kurtarma](../../site-recovery/index.yml):

- Müşteri kiracılarında Azure sanal makineleri için olağanüstü durum kurtarma seçeneklerini yönetme (VM uzantılarını kopyalamak için RunAs hesaplarını kullanamayacağınızı unutmayın)

[Azure Sanal Makineler](../../virtual-machines/index.yml):

- Müşteri kiracılarında Azure Sanal M'lerde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlamak için sanal makine uzantılarını kullanın
- Azure VM'leri müşteri kiracılarında sorun gidermek için önyükleme tanılama yöntemini kullanma
- Müşteri kiracılarında seri konsollu VM'lere erişin
- Bir VM'ye uzaktan giriş için Azure Active Directory'yi kullanamayacağınızı ve disk şifreleme için parolalar, sırlar veya şifreleme anahtarları için Bir Anahtar Kasası ile Bir VM'yi entegre edemezsiniz unutmayın

[Azure Sanal Ağ](../../virtual-network/index.yml):

- Müşteri kiracıları içinde sanal ağları ve sanal ağ arabirim kartlarını (vNIC'ler) dağıtma ve yönetme

Destek istekleri:

- Azure portalındaki Yardım + **destek** bıçağından temsilci kaynakları için destek isteklerini açın (temsilci kapsamı için kullanılabilen destek planını seçme)

## <a name="current-limitations"></a>Geçerli sınırlamalar
Tüm senaryolarda, lütfen aşağıdaki geçerli sınırlamalara dikkat edin:

- Azure Kaynak Yöneticisi tarafından işlenen istekler Azure temsilci kaynak yönetimi kullanılarak gerçekleştirilebilir. Bu istekler için iSTEM'ler işlemi `https://management.azure.com`. Ancak, kaynak türü (KeyVault sırları erişimi veya depolama veri erişimi gibi) bir örnek tarafından işlenen istekleri Azure yetkili kaynak yönetimi ile desteklenmez. Bu istekler için iSTEM'ler genellikle örneğinize özgü bir `https://myaccount.blob.core.windows.net` adresle `https://mykeyvault.vault.azure.net/`başlar( örneğin). İkincisi de genellikle veri işlemleri yerine yönetim işlemleri vardır. 
- Rol atamaları, rol tabanlı erişim denetimi (RBAC) [yerleşik rolleri](../../role-based-access-control/built-in-roles.md)kullanmalıdır. Tüm yerleşik roller şu anda Sahibi veya [DataActions](../../role-based-access-control/role-definitions.md#dataactions) izni olan yerleşik roller dışında Azure temsilci kaynak yönetimiyle desteklenir. Kullanıcı Erişim Yöneticisi rolü yalnızca [yönetilen kimliklere rol atamada](../how-to/deploy-policy-remediation.md#create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant)sınırlı kullanım için desteklenir.  Özel roller ve [klasik abonelik yöneticisi rolleri](../../role-based-access-control/classic-administrators.md) desteklenmez.
- Siz Azure Databricks kullanan abonelikleri kullanabilmenize izin veremeseniz de, yönetici kiracıdaki kullanıcılar şu anda devralınan bir abonelikte Azure Databricks çalışma alanlarını başlatamaz.
- Kaynak kilitleri olan Azure temsilcili kaynak yönetimi için abonelikleri ve kaynak gruplarını yerleşik olarak kullanabilmenize rağmen, bu kilitler eylemleri yönetici kiracıdaki kullanıcılar tarafından gerçekleştirilmesini engellemez. Azure tarafından yönetilen uygulamalar veya Azure Planları (sistem tarafından atanan reddet atamaları) gibi sistem tarafından yönetilen kaynakları koruyan [atamaları reddetme,](../../role-based-access-control/deny-assignments.md) yönetici kiracıdaki kullanıcıların bu kaynaklar üzerinde hareket etmesini engeller; ancak, şu anda müşteri kiracıkullanıcılar kendi reddi atamaları (kullanıcı tarafından atanan reddet atamaları) oluşturamaz.

## <a name="next-steps"></a>Sonraki adımlar

- Müşterilerinizi [Azure Kaynak Yöneticisi şablonlarını kullanarak](../how-to/onboard-customer.md) veya Azure [Marketi'nde özel veya genel olarak yönetilen bir hizmet teklifi yayınlayarak](../how-to/publish-managed-services-offers.md)Azure'a devredilen kaynak yönetimine dahil edin.
- Azure portalındaki **müşterilerime** giderek [müşterileri görüntüleyin ve yönetin.](../how-to/view-manage-customers.md)
