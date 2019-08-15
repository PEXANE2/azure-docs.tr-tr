---
title: Azure aboneliklerinin fatura sahipliğini alma
description: Diğer kullanıcılardan Azure aboneliklerinin faturalama sahipliğini isteme hakkında bilgi edinin.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 110bd975de1d865c2aa3d3b088c98cdc8b9e45ad
ms.sourcegitcommit: b12a25fc93559820cd9c925f9d0766d6a8963703
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69019568"
---
# <a name="get-billing-ownership-of-azure-subscriptions-from-other-accounts"></a>Diğer hesaplardan Azure aboneliklerinin fatura sahipliğini alma

Mevcut faturalandırma sahibi kuruluşunuzu terk edebiliyor veya faturalandırma hesabınız üzerinden abonelikler için ödeme yapmak istiyorsanız, Azure aboneliklerinin sahipliğini devralmak isteyebilirsiniz. Sahipliğin alınması, hesabınıza aboneliklerin fatura sorumluluklarını aktarır.

Bu makale, bir Microsoft Müşteri Sözleşmesi için faturalandırma hesabı için geçerlidir. [Bir Microsoft Müşteri sözleşmesine erişiminiz olup olmadığını denetleyin](#check-for-access).

Faturalandırma sahipliğini istemek için bir **Fatura bölümü sahibi** veya **Fatura bölümü katılımcısı**olmanız gerekir. Daha fazla bilgi için bkz. [Fatura bölümü rol görevleri](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="request-billing-ownership"></a>Faturalandırma sahipliğini iste

1. [Azure Portal](https://portal.azure.com) bir fatura bölümü sahibi veya Microsoft Müşteri Sözleşmesi faturalama hesabı için katkıda bulunan olarak oturum açın.

2. **Maliyet yönetimi + faturalandırma**için arama yapın.

   ![Maliyet yönetimi + faturalandırma Azure portal araması gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Sol taraftaki **Fatura bölümlerini** seçin. Erişimlerinize bağlı olarak, bir faturalandırma hesabı veya faturalandırma profili seçmeniz gerekebilir. Faturalandırma hesabı veya profilinden **Fatura bölümleri**' ni seçin.
   
   ![Fatura bölümlerini seçmeyi gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-select-invoice-sections.png)        

4. Listeden bir fatura seçin. Aboneliklerin sahipliğini aldıktan sonra, bu fatura bölümüne faturalandırılır.

5. Sol alt taraftaki **Istekleri aktar** ' ı seçin ve ardından **Ekle**' yi seçin.
 
   ![Aktarım isteklerini seçmeyi gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-select-transfer-requests.png)

6. Faturalama sahipliğini istediğiniz kullanıcının e-posta adresini girin. Kullanıcının bir Microsoft çevrimiçi hizmet programı faturalandırma hesabında veya bir Kurumsal Anlaşma hesap sahibinde hesap yöneticisi olması gerekir. Daha fazla bilgi için bkz. [Azure Portal faturalandırma hesaplarınızı görüntüleme](billing-view-all-accounts.md). **Aktarım Isteği gönder**' i seçin.

   ![Aktarım isteği göndermeyi gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-send-transfer-requests.png)

7. Kullanıcı, aktarım isteğinizi gözden geçirmek için yönergeler içeren bir e-posta alır.

   ![Gözden geçirme aktarım isteği e-postasını gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-review-transfer-request-email.png)

8. Aktarım isteğini onaylamak için kullanıcı e-postadaki bağlantıyı seçer ve yönergeleri izler.

    ![Gözden geçirme aktarım isteği e-postasını gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-review-transfer-request.png)

## <a name="check-the-transfer-request-status"></a>Aktarım isteği durumunu denetleyin

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Maliyet yönetimi + faturalandırma**için arama yapın.

   ![Maliyet yönetimi + faturalandırma Azure portal araması gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)


3. Sol taraftaki **Fatura bölümlerini** seçin. Erişimlerinize bağlı olarak, bir faturalandırma hesabı veya faturalandırma profili seçmeniz gerekebilir. Faturalandırma hesabı veya profilinden **Fatura bölümleri**' ni seçin.
   
   ![Fatura bölümlerini seçmeyi gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-select-invoice-sections.png)        

4. Aktarım isteğini gönderdiğiniz listeden fatura bölümünü seçin.

5. Sol alt taraftaki **Aktarım isteklerini** seçin. Aktarım istekleri sayfasında aşağıdaki bilgiler görüntülenir:

    ![Aktarım isteklerinin listesini gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-select-transfer-requests-for-status.png)

   |Sütun|Tanım|
   |---------|---------|
   |İstek tarihi|Aktarım isteğinin gönderildiği tarih|
   |Alıcı|Faturalandırma sahipliğini aktarmaya yönelik isteği gönderdiğiniz kullanıcının e-posta adresi|
   |Son kullanma tarihi|İsteğin süresinin dolacağı tarih|
   |Durum|Aktarım isteğinin durumu|

    Aktarım isteği aşağıdaki durumlardan birine sahip olabilir:

   |Durum|Tanım|
   |---------|---------|
   |Sürüyor|Kullanıcı Aktarım isteğini kabul etmedi|
   |İşleniyor|Kullanıcı Aktarım isteğini onayladı. Kullanıcının seçtiği abonelikler için fatura bölümüne aktarılan abonelikler|
   |Tamamlandı| Kullanıcının seçtiği Aboneliklerle ilgili faturalandırma fatura bölümüne aktarılır|
   |Hatalarla tamamlandı|İstek tamamlandı, ancak kullanıcının seçtiği bazı abonelikler için faturalandırılır|
   |Süresi dolmuş|Kullanıcı, isteği zamanında kabul etmedi ve süresi dolmadı|
   |İptal edildi|Aktarım isteğine erişimi olan birisi isteği iptal etti|
   |Reddedildi|Kullanıcı Aktarım isteğini reddetti|

7. Ayrıntıları görüntülemek için bir aktarım isteği seçin. Aktarım ayrıntıları sayfasında aşağıdaki bilgiler görüntülenir:
   
   ![Aktarılan Aboneliklerin listesini gösteren ekran görüntüsü](./media/billing-mca-request-billing-ownership/mca-transfer-completed.png)
    
   |Sütun  |Tanım|
   |---------|---------|
   |Aktarım isteği KIMLIĞI|Aktarım isteğinizin benzersiz KIMLIĞI. Bir destek isteği gönderirseniz, destek isteğinizi hızlandırmak için KIMLIĞI Azure desteği ile paylaşabilirsiniz|
   |Aktarım isteği açık|Aktarım isteğinin gönderildiği tarih|
   |Aktarım isteği|Aktarım isteğini gönderen kullanıcının e-posta adresi|
   |Aktarım isteğinin süresi doluyor| Aktarım isteğinin süresinin dolacağı tarih|
   |Alıcının e-posta adresi|Faturalandırma sahipliğini aktarmaya yönelik isteği gönderdiğiniz kullanıcının e-posta adresi|
   |Alıcıya gönderilen aktarım bağlantısı|Aktarım isteğini gözden geçirmek için kullanıcıya gönderilen URL|

## <a name="supported-subscription-types"></a>Desteklenen Abonelik türleri

Aşağıda listelenen abonelik türlerinin fatura sahipliğini isteyebilirsiniz.

- [Eylem paketi](https://azure.microsoft.com/offers/ms-azr-0025p/)\* 
- [Open Lisansı ile Azure](https://azure.microsoft.com/offers/ms-azr-0111p/)\*
- [Azure Pass sponsorluk](https://azure.microsoft.com/offers/azure-pass/)\*
- [Kurumsal Geliştirme ve test](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Ücretsiz deneme](https://azure.microsoft.com/offers/ms-azr-0044p/)\*
- [Kullandıkça öde](https://azure.microsoft.com/offers/ms-azr-0003p/) 
- [Kullandıkça Öde geliştirme ve test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Microsoft Azure planı](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*
- [Sponsorlu Microsoft Azure teklifi](https://azure.microsoft.com/offers/ms-azr-0036p/)\*
- [Microsoft Kurumsal Anlaşma](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Microsoft İş Ortağı Ağı](https://azure.microsoft.com/offers/ms-azr-0025p/)\*
- [MSDN Platformları](https://azure.microsoft.com/offers/ms-azr-0062p/)\*
- [Visual Studio Enterprise (BizSpark) aboneleri](https://azure.microsoft.com/offers/ms-azr-0064p/)\*
- [Visual Studio Enterprise (MPN) aboneleri](https://azure.microsoft.com/offers/ms-azr-0029p/)\*
- [Visual Studio Enterprise aboneler](https://azure.microsoft.com/offers/ms-azr-0063p/)\*
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)\*
- [Visual Studio Test Professional aboneler](https://azure.microsoft.com/offers/ms-azr-0060p/)\*

\*Abonelikte bulunan tüm krediler, aktarımdan sonra yeni hesapta kullanılabilir olmayacaktır.

\*\*Yalnızca Azure Web sitesinde kaydolma sırasında oluşturulan hesaplara ait abonelikler için desteklenir.


## <a name="additional-information"></a>Ek bilgiler

Aşağıdaki bölümde, abonelikleri aktarma hakkında ek bilgiler sağlanmaktadır.

### <a name="no-service-downtime"></a>Hizmet kapalı kalma süresi yok

Abonelikteki Azure Hizmetleri herhangi bir kesinti olmadan çalışmaya devam ediyor. Yalnızca kullanıcının aktarmaya seçtiği Azure abonelikleriyle ilgili faturalandırma ilişkilerini geçiririz.

### <a name="disabled-subscriptions"></a>Devre dışı bırakılan abonelikler

Devre dışı bırakılmış abonelikler aktarılamaz. Aboneliklerin, faturalandırma sahiplerinin aktarılması için etkin durumda olması gerekir.

### <a name="azure-resources-transfer"></a>Azure kaynakları aktarımı

Aboneliklerden VM 'Ler, diskler ve Web siteleri aktarımı gibi tüm kaynaklar.

### <a name="azure-marketplace-products-transfer"></a>Azure Market ürünleri aktarımı

Azure Market ürünleri, ilgili abonelikleriyle birlikte aktarılır.

### <a name="azure-reservations-transfer"></a>Azure ayırmaları aktarımı

Azure ayırmaları, aboneliklerle otomatik olarak taşınamaz. Rezervasyonları taşımak için [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

### <a name="access-to-azure-services"></a>Azure hizmetlerine erişim

(Azure RBAC (rol tabanlı erişim denetimi)) kullanılarak atanmış mevcut kullanıcılar, gruplar veya hizmet sorumluları için erişim [.. /Role-based-Access-Control/Overview.exe] geçiş sırasında etkilenmez.

### <a name="azure-support-plan"></a>Azure destek planınızı

Azure desteği aboneliklerle aktarılmaz. Kullanıcı tüm Azure aboneliklerini aktardığından bunların destek planını iptal etmesini isteyin.

### <a name="charges-for-transferred-subscription"></a>Aktarılan abonelik için ücretler

Aboneliklerin orijinal faturalama sahibi, aktarımın tamamlandığı noktaya kadar raporlanan ücretlerden sorumludur. Fatura bölümlerinizin, aktarma zamanından itibaren raporlanan ücretler sorumludur. Transferden önce gerçekleşen ancak daha sonra raporlanan bazı ücretler olabilir. Bu ücretler fatura bölüminizdeki görünür.

### <a name="cancel-a-transfer-request"></a>Aktarım isteğini iptal et

İstek onaylanana veya reddetene kadar aktarım isteğini iptal edebilirsiniz. Aktarım isteğini iptal etmek için, [Aktarım ayrıntıları sayfasına](#check-the-transfer-request-status) gidin ve sayfanın altındaki iptal ' i seçin.

### <a name="software-as-a-service-saas-transfer"></a>Hizmet olarak yazılım (SaaS) aktarımı

SaaS ürünleri aboneliklerle aktarılmaz. Kullanıcının SaaS ürünlerinin fatura sahipliğini aktarmak için [Azure desteği 'Ne başvurmasını](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) isteyin. Faturalama sahipliğiyle birlikte Kullanıcı kaynak sahipliğini de aktarabilir. Kaynak sahipliği, ürünün ayrıntılarını silme ve görüntüleme gibi yönetim işlemleri gerçekleştirmenize olanak tanır. Kullanıcının kaynak sahipliğini aktarmak için SaaS ürününde bir kaynak sahibi olması gerekir.

## <a name="check-for-access"></a>Erişimi denetle
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Yardıma ihtiyacınız varsa, sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

## <a name="next-steps"></a>Sonraki adımlar

- Azure aboneliklerinin faturalama sahipliği fatura bölümüne aktarılır. [Azure Portal](https://portal.azure.com)bu Aboneliklerle ilgili ücretleri izleyin.
- Başkalarına bu abonelikler için faturalandırmayı görüntüleme ve yönetme izinleri verin. Daha fazla bilgi için bkz. [Fatura bölümü rolleri ve görevleri](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).
