---
title: İş ortakları için Azure maliyet yönetimi 'ni kullanmaya başlama
description: Bu makalede, iş ortaklarının Azure maliyet yönetimi özelliklerini nasıl kullandıkları ve müşterileri için maliyet yönetimi erişimini nasıl etkinleşdikleri açıklanmaktadır.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/14/2019
ms.topic: conceptual
ms.service: cost-management
manager: aparnag
ms.custom: secdec18
ms.openlocfilehash: 9d95e23cf92c7ee98291831d60088d610c3e5c52
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72377700"
---
# <a name="get-started-with-azure-cost-management-for-partners"></a>İş ortakları için Azure maliyet yönetimi 'ni kullanmaya başlama

Azure maliyet yönetimi, müşterilerinin Microsoft Müşteri anlaşmasıyla eklendi olan iş ortakları için yerel olarak kullanılabilir. Bu makalede, iş ortaklarının [Azure maliyet yönetimi](https://docs.microsoft.com/azure/cost-management/) özelliklerini nasıl kullanacağı açıklanır. Ayrıca, iş ortaklarının müşterileri için maliyet yönetimi erişimini nasıl etkinleştireceğinizi açıklar. CSP müşterileri, CSP iş ortakları tarafından etkinleştirildiğinde maliyet yönetimi özelliklerini kullanabilir.

CSP iş ortakları maliyet yönetimini şu şekilde kullanır:

- Faturalanmış maliyetleri anlayın ve maliyetleri müşteri, abonelikler, kaynak grupları ve hizmetlerle ilişkilendirin.
- Maliyetleri müşteri, abonelik, kaynak grubu, kaynak, ölçüm, hizmet ve diğer birçok boyuta göre analiz etme becerilerinin yanı sıra [Maliyet analizinde](quick-acm-cost-analysis.md) Azure maliyetlerinin sezgisel bir görünümünü alın.
- Maliyet analizine uygulanan Iş ortağı kazanılmış kredisi (PEC) olan kaynak maliyetlerini görüntüleyin.
- Maliyetler bütçeleri aştığında programlı [bütçeleri](tutorial-acm-create-budgets.md) ve uyarıları kullanarak bildirimleri ve Otomasyonu ayarlayın.
- Maliyet yönetimi verilerine müşteri erişimi sağlayan Azure Resource Manager ilkesini etkinleştirin. Müşteriler daha sonra [Kullandıkça Öde tarifesine](https://azure.microsoft.com/pricing/calculator/)sahip aboneliklerine ait tüketim maliyeti verilerini görüntüleyebilir.

Azure maliyet yönetimi 'nde kullanılabilen tüm işlevler REST API 'Leri ile de kullanılabilir. Maliyet yönetimi görevlerini otomatikleştirmek için API 'Leri kullanın.

## <a name="prerequisites"></a>Önkoşullar

Azure maliyet yönetimi, faturalandırma hesabınıza veya aboneliğine yönelik okuma erişimi gerektirir. Verileriniz üzerinde, faturalama hesabından veya bir yönetim grubundan, uygulamalarınızı yönettiğiniz tek tek kaynak gruplarına kadar erişim verilebilir. Abonelik kullanıcılarının fiyatlandırma ve maliyetleri görmesini sağlamak için, faturalandırma hesabınız için görüntüleme ücretlerine erişim etkinleştirilmelidir. Azure maliyet yönetimi 'ne erişimi etkinleştirme ve atama hakkında daha fazla bilgi için bkz. [verilere erişim atama](assign-access-acm-data.md). Desteklenen hesap türlerinin tam listesini görüntülemek için bkz. [maliyet yönetimi verilerini anlama](understand-cost-mgt-data.md).


## <a name="how-cost-management-uses-scopes"></a>Maliyet yönetimi kapsamları nasıl kullanır

Kapsam, faturalandırma verilerini yönettiğiniz, ödemelere özgü rollere sahip olan roller, faturaları görüntüleme ve genel hesap yönetimi yürütme yerdir. Faturalandırma ve hesap rolleri, RBAC kullanan kaynak yönetimi için kullanılan kapsamlardan ayrı olarak yönetilir. Erişim denetimi farklılıkları da dahil olmak üzere ayrı kapsamların amacını açıkça ayırt etmek için sırasıyla faturalandırma kapsamları ve RBAC kapsamları olarak adlandırılır.

Faturalama kapsamlarını ve RBAC kapsamlarını ve maliyet yönetiminin kapsamlar ile nasıl çalıştığını anlamak için bkz. [kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).

## <a name="manage-costs-with-partner-tenant-billing-scopes"></a>İş ortağı kiracı faturalama kapsamları ile maliyetleri yönetme

Müşterilerinizi bir Microsoft Müşteri sözleşmesine eklendi ettikten sonra, kiracınızda aşağıdaki _Faturalama kapsamları_ kullanılabilir. Maliyet yönetimi 'nde maliyetleri yönetmek için kapsamları kullanın.

### <a name="billing-account-scope"></a>Faturalandırma hesabı kapsamı

Tüm müşterileriniz ve faturalandırma profilleriniz genelinde vergi öncesi maliyetleri görüntülemek için faturalandırma hesabı kapsamını kullanın. Ayrıca, Microsoft Müşteri anlaşmasındaki müşteriler için tüketim tabanlı ürünlerin fatura maliyetlerini görüntüleyebilirsiniz. Fatura ücretleri, Microsoft Müşteri anlaşmasındaki ve CSP teklifinde müşterilere yönelik satın alınan tabanlı ürünler için de gösterilir. Şu anda, kapsamdaki maliyetleri görüntülemek için varsayılan para birimi ABD doları cinsindendir. Kapsam için ayarlanan bütçeler de USD olarak gösterilir.

Müşteri tarafından faturalandırılan para biriminden bağımsız olarak, iş ortakları bütçeleri ayarlamak ve müşterileri, abonelikleri, kaynakları ve kaynak grupları genelinde ABD Doları cinsinden maliyetleri yönetmek için kapsamı kullanır.

İş ortakları, maliyet analizi görünümündeki müşteriler arasında belirli bir faturalandırma para birimindeki maliyetleri de filtreleyebilir. Desteklenen müşteri faturalandırma para birimlerindeki maliyetleri görüntülemek için **gerçek maliyet** listesini seçin.

![Para birimleri için gerçek maliyet seçimini gösteren örnek](./media/get-started-partners/actual-cost-selector.png)

Bir rezervasyon dönemi genelinde ayrılmış örnek itfası maliyetlerini görüntülemek için faturalandırma kapsamlarında [itfası maliyeti görünümünü](quick-acm-cost-analysis.md#customize-cost-views) kullanın.

### <a name="billing-profile-scope"></a>Faturalandırma profili kapsamı

Bir faturaya dahil edilen tüm ürünler ve abonelikler için tüm müşterileriniz genelinde faturalandırma para birimindeki ön vergi maliyetlerini görüntülemek için Faturalandırma profili kapsamını kullanın. **InvoiceId** filtresini kullanarak, belirli bir fatura için faturalandırma profilindeki maliyetleri filtreleyebilirsiniz. Filtre, belirli bir faturaya ait tüketim ve ürün satın alma maliyetlerini gösterir. Ayrıca, ön vergi maliyetlerini görmek için faturada belirli bir müşterinin maliyetlerini filtreleyebilirsiniz.

Müşterileri bir Microsoft Müşteri sözleşmesine ekledikten sonra, SaaS, Azure Marketi ve rezervasyonlar gibi yetkilendirme ve satın alınan ürünlerin ücretini gösteren bir müşteri faturası alırsınız. Aynı faturalandırma para biriminde faturalandırılırken, faturada yeni Microsoft Müşteri sözleşmesinde bulunmayan müşteri ücretleri de gösterilir.

Fatura profili kapsamı, müşteri faturasına karşı ücretleri mutabık kılmak için müşterileriniz için bir fatura için tahakkuk eden tüm maliyetleri görmenizi sağlar. Faturada olduğu gibi, kapsam, yeni Microsoft Müşteri anlaşmasındaki her müşteri için maliyetleri gösterir. Kapsam aynı zamanda geçerli CSP teklifinde müşteri yetkilendirme ürünleri için her ücreti de gösterir.

Faturalandırma profili ve faturalandırma hesabı kapsamları, yetkilendirme ve satın alma tabanlı ürünlerin ücretlerini gösteren tek alanlardır.

Faturalandırma profilleri, bir faturaya dahil edilen abonelikleri tanımlar. Faturalandırma profilleri, bir kurumsal anlaşma kaydının işlevsel eşdeğeridir. Kayıt, faturaların oluşturulduğu kapsamdır. Benzer şekilde, Azure Marketi ve ayırmaları gibi kullanım tabanlı olmayan satın alma işlemleri yalnızca Faturalandırma profili kapsamında kullanılabilir.

Şu anda, Faturalandırma profili kapsamındaki maliyetler görüntülenirken müşterinin faturalandırma para birimi varsayılan para birimidir. Faturalama profili kapsamında ayarlanan bütçeler faturalandırma para birimindedir.

İş ortakları, faturalara mutabık kılmak için kapsamı kullanabilir. Ayrıca, bir için ödeme para birimindeki bütçeleri ayarlamak için kapsamı kullanırlar:

- Belirli filtrelenmiş fatura
- Müşterisi
- Abonelik
- Kaynak grubu
- Kaynak
- Azure hizmeti
- Ölçüm
- Resellermpnıd

### <a name="customer-scope"></a>Müşteri kapsamı

İş ortakları, Microsoft Müşteri anlaşmasıyla ilgili eklendi müşterilerin maliyetlerini yönetmek için kapsamı kullanır. Kapsam, iş ortaklarının belirli bir müşteri için vergi öncesi maliyetlerini görüntülemesine olanak tanır. Ayrıca, belirli bir abonelik, kaynak grubu veya kaynak için vergi öncesi maliyetlerine filtre uygulayabilirsiniz.

Müşteri kapsamı, geçerli CSP teklifinde bulunan müşterileri içermez. Azure kullanımı değil, geçerli CSP teklifi için yetkilendirme maliyetleri, müşteri filtresini uyguladığınızda faturalandırma hesabı ve faturalandırma profili kapsamlarında kullanılabilir.

## <a name="partner-access-to-billing-scopes-in-cost-management"></a>Maliyet yönetimi 'nde faturalandırma kapsamlarına iş ortağı erişimi

Yalnızca **genel yönetici** ve yönetici Aracısı rollerine sahip olan kullanıcılar, doğrudan ortağın Azure **kiracısında** faturalandırma hesapları, faturalandırma profilleri ve müşteriler için maliyetleri yönetebilir ve görüntüleyebilir. İş Ortağı Merkezi rolleri hakkında daha fazla bilgi için bkz. [Kullanıcı rolleri ve Izinleri atama](/partner-center/permissions-overview).

### <a name="enable-cost-management-in-the-customer-tenant"></a>Müşteri kiracısında maliyet yönetimini etkinleştirme

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
@no__t-, müşterilerin Kullandıkça Öde ücretlerini görüntülemesine izin veren 0No__t-1

İlke **Hayır**olarak ayarlandığında, müşteriyle ilişkili abonelik kullanıcıları Için Azure maliyet yönetimi kullanılamaz. Bir iş ortağı tarafından etkinleştirilmediği müddetçe, tüm abonelik kullanıcıları için maliyet görünürlük ilkesi varsayılan olarak devre dışıdır.

Maliyet ilkesi **Evet**olarak ayarlandığında, müşteri kiracısıyla ilişkili abonelik kullanıcıları Kullandıkça Öde tarifelerine göre kullanım ücretlerini görebilirler.

Maliyet görünürlüğü ilkesi etkinleştirildiğinde, abonelik kullanımı olan tüm hizmetler Kullandıkça Öde tarifelerine göre maliyetleri gösterir. Rezervasyon kullanımı, gerçek ve ortaya çıkan maliyetler için sıfır ücret ile görünür. Satın alımlar ve yetkilendirmeler belirli bir abonelikle ilişkili değildir. Bu nedenle, satın alma işlemleri abonelik kapsamında gösterilmez.

Müşteri kiracının maliyetlerini görüntülemek için maliyet yönetimi + Faturalandırma ' i açın ve faturalama hesapları ' na tıklayın. Faturalandırma hesapları listesinde, bir faturalandırma hesabına tıklayın.

![Bir faturalandırma hesabı seçin](./media/get-started-partners/select-billing-account.png)

**Faturalama**altında **Azure abonelikleri**' ne ve ardından bir müşteriye tıklayın.

![Bir Azure aboneliği müşterisi seçin](./media/get-started-partners/subscriptions-select-customer.png)

**Maliyet Analizi** ' ne tıklayın ve maliyetleri gözden geçirmeye başlayın.
Maliyet analizi, bütçeler ve uyarılar, Kullandıkça Öde tarifesine göre, abonelik ve kaynak grubu RBAC kapsamları için artık kullanılabilir.

![Maliyet analizini müşteri olarak görüntüleme ](./media/get-started-partners/customer-tenant-view-cost-analysis.png)

RBAC kapsamlarındaki ayrılmış örnekler için itfası görünümleri ve gerçek maliyetler sıfır ücret gösterir. Ayrılmış örnek maliyetleri yalnızca satınalmaların yapıldığı faturalandırma kapsamlarında gösterilir.

## <a name="analyze-costs-in-cost-analysis"></a>Maliyet analizinde maliyetleri analiz etme

İş ortakları, belirli bir müşteri ya da bir fatura için müşteriler genelinde maliyet analizinde maliyetleri araştırabilir ve analiz edebilir. Filtre ve gruplandırma özellikleri, aşağıdakiler de dahil olmak üzere birden çok alana göre maliyetleri analiz etmenize olanak tanır:

| **Alan** | **Açıklama** | **Iş Ortağı Merkezi 'nde denk sütun** |
| --- | --- | --- |
| Partnertenantıd | Ortağın Azure Active Directory kiracısı için tanımlayıcı | İş ortağı Azure Active Directory Tenantıd Iş ortağı KIMLIĞI olarak çağrıldı. GUID biçiminde. |
| PartnerName | Kiracının Azure Active Directory kiracı adı | İş ortağı adı |
| Customertenantıd | Müşterinin aboneliğine ait Azure Active Directory kiracının tanımlayıcısı | Müşterinin kuruluş KIMLIĞI. Örneğin, müşteri Azure Active Directory Tenantıd. |
| CustomerName | Müşterinin aboneliğini içeren Azure Active Directory kiracının adı | Iş Ortağı Merkezi 'nde raporlanan müşterinin kuruluş adı. Sistem bilgileriniz ile faturayı mutabık kılma için önemli. |
| Resellermpnıd | Abonelikle ilişkili Bayi için MPNıD | MPN, aboneliğin kayıt satıcısının KIMLIĞI. Geçerli etkinlik için kullanılamaz. |
| abonelik kimliği | Azure aboneliği için Microsoft tarafından oluşturulan benzersiz tanımlayıcı | Yok |
| subscriptionName | Azure aboneliğinin adı | Yok |
| Billingprofileıd | Faturalandırma profili için tanımlayıcı. Müşteriler genelinde tek bir faturalandırma para birimiyle, faturalardaki maliyetleri gruplandırır. | MCAPı Iş ortağı faturalama grubu KIMLIĞI. API isteklerinde kullanılır, ancak yanıtlara dahil edilmez. |
| InvoiceId | Faturada belirli bir işlemin göründüğü fatura KIMLIĞI | Belirtilen hareketin göründüğü fatura numarası. |
| resourceGroup | Azure Kaynak grubunun adı. Kaynak yaşam döngüsü yönetimi için kullanılır. | Kaynak grubunun adı. |
| Partnerearnedtrate | İş ortağı yönetici bağlantısı erişimine dayalı bir iş ortağı kazanılmış kredisi (PEC) varsa, indirim ücreti uygulanır. | İş ortağı kazanılmış kredi (PEC) oranı. Örneğin, %0 veya %15. |
| Ortaklıklıya Tattatmi | Ortağın kazanılmış kredisi uygulanıp uygulanmadığı gösterir. | Yok |

[Maliyet Analizi](quick-acm-cost-analysis.md) görünümünde, [görünümleri kaydedebilir](quick-acm-cost-analysis.md#saving-and-sharing-customized-views) ve verileri [CSV ve PNG](quick-acm-cost-analysis.md#automation-and-offline-analysis) dosyalarına dışarı aktarabilirsiniz.

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

İş ortakları, dolaylı sağlayıcılar ve müşteriler ortak görevler için aşağıdaki bölümlerde açıklanan maliyet yönetimi API 'Lerini kullanabilir.

### <a name="azure-cost-management-apis-for-partners"></a>İş ortakları için Azure maliyet yönetimi API 'Leri

Bir iş ortağı kiracısındaki faturalandırma kapsamlarına erişimi olan iş ortakları ve kullanıcılar aşağıdaki API 'Leri kullanabilir.

#### <a name="to-get-a-list-of-billing-accounts"></a>Faturalama hesaplarının bir listesini almak için

```
armclient get "providers/Microsoft.billing/billingAccounts?api-version=2019-10-01-preview"
```

#### <a name="to-get-a-list-of-customers"></a>Müşterilerin bir listesini almak için

```
armclient get "providers/Microsoft.billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers?api-version=2019-10-01-preview"
```
#### <a name="to-get-a-list-of-subscriptions"></a>Aboneliklerin listesini almak için

```
armclient get "/providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers/9553eda2-2bd7-4ae6-a1f8-6a19eb40be22/billingSubscriptions?api-version=2019-10-01-preview"
```

#### <a name="to-create-new-subscription"></a>Yeni abonelik oluşturmak için

```
armclient post "/providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers/9553eda2-2bd7-4ae6-a1f8-6a19eb40be22/providers/Microsoft.Subscription/createSubscription?api-version=2018-11-01-preview" @createsub.json -verbose
```

#### <a name="to-get-or-download-usage-for-azure-services"></a>Azure hizmetleri için kullanımı almak veya indirmek için

```
armclient GET /providers/Microsoft.Billing/BillingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/providers/Microsoft.Consumption/usageDetails?api-version=2019-10-01
```

#### <a name="to-get-a-list-of-billing-profiles"></a>Faturalama profillerinin bir listesini almak için

```
armclient get "providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/billingProfiles?api-version=2019-10-01-preview
```

#### <a name="to-get-or-download-the-price-sheet-for-consumed-azure-services"></a>Tüketilen Azure hizmetleri için fiyat listesi almak veya indirmek için

```
armclient post "/providers/Microsoft.Billing/BillingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/BillingProfiles/JUT6-EU3Q-BG7-TGB/pricesheet/default/download?api-version=2019-10-01-preview&format=csv" -verbose
```

#### <a name="to-get-customer-costs-for-the-last-two-months-sorted-by-month"></a>Son iki aya ait müşteri maliyetlerini almak için, aya göre sıralanmış

```
armclient post providers/microsoft.billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31//providers/microsoft.costmanagement/query?api-version=2019-10-01 @CCMQueryCustomer.json
```

#### <a name="to-get-azure-subscription-costs-for-the-last-two-months-sorted-by-month"></a>Son iki aya ait Azure abonelik maliyetlerini, aya göre sıralanmış olarak almak için

```
armclient post providers/microsoft.billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31//providers/microsoft.costmanagement/query?api-version=2019-10-01 @CCMQuerySubscription.json
```

#### <a name="to-get-daily-costs-for-the-current-month"></a>Geçerli aya ait günlük maliyetleri almak için

```
armclient post providers/microsoft.billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31//providers/microsoft.costmanagement/query?api-version=2019-10-01 @CCMQueryDaily.json
```

#### <a name="to-get-the-policy-for-customers-to-view-costs"></a>Müşterilerin maliyetleri görüntüleme ilkesini almak için

```
armclient get "providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers/9553eda2-2bd7-4ae6-a1f8-6a19eb40be22/policies/default?api-version=2019-10-01-preview"
```

#### <a name="to-set-the-policy-for-customers-to-view-costs"></a>Müşterilerin maliyetleri görüntülemesi için ilkeyi ayarlamak için

```
armclient put "providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers/9553eda2-2bd7-4ae6-a1f8-6a19eb40be22/policies/default?api-version=2019-10-01-preview" @policy.json
```

### <a name="azure-cost-management-apis-for-indirect-providers"></a>Dolaylı sağlayıcılar için Azure maliyet yönetimi API 'Leri

Bir müşteri kiracısında RBAC kapsamlarına erişimi olan dolaylı sağlayıcılar aşağıdaki API 'Leri kullanabilir. Başlamak için Kullanıcı olarak veya hizmet sorumlusu ile oturum açın.

#### <a name="to-get-the-billing-account-information"></a>Faturalandırma hesabı bilgilerini almak için

```
armclient get "providers/Microsoft.billing/billingAccounts?api-version=2019-10-01-preview"
```

#### <a name="to-get-a-list-of-customers"></a>Müşterilerin bir listesini almak için

```
armclient get "providers/Microsoft.billing/billingAccounts/ec1b88ba-5681-517e-f657-4cc6a4a407cb:52f143a9-6524-4e5e-9d4a-120c7a79ca65_2019-05-31/customers?api-version=2019-10-01-preview"
```

#### <a name="to-get-a-list-of-resellers-associated-with-the-customer"></a>Müşteriyle ilişkili satıcıların bir listesini almak için

```
armclient get "/providers/Microsoft.Billing/billingAccounts/ec1b88ba-5681-517e-f657-4cc6a4a407cb:52f143a9-6524-4e5e-9d4a-120c7a79ca65_2019-05-31/customers/b51df1fa-62fa-4c92-9a74-fe860016d4db?api-version=2019-10-01-preview&$expand=resellers
```

#### <a name="to-get-a-list-of-subscriptions-with-reseller-information"></a>Satıcı bilgileriyle Aboneliklerin listesini almak için

```
armclient get "/providers/Microsoft.Billing/billingAccounts/ec1b88ba-5681-517e-f657-4cc6a4a407cb:52f143a9-6524-4e5e-9d4a-120c7a79ca65_2019-05-31/customers/b51df1fa-62fa-4c92-9a74-fe860016d4db/billingSubscriptions?api-version=2019-10-01-preview
```

#### <a name="to-create-a-subscription"></a>Abonelik oluşturmak için

```
armclient post "/providers/Microsoft.Billing/billingAccounts/ec1b88ba-5681-517e-f657-4cc6a4a407cb:52f143a9-6524-4e5e-9d4a-120c7a79ca65_2019-05-31/customers/b51df1fa-62fa-4c92-9a74-fe860016d4db/providers/Microsoft.Subscription/createSubscription?api-version=2018-11-01-preview" @createsub_reseller.json
```

### <a name="azure-cost-management-apis-for-customers"></a>Müşteriler için Azure maliyet yönetimi API 'Leri

Müşteriler, API 'Lere erişmek için aşağıdaki bilgileri kullanır. Başlamak için Kullanıcı olarak oturum açın.

#### <a name="to-get-or-download-azure-consumption-usage-information-with-retail-rates"></a>Azure tüketim kullanım bilgilerini perakende tarifelerinde almak veya indirmek için

```
armclient post /subscriptions/66bada28-271e-4b7a-aaf5-c0ead63923d7/providers/microsoft.costmanagement/query?api-version=2019-10-01 @CCMQueryDaily.json
```

## <a name="next-steps"></a>Sonraki adımlar
- Maliyet yönetimi 'nde [maliyetleri çözümlemeye başlayın](quick-acm-cost-analysis.md)
- Maliyet yönetimi 'nde [bütçeler oluşturma ve yönetme](tutorial-acm-create-budgets.md)
