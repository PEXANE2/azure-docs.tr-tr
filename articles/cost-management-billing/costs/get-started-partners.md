---
title: İş ortakları için Azure Maliyet Yönetimi’ni kullanmaya başlama
description: Bu makalede iş ortaklarının Azure Maliyet Yönetimi özelliklerini nasıl kullanacağı ve müşterileri için Maliyet Yönetimi'ni nasıl etkinleştirebilecekleri anlatılmaktadır.
author: bandersmsft
ms.author: banders
ms.date: 08/04/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: aparnag
ms.custom: secdec18
ms.openlocfilehash: b5f00ee67803819b4d13cdc630bed3de26ac1be9
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87563951"
---
# <a name="get-started-with-azure-cost-management-for-partners"></a>İş ortakları için Azure Maliyet Yönetimi’ni kullanmaya başlama

Azure Maliyet Yönetimi, müşterilerini bir Microsoft Müşteri Sözleşmesi'ne eklemiş ve [bir Azure Planı satın almış olan](/partner-center/purchase-azure-plan) doğrudan iş ortakları tarafından yerel olarak kullanılabilir. Bu makalede iş ortaklarının Azure Planı'ndaki aboneliklerin maliyetlerini görüntülemek için [Azure Maliyet Yönetimi](../index.yml) özelliklerini nasıl kullanabilecekleri açıklanmıştır. Ayrıca iş ortaklarının, müşterileri için perakende fiyatına Maliyet Yönetimi erişimini nasıl etkinleştirebilecekleri de anlatılmıştır.

Doğrudan iş ortakları ve dolaylı sağlayıcılar için genel yönetici ve yönetici aracıları, iş ortağı kiracısında Maliyet Yönetimi’ne erişebilir ve faturalanan ücretlerdeki maliyetleri yönetebilir.

Kurumsal bayiler ve müşteriler, Maliyet Yönetimi'ne müşteri kiracısında erişip aboneliklere ait maliyetleri görüntüleyebilir ve bu maliyetler perakende ücretleri üzerinden hesaplanır ve gösterilir. Ancak bu kullanıcıların maliyetleri görüntüleyebilmek için müşteri kiracısında aboneliğe RBAC erişimine sahip olması gerekir. Maliyet görünürlüğü ilkesinin sağlayıcı tarafından müşteri kiracısında etkinleştirilmesi gerekir.

Müşteriler, CSP iş ortakları tarafından etkinleştirilmesi durumunda Maliyet Yönetimi özelliklerini kullanabilir.

CSP iş ortakları, Maliyet Yönetimi ile şu işlemleri gerçekleştirebilir:

