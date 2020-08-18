---
title: Güncelleştirilmiş Azure faturalama hesabınızı kullanmaya başlama
description: Yeni faturalama ve maliyet yönetimi deneyimindeki değişiklikleri anlamak için güncelleştirilmiş Azure faturalama hesabınızı kullanmaya başlayın
author: bandersmsft
ms.reviewer: amberbhargava
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 08/03/2020
ms.author: banders
ms.openlocfilehash: 37b50e8761609f864d2cb16e7869614db64bac71
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87844924"
---
# <a name="get-started-with-your-updated-azure-billing-account"></a>Güncelleştirilmiş Azure faturalama hesabınızı kullanmaya başlama

Maliyetleri ve faturaları yönetmek, bulut deneyiminizin temel bileşenlerinden biridir. Buluttaki harcamalarınızı denetlemenize ve anlamanıza yardımcı olur. Microsoft, maliyetlerinizi ve faturalarınızı yönetmenizi kolaylaştırmak için Azure faturalama hesabınızı gelişmiş maliyet yönetimi ve faturalama özellikleriyle güncelleştiriyor. Bu makalede faturalama hesabınıza gelen güncelleştirmeler açıklanır ve yeni özellikler adım adım gösterilir.

> [!IMPORTANT]
> Microsoft’tan hesabınıza yönelik güncelleştirmeler konusunda sizi bilgilendiren bir e-posta aldığınızda hesabınız güncelleştirilecektir. Bildirim, hesabınız güncelleştirilmeden 60 gün önce gönderilir.

## <a name="more-flexibility-with-your-new-billing-account"></a>Yeni faturalama hesabınızda daha fazla esneklik

Aşağıdaki diyagramda eski ve yeni faturalama hesabınız karşılaştırılır:

![Eski ve yeni hesap arasındaki faturalama hiyerarşisinin karşılaştırmasını gösteren diyagram](./media/mosp-new-customer-experience/comparison-old-new-account.png)

Yeni faturalama hesabınız, faturalarınızı ve ödeme yöntemlerinizi yönetmenize olanak sağlayan bir veya daha fazla faturalama profili içerir. Her faturalama profili, faturalama profilinin faturasındaki maliyetleri düzenlemenize olanak sağlayan bir veya daha fazla fatura bölümü içerir.

![Yeni faturalama hiyerarşisini gösteren diyagram](./media/mosp-new-customer-experience/new-billing-account-hierarchy.png)

