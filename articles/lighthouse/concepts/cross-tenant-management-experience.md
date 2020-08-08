---
title: Kiracılar arası yönetim deneyimleri
description: Azure Temsilcili kaynak yönetimi, bir çapraz kiracı yönetim deneyimi sunar.
ms.date: 08/07/2020
ms.topic: conceptual
ms.openlocfilehash: 9ec3896b85f825b22dc9b57d4220e1cdcdf3e390
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88003619"
---
# <a name="cross-tenant-management-experiences"></a>Kiracılar arası yönetim deneyimleri

Bir hizmet sağlayıcısı olarak, [Azure Mathouse](../overview.md) ' ı kullanarak birden çok müşteriye ait kaynakları, [Azure Portal](https://portal.azure.com)kendi kiracınızın içinden yönetebilirsiniz. Yönetilen kiracılar arasında çok sayıda görev ve hizmet, [Azure tarafından atanan kaynak yönetimi](../concepts/azure-delegated-resource-management.md)kullanılarak gerçekleştirilebilir.

> [!NOTE]
> Azure Temsilcili kaynak yönetimi, platformlar arası yönetimi basitleştirmek için [birden çok Azure AD kiracısına sahip bir kuruluşta](enterprise.md) de kullanılabilir.

## <a name="understanding-customer-tenants"></a>Müşteri kiracılarını anlama

Azure Active Directory (Azure AD) kiracısı, kuruluşun bir gösterimidir. Bu, bir kuruluşun Azure, Microsoft 365 veya diğer hizmetlere kaydolarak Microsoft ile bir ilişki oluşturduklarında aldığı adanmış bir Azure AD örneğidir. Her Azure AD kiracısı farklı ve diğer Azure AD kiracılarından ayrıdır ve kendi kiracı KIMLIĞINE (bir GUID) sahiptir. Daha fazla bilgi için bkz. [Azure Active Directory nedir?](../../active-directory/fundamentals/active-directory-whatis.md)

Genellikle, bir müşterinin Azure kaynaklarını yönetmek için, hizmet sağlayıcılarının bu müşterinin kiracısıyla ilişkili bir hesabı kullanarak Azure portal oturum açması gerekir, bu da müşterinin kiracısında hizmet sağlayıcısı için Kullanıcı hesapları oluşturmak ve yönetmek için bir yönetici gerektirir.

Azure Use ile, ekleme işlemi, hizmet sağlayıcısının kiracısındaki kullanıcıları, müşterinin kiracısında Temsilcili abonelikler ve kaynak grupları üzerinde çalışabilecektir. Bu kullanıcılar daha sonra kendi kimlik bilgilerini kullanarak Azure portal oturum açabilirler. Azure portal içinde, erişimleri olan tüm müşterilere ait olan kaynakları yönetebilir. Bu işlem, Azure portal [müşteriler](../how-to/view-manage-customers.md) sayfasından veya Azure Portal ya da API 'ler aracılığıyla doğrudan söz konusu müşterinin aboneliği kapsamında çalışarak yapılabilir.

Azure Athouse, farklı kiracılarda farklı hesaplarda oturum açmak zorunda kalmadan birden fazla müşteriye ait kaynakların yönetilmesine daha fazla esneklik sağlar. Örneğin, bir hizmet sağlayıcısı farklı sorumluluklara ve erişim düzeylerine sahip iki müşteriye sahip olabilir. Yetkili kullanıcılar, Azure Athouse kullanılarak bu kaynaklara erişmek için hizmet sağlayıcının kiracısında oturum açabilir.

![Bir hizmet sağlayıcı kiracısıyla yönetilen müşteri kaynakları](../media/azure-delegated-resource-management-service-provider-tenant.jpg)

## <a name="apis-and-management-tool-support"></a>API 'Ler ve Yönetim Aracı desteği

Temsilcili kaynaklar üzerinde doğrudan portalda veya API 'Ler ile yönetim araçlarını kullanarak (Azure CLı ve Azure PowerShell) yönetim görevleri gerçekleştirebilirsiniz. Tüm mevcut API 'Ler, işlevsellik çapraz Kiracı Yönetimi için desteklendiği ve Kullanıcı uygun izinlere sahip olduğu sürece, temsilcili kaynaklarla çalışırken kullanılabilir.

