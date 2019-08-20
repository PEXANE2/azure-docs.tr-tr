---
title: Azure AD Domain Services eşleşmeyen Dizin hatalarını çözümle | Microsoft Docs
description: Mevcut Azure AD Domain Services yönetilen etki alanları için eşleşmeyen Dizin hatalarını anlayın ve çözün
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 40eb75b7-827e-4d30-af6c-ca3c2af915c7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: iainfou
ms.openlocfilehash: 4978f7b782271daff996807172a24103bd8d9860
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69617290"
---
# <a name="resolve-mismatched-directory-errors-for-existing-azure-ad-domain-services-managed-domains"></a>Mevcut Azure AD Domain Services yönetilen etki alanları için eşleşmeyen Dizin hatalarını çözümle
Azure AD Domain Services yönetilen bir etki alanınız var. Azure portal gitmeniz ve yönetilen etki alanını görüntülediğinizde, aşağıdaki hata iletisini görürsünüz:

![Eşleşmeyen Dizin hatası](./media/getting-started/mismatched-tenant-error.png)

Hata çözülene kadar bu yönetilen etki alanını yönetemezsiniz.


## <a name="whats-causing-this-error"></a>Bu hataya neden olan nedir?
Bu hata, yönetilen etki alanınız ve sanal ağınızın etkinleştirildiği durumlarda iki farklı Azure AD kiracılarına ait olduğunda meydana gelir. Örneğin, ' contoso.com ' adlı bir yönetilen etki alanınız var ve contoso Azure AD kiracısı için etkinleştirildi. Ancak, yönetilen etki alanının etkinleştirildiği Azure sanal ağı, farklı bir Azure AD kiracısı olan Fabrikam 'a aittir.

Yeni Azure portal (ve özellikle Azure AD Domain Services uzantısı) Azure Resource Manager oluşturulmuştur. Modern Azure Resource Manager ortamında bazı kısıtlamalar, kaynaklara göre daha fazla güvenlik ve rol tabanlı erişim denetimi (RBAC) sunmak için zorlanır. Azure AD kiracısı için Azure AD Domain Services etkinleştirme, kimlik bilgisi karmalarının yönetilen etki alanıyla eşitlenmesine neden olduğundan hassas bir işlemdir. Bu işlem, dizin için bir kiracı yöneticisi olmanızı gerektirir. Ayrıca, yönetilen etki alanını etkinleştirdiğiniz sanal ağ üzerinde yönetici ayrıcalıklarına sahip olmanız gerekir. RBAC denetimlerinin tutarlı bir şekilde çalışması için, yönetilen etki alanı ve sanal ağ aynı Azure AD kiracısına ait olmalıdır.

Kısaca, başka bir Azure AD kiracısı olan ' fabrikam.com ' tarafından sahip olunan bir Azure aboneliğine ait olan bir sanal ağda Azure AD kiracısı ' contoso.com ' için yönetilen bir etki alanını etkinleştiremezsiniz. 

**Geçerli yapılandırma**: Bu dağıtım senaryosunda, contoso Managed etki alanı contoso Azure AD kiracısı için etkinleştirilir. Yönetilen etki alanı, contoso Azure AD kiracının sahip olduğu bir Azure aboneliğine ait olan bir sanal ağda kullanıma sunulur. Bu nedenle, hem yönetilen etki alanı hem de sanal ağ aynı Azure AD kiracısına aittir. Bu yapılandırma geçerli ve tam olarak destekleniyor.

![Geçerli kiracı yapılandırması](./media/getting-started/valid-tenant-config.png)

**Eşleşmeyen kiracı yapılandırması**: Bu dağıtım senaryosunda, contoso Managed etki alanı contoso Azure AD kiracısı için etkinleştirilir. Ancak, yönetilen etki alanı fabrikam Azure AD kiracının sahip olduğu bir Azure aboneliğine ait olan bir sanal ağda kullanıma sunulur. Bu nedenle, yönetilen etki alanı ve sanal ağ iki farklı Azure AD kiracılarına aittir. Bu yapılandırma eşleşmeyen kiracı yapılandırması ve desteklenmiyor. Sanal ağ, yönetilen etki alanı ile aynı Azure AD kiracısına (contoso) taşınmalıdır. Ayrıntılar için [çözüm](#resolution) bölümüne bakın.

![Eşleşmeyen kiracı yapılandırması](./media/getting-started/mismatched-tenant-config.png)

Bu nedenle, yönetilen etki alanı ve üzerinde etkin olduğu sanal ağ, iki farklı Azure AD kiracılarına ait olduğunda bu hatayı görürsünüz.

Kaynak Yöneticisi ortamında aşağıdaki kurallar geçerlidir:
- Azure AD dizininde birden çok Azure aboneliği olabilir.
- Bir Azure aboneliğinde, sanal ağlar gibi birden fazla kaynak olabilir.
- Azure AD dizini için tek bir Azure AD Domain Services yönetilen etki alanı etkinleştirilir.
- Azure AD Domain Services yönetilen bir etki alanı, aynı Azure AD kiracısındaki Azure aboneliklerinden herhangi birine ait olan bir sanal ağ üzerinde etkinleştirilebilir.


## <a name="resolution"></a>Çözüm
Eşleşmeyen Dizin hatasını çözümlemek için iki seçeneğiniz vardır. Şunları yapabilirsiniz:

- Varolan yönetilen etki alanını silmek için **Sil** düğmesine tıklayın. Yönetilen etki alanı ve sanal ağın Azure AD dizinine ait olması için [Azure Portal](https://portal.azure.com)kullanarak yeniden oluşturun. Daha önce silinen etki alanına katılmış tüm makineleri yeni oluşturulan yönetilen etki alanına ekleyin.

- Sanal ağı içeren Azure aboneliğini, yönetilen etki alanınızı ait olduğu Azure AD dizinine taşıyın. [Azure aboneliğinin sahipliğini başka bir hesaba aktarma](../billing/billing-subscription-transfer.md) makalesindeki adımları izleyin.


## <a name="related-content"></a>İlgili içerik
* [Azure AD Domain Services-Başlarken Kılavuzu](tutorial-create-instance.md)
* [Sorun giderme kılavuzu-Azure AD Domain Services](troubleshoot.md)
