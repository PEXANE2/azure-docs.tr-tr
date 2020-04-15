---
title: Azure Kaynak Grafiği'ne Genel Bakış
description: Azure Kaynak Grafiği hizmetinin, abonelikler ve kiracılar arasında kaynakların ölçek olarak karmaşık şekilde sorgulanmasına nasıl olanak sağladığını anlayın.
ms.date: 03/02/2020
ms.topic: overview
ms.openlocfilehash: f5c091f60faedb76e3ca6cd68505c06f51be21b6
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81381527"
---
# <a name="what-is-azure-resource-graph"></a>Azure Kaynak Grafı nedir?

Azure Kaynak Grafiği, ortamınızı etkin bir şekilde yönetebilmeniz için belirli bir abonelik kümesinde ölçekte sorgu yapma olanağıyla verimli ve performanslı kaynak araması sağlayarak Azure Kaynak Yönetimi'ni genişletmek üzere tasarlanan bir hizmettir. Bu sorgular aşağıdaki özellikleri sağlar:

- Karmaşık filtreleme, gruplandırma ve kaynak özelliklerine göre sıralama ile kaynakları sorgulama özelliği.
- Yönetişim gereksinimlerine dayalı kaynakları yinelemeli olarak keşfetme becerisi.
- Uygulanan ilkenin geniş bir bulut ortamındaki etkisini değerlendirme özelliği.
- Kaynak [özelliklerinde yapılan değişiklikleri ayrıntılandırma](./how-to/get-resource-changes.md) yeteneği (önizleme).

Bu belgede her özelliği ayrıntılı olarak inceleyeceksiniz.

> [!NOTE]
> Azure Kaynak Grafiği, Azure portalının arama çubuğuna, yeni gözatma 'Tüm kaynaklar' deneyimine ve Azure İlkesi'nin [geçmiş](../policy/how-to/determine-non-compliance.md#change-history-preview)
> _görsel diff'ine_güç verir. Müşterilerin büyük ölçekli ortamları yönetmesine yardımcı olmak için tasarlanmıştır.

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Kaynak Grafiği, Azure Resource Manager'ı nasıl tamamlar

Azure Kaynak Yöneticisi şu anda temel kaynak alanları üzerindeki sorguları özellikle kaynak adı, kimlik, tür, kaynak grubu, abonelik ve Konum olarak destekler. Kaynak Yöneticisi ayrıca, ayrıntılı özellikler için tek tek kaynak sağlayıcılarını tek seferde bir kaynak için arayarak olanaklar sağlar.

