---
title: Çalışma alanları ve kiracılar arasında Azure Sentinel 'i genişletme | Microsoft Docs
description: MSSP hizmet sağlayıcıları için Azure Sentinel 'e birden çok kiracı ile çalışma.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/11/2020
ms.author: yelevin
ms.openlocfilehash: d76f8e2d750b8ab2d82e9424f929d8b8353ac25a
ms.sourcegitcommit: 1383842d1ea4044e1e90bd3ca8a7dc9f1b439a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84816444"
---
# <a name="extend-azure-sentinel-across-workspaces-and-tenants"></a>Azure Sentinel’i çalışma alanlarına ve kiracılara genişletme

## <a name="the-need-to-use-multiple-azure-sentinel-workspaces"></a>Birden çok Azure Sentinel çalışma alanı kullanma gereksinimi

Azure Sentinel, Log Analytics çalışma alanının üzerine kurulmuştur. Azure Sentinel 'i hazırlama bölümündeki ilk adımın, bu amaçla kullanmak istediğiniz Log Analytics çalışma alanını seçtiğine dikkat edin.

Tek bir çalışma alanı kullanırken Azure Sentinel deneyiminin tüm avantajına ulaşabilirsiniz. Bu nedenle, birden çok çalışma alanına sahip olmanızı gerektirebilecek bazı durumlar da vardır. Aşağıdaki tabloda bu durumların bazıları listelenmektedir ve mümkün olduğunda, gereksinimin tek bir çalışma alanıyla nasıl karşılanıp karşılanmadığı gösterilmektedir:

