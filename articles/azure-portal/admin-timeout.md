---
title: Azure portal kullanıcıları için dizin düzeyinde eylemsizlik zaman aşımını ayarla | Microsoft Docs
description: Yöneticiler, oturum kapatmadan önce en fazla boşta kalma süresini uygulayabilir. Etkin olmama zaman aşımı ilkesi dizin düzeyinde ayarlanır.
services: azure-portal
keywords: ayarlar, zaman aşımı
author: mblythe
ms.author: mblythe
ms.date: 12/19/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 55136b5418b0c455ef66bd322f519c1e52114b93
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75640508"
---
# <a name="set-directory-level-inactivity-timeout"></a>Dizin düzeyinde eylemsizlik zaman aşımını ayarla

Eylemsizlik zaman aşımı ayarı, kullanıcılar iş istasyonlarını güvenli hale getirmeye unutur durumunda kaynaklarınızın yetkisiz erişimden korunmasına yardımcı olur. Bir Kullanıcı bir süredir boşta kaldığında Azure portal oturumu otomatik olarak imzalanır. Yöneticiler, oturum kapatmadan önce en fazla boşta kalma süresini uygulayabilir. Etkin olmama zaman aşımı ayarı dizin düzeyinde geçerlidir. Dizinler hakkında daha fazla bilgi için bkz. [Active Directory Domain Services genel bakış](/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).

## <a name="configure-the-inactive-timeout-setting"></a>Etkin olmayan zaman aşımı ayarını yapılandırın

Yönetici değilseniz ve Azure portal tüm kullanıcıları için boşta kalma zaman aşımı ayarını zorlamak istiyorsanız aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Genel sayfa üstbilgisindeki **ayarları** seçin.
3. **Dizin düzeyindeki zaman aşımını yapılandır**bağlantı metnini seçin.

    ![Bağlantı metni vurgulanmış şekilde Portal ayarlarını gösteren ekran görüntüsü](./media/admin-timeout/settings.png)

4. Yeni bir sayfa açılır. **Dizin düzeyi eylemsizlik zaman aşımını yapılandır** sayfasında, ayarı açmak için **Azure Portal için Dizin düzeyi boşta kalma zaman aşımını etkinleştir** ' i seçin.
5. Sonra, oturumu otomatik olarak kapatmadan önce bir kullanıcının boşta kalabileceği en uzun süreyi **saat** ve **dakika** cinsinden girin.
6. **Uygula**’yı seçin.

    ![Dizin düzeyinde eylemsizlik zaman aşımını ayarlamak için sayfayı gösteren ekran görüntüsü](./media/admin-timeout/configure.png)

Etkin olmayan zaman aşımı ilkesinin doğru ayarlandığını onaylamak için genel sayfa başlığından **Bildirimler** ' i seçin. Bir başarı bildiriminin listelendiğini doğrulayın.

  ![Dizin düzeyinde eylemsizlik zaman aşımı için başarılı bildirim iletisini gösteren ekran görüntüsü](./media/admin-timeout/confirmation.png)

Bu ayar yeni oturumlar için geçerli olur. Zaten oturum açmış olan kullanıcılar için hemen uygulanmaz.

> [!NOTE]
> Yönetici dizin düzeyinde bir zaman aşımı ayarı yapılandırmışsa, kullanıcılar ilkeyi geçersiz kılabilir ve kendi etkin olmayan oturum açma süresini ayarlayabilir. Ancak Kullanıcı, dizin düzeyinde ayarlandıklardan daha az bir zaman aralığı seçmesi gerekir.
>

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portal tercihlerinizi ayarlama](set-preferences.md)
* [Kullanıcı ayarlarını dışarı aktarma veya silme](azure-portal-export-delete-settings.md)
* [Yüksek karşıtlığı açma veya tema değiştirme](azure-portal-change-theme-high-contrast.md)
