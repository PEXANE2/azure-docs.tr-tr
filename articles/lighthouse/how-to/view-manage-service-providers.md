---
title: Hizmet sağlayıcılarını görüntüleme ve yönetme
description: Müşteriler, hizmet sağlayıcıları, hizmet sağlayıcısı teklifleri ve Temsilcili kaynaklar hakkındaki bilgileri görüntülemek için Azure portal hizmet sağlayıcıları sayfasını kullanabilir.
ms.date: 08/12/2020
ms.topic: how-to
ms.openlocfilehash: c22408a52d973a244d67528a73d4eaa487f166ba
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88167173"
---
# <a name="view-and-manage-service-providers"></a>Hizmet sağlayıcılarını görüntüleme ve yönetme

Müşteriler, hizmet sağlayıcıları ve hizmet sağlayıcı teklifleri hakkındaki ayrıntıları görüntülemek, [Azure açık Thouse](../overview.md)'a belirli kaynaklar atamak ve yeni hizmet sağlayıcısı teklifleri için alışveriş yapmak üzere [Azure Portal](https://portal.azure.com) **hizmet sağlayıcıları** sayfasını kullanabilir.

> [!TIP]
> Burada hizmet sağlayıcılarına ve müşterilere başvuracağız. [birden çok kiracıyı yöneten kuruluşlar](../concepts/enterprise.md) , yönetim deneyimlerini birleştirmek için aynı süreci kullanabilir.

Azure portal **hizmet sağlayıcıları** sayfasına erişmek için, müşteri **tüm hizmetleri**seçip **hizmet sağlayıcıları** arayabilir ve bunu seçebilirsiniz. Ayrıca, Azure portal üst tarafına yakın arama kutusuna "hizmet sağlayıcıları" veya "Azure açık Thouse" girerek da bulabilir.

