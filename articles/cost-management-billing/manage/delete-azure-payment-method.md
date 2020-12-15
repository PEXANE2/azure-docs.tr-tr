---
title: Azure fatura ödeme yöntemini silme
description: Azure aboneliği tarafından kullanılan bir ödeme yönteminin nasıl silineceğini açıklar.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 10/07/2020
ms.author: banders
ms.openlocfilehash: 15d6c7731b541de638ceaf7828a7ce962cbf154a
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91827544"
---
# <a name="delete-an-azure-billing-payment-method"></a>Azure fatura ödeme yöntemini silme

Bu belge, farklı türlerdeki Azure aboneliklerinden kredi kartı gibi bir ödeme yöntemini silmenize yardımcı olmak için yönergeler sağlar. Aşağıdakiler için bir ödeme yöntemini silebilirsiniz:

- Microsoft Müşteri Sözleşmesi (MCA)
- Microsoft Online Services Programı (MOSP), ayrıca kullandıkça öde olarak da adlandırılır

Azure abonelik türünüz ne olursa olsun, ilişkili ödeme yöntemini silebilmeniz için bunu iptal etmeniz gerekir.

Microsoft İş Ortağı Sözleşmesi ve Kurumsal Anlaşma gibi diğer Azure abonelik türleri için ödeme yönteminin kaldırılması desteklenmez.

## <a name="delete-an-mca-payment-method"></a>MCA ödeme yöntemini silme

Yalnızca Microsoft Müşteri Sözleşmesi hesabını oluşturan kullanıcı ödeme yöntemini silebilir.

Microsoft Müşteri Sözleşmesi için ödeme yöntemini silmek üzere aşağıdaki adımları uygulayın.

1. https://portal.azure.com/ adresinden Azure portalında oturum açın.
1. **Maliyet Yönetimi ve Faturalama** bölümüne gidin.
1. Gerekirse, bir faturalama kapsamı seçin.
1. Sol menü listesinde, **Faturalandırma** altındaki **Faturalama profilleri**’ni seçin.  
    :::image type="content" source="./media/delete-azure-payment-method/billing-profiles.png" alt-text="Azure portalda Faturalama profillerini gösteren örnek ekran görüntüsü" lightbox="./media/delete-azure-payment-method/billing-profiles.png" :::
1. Faturalama profilleri listesinde, ödeme yönteminin kullanıldığı yeri seçin.  
    :::image type="content" source="./media/delete-azure-payment-method/select-billing-profile.png" alt-text="Faturalama profillerinin listesini gösteren örnek resim" :::
1. Sol menü listesinde, **Ayarlar** altında **Ödeme yöntemleri**’ni seçin.
1. Faturalama profilinizin ödeme yöntemleri sayfasında, **Kredi kartlarınız** bölümünde bir ödeme yöntemleri tablosu gösterilir. Silmek istediğiniz kredi kartını bulun, üç noktayı ( **…** ) seçin ve ardından **Sil**’i seçin.  
    :::image type="content" source="./media/delete-azure-payment-method/delete-credit-card.png" alt-text="Kredi kartının silineceği yeri gösteren örnek" :::
1. Ödeme yöntemini sil sayfası görüntülenir. Azure, ödeme yönteminin kullanımda olup olmadığını denetler.
    - Ödeme yöntemi kullanılmıyorsa, **Sil** seçeneği etkinleştirilir. Kredi kartı bilgilerini silmek için bunu seçin.
    - Ödeme yöntemi kullanılıyorsa, değiştirilmeli veya ayrılmalıdır. Aşağıdaki bölümleri okumaya devam edin. Bu bölümlerde, aboneliğiniz tarafından kullanılan ödeme yönteminin nasıl **ayrılacağı** açıklanmaktadır.

### <a name="detach-payment-method-used-by-an-mca-billing-profile"></a>MCA faturalama profili tarafından kullanılan ödeme yöntemini ayırma

Ödeme yönteminiz bir MCA faturalama profili tarafından kullanılıyorsa aşağıdaki örneğe benzer bir ileti görürsünüz.

:::image type="content" source="./media/delete-azure-payment-method/payment-method-in-use-microsoft-customer-agreement.png" alt-text="Bir ödeme yönteminin bir Microsoft Müşteri Sözleşmesi tarafından kullanılmakta olduğunu gösteren örnek resim" :::

Ödeme yöntemini ayırmak için bir dizi koşul karşılanmalıdır. Herhangi bir koşul karşılanmıyorsa koşulun nasıl karşılanacağını açıklayan yönergeler görüntülenir. Sorunu çözebileceğiniz konuma götüren bir bağlantı da görüntülenir.

Koşulların tümü karşılandığında, ödeme yöntemini faturalama profilinden ayırabilirsiniz.

> [!NOTE]
> Varsayılan ödeme yöntemi ayrılmışsa faturalama profili _etkin değil_ durumuna getirilir. Bu süreçte silinen hiçbir şey kurtarılamaz. Bir faturalama profili devre dışı olarak ayarlandıktan sonra, yeni kaynaklar oluşturmak için yeni bir Azure aboneliğine kaydolmanız gerekir.

