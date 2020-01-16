---
title: Faturanızı gereksinimlerinize göre düzenleyin-Azure
description: Faturanızda maliyeti nasıl düzenleyebileceğinizi öğrenin.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: c170a6a1a731a648c16e0081e760947256df8a0e
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75991067"
---
# <a name="organize-costs-by-customizing-your-billing-account"></a>Faturalandırma hesabınızı özelleştirerek maliyetleri düzenleyin

Microsoft Müşteri sözleşmeniz için faturalandırma hesabınız, departmanlarınızın, projenin veya geliştirme ortamının yanı olmadığı gereksinimlerinize göre maliyetlerinizi düzenlemenize yönelik esneklik sağlar. 

Bu makalede, maliyetlerinizi düzenlemek için Azure portal nasıl kullanabileceğiniz açıklanır. Microsoft Müşteri Sözleşmesi için bir faturalandırma hesabı için geçerlidir. [Microsoft Müşteri Sözleşmesi’ne erişiminiz olup olmadığını denetleyin](#check-access-to-a-microsoft-customer-agreement).

## <a name="structure-your-account-with-billing-profiles-and-invoice-sections"></a>Faturalandırma profilleri ve fatura bölümleri ile hesabınızı yapılandırma

Bir Microsoft Müşteri Sözleşmesi faturalandırma hesabında, maliyetlerinizi düzenlemek için faturalandırma profillerini ve fatura bölümlerini kullanırsınız.

![Microsoft müşteri anlaşması faturalandırma hiyerarşisini gösteren ekran görüntüsü](./media/mca-section-invoice/mca-hierarchy.png)

### <a name="billing-profile"></a>Faturalama profili

Faturalandırma profili bir faturayı ve ödeme yöntemleri ve faturalandırma adresi gibi ilgili faturalandırma bilgilerini temsil eder. Hesabınızdaki her bir faturalama profili için ay başında aylık bir fatura oluşturulur. Fatura, Azure kullanımı ve önceki aydan diğer satın alma ücretlerini içerir.

Azure 'a kaydolduğunuzda Faturalandırma hesabınızla birlikte Faturalandırma profili de otomatik olarak oluşturulur. Maliyetlerinizi birden çok aylık faturalarda düzenlemek için ek faturalandırma profilleri oluşturabilirsiniz. 

> [!IMPORTANT]
>
> Ek faturalandırma profillerinin oluşturulması, genel maliyetinizi etkileyebilir. Daha fazla bilgi için bkz. [yeni faturalandırma profilleri eklerken göz önünde bulundurmanız gerekenler](#things-to-consider-when-adding-new-billing-profiles).

### <a name="invoice-section"></a>Fatura bölümü

Fatura bölümü faturanızda bir maliyet gruplandırması temsil eder. Hesabınızdaki her faturalandırma profili için otomatik olarak bir fatura bölümü oluşturulur. Gereksinimlerinize göre maliyetlerinizi düzenlemek için başka bölümler de oluşturabilirsiniz. Her fatura bölümü faturada, söz konusu ay tahakkuk eden ücretler ile görüntülenir. 

Aşağıdaki görüntüde, mühendislik ve pazarlama olmak üzere iki fatura bölümü olan bir fatura gösterilmektedir. Her bölüm için Özet ve ayrıntı ücretleri faturada görüntülenir. Görüntüde gösterilen fiyatlar yalnızca örnek amaçlıdır ve Azure hizmetlerinin gerçek fiyatlarını temsil etmez. 

![Bölümleri olan bir faturayı gösteren resim](./media/mca-section-invoice/mca-invoice-with-sections.png)

## <a name="billing-account-structure-for-common-scenarios"></a>Genel senaryolar için faturalandırma hesabı yapısı

Bu bölümde, maliyetleri ve ilgili faturalandırma hesabı yapılarını düzenlemek için yaygın senaryolar açıklanmaktadır:

|Senaryo  |Yapı  |
|---------|---------|
|Azure için jak atar ve tek bir aylık faturaya ihtiyaç duyuyor. | Faturalama profili ve fatura bölümü. Bu yapı, Azure 'a kaydolursa otomatik olarak bir jak ayarlanır ve ek adımlar gerektirmez. |

![Basit faturalandırma senaryosu için bilgi grafiği](./media/mca-section-invoice/organize-billing-scenario1.png)

|Senaryo  |Yapı  |
|---------|---------|
|Contoso, tek bir aylık faturaya ihtiyacı olan, maliyetleri departmanlarına göre gruplamak için küçük bir kuruluştur.  | Her biri pazarlama ve finans departmanları için Contoso ve fatura için bir fatura profili. |

![Basit faturalandırma senaryosu için bilgi grafiği](./media/mca-section-invoice/organize-billing-scenario2.png)

|Senaryo  |Yapı  |
|---------|---------|
|Fabrikam, mühendisler ve pazarlama departmanları için ayrı faturalar gerektiren orta büyüklükteki bir kuruluştur. Mühendislik departmanı için ortamları ortamlar-üretimler ve geliştirmeye göre gruplamak ister.  | Her bir pazarlama ve finans departmanları için bir faturalandırma profili. Pazarlama departmanı için, her biri üretim ve geliştirme ortamı için bir fatura bölümü. |

![Basit faturalandırma senaryosu için bilgi grafiği](./media/mca-section-invoice/organize-billing-scenario3.png)

## <a name="create-a-new-invoice-section"></a>Yeni fatura bölümü oluşturma

Fatura bölümü oluşturmak için **faturalama profili sahibi** veya **faturalama profili katkıda bulunanı** olmanız gerekir. Daha fazla bilgi için bkz. [Faturalandırma profili için fatura bölümlerini yönetme](understand-mca-roles.md#manage-invoice-sections-for-billing-profile).

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Portalda maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/mca-section-invoice/search-cmb.png)

3. Sol bölmeden **faturalandırma profilleri** ' ni seçin. Listeden bir faturalandırma profili seçin. Yeni bölüm, seçilen Faturalandırma profili faturasında görüntülenir. 

   [Faturalama profili listesini gösteren ekran görüntüsü ![](./media/mca-section-invoice/mca-select-profile.png)](./media/mca-section-invoice/mca-select-profile-zoomed-in.png#lightbox)

4. Sol bölmeden **Fatura bölümleri** ' ni seçin ve ardından sayfanın en üstünden **Ekle** ' yi seçin.

   [Fatura bölümleri eklemeyi gösteren ekran görüntüsünü ![](./media/mca-section-invoice/mca-list-invoice-sections.png)](./media/mca-section-invoice/mca-list-invoice-sections-zoomed-in.png#lightbox)

5. Fatura bölümü için bir ad girin. 

   [Fatura bölümü oluşturma sayfasını gösteren ![ekran görüntüsü](./media/mca-section-invoice/mca-create-invoice-section.png)](./media/mca-section-invoice/mca-create-invoice-section-zoomed-in.png#lightbox)

6. **Oluştur**’u seçin.

## <a name="create-a-new-billing-profile"></a>Yeni bir faturalandırma profili oluşturun

Bir faturalandırma profili oluşturmak için, bir faturalandırma **hesabı sahibi** veya bir **faturalandırma hesabı katılımcısı**olmanız gerekir. Daha fazla bilgi için bkz. [faturalandırma hesabı için faturalandırma profillerini yönetme](understand-mca-roles.md#manage-billing-profiles-for-billing-account).

> [!IMPORTANT]
>
> Ek faturalandırma profillerinin oluşturulması, genel maliyetinizi etkileyebilir. Daha fazla bilgi için bkz. [yeni faturalandırma profilleri eklerken göz önünde bulundurmanız gerekenler](#things-to-consider-when-adding-new-billing-profiles).

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Portalda maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/mca-section-invoice/search-cmb.png)

3. Sol bölmeden **faturalandırma profilleri** ' ni seçin ve ardından sayfanın en üstünden **Ekle** ' yi seçin.

   [Faturalama profili listesini gösteren ekran görüntüsü ![](./media/mca-section-invoice/mca-list-profiles.png)](./media/mca-section-invoice/mca-list-profiles-zoomed-in.png#lightbox)

    > [!Note]
    >
    > Faturalandırma profili sayfasında Ekle düğmesini görmüyorsanız, bu özellik hesabınız için kullanılamaz. Şu anda yalnızca bir Microsoft temsilcisiyle çalışırken ayarlanmış olan hesaplarda kullanılabilir.

4. Formu doldurup **Oluştur**'a tıklayın.

   [Faturalama profili oluşturma sayfasını gösteren ![ekran görüntüsü](./media/mca-section-invoice/mca-add-profile.png)](./media/mca-section-invoice/mca-add-profile-zoomed-in.png#lightbox)

    |Alan  |Tanım  |
    |---------|---------|
    |Ad     | Azure portal faturalandırma profilini kolayca belirlemenize yardımcı olan bir görünen ad.  |
    |PO numarası    | İsteğe bağlı bir satınalma siparişi numarası. PO numarası, Faturalandırma profili için oluşturulan faturalardaki görüntülenir. |
    |Faturalandırma adresi   | Faturalandırma adresi Faturalandırma profili için oluşturulan faturalarda görüntülenir. |
    |E-posta faturası   | Bu Faturalandırma profilinin faturalarını e-posta ile almak için e-posta faturası kutusunu işaretleyin. Bunu kabul ediyorsanız, Azure portal faturaları görüntüleyip indirebilirsiniz.|

5. **Oluştur**’u seçin.

## <a name="link-charges-to-invoice-sections-and-billing-profiles"></a>Ücretleri fatura bölümlerine ve faturalandırma profillerine bağlama

Faturalandırma hesabınızı gereksinimlerinize göre özelleştirdikten sonra, istediğiniz fatura bölümüne ve faturalandırma profilinize abonelikleri ve diğer ürünleri bağlayabilirsiniz.

### <a name="link-a-new-subscription"></a>Yeni bir abonelik bağla

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

2. **Abonelikler** araması yapın.

   [Portalda abonelik için arama ' yı gösteren ekran görüntüsünü ![](./media/mca-section-invoice/search-subscriptions.png)](./media/mca-section-invoice/search-subscriptions.png#lightbox)

3. Sayfanın üst kısmında **Ekle**’yi seçin.

   ![Abonelikler görünümünde Ekle düğmesini gösteren ekran görüntüsü](./media/mca-section-invoice/subscription-add.png)

4. Birden çok faturalandırma hesabına erişiminiz varsa, Microsoft Müşteri Sözleşmesi Faturalandırma hesabınızı seçin.

   ![Abonelikler görünümünde Ekle düğmesini gösteren ekran görüntüsü](./media/mca-section-invoice/mca-create-azure-subscription.png)

5. Aboneliğin kullanımı için faturalandırılacak faturalandırma profilini seçin. Bu abonelik için Azure kullanımı ve diğer satın alma ücretleri, seçilen faturalandırma profilinin faturasına faturalandırılır.

6. Abonelik ücretini bağlamak için fatura bölümünü seçin. Ücretler, Faturalandırma profili faturasında Bu bölüm altında görüntülenir.

7. Bir Azure planı seçin ve aboneliğiniz için kolay bir ad girin. 

9. **Oluştur**'a tıklayın.  

### <a name="link-existing-subscriptions-and-products"></a>Mevcut abonelikleri ve ürünleri bağlayın

Mevcut Azure abonelikleriniz veya Azure Marketi ve uygulama kaynak kaynakları gibi diğer ürünler varsa, maliyetlerinizi yeniden düzenlemek için bunları mevcut fatura bölümünden başka bir fatura bölümüne taşıyabilirsiniz. 

> [!IMPORTANT]
>
> Abonelikler ve diğer ürünler yalnızca aynı faturalandırma profiline ait olan fatura bölümleri arasında taşınabilir. Farklı faturalandırma profillerindeki fatura bölümleri arasında abonelikler ve ürünlerin taşınması desteklenmez.

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Portalda abonelik aramasını gösteren ekran görüntüsü](./media/mca-section-invoice/search-cmb.png)

3. Aboneliği yeni bir faturaya bağlamak için ekranın sol tarafındaki **Azure abonelikleri** ' ni seçin. Azure Marketi ve uygulama kaynak kaynakları gibi diğer ürünler için **Yinelenen ücretler**' i seçin.

   [Fatura bölümünü değiştirme seçeneğini gösteren ekran görüntüsünü ![](./media/mca-section-invoice/mca-select-change-invoice-section.png)](./media/mca-section-invoice/mca-select-change-invoice-section.png#lightbox)

4. Sayfasında, yeni bir faturaya bağlamak istediğiniz abonelik veya ürün için üç nokta (üç nokta) simgesine tıklayın. **Fatura Değiştir bölümünü**seçin.

5. Açılan listeden yeni fatura bölümünü seçin. Aşağı açılan pencerede yalnızca mevcut fatura profiliyle ilişkili fatura bölümleri görüntülenir.

    [Yeni bir fatura seçme bölümünün gösterildiği ekran görüntüsünü ![](./media/mca-section-invoice/mca-select-new-invoice-section.png)](./media/mca-section-invoice/mca-select-new-invoice-section-zoomed-in.png#lightbox)

6. **Kaydet**’i seçin.

## <a name="things-to-consider-when-adding-new-billing-profiles"></a>Yeni faturalandırma profilleri eklerken göz önünde bulundurmanız gerekenler

### <a name="azure-usage-charges-may-be-impacted"></a>Azure kullanım ücretleri etkilenebilir

Microsoft Müşteri Sözleşmesi için faturalandırma hesabınızda, Azure kullanımı her faturalandırma profili için aylık olarak toplanır. Katmanlı fiyatlandırmayla Azure kaynakları için fiyatlar, her faturalandırma profilinin kullanımına göre belirlenir. Fiyat hesaplanırken, kullanım faturalandırma profilleri arasında toplanmaz. Bu, birden çok Faturalandırma profili içeren hesaplara yönelik olarak Azure kullanımının genel maliyetini etkileyebilir.

Maliyetlerin iki senaryo için nasıl değişeceğini gösteren bir örneğe bakalım. Senaryolarda kullanılan fiyatlar yalnızca örnek amaçlıdır ve Azure hizmetlerinin gerçek fiyatlarını temsil etmez.

#### <a name="you-only-have-one-billing-profile"></a>Yalnızca bir faturalandırma profiliniz vardır.

Azure blok blob depolamayı kullandığınızı varsayalım. Bu, ilk 50 terabayt (TB) için GB başına ABD Doları, sonra da GB başına 00184, sonraki 450 terabayt (TB) için. 00177. Faturalandırma profilinize faturalandırılan aboneliklerde 100 TB kullandınız. Bu, ücretlendirilebilecek kadarını aşağıda bulabilirsiniz.

|  Katman fiyatlandırması (USD) |Miktar | Tutar (USD)|
|---------|---------|---------|
|ilk 50 TB/ay için TB başına 1,84    | 50 TB        | 92,0   |
|sonraki 450 TB/ay için 1,77 TB    |  50 TB         | 88,5   |
|Toplam     |     100 TB  | 180,5

Bu senaryoda 100 TB veri kullanmanın toplam ücreti **180,5** ' dir

#### <a name="you-have-multiple-billing-profiles"></a>Birden çok faturalandırma profiliniz var.

Şimdi başka bir faturalandırma profili oluşturduğunuzu ve ikinci faturalandırma profiline faturalandırılan abonelikler aracılığıyla ilk faturalandırma profiline ve 50 GB 'a faturalandırılan abonelikler aracılığıyla 50 GB kullandığınızı varsayalım.

`Charges for the first billing profile`

|  Katman fiyatlandırması (USD) |Miktar | Tutar (USD)|
|---------|---------|---------|
|ilk 50 TB/ay için TB başına 1,84    | 50 TB        | 92,0  |
|sonraki 450 TB/ay için 1,77 TB    |  0 TB         | 0.0  |
|Toplam     |     50 TB  | 92,0 

`Charges for the second billing profile`

|  Katman fiyatlandırması (USD) |Miktar | Tutar (USD)|
|---------|---------|---------|
|ilk 50 TB/ay için TB başına 1,84    | 50 TB        | 92,0  |
|sonraki 450 TB/ay için 1,77 TB    |  0 TB         | 0.0  |
|Toplam     |     50 TB  | 92,0 

Bu senaryoda 100 TB veri kullanmanın toplam ücreti **184,0** ' dir (92,0 * 2).

### <a name="azure-reservation-benefits-might-not-apply-to-all-subscriptions"></a>Azure rezervasyon avantajları tüm abonelikler için uygulanmayabilir

Paylaşılan kapsama sahip Azure ayırmaları, tek bir faturalandırma profilindeki aboneliklere uygulanır ve faturalandırma profilleri arasında paylaşılmaz. 

![Farklı faturalandırma hesabı yapısına yönelik rezervasyon uygulaması için bilgi grafiği](./media/mca-section-invoice/mca-reservations-benefits-by-bg.png)

Yukarıdaki görüntüde contoso 'nun iki aboneliği vardır. Azure ayırma avantajı, faturalandırma hesabının nasıl yapılandırıldığına bağlı olarak farklı şekilde uygulanır. Soldaki senaryoda, ayırma avantajı, her iki aboneliğe de mühendislik faturalandırma profiline faturalandırılır. Sağdaki senaryoda, ayırma avantajı yalnızca, mühendislik faturalama profiline faturalandırılan tek abonelik olduğundan, abonelik 1 ' e uygulanır. 

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi’ne erişimi denetleme
[!INCLUDE [billing-check-mca](../../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Müşteri Sözleşmesi için ek Azure aboneliği oluşturma](create-subscription.md)
- [Azure portalında faturalama rollerini yönetme](understand-mca-roles.md#manage-billing-roles-in-the-azure-portal)
- [Diğer ödeme hesaplarındaki kullanıcılardan Azure aboneliklerinin fatura sahipliğini alma](mca-request-billing-ownership.md)
