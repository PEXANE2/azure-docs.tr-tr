---
title: Azure portalı kullanıcıları için dizin düzeyinde etkinlik zaman larını ayarlama | Microsoft Dokümanlar
description: Yöneticiler, oturum oturumu niskene gitmeden önce en fazla boşta kalma süresini zorlayabilir. Hareketsizlik zaman adabı dizini dizin düzeyinde ayarlanır.
services: azure-portal
keywords: ayarlar, zaman sonu
author: mgblythe
ms.author: mblythe
ms.date: 02/10/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: e27135d09da7060f2a948e37f6026fe66fbef5b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79096617"
---
# <a name="set-directory-level-inactivity-timeout"></a>Dizin düzeyinde etkinlik zaman ayarı ayarlama

Etkinlik dışı zaman ayarı, kullanıcılar iş istasyonlarını güvenli hale almayı unutursa kaynaklarınızı yetkisiz erişimden korumaya yardımcı olur. Bir kullanıcı bir süre boşta kaldığında, Azure portal oturumu otomatik olarak oturumu tamamlar. Genel Yönetici [rolündeki](../active-directory/users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator) yöneticiler, oturum oturumun oturumun oturumunun imzalanmasından önce en fazla boşta kalma süresini zorlayabilir. Hareketsizlik zaman ayarı dizin düzeyinde geçerlidir. Dizinler hakkında daha fazla bilgi için [Active Directory Domain Services Genel Bakış'a](/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)bakın.

## <a name="configure-the-inactive-timeout-setting"></a>Etkin olmayan zaman aşım ayarını yapılandırma

Global Administrator iseniz ve Azure portalının tüm kullanıcıları için boşta zaman ayarı uygulamak istiyorsanız aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Genel sayfa üstbilgisinden **Ayarlar'ı** seçin.
3. Bağlantı **metnini seçin Dizin düzeyi zaman aşıntını yapılandırın.**

    ![Bağlantı metni vurgulanmış portal ayarlarını gösteren ekran görüntüsü](./media/admin-timeout/settings.png)

4. Yeni bir sayfa açılır. Yapı **dizin düzeyi etkinlik zaman aşım** sayfasında, Azure portalının ayarı açmak **için dizin düzeyinde boşta kalma süresini etkinleştir'i** seçin.
5. Ardından, oturum otomatik olarak oturumu oturumu nisbeten oturumu niçin bir kullanıcının boşta kabileceği maksimum süre için **Saat** ve **Dakika'yı** girin.
6. **Uygula**’yı seçin.

    ![Dizin düzeyinde etkinlik zaman ayarı ayarlamak için sayfayı gösteren ekran görüntüsü](./media/admin-timeout/configure.png)

Hareketsizlik zaman dilimi ilkesinin doğru ayarlandığını doğrulamak için, genel sayfa üstbilgisinden **Bildirimler'i** seçin. Başarı bildiriminin listeli olduğunu doğrulayın.

  ![Dizin düzeyinde etkinlik zaman ayarı için başarılı bildirim iletisi gösteren ekran görüntüsü](./media/admin-timeout/confirmation.png)

Ayar, yeni oturumlar için etkindir. Oturum açmış olan kullanıcılar için hemen geçerli olmayacaktır.

> [!NOTE]
> Bir Global Administrator dizin düzeyinde bir zaman ayarı yapılandıysa, kullanıcılar ilkeyi geçersiz kılabilir ve kendi etkin olmayan oturum açma sürelerini ayarlayabilir. Ancak, kullanıcı nın Genel Yönetici tarafından dizin düzeyinde ayarlanandan daha az bir zaman aralığı seçmesi gerekir.
>

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalı tercihlerinizi ayarlama](set-preferences.md)
* [Kullanıcı ayarlarını dışarı aktarma veya silme](azure-portal-export-delete-settings.md)
* [Yüksek karşıtlığı açma veya tema değiştirme](azure-portal-change-theme-high-contrast.md)
