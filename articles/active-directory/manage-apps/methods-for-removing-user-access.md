---
title: Bir kullanıcının bir uygulamaya erişimi nasıl kaldırılır | Microsoft Dokümanlar
description: Bir kullanıcının uygulamaya erişimini nasıl kaldırlayacağımı anlama
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/17/2018
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b69502995eff88df53af3671a8e611809f83e59
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65826108"
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>Bir kullanıcının bir uygulamaya erişimi kaldırma

Bu makale, bir kullanıcının bir uygulamaya erişimini nasıl kaldırabileceğinizi anlamanıza yardımcı olur.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Belirli bir kullanıcının veya grubun atamasını bir uygulamaya kaldırmak istiyorum

Bir uygulamaya kullanıcı veya grup atamasını kaldırmak için, Azure Active Directory makalesindeki [bir kurumsal uygulamadan kullanıcı veya grup atamasını kaldır'da](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) listelenen adımları izleyin.

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>Her kullanıcı için bir uygulamaya tüm erişimi devre dışı kalmak istiyorum

Bir uygulamada tüm kullanıcı oturum açmalarını devre dışı katmak [için, Azure Active Directory makalesinde bir kurumsal uygulama için kullanıcı oturum açmalarını devre dışı bırak'da](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) listelenen adımları izleyin.

## <a name="i-want-to-delete-an-application-entirely"></a>Bir uygulamayı tamamen silmek istiyorum

**Bir uygulamayı silmek**için aşağıdaki yönergeleri izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **Tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Silmek istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, üst uygulamanın **Genel Bakış** bölmesinden **Sil** simgesini tıklatın.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Gelecekteki tüm kullanıcı onayı işlemlerini herhangi bir uygulamaya devre dışı kalmak istiyorum

Tüm dizininizin kullanıcı onayını devre dışı bırakmak, son kullanıcıların herhangi bir uygulamaya onay vermesini engeller. Yöneticiler yine de kullanıcı adına izin verebilir. Uygulama onayı hakkında daha fazla bilgi için ve bunu neden yapmak isteyip istemediğiniz hakkında, [Kullanıcı yı ve yönetici onayLarını anlama](../develop/howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent)'yı okuyun. Ayrıca bakınız, [İzinler ve onay.](../develop/v2-permissions-and-consent.md)

**Tüm dizininizdeki gelecekteki tüm kullanıcı onayı işlemlerini devre dışı kakmak**için aşağıdaki yönergeleri izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Azure **Etkin Dizin Uzantısını** Açma 

3.  Gezinti menüsünde **Kurumsal uygulamaları** tıklatın.

5.  **Kullanıcı ayarlarını**tıklatın.

6.  Kullanıcıların ayarla, **uygulamaların kendi adlarına şirket verilerine erişmesine izin verebilir** ve **"Hayır"** düğmesini tıklattı.


## <a name="next-steps"></a>Sonraki adımlar

[Uygulamalara erişimi yönetme](what-is-access-management.md)
