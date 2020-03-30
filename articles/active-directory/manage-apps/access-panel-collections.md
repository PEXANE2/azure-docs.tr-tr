---
title: Azure Active Directory'de Uygulamalarım için koleksiyonlar oluşturun | Microsoft Dokümanlar
description: Son kullanıcılarınız için uygulamalarım için daha basit bir Uygulamam deneyimi için Uygulamalarım sayfalarını özelleştirmek için Uygulamalarım koleksiyonlarını kullanın. Uygulamaları ayrı sekmelere sahip gruplar halinde düzenleyin.
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
ms.topic: article
ms.date: 02/10/2020
ms.author: mimart
ms.reviewer: kasimpso
ms.collection: M365-identity-device-management
ms.openlocfilehash: c91b9ffc9e3487e492c91cb0f5825d0b725f9410
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77120089"
---
# <a name="create-collections-on-the-my-apps-portal"></a>Uygulamalarım portalında koleksiyon oluşturma

Kullanıcılarınız, erişebildikleri bulut tabanlı uygulamaları görüntülemek ve başlatmak için Uygulamalarım portalını kullanabilir. Varsayılan olarak, bir kullanıcının erişebileceği tüm uygulamalar tek bir sayfada birlikte listelenir. Bu sayfayı kullanıcılarınız için daha iyi düzenlemek için, Azure AD Premium P1 veya P2 lisansınız varsa koleksiyonlar ayarlayabilirsiniz. Koleksiyonla, ilişkili uygulamaları (örneğin, iş rolüne, göreve veya projeye göre) gruplayabilir ve bunları ayrı bir sekmede görüntüleyebilirsiniz. Bir koleksiyon aslında bir filtreyi kullanıcının zaten erişebileceği uygulamalara uygular, böylece kullanıcı yalnızca koleksiyonda kendilerine atanmış olan uygulamaları görür.

