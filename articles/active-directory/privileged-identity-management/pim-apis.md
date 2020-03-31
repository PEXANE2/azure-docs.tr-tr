---
title: PIM için Microsoft Grafik API'leri (Önizleme) - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM) için Microsoft Grafik API'lerini kullanma hakkında bilgi sağlar (Önizleme).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 01/02/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6da6bffbc54bfa6e9c39ddace665eb7cfec58614
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75638672"
---
# <a name="microsoft-graph-apis-for-privileged-identity-management-preview"></a>Ayrıcalıklı Kimlik Yönetimi için Microsoft Grafik API'leri (Önizleme)

Azure Etkin Dizini için [Microsoft Grafik API'lerini](https://developer.microsoft.com/graph/docs/concepts/overview) kullanarak tüm Ayrıcalıklı Kimlik Yönetimi görevlerini gerçekleştirebilirsiniz. Bu makalede, Ayrıcalıklı Kimlik Yönetimi için Microsoft Graph API'lerini kullanmak için önemli kavramlar açıklanmaktadır.

Microsoft Graph API'leri hakkında ayrıntılı bilgi için [Azure AD Ayrıcalıklı Kimlik Yönetimi API başvurusuna](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)göz atın.

> [!IMPORTANT]
> Microsoft Graph'taki /beta sürümü altındaki API'ler önizlemededir ve değiştirilebilir. Bu API'lerin üretim uygulamalarında kullanımı desteklenmez.
>
> Tüm PIM işlemleri Grafik API komutlarımız aracılığıyla desteklenir, ancak etkinleştirme isteklerini onaylamak için farklı bir sistem kullanırız. Onay için Grafik API şu anda geliştirilmektedir ve önümüzdeki birkaç ay içinde piyasaya sürülecektir.

## <a name="required-permissions"></a>Gerekli izinler

Ayrıcalıklı Kimlik Yönetimi için Microsoft Grafik API'lerini aramak için aşağıdaki izinlerden **birine veya birkaçına** sahip olmalısınız:

- `Directory.AccessAsUser.All`
- `Directory.Read.All`
- `Directory.ReadWrite.All`
- `PrivilegedAccess.ReadWrite.AzureAD`

### <a name="set-permissions"></a>İzinleri ayarlama

Uygulamaların Ayrıcalıklı Kimlik Yönetimi için Microsoft Grafik API'lerini aramaları için gerekli izinlere sahip olmaları gerekir. Gerekli izinleri belirtmenin en kolay yolu [Azure AD onay çerçevesini](../develop/consent-framework.md)kullanmaktır.

### <a name="set-permissions-in-graph-explorer"></a>Grafik Gezgini'nde izinleri ayarlama

Aramalarınızı sınamak için Grafik Gezgini kullanıyorsanız, araçtaki izinleri belirtebilirsiniz.

1. Genel Yönetici olarak [Graph Explorer'da](https://developer.microsoft.com/graph/graph-explorer) oturum açın.

1. **İzinleri değiştir'i**tıklatın.

    ![Graph Explorer - izinleri değiştirin](./media/pim-apis/graph-explorer.png)

1. Eklemek istediğiniz izinlerin yanındaki onay kutularını seçin. `PrivilegedAccess.ReadWrite.AzureAD`Grafik Gezgini'nde henüz kullanıma sunulmadı.

    ![Graph Explorer - izinleri değiştirin](./media/pim-apis/graph-explorer-modify-permissions.png)

1. İzin değişikliklerini uygulamak için **İzinleri Değiştir'i** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Ayrıcalıklı Kimlik Yönetimi API başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)
