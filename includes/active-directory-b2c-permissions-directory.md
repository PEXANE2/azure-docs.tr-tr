---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/12/2020
ms.author: mimart
ms.openlocfilehash: f8c972bdb9195008c2983d3993e8d9369749b284
ms.sourcegitcommit: 3988965cc52a30fc5fed0794a89db15212ab23d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85200167"
---
#### <a name="app-registrations"></a>[Uygulama kayıtları](#tab/app-reg-ga/) 

1. **Yönet**altında **API izinleri**' ni seçin.
1. **Yapılandırılan izinler**altında **izin Ekle**' yi seçin.
1. **Microsoft API 'leri** sekmesini seçin ve ardından **Microsoft Graph**' yi seçin.
1. **Uygulama izinleri**' ni seçin.
1. Uygun izin grubunu genişletin ve yönetim uygulamanıza verilecek iznin onay kutusunu seçin. Örneğin:
    * **AuditLog**  >  **Denetimgünlüğü. Read. All**: dizinin denetim günlüklerini okumak için.
    * **Dizin**  >  **Directory. ReadWrite. All**: Kullanıcı geçişi veya Kullanıcı yönetimi senaryoları için.
    * **İlke**  >  **Policy. ReadWrite. TrustFramework**: sürekli tümleştirme/sürekli teslım (CI/CD) senaryoları için. Örneğin, Azure Pipelines ile özel ilke dağıtımı.
1. **Izin Ekle**' yi seçin. Yönlendirildiğinden, bir sonraki adıma geçmeden önce birkaç dakika bekleyin.
1. **Yönetici onayı ver ' i (kiracı adınız)** seçin.
1. Şu anda genel yönetici hesabı ile oturum açmadıysanız, Azure AD B2C kiracınızdaki bir hesapla oturum açarak en az *bulut uygulama Yöneticisi* rolüne atanır ve ardından **yönetici onayı ver (kiracı adınız)** seçeneğini belirleyin.
1. **Yenile**' yi seçin ve ardından "verilen..." öğesini doğrulayın **durum**' un altında görüntülenir. İzinlerin yayılması birkaç dakika sürebilir.

#### <a name="applications-legacy"></a>[Uygulamalar (eski)](#tab/applications-legacy/)

1. **Kayıtlı uygulamaya** Genel Bakış sayfasında, **Ayarlar**' ı seçin.
1. **API erişimi**altında **gerekli izinler**' i seçin.
1. **Microsoft Graph**seçin.
1. **Uygulama izinleri**altında, yönetim uygulamanıza verilecek iznin onay kutusunu seçin. Örneğin:
    * **Tüm denetim günlüğü verilerini okuyun**: dizinin denetim günlüklerini okumak için bu izni seçin.
    * **Dizin verilerini okuma ve yazma**: Kullanıcı geçişi veya Kullanıcı yönetimi senaryoları için bu izni seçin.
    * **Kuruluşunuzun güven çerçevesi Ilkelerini okuyun ve yazın**: sürekli tümleştirme/sürekli teslım (CI/CD) senaryoları için bu izni seçin. Örneğin, Azure Pipelines ile özel ilke dağıtımı.
1. **Kaydet**’i seçin.
1. **Izin ver**' i seçin ve ardından **Evet**' i seçin. İzinlerin tam olarak yayılması birkaç dakika sürebilir.
