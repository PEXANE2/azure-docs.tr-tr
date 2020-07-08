---
title: Azure AD Galeri uygulaması ekleme sorunu | Microsoft Docs
description: Azure AD Galeri uygulamaları eklerken herkesin karşılaştığı yaygın sorunları ve bunları çözmek için neler yapabileceğinizi anlayın
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
ms.topic: troubleshooting
ms.date: 09/11/2018
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4387a3ca02297311cd9ab025c315ca3324cbbbe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84760414"
---
# <a name="problem-adding-an-azure-ad-gallery-application"></a>Azure AD Galeri uygulaması ekleme sorunu

Bu makale, Azure AD Galeri uygulamaları eklerken herkesin karşılaştığı yaygın sorunları ve bunları çözmek için yapabileceklerinizi anlamanıza yardımcı olur.

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a>"Ekle" düğmesine tıkladım ve uygulamamın görünmesi uzun sürdü

Bazı durumlarda, bir uygulamanın dizininize eklendikten sonra görünmesi 1-2 dakika (ve bazen daha uzun süre) alabilir. Bu, normal beklenen performans olmadığından, [Azure Portal](https://portal.azure.com/) uygulamanın sağ üst köşesindeki **Bildirimler** simgesine (zil) tıklayarak ve **uygulama ekleme** etiketli **devam eden** veya **tamamlanmış** bildirimi arayarak uygulama ekleme işleminin devam ettiğini görebilirsiniz.

Uygulamanız hiçbir zaman eklenmediyse veya **Ekle** düğmesine tıkladığınızda bir hata yaşarsanız **hata** durumunda bir **bildirim** görürsünüz. Bir destek mühendisiyle daha fazla bilgi edinmek veya paylaşmak için hata hakkında daha fazla ayrıntı isterseniz, bir [Portal bildiriminin ayrıntılarını görme](#how-to-see-the-details-of-a-portal-notification) bölümündeki adımları izleyerek hata hakkında daha fazla bilgi görebilirsiniz.

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a>"Ekle" düğmesine tıkladım ve Uygulamam görünmüyor

Bazen geçici sorunlar, ağ sorunları veya bir hata nedeniyle uygulama ekleme başarısız olur. Azure portal, bu durum, uygulamanın sağ üst köşesinde bulunan **Bildirimler** simgesine (zil) tıkladığınızda ve **uygulama bildiriminin eklenmesinin** yanında kırmızı (!) bir simge görüyorsanız bunu söyleyebilir. Bu, uygulama oluşturulurken bir hata olduğunu gösterir.

**Ekle** düğmesine tıkladığınızda bir hatayla karşılaşırsanız bir **hata** durumunda **bildirim** görürsünüz. Bir destek mühendisiyle daha fazla bilgi edinmek veya paylaşmak için hata hakkında daha fazla ayrıntı isterseniz, bir [Portal bildiriminin ayrıntılarını görme](#how-to-see-the-details-of-a-portal-notification) bölümündeki adımları izleyerek hata hakkında daha fazla bilgi görebilirsiniz.

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a>Uygulamamın eklendikten sonra nasıl ayarlanacağını bilmiyorum

Uygulamalar hakkında bilgi edinmek için yardıma ihtiyacınız varsa, [SaaS uygulamalarını Azure Active Directory makaleyle tümleştirme hakkında öğreticilerin listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) başlamak için iyi bir yerdir.

Buna ek olarak, [Azure AD uygulamaları belge kitaplığı](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) , Azure AD ile çoklu oturum açma ve nasıl çalıştığı hakkında daha fazla bilgi edinmenize yardımcı olur.

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Portal bildiriminin ayrıntılarını görme

Aşağıdaki adımları izleyerek, herhangi bir portal bildiriminin ayrıntılarını görebilirsiniz:

1.  Azure portal sağ üst köşesindeki **Bildirimler** simgesini (zil) seçin

2.  Bir **hata** durumunda (bunların yanında kırmızı (!) herhangi bir bildirim seçin.

    >[!NOTE]
    >**Başarılı** veya **devam eden** durumunda bildirimlere tıklayükleyemezsiniz.
    >
    >

4.  Sorun hakkında daha fazla ayrıntıyı anlamak için **bildirim ayrıntıları** altındaki bilgileri kullanın.

5.  Hala yardıma ihtiyacınız varsa, sorununuz hakkında yardım almak için bu bilgileri bir destek mühendisiyle veya ürün grubuyla de paylaşabilirsiniz.

6.  Tüm bildirim ayrıntılarını bir destek veya ürün grubu mühendisiyle paylaşmak üzere kopyalamak için **kopyalama hatası** metin kutusunun sağ tarafındaki **Kopyala** **simgesine** tıklayın.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Destek mühendisine bildirim ayrıntıları göndererek yardım alma

Daha hızlı yardımcı olmaları için yardıma ihtiyacınız varsa, **aşağıda listelenen tüm ayrıntıları** bir destek mühendisiyle paylaşmanız çok önemlidir. Bunu **, bir ekran görüntüsü alarak** veya **kopyalama hatası simgesine tıklayarak kopyalama hatası** metin **Copy error icon**kutusunun sağında bulabilirsiniz.

## <a name="notification-details-explained"></a>Bildirim ayrıntıları açıklanmaktadır

Bildirimler hakkında daha fazla ayrıntı için aşağıdaki açıklamalara bakın.

### <a name="essential-notification-items"></a>Önemli bildirim öğeleri

- **Title** : bildirimin açıklayıcı başlığı

  * Örnek – **uygulama proxy 'si ayarları**

- **Açıklama** : işlemin sonucu olarak ne gerçekleştiğini açıklama

  -   Örnek – **girilen iç URL zaten başka bir uygulama tarafından kullanılıyor**

- **BILDIRIM kimliği** : BILDIRIMIN benzersiz kimliği

  -   Örnek – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

- **Istemci Istek kimliği** : tarayıcınız tarafından yapılan belırlı istek kimliği

  -   Örnek – **302fd775-3329-4670-a9f3-kir, 37004f0bc**

- **Zaman DAMGASı UTC** – bildirimin gerçekleştiği zaman DAMGASı (UTC)

  -   Örnek – **2017-03-23T19:50:43.7583681 z**

- **Iç Işlem kimliği** : sistemlerimizde hatayı aramak için KULLANABILMEMIZ iç kimlik

  -   Örnek – **71a2f329-ca29-402f-AA72-bc00a7aca603**

- **UPN** : işlemi gerçekleştiren Kullanıcı

  -   Örnek – **tperkins \@ f128.info**

- **KIRACı kimliği** : işlemi gerçekleştiren kullanıcının bir üyesi olduğu kiracının benzersiz kimliği

  -   Örnek – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

- **Kullanıcı nesne kimliği** – işlemi gerçekleştiren kullanıcının benzersiz kimliği

  -   Örnek – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Ayrıntılı bildirim öğeleri

-   **Görünen ad** – **(boş olabilir)** hata için daha ayrıntılı bir görünen ad

    -   Örnek – **uygulama proxy 'si ayarları**

-   **Durum** : bildirimin belirli durumu

    -   Örnek: **başarısız**

-   **Nesne kimliği** – **(boş olabilir)** işlemin gerçekleştirildiği nesne kimliği

    -   Örnek: **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Ayrıntılar** : işlemin sonucu olarak ne gerçekleştiğini ayrıntılı açıklama

    -   Örnek – **İç URL `https://bing.com/` zaten kullanımda olduğundan geçersizdir**

-   **Kopyalama hatası** – tüm bildirim ayrıntılarını bir destek veya ürün grubuyla paylaşmak üzere kopyalamak için **kopyalama hatası** metin kutusunun sağ tarafındaki **Kopyala simgesine** tıklayın 
-   engineer (mühendis)

    -   Örneğinde```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