- Fatura edilen maliyetleri anlama ve maliyetlerle müşteriler, abonelikler, kaynak grupları ve hizmetler arasında ilişki kurma.
- [Maliyet analizi](quick-acm-cost-analysis.md) ile Azure maliyetlerini sezgisel bir şekilde görüntüleyerek müşteri, abonelik, kaynak grubu, kaynak, ölçüm, hizmet ve diğer bir çok boyuta göre maliyet analizi gerçekleştirme.
- Maliyet Analizi'nde İş Ortağı Tarafından Kazanılmış Kredi (PEC) uygulanan kaynak maliyetlerini görüntüleme.
- Programlı [bütçeleri](tutorial-acm-create-budgets.md) ve maliyetlerin bütçeyi aştığı durumlarda uyarıları kullanarak bildirim ve otomasyon sağlama.
- Müşteriye Maliyet Yönetimi verilerine erişim sağlayan Azure Resource Manager ilkesini etkinleştirme. Bu işlemin ardından müşteriler, aboneliklerine ait tüketim maliyeti verilerini [kullandıkça öde fiyatlarını](https://azure.microsoft.com/pricing/calculator/) kullanarak görüntüleyebilir.
- Maliyet ve kullanım verilerini, kullandıkça öde aboneliğiyle bir depolama blobuna aktarabilir.

Aşağıdaki örnekte tüm müşterilere ait maliyetler gösterilmiştir.

[![Tüm müşterilere ait maliyetleri gösteren örnek](./media/get-started-partners/customer-costs1.png)](./media/get-started-partners/customer-costs1.png#lightbox)

Aşağıdaki örnekte tek bir müşteriye ait maliyetler gösterilmiştir.

[![Tek bir müşteriye ait maliyetleri gösteren örnek](./media/get-started-partners/customer-costs2.png)](./media/get-started-partners/customer-costs2.png#lightbox)

Azure Maliyet Yönetimi'ndeki tüm işlevler REST API'leri aracılığıyla da kullanılabilir. API'leri kullanarak maliyet yönetimi görevlerini otomatikleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

İş ortakları için Azure Maliyet Yönetimi yalnızca Azure planı kullanan aboneliklerde yerel olarak kullanılabilir.

Azure portalında Azure Maliyet Yönetimi'ni etkinleştirmek için Microsoft Müşteri Sözleşmesi müşteri kabulünü onaylamış (müşteri adına) ve müşteriyi Azure Planı'na geçirmiş olmanız gerekir. Azure Maliyet Yönetimi'nde yalnızca Azure planına geçirilmiş olan aboneliklerin maliyetleri görüntülenir.

Azure Maliyet Yönetimi, ödeme hesabı veya abonelik için okuma yetkisine ihtiyaç duyar.

Azure Maliyet Yönetimi için ödeme hesabı erişimini etkinleştirme ve atama hakkında daha fazla bilgi için bkz. [Kullanıcı rollerini ve izinlerini atama](/partner-center/permissions-overview). **Genel yönetici** ve **Yönetici aracısı** rolleri ödeme hesabında maliyetleri yönetebilir.

Azure Maliyet Yönetimi'ne abonelik kapsamında erişmek için bir abonelikte RBAC erişimine sahip olan tüm kullanıcılar perakende (kullandıkça öde) ücretlerine ait maliyetleri görüntüleyebilir. Ancak [müşteri kiracısında maliyet görünürlüğü](#enable-the-policy-to-view-azure-usage-charges) ilkesinin etkinleştirilmiş olması gerekir. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](understand-cost-mgt-data.md).

## <a name="how-cost-management-uses-scopes"></a>Maliyet Yönetimi, kapsamları nasıl kullanır?

Kapsamlar; faturalandırma verilerini yönetmenizi, ödemelere özgü rollere sahip olmanızı, faturaları görüntülemenizi ve genel hesap yönetimi işlemlerini yaptığınız yerdir. Faturalama ve hesap rolleri, kaynak yönetimi için kullanılan ve RBAC Kullanan kapsamlardan ayrı yönetilir. Erişim denetimi farklılıkları dahil olmak üzere ayrı kapsamların amacını net bir şekilde belirlemek için faturalama kapsamları ve RBAC kapsamları şeklinde kullanılır.

Faturalama kapsamlarını, RBAC kapsamlarını ve maliyet yönetiminin farklı kapsamlarda nasıl çalıştığını anlamak için bkz. [Kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).

## <a name="manage-costs-with-partner-tenant-billing-scopes"></a>İş ortağı kiracı faturalama kapsamları ile maliyetleri yönetme

Müşterilerinizi bir Microsoft Müşteri Sözleşmesi'ne ekledikten sonra aşağıdaki _faturalama kapsamları_ kiracınızda kullanılabilir duruma gelir. Bu kapsamları kullanarak Maliyet Yönetimi'ndeki maliyetleri yönetebilirsiniz.

### <a name="billing-account-scope"></a>Ödeme hesabı kapsamı

Ödeme hesabı kapsamını kullanarak tüm müşteriler ve faturalama profilleri için vergilerden önceki maliyetleri görüntüleyebilirsiniz. Fatura maliyetleri yalnızca müşterinin Microsoft Müşteri Sözleşmesi kapsamındaki tüketim tabanlı ürünleri için gösterilir. Ancak fatura maliyetleri, hem Microsoft Müşteri Sözleşmesi hem de CSP teklifini kullanan müşterilere ait satın alma tabanlı ürünler için gösterilir. Kapsamdaki maliyetlerin görüntülenmesi için kullanılan varsayılan para birimi, ABD dolarıdır. Kapsam için belirlenen bütçeler de ABD doları olarak gösterilir.

Müşteriye fatura düzenlemek için kullanılan para birimlerinden bağımsız olarak iş ortakları, Ödeme hesabı kapsamı ile müşteriler, abonelikler, kaynaklar ve kaynak grupları için gerçekleştirdikleri bütçe belirleme ve maliyet yönetimi işlemlerinde ABD dolarını kullanır.

İş ortakları ayrıca maliyet analizi görünümünde müşteriler için belirli bir para birimindeki maliyetleri filtreleyebilir. Desteklenen müşteri faturalandırma para birimlerinin kullanılabileceği maliyetleri görüntülemek için **Gerçek maliyet** listesini seçin.

![Para birimleri için Gerçek maliyet seçimini gösteren örnek](./media/get-started-partners/actual-cost-selector.png)

Bir rezervasyon dönemindeki ayrılmış örnek amorti edilmiş maliyetlerini görüntülemek için faturalama kapsamlarında [amorti edilmiş maliyet görünümünü](quick-acm-cost-analysis.md#customize-cost-views) kullanın.

### <a name="billing-profile-scope"></a>Faturalama profili kapsamı

Bir faturaya dahil edilen tüm ürünler ve abonelikler için tüm müşterilerinizin faturalama para biriminde vergi öncesi maliyetleri görüntülemek üzere faturalama profili kapsamını kullanın. **InvoiceID** filtresini kullanarak faturalama profilindeki maliyetleri belirli bir faturaya göre filtreleyebilirsiniz. Filtre, belirli bir faturaya ait olan tüketim ve ürün satın alma maliyetlerini gösterir. Ayrıca faturada belirli bir müşteriye ait olan maliyetleri filtreleyerek vergi öncesi maliyetleri görüntüleyebilirsiniz.

Müşterileri Microsoft Müşteri Sözleşmesi'ne ekledikten sonra Microsoft Müşteri Sözleşmesi'ne eklenen müşteriler için tüm ürünlere ait ücretleri (tüketim, satın alma ve yetkilendirme) içeren bir fatura alırsınız. Aynı para biriminde düzenlenen bu faturalar yetkilendirmenin yanı sıra SaaS, Azure Market ve CSP teklifinde olan müşteriler için rezervasyonlar gibi satın alınan ürünlere ait ücretleri de içerir.

Faturalama profili kapsamı, ücretlerin müşteri faturasıyla mutabık kılınmasına yardımcı olmak için müşterilerinize fatura düzenlenmesine neden olan tüm maliyetleri görmenizi sağlar. Faturada olduğu gibi kapsam da yeni Microsoft Müşteri Sözleşmesi'ndeki tüm müşteriler için geçerlidir. Kapsam ayrıca geçerli CSP teklifinde bulunan müşteri yetkilendirme ürünleri için tahsil edilen ücretleri de gösterir.

Faturalama profili ve ödeme hesabı kapsamları, yetkilendirmenin yanı sıra Azure Market ve rezervasyon satın alma işlemleri gibi satın alma tabanlı ürünleri gösteren katmanlardır.

Faturalama profilleri, faturaya dahil edilen abonelikleri tanımlar. Faturalama profilleri, işlev açısından kurumsal anlaşma kaydına eşdeğerdir. Faturalama profili, faturaların oluşturulduğu kapsamdır.

Şu anda müşterinin faturalandırma para birimi, faturalama profili kapsamındaki maliyetler görüntülenirken varsayılan olarak kullanılmaktadır. Faturalama profili kapsamında belirlenen bütçeler, faturalandırma para birimini kullanır.

İş ortakları, faturaları mutabık kılmak için kapsamı kullanabilir. Ayrıca kapsamı kullanarak şu öğelerin faturalandırma para biriminde bütçe belirleyebilir:

- Belirli filtrelerin uygulandığı faturalar
- Müşteri
- Abonelik
- Kaynak grubu
- Kaynak
- Azure hizmeti
- Ölçüm
- ResellerMPNID

### <a name="customer-scope"></a>Müşteri kapsamı

İş ortakları, Microsoft Müşteri Sözleşmesi'ne eklenmiş olan müşterilerle ilgili maliyetleri yönetmek için bu kapsamı kullanır. Kapsam, iş ortaklarının belirli bir müşteriye ait vergi öncesi maliyetleri bir faturalandırma para biriminde görüntülemesini sağlar. Ayrıca belirli bir abonelik, kaynak grubu veya kaynak için geçerli vergi öncesi maliyetleri filtreleyebilirsiniz.

Müşteri kapsamı, geçerli CSP teklifindeki müşterileri kapsamaz. Kapsam yalnızca Microsoft Müşteri Sözleşmesi'ne sahip olan müşterileri kapsar. Geçerli CSP teklifi müşterileri için yetkilendirme maliyetleri (Azure kullanımı hariç), müşteri filtresini uyguladığınızda ödeme hesabı ve faturalama profili kapsamlarında kullanılabilir. Bu kapsamda belirlenen bütçeler, faturalandırma para birimini kullanır.

## <a name="partner-access-to-billing-scopes-in-cost-management"></a>Maliyet Yönetimi'ndeki faturalama kapsamlarına iş ortağı erişimi

Doğrudan iş ortağının Azure kiracısında bulunan ödeme hesapları, faturalama profilleri ve müşteriler ile ilgili maliyetleri yönetme ve görüntüleme işlemleri yalnızca **Genel yönetici** ve **Yönetici aracısı** rolleri tarafından gerçekleştirilebilir. İş ortağı merkezi rolleri hakkında daha fazla bilgi için bkz. [Kullanıcı rollerini ve izinlerini atama](/partner-center/permissions-overview).

## <a name="enable-cost-management-for-customer-tenant-subscriptions"></a>Müşteri kiracı abonelikleri için maliyet yönetimini etkinleştirme

İş ortakları, Microsoft Müşteri Sözleşmesi'ne ekledikleri müşteriler için Maliyet Yönetimi'ni etkinleştirebilir. İş ortakları daha sonra müşterilerin, kullandıkları Azure hizmetlerinin maliyetlerinin kullandıkça öde perakende ücretleri üzerinden hesaplanmış halini görüntülemesine izin verecek bir ilkeyi etkinleştirebilir. Maliyetler, RBAC aboneliği ve kaynak grubu kapsamlarındaki kullanımları için müşterilerin faturalandırma para biriminde gösterilir.

İş ortağı, maliyet görünürlüğü ilkesini etkinleştirdikten sonra abonelikte Azure Resource Manager erişimine sahip olan tüm kullanıcılar, kullandıkça öde fiyatları üzerinden maliyetleri yönetebilir ve analiz edebilir. Benzer şekilde Azure aboneliklerinde uygun RBAC erişimine sahip olan kurumsal bayiler ve müşteriler de maliyetleri görüntüleyebilir.

Sağlayıcının genel yöneticileri ve yönetici temsilcileri, aboneliğe ve kaynak grubuna erişimleri varsa ilke fark etmeksizin abonelik maliyetlerini görüntüleyebilir.

### <a name="enable-the-policy-to-view-azure-usage-charges"></a>Azure kullanımı ücretlerini görüntülemek için ilkeyi etkinleştirme

İlkeyi görüntülemek ve güncelleştirmek için **yönetici temsilcisi** grubunun üyesi olmalısınız. Müşterilerin Azure kullanım ücretlerini görüntülemelerine olanak tanıyan ilkeyi etkinleştirmek için aşağıdaki bilgileri kullanın.

Azure portalında iş ortağı kiracısında oturum açıp **Maliyet Yönetimi + Faturalandırma**'yı seçin. Faturalama Kapsamı alanında uygun faturalama kapsamını seçin ve sonra **Müşteriler**'i seçin. Müşteri listesi, ödeme hesabıyla ilişkilendirilir.

Müşteri listesinde maliyetleri görüntülemesine izin vermek istediğiniz müşteriyi seçin.

[![Maliyet Yönetimi’nde müşterileri seçme](./media/get-started-partners/customer-list.png)](./media/get-started-partners/customer-list.png#lightbox)

**Ayarlar**'ın altında **İlkeler**'i seçin.

Seçilen müşteriye ait aboneliklerle ilgili **Azure Kullanımı** ücretleri için geçerli olan maliyet görünürlüğü ilkesi gösterilir.
![Müşterilerin kullandıkça öde ücretlerini görmesine izin vermek için kullanılan ilke](./media/get-started-partners/cost-management-billing-policies.png)

İlke **Hayır** olarak ayarlandığında müşteriyle ilişkilendirilmiş olan abonelik kullanıcıları, Azure Maliyet Yönetimi'ni kullanamaz. Bir iş ortağı tarafından etkinleştirilmediği sürece maliyet görünürlüğü ilkesi varsayılan olarak tüm abonelik kullanıcıları için devre dışı bırakılmıştır.

Maliyet ilkesi **Evet** olarak ayarlandığında müşteri kiracısıyla ilişkilendirilmiş olan abonelik kullanıcıları, kullandıkça öde fiyatları üzerinden kullanım maliyetlerini görebilir.

Maliyet görünürlüğü ilkesi etkinleştirildiğinde abonelik kullanımına sahip olan tüm hizmetler, kullandıkça öde fiyatları üzerinden maliyetleri görüntüler. Rezervasyon kullanımı, gerçek ve amorti edilen maliyetler için sıfır ücretle gösterilir. Satın alma ve yetkilendirme işlemleri belirli bir abonelikle ilişkilendirilmez. Bu nedenle satın alma işlemleri, abonelik kapsamında görüntülenmez. Doğrudan iş ortağının veya dolaylı sağlayıcının genel yöneticisi/yönetici aracısı her müşterinin büyük ölçekte maliyet görünürlüğü ilkesini ayarlamak için [Müşteri Güncelleştirme API'sini](/rest/api/billing/2019-10-01-preview/policies/updatecustomer) de kullanabilir.

### <a name="view-subscription-costs-in-the-customer-tenant"></a>Müşteri kiracısında abonelik maliyetlerini görüntüleme

Bir aboneliğin maliyetlerini görüntülemek için müşterinin Azure kiracısında **Maliyet Yönetimi + Faturalandırma** sayfasını açın. Maliyetleri gözden geçirmeye başlamak için gerekli aboneliğin **Maliyet analizini** seçin. Müşteri kiracısında her aboneliğin tüketim maliyetlerini ayrı ayrı görüntüleyebilirsiniz.

[![Maliyet analizini müşteri olarak görüntüleme ](./media/get-started-partners/subscription-costs.png)](./media/get-started-partners/subscription-costs.png#lightbox)

Abonelik ve kaynak grubu RBAC kapsamları için maliyet analizi, bütçeler ve uyarılar, kullandıkça öde fiyatını temel alan maliyetler üzerinden kullanılabilir.

RBAC kapsamlarındaki ayrılmış örneklere ait amorti edilmiş görünümler ve gerçek maliyetler sıfır olarak görünür. Ayrılmış örnek maliyetleri yalnızca satın alma işleminin gerçekleştirildiği faturalama kapsamlarında gösterilir.

Görünümde gösterilen maliyetleri hesaplamak için kullanılan perakende fiyatları, tüm müşteriler için Azure Fiyatlandırma Hesaplayıcısı’nda gösterilen fiyatlarla aynıdır. Gösterilen fiyatlar, iş ortağının sahip olabileceği İş Ortağı Tarafından Kazanılmış Krediler, Katman İndirimleri, Genel Hizmet indirimleri gibi indirimleri veya kredileri içermez.

## <a name="analyze-costs-in-cost-analysis"></a>Maliyet analizindeki maliyetleri analiz etme

İş ortağı kiracısındaki faturalama kapsamlarına erişim sahibi olan iş ortakları farklı müşterilere, tek bir müşteriye veya bir faturaya ait olan faturalandırılmış maliyetleri maliyet analizinde keşfedebilir ve analiz edebilir. [Maliyet analizi](quick-acm-cost-analysis.md) görünümünde ayrıca [görünümleri kaydedebilir](quick-acm-cost-analysis.md#saving-and-sharing-customized-views) ve verileri [CSV ve PNG dosyaları](quick-acm-cost-analysis.md#download-usage-data) olarak dışarı aktarabilirsiniz.

Müşteri kiracısındaki aboneliğe erişim sahibi olan RBAC kullanıcıları da müşteri kiracısındaki abonelikler için perakende maliyetlerini analiz edebilir, görünümleri kaydedebilir ve verileri CSV ve PNG dosyaları olarak dışarı aktarabilir.

Maliyet analizindeki filtreleme ve gruplama özelliklerini kullanarak maliyetleri birden fazla alana göre analiz edebilirsiniz. Bir sonraki bölümde iş ortağına özgü alanlar gösterilmiştir.

## <a name="data-fields"></a>Veri alanları

Aşağıdaki veri alanları, kullanım ayrıntıları dosyalarında ve Maliyet Yönetimi API'lerinde mevcuttur. Mevcut olduğunda İş Ortağı Merkezi ile eşdeğer bilgiler gösterilmiştir. Aşağıda koyu renkle yazılmış olan alanlarda iş ortakları maliyet analizindeki filtreleme ve gruplama özelliklerini kullanarak maliyetleri birden fazla alana göre analiz edebilir. Koyu renkle yazılmış olan alanlar yalnızca iş ortakları tarafından desteklenen Microsoft Müşteri Sözleşmeleri için geçerlidir.

| **Alan adı** | **Açıklama** | **İş Ortağı Merkezi eşdeğeri** |
| --- | --- | --- |
| invoiceId | Belirli bir işleme ait faturada gösterilen fatura kimliği. | İşlemin gösterildiği fatura numarası. |
| previousInvoiceID | Para iadesi (eksi maliyet) varsa özgün faturanın başvurusu. Yalnızca para iadesi olduğunda doldurulur. | Yok |
| billingAccountName | İş ortağını temsil eden ödeme hesabının adı. Microsoft Müşteri Sözleşmesi'ne eklenmiş olan müşteriler ve SaaS, Azure Market ve rezervasyonlar gibi yetkilendirme satın alma işlemleri gerçekleştirmiş olan CSP müşterileri için geçerli tüm maliyetler tahakkuk eder. | Yok |
| billingAccountID | İş ortağını temsil eden ödeme hesabının tanımlayıcısı. | MCAPI İş Ortağı Ticaret Kök Kimliği. İstekte kullanılır ancak yanıta dahil edilmez.|
| billingProfileID | Microsoft Müşteri Sözleşmesi'ne eklenmiş olan müşteriler ve SaaS, Azure Market ve rezervasyonlar gibi yetkilendirme satın alma işlemleri gerçekleştirmiş olan CSP müşterileri için geçerli tüm maliyetler için farklı faturalardaki maliyetleri tek bir faturalama para biriminde gruplayan faturalama profilinin tanımlayıcısı. | MCAPI İş Ortağı Faturalama Grubu Kimliği. İstekte kullanılır ancak yanıta dahil edilmez. |
| billingProfileName | Microsoft Müşteri Sözleşmesi'ne eklenmiş olan müşteriler ve SaaS, Azure Market ve rezervasyonlar gibi yetkilendirme satın alma işlemleri gerçekleştirmiş olan CSP müşterileri için geçerli tüm maliyetler için farklı faturalardaki maliyetleri tek bir faturalama para biriminde gruplayan faturalama profilinin adı. | Yok |
| invoiceSectionName | Faturada ücretlendirilen projenin adı. İş ortakları tarafından eklenen Microsoft Müşteri Sözleşmeleri için geçerli değildir. | Yok |
| invoiceSectionID | Faturada ücretlendirilen projenin tanımlayıcısı. İş ortakları tarafından eklenen Microsoft Müşteri Sözleşmeleri için geçerli değildir. | Yok |
| **CustomerTenantID** | Müşterinin aboneliğinin Azure Active Directory kiracısının tanımlayıcısı. | Müşterinin kuruluş kimliği - müşterinin Azure Active Directory kiracı kimliği. |
| **CustomerName** | Müşterinin aboneliğinin Azure Active Directory kiracısının adı. | İş Ortağı Merkezi'nde gösterildiği şekliyle müşterinin kuruluş adı. Faturanın sistem bilgilerinizle mutabık kılınması açısından önemlidir. |
| **CustomerTenantDomainName** | Müşterinin aboneliğinin Azure Active Directory kiracısının etki alanı adı. | Müşterinin Azure Active Directory kiracısı etki alanı. |
| **PartnerTenantID** | İş ortağının Azure Active Directory kiracısının tanımlayıcısı. | İş ortağı Azure Active Directory Kiracı Kimliği (İş Ortağı Kimliği olarak da anılır), GUID biçiminde. |
| **PartnerName** | İş ortağının Azure Active Directory kiracısının adı. | İş ortağının adı. |
| **ResellerMPNID** | Abonelikle ilişkili kurumsal bayinin MPNID bilgisi. | Abonelikle ilişkili kayıtlı kurumsal bayinin MPN ID bilgisi. Geçerli etkinlik için mevcut değildir. |
| costCenter | Abonelikle ilişkili maliyet merkezi. | Yok |
| billingPeriodStartDate | Faturada gösterilen fatura dönemi başlangıç tarihi. | Yok |
| billingPeriodEndDate | Faturada gösterilen fatura dönemi bitiş tarihi. | Yok |
| servicePeriodStartDate | Hizmet kullanımından ücret tahsilatı yapıldığı ücretlendirme döneminin başlangıç tarihi. Azure hizmetlerine ait fiyatlar, ücretlendirme dönemine göre belirlenir. | İş Ortağı Merkezi'nde ChargeStartDate. Bir önceki faturalama döneminde tahsil edilmemiş görünmeyen kullanım verilerinin tarihlerinin belirtildiği durumlar dışında faturalama dönemi başlangıç tarihi. Saat her zaman günün başlangıcıdır (00:00). |
| servicePeriodEndDate | Hizmet kullanımından ücret tahsilatı yapıldığı dönemin bitiş tarihi. Azure hizmetlerine ait fiyatlar, ücretlendirme dönemine göre belirlenir. | Yok |
| date | Azure tüketim verileri için kullanımın ücretlendirildiği tarihi gösterir. Ayrılmış örnek için satın alma tarihini gösterir. Market ve destek gibi yinelenen ve tek seferlik ücretler için satın alma tarihini gösterir. | Yok |
| productID | Tüketim veya satın alma sonucunda ücret tahakkuk etmiş olan ürünün tanımlayıcısı. Bu, İş Ortağı Merkezi'nde gösterilen productID ve SKuID değerlerinin kısaltmasıdır. | Ürünün kimliği. |
| ürün | Faturada gösterildiği şekilde tüketim veya satın alma sonucunda ücret tahakkuk etmiş olan ürünün adı. | Ürünün katalogdaki adı. |
| serviceFamily | Satın alınan veya ücretlendirilen ürünün hizmet ailesini gösterir. Örneğin, Depolama veya Hesaplama. | Yok |
| productOrderID | Aboneliğin ait olduğu varlığın tanımlayıcısı veya Azure planının adı. Örneğin, Azure Planı. | CommerceSubscriptionID |
| productOrderName | Aboneliğin ait olduğu Azure planının adı. Örneğin, Azure Planı. | Yok|
| consumedService | Eski EA kullanım ayrıntılarında kullanılan tüketilen hizmet (eski sınıflandırma). | İş Ortağı Merkezi'nde gösterilen hizmet. Örneğin Microsoft.Storage, Microsoft.Compute ve microsoft.operationalinsights. |
| meterID | Ölçülen tüketim için ölçülen tanımlayıcı. | Kullanılan ölçümün tanımlayıcısı. |
| meterName | Ölçülen tüketim için kullanılan ölçünün adını tanımlar. | Kullanılan ölçümün adı. |
| meterCategory | Kullanım için en üst düzey hizmeti belirtir. | Kullanım için en üst düzey hizmet. |
| meterSubCategory | Fiyatı etkileyebilecek Azure hizmet türünü veya alt kategorisini tanımlar. | Fiyatı etkileyebilecek Azure hizmet türü.|
| meterRegion | Veri merkezi konumuna bağlı olarak ücretlendirilen belirli hizmetler için veri merkezinin konumunu belirtir. | Geçerli ve doldurulduğu yerlerde hizmetlere ait veri merkezinin bölgesel konumu. |
| abonelik kimliği | Azure aboneliği için Microsoft tarafından oluşturulan benzersiz tanımlayıcı. | EntitlementID |
| subscriptionName | Azure aboneliğinin adı. | Yok |
| Süre | Teklifin geçerlilik süresini görüntüler. Örneğin ayrılmış örneklerde, bir yıl için 12 ay görüntülenir. Tek seferlik veya yinelenen satın alma işlemlerinde SaaS, Azure Market ve destek için tek bir ay gösterilir. Azure tüketimi için geçerli değildir. | Yok |
| publisherType (firstParty, thirdPartyReseller, thirdPartyAgency) | Yayımcıyı birinci taraf, üçüncü taraf satıcı veya üçüncü taraf aracı olarak tanımlayan yayımcı türü. | Yok |
| partNumber | Kullanılmayan ayrılmış örnekler ve Azure Market hizmetleri için parça numarası. | Yok |
| publisherName | Microsoft veya üçüncü taraf yayımcılar dahil olmak üzere hizmet yayımcısının adı. | Ürünün yayımcısının adı.|
| reservationId | Ayrılmış örnek satın alma işlemine ait tanımlayıcı. | Yok |
| reservationName | Ayrılmış örneğin adı. | Yok |
| reservationOrderId | Ayrılmış örneğin OrderID değeri. | Yok |
| frequency | Ayrılmış örnek için ödeme sıklığı. | Yok |
| resourceGroup | Yaşam döngüsü kaynak yönetimi için kullanılan Azure kaynak grubunun adı. | Kaynak grubunun adı. |
| instanceID (veya) ResourceID | Kaynak örneğinin tanımlayıcısı. | Kaynak özelliklerinin tamamını içeren bir ResourceURI olarak gösterilir. |
| resourceLocation | Kaynak konumunun adı. | Kaynağın konumu. |
| Konum | Kaynağın normalleştirilmiş konumu. | Yok |
| effectivePrice | Fiyatlandırma para birimi cinsinden hizmetin geçerli birim fiyatı. Ürün, hizmet ailesi, ölçüm ve teklif için benzersizdir. Ödeme hesabı için fiyat listesinde fiyatlandırma ile birlikte kullanılır. Katmanlı fiyatlandırma veya dahil edilen miktar olduğunda tüketim için karıştırılmış fiyatı gösterir. | Ayarlamalar yapıldıktan sonra birim fiyatı. |
| Miktar | Satın alınan veya tüketilen ölçülen miktar. Faturalama dönemi boyunca kullanılan ölçüm miktarı. | Birim sayısı. Mutabakat sırasında faturalandırma sisteminizdeki bilgilerle eşleştiğinden emin olun. |
| unitOfMeasure | Hizmetin ücretlendirildiği birimi tanımlar. Örneğin, GB ve saat. | Hizmetin ücretlendirildiği birimi tanımlar. Örneğin GB, saat, 10.000'lik bloklar. |
| PricingCurrency | Birim fiyatı tanımlayan para birimi. | Fiyat listesindeki para birimi.|
| billingCurrency | Faturalandırılan maliyeti tanımlayan para birimi. | Müşterinin coğrafi bölgesinin para birimi. |
| chargeType | Satın alma ve para iadesi gibi maliyetin Azure Maliyet Yönetimi'nde temsil ettiği ücret türünü tanımlar. | Ücretin veya düzeltmenin türü. Geçerli etkinlik için mevcut değildir. |
| costinBillingCurrency | Faturalandırılan para biriminde vergilerden önce ExtendedCost veya karıştırılmış maliyet. | Yok |
| costinPricingCurrency | Fiyatlarla ilişkilendirmek için fiyatlandırma para biriminde vergilerden önce ExtendedCost veya karıştırılmış maliyet. | Yok |
| **costinUSD** | ABD doları cinsinden vergilerden önce tahmini ExtendedCost veya karıştırılmış maliyet. | Yok |
| **paygCostInBillingCurrency** | Fiyatlandırmanın perakende fiyatları üzerinden yapılması durumunda maliyetleri gösterir. Faturalandırma para biriminde kullandıkça öde fiyatlarını gösterir. Yalnızca RBAC kapsamlarında kullanılabilir. | Yok |
| **paygCostInUSD** | Fiyatlandırmanın perakende fiyatları üzerinden yapılması durumunda maliyetleri gösterir. ABD doları cinsinden kullandıkça öde fiyatlarını gösterir. Yalnızca RBAC kapsamlarında kullanılabilir. | Yok |
| exchangeRate | Fiyatlandırma para birimini faturalama para birimine dönüştürürken kullanılan döviz kuru. | İş Ortağı Merkezi'nde PCToBCExchangeRate olarak adlandırılır. Fiyatlandırma para birimi ile faturalandırma para birimi döviz kuru.|
| exchangeRateDate | Fiyatlandırma para birimini faturalama para birimine dönüştürürken kullanılan döviz kurunun tarihi. | İş Ortağı Merkezi'nde PCToBCExchangeRateDat olarak adlandırılır. Fiyatlandırma para birimi ile faturalandırma para birimi döviz kuru tarihi.|
| isAzureCreditEligible | Maliyetin Azure kredisi ile ödemeye uygun olup olmadığını gösterir. | Yok |
| serviceInfo1 | Hizmete özgü isteğe bağlı meta verileri yakalayan eski alan. | Azure hizmeti iç meta verileri. |
| serviceInfo2 | Hizmete özgü isteğe bağlı meta verileri yakalayan eski alan. | Hizmet bilgileri. Örneğin, sanal makinenin görüntü türü ve ExpressRoute ISS adı.|
| additionalInfo | Hizmete özgü meta veriler. Örneğin, sanal makinenin görüntü türü. | Diğer sütunlarda yer almayan ek bilgiler. Hizmete özgü meta veriler. Örneğin, sanal makinenin görüntü türü.|
| etiketler | Ölçüme atadığınız etiketler. Etiketleri kullanarak fatura kayıtlarını gruplayabilirsiniz. Örneğin, ölçümü kullanan departmana göre maliyetleri dağıtmak için etiketleri kullanabilirsiniz. | Müşteri tarafından eklenen etiketler.|
| **partnerEarnedCreditRate** | İş ortağı yöneticisi bağlantı erişimine göre iş ortağı tarafından kazanılmış kredi (PEC) olması halinde uygulanan indirimin oranı. | İş ortağı tarafından kazanılmış kredinin (PEC) oranı. Örneğin %0 veya %15. |
| **partnerEarnedCreditApplied** | İş ortağı tarafından kazanılmış kredinin uygulanıp uygulanmadığını belirtir. | Yok |

## <a name="view-partner-earned-credit-pec-resource-costs"></a>İş Ortağı Tarafından Kazanılmış Kredi (PEC) kaynak maliyetlerini görüntüleme

İş ortakları, Azure Maliyet Yönetimi'nde PEC avantajına sahip olan maliyetleri görüntülemek için maliyet analizini kullanabilir.

Azure portalında iş ortağı kiracısında oturum açıp **Maliyet Yönetimi + Faturalandırma**'yı seçin. **Maliyet Yönetimi** bölümünde **Maliyet analizi**'ni seçin.

Maliyet analizi görünümü, iş ortağına ait ödeme hesabının maliyetlerini gösterir. Faturaları mutabık kılmak için **Kapsamı** iş ortağı, belirli bir müşteri veya faturalama profili olarak ayarlayın.

PEC maliyetlerinin detayına inmek için halka grafikte açılan listeyi ve ardından **PartnerEarnedCreditApplied** öğesini seçin.

![İş ortağı tarafından kazanılmış kredinin nasıl görüntüleneceğini gösteren örnek](./media/get-started-partners/cost-analysis-pec1.png)

**PartnerEarnedCreditApplied** özelliği _True_ olduğunda ilgili maliyet, iş ortağı tarafından elde edilen yönetim erişimi avantajına sahip olur.

**PartnerEarnedCreditApplied** özelliği _False_ olduğunda ilgili maliyet, kredi için gerekli uygunluğu sağlamamış olur. Satın alınan hizmet de iş ortağı tarafından kazanılmış kredi için uygun olmayabilir.

Hizmet kullanım verilerinin Maliyet Yönetimi'ne yansıtılması normalde 8-24 saat arası sürer. Daha fazla bilgi için bkz. [Maliyet ile kullanım verilerinin güncelleştirilmesi ve saklama](understand-cost-mgt-data.md#cost-and-usage-data-updates-and-retention). PEC kredileri, Azure Maliyet Yönetimi erişiminden itibaren 48 saat içinde görünür.


İsterseniz **PartnerEarnedCreditApplied** özelliğini **Gruplandırma ölçütü** seçeneklerini kullanarak gruplandırabilir ve filtreleyebilirsiniz. Bu seçenekleri kullanarak PEC içeren ve içermeyen maliyetleri inceleyebilirsiniz.

![İş ortağı tarafından kazanılmış krediye göre gruplandırma veya filtreleme](./media/get-started-partners/cost-analysis-pec2.png)

## <a name="export-cost-data-to-azure-storage"></a>Maliyet verilerini Azure Depolama'ya aktarma

Bir iş ortağı kiracısındaki faturalama kapsamlarına erişim sahibi olan iş ortakları, maliyet ve kullanım verilerini bir Azure Depolama blobuna aktarabilir. Blob, iş ortağı kiracısında bulunan bir abonelik olmalıdır ve paylaşılan hizmet aboneliği veya müşteri aboneliği olmamalıdır. Maliyet verilerinin dışarı aktarılmasını etkinleştirmek için dışarı aktarılan maliyet verilerini barındırmak üzere iş ortağı kiracısında bağımsız bir kullandıkça öde aboneliği oluşturmanız önerilir. Dışarı aktarma depolama hesabı, kullandıkça öde aboneliğinde barındırılan Azure Depolama blobunda oluşturulur. İş ortağının dışarı aktarma işini gerçekleştirdiği kapsama bağlı olarak ilgili veriler otomatik olarak ve yinelenen bir şekilde depolama hesabına aktarılır.

Abonelikte RBAC erişimine sahip olan kullanıcılar da maliyet verilerini müşteri kiracısındaki herhangi bir abonelikte barındırılan Azure Depolama blobuna aktarabilir.

### <a name="create-an-export-in-a-partner-tenant-or-customer-tenant"></a>İş ortağı kiracısında veya müşteri kiracısında dışarı aktarma işi oluşturma

Azure portalında iş ortağı kiracısında veya müşteri kiracısında oturum açıp **Maliyet Yönetimi + Faturalandırma**'yı seçin. Uygun bir kapsam seçin (Microsoft İş Ortağı Sözleşmesi ödeme hesabı gibi) ve **Maliyet Analizi**'ni seçin. Sayfa yüklendiğinde **Dışarı aktar**'ı seçin. Dışarı Aktarma Zamanla'nın altında **Tüm dışarı aktarımları görüntüle**'yi seçin.

![Dışarı aktar'ı ve Tüm dışarı aktarımları görüntüle'yi seçin](./media/get-started-partners/export01.png)

Daha sonra **Ekle**'yi seçip bir ad girin ve dışarı aktarma türünü seçin. **Depolama** sekmesini seçin ve gerekli bilgileri girin.

![Yeni dışarı aktarma ekleyip Depolama sekmesini seçin](./media/get-started-partners/export02.png)

İş ortağı kiracısında bir dışarı aktarma işi oluşturduğunuzda, iş ortağı kiracısındaki kullandıkça öde aboneliğini seçin. Bu aboneliği kullanarak bir Azure Depolama hesabı oluşturun.

Müşteri kiracısındaki RBAC kullanıcıları için müşteri kiracısındaki bir aboneliği seçin. Aboneliği kullanarak bir Azure Depolama hesabı oluşturun.

İçeriği gözden geçirin ve **Oluştur**'u seçerek dışarı aktarma işini zamanlayın.

Dışarı aktarma listesindeki verileri doğrulamak için depolama hesabı adını seçin. Depolama hesabı sayfasında **Kapsayıcılar**'ı ve ardından kapsayıcıyı seçin. Karşılık gelen klasöre gidin ve CSV dosyasını seçin. **İndir**'i seçip CSV dosyasını indirin ve ardından açın. Dışarı aktarılan veriler, maliyet verilerini Azure portalındaki kullanım ayrıntılarına benzer şekilde gösterir.

![Dışarı aktarılan verilerin örneği](./media/get-started-partners/example-export-data.png)

## <a name="cost-management-rest-apis"></a>Maliyet Yönetimi REST API’leri

İş ortakları ve müşteriler, aşağıdaki bölümlerde anlatılan Maliyet Yönetimi API'lerini kullanarak yaygın görevleri gerçekleştirebilirler.

### <a name="azure-cost-management-apis---direct-and-indirect-providers"></a>Azure Maliyet Yönetimi API'leri - Doğrudan ve dolaylı sağlayıcılar

Bir iş ortağı kiracısındaki faturalama kapsamlarına erişim sahibi olan iş ortakları, aşağıdaki API'leri kullanarak faturalandırılan maliyetleri görüntüleyebilir.

Abonelik kapsamındaki API'ler, abonelikte erişim sahibi olan iş ortakları tarafından maliyet ilkesinden bağımsız olarak çağrılabilir. Müşteri veya kurumsal bayi gibi aboneliğe erişimi olan diğer kullanıcılar, API'leri ancak iş ortağının müşteri kiracısında maliyet ilkesini etkinleştirmesi durumunda çağırabilir.


#### <a name="to-get-a-list-of-billing-accounts"></a>Faturalama dönemlerinin listesini almak için

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-customers"></a>Müşterilerin listesini almak için

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-subscriptions"></a>Aboneliklerin listesini almak için

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/billingSubscriptions?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-invoices-for-a-period-of-time"></a>Belirli bir süreye ait faturaların listesini almak için

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/invoices?api-version=2019-10-01-preview&periodStartDate={periodStartDate}&periodEndDate={periodEndDate}
```

API çağrısı, aşağıdaki JSON koduna benzer öğelere sahip bir fatura dizisi döndürür.

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

Yukarıdaki döndürülen kimlik alanı değerini kullanın ve kullanım ayrıntılarını sorgulamak için aşağıdaki örnekte kapsamla değiştirin.

```
GET https://management.azure.com/{id}/providers/Microsoft.Consumption/UsageDetails?api-version=2019-10-01
```

Örnek, belirli bir faturayla ilişkilendirilmiş olan kullanım raporlarını döndürür.


#### <a name="to-get-the-policy-for-customers-to-view-costs"></a>Müşterilerin maliyetleri görüntülemesini sağlayacak ilkeyi almak için

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/policies/default?api-version=2019-10-01-preview
```

#### <a name="to-set-the-policy-for-customers-to-view-costs"></a>Müşterilerin maliyetleri görüntülemesini sağlayacak ilkeyi ayarlamak için

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/policies/default?api-version=2019-10-01-preview
```

#### <a name="to-get-azure-service-usage-for-a-billing-account"></a>Bir ödeme hesabına ait Azure hizmeti kullanımını almak için

```
GET https://management.azure.com/providers/Microsoft.Billing/BillingAccounts/{billingAccountName}/providers/Microsoft.Consumption/usageDetails?api-version=2019-10-01
```

#### <a name="to-download-a-customers-azure-service-usage"></a>Bir müşterinin Azure hizmeti kullanımını indirmek için

Aşağıdaki GET çağrısı zaman uyumsuz bir işlemdir.

```
GET https://management.azure.com/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/providers/Microsoft.Consumption/usageDetails/download?api-version=2019-10-01 -verbose
```

İşlemin durumunu denetlemek için yanıtta döndürülen `Location` URI'sini çağırın. Durum *Tamamlandı* olduğunda `downloadUrl` özelliği, oluşturulan raporu indirmek için kullanabileceğiniz bir bağlantı içerir.


#### <a name="to-get-or-download-the-price-sheet-for-consumed-azure-services"></a>Tüketilen Azure hizmetlerine ait fiyat listesini almak veya indirmek için

Öncelikle aşağıdaki POST çağrısını kullanın.

```
POST https://management.azure.com/providers/Microsoft.Billing/BillingAccounts/{billingAccountName}/billingProfiles/{billingProfileID}/pricesheet/default/download?api-version=2019-10-01-preview&format=csv" -verbose
```

Ardından zaman uyumsuz işlem özellik değerini çağırın. Örneğin:

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/billingProfiles/{billingProfileID}/pricesheetDownloadOperations/{operation}?sessiontoken=0:11186&api-version=2019-10-01-preview
```
Yukarıdaki GET çağrısı, fiyat listesini içeren indirme bağlantısını döndürür.


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

#### <a name="delete-a-budget"></a>Bütçeyi silme

```
DELETE
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.CostManagement/budgets/{budgetName}?api-version=2019-10-01
```


## <a name="next-steps"></a>Sonraki adımlar
- Maliyet Yönetimi'nde [maliyetleri analiz etmeye başlayın](quick-acm-cost-analysis.md)
- Maliyet Yönetimi'nde [bütçe oluşturun ve yönetin](tutorial-acm-create-budgets.md)
