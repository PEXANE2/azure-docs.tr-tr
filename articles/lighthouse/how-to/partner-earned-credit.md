---
title: İş ortağı KIMLIĞINIZI, temsilcili kaynaklar üzerinde ortağın kazanıldığını etkinleştirmek için bağlayın
description: İş ortağı KIMLIĞINIZI, Azure açık Thouse aracılığıyla yönettiğiniz müşteri kaynaklarında iş ortağı kazanılan kredisi (PEC) alacak şekilde ilişkilendirmeyi öğrenin.
ms.date: 10/05/2020
ms.topic: how-to
ms.openlocfilehash: d9d80a94e52f6f6a8aef5f5284659750084b0b5e
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91974456"
---
# <a name="link-your-partner-id-to-enable-partner-earned-credit-on-delegated-resources"></a>İş ortağı KIMLIĞINIZI, temsilcili kaynaklar üzerinde ortağın kazanıldığını etkinleştirmek için bağlayın

[Microsoft iş ortağı ağı](https://partner.microsoft.com/)üyesiyse Iş ortağı kimliğinizi, temsilcili müşteri kaynaklarını yönetmek için kullanılan kimlik bilgileriyle bağlantılandırabilirsiniz. Bu, müşteri görevlendirmeleri genelinde etkileri izlemenize ve [yönetilen hizmetler (PEC) için iş ortağı tarafından kazanılan krediyi](/partner-center/partner-earned-credit)almanıza olanak sağlar.

[Azure Market 'Te yönetilen hizmet teklifleri olan müşteriler](publish-managed-services-offers.md)eklerseniz, bu bağlantı, teklifleri yayımlamak Için kullanılan Iş Ortağı Merkezi hesabıyla ilişkili MPN kimliği kullanılarak otomatik olarak gerçekleşir. Bu müşterilere PEC almak için başka bir eylem gerekmez.

[Azure Kaynak Yönetimi şablonlarını kullanarak müşteriler](onboard-customer.md)eklerseniz, bu bağlantıyı oluşturmak için işlem yapmanız gerekir. Bu işlem, [MPN kimliğiniz](../../cost-management-billing/manage/link-partner-id.md) , eklendi aboneliklerinizin her birine erişimi olan yönetim kiracınızdaki en az bir kullanıcı hesabıyla ilişkilendirilerek yapılır.

## <a name="associate-your-partner-id-when-you-onboard-new-customers"></a>Yeni müşterileri eklediğinizde iş ortağı KIMLIĞINIZI ilişkilendirin

Azure Resource Manager şablonlar (ARM şablonları) aracılığıyla müşterileri eklerken, iş ortağı KIMLIĞINIZI bağlamak ve iş ortağı kazanılmış krediyi etkinleştirmek için aşağıdaki işlemi kullanın. Bu adımları tamamlayabilmeniz için [MPN iş ortağı kimliğinizi](/partner-center/partner-center-account-setup#locate-your-mpn-id) bilmeniz gerekir. İş ortağı profilinizde gösterilen **İlişkili MPN kimliğini** kullandığınızdan emin olun.

Kolaylık olması için kiracınızda bir hizmet sorumlusu hesabı oluşturmanızı, **ilişkili MPN Kimliğinizle**bağlamayı ve ardından, sahip olduğunuz her MÜŞTERIYE, [PEC için uygun olan bir Azure yerleşik rolüyle](/partner-center/azure-roles-perms-pec)birlikte erişim vermeyi öneririz.

1. Yönettiğiniz kiracınızda [bir hizmet sorumlusu hesabı oluşturun](../../active-directory/develop/howto-authenticate-service-principal-powershell.md) . Bu örnek için, bu hizmet sorumlusu PEC Automation hesabını adı vereceğiz.
1. Bu hizmet sorumlusu hesabını kullanarak, yönetim kiracınızdaki [ilişkili MPN Kimliğinizle bağlantı](../../cost-management-billing/manage/link-partner-id.md#link-to-a-partner-id) yapın. Bunu yalnızca bir kez yapmanız gerekir.
1. [ARM şablonları kullanarak bir müşteriyi](onboard-customer.md)eklediğinizde, PEC [Için uygun olan Azure yerleşik rolüne](/partner-center/azure-roles-perms-pec)sahıp bir kullanıcı olarak PEC Automation hesabını içeren bir yetkilendirme eklediğinizden emin olun.

Bu adımları izleyerek, yönettiğiniz her müşteri kiracısı iş ortağı KIMLIĞINIZLE ilişkilendirilecektir ve bu müşteriler için PEC almanızı sağlar. PEC Automation hesabının, müşteri kiracısında herhangi bir eylem doğrulaması yapması veya herhangi bir işlem yapması gerekmez.

## <a name="add-your-partner-id-to-previously-onboarded-customers"></a>İş ortağı KIMLIĞINIZI daha önce eklendi müşterilerine ekleyin

Zaten bir müşteriyi eklendi, PEC Automation hesabı hizmet sorumlunuzu eklemek için başka bir dağıtım yapmak istemeyebilirsiniz. Bunun yerine, **ilişkili MPN kimliğinizi** bu müşterinin kiracısında çalışmaya erişimi olan bir kullanıcı hesabıyla bağlayabilirsiniz. Hesaba [PEC için uygun bir Azure yerleşik rolü](/partner-center/azure-roles-perms-pec)verildiğinden emin olun.

Hesap, yönetim kiracınızda [ilişkili MPN Kimliğinizle bağlantı](../../cost-management-billing/manage/link-partner-id.md#link-to-a-partner-id) kurulduktan sonra, bu MÜŞTERI için PEC elde edersiniz.

## <a name="confirm-partner-earned-credit"></a>Ortağın kazanılmış krediyi Onayla

[PEC ayrıntılarını Azure Portal görüntüleyebilir](/partner-center/partner-earned-credit-explanation#azure-cost-management) ve Pec 'in avantajına hangi maliyetlerin alındığını doğrulayabilirsiniz.

Yukarıdaki adımları izlediyseniz ve ilişkilendirmeyi görmüyorsanız, Azure portal bir destek isteği açın.

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft iş ortağı ağı](/partner-center/mpn-overview)hakkında daha fazla bilgi edinin.
- [PEC 'in nasıl hesaplanacağını ve ödendiğini](/partner-center/partner-earned-credit-explanation)öğrenin.
- [Müşterileri](onboard-customer.md) Azure ışıklı kullanmaya ekleme.