---
title: Azure AD Etki Alanı Hizmetleri için SKU'yu değiştirme | Microsoft Dokümanlar
description: İş gereksinimleriniz değişirse, Azure AD Etki Alanı Hizmetleri yönetilen etki alanı için SKU katmanının nasıl değiştirileceğini öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: iainfou
ms.openlocfilehash: b65310569e95173b88dd0aa0dfe1dbacd86cc8fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79126708"
---
# <a name="change-the-sku-for-an-existing-azure-ad-domain-services-managed-domain"></a>Varolan bir Azure AD Etki Alanı Hizmetleri yönetilen etki alanı için SKU'yu değiştirme

Azure Etkin Dizin Etki Alanı Hizmetleri'nde (Azure AD DS) kullanılabilir performans ve özellikler SKU türüne dayanır. Bu özellik farklılıkları yedekleme sıklığını veya en fazla tek yönlü giden orman güvenlerinin sayısını (şu anda önizlemede) içerir. Yönetilen etki alanını oluştururken bir SKU seçersiniz ve yönetilen etki alanı dağıtıldıktan sonra işletmegereksinimleriniz değiştikçe SKU'ları yukarı veya aşağı değiştirebilirsiniz. İş gereksinimlerindeki değişiklikler, daha sık yedekleme gereksinimini veya ek orman güvenleri oluşturmayı içerebilir. Farklı SKU'ların sınırları ve fiyatlandırması hakkında daha fazla bilgi için [Azure AD DS SKU kavramları][concepts-sku] ve Azure AD [DS fiyatlandırma][pricing] sayfalarına bakın.

Bu makalede, Azure portalını kullanarak mevcut bir Azure AD DS yönetilen etki alanı için SKU'yu nasıl değiştireceğiniz gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, bir Azure [Active Directory Etki Alanı Hizmetleri örneği oluşturmak ve yapılandırmak][create-azure-ad-ds-instance]için öğreticiyi tamamlayın.

## <a name="sku-change-limitations"></a>SKU değişiklik sınırlamaları

Azure AD DS yönetilen etki alanı dağıtıldıktan sonra SNU'ları yukarı veya aşağı değiştirebilirsiniz. Ancak, bir kaynak ormanı kullanıyorsanız (şu anda önizlemede) ve Azure AD DS'den şirket içi AD DS ortamına tek yönlü giden orman güvenleri oluşturduysanız, SKU değiştirme işlemi için bazı sınırlamalar vardır. *Premium* ve *Kurumsal* SK'ler oluşturabileceğiniz güven sayısında bir sınır tanımlar. Şu anda yapılandırdığınızdan daha düşük bir maksimum sınıra sahip bir SKU'ya geçiş yapamazsınız.

Örnek:

* *Premium* SKU'da iki orman tröstü oluşturduysanız, *Standart* SKU'ya geçiş yapamazsınız. *Standart* SKU orman güvenlerini desteklemez.
* Veya *Premium* SKU'da yedi güven oluşturduysanız, *Enterprise* SKU'ya geçiş yapamazsınız. *Enterprise* SKU en fazla beş güveni destekler.

Bu sınırlar hakkında daha fazla bilgi için [Azure AD DS SKU özellikleri ve limitlerine][concepts-sku]bakın.

## <a name="select-a-new-sku"></a>Yeni bir SKU seçin

Azure portalını kullanarak Azure AD DS yönetilen bir etki alanının SKU'yu değiştirmek için aşağıdaki adımları tamamlayın:

1. Azure portalının üst kısmında, Azure **AD Etki Alanı Hizmetlerini**arayın ve seçin. Aaddscontoso.com *gibi*yönetilen etki alanınızı listeden seçin.
1. Azure AD DS sayfasının sol tarafındaki menüde **Ayarlar > SKU'yu**seçin.

    ![Azure portalında Azure AD DS yönetilen etki alanınız için SKU menüsü seçeneğini seçin](media/change-sku/overview-change-sku.png)

1. Açılan menüden Azure AD DS yönetilen etki alanınız için istediğiniz SKU'yu seçin. Kaynak ormanınvarsa, orman güvenleri yalnızca *Enterprise* SKU'da veya daha yüksek olan standart SKU'yu seçemezsiniz. *Standard*

    Açılan menüden istediğiniz SKU'yu seçin ve ardından **Kaydet'i**seçin.

    ![Azure portalındaki açılır menüden gerekli SKU'yu seçin](media/change-sku/change-sku-selection.png)

SKU türünü değiştirmek bir veya iki dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak ormanınız varsa ve SKU değişikliğinden sonra ek güvenler oluşturmak istiyorsanız, bkz. [Azure AD DS'deki bir şirket içi etki alanına giden orman güveni oluştur (önizleme).][create-trust]

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[concepts-sku]: administration-concepts.md#azure-ad-ds-skus
[create-trust]: tutorial-create-forest-trust.md

<!-- EXTERNAL LINKS -->
[pricing]: https://azure.microsoft.com/pricing/details/active-directory-ds/
