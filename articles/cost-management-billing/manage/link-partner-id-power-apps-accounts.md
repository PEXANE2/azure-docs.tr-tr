---
title: Azure kimlik bilgilerinizle Power Apps hesaplarınıza bir iş ortağı KIMLIĞI bağlayın
description: Bu makale, Microsoft iş ortaklarının Microsoft Power Apps uygulamalarını kullanmasına yardımcı olmak için Azure kimlik bilgilerini kullanmasına yardımcı olur.
author: bandersmsft
ms.reviewer: akangaw
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 03/16/2021
ms.author: banders
ms.openlocfilehash: adaff7a6b8559fe9604412a44eced6c490231e3c
ms.sourcegitcommit: 27cd3e515fee7821807c03e64ce8ac2dd2dd82d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103603840"
---
# <a name="link-a-partner-id-to-your-power-apps-accounts"></a>Power Apps hesaplarınıza bir iş ortağı KIMLIĞI bağlama

Bu makale, Power Apps hizmet sağlayıcıları olan Microsoft iş ortaklarının, hizmetini Microsoft Power Apps 'teki müşterilerle ilişkilendirmelerini sağlar. (Microsoft iş ortağı), müşteriniz için Power Apps hizmetlerini yönet, yapılandırmanıza ve destekledikleri zaman, müşterinizin ortamına erişiminiz vardır. İş ortağı ağ KIMLIĞINIZI hizmet teslimi için kullanılan hesap kimlik bilgileriyle ilişkilendirmek için, Azure kimlik bilgilerinizi ve bir Iş ortağı yönetici bağlantısını (PAL) kullanabilirsiniz.

PAL, Microsoft 'un, Power Apps müşterileri olan iş ortaklarını belirlemesine ve tanımasını sağlar. Microsoft, hesabın izinlerini (Power Apps rolü) ve kapsamını (kiracı, kaynak grubu, kaynak) temel alarak bir iş ortağının organizasyonu için kullanım özniteliklerini sağlar.

## <a name="get-access-from-your-customer"></a>Müşterinizden erişim elde etme

İş ortağı KIMLIĞINIZI bağlayabilmeniz için, müşterinizin aşağıdaki seçeneklerden birini kullanarak kendi güç uygulamaları kaynaklarına erişmenizi sağlamanız gerekir:

- **Konuk Kullanıcı** -müşteriniz sizi Konuk Kullanıcı olarak ekleyebilir ve tüm güç uygulamaları rollerini atayabilir. Daha fazla bilgi için bkz. [Başka bir dizinden konuk kullanıcılar ekleme](../../active-directory/external-identities/what-is-b2b.md).
- **Dizin hesabı** -müşteriniz kendi dizinlerinde sizin için bir kullanıcı hesabı oluşturabilir ve herhangi bir Power Apps rolü atayabilir.
- **Hizmet sorumlusu** -müşteriniz, kuruluşunuzda kendi dizinine bir uygulama veya betik ekleyebilir ve herhangi bir Power Apps rolü atayabilir. Uygulamanın veya betiğin kimliği, hizmet sorumlusu olarak bilinir.
- **Yönetici temsilcisi** -kullanıcılarınız, kullanıcılarınızın kiracınızın içinden çalışabilmesi için bir kaynak grubu temsilciliğini alabilir. Daha fazla bilgi için bkz. [iş ortakları: yönetici temsilcisi](/power-platform/admin/for-partners-delegated-administrator).

## <a name="link-customer-to-a-partner-id"></a>Müşteriyi bir iş ortağı KIMLIĞIYLE ilişkilendirin

Müşterinizin kaynaklarına erişiminiz olduğunda, Microsoft İş Ortağı Ağı KIMLIĞINIZI (MPN ID) Kullanıcı KIMLIĞINIZ veya hizmet sorumlusuna bağlamak için Azure portal, PowerShell veya Azure CLı 'yi kullanın. İş ortağı KIMLIĞINI her bir müşteri kiracısına bağlayın.

