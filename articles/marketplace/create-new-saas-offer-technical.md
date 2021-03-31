---
title: Microsoft Iş Ortağı Merkezi 'nde SaaS teklifinizin teknik ayrıntılarını ekleme
description: Microsoft ticari Market için hizmet olarak yazılım (SaaS) teklifi için teknik ayrıntılar sağlamayı öğrenin.
author: mingshen-ms
ms.author: mingshen
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 09/02/2020
ms.openlocfilehash: 28c0be40387f411286230f94c19fa23a80e650af
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96746412"
---
# <a name="how-to-add-technical-details-for-your-saas-offer"></a>SaaS teklifinizin teknik ayrıntılarını ekleme

Bu makalede, Microsoft Commercial Market 'in çözümünüze bağlanmasına yardımcı olacak teknik ayrıntıların nasıl girilmesi açıklanmaktadır. Bu bağlantı, teklifi edinmeyi ve yönetmeyi tercih ettiklerinde müşteri için teklifinizi sağlamamızı sağlar. Bu ayarlar hakkında daha fazla bilgi için bkz. [Teknik bilgiler](plan-saas-offer.md#technical-information).

> [!NOTE]
> İşlemleri bağımsız olarak işlemeyi seçerseniz, bu seçeneği görmezsiniz. Bunun yerine, [SaaS teklifinizi satma](create-new-saas-offer-marketing.md)bölümüne atlayın.

## <a name="technical-configuration"></a>Teknik yapılandırma

**Teknik yapılandırma** sekmesinde, ticari Market 'in SaaS uygulamanız veya çözümünüz ile iletişim kurmak için kullandığı teknik ayrıntıları tanımlayacaksınız. 

- **Giriş sayfası URL 'si** (gerekli) – `https://contoso.com/signup` müşterilerin ticari marketten teklifinizi aldıktan sonra ve yeni oluşturulan SaaS aboneliğinden yapılandırma işlemini tetiklemesini sağlayacak SaaS Web sitesi URL 'sini (örneğin:) tanımlayın.

  > [!IMPORTANT]
  > Giriş sayfanız 24/7 olmalıdır ve çalışıyor olmalıdır. Bu, ticari Market 'te sunulan SaaS tekliflerinizin yeni satınalmaları veya bir teklifin etkin bir aboneliğinin yapılandırma istekleri hakkında bildirim almanız için tek yoldur.

- **Bağlantı Web kancası** (gerekli) – Microsoft 'un size gönderilmesi gereken tüm zaman uyumsuz olaylar için (örneğin, SaaS aboneliği iptal edildi), bir bağlantı Web kancası URL 'si sağlamanızı istiyoruz. Olayı size bildirmek için bu URL 'YI çağıracağız.

  > [!IMPORTANT]
  > Comercial Marketi aracılığıyla satın alınan müşterilerinizin SaaS abonelikleriyle ilgili güncelleştirmeler hakkında bildirim almanız gereken tek yol, Web kancası çalışıyor ve 24/7 çalıştırıyor olmalıdır.

- **Azure Active Directory KIRACı kimliği** (gerekli) – Azure Active Directory (Azure AD) uygulamanıza AIT Kiracı kimliğini bulmak için Azure Active Directory içindeki [uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) dikey penceresine gidin. **Görünen ad** sütununda, uygulamayı seçin. Ardından listelenen **Dizin (kiracı) kimliği** numarasını (örneğin, `50c464d3-4930-494c-963c-1e951d15360e` ) arayın.

- **Azure Active Directory Uygulama kimliği** (gerekli) – [uygulama kimliğinizi](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in)bulmak için Azure Active Directory [uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) dikey penceresine gidin. **Görünen ad** sütununda, uygulamayı seçin. Ardından listelenen uygulama (istemci) KIMLIĞI numarasını (örneğin, `50c464d3-4930-494c-963c-1e951d15360e` ) arayın.

Sonraki sekmeye geçmeden önce **Taslağı kaydet** ' i seçin: plana genel bakış.

## <a name="next-steps"></a>Sonraki adımlar

- [SaaS teklifiniz için planlar oluşturma](create-new-saas-offer-plans.md).