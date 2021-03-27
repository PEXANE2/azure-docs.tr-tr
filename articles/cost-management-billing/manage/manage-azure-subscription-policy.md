---
title: Azure abonelik ilkelerini yönetme
description: Azure aboneliklerinin nasıl yönetileceğini, dizinlerden ve dizinlerinden denetlemek için Azure abonelik ilkelerini yönetme hakkında bilgi edinin.
author: anuragdalmia
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 03/10/2021
ms.reviewer: banders
ms.author: andalmia
ms.openlocfilehash: 105e090655021ed4046f8880ef9816d7f7ff559f
ms.sourcegitcommit: a9ce1da049c019c86063acf442bb13f5a0dde213
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2021
ms.locfileid: "105631879"
---
# <a name="manage-azure-subscription-policies"></a>Azure abonelik ilkelerini yönetme

>[!NOTE]
>Bu özellik şu anda önizleme aşamasındadır ve yavaş şekilde kullanıma sunulmakta olduğundan, herkes Azure portal bu deneyimi henüz göremez.

Bu makale, Azure aboneliklerinin dizin ve dizinlere taşınmasını denetleyen abonelik işlemleri için Azure abonelik ilkelerini yapılandırmanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

- Yalnızca dizin [genel yöneticileri](../../active-directory/roles/permissions-reference.md#global-administrator) , abonelik ilkelerini düzenleyebilir. Abonelik ilkelerini düzenlemeden önce, genel yöneticinin [tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişimi yükselmelidir](../../role-based-access-control/elevate-access-global-admin.md). Ardından, abonelik ilkelerini düzenleyebilir.
- Diğer tüm kullanıcılar yalnızca geçerli ilke ayarını okuyabilir.

## <a name="available-subscription-policy-settings"></a>Kullanılabilir abonelik ilkesi ayarları

Azure aboneliklerinin dizin ve dizinlere taşınmasını denetlemek için aşağıdaki ilke ayarlarını kullanın.

### <a name="subscriptions-leaving-aad-directory"></a>AAD dizininden çıkılırken abonelikler

İlke, kullanıcıların abonelikleri geçerli dizinden taşımasını sağlar veya engeller. [Abonelik sahipleri](../../role-based-access-control/built-in-roles.md#owner) [, bir Azure aboneliğinin dizinini](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md) üye oldukları başka birine değiştirebilir. Yönetim sorunları doğurur, bu nedenle Genel Yöneticiler Dizin kullanıcılarının dizini değiştirmesine izin verebilir veya bu izni vermez.

### <a name="subscriptions-entering-aad-directory"></a>AAD dizinine giren abonelikler

İlke, mevcut dizine erişimi olan diğer dizinlerden, abonelikleri geçerli dizine taşımak için kullanıcılara izin verir veya bunları engeller. [Abonelik sahipleri](../../role-based-access-control/built-in-roles.md#owner) [, bir Azure aboneliğinin dizinini](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md) üye oldukları başka birine değiştirebilir. Yönetim sorunları doğurur, bu nedenle Genel Yöneticiler Dizin kullanıcılarının dizini değiştirmesine izin verebilir veya bu izni vermez.

### <a name="exempted-users"></a>Muaf tutulan kullanıcılar

Genel Yöneticiler, idare nedenleriyle, tüm abonelik Dizin taşımalarını geçerli dizinden daha fazla bir şekilde engelleyebilir. Ancak, belirli kullanıcıların iki işlem yapmasına izin vermek isteyebilir. Her iki durumda da, kullanıcıların başka herkes için geçerli olan ilke ayarını atlamasına izin veren muaf tutulan kullanıcıların bir listesini yapılandırabilirler.

## <a name="setting-subscription-policy"></a>Abonelik ilkesi ayarlanıyor

1. [Azure portalında](https://portal.azure.com/) oturum açın.
1. **Abonelikler**' e gidin. **Ilkeleri Yönet** komutu, komut çubuğunda gösterilir.  
    :::image type="content" source="./media/manage-azure-subscription-policy/subscription-blade-manage-policies.png" alt-text="Aboneliklerde Ilkeleri Yönet ' i gösteren ekran görüntüsü." lightbox="./media/manage-azure-subscription-policy/subscription-blade-manage-policies.png" :::
1. Dizin için ayarlanmış geçerli abonelik ilkeleri hakkındaki ayrıntıları görüntülemek için **Ilkeleri Yönet** ' i seçin. [Yükseltilmiş izinlere](../../role-based-access-control/elevate-access-global-admin.md) sahip bir genel yönetici, muaf tutulan kullanıcıları ekleme veya kaldırma dahil olmak üzere ayarlarda düzenleme yapabilir.  
    :::image type="content" source="./media/manage-azure-subscription-policy/subscription-blade-policies.png" alt-text="Belirli ilke ayarlarını ve muaf tutulan kullanıcıları gösteren ekran görüntüsü." lightbox="./media/manage-azure-subscription-policy/subscription-blade-policies.png" :::
1. Değişiklikleri kaydetmek için alttaki **Değişiklikleri Kaydet** ' i seçin. Değişiklikler hemen geçerli olur.

## <a name="read-subscription-policy"></a>Abonelik ilkesini oku

Genel olmayan yöneticiler, Dizin ilke ayarlarını görüntülemek için abonelik ilkesi alanına gidebilirler. Bunlar herhangi bir düzenleme yapamaz. Bu kişiler, gizlilik nedenleriyle muaf tutulan kullanıcıların listesini göremez. Bunlar, dizin ayarları izin vermedikçe, ilke değişiklikleri için istekleri göndermek üzere genel yöneticilerini görüntüleyebilirler.

:::image type="content" source="./media/manage-azure-subscription-policy/subscription-blade-policies-reader.png" alt-text="Aboneliklerdeki ilkeleri okuyucu olarak yönetme gösteren ekran görüntüsü." lightbox="./media/manage-azure-subscription-policy/subscription-blade-policies-reader.png" :::

## <a name="next-steps"></a>Sonraki adımlar

- [Maliyet yönetimi + faturalandırma belgelerini](../index.yml) okuyun