Azure Kaynak Grafiği ile, her kaynak sağlayıcısına tek tek çağrı yapmanıza gerek kalmadan, kaynak sağlayıcılarının geri döndürdüğü bu özelliklere erişebilirsiniz. Desteklenen kaynak türlerinin listesi için [tablo yu ve kaynak türü başvurularını](./reference/supported-tables-resources.md)gözden geçirin. Desteklenen kaynak türlerini görmenin alternatif bir yolu [Azure Kaynak Grafiği Explorer Schema tarayıcısıdır.](./first-query-portal.md#schema-browser)

Azure Kaynak Grafiği ile şunları yapabilirsiniz:

- Her kaynak sağlayıcısına ayrı ayrı arama yapmanıza gerek kalmadan kaynak sağlayıcıları tarafından döndürülen özelliklere erişin.
- Hangi özelliklerin ne zaman değiştiğini görmek için kaynağa yapılan değişiklik geçmişinin son 14 gününü görüntüleyin. (önizleme)

## <a name="how-resource-graph-is-kept-current"></a>Kaynak Grafiği nasıl geçerli tutulur?

Bir Azure kaynağı güncelleştirildiğinde, Kaynak Grafiği Kaynak Yöneticisi tarafından değişiklik hakkında bilgilendirilir.
Kaynak Grafiği daha sonra veritabanını güncelleştirir. Kaynak Grafiği de düzenli _tam tama_yapar. Bu tama, eksik bildirimler varsa veya kaynak Kaynak Yöneticisi dışında güncelleştirildiğinde Kaynak Grafiği verilerinin güncel olmasını sağlar.

> [!NOTE]
> Kaynak Grafiği `GET` özellikleri ve değerleri toplamak için her kaynak sağlayıcısının en son önizleme olmayan API'sini kullanır. Sonuç olarak, beklenen özellik kullanılamayabilir. Bazı durumlarda, kullanılan API sürümü sonuçlarda daha güncel veya yaygın olarak kullanılan özellikleri sağlamak için geçersiz kılınmıştır. Ortamınızdaki tam bir liste için her kaynak türü örneği [için API göster sürümüne](./samples/advanced.md#apiversion) bakın.

## <a name="the-query-language"></a>Sorgu dili

Azure Kaynak Grafiği'nin ne olduğunu daha iyi anladığınıza göre, sorguların nasıl oluşturulabildiğini öğrenelim.

Azure Kaynak Grafiği'nin sorgu dilinin Azure Veri Gezgini tarafından kullanılan [Kusto sorgu dilini](/azure/data-explorer/data-explorer-overview) temel alan bir dil olduğunu anlamak önemlidir.

Önce, Azure Kaynak Grafiği ile kullanılabilecek işlemler ve işlevler hakkında ayrıntılar için, bkz. [Kaynak Grafiği sorgu dili](./concepts/query-language.md).
Kaynakları göz atmak için, bkz, [kaynakları keşfedin](./concepts/explore-resources.md).

## <a name="permissions-in-azure-resource-graph"></a>Azure Kaynak Grafiği’nde izinler

Kaynak Grafı’nı kullanmak için, sorgulamak istediğiniz kaynaklara en az okuma erişimi olan [Rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) (RBAC) kapsamında uygun izinlere sahip olmanız gerekir. Sonuç döndürülmesi için Azure nesnesinde veya nesne grubunda en azından `read` iznine sahip olmanız gerekir.

> [!NOTE]
> Kaynak Grafiği, oturum açma sırasında bir anabilim tarafından kullanılabilen abonelikleri kullanır. Etkin bir oturum sırasında eklenen yeni bir aboneliğin kaynaklarını görmek için asıl yöneticinin bağlamı yenilemesi gerekir. Bu eylem oturum açarken ve geri dönerken otomatik olarak gerçekleşir.

Azure CLI ve Azure PowerShell, kullanıcının erişediği abonelikleri kullanır. REST API'yi doğrudan kullanırken, abonelik listesi kullanıcı tarafından sağlanır. Kullanıcı listedeki aboneliklerden herhangi biri için erişebiliyorsa, sorgu sonuçları kullanıcının erişediği abonelikler için döndürülür. Bu davranış, [Kaynak Gruplarını](/rest/api/resources/resourcegroups/list) \- ararken olduğu gibi aynıdır - Sonucun kısmi olabileceğine dair herhangi bir belirti olmadan erişebildiğiniz kaynak gruplarını listele.
Abonelik listesinde kullanıcının uygun haklara sahip olduğu abonelik yoksa, yanıt _403_ (Yasaktır).

## <a name="throttling"></a>Azaltma

Ücretsiz hizmet olarak, Kaynak Grafiği sorguları tüm müşteriler için en iyi deneyimi ve yanıt süresini sağlamak için daraltılır. Kuruluşunuz büyük ölçekli ve sık sık sorgular için Kaynak Grafiği API'sını kullanmak istiyorsa, [Kaynak Grafiği portalı sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/ResourceGraph)'Geri Bildirim' portalını kullanın.
Ekibin sizinle iletişim kurabilmesi için iş durumunuzu sağlayın ve 'Microsoft geri bildiriminiz hakkında size e-posta gönderebilirsiniz' onay kutusunu seçin.

Kaynak Grafiği sorguları kullanıcı düzeyinde daraltır. Hizmet yanıtı aşağıdaki HTTP üstbilgilerini içerir:

- `x-ms-user-quota-remaining`(int): Kullanıcı için kalan kaynak kotası. Bu değer, sorgu sayısıiçin eşlenir.
- `x-ms-user-quota-resets-after`(hh:mm:ss): Kullanıcının kota tüketimi sıfırlanana kadar geçen süre

Daha fazla bilgi [için, daraltılmış istekler için Yönerge'ye](./concepts/guidance-for-throttled-requests.md)bakın.

## <a name="running-your-first-query"></a>İlk sorgunuzu çalıştırma

Azure portalının bir parçası olan Azure Kaynak Grafiği Gezgini, Kaynak Grafiği sorgularının doğrudan Azure portalında çalıştırılmasını sağlar. Portal iş akışınıza gerçek zamanlı dinamik bilgiler sağlamak için sonuçları dinamik grafikler olarak sabitle. Daha fazla bilgi için Azure [Kaynak Grafiği Gezgini ile ilk sorguya](first-query-portal.md)bakın.

Kaynak Grafiği, .NET için Azure CLI, Azure PowerShell, Azure SDK ve daha fazlasını destekler. Sorgu her dil için aynı yapılandırılmıştır. Kaynak Grafiği'ni şu şekilde etkinleştirmeyi öğrenin:

- [Azure portalı ve Kaynak Grafiği Gezgini](first-query-portal.md) 
- [Azure CLI](first-query-azurecli.md#add-the-resource-graph-extension)
- [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalını](first-query-portal.md)kullanarak ilk sorgunuzu çalıştırın.
- [Azure CLI](first-query-azurecli.md)ile ilk sorgunuzu çalıştırın.
- Azure [PowerShell](first-query-powershell.md)ile ilk sorgunuzu çalıştırın.