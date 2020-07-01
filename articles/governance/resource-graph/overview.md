---
title: Azure Kaynak Grafiği'ne Genel Bakış
description: Azure Kaynak Grafiği hizmeti 'nin, abonelikler ve kiracılar arasında ölçeklendirerek kaynakların karmaşık şekilde sorgulanmasını nasıl sağladığını anlayın.
ms.date: 06/29/2020
ms.topic: overview
ms.openlocfilehash: 055bb0be75de4d5f6a5a27c5fb3b218fda8777d7
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85565682"
---
# <a name="what-is-azure-resource-graph"></a>Azure Kaynak Grafı nedir?

Azure Kaynak Grafiği, ortamınızı etkili bir şekilde yönetebilmeniz için, belirli bir abonelik kümesi genelinde ölçeği sorgulama olanağı sunarak Azure Kaynak yönetimini uzatmak için tasarlanan bir Azure hizmetidir. Bu sorgular aşağıdaki özellikleri sağlar:

- Karmaşık filtreleme, gruplandırma ve kaynak özelliklerine göre sıralama ile kaynakları sorgulama özelliği.
- Kaynakları idare gereksinimlerine göre yineleyebilme özelliği.
- Uygulanan ilkenin geniş bir bulut ortamındaki etkisini değerlendirme özelliği.
- [Kaynak özelliklerinde yapılan değişiklikleri ayrıntılandırın](./how-to/get-resource-changes.md) (Önizleme) özelliği.

Bu belgede her özelliği ayrıntılı olarak inceleyeceksiniz.

> [!NOTE]
> Azure Kaynak Grafiği Azure Portal 'nin arama çubuğunu, yeni ' tüm kaynaklar ' deneyimini ve Azure ilkesinin [değişiklik geçmişi](../policy/how-to/determine-non-compliance.md#change-history) 
>  _görsel farkını_bir şekilde güçlendirir. Müşterilerin büyük ölçekli ortamları yönetmesine yardımcı olmak için tasarlanmıştır.

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Kaynak Grafiği, Azure Resource Manager'ı nasıl tamamlar

Azure Resource Manager şu anda temel kaynak alanları, özellikle kaynak adı, KIMLIK, tür, kaynak grubu, abonelik ve konum üzerinde sorguları desteklemektedir. Kaynak Yöneticisi ayrıca, tek seferde bir kaynağın ayrıntılı özelliklerine yönelik ayrı kaynak sağlayıcıları çağırma olanakları sağlar.

