---
title: Azure Marketi'nde yönetilen bir hizmet teklifi yayınlama
description: Müşterilerin Azure temsilcikaynak yönetimine dahil ettiği yönetilen bir hizmet teklifini nasıl yayınlayacağınızı öğrenin.
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: 922e78a0715ce5c758ca7d068a38af43e214a524
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673946"
---
# <a name="publish-a-managed-service-offer-to-azure-marketplace"></a>Azure Marketi'nde yönetilen bir hizmet teklifi yayınlama

Bu makalede, [İş Ortağı Merkezi'ndeki](https://partner.microsoft.com/) [Ticari Pazar](../../marketplace/partner-center-portal/commercial-marketplace-overview.md) programını kullanarak Azure [Market'te](https://azuremarketplace.microsoft.com) genel veya özel yönetilen bir hizmet teklifini nasıl yayınlayacağınızı öğreneceksiniz. Teklifi satın alan müşteriler, [Azure temsilci kaynak yönetimi](../concepts/azure-delegated-resource-management.md)için yerleşik aboneliklere ve kaynak gruplarına binebilir.

## <a name="publishing-requirements"></a>Yayımlama gereksinimleri

Teklifler oluşturmak ve yayınlamak için [İş Merkezi'nde](../../marketplace/partner-center-portal/create-account.md) geçerli bir hesabınız olması gerekir. Zaten bir hesabınız yoksa, kayıt [işlemi,](https://aka.ms/joinmarketplace) İş Ortağı Merkezi'nde bir hesap oluşturma ve Ticari Pazar programına kaydolma adımlarında size yol açar.

Yönetilen [Hizmet teklif lerine göre,](https://docs.microsoft.com/legal/marketplace/certification-policies#7004-business-requirements)Yönetilen Hizmet teklifini yayınlamak için [Gümüş veya Altın Bulut Platformu uzmanlık düzeyine](https://docs.microsoft.com/partner-center/learn-about-competencies) sahip olmalısınız veya Azure Uzmanı [MSP](https://partner.microsoft.com/membership/azure-expert-msp) olmalısınız.

Microsoft İş Ortağı Ağı (MPN) Kimliğiniz, müşteri etkileşimleri üzerindeki etkinizi izlemek için yayımladığınız tekliflerle [otomatik olarak ilişkilendirilir.](../../billing/billing-partner-admin-link-started.md)

> [!NOTE]
> Azure Marketi'nde bir teklif yayınlamak istemiyorsanız, Azure Kaynak Yöneticisi şablonlarını kullanarak müşterilere el ile binebilirsiniz. Daha fazla bilgi için, [Azure temsilcili kaynak yönetimine](onboard-customer.md)giden bir müşteriye bakın.

## <a name="create-your-offer"></a>Teklifinizi oluşturun

Sağlamanız gereken tüm bilgiler ve varlıklar da dahil olmak üzere teklifinizi nasıl oluşturacağınıza ilişkin ayrıntılı talimatlar için [bkz.](../../marketplace/partner-center-portal/create-new-managed-service-offer.md)

Genel yayımlama süreci hakkında bilgi edinmek için [Azure Marketi ve AppSource Yayımlama Kılavuzu'na](../../marketplace/marketplace-publishers-guide.md)bakın. Ayrıca [ticari pazar sertifikasyonu ilkelerini](https://docs.microsoft.com/legal/marketplace/certification-policies)( özellikle [Yönetilen Hizmetler](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services) bölümünü) gözden geçirmelisiniz.

Bir müşteri teklifinizi ekledikten sonra, bir veya daha fazla abonelik veya kaynak grubuna temsilci atayabilir ve bu grup azure [temsilci kaynak yönetimi için geçerli](#the-customer-onboarding-process)olacaktır.

> [!IMPORTANT]
> Yönetilen hizmet teklifindeki her plan, kiracınızdaki Azure Etkin Dizin (Azure AD) varlıklarını tanımladığınız ve bu planı satın alan müşteriler için temsilci kaynak gruplarına ve/veya aboneliklere erişebilecek bir **Bildirim Ayrıntıları** bölümünü içerir. Eklediğiniz herhangi bir grubun (veya kullanıcı veya hizmet sorumlusunun) planı satın alan her müşteri için aynı izinlere sahip olacağını unutmayın. Her müşteriyle çalışmak üzere farklı gruplar atamak için, her müşteriye özel ayrı bir [özel plan](../../marketplace/private-offers.md) yayımlamanız gerekir.

## <a name="publish-your-offer"></a>Teklifinizi yayınlayın

Tüm bölümleri tamamladıktan sonra, bir sonraki adımınız teklifi Azure Marketi'nde yayınlamaktır. Teklifinizi canlı yapma işlemini başlatmak için **Yayımla** düğmesini seçin. Bu işlem hakkında daha fazla bilgiyi [burada](../../marketplace/partner-center-portal/create-new-managed-service-offer.md#publish)bulabilirsiniz.

[Teklifinizin güncelleştirilmiş bir sürümünü](../../marketplace/partner-center-portal/update-existing-offer.md) istediğiniz zaman yayımlayabilirsiniz. Örneğin, daha önce yayımlanmış bir teklife yeni bir rol tanımı eklemek isteyebilirsiniz. Bunu yaptığınızda, teklifi zaten eklemiş olan müşteriler Azure portalındaki [**Hizmet sağlayıcıları**](view-manage-service-providers.md) sayfasında bir güncelleştirmenin kullanılabilir olduğunu bildiren bir simge görür. Her müşteri değişiklikleri [gözden geçirebilecek](view-manage-service-providers.md#update-service-provider-offers) ve yeni sürüme güncelleştirmek isteyip istemediğine karar verebilir. 

## <a name="the-customer-onboarding-process"></a>Müşteri onboarding işlemi

Bir müşteri teklifinizi ekledikten sonra, [bir veya daha fazla özel abonelik veya kaynak grubuna (azure](view-manage-service-providers.md#delegate-resources)temsilcikaynak yönetimi için dahil edilecek) temsilci olarak devredilebilir. Bir müşteri bir teklifi kabul etmiş ancak henüz herhangi bir kaynak temsilcisi görevlendirmediyse, Azure portalında [**Hizmet sağlayıcıları**](view-manage-service-providers.md) sayfasının Sağlayıcı **teklifleri** bölümünün üst kısmında bir not görür.

> [!IMPORTANT]
> Temsilci, müşterinin kiracısında, yerleşik abonelik için [Sahibinin yerleşik rolüne](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) sahip olan (veya gemide bulunan kaynak gruplarını içeren) konuk olmayan bir hesap tarafından yapılmalıdır. Aboneliği devratabilecek tüm kullanıcıları görmek için, müşterinin kiracısındaki bir kullanıcı Azure portalındaki aboneliği seçebilir, **Access denetimini (IAM)** açabilir ve [Sahibi rolü olan tüm kullanıcıları görüntüleyebilir.](../../role-based-access-control/role-assignments-list-portal.md#list-owners-of-a-subscription)

Müşteri bir aboneliği (veya bir abonelik içindeki bir veya daha fazla kaynak grubuna) temsilci olarak devrettikten sonra, **Microsoft.ManagedServices** kaynak sağlayıcısı bu abonelik için kaydedilir ve kiracınızdaki kullanıcılar teklifinizdeki yetkilendirmelere göre devredilen kaynaklara erişebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Pazar Hakkında](../../marketplace/partner-center-portal/commercial-marketplace-overview.md)Bilgi Edinin.
- Kiracılar [arası yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
- Azure portalındaki **müşterilerime** giderek [müşterileri görüntüleyin ve yönetin.](view-manage-customers.md)