| Gereksinim | Description | Çalışma alanı sayısını azaltmanın yolları |
|-------------|-------------|--------------------------------|
| Sogemenlik ve mevzuat uyumluluğu | Çalışma alanı belirli bir bölgeye bağlıdır. Yasal gereksinimleri karşılamak için verilerin farklı [Azure coğrafi](https://azure.microsoft.com/global-infrastructure/geographies/) bölgelerde tutulması gerekiyorsa, bu, ayrı çalışma alanlarına bölünmelidir. |  |
| Veri sahipliği | Veri sahiplerinin sınırları (örneğin, yan kuruluşlar veya bağlı şirketler), ayrı çalışma alanları kullanılarak daha iyi bir şekilde ayarlanır. |  |
| Birden çok Azure kiracısından | Azure Sentinel, Microsoft ve Azure SaaS kaynaklarından yalnızca kendi Azure Active Directory (Azure AD) kiracı sınırının içindeki veri toplamayı destekler. Bu nedenle, her Azure AD kiracısı ayrı bir çalışma alanı gerektirir. |  |
| Ayrıntılı veri erişim denetimi | Kuruluşun, Azure Sentinel tarafından toplanan bazı verilere erişmesi için kuruluş içinde veya dışında farklı gruplara izin vermeniz gerekebilir. Örneğin:<br><ul><li>Kaynak sahiplerinin, kaynaklarıyla ilgili verilere erişimi</li><li>Bölgesel veya yan kuruluşların, kuruluşun bölümleriyle ilgili verilere erişimi</li></ul> | [Kaynak RBAC](https://techcommunity.microsoft.com/t5/azure-sentinel/controlling-access-to-azure-sentinel-data-resource-rbac/ba-p/1301463) veya [tablo düzeyi RBAC](https://techcommunity.microsoft.com/t5/azure-sentinel/table-level-rbac-in-azure-sentinel/ba-p/965043) kullanma |
| Ayrıntılı saklama ayarları | Tarihsel olarak, farklı veri türleri için farklı saklama dönemleri belirlemenin tek yolu birden çok çalışma alanı vardı. Bu, daha fazla durumda, tablo düzeyinde bekletme ayarlarının tanıtımı sayesinde artık gerekli değildir. | [Tablo düzeyinde bekletme ayarlarını](https://techcommunity.microsoft.com/t5/azure-sentinel/new-per-data-type-retention-is-now-available-for-azure-sentinel/ba-p/917316) kullan veya [veri silmeyi](../azure-monitor/platform/personal-data-mgmt.md#how-to-export-and-delete-private-data) otomatik hale getir |
| Bölünmüş faturalandırma | Çalışma alanlarını ayrı aboneliklere yerleştirerek farklı taraflara faturalandırılabilirler. | Kullanım raporlama ve çapraz doldurma |
| Eski mimari | Birden çok çalışma alanının kullanımı, daha önce doğru olmayan önemli sınırlamalara veya en iyi uygulamalara geçen bir geçmiş tasarımdan ayrılabilir. Ayrıca, Azure Sentinel 'e daha iyi uyum sağlamak için değiştirilebilen rastgele bir tasarım seçeneği de olabilir.<br><br>Örnekler arasında şunlar yer almaktadır:<br><ul><li>Azure Güvenlik Merkezi 'Ni dağıttığınızda, abonelik başına varsayılan çalışma alanını kullanma</li><li>Ayrıntılı erişim denetimi veya bekletme ayarları için gerekenler, görece yeni olan çözümler</li></ul> | Çalışma alanlarını yeniden mimari |

### <a name="managed-security-service-provider-mssp"></a>Yönetilen güvenlik hizmeti sağlayıcısı (MSSP)

Birden çok çalışma alanı için bir MSSP Azure Sentinel hizmeti olan belirli bir kullanım durumu. Bu durumda, Yukarıdaki gereksinimlerin hepsi uygulanmadığından, kiracılar genelinde birden çok çalışma alanı, en iyi yöntem olarak da geçerlidir. MSSP, kiracı genelinde Azure Sentinel çalışma alanı yeteneklerini genişletmek için [Azure](../lighthouse/overview.md) Use 'ı kullanabilir.

## <a name="azure-sentinel-multiple-workspace-architecture"></a>Azure Sentinel birden çok çalışma alanı mimarisi

Yukarıdaki gereksinimler tarafından belirtildiği gibi, birden çok Azure Sentinel çalışma alanı (potansiyel olarak Azure Active Directory (Azure AD) kiracılarının tek bir SOC tarafından merkezi olarak izlenmesi ve yönetilmesi gerektiği durumlar vardır.

- Bir MSSP Azure Sentinel hizmeti.

- Birden çok yan kuruluşları sunan genel bir SOC, her birinin kendi yerel SOC 'i vardır.

- Bir kuruluş içindeki birden çok Azure AD kiracısından oluşan bir SOC.

Azure Sentinel, bu gereksinimi karşılamak için, aşağıdaki diyagramda gösterildiği gibi, SOC tarafından kapsanan her şey için tek bir cam bölmesi sağlayan, merkezi izleme, yapılandırma ve yönetimi etkinleştiren çoklu çalışma alanı yetenekleri sunar.

:::image type="content" source="media/extend-sentinel-across-workspaces-tenants/cross-workspace-architecture.png" alt-text="Çapraz çalışma alanı mimarisi":::

Bu model, tüm verilerin tek bir çalışma alanına kopyalandığı tam merkezi bir model üzerinde önemli avantajlar sunar:

- Küresel ve yerel SOCs 'ye veya MSSP 'nin müşterilerine esnek rol ataması.

- Veri sahipliği, veri gizliliği ve mevzuat uyumluluğuyla ilgili daha az zorluk.

- En az ağ gecikmesi ve ücretleri.

- Yeni yan kuruluşları veya müşterileri kolay bir şekilde ekleme ve çıkarma.

Aşağıdaki bölümlerde, bu modelin nasıl çalışalınacağını ve özellikle nasıl yapılacağını açıklayacağız:

- Birden çok çalışma alanını, büyük olasılıkla kiracılar genelinde, tek bir cam bölmesi ile SOC ile birlikte izleyin.

- Çoklu çalışma alanlarını, büyük olasılıkla kiracılar genelinde, Otomasyonu kullanarak merkezi olarak yapılandırın ve yönetin.

## <a name="cross-workspace-monitoring"></a>Çapraz çalışma alanı izleme

### <a name="manage-incidents-on-multiple-workspaces"></a>Birden çok çalışma alanı üzerinde olayları yönetme

Azure Sentinel, birden fazla çalışma alanında merkezi olay izlemeyi ve yönetimini kolaylaştırmaya yönelik [birden çok çalışma alanı olay görünümünü](./multiple-workspace-view.md) destekler. Merkezi olay görünümü olayları doğrudan yönetmenizi veya kaynak ayrıntılarına, kaynak çalışma alanının bağlamında saydam bir şekilde gitmeyi sağlar.

### <a name="cross-workspace-querying"></a>Çapraz çalışma alanı sorgulama

Azure Sentinel, tek bir [sorgudaki birden fazla çalışma alanını](../azure-monitor/log-query/cross-workspace-query.md)sorgulamayı destekler, böylece tek bir sorgudaki birden çok çalışma alanındaki verileri arayabilir ve ilişkilendirmenize olanak tanır. 

- Farklı bir çalışma alanındaki bir tabloya başvurmak için [Workspace () ifadesini](../azure-monitor/log-query/workspace-expression.md) kullanın. 
- Birden çok çalışma alanındaki tablolar arasında bir sorgu uygulamak için, Workspace () ifadesinin yanı sıra [UNION işlecini](https://docs.microsoft.com/azure/data-explorer/kusto/query/unionoperator?pivots=azuremonitor) kullanın.

Çalışma alanları arası sorguları basitleştirmek için, kaydedilmiş [işlevleri](../azure-monitor/log-query/functions.md) kullanabilirsiniz. Örneğin, bir çalışma alanına yönelik bir başvuru uzunsa, ifadeyi `workspace("customer-A's-hard-to-remember-workspace-name").SecurityEvent` adlı bir işlev olarak kaydetmek isteyebilirsiniz `SecurityEventCustomerA` . Daha sonra sorguları farklı yazabilirsiniz `SecurityEventCustomerA | where ...` .

Bir işlev, yaygın olarak kullanılan bir UNION 'yi de basitleştirir. Örneğin, aşağıdaki ifadeyi adlı bir işlev olarak kaydedebilirsiniz `unionSecurityEvent` :

`union workspace(“hard-to-remember-workspace-name-1”).SecurityEvent, workspace(“hard-to-remember-workspace-name-2”).SecurityEvent`

Sonra, ile başlayarak her iki çalışma alanı arasında bir sorgu yazabilirsiniz `unionSecurityEvent | where ...` .

> [!NOTE] 
> Aynı sorgudaki birden çok çalışma alanını sorgulamak performansı etkileyebilir ve bu nedenle yalnızca mantık bu işlevi gerektirdiğinde önerilir.

### <a name="using-cross-workspace-workbooks"></a>Çalışma alanları arası çalışma kitaplarını kullanma

[Çalışma kitapları](./overview.md#workbooks) , Azure Sentinel 'e panolar ve uygulamalar sağlar. Birden çok çalışma alanıyla çalışırken, çalışma alanları arasında izleme ve eylemler sağlar.

Çalışma kitapları, her biri farklı düzeylerde Son Kullanıcı uzmanlığına sahip olan üç yöntemden birine yönelik çoklu çalışma alanları sorguları sağlayabilir:

| Yöntem  | Description | Ne zaman kullanmalıyım? |
|---------|-------------|--------------------|
| Çapraz çalışma alanı sorguları yazma | Çalışma kitabı Oluşturucu, çalışma kitabında çapraz çalışma alanı sorguları (yukarıda açıklanmıştır) yazabilir. | Bu seçenek, çalışma kitabı oluşturucularının kullanıcıyı çalışma alanı yapısından tamamen kalmalarını sağlar. |
| Çalışma kitabına çalışma alanı seçici ekleme | Çalışma kitabı Oluşturucusu, [burada](https://techcommunity.microsoft.com/t5/azure-sentinel/making-your-azure-sentinel-workbooks-multi-tenant-or-multi/ba-p/1402357)açıklandığı gibi çalışma kitabının bir parçası olarak çalışma alanı seçiciyi uygulayabilir. | Bu seçenek, kullanıcıya, kullanımı kolay bir açılan kutu aracılığıyla çalışma kitabı tarafından gösterilen çalışma alanları üzerinde denetim sağlar. |
| Çalışma kitabını etkileşimli olarak düzenleme | Var olan bir çalışma kitabını değiştiren gelişmiş bir Kullanıcı, içindeki sorguları düzenleyerek, düzenleyicideki çalışma alanı seçicisini kullanarak hedef çalışma alanlarını seçebilirsiniz. | Bu seçenek, bir Power User 'ın, mevcut çalışma kitaplarını birden çok çalışma alanıyla çalışacak şekilde kolayca değiştirmesini sağlar. |
|

## <a name="cross-workspace-management-using-automation"></a>Otomasyon kullanarak çapraz çalışma alanı yönetimi

Birden çok Azure Sentinel çalışma alanını yapılandırmak ve yönetmek için Azure Sentinel yönetim API 'sinin kullanımını otomatikleştirmeniz gerekir. Uyarı kuralları, arama sorguları, çalışma kitapları ve PlayBook 'lar dahil olmak üzere Azure Sentinel kaynaklarının dağıtımını otomatikleştirme hakkında daha fazla bilgi için bkz. [Azure Sentinel: API 'ler, tümleştirme ve yönetim Otomasyonu](https://techcommunity.microsoft.com/t5/azure-sentinel/extending-azure-sentinel-apis-integration-and-management/ba-p/1116885).

Ayrıca bkz. Azure [Sentinel 'ı kod olarak dağıtma ve yönetme](https://techcommunity.microsoft.com/t5/azure-sentinel/deploying-and-managing-azure-sentinel-as-code/ba-p/1131928) [ve Azure](https://techcommunity.microsoft.com/t5/azure-sentinel/combining-azure-lighthouse-with-sentinel-s-devops-capabilities/ba-p/1210966) ve özel bir GitHub deposundan kaynak dağıtma ve yapılandırma için birleştirilmiş, topluluk tarafından katkıda bulunulan bir yöntem. 


## <a name="whats-not-supported-across-workspaces"></a>Çalışma alanları arasında ne desteklenmez?

Çalışma alanları arasında aşağıdaki özellikler desteklenmez:

- Zamanlanmış bir uyarı kuralı, bir çalışma alanı sorgusu kullanılarak çalışma alanları arasında çalıştırılamaz.

- Arama sorguları, çalışma alanları arası sorguları desteklemez.

## <a name="managing-workspaces-across-tenants-using-azure-lighthouse"></a>Azure açık Thouse kullanarak kiracılar genelinde çalışma alanlarını yönetme

Yukarıda belirtildiği gibi birçok senaryoda, farklı Azure Sentinel çalışma alanları farklı Azure AD kiracılarında bulunabilir. [Azure](../lighthouse/overview.md) açık çalışma alanları, kiracı sınırları genelinde tüm platformlar arası etkinlikleri genişletmek için, yönetim kiracınızdaki kullanıcıların tüm kiracılarda Azure Sentinel çalışma alanlarında çalışmasına olanak tanımak için kullanabilirsiniz. Azure Use [eklendi](../lighthouse/how-to/onboard-customer.md)ise, portalda farklı çalışma alanı seçicilerinde kullanılabilir olmasını sağlamak için, yönetmek istediğiniz çalışma alanlarını içeren tüm abonelikleri seçmek üzere Azure Portal [Dizin + abonelik seçicisini](./multiple-tenants-service-providers.md#how-to-access-azure-sentinel-from-other-tenants) kullanın.

Azure ışıklı kullanımı kullanılırken, her bir Azure Sentinel rolü için bir grup oluşturmanız ve her kiracıdan bu gruplara temsilci izinleri oluşturmanız önerilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel 'in yeteneklerini birden çok çalışma alanında ve kiracıların tamamında nasıl genişletileceğini öğrendiniz. Azure Sentinel 'in çapraz çalışma alanı mimarisini uygulamayla ilgili pratik yönergeler için aşağıdaki makalelere bakın:

- Azure Pthouse kullanarak Azure Sentinel 'de [birden çok kiracı ile nasıl çalışacağınızı](./multiple-tenants-service-providers.md) öğrenin.
- [Birden çok çalışma alanındaki olayları sorunsuz şekilde görüntülemeyi ve yönetmeyi](./multiple-workspace-view.md) öğrenin.