---
title: Azure AD Yetkilendirme Yönetimi (Önizleme) içindeki Katalog oluşturucuları için erişim yönetimini devretmek-Azure Active Directory
description: BT yöneticilerinde erişim yönetimini, erişimi yönetebilmeleri için Katalog oluşturucuları ve proje yöneticilerine nasıl atayacağınızı öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/07/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: da6183599f2dacd1a98d8fe359edda741b903b50
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170769"
---
# <a name="delegate-access-governance-to-catalog-creators-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi (Önizleme) içindeki Katalog oluşturucuları için erişim yönetimine temsilci seçme

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmeyebilir veya kısıtlı özelliklere sahip olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Yönetici olmayan kullanıcılara temsilci seçmek için, bu kullanıcıları kendi kataloglarını oluşturabilmeleri için Azure AD Yetkilendirme Yönetimi tanımlı Katalog Oluşturucu rolüne ekleyebilirsiniz. Bireysel kullanıcılar ekleyebilir veya üyeleri, daha sonra kataloglar oluşturabilebilen bir grup ekleyebilirsiniz.

## <a name="as-an-it-administrator-delegate-to-a-catalog-creator"></a>BT Yöneticisi olarak, bir katalog oluşturucuya temsilci seçme

Bir kullanıcıyı Katalog Oluşturucu rolüne atamak için aşağıdaki adımları izleyin.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol taraftaki menüde, **Yetkilendirme Yönetimi** bölümünde, **Ayarlar**' a tıklayın.

1. **Düzenle**' ye tıklayın.

    ![Katalog oluşturucuları ekleme ayarları](./media/entitlement-management-delegate/settings-delegate.png)

1. **Temsilci yetkilendirme yönetimi** bölümünde, bu yetkilendirme yönetimi rolünü atamak istediğiniz kullanıcıları veya grupları seçmek için **Katalog oluşturucuları Ekle** ' ye tıklayın.

1. **Seçin**’e tıklayın.

1. **Kaydet**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Kaynak kataloğu oluşturma ve yönetme](entitlement-management-catalog-create.md)
- [Paket yöneticilerine erişim yönetimi yetkisi verme](entitlement-management-delegate-managers.md)