#### <a name="to-detach-a-payment-method"></a>Ödeme yöntemini ayırmak için

1. Ödeme yöntemini sil alanında, **Geçerli ödeme yöntemini ayırın** bağlantısını seçin.
1. Tüm koşullar karşılanıyorsa **Ayır**’ı seçin. Aksi halde, bir sonraki adıma devam edin.
1. Ayır seçeneği kullanılamıyorsa koşulların bir listesi gösterilir. Listelenen eylemleri gerçekleştirin. Varsayılan ödeme yöntemini ayır alanında gösterilen bağlantıyı seçin. Aşağıda, gerçekleştirmeniz gereken eylemleri açıklayan bir düzeltme eylemi örneği verilmiştir.  
    :::image type="content" source="./media/delete-azure-payment-method/azure-subscriptions.png" alt-text="MCA için ödeme yöntemini ayırmaya yönelik gereken düzeltici eylemi gösteren örnek" :::
1. Düzeltici eylem bağlantısını seçtiğinizde, işlem yaptığınız Azure sayfasına yönlendirilirsiniz. Gereken düzeltici eylemi gerçekleştirin.
1. Gerekirse, tüm diğer düzeltici eylemleri gerçekleştirin.
1. **Maliyet Yönetimi + Faturalandırma** > **Faturalama profilleri** > **Ödeme yöntemleri**’ne geri gidin. **Ayır**’ı seçin. Varsayılan ödeme yöntemini ayır sayfasının alt kısmında **Ayır**’ı seçin.

> [!NOTE]
> - Aboneliği iptal ettikten sonra, aboneliğin silinmesi 90 güne kadar sürebilir. Bekleme süresini kısaltmak istiyorsanız, bir Azure destek isteği açın ve aboneliğin hemen silinmesini isteyin.
> - Ödeme yöntemini, yalnızca faturalama profili için önceki tüm ücretler kapatıldıktan sonra silebilirsiniz. Etkin bir faturalama dönemindeyseniz ödeme yönteminizi silmek için faturalama döneminin sonuna kadar beklemeniz gerekir. **Faturalama döneminizin bitmesini beklerken diğer tüm ayırma koşullarının karşılandığından emin olun**.

## <a name="delete-a-mosp-payment-method"></a>MOSP ödeme yöntemini silme

Ödeme yöntemini silmek için bir hesap yöneticisi olmanız gerekir.

Ödeme yönteminiz bir MOSP aboneliği tarafından kullanılıyorsa, aşağıdaki adımları uygulayın.

1. https://portal.azure.com/ adresinden Azure portalında oturum açın.
1. **Maliyet Yönetimi ve Faturalama** bölümüne gidin.
1. Gerekirse, bir faturalama kapsamı seçin.
1. Sol menü listesinde, **Faturalandırma** altında **Ödeme yöntemleri**’ni seçin.
1. Ödeme yöntemleri alanında, ödeme yönteminizin bulunduğu _satırı_ seçin. Ödeme yöntemi bağlantısını seçmeyin. Ödeme yöntemini seçtiğinizi belirten görsel bir gösterge olmayabilir.
1. **Sil**’i seçin.  
    :::image type="content" source="./media/delete-azure-payment-method/delete-mosp-payment-method.png" alt-text="MOSP için ödeme yöntemini ayırmaya yönelik gereken düzeltme eylemini gösteren örnek" :::
1. Ödeme yöntemini sil alanında, tüm koşullar karşılanıyorsa **Sil**’i seçin. Sil seçeneği kullanılamıyorsa bir sonraki adıma ilerleyin.
1. Koşulların bir listesi gösterilir. Listelenen eylemleri gerçekleştirin. Ödeme yöntemini sil alanında gösterilen bağlantıyı seçin.  
    :::image type="content" source="./media/delete-azure-payment-method/payment-method-in-use-mosp.png" alt-text="Ödeme yönteminin bir MOSP aboneliği tarafından kullanılmakta olduğunu gösteren örnek resim" :::
1. Düzeltici eylem bağlantısını seçtiğinizde, işlem yaptığınız Azure sayfasına yönlendirilirsiniz. Gereken düzeltici eylemi gerçekleştirin.
1. Gerekirse, tüm diğer düzeltici eylemleri gerçekleştirin.
1. **Maliyet Yönetimi + Faturalandırma** > **Faturalama profilleri** > **Ödeme yöntemleri**’ne geri gidin ve ödeme yöntemini silin.

> [!NOTE]
> Aboneliği iptal ettikten sonra, aboneliğin silinmesi 90 güne kadar sürebilir. Bekleme süresini kısaltmak istiyorsanız, bir Azure destek isteği açın ve aboneliğin hemen silinmesini isteyin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure aboneliğinizi iptal etme hakkında daha fazla bilgi için bkz. [Azure aboneliğinizi iptal etme](cancel-azure-subscription.md).
- Kredi kartı ekleme veya güncelleştirme hakkında daha fazla bilgi için bkz. [Azure için kredi kartı ekleme veya güncelleştirme](change-credit-card.md).