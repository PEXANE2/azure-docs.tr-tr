---
title: Azure AD Etki Alanı Hizmetleri'ndeki eşleşmemiş dizin hatalarını düzeltme | Microsoft Dokümanlar
description: Azure AD Etki Alanı Hizmetlerinde eşleşmeyan bir dizin hatasının ne anlama geldiğini ve nasıl çözüleceğinizi öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 40eb75b7-827e-4d30-af6c-ca3c2af915c7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 03/31/2020
ms.author: iainfou
ms.openlocfilehash: 7a74d5e609c615bb6b973eceebb144c58e46f7cd
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80519466"
---
# <a name="resolve-mismatched-directory-errors-for-existing-azure-ad-domain-services-managed-domains"></a>Mevcut Azure AD Etki Alanı Hizmetleri yönetilen etki alanları için eşleşmemiş dizin hatalarını çözme

Azure Etkin Dizin Etki Alanı Hizmetleri (Azure AD DS) yönetilen etki alanı eşleşmeyen bir kiracı hatası gösteriyorsa, yönetilen etki alanını çözülene kadar yönetemezsiniz. Bu hata, temel azure sanal ağı farklı bir Azure REKLAM dizinine taşınırsa oluşur.

Bu makalede, hatanın neden oluştuğu ve nasıl çözüleceği açıklanmaktadır.

## <a name="what-causes-this-error"></a>Bu hataya ne neden olur?

Bir Azure AD DS yönetilen etki alanı ve sanal ağ iki farklı Azure AD kiracısına ait olduğunda, eşleşmeyen bir dizin hatası gerçekleşir. Örneğin, Contoso'nun Azure AD kiracısında çalışan *aaddscontoso.com* adlı bir Azure AD DS yönetilen etki alanınız olabilir. Ancak, yönetilen etki alanı için Azure sanal ağı Fabrikam Azure AD kiracısının bir parçasıdır.

Azure, kaynaklara erişimi sınırlamak için rol tabanlı erişim denetimi (RBAC) kullanır. Azure AD DS'yi bir Azure AD kiracısında etkinleştirdiğinizde, kimlik bilgisi açıkları yönetilen etki alanına eşitlenir. Bu işlem, Azure AD dizininin kiracı yöneticisi olmanızı gerektirir ve kimlik bilgilerine erişim denetlenmelidir. Kaynakları bir Azure sanal ağına dağıtmak ve trafiği denetlemek için, Azure AD DS'yi dağıttığınız sanal ağda yönetim ayrıcalıklarına sahip olmalısınız.

RBAC'ın Tutarlı bir şekilde çalışabilmesi ve Azure AD DS'nin kullandığı tüm kaynaklara güvenli bir şekilde erişabilmesi için yönetilen etki alanının ve sanal ağın aynı Azure AD kiracısına ait olması gerekir.

Kaynak Yöneticisi ortamında aşağıdaki kurallar geçerlidir:

- Azure REKLAM dizininde birden çok Azure aboneliği olabilir.
- Azure aboneliğinde sanal ağlar gibi birden çok kaynak olabilir.
- Azure AD dizini için tek bir Azure AD Etki Alanı Hizmetleri yönetilen etki alanı etkinleştirilir.
- Azure AD Etki Alanı Hizmetleri yönetilen etki alanı, aynı Azure AD kiracısı içindeki Azure aboneliklerinden herhangi biri için sanal ağda etkinleştirilebilir.

### <a name="valid-configuration"></a>Geçerli yapılandırma

Aşağıdaki örnek dağıtım senaryosunda, Contoso Azure AD DS yönetilen etki alanı Contoso Azure AD kiracısında etkinleştirilir. Yönetilen etki alanı, Contoso Azure AD kiracısına ait bir Azure aboneliğine ait sanal bir ağda dağıtılır. Hem yönetilen etki alanı hem de sanal ağ aynı Azure AD kiracısına aittir. Bu örnek yapılandırma geçerlidir ve tam olarak desteklenir.

![Aynı Azure AD kiracısının yönetilen etki alanı ve sanal ağ bölümüyle geçerli Azure AD DS kiracı yapılandırması](./media/getting-started/valid-tenant-config.png)

### <a name="mismatched-tenant-configuration"></a>Eşleşmemiş kiracı yapılandırması

Bu örnek dağıtım senaryosunda, Contoso Azure AD DS yönetilen etki alanı Contoso Azure AD kiracısında etkinleştirilir. Ancak, yönetilen etki alanı, Fabrikam Azure AD kiracısına ait bir Azure aboneliğine ait sanal bir ağda dağıtılır. Yönetilen etki alanı ve sanal ağ iki farklı Azure AD kiracısına aittir. Bu örnek yapılandırma eşleşmeyen bir kiracıdır ve desteklenmez. Sanal ağ, yönetilen etki alanıyla aynı Azure AD kiracısına taşınmalıdır.

![Eşleşmemiş kiracı yapılandırması](./media/getting-started/mismatched-tenant-config.png)

## <a name="resolve-mismatched-tenant-error"></a>Eşleşmeyan kiracı hatalarını çözümle

Aşağıdaki iki seçenek eşleşmez dizin hatasını çözer:

* [Azure AD DS yönetilen etki alanını](delete-aadds.md) mevcut Azure AD dizininizden silin. Kullanmak istediğiniz sanal ağla aynı Azure AD dizininde [değiştirilen bir Azure AD DS yönetilen etki alanı oluşturun.](tutorial-create-instance.md) Hazır olduğunuzda, daha önce silinen etki alanına yeniden yönetilen etki alanına katılan tüm makinelere katılın.
* Sanal ağı içeren [Azure aboneliğini](../cost-management-billing/manage/billing-subscription-transfer.md) Azure AD DS yönetilen etki alanıyla aynı Azure REKLAM dizinine taşıyın.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS ile sorun giderme sorunları hakkında daha fazla bilgi için [sorun giderme kılavuzuna](troubleshoot.md)bakın.