Ödeme hesabındaki roller en yüksek izin düzeyine sahiptir. Bu roller, kuruluşta faturaları görüntülemesi ve hesabınızın tamamına ilişkin maliyetleri izlemesi gereken finans ve BT yöneticileri gibi kullanıcılara veya hesaba kaydolan bireylere atanmalıdır. Daha fazla bilgi için bkz. [ödeme hesabı rolleri ve görevleri](../manage/understand-mca-roles.md#billing-account-roles-and-tasks). Hesabınız güncelleştirildiğinde, eski faturalama hesabı üzerinde hesap yöneticisi rolüne sahip olan kullanıcıya yeni hesapta sahip rolü verilir.

## <a name="billing-profiles"></a>Faturalama profilleri

Faturalama profili, faturanızı ve ödeme yöntemlerinizi yönetmek için kullanılır. Hesabınızdaki her bir faturalama profili için ay başında aylık bir fatura oluşturulur. Fatura, faturalama profiliyle ilişkilendirilmiş tüm abonelikler için önceki aya ait ilgili ücretleri içerir.

Hesabınız güncelleştirildiğinde her abonelik için otomatik olarak bir faturalama profili oluşturulur. Aboneliğin ücretleri ilgili faturalama profiline faturalandırılır ve faturasında görüntülenir.

Faturalama profillerindeki roller, faturaları ve ödeme yöntemlerini görüntüleme ve yönetme izinlerine sahiptir. Bu roller, kuruluştaki muhasebe ekibinin üyeleri gibi faturaları ödeyen kullanıcılara atanmalıdır. Daha fazla bilgi için bkz. [faturalama profili rolleri ve görevleri](../manage/understand-mca-roles.md#billing-profile-roles-and-tasks). 

Hesabınız güncelleştirildiğinde, başkalarına [faturaları görüntüleme](download-azure-invoice.md#allow-others-to-download-the-your-subscription-invoice) izni verdiğiniz her abonelik için sahip, katkıda bulunan, okuyucu veya fatura okuyucusu Azure RBAC rolüne sahip kullanıcılara ilgili faturalama profilinde okuyucu rolü verilir.

## <a name="invoice-sections"></a>Fatura bölümleri

Fatura bölümü, faturanızdaki maliyetleri düzenlemek için kullanılır. Örneğin, tek bir faturaya ihtiyacınız olabilir ama maliyetleri departmana, ekibe veya projeye göre düzenlemek isteyebilirsiniz. Bu senaryoda, her bir bölüm, ekip veya proje için fatura bölümü oluşturduğunuz tek bir faturalama profiliniz vardır.

Hesabınız güncelleştirildiğinde her faturalama profili için bir fatura bölümü oluşturulur ve fatura bölümüne ilgili abonelik atanır. Daha fazla abonelik ekledikçe ek bölümler oluşturabilir ve abonelikleri fatura bölümlerine atayabilirsiniz. Faturalama profilinizin faturasında, atadığınız her aboneliğin kullanımını yansıtan bölümler görürsünüz.

Fatura bölümündeki roller, kimlerin Azure abonelikleri oluşturduğunu denetleme iznine sahiptir. Roller, kuruluştaki ekipler için Azure ortamını ayarlayan mühendislik liderleri ve teknik mimarlar gibi kullanıcılara atanmalıdır. Daha fazla bilgi için bkz. [fatura bölümü rolleri ve görevleri](../manage/understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="enhanced-features-in-your-new-experience"></a>Yeni deneyiminizdeki iyileştirilmiş özellikler

Yeni deneyiminiz, maliyetlerinizi ve faturalarınızı yönetmenizi kolaylaştıran aşağıdaki maliyet yönetimi ve faturalama özelliklerini içerir:

#### <a name="invoice-management"></a>Fatura yönetimi

**Daha tahmin edilebilir aylık faturalama dönemi**: Yeni hesabınızda faturalama dönemi, Azure'ı kullanmak için ne zaman kaydolduğunuza bakılmaksızın ayın ilk günü başlar ve son günü biter. Her ayın başında bir fatura oluşturulur ve bu fatura önceki aydan gelen tüm ücretleri içerir.

**Birden çok abonelik için tek bir aylık fatura alın**: Aboneliklerinizden her biri için ayrı bir aylık fatura veya birden çok abonelik için tek bir fatura arasında seçim yapma esnekliğiniz vardır.

**Azure abonelikleri, destek planları ve Azure Market ürünleri için tek bir aylık fatura alın**: Azure aboneliklerinin kullanım ücretleri, destek planları ve Azure Market alışverişleri dahil tüm ücretler için tek bir aylık fatura alacaksınız.

**Maliyetleri faturanızda ihtiyaçlarınız temelinde gruplandırın**: Maliyetleri faturanızda ihtiyaçlarınız temelinde departmanlara, projelere veya ekiplere göre gruplandırabilirsiniz.

**Faturaya isteğe bağlı bir satın alma siparişi numarası ayarlayın**: Faturanızı iç finans sisteminizle ilişkilendirmek için bir satın alma siparişi numarası ayarlayın. Azure portalda istediğiniz zaman bu numarayı yönetin ve güncelleştirin.

#### <a name="payment-management"></a>Ödeme yönetimi

**Kredi kartı kullanarak faturaları hemen ödeyin**: Ücretin kredi kartınızdan otomatik ödemeyle alınmasını beklemek zorunda değilsiniz. Azure portalda, vadesi gelmiş veya vadesi geçmiş bir faturayı ödemek için herhangi bir kredi kartı kullanabilirsiniz.

**Hesaba uygulanan tüm ödemeleri izleyin**: Azure portalda hesabınıza uygulanan tüm ödemeleri, kullanılan ödeme yöntemi, ödeme tutarı ve ödeme tarihiyle birlikte görüntüleyin.

#### <a name="cost-management"></a>Maliyet yönetimi

**Kullanım verilerinin depolama hesabına aylık aktarımını zamanlayın**: Maliyet ve kullanım verilerinizi günlük, haftalık veya aylık olarak bir depolama hesabında otomatik şekilde yayımlayın.

#### <a name="account-and-subscription-management"></a>Hesap ve abonelik yönetimi

**Faturalama işlemlerini yapmaları için birden çok yönetici atayın**: Hesabınızın faturalamasını yönetmeleri için birden çok kullanıcıya faturalama izinleri atayın. Başkalarına okuma veya yazma izni ya da her iki izni de sağlayarak esneklik elde edin.

**Doğrudan Azure portalda ek abonelikler oluşturun**: Azure portalda tüm aboneliklerinizi tek tıklamayla oluşturun.

#### <a name="api-support"></a>API desteği

**Faturalama ve maliyet yönetimi işlemlerini API'ler, SDK ve PowerShell aracılığıyla gerçekleştirin**: Faturalama ve maliyet verilerini tercih ettiğiniz veri analizi araçlarına çekmek için maliyet yönetimi, faturalama ve tüketim API'lerini kullanın.

**Tüm abonelik işlemlerini API'ler, SDK ve PowerShell üzerinden gerçekleştirin**: Azure aboneliklerinizin yönetimini otomatikleştirmek, örneğin aboneliği oluşturma, yeniden adlandırma ve iptal etme işlemlerini yapmak için Azure aboneliği API'lerini kullanın.

## <a name="get-prepared-for-your-new-experience"></a>Yeni deneyiminize hazır olun

Yeni deneyiminize hazır olmanız için şunları öneririz:

**Aylık faturalama dönemi ve farklı fatura tarihi**

Yeni deneyimde faturanız her ayın yaklaşık dokuzuncu günü oluşturulur ve önceki ayın tüm ücretlerini içerir. Bu tarih, eski hesabınızda faturanızın oluşturulduğu tarihten farklı olabilir. Faturalarınızı başkalarıyla paylaşıyorsanız tarih değişikliğini bu kişilere bildirin.

**Yeni faturalama ve maliyet yönetimi API'leri**

Faturalama ve maliyet verilerinizi sorgulamak ve güncelleştirmek için Maliyet Yönetimi ve Faturalama API'lerini kullanıyorsanız artık yeni API'leri kullanmalısınız. Aşağıdaki tabloda yeni faturalama hesabında kullanılamayacak API'ler ve yeni faturalama hesabınızda yapmanız gereken değişiklikler listelenir.

|API | Değişiklikler  |
|---------|---------|
|[Faturalama Hesapları - Listeleme](https://docs.microsoft.com/rest/api/billing/2019-10-01-preview/billingaccounts/list) | Faturalama Hesapları - Listeleme API'sinde eski faturalama hesabınızın agreementType ayarı **MicrosoftOnlineServiceProgram**'dır, yeni faturalama hesabınızın agreementType ayarı **MicrosoftCustomerAgreement** olur. agreementType üzerinde bir bağımlılığınız varsa lütfen güncelleştirin. |
|[Faturalar - Faturalama Aboneliğine Göre Listeleme](https://docs.microsoft.com/rest/api/billing/2019-10-01-preview/invoices/listbybillingsubscription)     | Bu API yalnızca hesabınız güncelleştirilmeden önce oluşturulmuş olan faturaları döndürür. Yeni faturalama hesabınızda oluşturulan faturaları almak için [Faturalar - Faturalama Hesabına Göre Listeleme](https://docs.microsoft.com/rest/api/billing/2019-10-01-preview/invoices/listbybillingaccount) API'sini kullanmanız gerekebilir. |

## <a name="additional-information"></a>Ek bilgiler

Aşağıdaki bölümlerde yeni deneyiminiz hakkında ek bilgiler sağlanır.

**Hizmet kapalı kalma süresi yoktur** Aboneliğinizdeki Azure hizmetleri kesintisiz olarak çalışmaya devam eder. Tek güncelleştirme, faturalama deneyiminizde yapılan güncelleştirmedir. Mevcut kaynaklar, kaynak grupları veya yönetim grupları üzerinde herhangi bir etki söz konusu değildir.

**Azure kaynaklarında değişiklik yoktur** Azure RBAC (rol tabanlı erişim denetimi) kullanılarak ayarlanan Azure kaynaklarının erişimi güncelleştirmeden etkilenmez.

**Yeni deneyimde geçmiş faturalar kullanılabilir** Hesabınız güncelleştirilmeden önce oluşturulan faturalar Azure portalda kullanılabilir olmaya devam eder.

**Ayın ortasında güncelleştirilen hesabın faturaları** Hesabınız ayın ortasında güncelleştirildiyse hesabınızın güncelleştirildiği güne kadar biriken ücretler için bir fatura alırsınız. Ayın kalan bölümü için de başka bir fatura alırsınız. Örneğin hesabınızın tek bir aboneliği olduğunu ve 15 Eylül'de güncelleştirildiğini düşünün. 15 Eylül'e kadar biriken ücretler için bir fatura alırsınız. 15 Eylül ile 30 Eylül arasındaki dönem için de başka bir fatura alırsınız. Eylül'den sonra her ay için bir fatura alırsınız.

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.

Yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Sonraki adımlar

Faturalama hesabınız hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın.

- [Yeni faturalama hesabınız için yönetim rollerini anlama](../manage/understand-mca-roles.md)
- [Yeni faturalama hesabınız için ek bir Azure aboneliği oluşturma](../manage/create-subscription.md)
- [Maliyetlerinizi düzenlemek için faturanızda bölümler oluşturma](../manage/mca-section-invoice.md)
