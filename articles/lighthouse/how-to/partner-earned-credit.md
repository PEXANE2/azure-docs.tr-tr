---
title: Temsilcili kaynaklar üzerindeki etkisini izlemek için iş ortağı KIMLIĞINIZI bağlayın
description: İş ortağı KIMLIĞINIZI, Azure açık Thouse aracılığıyla yönettiğiniz müşteri kaynaklarında iş ortağı kazanılan kredisi (PEC) alacak şekilde ilişkilendirmeyi öğrenin.
ms.date: 10/13/2020
ms.topic: how-to
ms.openlocfilehash: 95483cfabb7632182a7c23ae4963f2d38a2bd2c3
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92019927"
---
# <a name="link-your-partner-id-to-track-your-impact-on-delegated-resources"></a>Temsilcili kaynaklar üzerindeki etkisini izlemek için iş ortağı KIMLIĞINIZI bağlayın 

[Microsoft iş ortağı ağı](https://partner.microsoft.com/)üyesiyse Iş ortağı kimliğinizi, temsilcili müşteri kaynaklarını yönetmek için kullanılan kimlik bilgileriyle bağlantılandırabilirsiniz. İş ortağı yönetici bağlantısı (PAL), Microsoft 'un Azure müşteri başarısını çalıştıran iş ortaklarını belirlemesine ve tanımasını sağlar. Bu bağlantı Ayrıca [CSP (bulut çözümü sağlayıcısı)](/partner-center/csp-overview) iş ortaklarının [, Microsoft Müşteri SÖZLEŞMESI 'ni (MCA) imzalayan](/partner-center/confirm-customer-agreement) ve [Azure planı kapsamında](/partner-center/azure-plan-get-started)olan müşteriler için [yönetilen hizmetler (PEC) için iş ortağı kazanılmış kredisi](/partner-center/partner-earned-credit) almasına izin verir.

[Azure Market 'Te yönetilen hizmet teklifleri olan müşteriler](publish-managed-services-offers.md)eklerseniz,, teklifleri yayımlamak Için kullanılan Iş Ortağı Merkezi hesabıyla ilişkili MPN kimliği kullanılarak otomatik olarak bağlantı gerçekleşir. Bu müşterilere yönelik etkileri izlemek için başka bir eylem gerekmez.

[Azure Kaynak Yönetimi şablonlarını kullanarak müşteriler](onboard-customer.md)eklerseniz, bu bağlantıyı oluşturmak için işlem yapmanız gerekir. Bu işlem, [MPN kimliğiniz](../../cost-management-billing/manage/link-partner-id.md) , eklendi aboneliklerinizin her birine erişimi olan yönetim kiracınızdaki en az bir kullanıcı hesabıyla ilişkilendirilerek yapılır.

## <a name="associate-your-partner-id-when-you-onboard-new-customers"></a>Yeni müşterileri eklediğinizde iş ortağı KIMLIĞINIZI ilişkilendirin

Azure Resource Manager şablonlar (ARM şablonları) aracılığıyla müşterileri eklerken, iş ortağı KIMLIĞINIZI bağlamak için aşağıdaki işlemi kullanın (varsa, iş ortağı tarafından kazanılan kredileri etkinleştirin). Bu adımları tamamlayabilmeniz için [MPN iş ortağı kimliğinizi](/partner-center/partner-center-account-setup#locate-your-mpn-id) bilmeniz gerekir. İş ortağı profilinizde gösterilen **İlişkili MPN kimliğini** kullandığınızdan emin olun.

Kolaylık olması için kiracınızda bir hizmet sorumlusu hesabı oluşturmanızı, **ilişkili MPN Kimliğinizle**bağlamayı ve ardından, sahip olduğunuz her MÜŞTERIYE, [PEC için uygun olan bir Azure yerleşik rolüyle](/partner-center/azure-roles-perms-pec)birlikte erişim vermeyi öneririz.

1. Yönettiğiniz kiracınızda [bir hizmet sorumlusu hesabı oluşturun](../../active-directory/develop/howto-authenticate-service-principal-powershell.md) . Bu örnekte, bu hizmet sorumlusu için ad *sağlayıcı Otomasyonu hesabını* kullanacağız.
1. Bu hizmet sorumlusu hesabını kullanarak, yönetim kiracınızdaki [ilişkili MPN Kimliğinizle bağlantı](../../cost-management-billing/manage/link-partner-id.md#link-to-a-partner-id) yapın. Bunu yalnızca bir kez yapmanız gerekir.
1. [ARM şablonları kullanarak bir müşteriyi](onboard-customer.md)EKLEDIĞINIZDE, [PEC Için uygun olan Azure yerleşik rolüne](/partner-center/azure-roles-perms-pec)sahip bir kullanıcı olarak sağlayıcı Otomasyonu hesabını içeren bir yetkilendirme eklediğinizden emin olun.

Bu adımları izleyerek, yönettiğiniz her müşteri kiracının iş ortağı KIMLIĞINIZLE ilişkilendirilmesi gerekir. Sağlayıcı Otomasyonu hesabının, müşteri kiracısında herhangi bir eylem doğrulaması veya yapması gerekmez.

:::image type="content" source="../media/lighthouse-pal.jpg" alt-text="Azure ınthouse ile PAL işlemini gösteren diyagram.":::

## <a name="add-your-partner-id-to-previously-onboarded-customers"></a>İş ortağı KIMLIĞINIZI daha önce eklendi müşterilerine ekleyin

Zaten bir müşteriyi eklendi, sağlayıcı Otomasyon hesabı hizmet sorumlunuzu eklemek için başka bir dağıtım gerçekleştirmek istemeyebilirsiniz. Bunun yerine, **ilişkili MPN kimliğinizi** bu müşterinin kiracısında çalışmaya erişimi olan bir kullanıcı hesabıyla bağlayabilirsiniz. Hesaba [PEC için uygun bir Azure yerleşik rolü](/partner-center/azure-roles-perms-pec)verildiğinden emin olun.

Hesap, yönetim kiracınızda [ilişkili MPN Kimliğinizle bağlantı](../../cost-management-billing/manage/link-partner-id.md#link-to-a-partner-id) kurulduktan sonra, bu müşteriyle ilgili olarak tanınabilmesi için tanınma izini sağlayacaksınız.

## <a name="confirm-partner-earned-credit"></a>Ortağın kazanılmış krediyi Onayla

[PEC ayrıntılarını Azure Portal görüntüleyebilir](/partner-center/partner-earned-credit-explanation#azure-cost-management) ve Pec 'in avantajına hangi maliyetlerin alındığını doğrulayabilirsiniz. PEC 'in yalnızca, MCA 'yi imzalayan ve Azure planının altında olan CSP müşterileri için geçerli olduğunu unutmayın.

Yukarıdaki adımları izlediyseniz ve ilişkilendirmeyi görmüyorsanız, Azure portal bir destek isteği açın.

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft iş ortağı ağı](/partner-center/mpn-overview)hakkında daha fazla bilgi edinin.
- [PEC 'in nasıl hesaplanacağını ve ödendiğini](/partner-center/partner-earned-credit-explanation)öğrenin.
- [Müşterileri](onboard-customer.md) Azure ışıklı kullanmaya ekleme.