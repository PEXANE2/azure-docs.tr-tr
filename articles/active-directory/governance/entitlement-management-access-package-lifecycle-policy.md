---
title: Azure AD yetkilendirme yönetiminde bir erişim paketi için yaşam döngüsü ayarlarını değiştirme - Azure Etkin Dizin
description: Azure Active Directory yetkilendirme yönetiminde bir erişim paketi için yaşam döngüsü ayarlarını nasıl değiştireceğinizi öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 959d85f496a4a573a969bf736aba137d5b86154a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261989"
---
# <a name="change-lifecycle-settings-for-an-access-package-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde bir erişim paketi için yaşam döngüsü ayarlarını değiştirme

Erişim paketi yöneticisi olarak, varolan bir ilkeyi düzenleyerek bir erişim paketinin yaşam döngüsü ayarlarını istediğiniz zaman değiştirebilirsiniz. Bir ilkenin son kullanma tarihini değiştirirseniz, bekleyen onay veya onaylı durumda olan isteklerin son kullanma tarihi değişmez.

Bu makalede, varolan bir erişim paketinin yaşam döngüsü ayarlarının nasıl değiştirilen açıklanmaktadır.

## <a name="open-lifecycle-settings"></a>Yaşam döngüsü ayarlarını açın

Bir erişim paketinin yaşam döngüsü ayarlarını değiştirmek için ilgili ilkeyi açmanız gerekir. Bir erişim paketinin yaşam döngüsü ayarlarını açmak için aşağıdaki adımları izleyin.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. **İlkeler'i** tıklatın ve ardından dönüştürmek istediğiniz yaşam döngüsü ayarlarına sahip ilkeyi tıklatın.

    İlke ayrıntıları bölmesi sayfanın alt kısmında açılır.

    ![Erişim paketi - İlke ayrıntıları bölmesi](./media/entitlement-management-shared/policy-details.png)

1. İlkeyi yeniden yapmak için **Edit'i** tıklatın.

    ![Erişim paketi - İlkeyi güncelle](./media/entitlement-management-shared/policy-edit.png)

1. Yaşam **döngüsü** ayarlarını açmak için Yaşam Döngüsü sekmesini tıklatın.

[!INCLUDE [Entitlement management lifecycle policy](../../../includes/active-directory-entitlement-management-lifecycle-policy.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketi için istek ve onay ayarlarını değiştirme](entitlement-management-access-package-request-policy.md)