Azure Kaynak Grafiği ile, her kaynak sağlayıcısına tek tek çağrı yapmanıza gerek kalmadan, kaynak sağlayıcılarının geri döndürdüğü bu özelliklere erişebilirsiniz. Desteklenen kaynak türlerinin bir listesi için [tablo ve kaynak türü başvurusunu](./reference/supported-tables-resources.md)gözden geçirin. Desteklenen kaynak türlerini görmenin alternatif bir yolu, [Azure Kaynak Grafiği Gezgini şema tarayıcısı](./first-query-portal.md#schema-browser)aracılığıyla yapılır.

Azure Kaynak Grafiği ile şunları yapabilirsiniz:

- Kaynak sağlayıcıları tarafından döndürülen özelliklere, her bir kaynak sağlayıcısına tek tek çağrılar yapmaya gerek kalmadan erişin.
- Hangi özelliklerin değiştirildiğini ve ne zaman değiştiğini görmek için kaynakta yapılan değişiklik geçmişinin son 14 günü görüntüleyin. (önizleme)

## <a name="how-resource-graph-is-kept-current"></a>Kaynak grafiğinin nasıl güncel tutulduğu

Bir Azure kaynağı güncelleştirildiği zaman, kaynak grafiğine değişikliğin Kaynak Yöneticisi bildirilir.
Kaynak Grafiği daha sonra veritabanını güncelleştirir. Kaynak Grafiği de düzenli bir _tam tarama_yapar. Bu tarama, eksik bildirimler varsa veya bir kaynağın Kaynak Yöneticisi dışında güncelleştirildiği durumlarda kaynak Grafik verilerinin güncel olmasını sağlar.

> [!NOTE]
> Kaynak Grafiği `GET` , özellikleri ve değerleri toplamak için her kaynak sağlayıcının en son Önizleme olmayan API 'sine kullanır. Sonuç olarak, beklenen özellik kullanılamıyor olabilir. Bazı durumlarda kullanılan API sürümü, sonuçlarda daha güncel veya yaygın olarak kullanılan özellikler sağlamak için geçersiz kılındı. Ortamınızdaki tüm bir liste için [her kaynak türü örneği IÇIN API sürümünü göster](./samples/advanced.md#apiversion) ' i inceleyin.

## <a name="the-query-language"></a>Sorgu dili

Azure Kaynak Grafının ne olduğunu daha iyi kavradığınıza göre, sorgu oluşturma konusuna bakalım.

Azure Kaynak grafiğinin sorgu dilinin Azure Veri Gezgini tarafından kullanılan [kusto sorgu diline](/azure/data-explorer/data-explorer-overview) göre olduğunu anlamak önemlidir.

Önce, Azure Kaynak Grafiği ile kullanılabilecek işlemler ve işlevler hakkında ayrıntılar için, bkz. [Kaynak Grafiği sorgu dili](./concepts/query-language.md). Kaynakları göz atmak için, bkz, [kaynakları keşfedin](./concepts/explore-resources.md).

## <a name="permissions-in-azure-resource-graph"></a>Azure Kaynak Grafiği’nde izinler

Kaynak Grafı’nı kullanmak için, sorgulamak istediğiniz kaynaklara en az okuma erişimi olan [Rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) (RBAC) kapsamında uygun izinlere sahip olmanız gerekir. Sonuç döndürülmesi için Azure nesnesinde veya nesne grubunda en azından `read` iznine sahip olmanız gerekir.

> [!NOTE]
> Kaynak Grafiği, oturum açma sırasında bir sorumlu için kullanılabilen abonelikleri kullanır. Etkin bir oturum sırasında eklenen yeni aboneliğin kaynaklarını görmek için sorumlu bağlamı yenilemeniz gerekir. Bu eylem, oturumunuzu kapatıp yeniden oturum açarken otomatik olarak gerçekleşir.

Azure CLı ve Azure PowerShell kullanıcının erişimi olan abonelikleri kullanır. REST API doğrudan kullanırken, abonelik listesi Kullanıcı tarafından sağlanır. Kullanıcının listedeki aboneliklerden herhangi birine erişimi varsa, sorgu sonuçları kullanıcının erişimi olan abonelikler için döndürülür. Bu davranış, [kaynak grupları çağırma ile](/rest/api/resources/resourcegroups/list) aynıdır \- ; sonucun kısmi olabileceğini belirten herhangi bir gösterge olmadan, erişebileceğiniz kaynak gruplarını elde edersiniz. Abonelik listesinde kullanıcının uygun haklara sahip bir abonelik yoksa, yanıt _403_ (yasak) olur.

## <a name="throttling"></a>Azaltma

Ücretsiz bir hizmet olarak, kaynak grafiğine yapılan sorgular, tüm müşteriler için en iyi deneyimi ve yanıt süresini sağlamak üzere azaltılır. Kuruluşunuz büyük ölçekli ve sık kullanılan sorgular için kaynak Graph API kullanmak istiyorsa, [Kaynak Grafiği portalı sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/ResourceGraph)Portal ' geri bildirim ' kullanın.
Takımınızın sizinle iletişim kurabilmesi için iş büyük ve küçük ' Microsoft sizi geri bildiriminiz hakkında size e-posta ile gönderebilirsiniz ' onay kutusunu seçin.

Kaynak Grafiği, kullanıcı düzeyindeki sorguları kısıtlar. Hizmet yanıtı aşağıdaki HTTP üstbilgilerini içerir:

- `x-ms-user-quota-remaining`(int): Kullanıcı için kalan Kaynak kotası. Bu değer sorgu sayısı ile eşlenir.
- `x-ms-user-quota-resets-after`(SS: DD: SS): kullanıcının kota tüketimi sıfırlanana kadar geçen süre

Daha fazla bilgi için bkz. [Kısıtlanmış istekler Için rehberlik](./concepts/guidance-for-throttled-requests.md).

## <a name="running-your-first-query"></a>İlk sorgunuzu çalıştırma

Azure portal bir parçası olan Azure Kaynak grafik Gezgini, kaynak grafik sorgularını doğrudan Azure portal üzerinde çalıştırmaya izin verebilir. Portal iş akışınıza gerçek zamanlı dinamik bilgiler sağlamak için sonuçları dinamik grafik olarak sabitleyin. Daha fazla bilgi için bkz. [Azure Resource Graph Explorer Ile ilk sorgu](./first-query-portal.md).

Kaynak Grafiği Azure CLı, Azure PowerShell, Python için Azure SDK ve daha fazlasını destekler. Sorgu her dil için aynı şekilde yapılandırılmıştır. Kaynak grafiğini nasıl etkinleştireceğinizi öğrenin:

- [Azure portal ve kaynak grafiği Gezgini](./first-query-portal.md) 
- [Azure CLI](./first-query-azurecli.md#add-the-resource-graph-extension)
- [Azure PowerShell](./first-query-powershell.md#add-the-resource-graph-module)
- [Python](./first-query-python.md#add-the-resource-graph-library)

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu dili](./concepts/query-language.md)hakkında daha fazla bilgi edinin.
- Bkz. [Başlangıç sorgularında](./samples/starter.md)kullanılan dil.
- Gelişmiş [sorgularda](./samples/advanced.md)gelişmiş kullanımlar bölümüne bakın.