> [!NOTE]
> Bu makalede, bir yöneticinin koleksiyonları nasıl etkinleştirebileceği ve oluşturabileceği ele alınabilir. Son kullanıcı için Uygulamalarım portalı ve koleksiyonlarının nasıl kullanılacağı hakkında bilgi için [Access'e](https://docs.microsoft.com/azure/active-directory/user-help/my-applications-portal-workspaces)bakın ve koleksiyonları kullanın.

## <a name="enable-the-latest-my-apps-features"></a>En son Uygulamalarım özelliklerini etkinleştirin

1. Azure [**portalını**](https://portal.azure.com/) açın ve kullanıcı yöneticisi veya Global Administrator olarak oturum açın.

2. Azure **Etkin Dizin** > **Kullanıcı ayarlarına**gidin.

3. **Kullanıcı özellik önizlemeleri**altında, **kullanıcı özelliği önizleme ayarlarını yönet'i**seçin.

4. Kullanıcılar Altında **Uygulamalarım için önizleme özelliklerini kullanabilir,** aşağıdaki seçeneklerden birini seçin:
   * **Seçili** - Belirli bir grubun özelliklerini etkinleştirin. Özellikleri etkinleştirmek istediğiniz grubu seçmek için **grup seç** seçeneğini kullanın.  
   * **Tüm** - Tüm kullanıcılar için özellikleri sağlar.

> [!NOTE]
> Uygulamalarım portalını açmak için kullanıcılar `https://myapps.microsoft.com` kuruluşunuz `https://myapps.microsoft.com/contoso.com`için özelleştirilmiş bağlantıyı veya özelleştirilmiş bağlantıyı kullanabilir. Yeni Uygulamalarım deneyimini etkinleştirdikten sonra, **Güncelleştirilmiş Uygulamalarım deneyimi,** Uygulamalarım sayfamın üst kısmında görüntülenir ve kullanıcılar yeni deneyimi görüntülemek için **Tazyideneyin'i** seçebilir. Yeni deneyimi kullanmayı bırakmak için kullanıcılar sayfanın üst kısmındaki **Yeni Deneyim Bırak** başlığından **Evet'i** seçebilir.

## <a name="create-a-collection"></a>Koleksiyon oluşturma

Koleksiyon oluşturmak için Azure AD Premium P1 veya P2 lisansınolması gerekir.

1. Azure [**portalını**](https://portal.azure.com/) açın ve Azure AD Premium P1 veya P2 lisansıyla yönetici olarak oturum açın.

2. Azure **Active Directory** > **Enterprise Applications'a**gidin.

3. **Yönet**altında, **Koleksiyonlar'ı**seçin.

4. **Yeni koleksiyonu**seçin. Yeni **koleksiyon** sayfasına, koleksiyon için bir **Ad** girin (adda "koleksiyon" kullanmamanızı öneririz. Sonra bir **Açıklama**girin.

   ![Yeni toplama sayfası](media/acces-panel-collections/new-collection.png)

5. **Uygulamalar** sekmesini seçin. Uygulama **Ekle + Ekle**ve ardından Uygulama **Ekle** sayfasında, koleksiyona eklemek istediğiniz tüm uygulamaları seçin veya uygulamaları bulmak için **Arama** kutusunu kullanın.

   ![Koleksiyona uygulama ekleme](media/acces-panel-collections/add-applications.png)

6. Uygulama eklemeyi bitirdiğinizde **Ekle'yi**seçin. Seçili uygulamaların listesi görüntülenir. Listedeki uygulamaların sırasını değiştirmek için yukarı okları kullanabilirsiniz. Bir uygulamayı aşağı taşımak veya koleksiyondan silmek için **Daha Fazla** menüsünü (**...**) seçin.

7. **Sahipleri** sekmesini seçin . Select **+ Ekle kullanıcıları ve grupları**, ve sonra Ekle kullanıcıları ve **gruplar** sayfasında, sahiplik atamak istediğiniz kullanıcıları veya grupları seçin. Kullanıcıları ve grupları seçmeyi **bitirdiğinizde, Seç'i**seçin.

9. Kullanıcılar **ve gruplar** sekmesini seçin. + **Kullanıcıları ve grupları seçin**ve ardından kullanıcı ve **gruplar** ekle sayfasında, koleksiyonu atamak istediğiniz kullanıcıları veya grupları seçin. Veya kullanıcıları veya grupları bulmak için **Arama** kutusunu kullanın. Kullanıcıları ve grupları seçmeyi **bitirdiğinizde, Seç'i**seçin.

   ![Kullanıcı ve grup ekleme](media/acces-panel-collections/add-users-and-groups.png)

11. **Gözden Geçir + Oluştur'u**seçin. Yeni koleksiyonun özellikleri görüntülenir.


## <a name="view-audit-logs"></a>Denetim günlüklerini görüntüleme

Denetim günlükleri, koleksiyon oluşturma son kullanıcı eylemleri de dahil olmak üzere Uygulamalarım koleksiyon işlemlerini kaydeder. Uygulamalarım'dan aşağıdaki olaylar oluşturulur:

* Koleksiyon oluşturma
* Koleksiyonu düzenle
* Koleksiyonu silme
* Bir uygulama başlatma (son kullanıcı)
* Self servis uygulama ekleme (son kullanıcı)
* Self servis uygulama silme (son kullanıcı)

Etkinlik **bölümündeki Azure Active Directory** > **Enterprise Applications** > **Audit günlüklerini** seçerek Azure [portalındaki](https://portal.azure.com) denetim günlüklerine erişebilirsiniz. **Hizmet**için **Uygulamalarım'ı**seçin.

## <a name="get-support-for-my-account-pages"></a>Hesabım sayfaları için destek alın

Uygulamalarım sayfasından, bir kullanıcı hesap ayarlarını açmak için **Hesabımı** > **Görüntüle'yi** seçebilir. Azure AD **Hesabım** sayfasında, kullanıcılar güvenlik bilgilerini, cihazlarını, parolalarını ve daha fazlasını yönetebilir. Ayrıca Office hesap ayarlarına da erişebilirler.

Azure AD hesabı sayfası veya Office hesabı sayfasıyla ilgili bir sorun için destek isteği göndermeniz gerekiyorsa, isteğinizin düzgün yönlendirilen için aşağıdaki adımları izleyin: 

* Azure AD **"Hesabım"** sayfasındaki sorunlar için Azure portalı içinden bir destek isteği açın. Azure **portalı** > **Azure Active Directory** > Yeni destek**isteğine**gidin.

* **Office "Hesabım"** sayfasındaki sorunlar için Microsoft 365 yönetici merkezi içinden bir destek isteği açın. Microsoft **365 yönetici merkezi** > **Desteği'ne**gidin. 

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory'deki uygulamalar için son kullanıcı deneyimleri](end-user-experiences.md)