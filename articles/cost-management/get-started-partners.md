---
title: İş ortakları için Azure maliyet yönetimi 'ni kullanmaya başlama
description: Bu makalede, iş ortaklarının Azure maliyet yönetimi özelliklerini nasıl kullandıkları ve müşterileri için maliyet yönetimi erişimini nasıl etkinleşdikleri açıklanmaktadır.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: cost-management
manager: aparnag
ms.custom: secdec18
ms.openlocfilehash: cd3efbea7b194da54bc1d9bebd1cc77987bd9dea
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2019
ms.locfileid: "74072346"
---
# <a name="get-started-with-azure-cost-management-for-partners"></a>İş ortakları için Azure maliyet yönetimi 'ni kullanmaya başlama

Azure maliyet yönetimi, müşterileri Microsoft Müşteri anlaşmasıyla eklendi ve bir Azure planı satın almış olan iş ortakları için yerel olarak kullanılabilir. Bu makalede, iş ortaklarının [Azure maliyet yönetimi](https://docs.microsoft.com/azure/cost-management/) özelliklerini nasıl kullanacağı açıklanır. Ayrıca, iş ortaklarının müşterileri için maliyet yönetimi erişimini nasıl etkinleştireceğinizi açıklar. Müşteriler, CSP iş ortakları tarafından etkinleştirildiğinde maliyet yönetimi özelliklerini kullanabilir.

CSP iş ortakları maliyet yönetimini şu şekilde kullanır:

- Faturalanmış maliyetleri anlayın ve maliyetleri müşteri, abonelikler, kaynak grupları ve hizmetlerle ilişkilendirin.
- Maliyetleri müşteri, abonelik, kaynak grubu, kaynak, ölçüm, hizmet ve diğer birçok boyuta göre analiz etme becerilerinin yanı sıra [Maliyet analizinde](quick-acm-cost-analysis.md) Azure maliyetlerinin sezgisel bir görünümünü alın.
- Maliyet analizine uygulanan Iş ortağı kazanılmış kredisi (PEC) olan kaynak maliyetlerini görüntüleyin.
- Maliyetler bütçeleri aştığında programlı [bütçeleri](tutorial-acm-create-budgets.md) ve uyarıları kullanarak bildirimleri ve Otomasyonu ayarlayın.
- Maliyet yönetimi verilerine müşteri erişimi sağlayan Azure Resource Manager ilkesini etkinleştirin. Müşteriler daha sonra [Kullandıkça Öde tarifesine](https://azure.microsoft.com/pricing/calculator/)sahip aboneliklerine ait tüketim maliyeti verilerini görüntüleyebilir.

Tüm müşterilerin maliyetlerini gösteren bir örnek aşağıda verilmiştir.
tüm müşterilerin maliyetlerini gösteren örnek ![](./media/get-started-partners/customer-costs1.png)

Tek bir müşterinin maliyetlerini gösteren bir örnek aşağıda verilmiştir.
tek bir müşterinin maliyetlerini gösteren örnek ![](./media/get-started-partners/customer-costs2.png)

Azure maliyet yönetimi 'nde kullanılabilen tüm işlevler REST API 'Leri ile de kullanılabilir. Maliyet yönetimi görevlerini otomatikleştirmek için API 'Leri kullanın.

## <a name="prerequisites"></a>Önkoşullar

Azure maliyet yönetimi, faturalandırma hesabınıza veya aboneliğine yönelik okuma erişimi gerektirir. Verileriniz üzerinde, faturalama hesabından veya bir yönetim grubundan, uygulamalarınızı yönettiğiniz tek tek kaynak gruplarına kadar erişim verilebilir. Bir faturalandırma hesabı için Azure maliyet yönetimine erişimi etkinleştirme ve atama hakkında daha fazla bilgi için bkz. [Kullanıcı rolleri ve Izinleri atama](/partner-center/permissions-overview). **Genel yönetici** ve **yönetici Aracısı** rolleri bir faturalandırma hesabının maliyetlerini yönetebilir.

Desteklenen hesap türlerinin tam listesini görüntülemek için bkz. [maliyet yönetimi verilerini anlama](understand-cost-mgt-data.md).


## <a name="how-cost-management-uses-scopes"></a>Maliyet yönetimi kapsamları nasıl kullanır

Kapsam, faturalandırma verilerini yönettiğiniz, ödemelere özgü rollere sahip olan roller, faturaları görüntüleme ve genel hesap yönetimi yürütme yerdir. Faturalandırma ve hesap rolleri, RBAC kullanan kaynak yönetimi için kullanılan kapsamlardan ayrı olarak yönetilir. Erişim denetimi farklılıkları da dahil olmak üzere ayrı kapsamların amacını açıkça ayırt etmek için sırasıyla faturalandırma kapsamları ve RBAC kapsamları olarak adlandırılır.

Faturalama kapsamlarını ve RBAC kapsamlarını ve maliyet yönetiminin kapsamlar ile nasıl çalıştığını anlamak için bkz. [kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).

## <a name="manage-costs-with-partner-tenant-billing-scopes"></a>İş ortağı kiracı faturalama kapsamları ile maliyetleri yönetme

Müşterilerinizi bir Microsoft Müşteri sözleşmesine eklendi ettikten sonra, kiracınızda aşağıdaki _Faturalama kapsamları_ kullanılabilir. Maliyet yönetimi 'nde maliyetleri yönetmek için kapsamları kullanın.

### <a name="billing-account-scope"></a>Faturalandırma hesabı kapsamı

Tüm müşterileriniz ve faturalandırma profilleriniz genelinde vergi öncesi maliyetleri görüntülemek için faturalandırma hesabı kapsamını kullanın. Fatura ücretleri yalnızca Microsoft Müşteri anlaşmasındaki müşterinin tüketim tabanlı ürünleri için gösterilir. Ancak, Microsoft Müşteri anlaşmasındaki ve CSP teklifinde müşterilere yönelik satın alınan tabanlı ürünler için fatura maliyetleri gösterilir. Şu anda, kapsamdaki maliyetleri görüntülemek için varsayılan para birimi ABD doları cinsindendir. Kapsam için ayarlanan bütçeler de USD olarak gösterilir.

Müşteri tarafından faturalandırılan farklı para birimlerinden bağımsız olarak, iş ortakları bütçe hesabı kapsamını kullanarak, müşteriler, abonelikler, kaynaklar ve kaynak gruplarında ABD Doları cinsinden maliyetleri ayarlar ve maliyetleri yönetir.

İş ortakları, maliyet analizi görünümündeki müşteriler arasında belirli bir faturalandırma para birimindeki maliyetleri de filtreleyebilir. Desteklenen müşteri faturalandırma para birimlerindeki maliyetleri görüntülemek için **gerçek maliyet** listesini seçin.

![Para birimleri için gerçek maliyet seçimini gösteren örnek](./media/get-started-partners/actual-cost-selector.png)

Bir rezervasyon dönemi genelinde ayrılmış örnek itfası maliyetlerini görüntülemek için faturalandırma kapsamlarında [itfası maliyeti görünümünü](quick-acm-cost-analysis.md#customize-cost-views) kullanın.

### <a name="billing-profile-scope"></a>Faturalandırma profili kapsamı

Bir faturaya dahil edilen tüm ürünler ve abonelikler için tüm müşterileriniz genelinde faturalandırma para birimindeki ön vergi maliyetlerini görüntülemek için Faturalandırma profili kapsamını kullanın. **InvoiceId** filtresini kullanarak, belirli bir fatura için faturalandırma profilindeki maliyetleri filtreleyebilirsiniz. Filtre, belirli bir faturaya ait tüketim ve ürün satın alma maliyetlerini gösterir. Ayrıca, ön vergi maliyetlerini görmek için faturada belirli bir müşterinin maliyetlerini filtreleyebilirsiniz.

Müşterileri bir Microsoft Müşteri sözleşmesine ekledikten sonra, Microsoft Müşteri anlaşmasındaki bu müşterilere yönelik tüm ürünlerin (tüketim, satın almalar ve yetkilendirmeler) tüm ücretlerini içeren bir fatura alırsınız. Aynı para biriminde faturalandırılırken, bu faturalar aynı zamanda, CSP teklifinde bulunan müşterilere yönelik destek ve satın alma, Azure Marketi ve rezervasyonlar gibi ücretli ürünlerin ücretini de kapsar.

Fatura profili kapsamı, müşteri faturasına karşı ücretleri mutabık kılmak için müşterileriniz için bir fatura için tahakkuk eden tüm maliyetleri görmenizi sağlar. Faturada olduğu gibi, kapsam, yeni Microsoft Müşteri anlaşmasındaki her müşteri için maliyetleri gösterir. Kapsam aynı zamanda geçerli CSP teklifinde müşteri yetkilendirme ürünleri için her ücreti de gösterir.

Faturalandırma profili ve faturalandırma hesabı kapsamları, Azure Marketi ve rezervasyon satın alımları gibi yetkilendirme ve satın alma tabanlı ürünlerin ücretlerini gösteren tek geçerli kapsamlardır.

Faturalandırma profilleri, bir faturaya dahil edilen abonelikleri tanımlar. Faturalandırma profilleri, bir kurumsal anlaşma kaydının işlevsel eşdeğeridir. Faturalandırma profili, faturaların oluşturulduğu kapsamdır.

Şu anda, Faturalandırma profili kapsamındaki maliyetler görüntülenirken müşterinin faturalandırma para birimi varsayılan para birimidir. Faturalama profili kapsamında ayarlanan bütçeler faturalandırma para birimindedir.

İş ortakları, faturalara mutabık kılmak için kapsamı kullanabilir. Ayrıca, aşağıdaki öğeler için faturalandırma para birimindeki bütçeleri ayarlamak için kapsamı kullanırlar:

- Belirli filtrelenmiş fatura
- Müşteri
- Abonelik
- Kaynak grubu
- Kaynak
- Azure hizmeti
- Ölçüm
- Resellermpnıd

### <a name="customer-scope"></a>Müşteri kapsamı

İş ortakları, Microsoft Müşteri anlaşmasıyla ilgili eklendi müşterilerin maliyetlerini yönetmek için kapsamı kullanır. Kapsam, iş ortaklarının belirli bir müşteri için vergi öncesi maliyetlerini görüntülemesine olanak tanır. Ayrıca, belirli bir abonelik, kaynak grubu veya kaynak için vergi öncesi maliyetlerine filtre uygulayabilirsiniz.

Müşteri kapsamı, geçerli CSP teklifinde bulunan müşterileri içermez. Kapsam yalnızca Microsoft Müşteri Sözleşmesi olan müşterileri içerir. Azure kullanımı değil, geçerli CSP teklifi için yetkilendirme maliyetleri, müşteri filtresini uyguladığınızda faturalandırma hesabı ve faturalandırma profili kapsamlarında kullanılabilir.

## <a name="partner-access-to-billing-scopes-in-cost-management"></a>Maliyet yönetimi 'nde faturalandırma kapsamlarına iş ortağı erişimi

Yalnızca **genel yönetici** ve yönetici Aracısı rollerine sahip olan kullanıcılar, doğrudan ortağın Azure **kiracısında** faturalandırma hesapları, faturalandırma profilleri ve müşteriler için maliyetleri yönetebilir ve görüntüleyebilir. İş Ortağı Merkezi rolleri hakkında daha fazla bilgi için bkz. [Kullanıcı rolleri ve Izinleri atama](/partner-center/permissions-overview).

## <a name="enable-cost-management-in-the-customer-tenant"></a>Müşteri kiracısında maliyet yönetimini etkinleştirme

İş ortakları, müşterilerin bir Microsoft Müşteri sözleşmesine eklendi sonra maliyet yönetimine erişmesini sağlayabilir. Daha sonra, iş ortakları, müşterilerin maliyetlerini Kullandıkça Öde perakende tarifelerine göre görüntülemesini sağlayan bir ilkeyi etkinleştirebilir. Maliyetler, RBAC aboneliği ve kaynak grupları kapsamlarında tüketilen kullanımlar için müşterinin faturalandırma para biriminde gösterilir.

Maliyet görünürlüğü ilkesi iş ortağı tarafından etkinleştirildiğinde, aboneliğe Azure Resource Manager erişimi olan herhangi bir Kullanıcı, Kullandıkça Öde tarifelerine göre maliyetleri yönetebilir ve analiz edebilir. Etkin, Azure aboneliklerine uygun RBAC erişimine sahip satıcılar ve müşteriler maliyeti görüntüleyebilir.

İlke ne olursa olsun, iş ortakları, abonelik ve kaynak grubuna erişimleri varsa maliyetleri de görüntüleyebilirler.

### <a name="enable-the-policy-to-view-azure-usage-charges"></a>Azure kullanım ücretlerini görüntülemek için ilkeyi etkinleştirin

İş ortakları, bu bilgileri kullanarak müşterilerine yönelik Azure kullanım ücretlerini görüntüleme ilkesini etkinleştirir.

Azure portal, iş ortağı kiracısında oturum açın ve **maliyet yönetimi + faturalandırma**' e tıklayın. Bir faturalandırma hesabı seçin ve ardından **müşteriler**' e tıklayın. Müşterilerin listesi faturalandırma hesabıyla ilişkilendirilir.

Müşteriler listesinde, maliyetleri görüntülemek için izin vermek istediğiniz müşteriyi seçin.

![Maliyet yönetimi 'nde müşterileri seçin](./media/get-started-partners/customer-list.png)

**Ayarlar**altında **ilkeler**' e tıklayın.

Geçerli maliyet görünürlüğü ilkesi, Seçili müşteriyle ilgili aboneliklerle ilişkili **Azure kullanım** ücretleri için gösterilir.
müşterilerin Kullandıkça Öde ücretlerini görüntülemesine izin vermek için ![](./media/get-started-partners/cost-management-billing-policies.png)

İlke **Hayır**olarak ayarlandığında, müşteriyle ilişkili abonelik kullanıcıları Için Azure maliyet yönetimi kullanılamaz. Bir iş ortağı tarafından etkinleştirilmediği müddetçe, tüm abonelik kullanıcıları için maliyet görünürlük ilkesi varsayılan olarak devre dışıdır.

Maliyet ilkesi **Evet**olarak ayarlandığında, müşteri kiracısıyla ilişkili abonelik kullanıcıları Kullandıkça Öde tarifelerine göre kullanım ücretlerini görebilirler.

Maliyet görünürlüğü ilkesi etkinleştirildiğinde, abonelik kullanımı olan tüm hizmetler Kullandıkça Öde tarifelerine göre maliyetleri gösterir. Rezervasyon kullanımı, gerçek ve ortaya çıkan maliyetler için sıfır ücret ile görünür. Satın alımlar ve yetkilendirmeler belirli bir abonelikle ilişkili değildir. Bu nedenle, satın alma işlemleri abonelik kapsamında gösterilmez.

Müşteri kiracının maliyetlerini görüntülemek için maliyet yönetimi + Faturalandırma ' i açın ve faturalama hesapları ' na tıklayın. Faturalandırma hesapları listesinde, bir faturalandırma hesabına tıklayın.

![Bir faturalandırma hesabı seçin](./media/get-started-partners/select-billing-account.png)

**Faturalama**altında **Azure abonelikleri**' ne ve ardından bir müşteriye tıklayın.

![Bir Azure aboneliği müşterisi seçin](./media/get-started-partners/subscriptions-select-customer.png)

**Maliyet Analizi** ' ne tıklayın ve maliyetleri gözden geçirmeye başlayın.
Maliyet analizi, bütçeler ve uyarılar, Kullandıkça Öde tarifesine göre abonelik ve kaynak grubu RBAC kapsamları için kullanılabilir.

![Maliyet analizini müşteri olarak görüntüleme ](./media/get-started-partners/customer-tenant-view-cost-analysis.png)

RBAC kapsamlarındaki ayrılmış örnekler için itfası görünümleri ve gerçek maliyetler sıfır ücret gösterir. Ayrılmış örnek maliyetleri yalnızca satınalmaların yapıldığı faturalandırma kapsamlarında gösterilir.

## <a name="analyze-costs-in-cost-analysis"></a>Maliyet analizinde maliyetleri analiz etme

İş ortakları, belirli bir müşteri ya da bir fatura için müşteriler genelinde maliyet analizinde maliyetleri araştırabilir ve analiz edebilir.

Kullanım ayrıntı dosyaları ve maliyet yönetimi API 'Lerinde aşağıdaki alanlar bulunur. Maliyetleri birden çok alana göre analiz etmek için, maliyet analizinde filtre ve gruplandırma özelliklerini kullanabilirsiniz. Alanların tüm listesini görüntülemek için bkz. [maliyet yönetimi veri alanları](understand-cost-mgt-data.md#cost-management-data-fields).

| Alan adı | Açıklama |
| --- | --- |
| Customertenantıd | Müşteri&#39;aboneliğinin Azure Active Directory kiracının tanımlayıcısı. |
| CustomerName | Müşteri&#39;aboneliğinin Azure Active Directory kiracının adı. |
| CustomerTenantDomainName | Müşteri&#39;aboneliğinin Azure Active Directory kiracının etki alanı adı. |
| Partnertenantıd | İş ortağı&#39;Azure Active Directory kiracının tanımlayıcısı. |
| PartnerName | Kiracının kiracı Azure Active Directory adı. |
| Resellermpnıd | Abonelikle ilişkili Bayi için MPNıD. |
| Maliyetinusd | ABD Doları cinsinden vergi öncesi tahmini ExtendedCost veya karıştırılan maliyet. |
| Paygcostınbillingcurrency | Fiyatlandırma perakende fiyatlarında ise maliyetleri gösterir. Faturalandırma para birimiyle Kullandıkça Öde fiyatlarını gösterir. Yalnızca RBAC kapsamlarında kullanılabilir. |
| Paygcostınusd | Fiyatlandırma perakende fiyatlarında ise maliyetleri gösterir. ABD Doları cinsinden Kullandıkça Öde fiyatlarını gösterir. Yalnızca RBAC kapsamlarında kullanılabilir. |
| Partnerearnedtrate | İş ortağı yönetici bağlantısı erişimine dayalı bir iş ortağı kazanılmış kredisi (PEC) varsa, uygulanan indirim oranı. |
| Ortaklıklıya Tattatmi | Ortağın kazanılmış kredisi uygulanıp uygulanmadığı gösterir. |

[Maliyet Analizi](quick-acm-cost-analysis.md) görünümünde, [görünümleri kaydedebilir](quick-acm-cost-analysis.md#saving-and-sharing-customized-views) ve verileri [CSV ve PNG dosyalarına](quick-acm-cost-analysis.md#automation-and-offline-analysis)dışarı aktarabilirsiniz.

## <a name="view-partner-earned-credit-pec-resource-costs"></a>Iş ortağı kazanılmış kredi (PEC) kaynak maliyetlerini görüntüle

Azure maliyet yönetimi 'nde iş ortakları, PEC avantajlarının alındığı maliyetleri görüntülemek için maliyet analizini kullanabilir.

Azure portal, iş ortağı kiracısında oturum açın ve **maliyet yönetimi + faturalandırma**' i seçin. **Maliyet yönetimi**altında **Maliyet Analizi**' ne tıklayın.

Maliyet analizi görünümü iş ortağı için faturalandırma hesabının maliyetlerini gösterir. Faturaları mutabık kılmak için iş ortağı, belirli bir müşteri veya faturalandırma profili için gereken **kapsamı** seçin.

Halka grafiğinde, açılan listeye tıklayın ve sonra da PEC 'in maliyetlerine gitmek için **Partnertatnedalacaktappnda** ' i seçin.

![İş ortağının kazanılan krediyi görüntülemeyi gösteren örnek](./media/get-started-partners/cost-analysis-pec1.png)

**Partnerearnedtappekonomi** özelliği _true_olduğunda, ilişkili maliyet ortağın kazanılan yönetici erişiminin avantajına sahiptir.

**Partnerearnedtappekonomi** özelliği _false_olduğunda, ilişkili maliyet kredi için gereken uygunluğu karşılıyordu. Ya da satın alınan hizmet, iş ortağının kazanılmış kredisi için uygun değildir.

Hizmet kullanım verilerinin normalde maliyet yönetimi 'nde görüntülenmesi 8-24 saat sürer. Daha fazla bilgi için bkz. [kullanım verileri güncelleştirme sıklığı değişir](understand-cost-mgt-data.md#usage-data-update-frequency-varies). PEC kredileri, Azure maliyet yönetimi 'nde erişim zamanından 48 saat içinde görünür.


Ayrıca, **Grup ölçütü** seçeneklerini kullanarak **Partnertatnedalacaklı tapptatmi** özelliğine göre gruplandırabilirsiniz ve filtre uygulayabilirsiniz. PEC 'e sahip olmayan ve desteklemeyen maliyetleri incelemek için seçenekleri kullanın.

![İş ortağı tarafından grup veya filtreleme-kazanılan kredi](./media/get-started-partners/cost-analysis-pec2.png)

## <a name="cost-management-rest-apis"></a>Maliyet yönetimi REST API 'Leri

İş ortakları ve müşteriler ortak görevler için aşağıdaki bölümlerde açıklanan maliyet yönetimi API 'Lerini kullanabilir.

### <a name="azure-cost-management-apis---direct-and-indirect-providers"></a>Azure maliyet yönetimi API 'Leri-doğrudan ve dolaylı sağlayıcılar

Bir iş ortağı kiracısındaki faturalandırma kapsamlarına erişimi olan iş ortakları, faturalanmış maliyetleri görüntülemek için aşağıdaki API 'Leri kullanabilir.

Aboneliğe erişimleri varsa, abonelik kapsamındaki API 'Ler, maliyet ilkesinden bağımsız olarak bir iş ortağı tarafından çağrılabilir. Abonelik erişimi olan diğer kullanıcılar, müşteri veya satıcı gibi, yalnızca iş ortağı müşteri kiracının maliyet ilkesini etkinleştirdikten sonra API 'Leri çağırabilir.


#### <a name="to-get-a-list-of-billing-accounts"></a>Faturalama hesaplarının bir listesini almak için

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-customers"></a>Müşterilerin bir listesini almak için

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-subscriptions"></a>Aboneliklerin listesini almak için

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/billingSubscriptions?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-invoices-for-a-period-of-time"></a>Bir süre için faturaların listesini almak için

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/invoices?api-version=2019-10-01-preview&periodStartDate={periodStartDate}&periodEndDate={periodEndDate}
```

API çağrısı, aşağıdaki JSON koduna benzer öğeler içeren bir fatura dizisi döndürür.

```
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/{billingAccountID}/billingProfiles/{BillingProfileID}/invoices/{InvoiceID}",
      "name": "{InvoiceID}",
      "properties": {
        "amountDue": {
          "currency": "USD",
          "value": x.xx
        },
        ...
    }
```

Önceki döndürülen KIMLIK alanı değerini kullanın ve aşağıdaki örnekte, kullanım ayrıntılarını sorgulayan kapsam olarak değiştirin.

```
GET https://management.azure.com/{id}/providers/Microsoft.Consumption/UsageDetails?api-version=2019-10-01
```

Örnek, belirli faturayla ilişkili kullanım kayıtlarını döndürür.


#### <a name="to-get-the-policy-for-customers-to-view-costs"></a>Müşterilerin maliyetleri görüntüleme ilkesini almak için

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/policies/default?api-version=2019-10-01-preview
```

#### <a name="to-set-the-policy-for-customers-to-view-costs"></a>Müşterilerin maliyetleri görüntülemesi için ilkeyi ayarlamak için

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/policies/default?api-version=2019-10-01-preview
```

#### <a name="to-get-azure-service-usage-for-a-billing-account"></a>Bir faturalandırma hesabı için Azure hizmet kullanımını almak için

```
GET https://management.azure.com/providers/Microsoft.Billing/BillingAccounts/{billingAccountName}/providers/Microsoft.Consumption/usageDetails?api-version=2019-10-01
```

#### <a name="to-download-a-customers-azure-service-usage"></a>Müşterinin Azure hizmet kullanımını indirmek için

Aşağıdaki Get çağrısı zaman uyumsuz bir işlemdir.

```
GET https://management.azure.com/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/providers/Microsoft.Consumption/usageDetails/download?api-version=2019-10-01 -verbose
```

İşlemin durumunu denetlemek için yanıtta döndürülen `Location` URI 'sini çağırın. Durum *tamamlandığında*, `downloadUrl` özelliği oluşturulan raporu indirmek için kullanabileceğiniz bir bağlantı içerir.


#### <a name="to-get-or-download-the-price-sheet-for-consumed-azure-services"></a>Tüketilen Azure hizmetleri için fiyat listesi almak veya indirmek için

İlk olarak, aşağıdaki gönderiyi kullanın.

```
POST https://management.azure.com/providers/Microsoft.Billing/BillingAccounts/{billingAccountName}/billingProfiles/{billingProfileID}/pricesheet/default/download?api-version=2019-10-01-preview&format=csv" -verbose
```

Ardından, zaman uyumsuz işlem özelliği değerini çağırın. Örneğin:

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/billingProfiles/{billingProfileID}/pricesheetDownloadOperations/{operation}?sessiontoken=0:11186&api-version=2019-10-01-preview
```
Önceki Get çağrısı, Fiyat listesini içeren indirme bağlantısını döndürür.


#### <a name="to-get-aggregated-costs"></a>Toplu maliyetleri almak için

```
POST https://management.azure.com/providers/microsoft.billing/billingAccounts/{billingAccountName}/providers/microsoft.costmanagement/query?api-version=2019-10-01
```

#### <a name="create-a-budget-for-a-partner"></a>İş ortağı için bütçe oluşturma

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/providers/Microsoft.CostManagement/budgets/partnerworkshopbudget?api-version=2019-10-01
```

#### <a name="create-a-budget-for-a-customer"></a>Müşteri için bütçe oluşturma

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/providers/Microsoft.Consumption/budgets/{budgetName}?api-version=2019-10-01
```

#### <a name="delete-a-budget"></a>Bir bütçeyi silme

```
PUT
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.CostManagement/budgets/{budgetName}?api-version=2019-10-01
```


## <a name="next-steps"></a>Sonraki adımlar
- Maliyet yönetimi 'nde [maliyetleri çözümlemeye başlayın](quick-acm-cost-analysis.md)
- Maliyet yönetimi 'nde [bütçeler oluşturma ve yönetme](tutorial-acm-create-budgets.md)
