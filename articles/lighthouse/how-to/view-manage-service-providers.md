---
title: Hizmet sağlayıcılarını görüntüleme ve yönetme
description: Müşteriler, hizmet sağlayıcıları, hizmet sağlayıcı teklifleri ve devredilen kaynaklar hakkındaki bilgileri görüntülemek için Azure portalındaki Hizmet sağlayıcıları sayfasını kullanabilir.
ms.date: 02/25/2020
ms.topic: conceptual
ms.openlocfilehash: 94103c293ffa7ccfb9d7da0a237dc1b1c6540b72
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270634"
---
# <a name="view-and-manage-service-providers"></a>Hizmet sağlayıcılarını görüntüleme ve yönetme

Müşteriler, hizmet sağlayıcılar ve hizmet sağlayıcı teklifleri hakkındaki bilgileri görüntülemek, [Azure temsilci kaynak yönetimi](../concepts/azure-delegated-resource-management.md)aracılığıyla belirli kaynakları devretmek ve yeni hizmet sağlayıcı teklifleri için alışveriş yapmak için Azure [portalındaki](https://portal.azure.com) **Hizmet sağlayıcıları** sayfasını kullanabilir. Burada hizmet sağlayıcılara ve müşterilere atıfta bulunsak da, birden çok kiracıyı yöneten işletmeler yönetim deneyimlerini pekiştirmek için aynı işlemi kullanabilir.

Azure portalındaki **Hizmet sağlayıcıları** sayfasına erişmek için müşteri **Tüm hizmetleri**seçebilir, ardından **Servis sağlayıcılarını** arayabilir ve seçebilir. Ayrıca, Azure portalının üst kısmındaki arama kutusuna "Hizmet sağlayıcıları" girerek de bulabilirler.

**Hizmet sağlayıcıları** sayfasının yalnızca Azure temsilci kaynak yönetimi aracılığıyla müşterinin aboneliklerine veya kaynak gruplarına erişimi olan hizmet sağlayıcıları hakkında bilgi gösterdiğini unutmayın. Bir müşteri, müşterinin kaynaklarına erişmek için Azure temsilci kaynak yönetimini kullanmayan ek hizmet sağlayıcılarla çalışıyorsa, bu hizmet sağlayıcıları hakkındaki bilgiler burada gösterilmez.

> [!NOTE]
> Hizmet sağlayıcıları, Azure portalında **müşterilerim** için gezinerek müşterileri hakkındaki bilgileri görüntüleyebilir. Daha fazla bilgi için [bkz.](view-manage-customers.md)

## <a name="view-service-provider-details"></a>Servis sağlayıcı ayrıntılarını görüntüleme

Müşteri, servis sağlayıcıları hakkındaki bilgileri görüntülemek için **Servis sağlayıcıları** sayfasının sol tarafındaki **Sağlayıcı tekliflerini** seçebilir.

Her hizmet sağlayıcı teklifi için müşteri, hizmet sağlayıcısının adını ve onunla ilişkili teklifi, müşterinin biniş işlemi sırasında girdiği adı görür.

**Temsilcilikler** sütununda müşteri, bu teklif için kaç abonelik ve/veya kaynak grubuna devredildiğini görür. Hizmet sağlayıcısı, bu aboneliklere ve/veya kaynak gruplarına teklifte belirtilen erişim düzeylerine göre erişebilir ve bunları yönetebilir.

## <a name="delegate-resources"></a>Temsilci kaynakları

Bir hizmet sağlayıcısının bir müşterinin kaynaklarına erişebilmeleri ve yönetmeden önce, bu kaynakların devredilmesi gerekir. Bir müşteri bir teklifi kabul etmiş ancak henüz herhangi bir kaynak temsilcisi görevlendirmemişse, **Sağlayıcı teklifleri** bölümünün üst kısmında bir not görür. Bu, müşterinin servis sağlayıcısının müşterinin kaynaklarından herhangi biri sine erişebilmesi için harekete geçmeleri gerektiğini bilmesini sağlar.

Abonelikleri veya kaynak gruplarını devretmek için:

1. Servis sağlayıcı, teklif ve adı içeren satırın kutusunu işaretleyin. Ardından ekranın üst **kısmındaki Temsilci kaynaklarını** seçin.
1. **Temsilci kaynakları** sayfasının **Teklif ayrıntıları** bölümünde, hizmet sağlayıcısı ve teklif hakkındaki ayrıntıları gözden geçirin. Teklifin rol atamalarını incelemek **için, seçili teklifin ayrıntılarını görmek için buraya tıklayın'ı**seçin.
1. **Temsilci** bölümünde, **Temsilci aboneliklerini** veya Temsilci kaynak **gruplarını**seçin.
1. Bu teklif için temsilci vermek istediğiniz abonelikleri ve/veya kaynak gruplarını seçin ve ardından **Ekle'yi**seçin.
1. Bu hizmet sağlayıcısına seçtiğiniz kaynaklara erişim izni vermek istediğinizi doğrulamak için sayfanın altındaki onay kutusunu seçin ve ardından **Temsilci'yi**seçin.

## <a name="add-or-remove-service-provider-offers"></a>Servis sağlayıcı teklifleri ekleme veya kaldırma

Müşteri, **Teklif Ekle'yi**seçerek **Sağlayıcı teklifleri** sayfasından yeni bir servis sağlayıcı teklifi ekleyebilir. Servis sağlayıcı bu müşteri için bir teklif yayımlamış olmalıdır. Müşteri daha sonra **özel teklifler** ekranından bu teklifi seçebilir ve sonra **Oluştur'u**seçebilir.

Müşteri bir hizmet sağlayıcı teklifini kaldırmak isterse, söz leilgili satırdaki çöp kutusu simgesini seçebilir. Silme onaylandıktan sonra, bu hizmet sağlayıcısı artık bu teklif için daha önce görevlendirilen müşteri kaynaklarına erişemeyecektir.

## <a name="update-service-provider-offers"></a>Servis sağlayıcı tekliflerini güncelleştir

Bir müşteri teklif ekledikten sonra, bir hizmet sağlayıcısı aynı teklifin güncelleştirilmiş bir sürümünü Azure Marketi'nde yayımlayabilir. Örneğin, yeni bir rol tanımı eklemek isteyebilirler. Teklifin yeni bir sürümü yayımlanmışsa, **Sağlayıcı teklif** sayfasında bu teklif için satırda bir "güncelleştirme" simgesi gösterilir. Müşteri, teklifin geçerli sürümüyle yeni sürümü arasındaki farkları görmek için bu simgeyi seçebilir.

 ![Teklif simgesini güncelleştir](../media/update-offer.jpg)

Değişiklikleri gözden geçirdikten sonra, müşteri yeni sürüme güncelleştirmeyi seçebilir. Bunu yaptıktan sonra, yeni sürümde belirtilen yetkilendirmeler ve diğer ayarlar, bu teklif için devredilen abonelikler ve/veya kaynak grupları için geçerli olacaktır.

## <a name="view-delegations"></a>Heyetleri görüntüleyin

Delegasyonlar, müşterinin devrettiği kaynaklar için hizmet sağlayıcısına izin veren rol atamalarını temsil eder. Bu bilgileri görüntülemek **için, Servis sağlayıcıları** sayfasının sol tarafındaki **Temsilciler'i** seçin.

Sayfanın üst kısmındaki filtreler, temsilci bilgilerinizi sıralamanızı ve gruplandırmanızı sağlar. Belirli müşterilere, tekliflere veya anahtar kelimelere göre de filtre uygulayabilirsiniz.

> [!NOTE]
> Müşteriler, Azure portalında veya API'ler aracılığıyla temsilci kapsamı [için rol atama bilgilerini görüntülerken](../../role-based-access-control/role-assignments-list-portal.md#list-role-assignments-at-a-scope) bu rol atamalarını veya bu rolleri alan hizmet sağlayıcı kiracıdan herhangi bir kullanıcı görmez.

## <a name="audit-delegations-in-your-environment"></a>Ortamınızdaki denetim heyetleri

Müşteriler, [Azure temsilci kaynak yönetimi](../concepts/azure-delegated-resource-management.md)için hizmet sağlayıcılarına devredilen abonelikler ve/veya kaynak gruplarında görünürlük elde etmek isteyebilir. Bu, özellikle çok sayıda aboneliği olan veya yönetim görevlerini gerçekleştiren çok sayıda kullanıcısı olan müşteriler için kullanışlıdır.

Kapsamlar delegasyonunun yönetici kiracıya denetlenebilmek için yerleşik bir [ilke tanımı](../../governance/policy/samples/built-in-policies.md#lighthouse) sağlıyoruz. Bu ilkeyi, denetlemek istediğiniz tüm abonelikleri içeren bir yönetim grubuna atayabilirsiniz. Bu politikaya uygunluğu denetlediğinizde, devredilen abonelikler ve/veya kaynak grupları (ilkenin atandığı yönetim grubunda) uyumlu olmayan bir durumda gösterilir. Daha sonra sonuçları gözden geçirebilir ve beklenmeyen delegasyonlar olmadığını onaylayabilirsiniz.

İlke atama ve uyumluluk durumu sonuçlarını görüntüleme hakkında daha fazla bilgi için [Bkz. Hızlı Başlangıç: İlke ataması oluşturun.](../../governance/policy/assign-policy-portal.md)

## <a name="next-steps"></a>Sonraki adımlar
 
- [Azure Deniz Feneri](../overview.md)hakkında daha fazla bilgi edinin.
- Azure portalındaki **müşterilerime** giderek hizmet sağlayıcıların müşterileri nasıl [görüntüleyip yönetebileceğini](view-manage-customers.md) öğrenin.