### <a name="use-the-azure-portal-to-link-to-a-new-partner-id"></a>Yeni bir iş ortağı kimliğine bağlantı için Azure portalını kullanma

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Azure portalında [İş ortağı kimliğine bağlantı](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade) bölümüne gidin.
1. Kuruluşunuzun [Microsoft iş ortağı ağı](https://partner.microsoft.com/) kimliğini girin. İş ortağı profilinizde gösterilen  **ilişkili MPN kimliğini**  kullandığınızdan emin olun.  
    :::image type="content" source="./media/link-partner-id-power-apps-accounts/link-partner-id.png" alt-text="İş ortağı KIMLIĞI penceresinin bağlantısını gösteren ekran görüntüsü." lightbox="./media/link-partner-id-power-apps-accounts/link-partner-id.png" :::
1. İş ortağı KIMLIĞINIZI başka bir müşteriye bağlamak için dizini değiştirin. **Anahtar dizini** altında uygun dizini seçin.  
    :::image type="content" source="./media/link-partner-id-power-apps-accounts/switch-directory.png" alt-text="Dizin + abonelik penceresini gösteren ve dizininizi geçirebileceğiniz ekran görüntüsü." lightbox="./media/link-partner-id-power-apps-accounts/switch-directory.png" :::

### <a name="use-powershell-to-link-to-a-new-partner-id"></a>Yeni bir iş ortağı kimliğine bağlanmak için PowerShell’i kullanma

[Az. ManagementPartner](https://www.powershellgallery.com/packages/Az.ManagementPartner/) Azure PowerShell modülünü yükler.

Kullanıcı hesabı ya da hizmet sorumlusu ile müşterinin kiracısında oturum açın. Daha fazla bilgi için bkz [PowerShell ile oturum açma](/powershell/azure/authenticate-azureps).

```azurepowershell-interactive
Update-AzManagementPartner -PartnerId 12345
```

Yeni iş ortağı kimliğine bağlantı oluşturun. İş ortağı kimliği, kuruluşunuzun [Microsoft İş Ortağı Ağı](https://partner.microsoft.com/) kimliğidir. İş ortağı profilinizde gösterilen **ilişkili MPN kimliğini**  kullandığınızdan emin olun.

```azurepowershell-interactive
new-AzManagementPartner -PartnerId 12345
```

Bağlı iş ortağı kimliğini alma

```azurepowershell-interactive
get-AzManagementPartner
```

Bağlı iş ortağı kimliğini güncelleştirme

```azurepowershell-interactive
Update-AzManagementPartner -PartnerId 12345
```

Bağlı iş ortağı kimliğini silme

```azurepowershell-interactive
remove-AzManagementPartner -PartnerId 12345
```

### <a name="use-the-azure-cli-to-link-to-a-new-partner-id"></a>Yeni bir iş ortağı kimliğine bağlantı için Azure CLI’yı kullanma

İlk olarak, Azure CLı uzantısını yüklemeniz gerekir.

```azurecli-interactive
az extension add --name managementpartner
```

Kullanıcı hesabı ya da hizmet sorumlusu ile müşterinin kiracısında oturum açın. Daha fazla bilgi için bkz. [Azure CLI ile oturum açma](/cli/azure/authenticate-azure-cli).

```azurecli-interactive
az login --tenant TenantName
```

Yeni iş ortağı kimliğine bağlantı oluşturun. İş ortağı kimliği, kuruluşunuzun [Microsoft İş Ortağı Ağı](https://partner.microsoft.com/) kimliğidir.

```azurecli-interactive
az managementpartner create --partner-id 12345
```

Bağlı iş ortağı kimliğini alma

```azurecli-interactive
az managementpartner show
```

Bağlı iş ortağı kimliğini güncelleştirme

```azurecli-interactive
az managementpartner update --partner-id 12345
```

Bağlı iş ortağı kimliğini silme

```azurecli-interactive
az managementpartner delete --partner-id 12345
```

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

Aşağıdaki bölümlerde, bir iş ortağı KIMLIĞINI Power Apps hesaplarına bağlamak hakkında sık sorulan sorular ele alınmaktadır.

### <a name="who-should-link-the-partner-id"></a>İş ortağı KIMLIĞINI kimler bağlayamalıdır?

İş ortağı kuruluştan bir müşterinin Power Apps kaynakları üzerinde çalışan herhangi bir kullanıcının iş ortağı KIMLIĞINI hesaba bağlayabilmesini sağlayabilirsiniz. İdeal olarak, PAL 'teki ilişki projenin başlangıcında yapılmalıdır. Ancak, müşterinin dizinine her eriştiğinizde bu yapılabilir.

### <a name="can-a-partner-id-be-changed-after-its-linked"></a>Bir iş ortağı kimliği bağlandıktan sonra değiştirilebilir mi?

Evet. Bağlı bir iş ortağı kimliği değiştirilebilir, eklenebilir veya kaldırılabilir. Bu durum için bir örnek, şirketinizdeki bir çalışan kuruluştan ayrıldığında olabilir. Müşteri ile bir proje veya sözleşme sona erdiğinde başka bir örnek de olabilir.

### <a name="what-if-a-user-has-an-account-in-more-than-one-customer-tenant"></a>Bir kullanıcının birden fazla müşteri kiracısında hesabı varsa ne olur?

Her müşteri kiracısı için iş ortağı kimliği ve hesap arasında bağlantı oluşturulur. Her müşteri kiracısında iş ortağı kimliğini bağlayın.

### <a name="can-other-partners-or-customers-edit-or-remove-the-link-to-the-partner-id"></a>Diğer iş ortakları veya müşteriler, iş ortağı kimliğine bağlantıyı düzenleyebilir ya da kaldırabilir mi?

Bağlantı, kullanıcı hesabı düzeyinde ilişkilendirilir. Yalnızca siz iş ortağı kimliğine bağlantıyı düzenleyebilir veya kaldırabilirsiniz. Müşteri ve diğer iş ortakları, iş ortağı kimliğine bağlantıyı değiştiremez.

### <a name="which-mpn-id-should-i-use-if-my-company-has-multiple"></a>Şirketimin birden çok MPN kimliği varsa hangisini kullanmalıyım?

İş ortağı profilinizde gösterilen **İlişkili MPN kimliğini** kullandığınızdan emin olun. Bu, genellikle kuruluşunuzla yerel hesap KIMLIĞI ilişkidir.

### <a name="how-do-i-explain-pal-to-my-customer"></a>Müşterime PAL Nasıl yaparım? açıkla mı?

PAL, Microsoft 'un müşterilerin iş hedeflerine ulaşmasını ve bulutta değer elde etmesini sağlayan iş ortaklarını belirlemesine ve tanımasını sağlar. Müşterilerin, önce Power Apps kaynaklarına bir iş ortağı erişimi sağlaması gerekir. Erişim verildiğinde, ortağın Microsoft İş Ortağı Ağı KIMLIĞI (MPN KIMLIĞI) ilişkilendirilir. Bu ilişkilendirme, Microsoft 'un hizmet sağlayıcılarını anlamasına ve müşterileri en iyi şekilde desteklemesi için gereken araçları ve programları iyileştirmesine yardımcı olur.

### <a name="what-data-does-pal-collect"></a>PAL hangi verileri toplar?

Mevcut kimlik bilgilerine PAL ilişkilendirmesi Microsoft'a yeni müşteri verileri sağlamaz. Bu, bir ortağın bir müşterinin güç uygulamaları ortamlarında etkin olarak bulunduğu Microsoft 'a bilgi sağlar. Microsoft, Müşteri ortamından, iş ortağına müşteri tarafından sunulan hesap izinleri (Power Apps rolü) ve kapsamı (kiracı, kaynak grubu, kaynak) temelinde iş ortağı kuruluşa kullanım ve etkinin üzerinde çalışma özelliği verebilir.

### <a name="does-pal-association-affect-the-security-of-a-customers-power-apps-environment"></a>PAL ilişkilendirmesi bir müşterinin Power Apps ortamının güvenliğini etkiler mi?

PAL ilişkilendirmesi yalnızca ortağın MPN KIMLIĞINI zaten sağlanmış olan kimlik bilgisine ekler. Herhangi bir izni değiştirmez (Power Apps rolü) veya iş ortağı veya Microsoft 'a ek Power Apps hizmet verileri sağlar.

### <a name="next-steps"></a>Sonraki adımlar

- Güncelleştirmeleri almak veya geri bildirim göndermek için [Microsoft İş Ortağı Topluluğu](https://aka.ms/PALdiscussion)’ndaki tartışmaya katılın.
- Düşük kod uygulama geliştirme gelişmiş özelleştirmesi için PAL tabanlı Power Apps ilişkilendirmesi için [düşük kod uygulama geliştirme gelişmiş ÖZELLEŞTIRME SSS](https://assetsprod.microsoft.com/mpn/faq-low-code-app-development-advanced-specialization.pdf) makalesini okuyun.