[Get-AzSubscription cmdlet 'i](/powershell/module/Az.Accounts/Get-AzSubscription) , `HomeTenantId` `ManagedByTenantIds` her abonelik için ve özniteliklerini gösterir. Bu, döndürülen bir aboneliğin yönetilen bir müşteri kiracısına mi ait yoksa kendi yönettiğiniz kiracınıza mi ait olduğunu tanımlamanızı sağlar. Azure PowerShell

Benzer şekilde, [az Account List](/cli/azure/account?view=azure-cli-latest#az-account-list) gıbı Azure CLI komutları `homeTenantId` ve özniteliklerini gösterir `managedByTenants` .

> [!TIP]
> Azure CLı kullanırken bu değerleri görmüyorsanız, arkasından ' i çalıştırarak Önbelleğinizi temizlemeyi deneyin `az account clear` `az login --identity` .

Ayrıca, Azure Use görevlerini gerçekleştirmeye özgü API 'Ler sunuyoruz. Daha fazla bilgi için **başvuru** bölümüne bakın.

## <a name="enhanced-services-and-scenarios"></a>Geliştirilmiş hizmetler ve senaryolar

Birçok görev ve hizmet, yönetilen kiracılar genelinde Temsilcili kaynaklar üzerinde gerçekleştirilebilir. Bunlar, platformlar arası yönetimin özellikle etkili olabilecek önemli senaryolardan bazılarıdır.

[Azure yay](../../azure-arc/index.yml):

- Karma sunucuları ölçekte yönetme- [sunucular Için Azure yayı (Önizleme)](../../azure-arc/servers/overview.md):
  - Azure 'daki [Windows Server veya Linux makinelerini](../../azure-arc/servers/onboard-portal.md) , Azure 'daki abonelik ve/veya kaynak grupları için temsilci olarak bağlayın
  - Azure Ilkesi ve etiketleme gibi Azure yapılarını kullanarak bağlı makineleri yönetme
  - Müşterilerin karma ortamları arasında aynı ilke kümesinin uygulandığından emin olun
  - Müşterilerin karma ortamları arasında uyumluluğu izlemek için Azure Güvenlik Merkezi 'ni kullanma
- Karma Kubernetes kümelerini ölçekte yönetme- [Azure Arc etkin Kubernetes (Önizleme)](../../azure-arc/kubernetes/overview.md):
  - Azure 'da [bir Kubernetes kümesini Azure 'A bağlamak](../../azure-arc/kubernetes/connect-cluster.md) için Azure 'daki abonelik ve/veya kaynak gruplarına bağlama
  - Bağlı kümeler için [Gilar kullanma](../../azure-arc/kubernetes/use-gitops-connected-cluster.md)
  - Bağlı kümeler arasında ilkeleri zorunlu kıl

[Azure Otomasyonu](../../automation/index.yml):

- Atanan müşteri kaynaklarına erişmek ve bunlarla çalışmak için Otomasyon hesaplarını kullanma

[Azure Backup](../../backup/index.yml):

- Müşteri kiracılarında müşteri verilerini yedekleme ve geri yükleme
- Temsilci olan abonelikler için yedekleme öğelerinin (henüz yedekleme için yapılandırılmamış Azure kaynakları dahil) ve izleme bilgilerinin (işlerin ve uyarıların) işletimsel bilgilerini görüntülemeye yardımcı olması için [yedekleme Gezginini](../../backup/monitor-azure-backup-with-backup-explorer.md) kullanın. Yedekleme Gezgini Şu anda yalnızca Azure VM verileri için kullanılabilir.
- Geçmiş eğilimleri izlemek, yedekleme depolama tüketimini analiz etmek ve yedeklemeleri denetlemek ve geri yüklemek için, temsilcili abonelikler arasında [yedekleme raporları](../../backup/configure-reports.md) kullanın.

[Azure maliyet yönetimi + faturalandırma](../../cost-management-billing/index.yml):

- Yönetim kiracısından, CSP iş ortakları, Azure planı kapsamındaki müşteriler için vergi öncesi tüketim maliyetlerini (satın almalara dahil değil) görüntüleyebilir, yönetebilir ve analiz edebilir. Maliyet, perakende tariflerine ve iş ortağının müşterinin aboneliğine sahip olduğu Azure rol tabanlı erişim denetimi (Azure RBAC) erişimine göre yapılır.

[Azure Kubernetes hizmeti (AKS)](../../aks/index.yml):

- Barındırılan Kubernetes ortamlarını yönetme ve müşteri kiracılarında Kapsayıcılı uygulamaları dağıtma ve yönetme

[Azure izleyici](../../azure-monitor/index.yml):

- Tüm aboneliklerdeki uyarıları görüntüleyebilme olanağı sayesinde, temsilci atanmış abonelikler için uyarıları görüntüleme
- Temsilcili abonelikler için etkinlik günlüğü ayrıntılarını görüntüleme
- Log Analytics: birden çok Kiracıdaki uzak müşteri çalışma alanlarından verileri sorgulama
- Web kancaları aracılığıyla hizmet sağlayıcı kiracısındaki Azure Otomasyonu runbook 'ları veya Azure Işlevleri gibi Otomasyonu tetikleyen müşteri kiracılarında uyarı oluşturma
- SAP iş yükleri için, [Müşteri kiracılar genelinde toplu bir görünüm Ile SAP Çözümleri ölçümlerini izleyin](https://techcommunity.microsoft.com/t5/running-sap-applications-on-the/using-azure-lighthouse-and-azure-monitor-for-sap-solutions-to/ba-p/1537293)

[Azure ağı](../../networking/networking-overview.md):

- Müşteri kiracılarında [Azure sanal ağını](../../virtual-network/index.yml) ve sanal ağ arabirim kartlarını (vNIC) dağıtma ve yönetme
- Müşterilerin sanal ağ kaynaklarını korumak için [Azure Güvenlik duvarını](../../firewall/overview.md) dağıtma ve yapılandırma
- Müşteriler için [Azure sanal WAN](../../virtual-wan/virtual-wan-about.md), [ExpressRoute](../../expressroute/expressroute-introduction.md)ve [VPN ağ geçitleri](../../vpn-gateway/vpn-gateway-about-vpngateways.md) gibi bağlantı hizmetlerini yönetme
- Azure [Athouse kullanarak Azure ağ msp programı](../../networking/networking-partners-msp.md) için önemli senaryoları destekleme


[Azure ilkesi](../../governance/policy/index.yml):

- Uyumluluk anlık görüntüleri, atanan abonelikler içindeki atanmış ilkelerin ayrıntılarını gösterir
- Temsilci atanmış bir abonelik içinde ilke tanımları oluşturma ve düzenleme
- Atanan abonelik içinde müşteri tanımlı ilke tanımları atama
- Müşteriler, yazdığı ilkelerin yanı sıra hizmet sağlayıcı tarafından yazılan ilkeleri görür
- [DeployIfNotExists 'i düzeltebilir veya müşteri kiracısında atamaları değiştirebilirler](../how-to/deploy-policy-remediation.md)

[Azure Kaynak Grafiği](../../governance/resource-graph/index.yml):

- Şimdi döndürülen sorgu sonuçlarında kiracı KIMLIĞINI içerir, bu da bir aboneliğin müşteri kiracısına veya hizmet sağlayıcı kiracısına ait olup olmadığını tanımlamanızı sağlar

[Azure Güvenlik Merkezi](../../security-center/index.yml):

- Çapraz kiracı görünürlüğü
  - Güvenlik ilkelerine uyumluluğu izleyin ve tüm kiracıların kaynakları genelinde güvenlik kapsamı sağlayın
  - Tek bir görünümdeki birden çok müşteri arasında sürekli mevzuat uyumluluk izlemesi
  - Güvenli puan hesaplaması ile eylem yapılabilir güvenlik önerilerini izleyin, önceliklendirin ve önceliklendirin
- Çapraz kiracı güvenlik sonrası yönetimi
  - Güvenlik ilkelerini yönetin
  - İşlem yapılabilir güvenlik önerileri ile uyumlu olmayan kaynaklar üzerinde işlem gerçekleştirin
  - Güvenlikle ilgili verileri toplama ve depolama
- Çapraz kiracı tehdit algılama ve koruma
  - Kiracıların kaynakları genelinde tehditleri Algıla
  - Tam zamanında (JıT) VM erişimi gibi gelişmiş tehdit koruması denetimleri uygulayın
  - Uyarlamalı ağ sağlamlaştırma ile ağ güvenlik grubu yapılandırmasını Harden artırma
  - Sunucuların yalnızca Uyarlamalı uygulama denetimleriyle birlikte olması gereken uygulamaları ve süreçler çalıştırdığından emin olun
  - Dosya bütünlüğü Izleme (FIM) ile önemli dosyalarda ve kayıt defteri girdilerindeki değişiklikleri izleme

[Azure Sentinel](../../sentinel/multiple-tenants-service-providers.md):

- [Müşteri Kiracılarında](../../sentinel/multiple-tenants-service-providers.md) Azure Sentinel kaynaklarını yönetme
- [Birden çok müşteri kiracısından saldırıları izleyin ve güvenlik uyarılarını görüntüleyin](https://techcommunity.microsoft.com/t5/azure-sentinel/using-azure-lighthouse-and-azure-sentinel-to-monitor-across/ba-p/1043899)
- Birden çok Sentinel çalışma alanı genelinde, müşteri kiracılarına yayılan [olayları görüntüleme](../../sentinel/multiple-workspace-view.md)

[Azure hizmet durumu](../../service-health/index.yml):

- Azure Kaynak Durumu ile müşteri kaynaklarının sistem durumunu izleme
- Müşterileriniz tarafından kullanılan Azure hizmetlerinin sistem durumunu izleme

[Azure Site Recovery](../../site-recovery/index.yml):

- Müşteri kiracılarında Azure sanal makineler için olağanüstü durum kurtarma seçeneklerini yönetme ( `RunAs` VM uzantılarını kopyalamak için hesapları kullanmayacağınızı unutmayın)

[Azure sanal makineleri](../../virtual-machines/index.yml):

- Müşteri kiracılarında Azure VM 'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlamak için sanal makine uzantılarını kullanın
- Müşteri kiracılarında Azure VM 'lerinde sorun gidermek için önyükleme tanılamayı kullanma
- Müşteri kiracılarında seri konsol ile VM 'Lere erişme
- Gizli dizileri, [ilke aracılığıyla yönetilen kimlik](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/create-keyvault-secret)kullanarak disk şifrelemesi için parolalar, gizlilikler veya şifreleme anahtarları için Azure Key Vault ile tümleştirin ve parolaların müşteri kiracılarında bir Key Vault depolanmasını sağlar
- Müşteri kiracılarındaki VM 'Lerde uzaktan oturum açma için Azure Active Directory kullanmayacağınızı unutmayın

Destek istekleri:

- Temsilci seçme kaynakları için Azure portal [ **Yardım + Destek** 'Ten destek istekleri açın](../../azure-portal/supportability/how-to-create-azure-support-request.md#getting-started) (temsilci seçilen kapsam için kullanılabilen destek planını seçme)

## <a name="current-limitations"></a>Geçerli sınırlamalar
Tüm senaryolarla, lütfen aşağıdaki geçerli sınırlamalara dikkat edin:

- Azure Resource Manager tarafından işlenen istekler, Azure tarafından atanan kaynak yönetimi kullanılarak gerçekleştirilebilir. Bu isteklerin işlem URI 'Leri ile başlar `https://management.azure.com` . Ancak, kaynak türünün bir örneği tarafından işlenen istekler (Key Vault gizli dizi erişimi veya depolama veri erişimi), Azure tarafından atanan kaynak yönetimi ile desteklenmez. Bu isteklerin işlem URI 'Leri genellikle örneğiniz için benzersiz olan bir adresle başlar, örneğin `https://myaccount.blob.core.windows.net` veya `https://mykeyvault.vault.azure.net/` . İkincisi ayrıca yönetim işlemleri yerine genellikle veri operasyonlardır.
- Rol atamalarının rol tabanlı erişim denetimi (RBAC) [yerleşik rollerini](../../role-based-access-control/built-in-roles.md)kullanması gerekir. Tüm yerleşik roller Şu anda, sahip veya izin içeren yerleşik roller hariç Azure tarafından yetkilendirilen kaynak yönetimi ile desteklenmektedir [`DataActions`](../../role-based-access-control/role-definitions.md#dataactions) . Kullanıcı erişimi yönetici rolü yalnızca [yönetilen kimliklere rol atama](../how-to/deploy-policy-remediation.md#create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant)konusunda sınırlı kullanım için desteklenir.  Özel roller ve [Klasik abonelik yöneticisi rolleri](../../role-based-access-control/classic-administrators.md) desteklenmez.
- Azure Databricks kullanan abonelikler ekleyebilirsiniz, ancak yönetme kiracısındaki kullanıcılar şu anda bir temsilci olan abonelikte Azure Databricks çalışma alanlarını başlatamaz.
- Kaynak kilitleri olan abonelikler ve kaynak grupları ekleyebilirsiniz, ancak bu kilitler yönetim kiracısındaki kullanıcılar tarafından gerçekleştirilen eylemlerin gerçekleştirilmesini engellemez. Azure tarafından yönetilen uygulamalar veya Azure şemaları (sistem tarafından atanan reddetme atamaları) tarafından oluşturulanlar gibi sistem tarafından yönetilen kaynakları koruyan [atamaları reddetme](../../role-based-access-control/deny-assignments.md) , yönetim kiracısındaki kullanıcıların bu kaynaklara göre davranmasını önler; Bununla birlikte, müşteri kiracısındaki kullanıcılar kendi reddetme atamalarını oluşturamaz (Kullanıcı tarafından atanan reddetme atamaları).

## <a name="next-steps"></a>Sonraki adımlar

- Müşterilerinizi [Azure Resource Manager şablonları kullanarak](../how-to/onboard-customer.md) veya [Azure Market 'e özel veya genel olarak yönetilen bir hizmet teklifi yayımlayarak](../how-to/publish-managed-services-offers.md), Azure tarafından atanan kaynak yönetimine ekleyin.
- Azure portal **müşterilerime** giderek [müşterileri görüntüleyin ve yönetin](../how-to/view-manage-customers.md) .
