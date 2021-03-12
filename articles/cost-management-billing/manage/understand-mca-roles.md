---
title: Microsoft Müşteri Sözleşmeleri için faturalama rolleri - Azure
description: Azure'da Microsoft Müşteri Sözleşmeleri için ödeme hesaplarının faturalama rollerini öğrenin.
author: amberbhargava
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 03/10/2021
ms.author: banders
ms.openlocfilehash: e334a423fd11aa3a357d52099a792dcc905aedeb
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103011672"
---
# <a name="understand-microsoft-customer-agreement-administrative-roles-in-azure"></a>Azure’daki Microsoft Müşteri Sözleşmesi yönetici rollerini anlama

Microsoft Müşteri Sözleşmesi ödeme hesabını yönetmek için aşağıdaki bölümlerde açıklanan rolleri kullanın. Bu roller kaynaklara erişimi denetlemek için Azure'daki yerleşik rollere eklenmiştir. Daha fazla bilgi için bkz. [Azure yerleşik rolleri](../../role-based-access-control/built-in-roles.md).

Bu makale, Microsoft Müşteri Sözleşmesi’ne ilişkin ödeme hesabı için geçerlidir. [Microsoft Müşteri Sözleşmesi’ne erişiminiz olup olmadığını denetleyin](#check-access-to-a-microsoft-customer-agreement).

Microsoft Müşteri Sözleşmesi (MCA) Faturalandırma hesabınıza erişimi nasıl denetleyebileceğinizi öğrenmek için, [MCA faturalandırma hesabınıza yönelik erişimi yönetme](https://www.youtube.com/watch?v=9sqglBlKkho) videosunu izleyin.

>[!VIDEO https://www.youtube.com/embed/9sqglBlKkho]

## <a name="billing-role-definitions"></a>Faturalama rolü tanımları

Aşağıdaki tabloda ödeme hesabınızı, faturalama profillerinizi ve fatura bölümlerinizi yönetirken kullandığınız faturalama rolleri açıklanır.

|Rol|Açıklama|
|---|---|
|Faturalama hesabı sahibi|Ödeme hesabı için her şeyi yönetme|
|Faturalama hesabı katkıda bulunanı|Faturalandırma hesabındaki izinler dışında her şeyi yönetme|
|Faturalama hesabı okuyucusu|Faturalandırma hesabındaki her şeyin salt okunur görünümü|
|Faturalama profili sahibi|Faturalama profili için her şeyi yönetme|
|Faturalama profili katkıda bulunanı|Faturalandırma profilindeki izinler dışında her şeyi yönetme|
|Faturalama profili okuyucusu|Faturalandırma profilindeki her şeyin salt okunur görünümü|
|Fatura yöneticisi|Faturalandırma profili için faturaları görüntüleme ve ödeme|
|Fatura bölümü sahibi|Fatura bölümündeki her şeyi yönetme|
|Fatura bölümü katkıda bulunanı|Fatura bölümündeki izinler dışında her şeyi yönetme|
|Fatura bölümü okuyucusu|Fatura bölümündeki her şeyin salt okunur görünümü|
|Azure aboneliği oluşturucusu|Azure abonelikleri oluşturma|

## <a name="billing-account-roles-and-tasks"></a>Ödeme hesabı rolleri ve görevleri

Azure’ı kullanmak için kaydolduğunuzda bir ödeme hesabı oluşturulur. Faturaları, ödemeleri yönetmek ve maliyetleri izlemek için ödeme hesabınızı kullanırsınız. Faturalandırma hesabındaki roller en yüksek izin düzeyine sahiptir ve bu rollerdeki kullanıcılar, tüm hesabınız için maliyet ve fatura bilgilerine ilişkin görünürlük alır. Bu rolleri yalnızca faturaları görüntülemesi gereken kullanıcılara atayın ve finans ve muhasebe takımlarının üyesi gibi tüm hesabınız için maliyetleri izleyin. Daha fazla bilgi için bkz. [Ödeme hesabını anlama](../understand/mca-overview.md#your-billing-account).

Aşağıdaki tablolarda ödeme hesabı bağlamında görevleri tamamlamak için ihtiyacınız olan roller gösterilir.

### <a name="manage-billing-account-permissions-and-properties"></a>Ödeme hesabı izinlerini ve özelliklerini yönetme

|Görev|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu|
|---|---|---|---|
|Faturalandırma hesabı için rol atamalarını görüntüle|✔|✔|✔|
|Diğer kullanıcılara ödeme hesabını görüntüleme ve yönetme izinleri verme|✔|✘|✘|
|Adres, anlaşmalar ve daha fazlası gibi faturalandırma hesabı özelliklerini görüntüleyin|✔|✔|✔|
|Satım, görünen ad ve daha fazlası gibi faturalandırma hesabı özelliklerini güncelleştirin|✔|✔|✘|

### <a name="manage-billing-profiles-for-billing-account"></a>Ödeme hesabı için faturalama profillerini yönetme

|Görev|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu|
|---|---|---|---|
|Hesap için tüm faturalandırma profillerini görüntüleyin|✔|✔|✔|
|Yeni faturalandırma profilleri oluşturma|✔|✔|✘|

### <a name="manage-invoices-for-billing-account"></a>Ödeme hesabı için faturaları yönetme

|Görev|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu|
|---|---|---|---|
|Hesap için tüm faturaları görüntüleyin|✔|✔|✔|
|Kredi kartıyla fatura ödeyin|✔|✔|✘|
|Faturaları, Azure kullanım dosyalarını, Fiyat sayfalarını ve vergi belgelerini indirin|✔|✔|✔|

### <a name="manage-products-for-billing-account"></a>Faturalandırma hesabı için ürünleri yönetme

|Görev|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu|
|---|---|---|---|
|Hesap için satın alınan tüm ürünleri görüntüleyin|✔|✔|✔|
|İptal, otomatik yenilemeyi devre dışı bırakma gibi ürünler için faturalandırmayı yönetin ve daha fazlasını yapın|✔|✔|✘|

### <a name="manage-subscriptions-for-billing-account"></a>Ödeme hesabı için abonelikleri yönetme

|Görev|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu|
|---|---|---|---|
|Faturalandırma hesabı için oluşturulan tüm Azure aboneliklerini görüntüleyin|✔|✔|✔|
|Yeni Azure abonelikleri oluşturma|✔|✔|✘|
|Azure aboneliklerini iptal et|✘|✘|✘|

## <a name="billing-profile-roles-and-tasks"></a>Faturalama profili rolleri ve görevleri

Her faturalandırma hesabının en az bir faturalandırma profili vardır. Azure 'ı kullanmaya kaydolduğunuzda ilk faturalandırma profiliniz ayarlanır. Fatura profili için aylık bir fatura oluşturulur ve önceki aydan tüm ilişkili ücretleri içerir. Gereksinimlerinize göre daha fazla faturalandırma profili ayarlayabilirsiniz. Bir faturalandırma profilinde rollere sahip kullanıcılar maliyeti görüntüleyebilir, bütçeyi ayarlayabilir ve faturalarını yönetebilir ve ödeyebilirsiniz. Bu rolleri, kuruluşunuzda iş yönetim takımlarının üyeleri gibi Faturalandırma profili için bütçeyi yönetmekten sorumlu olan kullanıcılara atayın. Daha fazla bilgi için bkz. [Faturalama profillerini anlama](../understand/mca-overview.md#billing-profiles).

Aşağıdaki tablolarda faturalama profili bağlamında görevleri tamamlamak için ihtiyacınız olan roller gösterilir.

### <a name="manage-billing-profile-permissions-properties-and-policies"></a>Faturalama profili izinlerini, özelliklerini ve ilkelerini yönetme

|Görev|Faturalama profili sahibi|Faturalama profili katkıda bulunanı|Faturalama profili okuyucusu|Fatura yöneticisi|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Faturalandırma profili için rol atamalarını görüntüle|✔|✔|✔|✔|✔|✔|✔|
|Diğer kullanıcılara faturalama profilini görüntüleme ve yönetme izinleri verme|✔|✘|✘|✘|✔|✘|✘|
|PO numarası, faturanız ve daha fazlası gibi Faturalandırma profili özelliklerini görüntüleyin|✔|✔|✔|✔|✔|✔|✔|
|Faturalama profili özelliklerini güncelleştirme |✔|✔|✘|✘|✔|✔|✘|
|Azure rezervasyon satın alımları, Azure Market satın alımları ve daha fazlası gibi faturalandırma profilinde uygulanan ilkeleri görüntüleyin|✔|✔|✔|✔|✔|✔|✔|
|Faturalama profiline ilkeleri uygulama |✔|✔|✘|✘|✔|✔|✘|

### <a name="manage-invoices-for-billing-profile"></a>Faturalama profili için faturaları yönetme

|Görev|Faturalama profili sahibi|Faturalama profili katkıda bulunanı|Faturalama profili okuyucusu|Fatura yöneticisi|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Faturalama profilinin tüm faturalarını görüntüleme|✔|✔|✔|✔|✔|✔|✔|
|Kredi kartıyla fatura ödeyin|✔|✔|✘|✔|✔|✘|✘|
|Faturalama profili için faturaları, Azure kullanımı ve ücretleri dosyalarını, fiyat listelerini ve vergi belgelerini indirme|✔|✔|✔|✔|✔|✔|✔|

### <a name="manage-invoice-sections-for-billing-profile"></a>Faturalama profili için fatura bölümlerini yönetme

|Görev|Faturalama profili sahibi|Faturalama profili katkıda bulunanı|Faturalama profili okuyucusu|Fatura yöneticisi|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Faturalama profili için tüm fatura bölümlerini görüntüleme|✔|✔|✔|✔|✔|✔|✔|
|Faturalama profili için yeni fatura bölümü oluşturma|✔|✔|✘|✘|✔|✔|✘|

### <a name="manage-products-for-billing-profile"></a>Faturalandırma profili için ürünleri yönetme

|Görev|Faturalama profili sahibi|Faturalama profili katkıda bulunanı|Faturalama profili okuyucusu|Fatura yöneticisi|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Faturalandırma profili için satın alınan tüm ürünleri görüntüleyin|✔|✔|✔|✔|✔|✔|✔|
|İptal, otomatik yenilemeyi devre dışı bırakma gibi ürünler için faturalandırmayı yönetin ve daha fazlasını yapın|✔|✔|✘|✘|✔|✔|✘|
|Ürünlerin faturalandırma profilini değiştirme|✔|✔|✘|✘|✔|✔|✘|

### <a name="manage-payment-methods-for-billing-profile"></a>Faturalama profili için ödeme yöntemlerini yönetme

|Görev|Faturalama profili sahibi|Faturalama profili katkıda bulunanı|Faturalama profili okuyucusu|Fatura yöneticisi|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Faturalama profili için ödeme yöntemlerini görüntüleme|✔|✔|✔|✔|✔|✔|✔|
|Kredi kartını değiştirme, kredi kartını ayırma ve daha fazlası gibi ödeme yöntemlerini yönetin|✔|✔|✘|✘|✔|✔|✘|
|Faturalandırma profili için Azure kredi bakiyesini izleme|✔|✔|✔|✔|✔|✔|✔|

### <a name="manage-subscriptions-for-billing-profile"></a>Faturalama profili için abonelikleri yönetme

|Görev|Faturalama profili sahibi|Faturalama profili katkıda bulunanı|Faturalama profili okuyucusu|Fatura yöneticisi|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Faturalama profili için tüm Azure aboneliklerini görüntüleme|✔|✔|✔|✔|✔|✔|✔|
|Yeni Azure abonelikleri oluşturma|✔|✔|✘|✘|✔|✔|✘|
|Azure aboneliklerini iptal et|✘|✘|✘|✘|✘|✘|✘|
|Azure abonelikleri için faturalandırma profilini değiştirme|✔|✔|✘|✘|✔|✔|✘|

## <a name="invoice-section-roles-and-tasks"></a>Fatura bölümü rolleri ve görevleri

Her faturalandırma profili varsayılan olarak bir fatura bölümü içerir. Faturalandırma profilinin faturasında maliyeti gruplamak için daha fazla fatura bölümü oluşturabilirsiniz.  Bir fatura bölümünde rollere sahip olan kullanıcılar, kimlerin Azure abonelikleri oluşturduğunu ve diğer satınalmaları oluşturduğunu denetleyebilir. Bu rolleri, kuruluşumuzdaki takımlar için Azure ortamını ayarlayan mühendislik liderleri ve teknik mimarlar gibi kullanıcılara atayın. Daha fazla bilgi için bkz. [Fatura bölümünü anlama](../understand/mca-overview.md#invoice-sections).

Aşağıdaki tablolarda fatura bölümleri bağlamında görevleri tamamlamak için ihtiyacınız olan roller gösterilir.

### <a name="manage-invoice-section-permissions-and-properties"></a>Fatura bölümü izinlerini ve özelliklerini yönetme

|Görevler|Fatura bölümü sahibi|Fatura bölümü katkıda bulunanı|Fatura bölümü okuyucusu|Azure aboneliği oluşturucusu|Faturalama profili sahibi|Faturalama profili katkıda bulunanı|Faturalama profili okuyucusu |Fatura yöneticisi|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu 
|---|---|---|---|---|---|---|---|---|---|---|---|
|Fatura için rol atamalarını görüntüleyin bölümü|✔|✔|✔|✘|✔|✔|✔|✔|✔|✔|✔|
|Diğer kullanıcılara fatura bölümünü görüntüleme ve yönetme izinleri verme|✔|✘|✘|✘|✔|✘|✘|✘|✔|✘|✘|
|Fatura bölümü özelliklerini görüntüleme|✔|✔|✔|✘|✔|✔|✔|✔|✔|✔|✔|
|Fatura bölümü özelliklerini güncelleştirme|✔|✔|✘|✘|✔|✔|✘|✘|✔|✔|✘|

### <a name="manage-products-for-invoice-section"></a>Fatura bölümü için ürünleri yönetme

|Görevler|Fatura bölümü sahibi|Fatura bölümü katkıda bulunanı|Fatura bölümü okuyucusu|Azure aboneliği oluşturucusu|Faturalama profili sahibi|Faturalama profili katkıda bulunanı|Faturalama profili okuyucusu |Fatura yöneticisi|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu 
|---|---|---|---|---|---|---|---|---|---|---|---|
|Fatura bölümü için satın alınan tüm ürünleri görüntüleyin|✔|✔|✔|✘|✔|✔|✔|✔|✔|✔|✔|
|İptal, otomatik yenilemeyi devre dışı bırakma gibi ürünler için faturalandırmayı yönetin ve daha fazlasını yapın|✔|✔|✘|✘|✔|✔|✘|✘|✔|✔|✘|
|Ürünler için fatura bölümünü değiştirme|✔|✔|✘|✘|✔|✔|✘|✘|✔|✔|✘|

### <a name="manage-subscriptions-for-invoice-section"></a>Fatura bölümü için abonelikleri yönetme

|Görevler|Fatura bölümü sahibi|Fatura bölümü katkıda bulunanı|Fatura bölümü okuyucusu|Azure aboneliği oluşturucusu|Faturalama profili sahibi|Faturalama profili katkıda bulunanı|Faturalama profili okuyucusu |Fatura yöneticisi|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu 
|---|---|---|---|---|---|---|---|---|---|---|---|
|Fatura bölümü için tüm Azure aboneliklerini görüntüleme|✔|✔|✔|✘|✔|✔|✔|✔|✔|✔|✔|
|Azure abonelikleri oluşturma|✔|✔|✘|✔|✔|✔|✘|✘|✔|✔|✘|
|Azure aboneliklerini iptal et|✘|✘|✘|✘|✘|✘|✘|✘|✘|✘|✘|
|Azure aboneliğinin fatura bölümünü Değiştir|✔|✔|✘|✘|✔|✔|✘|✘|✔|✔|✘|
|Diğer ödeme hesaplarındaki kullanıcılardan aboneliklerin fatura sahipliğini isteme|✔|✔|✘|✘|✔|✔|✘|✘|✔|✔|✘|

## <a name="subscription-billing-roles-and-tasks"></a>Abonelik faturalama rolleri ve görevleri

Aşağıdaki tabloda abonelik bağlamında görevleri tamamlamak için ihtiyacınız olan roller gösterilir.

|Görevler|Fatura bölümü sahibi|Fatura bölümü katkıda bulunanı|Fatura bölümü okuyucusu|Azure aboneliği oluşturucusu|Faturalama profili sahibi|Faturalama profili katkıda bulunanı|Faturalama profili okuyucusu |Fatura yöneticisi|Faturalama hesabı sahibi|Faturalama hesabı katkıda bulunanı|Faturalama hesabı okuyucusu 
|---|---|---|---|---|---|---|---|---|---|---|---|
|Abonelik oluşturma|✔|✔|✘|✔|✔|✔|✘|✘|✔|✔|✘|
|Abonelik için maliyet merkezini güncelleştirme|✔|✔|✘|✘|✔|✔|✘|✘|✔|✔|✘|
|Abonelik için fatura bölümünü değiştirme|✔|✔|✘|✘|✔|✔|✘|✘|✔|✔|✘|
|Abonelik için faturalandırma profilini değiştirme|✘|✘|✘|✘|✔|✔|✘|✘|✔|✔|✘|
|Azure aboneliklerini iptal et|✘|✘|✘|✘|✘|✘|✘|✘|✘|✘|✘|

## <a name="manage-billing-roles-in-the-azure-portal"></a>Azure portalında yerleşik rolleri yönetme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Azure portalı aramasını gösteren ekran görüntüsü](./media/understand-mca-roles/billing-search-cost-management-billing.png)

3. Erişim vermek istediğiniz ödeme hesabı, faturalama profili veya fatura bölümü gibi bir kapsamda **Erişim denetimi (IAM)** öğesini seçin.

4. Erişim denetimi (IAM) sayfasında söz konusu kapsam için her role atanan kullanıcılar ve gruplar listelenir.

   ![Ödeme hesabı için yöneticilerin listesini gösteren ekran görüntüsü](./media/understand-mca-roles/billing-list-admins.png)

5. Kullanıcıya erişim vermek için sayfanın en üstündeki **Ekle**'yi seçin. Rol açılan listesinde rolü seçin. Erişim izni vermek istediğiniz kullanıcının e-posta adresini girin. Rolü atamak için **Kaydet**’i seçin.

   ![Ödeme hesabına yönetici ekleme işlemini gösteren ekran görüntüsü](./media/understand-mca-roles/billing-add-admin.png)

6. Kullanıcının erişimini kaldırmak için, kaldırmak istediğiniz rol atamasına sahip kullanıcıyı seçin. Kaldır’ı seçin.

   ![Ödeme hesabından yönetici kaldırma işlemini gösteren ekran görüntüsü](./media/understand-mca-roles/billing-remove-admin.png)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi’ne erişimi denetleme
[!INCLUDE [billing-check-mca](../../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun
Yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Sonraki adımlar

Ödeme hesabınız hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Microsoft Müşteri Sözleşmesi için ödeme hesabınızı kullanmaya başlama](../understand/mca-overview.md)
- [Microsoft Müşteri Sözleşmesi için ödeme hesabınıza yönelik bir Azure aboneliği oluşturma](create-subscription.md)
