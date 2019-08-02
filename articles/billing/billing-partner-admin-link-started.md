---
title: Bir iş ortağı kimliğini Azure hesaplarınızla bağlantılı hale getirme | Microsoft Docs
description: Müşteri kaynaklarını yönetmek için kullandığınız kullanıcı hesabına bir iş ortağı KIMLIĞI bağlayarak Azure müşterilerine sahip görevlendirmeleri izleyin.
services: billing
author: dhirajgandhi
manager: dhgandhi
ms.author: banders
ms.date: 03/12/2019
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 0448ffbccddc913bd6359f5f6bbf42988239afb4
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706397"
---
# <a name="link-a-partner-id-to-your-azure-accounts"></a>Bir iş ortağı kimliğini Azure hesaplarınızla bağlantılı hale getirme

Microsoft iş ortakları, Microsoft ürünlerini kullanarak müşterilerin iş ve görev hedeflerine ulaşmalarına yardımcı olan hizmetler sunar. İş ortağı kullanıcılarının müşteri adına Azure hizmetlerini yönetirken, yapılandırırken ve söz konusu hizmetler için destek sunarken müşterinin ortamına erişmesi gerekir. İş ortakları, İş Ortağı Yöneticisi Bağlantısını kullanarak iş ortağı ağ kimliklerini hizmet sunumu için kullanılan kimlik bilgileri ile ilişkilendirebilir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-access-from-your-customer"></a>Müşterinizden erişim izni alın

İş ortağı kimliğinizi bağlantılı hale getirmeden önce, müşteriniz aşağıdaki seçeneklerden birini kullanarak size Azure kaynaklarına erişim izni sağlamalıdır:

