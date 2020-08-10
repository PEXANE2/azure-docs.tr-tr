---
title: Faturanızı gereksinimlerinize göre düzenleme - Azure
description: Faturanızdaki maliyetleri düzenlemeyi öğrenin. Faturalama profilleri ve fatura bölümleri oluşturarak ödeme hesabınızı özelleştirebilirsiniz.
author: amberbhargava
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: banders
ms.openlocfilehash: 553263ebac3c64e27a7711b4407a7d4ba573998c
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87460040"
---
# <a name="organize-costs-by-customizing-your-billing-account"></a>Ödeme hesabınızı özelleştirerek maliyetleri düzenleme

Microsoft Müşteri Sözleşmesi ödeme hesabınız, maliyetlerinizi gereksinimlerinizi dikkate alarak departman, proje veya geliştirme ortamı ölçütlerine göre düzenleme esnekliği sunar.

Bu makalede Azure portalını kullanarak maliyetlerinizi nasıl düzenleyebileceğiniz anlatılmaktadır. Microsoft Müşteri Sözleşmesi’ne ilişkin ödeme hesabı için geçerlidir. [Microsoft Müşteri Sözleşmesi’ne erişiminiz olup olmadığını denetleyin](#check-access-to-a-microsoft-customer-agreement).

## <a name="structure-your-account-with-billing-profiles-and-invoice-sections"></a>Faturalama profillerini ve fatura bölümlerini kullanılarak hesabınızı yapılandırma

Microsoft Müşteri Sözleşmesi'nin ödeme hesabı bölümünde faturalama profillerini ve fatura bölümlerini kullanarak maliyetlerinizi düzenleyebilirsiniz.

![Microsoft Müşteri Sözleşmesi faturalandırma hiyerarşisini gösteren ekran görüntüsü](./media/mca-section-invoice/mca-hierarchy.png)

### <a name="billing-profile"></a>Faturalama profili

Faturalama profili, bir faturayı ve ödeme yöntemleri ve fatura adresi gibi ilgili ödeme bilgilerini temsil eder. Hesabınızdaki her bir faturalama profili için ay başında aylık bir fatura oluşturulur. Fatura, önceki aya ait Azure kullanımının ve diğer satın alımların ücretlerini içerir.

Azure'a kaydolduğunuzda otomatik olarak bir faturalama profili ve ödeme hesabı oluşturulur. Maliyetlerinizi birden fazla aylık faturaya bölmek için ek faturalama profilleri oluşturabilirsiniz.

> [!IMPORTANT]
>
> Ek faturalama profillerinin eklenmesi toplam maliyetinizi etkileyebilir. Daha fazla bilgi için bkz. [Yeni faturalama profili eklerken dikkat edilmesi gerekenler](#things-to-consider-when-adding-new-billing-profiles).

### <a name="invoice-section"></a>Fatura bölümü

Fatura bölümü, faturanızdaki bir maliyet grubunu temsil eder. Hesabınızdaki her faturalama profili için otomatik olarak bir fatura bölümü oluşturulur. Maliyetlerinizi gereksinimlerinize göre düzenlemek için başka bölümler de oluşturabilirsiniz. Her bir fatura bölümü, ilgili ay için tahakkuk eden ücretlerle birlikte faturada görüntülenir.

Aşağıdaki görüntüde Mühendislik ve Pazarlama olmak üzere iki fatura bölümüne sahip olan bir fatura gösterilmektedir. Faturada her bir bölümün özeti ve ayrıntılı ücretleri görüntülenmiştir. Görüntüde yer alan fiyatlar yalnızca örnek amaçlıdır ve Azure hizmetlerinin gerçek fiyatlarını yansıtmamaktadır.

![Bölümleri olan bir faturayı gösteren görüntü](./media/mca-section-invoice/mca-invoice-with-sections.png)

## <a name="billing-account-structure-for-common-scenarios"></a>Yaygın senaryolar için ödeme hesabı yapısı

Bu bölümde maliyetleri ve karşılık gelen ödeme hesabı yapılarını düzenlemeye yönelik yaygın senaryolar anlatılmaktadır:

|Senaryo  |Yapı  |
|---------|---------|
|Jack, Azure'a kaydoluyor ve aylık tek bir faturaya ihtiyaç duyuyor. | Faturalama profili ve fatura bölümü. Jack, Azure'a kaydolduğunda bu yapı otomatik olarak ayarlanır ve ek işlem gerçekleştirilmesine gerek yoktur. |

![Basit faturalandırma senaryosu için bilgi grafiği](./media/mca-section-invoice/organize-billing-scenario1.png)

|Senaryo  |Yapı  |
|---------|---------|
|Contoso, ayda yalnızca bir faturaya ihtiyaç duyan ancak maliyetlerini pazarlama ve mühendislik departmanları olacak şekilde departmanlara ayırmış olan küçük ölçekli bir kuruluştur.  | Contoso için bir faturalama profili ve pazarlama ile mühendislik departmanları için fatura bölümleri. |

![Basit faturalandırma senaryosu için bilgi grafiği](./media/mca-section-invoice/organize-billing-scenario2.png)

|Senaryo  |Yapı  |
|---------|---------|
|Fabrikam, mühendislik ve pazarlama departmanları için ayrı faturalara ihtiyaç duyan orta büyüklükteki bir kuruluştur. Şirket, mühendislik departmanı için maliyetleri üretim ve geliştirme ortamlarına göre gruplamak istiyor.  | Pazarlama ve mühendislik departmanlarına özgü faturalama profilleri. Mühendislik departmanı için üretim ve geliştirme ortamlarına ait fatura bölümleri. |

![Basit faturalandırma senaryosu için bilgi grafiği](./media/mca-section-invoice/organize-billing-scenario3.png)

## <a name="create-a-new-invoice-section"></a>Yeni fatura bölümü oluşturma

Fatura bölümü oluşturmak için **faturalama profili sahibi** veya **faturalama profili katkıda bulunanı** olmanız gerekir. Daha fazla bilgi için bkz. [Faturalama profili için fatura bölümlerini yönetme](understand-mca-roles.md#manage-invoice-sections-for-billing-profile).

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Portalda maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/mca-section-invoice/search-cmb.png)

3. Soldaki bölmede **Faturalama profilleri**'ni seçin. Listeden bir faturalama profili seçin. Seçilen faturalama profilinin faturasında yeni bir bölümü görüntülenir.

   [![Faturalama profili listesini gösteren ekran görüntüsü](./media/mca-section-invoice/mca-select-profile.png)](./media/mca-section-invoice/mca-select-profile-zoomed-in.png#lightbox)

4. Soldaki bölmeden **Fatura bölümleri**'ni seçtikten sonra sayfanın üst tarafından **Ekle**'yi seçin.

   [![Fatura bölümlerinin eklenmesini gösteren ekran görüntüsü](./media/mca-section-invoice/mca-list-invoice-sections.png)](./media/mca-section-invoice/mca-list-invoice-sections-zoomed-in.png#lightbox)

5. Fatura bölümü için bir ad girin.

   [![Fatura bölümü oluşturma sayfasını gösteren ekran görüntüsü](./media/mca-section-invoice/mca-create-invoice-section.png)](./media/mca-section-invoice/mca-create-invoice-section-zoomed-in.png#lightbox)

6. **Oluştur**’u seçin.

## <a name="create-a-new-billing-profile"></a>Yeni faturalama profili oluşturma

Faturalama profili oluşturmak için **ödeme hesabı sahibi** veya **ödeme hesabı katkıda bulunanı** olmanız gerekir. Daha fazla bilgi için bkz. [Ödeme hesabı için faturalama profillerini yönetme](understand-mca-roles.md#manage-billing-profiles-for-billing-account).

> [!IMPORTANT]
>
> Ek faturalama profillerinin eklenmesi toplam maliyetinizi etkileyebilir. Daha fazla bilgi için bkz. [Yeni faturalama profili eklerken dikkat edilmesi gerekenler](#things-to-consider-when-adding-new-billing-profiles).

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Portalda maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/mca-section-invoice/search-cmb.png)

3. Soldaki bölmeden **Faturalama profilleri**'ni seçtikten sonra sayfanın üst tarafından **Ekle**'yi seçin.

   [![Faturalama profili listesini gösteren ekran görüntüsü](./media/mca-section-invoice/mca-list-profiles.png)](./media/mca-section-invoice/mca-list-profiles-zoomed-in.png#lightbox)

    > [!Note]
    >
    > Faturalama profili sayfasında Ekle düğmesinin bulunmaması, özelliğin hesabınızda kullanılamadığını gösterir. Bu özellik şu an için yalnızca bir Microsoft temsilcisiyle birlikte oluşturulan hesaplar için kullanılabilir durumdadır.

4. Formu doldurup **Oluştur**'a tıklayın.

   [![Faturalama profili oluşturma sayfasını gösteren ekran görüntüsü](./media/mca-section-invoice/mca-add-profile.png)](./media/mca-section-invoice/mca-add-profile-zoomed-in.png#lightbox)

    |Alan  |Tanım  |
    |---------|---------|
    |Adı     | Azure portalında faturalama profilini kolayca belirlemenize yardımcı olan görünen ad.  |
    |Satın alma siparişi numarası    | İsteğe bağlı bir satın alma siparişi numarası. Satın alma siparişi numarası, faturalama profili için oluşturulan faturalara eklenir. |
    |Faturalandırma adresi   | Faturalandırma adresi, faturalama profili için oluşturulan faturalara eklenir. |
    |Faturayı e-posta ile gönder   | Bu faturalama profiline ait faturaları e-posta ile almak için Faturayı e-posta ile gönder kutusunu işaretleyin. Bunu tercih etmezseniz faturaları Azure portalından görüntüleyebilir ve indirebilirsiniz.|

5. **Oluştur**’u seçin.

## <a name="link-charges-to-invoice-sections-and-billing-profiles"></a>Ücretleri fatura bölümlerine ve faturalama profillerine bağlama

Ödeme hesabınızı gereksinimlerinize göre özelleştirdikten sonra abonelikleri ve diğer ürünleri istediğiniz fatura bölümüne ve faturalama profiline bağlayabilirsiniz.

### <a name="link-a-new-subscription"></a>Yeni abonelik bağlama

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Abonelikler** araması yapın.

   [![Portalda abonelik aramasını gösteren ekran görüntüsü](./media/mca-section-invoice/search-subscriptions.png)](./media/mca-section-invoice/search-subscriptions.png#lightbox)

3. Sayfanın üst kısmında **Ekle**’yi seçin.

   ![Abonelikler görünümünde Ekle düğmesini gösteren ekran görüntüsü](./media/mca-section-invoice/subscription-add.png)

4. Birden çok ödeme hesabına erişiminiz varsa, Microsoft Müşteri Sözleşmesi ödeme hesabınıza geçin.

   ![Abonelikler görünümünde Ekle düğmesini gösteren ekran görüntüsü](./media/mca-section-invoice/mca-create-azure-subscription.png)

5. Aboneliğin kullanımı için faturalandırılacak faturalama profilini seçin. Bu abonelik için Azure kullanımı ve diğer satın alma ücretleri, seçilen faturalama profilinin faturasına dahil edilir.

6. Aboneliğin ücretlerine bağlanacak fatura bölümünü seçin. Ücretler, faturalama profilinin faturasında bu bölümde görüntülenir.

7. Bir Azure planı seçin ve aboneliğiniz için kolay ad girin.

9. **Oluştur**’a tıklayın.  

### <a name="link-existing-subscriptions-and-products"></a>Mevcut abonelikleri ve ürünleri bağlama

Mevcut Azure abonelikleriniz veya Azure Market ve App Source kaynakları gibi diğer ürünleriniz varsa bunları mevcut fatura bölümünden başka bir fatura bölümüne taşıyarak maliyetlerinizi yeniden düzenleyebilirsiniz.

> [!IMPORTANT]
>
> Abonelikler ve diğer ürünler yalnızca aynı faturalama profiline ait olan fatura bölümleri arasında taşınabilir. Aboneliklerin ve ürünlerin farklı faturalama profillerindeki fatura bölümleri arasında taşınması desteklenmez.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Portalda abonelik aramasını gösteren ekran görüntüsü](./media/mca-section-invoice/search-cmb.png)

3. Aboneliği yeni bir fatura bölümüne bağlamak için ekranın sol tarafından **Azure abonelikleri**'ni seçin. Azure Market ve App Source kaynakları gibi diğer ürünler için **Yinelenen ücretler**'i seçin.

   [![Fatura bölümünü değiştirme seçeneğini gösteren ekran görüntüsü](./media/mca-section-invoice/mca-select-change-invoice-section.png)](./media/mca-section-invoice/mca-select-change-invoice-section.png#lightbox)

4. Sayfada yeni bir fatura bölümüne bağlamak istediğiniz abonelik veya ürün için üç nokta simgesine tıklayın. **Fatura bölümünü değiştir**'i seçin.

5. Açılan listeden yeni fatura bölümünü seçin. Açılan listede yalnızca mevcut fatura bölümüyle aynı faturalama profiliyle ilişkili fatura bölümleri görüntülenir.

    [![Yeni bir fatura bölümünün seçilmesini gösteren ekran görüntüsü](./media/mca-section-invoice/mca-select-new-invoice-section.png)](./media/mca-section-invoice/mca-select-new-invoice-section-zoomed-in.png#lightbox)

6. **Kaydet**’i seçin.

## <a name="things-to-consider-when-adding-new-billing-profiles"></a>Yeni faturalama profili eklerken dikkat edilmesi gerekenler

### <a name="azure-usage-charges-may-be-impacted"></a>Azure kullanım ücretleri etkilenebilir

Microsoft Müşteri Sözleşmesi ödeme hesabınızda Azure kullanımı her bir faturalama profili için aylık olarak toplanır. Katmanlı fiyatlandırmaya sahip Azure kaynakları için fiyatlar, her faturalama profilindeki kullanıma göre belirlenir. Fiyat hesaplanırken farklı faturalama profillerindeki kullanım toplanmaz. Bu, birden fazla faturalama profiline sahip olan hesaplar için genel Azure kullanımı maliyetini etkileyebilir.

Şimdi iki örnek senaryodaki maliyetlerin nasıl değiştiğine bakalım. Senaryolarda kullanılan fiyatlar yalnızca örnek amaçlıdır ve Azure hizmetlerinin gerçek fiyatlarını yansıtmamaktadır.

#### <a name="you-only-have-one-billing-profile"></a>Yalnızca bir faturalama profiliniz var.

İlk 50 terabayt (TB) için GB başına 0,00184 ABD doları, sonraki 450 terabayt için de GB başına 0,00177 ABD doları maliyete sahip olan Azure blok blobu depolama alanını kullandığınızı düşünelim. Aboneliklerde faturalama profiliniz için faturalanan 100 TB kullanım gerçekleştirdiğinizde alınacak ücret aşağıda gösterildiği gibi hesaplanır.

|  Katman fiyatlandırması (ABD doları) |Miktar | Tutar (ABD doları)|
|---------|---------|---------|
|İlk 50 TB/ay için TB başına 1,84    | 50 TB        | 92,0   |
|Sonraki 450 TB/ay için TB başına 1,77    |  50 TB         | 88,5   |
|Toplam     |     100 TB  | 180,5

Bu senaryoda 100 TB veri kullanmanın toplam ücreti **180,5** ABD doları olur.

#### <a name="you-have-multiple-billing-profiles"></a>Birden çok faturalama profiliniz var.

Şimdi başka bir faturalama profili oluşturduğunuzu ve abonelik üzerinden ilk faturalama profiline faturalanan 50 GB ve yine abonelik üzerinden ikinci faturalama profiline faturalanan 50 GB kullanım gerçekleştirdiğinizde alınacak ücret aşağıda gösterildiği gibi hesaplanır.

`Charges for the first billing profile`

|  Katman fiyatlandırması (ABD doları) |Miktar | Tutar (ABD doları)|
|---------|---------|---------|
|İlk 50 TB/ay için TB başına 1,84    | 50 TB        | 92,0  |
|Sonraki 450 TB/ay için TB başına 1,77    |  0 TB         | 0,0  |
|Toplam     |     50 TB  | 92,0

`Charges for the second billing profile`

|  Katman fiyatlandırması (ABD doları) |Miktar | Tutar (ABD doları)|
|---------|---------|---------|
|İlk 50 TB/ay için TB başına 1,84    | 50 TB        | 92,0  |
|Sonraki 450 TB/ay için TB başına 1,77    |  0 TB         | 0,0  |
|Toplam     |     50 TB  | 92,0

Bu senaryoda 100 TB veri kullanmanın toplam ücreti **184,0** (92,0 * 2) ABD doları olur.

### <a name="azure-reservation-benefits-might-not-apply-to-all-subscriptions"></a>Azure rezervasyon avantajları tüm abonelikler için uygulanmayabilir

Paylaşılan kapsama sahip olan Azure rezervasyonları, tek bir faturalama profilindeki aboneliklere uygulanır ve farklı faturalama profilleri arasında paylaşılmaz.

![Farklı ödeme hesabı yapılarındaki rezervasyon uygulamasını gösteren bilgi grafiği](./media/mca-section-invoice/mca-reservations-benefits-by-bg.png)

Yukarıdaki görüntüde Contoso'nun iki aboneliği vardır. Azure rezervasyon avantajı, ödeme hesabının yapısına bağlı olarak farklı şekilde uygulanmıştır. Soldaki senaryoda rezervasyon avantajı, mühendislik faturalama profiline faturalanan iki aboneliğe de uygulanmıştır. Sağdaki senaryoda rezervasyon avantajı yalnızca mühendislik faturalama profiline faturalanan abonelik olan 1 numaralı aboneliğe uygulanacaktır.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi’ne erişimi denetleme
[!INCLUDE [billing-check-mca](../../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Müşteri Sözleşmesi için ek Azure aboneliği oluşturma](create-subscription.md)
- [Azure portalında faturalama rollerini yönetme](understand-mca-roles.md#manage-billing-roles-in-the-azure-portal)
- [Diğer ödeme hesaplarındaki kullanıcılardan Azure aboneliklerinin fatura sahipliğini alma](mca-request-billing-ownership.md)
