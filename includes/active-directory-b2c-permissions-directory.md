---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/12/2020
ms.author: mimart
ms.openlocfilehash: bf8b9370abc58902d9c751505f633dee852210a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184375"
---
#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. Kayıtlı **uygulamaya** genel bakış sayfasında **Ayarlar'ı**seçin.
1. **API Access**altında, **Gerekli izinleri**seçin.
1. **Microsoft Graph'ı**seçin.
1. **Uygulama İzinleri**altında, yönetim başvurunuza vermek için izin onay kutusunu seçin. Örnek:
    * **Tüm denetim günlüğü verilerini okuyun**: Dizinin denetim günlüklerini okumak için bu izni seçin.
    * **Dizin verilerini okuma ve yazma**: Kullanıcı geçişi veya kullanıcı yönetimi senaryoları için bu izni seçin.
    * **Kuruluşunuzun güven çerçevesi ilkelerini okuyun ve yazın**: Sürekli tümleştirme/sürekli teslim (CI/CD) senaryoları için bu izni seçin. Örneğin, Azure Ardışık Hatları ile özel ilke dağıtımı.
1. **Kaydet'i**seçin.
1. **Hibe izinlerini**seçin ve ardından **Evet'i**seçin. İzinlerin tam olarak yayılması birkaç dakika sürebilir.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. **Yönet**altında, **API izinlerini**seçin.
1. **Yapılandırılmış izinler**altında, **İzin ekle'yi**seçin.
1. Microsoft **API'leri** sekmesini seçin ve ardından **Microsoft Graph'ı**seçin.
1. **Uygulama izinlerini**seçin.
1. Uygun izin grubunu genişletin ve yönetim başvurunuza vermek için izin onay kutusunu seçin. Örnek:
    * **AuditLog** > **AuditLog.Read.All**: Dizinin denetim günlüklerini okumak için.
    * **Dizin** > **Dizini.ReadWrite.All**: Kullanıcı geçişi veya kullanıcı yönetimi senaryoları için.
    * **Policy** > **Policy.ReadWrite.TrustFramework**: Sürekli tümleştirme/sürekli teslimat (CI/CD) senaryoları için. Örneğin, Azure Ardışık Hatları ile özel ilke dağıtımı.
1. **İzin Ekle'yi**seçin. Yönlendirildikçe, bir sonraki adıma geçmeden önce birkaç dakika bekleyin.
1. **(kiracı adınız) için Grant yönetici onayı**seçin.
1. Şu anda oturum açmış yönetici hesabınızı seçin veya En azından *Bulut uygulama yöneticisi* rolüne atanmış Olan Azure AD B2C kiracınızda bir hesapla oturum açın.
1. **Kabul Et**’i seçin.
1. **Yenile'yi**seçin ve ardından "Verilenler..." **Durum**altında görünür. İzinlerin yayılması birkaç dakika sürebilir.
