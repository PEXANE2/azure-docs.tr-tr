---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/12/2020
ms.author: mimart
ms.openlocfilehash: bf8b9370abc58902d9c751505f633dee852210a6
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78184375"
---
#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. **Kayıtlı uygulamaya** Genel Bakış sayfasında, **Ayarlar**' ı seçin.
1. **API erişimi**altında **gerekli izinler**' i seçin.
1. **Microsoft Graph**seçin.
1. **Uygulama izinleri**altında, yönetim uygulamanıza verilecek iznin onay kutusunu seçin. Örnek:
    * **Tüm denetim günlüğü verilerini okuyun**: dizinin denetim günlüklerini okumak için bu izni seçin.
    * **Dizin verilerini okuma ve yazma**: Kullanıcı geçişi veya Kullanıcı yönetimi senaryoları için bu izni seçin.
    * **Kuruluşunuzun güven çerçevesi Ilkelerini okuyun ve yazın**: sürekli tümleştirme/sürekli teslım (CI/CD) senaryoları için bu izni seçin. Örneğin, Azure Pipelines ile özel ilke dağıtımı.
1. **Kaydet**’i seçin.
1. **Izin ver**' i seçin ve ardından **Evet**' i seçin. İzinlerin tam olarak yayılması birkaç dakika sürebilir.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. **Yönet**altında **API izinleri**' ni seçin.
1. **Yapılandırılan izinler**altında **izin Ekle**' yi seçin.
1. **Microsoft API 'leri** sekmesini seçin ve ardından **Microsoft Graph**' yi seçin.
1. **Uygulama izinleri**' ni seçin.
1. Uygun izin grubunu genişletin ve yönetim uygulamanıza verilecek iznin onay kutusunu seçin. Örnek:
    * **Auditlog** > **Denetimgünlüğü. Read. All**: dizinin denetim günlüklerini okumak için.
    * **Dizin** > **Directory. ReadWrite. All**: Kullanıcı geçişi veya Kullanıcı yönetimi senaryoları için.
    * **İlke** > **Policy. ReadWrite. TrustFramework**: sürekli tümleştirme/sürekli teslım (CI/CD) senaryoları için. Örneğin, Azure Pipelines ile özel ilke dağıtımı.
1. **Izin Ekle**' yi seçin. Yönlendirildiğinden, bir sonraki adıma geçmeden önce birkaç dakika bekleyin.
1. **Yönetici onayı ver ' i (kiracı adınız)** seçin.
1. Şu anda oturum açmış olan yönetici hesabınızı seçin veya Azure AD B2C kiracınızda, en azından *bulut uygulama Yöneticisi* rolüne atanan bir hesapla oturum açın.
1. **Kabul Et**’i seçin.
1. **Yenile**' yi seçin ve ardından "verilen..." öğesini doğrulayın **durum**' un altında görüntülenir. İzinlerin yayılması birkaç dakika sürebilir.
