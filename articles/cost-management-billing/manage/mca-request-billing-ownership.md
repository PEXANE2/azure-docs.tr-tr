---
title: Azure aboneliklerinin fatura sahipliğini alma
description: Diğer kullanıcılardan Azure aboneliklerinin fatura sahipliğini istemeyi öğrenin.
author: amberbhargava
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 05/01/2020
ms.author: banders
ms.openlocfilehash: 3aaf2a949d5b3e6670e7885adea4579ac2802428
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82692318"
---
# <a name="get-billing-ownership-of-azure-subscriptions-from-other-accounts"></a>Diğer hesaplardan Azure aboneliklerinin fatura sahipliğini alma

Mevcut fatura sahibi kuruluşunuzdan ayrılıyorsa veya abonelikleri kendi ödeme hesabınızdan ödemek istiyorsanız Azure aboneliklerinin sahipliğini üstlenmek isteyebilirsiniz. Sahipliği aldığınızda aboneliklerin faturalama sorumlulukları sizin hesabınıza aktarılır.

Bu makale, Microsoft Müşteri Sözleşmesi’ne ilişkin ödeme hesabı için geçerlidir. [Microsoft Müşteri Sözleşmesi’ne erişiminiz olup olmadığını denetleyin](#check-for-access).

Fatura sahipliği istemek için **fatura bölümü sahibi** veya **fatura bölümü katkıda bulunanı** olmanız gerekir. Daha fazla bilgi için bkz. [Fatura bölümü rolleri görevleri](understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="request-billing-ownership"></a>Fatura sahipliğini isteme

1. Microsoft Müşteri Sözleşmesi’nin ödeme hesabı için fatura bölümü sahibi veya katkıda bulunanı olarak [Azure portalında](https://portal.azure.com) oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Azure portalında maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Faturalama kapsamları sayfasında, aboneliklerin kullanıma ilişkin ücretleri ödemek için kullanılacak ödeme hesabını seçin. Ödeme hesabının **Microsoft Müşteri Sözleşmesi** türünde olması gerekir.

    ![Portalda maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/mca-request-billing-ownership/list-of-scopes.png)

    > [!NOTE]
    >
    > Azure portalı, en son eriştiğiniz faturalama kapsamını anımsar ve Maliyet Yönetimi ve Faturalama sayfasına sonraki gelişinizde bunu görüntüler. Daha önce Maliyet Yönetimi ve Faturalama sayfasını ziyaret ettiyseniz faturalama kapsamı sayfasını görmezsiniz. Bu durumda, [doğru kapsamda](#check-for-access) olup olmadığınızı denetleyin. Aksi takdirde, bir Microsoft Müşteri Sözleşmesi’ne yönelik ödeme hesabını seçmek için [kapsamı değiştirin](view-all-accounts.md#switch-billing-scope-in-the-azure-portal).

4. Soldaki **Faturalama profilleri** seçeneğini belirleyin.

    ![Faturalama profili seçmeyi gösteren ekran görüntüsü](./media/mca-request-billing-ownership/mca-select-profiles.png)     

    > [!Note]
    >
    > Doğru faturalama kapsamında değilseniz Faturalama profilleri göremezsiniz. Microsoft Müşteri Sözleşmesi’nden ödeme hesabı seçip Faturalama profili belirlemeniz gerekir. Kapsam değiştirmeyi öğrenmek için bkz. [Azure portalında faturalama kapsamı değiştirme](view-all-accounts.md#switch-billing-scope-in-the-azure-portal).

5. Listeden bir **Faturalama profili** seçin. Aboneliklerin sahipliğini üstlendiğinizde bu aboneliklere ilişkin kullanımlar bu faturalama profiline faturalanır.

6. Sol taraftan **Fatura bölümleri**’ni seçin.

    ![Fatura bölümlerinin seçilmesini gösteren ekran görüntüsü](./media/mca-request-billing-ownership/mca-select-invoice-sections.png)   

7. Listeden bir fatura bölümü seçin. Aboneliklerin sahipliğini üstlendiğinizde bu aboneliklere ilişkin kullanımlar faturalama profilinin faturasının bu bölümüne atanır.

8. Sol alt köşeden **Aktarım istekleri**’ni ve ardından **Yeni istek ekle**’yi seçin.

    ![Aktarım isteklerinin seçilmesini gösteren ekran görüntüsü](./media/mca-request-billing-ownership/mca-select-transfer-requests.png)

9. Kendisinden fatura sahipliğini istediğiniz kullanıcının e-posta adresini girin. Kullanıcının bir Microsoft Çevrimiçi Hizmet Programı ödeme hesabında Hesap Yöneticisi veya Kurumsal Anlaşmada hesap sahibi olması gerekir. Daha fazla bilgi için bkz. [Ödeme hesaplarınızı Azure portalında görüntüleme](view-all-accounts.md). **Aktarım isteği gönder**’i seçin.

    ![Aktarım isteği gönderme işlemini gösteren ekran görüntüsü](./media/mca-request-billing-ownership/mca-send-transfer-requests.png)

10. Kullanıcı, aktarma isteğinizi gözden geçirme yönergelerini içeren bir e-posta alır.

    ![Aktarım isteğini gözden geçirme e-postasını gösteren ekran görüntüsü](./media/mca-request-billing-ownership/mca-review-transfer-request-email.png)

11. Kullanıcı, aktarma isteğini onaylamak için e-postadaki bağlantıyı seçer ve yönergeleri izler.

    ![Aktarım isteğini gözden geçirme e-postasını gösteren ekran görüntüsü](./media/mca-request-billing-ownership/mca-review-transfer-request.png)

## <a name="check-the-transfer-request-status"></a>Aktarım isteğinin durumunu denetleme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Azure portalında maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Faturalama kapsamları sayfasında, aktarım isteğinin gönderildiği ödeme hesabını seçin.

4. Soldaki **Faturalama profilleri** seçeneğini belirleyin.

    ![Faturalama profili seçmeyi gösteren ekran görüntüsü](./media/mca-request-billing-ownership/mca-select-profiles.png)     

5. Aktarım isteğinin gönderildiği **Faturalama profilini** seçin.

6. Sol taraftan **Fatura bölümleri**’ni seçin.

    ![Fatura bölümlerinin seçilmesini gösteren ekran görüntüsü](./media/mca-request-billing-ownership/mca-select-invoice-sections.png)   

7. Aktarım isteğinin gönderildiği listeden fatura bölümünü seçin.

8. Sol alt taraftaki **Aktarım istekleri**'ni seçin. Aktarım istekleri sayfasında aşağıdaki bilgiler görüntülenir:

    ![Aktarım isteklerinin listesini gösteren ekran görüntüsü](./media/mca-request-billing-ownership/mca-select-transfer-requests-for-status.png)

   |Sütun|Tanım|
   |---------|---------|
   |İstek tarihi|Aktarım isteğinin gönderildiği tarih|
   |Alıcı|Fatura sahipliğini aktarması için istek gönderdiğiniz kullanıcının e-posta adresi|
   |Son kullanma tarihi|İsteğin süresinin dolacağı tarih|
   |Durum|Aktarım isteğinin durumu|

    Aktarım isteğinin durumu aşağıdakilerden biri olabilir:

   |Durum|Tanım|
   |---------|---------|
   |Devam ediyor|Kullanıcı aktarım isteğini kabul etmedi|
   |İşleniyor|Kullanıcı aktarım isteğini onayladı. Kullanıcının seçtiği aboneliklerin ödemesi sizin fatura bölümünüze aktarılıyor|
   |Tamamlandı| Kullanıcının seçtiği aboneliklerin ödemesi sizin fatura bölümünüze aktarıldı|
   |Hatalarla tamamlandı|İstek tamamlandı ama kullanıcının seçtiği aboneliklerden bazılarının ödemesi aktarılamadı|
   |Süresi doldu|Kullanıcı belirtilen süre içinde isteği kabul etmediği için isteğin süresi doldu|
   |İptal edildi|Aktarım isteğine erişimi olan birisi isteği iptal etti|
   |Reddedildi|Kullanıcı aktarım isteğini reddetti|

9. Ayrıntılarını görüntülemek için bir aktarım isteği seçin. Aktarım ayrıntıları sayfasında aşağıdaki bilgiler görüntülenir:

    ![Aktarılan aboneliklerin listesini gösteren ekran görüntüsü](./media/mca-request-billing-ownership/mca-transfer-completed.png)

   |Sütun  |Tanım|
   |---------|---------|
   |Aktarım isteği kimliği|Aktarım isteğinizin benzersiz kimliği. Destek isteği gönderiyorsanız, destek isteğinizi hızlandırmak için Azure desteğiyle bu kimliği paylaşın|
   |Aktarım istenme tarihi|Aktarım isteğinin gönderildiği tarih|
   |Aktarım şu kişi tarafından istendi|Aktarım isteğini gönderen kullanıcının e-posta adresi|
   |Aktarım İsteğinin son kullanma tarihi| Aktarım isteğinin süresinin dolacağı tarih|
   |Alıcının e-posta adresi|Fatura sahipliğini aktarması için istek gönderdiğiniz kullanıcının e-posta adresi|
   |Alıcıya gönderilen aktarım bağlantısı|Aktarım isteğini gözden geçirmesi için kullanıcıya gönderilen URL|

## <a name="supported-subscription-types"></a>Desteklenen abonelik türleri

Aşağıda listelenen abonelik türlerinin fatura sahipliğini isteyebilirsiniz.

- [Eylem paketi](https://azure.microsoft.com/offers/ms-azr-0025p/)\*
- [Open Lisansı ile Azure](https://azure.microsoft.com/offers/ms-azr-0111p/)\*
- [Azure Pass Sponsorluğu](https://azure.microsoft.com/offers/azure-pass/)\*
- [Kurumsal Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Ücretsiz Deneme](https://azure.microsoft.com/offers/ms-azr-0044p/)\*
- [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Kullandıkça Öde Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Microsoft Azure Planı](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*
- [Microsoft Azure Sponsorluk Teklifi](https://azure.microsoft.com/offers/ms-azr-0036p/)\*
- [Microsoft Kurumsal Anlaşma](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Microsoft İş Ortağı Ağı](https://azure.microsoft.com/offers/ms-azr-0025p/)\*
- [MSDN Platformları](https://azure.microsoft.com/offers/ms-azr-0062p/)\*
- [Visual Studio Enterprise (BizSpark) aboneleri](https://azure.microsoft.com/offers/ms-azr-0064p/)\*
- [Visual Studio Enterprise (MPN) aboneleri](https://azure.microsoft.com/offers/ms-azr-0029p/)\*
- [Visual Studio Enterprise aboneleri](https://azure.microsoft.com/offers/ms-azr-0063p/)\*
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)\*
- [Visual Studio Test Professional aboneleri](https://azure.microsoft.com/offers/ms-azr-0060p/)\*

\* Abonelikte sağlanan kredilerin hiçbiri aktarım sonrasında yeni hesapta kullanılamaz.

\*\* Yalnızca Azure web sitesine kaydolurken oluşturulan hesaplardaki abonelikler için desteklenir.


## <a name="additional-information"></a>Ek bilgiler

Aşağıdaki bölümde, abonelik aktarma hakkında ek bilgiler sağlanır.

### <a name="no-service-downtime"></a>Hizmet kapalı kalma süresi yoktur

Abonelikteki Azure hizmetleri kesintisiz olarak çalışmaya devam eder. Yalnızca kullanıcının aktarmayı seçtiği Azure aboneliklerinin faturalama ilişkisini geçiririz.

### <a name="disabled-subscriptions"></a>Devre dışı bırakılan abonelikler

Devre dışı bırakılan abonelikler aktarılamaz. Fatura sahipliğini aktarmak için aboneliğin etkin durumda olması gerekir.

### <a name="azure-resources-transfer"></a>Azure kaynakları aktarımı

VM’ler, diskler ve web siteleri aktarımı gibi aboneliklerden gelen tüm kaynaklar.

### <a name="azure-marketplace-products-transfer"></a>Azure Market ürünleri aktarımı

Azure Market ürünleri, ilgili abonelikleriyle birlikte aktarılır.

### <a name="azure-reservations-transfer"></a>Azure rezervasyonları aktarımı

Kurumsal Anlaşma (EA) aboneliklerini aktarıyorsanız, Azure Rezervasyonları aboneliklerle birlikte otomatik olarak taşınmaz. Rezervasyonları taşımak için [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="access-to-azure-services"></a>Azure hizmetlerine erişim

Mevcut kullanıcılar, gruplar veya hizmet sorumlularına Azure RBAC (rol tabanlı erişim denetimi))[../role-based-access-control/overview.md] kullanılarak atanan erişim, aktarım sırasında etkilenmez.

### <a name="azure-support-plan"></a>Azure destek planınızı

Azure desteği aboneliklerle birlikte aktarılmaz. Kullanıcı tüm Azure aboneliklerini aktarıyorsa, destek planlarını iptal etmelerini isteyin.

### <a name="charges-for-transferred-subscription"></a>Aktarılan aboneliğin ücretleri

Aboneliklerin başlangıçtaki fatura sahibi, aktarımın tamamlandığı noktaya kadar bildirilen tüm ücretlerden sorumludur. Aktarım yapıldıktan sonra bildirilen ücretler sizin fatura bölümünüzün sorumluluğundadır. Aktarımdan önce gerçekleşen ancak daha sonra bildirilen ücretler olabilir. Bu ücretler fatura bölümünüzde gösterilir.

### <a name="cancel-a-transfer-request"></a>Aktarım isteğini iptal etme

Aktarım isteğini, istek onaylanana veya reddedilene kadar iptal edebilirsiniz. Aktarım isteğini iptal etmek için [aktarım ayrıntıları sayfasına](#check-the-transfer-request-status) gidin ve sayfanın altından iptal düğmesini seçin.

### <a name="software-as-a-service-saas-transfer"></a>Hizmet Olarak Yazılım (SaaS) aktarımı

SaaS ürünleri aboneliklerle birlikte aktarılmaz. SaaS ürünlerinin fatura sahipliğini aktarmak için kullanıcının [Azure desteğine başvurmasını](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) isteyin. Kullanıcı, fatura sahipliğiyle birlikte kaynak sahipliğini de aktarabilir. Kaynak sahipliği, ürünü silme ve ayrıntılarını görüntüleme gibi yönetim işlemleri gerçekleştirmenizi sağlar. Kullanıcının kaynak sahipliğini aktarabilmesi için SaaS ürününde kaynak sahibi olması gerekir.

## <a name="check-for-access"></a>Erişim denetimi
[!INCLUDE [billing-check-mca](../../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Sonraki adımlar

- Azure aboneliklerinin fatura sahipliği fatura bölümünüze aktarıldı. Bu aboneliklerin ücretlerini [Azure portalından](https://portal.azure.com) takip edin.
- Diğer kullanıcılara bu aboneliklere ait faturalamayı görüntüleme ve yönetme izni verin. Daha fazla bilgi için bkz. [Fatura bölümü rolleri ve görevleri](understand-mca-roles.md#invoice-section-roles-and-tasks).