- **Konuk Kullanıcı**: Müşteriniz sizi konuk kullanıcı olarak ekleyebilir ve herhangi bir rol tabanlı erişim denetimi (RBAC) rolü atayabilir. Daha fazla bilgi için bkz.[Başka bir dizinden konuk kullanıcılar ekleme](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

- **Dizin hesabı**: Müşteriniz kendi dizinlerinde sizin için bir kullanıcı hesabı oluşturabilir ve herhangi bir RBAC rolü atayabilir.

- **Hizmet sorumlusu**: Müşteriniz, kendi dizinine bir uygulama veya betik ekleyebilir ve herhangi bir RBAC rolü atayabilir. Uygulamanın veya betiğin kimliği hizmet sorumlusu olarak bilinir.

## <a name="link-to-a-partner-id"></a>İş ortağı kimliğine bağlantı

Müşterinin kaynaklarına erişiminiz olduğunda, Azure portal, PowerShell veya Azure CLı kullanarak Microsoft İş Ortağı Ağı KIMLIĞINIZI (MPN KIMLIĞI) Kullanıcı KIMLIĞINIZ veya hizmet sorumlusuna bağlayın. Her müşteri kiracısında iş ortağı KIMLIĞINI bağlayın.

### <a name="use-the-azure-portal-to-link-to-a-new-partner-id"></a>Yeni bir iş ortağı KIMLIĞINE bağlamak için Azure portal kullanın

1. Azure portal [bir iş ortağı kimliğine bağlantı](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade) bölümüne gidin.

2. Azure Portal’da oturum açın.

3. Microsoft iş ortağı KIMLIĞINI girin. İş ortağı KIMLIĞI, kuruluşunuzun [Microsoft iş ortağı ağı](https://partner.microsoft.com/) kimliğidir.

   ![İş ortağı KIMLIĞI bağlantısını gösteren ekran görüntüsü](./media/billing-link-partner-id/link-partner-ID.PNG)

4. Başka bir müşterinin iş ortağı KIMLIĞINI bağlamak için dizini değiştirin. **Anahtar dizini**altında dizininizi seçin.

   ![Anahtar dizinini gösteren ekran görüntüsü](./media/billing-link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-to-a-new-partner-id"></a>PowerShell kullanarak yeni bir iş ortağı KIMLIĞINE bağlantı

1. [Az. ManagementPartner](https://www.powershellgallery.com/packages/Az.ManagementPartner/) PowerShell modülünü yükler.

2. Kullanıcı hesabı ya da hizmet sorumlusu ile müşterinin kiracısında oturum açın. Daha fazla bilgi için bkz. [PowerShell Ile oturum açma](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

   ```azurepowershell-interactive
    C:\> Connect-AzAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
   ```

3. Yeni iş ortağı KIMLIĞINE bağlantı. İş ortağı KIMLIĞI, kuruluşunuzun [Microsoft iş ortağı ağı](https://partner.microsoft.com/) kimliğidir.

    ```azurepowershell-interactive
    C:\> new-AzManagementPartner -PartnerId 12345
    ```

#### <a name="get-the-linked-partner-id"></a>Bağlı iş ortağı KIMLIĞINI al
```azurepowershell-interactive
C:\> get-AzManagementPartner
```

#### <a name="update-the-linked-partner-id"></a>Bağlı iş ortağı KIMLIĞINI güncelleştirme
```azurepowershell-interactive
C:\> Update-AzManagementPartner -PartnerId 12345
```
#### <a name="delete-the-linked-partner-id"></a>Bağlı iş ortağı KIMLIĞINI silme
```azurepowershell-interactive
C:\> remove-AzManagementPartner -PartnerId 12345
```

### <a name="use-the-azure-cli-to-link-to-a-new-partner-id"></a>Azure CLı kullanarak yeni bir iş ortağı KIMLIĞINE bağlantı
1. Azure CLı uzantısını yükler.

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ```

2. Kullanıcı hesabı ya da hizmet sorumlusu ile müşterinin kiracısında oturum açın. Daha fazla bilgi için bkz. [Azure CLI Ile oturum açma](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ```

3. Yeni iş ortağı KIMLIĞINE bağlantı. İş ortağı KIMLIĞI, kuruluşunuzun [Microsoft iş ortağı ağı](https://partner.microsoft.com/) kimliğidir.

     ```azurecli-interactive
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>Bağlı iş ortağı KIMLIĞINI al
```azurecli-interactive
C:\ az managementpartner show
```

#### <a name="update-the-linked-partner-id"></a>Bağlı iş ortağı KIMLIĞINI güncelleştirme
```azurecli-interactive
C:\ az managementpartner update --partner-id 12345
```

#### <a name="delete-the-linked-partner-id"></a>Bağlı iş ortağı KIMLIĞINI silme
```azurecli-interactive
C:\ az managementpartner delete --partner-id 12345
```

## <a name="next-steps"></a>Sonraki adımlar

Güncelleştirmeleri almak veya geri bildirim göndermek için [Microsoft Iş ortağı topluluğundaki](https://aka.ms/PALdiscussion) tartışmaya katın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**İş ortağı KIMLIĞINI kimler bağlayabilirler?**

İş ortağı kuruluştan bir müşterinin Azure kaynaklarını yöneten tüm kullanıcılar iş ortağı KIMLIĞINI hesaba bağlayabilir.

**Bir iş ortağı KIMLIĞI, bağlandıktan sonra değiştirilebilir mi?**

Evet. Bağlı bir iş ortağı KIMLIĞI değiştirilebilir, eklenebilir veya kaldırılabilir.

**Bir kullanıcının birden fazla müşteri kiracısında hesabı varsa ne olacak?**

İş ortağı KIMLIĞI ve hesap arasındaki bağlantı, her müşteri kiracısı için yapılır. Her müşteri kiracısında iş ortağı KIMLIĞINI bağlayın.

**Diğer iş ortakları veya müşteriler iş ortağı KIMLIĞI bağlantısını düzenleyebilir veya kaldırabilir mi?**

Bağlantı, Kullanıcı hesabı düzeyinde ilişkilendirilir. Yalnızca iş ortağı KIMLIĞI bağlantısını düzenleyebilir veya kaldırabilirsiniz. Müşteri ve diğer iş ortakları, iş ortağı KIMLIĞI bağlantısını değiştiremezler.


**Şirketim birden çok olduğunda hangi MPN KIMLIĞINI kullanmalıyım?**

Ortak konum hesapları ve ilişkili MPN kimlikleri, bağlama iş ortağı KIMLIĞI için kullanılmalıdır.  [Iş ortağı hesapları](https://docs.microsoft.com/partner-center/account-structure) hakkında daha fazla bilgi edinin

**Bağlı iş ortağı KIMLIĞI için gelir raporlamasını etkileyenlerini nerede bulabilirim?**

Bulut ürün performansı raporlama, Iş Ortağı Merkezi 'ndeki iş ortakları tarafından [Içgörüme panolarım](https://partner.microsoft.com/membership/reports/myinsights)tarafından kullanılabilir. İş ortağı ilişkilendirme türü olarak Iş ortağı Yöneticisi bağlantısı ' nı seçmeniz gerekir.

**Neden müşterimi raporlarda göremiyorum?**

Aşağıdaki nedenlerden dolayı müşterileri raporlarda göremezsiniz

1. Bağlantılı Kullanıcı hesabının herhangi bir müşteri Azure aboneliğine veya kaynağına [rol tabanlı erişimi](https://docs.microsoft.com/azure/role-based-access-control/overview) yoktur.

2. Kullanıcının [rol tabanlı erişim](https://docs.microsoft.com/azure/role-based-access-control/overview) erişimine sahip olduğu Azure aboneliğinde hiç kullanım yoktur.

**İş ortağı kimliği bağlantısı Azure Stack ile çalışır mı?**

Evet, Azure Stack için iş ortağı kimliğinizi bağlayabilirsiniz.
