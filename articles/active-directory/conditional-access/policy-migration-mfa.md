---
title: Koşullu Erişim ilkelerini geçir - Azure Etkin Dizini
description: Bu makalede, Azure portalında çok faktörlü kimlik doğrulaması gerektiren klasik bir ilkenin nasıl geçirilen bir görünüm gösterilmektedir.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/04/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: d637ac464b689a25ce5d5a79cf47da0c85d38d0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74846053"
---
# <a name="migrate-a-classic-policy-in-the-azure-portal"></a>Azure portalında klasik bir ilke yi geçirme

Bu makalede, bir bulut uygulaması için **çok faktörlü kimlik doğrulaması** gerektiren klasik bir ilkenin nasıl geçirilen nasıl geçirilir. Ön koşul olmasa da, klasik ilkelerinizi geçiş yapmaya başlamadan önce [Azure portalında geçir](policy-migration.md) klasik ilkeleri okumanızı öneririz.

![Salesforce uygulaması için MFA gerektiren klasik politika ayrıntıları](./media/policy-migration/33.png)

Geçiş işlemi aşağıdaki adımlardan oluşur:

1. Yapılandırma ayarlarını almak için [klasik ilkeyi açın.](#open-a-classic-policy)
1. Klasik ilkenizi değiştirmek için yeni bir Azure AD Koşullu Erişim ilkesi oluşturun. 
1. Klasik politikayı devre dışı kılmış olur.

## <a name="open-a-classic-policy"></a>Klasik bir ilke açma

1. Azure [portalında,](https://portal.azure.com) **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**gidin.
1. Seçin, **Klasik ilkeler**.

   ![Klasik ilkeler görünümü](./media/policy-migration-mfa/12.png)

1. Klasik ilkeler listesinde, geçirmek istediğiniz ilkeyi seçin. Yapılandırma ayarlarını yeni bir Koşullu Erişim ilkesiyle yeniden oluşturabilmeniz için belgeleyin.

## <a name="create-a-new-conditional-access-policy"></a>Yeni bir Koşullu Erişim ilkesi oluşturma

1. Azure [portalında,](https://portal.azure.com) **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**gidin.
1. Yeni bir Koşullu Erişim ilkesi oluşturmak için **Yeni ilkeyi**seçin.
1. **Yeni** sayfada, **Ad** metin kutusunda, ilkeniz için bir ad yazın.
1. **Atamalar** bölümünde, **Kullanıcılar ve gruplar'ı**tıklatın.
   1. Klasik ilkenizde tüm kullanıcılar seçildiyse, **Tüm kullanıcılar'ı**tıklatın. 
   1. Klasik politikanızda seçili gruplar varsa, **kullanıcıları ve grupları seçin'i**tıklatın ve ardından gerekli kullanıcı ve grupları seçin.
   1. Dışlanan gruplar adediniz varsa, **Dışla** sekmesini tıklatın ve ardından gerekli kullanıcıları ve grupları seçin. 
   1. **Bitti'yi** seçin
1. **Atama** bölümünde, **Bulut uygulamaları veya eylemleri'ni**tıklatın.
1. Bulut **uygulamaları veya eylemleri** sayfasında aşağıdaki adımları gerçekleştirin:
   1. **Uygulamaları Seç'i**tıklatın.
   1. **Seç'i**tıklatın.
   1. **Seç** sayfasında bulut uygulamanızı seçin ve ardından **Seç'i**tıklatın.
   1. Bulut **uygulamaları** **sayfasında, Bitti'yi**tıklatın.
1. Çok **faktörlü kimlik doğrulamasını gerektirir:**
   1. Access **denetimleri** **bölümünde, Ver'i**tıklatın.
   1. **Hibe** sayfasında, **Access'i Ver'i**tıklatın ve ardından **çok faktörlü kimlik doğrulamayı gerektir'i**tıklatın.
   1. **Seç'i**tıklatın.
1. İlkenizin etkinlemesini etkinleştirmek için **Üzerine'yi** tıklatın ve ardından **Kaydet'i**seçin.

   ![Koşullu Erişim ilkesi oluşturma](./media/policy-migration-mfa/conditional-access-policy-migration.png)

## <a name="disable-the-classic-policy"></a>Klasik politikayı devre dışı

Klasik ilkenizi devre dışı kabmak için **Ayrıntılar** görünümünde **Devre Dışı Nı** tıklatın.

![Klasik ilkeleri devre dışı](./media/policy-migration-mfa/14.png)

## <a name="next-steps"></a>Sonraki adımlar

- Klasik ilke geçişi hakkında daha fazla bilgi için Azure [portalındaki klasik ilkeleri Geçir'e](policy-migration.md)bakın.
- [Yeni ilke kararlarının etkisini belirlemek için Koşullu Erişim için yalnızca rapor modunu kullanın.](concept-conditional-access-report-only.md)