> [!NOTE]
> **Hizmet sağlayıcıları** sayfasını görüntülemek için, müşterinin kiracısındaki bir kullanıcının [okuyucu yerleşik rolüne](../../role-based-access-control/built-in-roles.md#reader) (veya okuyucu erişimi de içeren başka bir yerleşik Role) sahip olması gerekir.
>
> Teklifleri eklemek veya güncelleştirmek, kaynak atamak ve teklifleri kaldırmak için, kullanıcının abonelik için [yerleşik rolüne](../../role-based-access-control/built-in-roles.md#owner) sahip olması gerekir.

**Hizmet sağlayıcılar** sayfasının yalnızca, Azure açık thouse aracılığıyla Müşterinin aboneliklerine veya kaynak gruplarına erişimi olan hizmet sağlayıcılarıyla ilgili bilgileri gösterdiğini unutmayın. Müşteri, müşterinin kaynaklarına erişmek için Azure Use kullanmayan ek hizmet sağlayıcılarıyla çalışıyorsa, bu hizmet sağlayıcılarıyla ilgili bilgiler burada gösterilmez.

> [!TIP]
> Servis sağlayıcılar, Azure portal **müşterilerime** giderek müşterileri hakkındaki bilgileri görüntüleyebilir. Daha fazla bilgi için bkz. [müşterileri ve Temsilcili kaynakları görüntüleme ve yönetme](view-manage-customers.md).

## <a name="view-service-provider-details"></a>Hizmet sağlayıcı ayrıntılarını görüntüle

Müşteri, hizmet sağlayıcılarının ayrıntılarını görüntülemek için **hizmet sağlayıcıları** sayfasının sol tarafında **servis sağlayıcı teklifleri** ' ni seçebilir.

Müşteri, her bir servis sağlayıcısı teklifi için hizmet sağlayıcının adını ve onunla ilişkili teklifi, ekleme işlemi sırasında müşterinin girdiği adı görür.

**Temsilciler** sütununda, müşteri, bu teklif için hizmet sağlayıcısına kaç abonelik ve/veya kaynak grubu atanmış olduğunu görür. Hizmet sağlayıcısı, bu abonelikleri ve/veya kaynak gruplarını teklifte belirtilen erişim düzeylerine göre erişebilecek ve yönetebilecektir.

## <a name="add-or-remove-service-provider-offers"></a>Hizmet sağlayıcı tekliflerini ekleme veya kaldırma

Müşteri, **teklif Ekle**' ye tıklayarak **hizmet sağlayıcı teklifleri** sayfasından yeni bir servis sağlayıcısı teklifi ekleyebilir. Hizmet sağlayıcısı bu müşteri için bir teklif yayımlamalıdır. Müşteri daha sonra **özel teklifler** ekranından bu teklifi seçip **Oluştur**' u seçin.

Müşteri bir servis sağlayıcısı teklifini kaldırmak isterse, bu teklif için satırda çöp kutusu simgesini seçebilir. Silme işlemini onayladıktan sonra, söz konusu hizmet sağlayıcının artık bu teklif için daha önce atanmış olan müşteri kaynaklarına erişimi olmayacaktır.

## <a name="delegate-resources"></a>Kaynaklara temsilci atama

Bir hizmet sağlayıcının bir müşterinin kaynaklarına erişebilmesi ve bunları yönetebilmesi için önce bunların temsilci olarak oluşturulması gerekir. Bir müşteri bir teklifi kabul ettiğinde ancak henüz hiç kaynak temsilcisi yoksa, **hizmet sağlayıcı teklifleri** bölümünün en üstünde bir not görür. Bu, müşterinin, servis sağlayıcı müşterinin kaynaklarına erişmeden önce eylemde bulunan bir işlem yapması gerektiğini bilmesini sağlar.

Abonelikler veya kaynak grupları atamak için:

1. Hizmet sağlayıcı, teklif ve ad içeren satırın kutusunu işaretleyin. Ardından ekranın üst kısmındaki **kaynakları devretmek** ' ı seçin.
1. **Kaynak temsilcisi** sayfasının **Teklif Ayrıntıları** bölümünde, hizmet sağlayıcısı ve teklifiyle ilgili ayrıntıları gözden geçirin. Teklifin rol atamalarını gözden geçirmek için, **Seçili teklifin ayrıntılarını görmek üzere buraya tıklayın ' ı**seçin.
1. **Temsilci** bölümünde, **temsilci abonelikleri** veya **temsilci kaynak grupları**' nı seçin.
1. Bu teklif için temsilci seçmek istediğiniz abonelikleri ve/veya kaynak gruplarını seçin ve ardından **Ekle**' yi seçin.
1. Bu hizmet sağlayıcısına seçtiğiniz kaynaklara erişim izni vermek istediğinizi onaylamak için sayfanın altındaki onay kutusunu seçin ve ardından **temsilci**' i seçin.

## <a name="update-service-provider-offers"></a>Hizmet sağlayıcı tekliflerini güncelleştirme

Bir müşteri teklif ekledikten sonra, hizmet sağlayıcı aynı teklifin güncelleştirilmiş bir sürümünü Azure Marketi 'ne yayımlayabilir. Örneğin, yeni bir rol tanımı eklemek isteyebilir. Teklifin yeni bir sürümü yayımlandıysa, **servis sağlayıcı** teklif sayfasında bu teklif için bir "güncelleştirme" simgesi gösterilir. Müşteri, teklifin geçerli sürümü ile yeni bir arasındaki farkları görmek için bu simgeyi seçebilir.

 ![Teklifi Güncelleştir simgesi](../media/update-offer.jpg)

Değişiklikleri gözden geçirdikten sonra müşteri yeni sürüme güncelleştirmeyi tercih edebilir. Bu yapıldıktan sonra, yeni sürümde belirtilen yetkilendirmeler ve diğer ayarlar, bu teklif için temsilci atanmış abonelikler ve/veya kaynak grupları için de geçerlidir.

## <a name="view-delegations"></a>Temsilcileri görüntüle

Temsilci atama, bir müşterinin Temsilcili kaynaklar için hizmet sağlayıcısına izin veren rol atamalarını temsil eder. Bu bilgileri görüntülemek için, **hizmet sağlayıcıları** sayfasının sol tarafındaki **Temsilciler** ' ı seçin.

Sayfanın üst kısmındaki filtreler, yetkilendirme bilgilerinizi sıralamanıza ve gruplandırmasına olanak tanır. Belirli müşterilere, tekliflere veya anahtar sözcüklere göre de filtre uygulayabilirsiniz.

> [!NOTE]
> Müşteriler, Azure portal veya API 'Ler aracılığıyla [temsilci seçilen kapsam için rol atamalarını görüntülerken](../../role-based-access-control/role-assignments-list-portal.md#list-role-assignments-at-a-scope) bu rol atamalarını veya hizmet sağlayıcı kiracısından gelen tüm kullanıcıları görmez.

## <a name="audit-delegations-in-your-environment"></a>Ortamınızdaki denetim temsilcileri

Müşteriler, Azure ışıklı kullanım için temsilci seçilmiş abonelikler ve/veya kaynak grupları hakkında görünürlük elde etmek isteyebilir. Bu, özellikle çok sayıda aboneliğe sahip olan veya yönetim görevlerini gerçekleştiren çok sayıda kullanıcıya sahip olan müşteriler için yararlıdır.

Bir yönetim kiracısına kapsamları temsilciliğini denetlemek için bir [Azure ilkesi yerleşik ilke tanımı](../../governance/policy/samples/built-in-policies.md#lighthouse) sağlıyoruz. Bu ilkeyi, denetlemek istediğiniz tüm abonelikleri içeren bir yönetim grubuna atayabilirsiniz. Bu ilkeyle uyumluluğu denetlediğinizde, tüm temsilci olan abonelikler ve/veya kaynak grupları (ilkenin atandığı yönetim grubu içinde) uyumlu olmayan bir durumda gösterilir. Daha sonra sonuçları gözden geçirebilir ve beklenmeyen temsilci yok olduğunu doğrulayabilirsiniz.

İlke atama ve uyumluluk durumu sonuçlarını görüntüleme hakkında daha fazla bilgi için bkz. [hızlı başlangıç: ilke ataması oluşturma](../../governance/policy/assign-policy-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ışıklı kullanım](../overview.md)hakkında daha fazla bilgi edinin.
- Hizmet **sağlayıcılarının, Azure Portal müşteriler sayfasında** [müşterileri nasıl görüntüleyip yönetebileceğini](view-manage-customers.md) öğrenin.
