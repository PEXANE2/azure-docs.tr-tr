---
title: SaaS karşılama API'leri - SSS | Azure Marketi
description: Azure Marketi'ndeki bir SaaS teklifinin müşterileri tarafından bulma ve satın alma deneyimleri.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/11/2019
ms.openlocfilehash: 6d3a84341d5221950da20f39456461dafc5d2e75
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275705"
---
# <a name="saas-fulfillment-apis---faq"></a>SaaS gerçekleştirme API’leri - SSS

Azure müşterilerinin SaaS teklifine abone olmasını sağlamak için Azure Marketi ile tümleştirme gereksinimleri listelenir.

## <a name="discovery-experience"></a>Keşif deneyimi

Teklif yayımlandıktan sonra Azure kullanıcıları Azure Marketi'nde SaaS teklifini keşfedebilir. Müşterileriniz ürün türüne (SaaS) göre teklifleri filtreleyip ilgilendikleri SaaS hizmetlerini keşfedebileceklerdir.

## <a name="purchase-experience"></a>Satın alma deneyimi

Bir kullanıcı belirli bir SaaS hizmetiyle ilgilendiğinde, kullanıcı azure marketinden abone olabilir.

### <a name="what-does-it-mean-for-an-azure-user-to-subscribe-to-a-saas-offer-in-azure-marketplace"></a>Bir Azure kullanıcısının Azure Marketi'ndeki bir SaaS teklifine abone olması ne anlama gelir?

Bu, bir kullanıcının SaaS hizmetiyle ilişkili kullanım koşullarını ve gizlilik bildirimini görüntüleyebileceği ve Microsoft'un faturasında SaaS teklifinin yayımcısı olan sizin belirlediğiniz faturalandırma koşullarına göre ödeme yapmayı kabul edebileceği anlamına gelir. Kullanıcılar, SaaS hizmet tüketimini ödemek için Azure'daki mevcut ödeme profillerini kullanabilir.

Bu birçok nedenden dolayı yararlıdır. Müşteriler artık, kullanmak istediği her ISV yazılımLarını veterinerlik etmek zorunda kalmadan, güvenilir bir kaynak olarak Microsoft Cloud Platform'u kullanarak tek bir yerde keşfedebilir ve abone olabilir. Ayrıca, müşteriler her ISV yazılımLarını bağımsız olarak açıkça ödemek zorunda kalmadan mevcut ödeme profillerini kullanabilirler.

### <a name="is-the-user-charged-automatically-when-the-offer-is-subscribed"></a>Teklif abone olunca kullanıcı otomatik olarak ücretlendirilir mi?

Kullanıcı, SaaS teklifine abone olmakla birlikte, SaaS hizmetinin tüketimi için Microsoft platformu üzerinden ödeme yapmayı kabul etmiştir. Ancak, ücretler yalnızca teklif tüketildiğinde başlar. Kullanıcı, teklifi tüketmeye başlamak için SaaS teklifinize gitmeli ve hesap oluşturmayı onaylamalıdır. Daha sonra, bu müşteri SaaS aboneliği için faturalandırmayı başlatması için Microsoft'a bildirimde bulunabilirsiniz.

### <a name="how-are-you-notified-when-a-user-subscribes-to-your-saas-offer"></a>Bir kullanıcı SaaS teklifinize abone olduğunda nasıl bilgilendirilirsiniz?

Azure kullanıcısı bir teklife abone olduktan sonra Azure'daki tüm tekliflerini keşfedebilir ve yönetebilir. Varsayılan olarak, yeni abone olunan Bir SaaS teklifinin durumu **'Sağlama, yerine getirme beklemede'** dir. Bu durumda, Azure kullanıcısına Azure portalındaki SaaS abonelik yönetimi deneyimine göz atmak için **'Hesabı Yapılandırma'** eylemi istenir.

