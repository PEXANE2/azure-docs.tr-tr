---
title: Kullanıcının bir uygulamaya erişimini kaldırma | Microsoft Docs
description: Bir kullanıcının uygulamaya erişiminin nasıl kaldırılacağını anlayın
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 10/17/2018
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f9626c256755e2fce81b593d95b8680f4bb55ee
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84763168"
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>Kullanıcının bir uygulamaya erişimini kaldırma

Bu makale, bir kullanıcının uygulamaya erişimini nasıl kaldırabileceğinizi anlamanıza yardımcı olur.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Belirli bir kullanıcının veya grubun atamasını bir uygulamaya kaldırmak istiyorum

Bir uygulamaya Kullanıcı veya grup atamasını kaldırmak için, Azure Active Directory makalesinde bir [Kurumsal uygulamadan Kullanıcı veya grup atamasını kaldırma](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) bölümünde listelenen adımları izleyin.

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>Her Kullanıcı için bir uygulamaya tüm erişimi devre dışı bırakmak istiyorum

Bir uygulamaya tüm Kullanıcı oturum açma işlemlerini devre dışı bırakmak için, [Azure Active Directory makalesinde bir kurumsal uygulama için Kullanıcı oturum açma Işlemlerini devre dışı bırak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) ' da listelenen adımları izleyin.

## <a name="i-want-to-delete-an-application-entirely"></a>Bir uygulamayı tamamen silmek istiyorum

**Bir uygulamayı silmek**için şu yönergeleri izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. Tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Silmek istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra üst uygulamanın **genel bakış** bölmesindeki **Sil** simgesine tıklayın.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Tüm gelecek Kullanıcı onay işlemlerini herhangi bir uygulamaya devre dışı bırakmak istiyorum

Tüm dizininiz için Kullanıcı onayını devre dışı bırakmak, son kullanıcıların herhangi bir uygulamaya yarışmasını önler. Yöneticiler kullanıcı adına yine de izin verebilir. Uygulama onayı hakkında daha fazla bilgi edinmek ve bunu yapmak istememenizin nedeni, [Kullanıcı ve yönetici onayını anlama](../develop/howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent)konusunu okuyun. Ayrıca bkz. [izinler ve onay](../develop/v2-permissions-and-consent.md).

Tüm **dizininizde tüm gelecek Kullanıcı onay işlemlerini devre dışı bırakmak**için şu yönergeleri izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2.  **Azure Active Directory uzantısını** açın 

3.  Gezinti menüsünde **Kurumsal uygulamalar** ' a tıklayın.

5.  **Kullanıcı ayarları**' na tıklayın.

6.  Kullanıcılar, **uygulamaların kendi adına şirket verilerine erişmesine izin** verebilir **Hayır** ' a geçiş yapar ve Kaydet düğmesine tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

[Uygulamalara erişimi yönetme](what-is-access-management.md)