Kullanıcı **'Hesabı Yapılandır'** seçeneğini tıkladığında, SaaS hizmet web sitesine yönlendirilir. Yönlendirildikleri URL, teklifin yayımlandığı sırada yayımcı tarafından sağlanır. Bu sayfa, yayıncının açılış sayfası olarak adlandırılır. Azure kullanıcıları, Azure'daki mevcut AAD kimlik bilgilerine bağlı olarak SaaS açılış sayfasına giriş yapabilmelidir.

Azure kullanıcısı açılış sayfasına yönlendirildiğinde, sorgu URL'sine bir belirteç eklenir. Bu belirteç kısa ömürlüdür ve 24 saatlik bir süre için geçerlidir. Daha sonra bu belirteç varlığını algılayabilir ve belirteçile ilişkili daha fazla bağlam almak için Microsoft'un API'sini arayabilirsiniz.

![Müşteri abonelik akışı](media/saas-metering-service-integration-flow-a.png)

Bir SaaS teklifinin yaşam döngüsündeki işlem senaryolarını işlemek için API sözleşmesi hakkında daha fazla bilgi için [SaaS karşılama API](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) belgesine bakın.

### <a name="how-do-you-know-the-saas-offer-to-which-the-user-subscribes-in-azure"></a>Kullanıcının Azure'da abone olduğu SaaS teklifini nereden biliyorsunuz?

`Resolve` API'ye verilen yanıt, SaaS aboneliğiyle ilişkili teklif ve plan bilgilerini içerir.

### <a name="how-can-the-azure-user-change-the-plan-associated-with-this-azure-subscription"></a>Azure kullanıcısı bu Azure aboneliğiyle ilişkili planı nasıl değiştirebilir?

* Azure kullanıcısı, SaaS aboneliğiyle ilişkili planı doğrudan SaaS deneyiminde veya Microsoft platformu aracılığıyla değiştirebilir.

* Dönüşümler faturalandırma döngüsünde herhangi bir zamanda yapılabilir. Kabul edildikten sonra yürürlüğe girecek olan herhangi bir dönüşümü kabul etmelisiniz.

* Ön ödemeli plan (**aylık** veya **yıllık**) oranları pro-rated vardır. Dönüşüm zamanına kadar yayılan herhangi bir fazlalık bir sonraki faturada ücretlendirilir. Yeni plana göre yeni aşımlar yayılacak.

>[!Note]
>Belirli dönüşüm yollarını desteklemek istemiyorsanız, düşürmeleri engelleyebilirsiniz.

Aşağıdaki sıra, Bir Azure müşterisi SaaS deneyiminde planı değiştirdiğinde akışı yakalar:

![Müşteri planı değişim akışı](media/saas-metering-service-integration-flow-b.png)

Aşağıdaki sıra, Bir Azure müşterisi Microsoft'un vitrininde plan yaptığında akışı yakalar

![Müşteri vitrin planı değişim akışı](media/saas-metering-service-integration-flow-c.png)

### <a name="how-can-the-azure-user-unsubscribe-from-the-plan-associated-with-azure-subscription"></a>Azure kullanıcısı, Azure aboneliğiyle ilişkili plandan nasıl aboneliğini iptal edebilir?

Azure kullanıcısı, satın alınan bir SaaS teklifinden doğrudan SaaS deneyiminde veya Microsoft platformu üzerinden aboneliğini iptal edebilir. Kullanıcı aboneliğini iptal ettikten sonra, artık bir sonraki faturalandırma döngüsünden ücretlendirilmez.

Aşağıdaki sıra, bir Azure müşterisi SaaS deneyiminde SaaS teklifine aboneliğini iptal ettiğinde akışı yakalar:

![Müşteri SaaS deneyiminde aboneliklerini iptal eder](media/saas-metering-service-integration-flow-d.png)

Aşağıdaki sıra, Azure kullanıcısı Microsoft'un vitrininde aboneliğini iptal ettiğinde akışı yakalar:

![Microsoft'un vitrininde müşteri aboneliklerini iptal eder](media/saas-metering-service-integration-flow-e.png)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için [Market ölçüm hizmeti API'leri'ne](./marketplace-metering-service-apis.md) bakın.
