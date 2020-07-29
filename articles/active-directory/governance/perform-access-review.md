---
title: Erişim incelemeleriyle & uygulamalara erişimi gözden geçirme-Azure AD
description: Azure Active Directory erişim incelemeleriyle grup üyelerinin veya uygulama erişiminin erişimini incelemeyi öğrenin.
services: active-directory
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 06/17/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b672cca97e70c97a5dccb1ca54daccd6c171932
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85077979"
---
# <a name="review-access-to-groups-and-applications-in-azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmeleriyle gruplara ve uygulamalara erişimi gözden geçirme

Azure Active Directory (Azure AD), kuruluşların Azure AD 'de bulunan gruplara ve uygulamalara ve Azure AD erişim gözden geçirmeleri adlı bir özellik ile diğer Microsoft çevrimiçi hizmetlerine erişimi nasıl yöneteceğini basitleştirir. Bu makale, belirlenen bir gözden geçirenin bir grubun üyeleri veya bir uygulamaya erişimi olan kullanıcılar için bir erişim incelemesi gerçekleştirdiğine göre değişir. Gözden geçirmek istiyorsanız, [Azure AD Yetkilendirme Yönetimi 'nde bir erişim paketine](entitlement-management-access-reviews-review-access.md) yönelik bir pakete erişimi oku konusunu okuyun

## <a name="perform-access-review-using-my-apps"></a>Uygulamalarımı kullanarak erişim incelemesi gerçekleştirme

Erişim gözden geçirme işlemini bildirim e-postalarından başlatabilir veya doğrudan siteye giderek gönderebilirsiniz.

- **E-posta**:

>[!IMPORTANT]
> E-posta alınırken gecikmeler olabilir ve bu durum 24 saate kadar sürebilir. azure-noreply@microsoft.comTüm e-postaları aldığınızdan emin olmak için beyaz liste.

1. Microsoft 'un, erişimi incelemenizi isteyen bir e-posta bulun. Bir grubun erişimini gözden geçirmek için örnek bir e-posta aşağıda verilmiştir.

    ![Bir gruba erişimi gözden geçirmek için Microsoft 'tan örnek e-posta](./media/perform-access-review/access-review-email.png)

1. Erişim gözden geçirmesini açmak için **Incelemeye başla** bağlantısına tıklayın.

- **E-postanız**yoksa, bekleyen erişim incelemelerinizi aşağıdaki adımları izleyerek bulabilirsiniz.

    1. Konumundaki uygulamalarım portalında oturum açın [https://myapps.microsoft.com](https://myapps.microsoft.com) .

        ![İzinleriniz olan uygulamaları listeleme uygulamalarım portalı](./media/perform-access-review/myapps-access-panel.png)

    1. Sayfanın sağ üst köşesinde adınızın ve varsayılan kuruluşunuzun yanındaki kullanıcıya tıklayın. Listede birden fazla kuruluş varsa erişim gözden geçirmesi isteğinde bulunan kuruluşu seçin.

    1. Bekleyen erişim incelemelerinin listesini görmek için **erişim İncelemeleri** kutucuğuna tıklayın.

        > [!NOTE]
        > **Erişim gözden geçirmeleri** kutucuğunda görünmüyorsa, bu kuruluş için gerçekleştirilecek bir erişim incelemesi yoktur ve şu anda hiçbir işlem yapmanız gerekmez.

        ![Uygulamalar ve gruplar için bekleyen erişim İncelemeleri listesi](./media/perform-access-review/access-reviews-list.png)

    1. Gerçekleştirmek istediğiniz erişim incelemesi için **gözden geçirmeyi Başlat** bağlantısına tıklayın.

Erişim gözden geçirmesini açtıktan sonra, erişiminin gözden geçirilmesi gereken kullanıcıların adlarını görürsünüz.

İstek kendi erişiminizi gözden geçirmek ise, sayfa farklı görünür. Daha fazla bilgi için bkz. [gruplar veya uygulamalar için kendinize erişimi gözden geçirme](review-your-access.md).

![Gözden geçirilecek kullanıcıları listelemek için açık erişim gözden geçirme](./media/perform-access-review/perform-access-review.png)

Erişimi onaylamak veya reddetmek için kullanabileceğiniz iki yol vardır:

- Her Kullanıcı isteği için uygun eylemi seçerek bir veya daha fazla kullanıcının ' el ile ' erişimini onaylayabilir veya reddedebilirsiniz.
- Sistem önerilerini kabul edebilirsiniz.

### <a name="approve-or-deny-access-for-one-or-more-users"></a>Bir veya daha fazla kullanıcı için erişimi onaylama veya reddetme

1. Kullanıcı listesini gözden geçirin ve devam eden erişimleri onaylama veya reddetme seçeneklerinden birini belirleyin.

    - Tek bir kullanıcıya erişimi onaylamak veya reddetmek için, yapılacak eylemi belirtmek üzere bir pencere açmak için satıra tıklayın. 
    - Birden çok kullanıcının erişimini onaylamak veya reddetmek için, kullanıcıların yanındaki onay işaretlerini ekleyin ve sonra gerçekleştirilecek eylemi belirtmek üzere bir pencere açmak için **X kullanıcıları gözden geçir** düğmesine tıklayın.

1. **Onayla** veya **Reddet**' e tıklayın. 

    ![Onaylama, reddetme ve bilmiyorum seçeneklerini içeren eylem penceresi](./media/perform-access-review/approve-deny.png)
    >[!NOTE]
    > Emin değilseniz, **bilinmiyor**' a tıklayabilirsiniz. ve Kullanıcı, erişiminin tutulmasını alır ve seçiminiz denetim günlüklerine kaydedilir.

1. Erişim gözden geçirmesi Yöneticisi, kararınız için **neden** kutusunda bir neden belirtmeniz gerekebilir. Bir neden gerekli olmasa bile. Kararınız için bir neden sağlayabilirsiniz ve eklediğiniz bilgiler diğer gözden geçirenler tarafından kullanılabilir olacaktır.

1. Gerçekleştirilecek eylemi belirledikten sonra **Kaydet**' e tıklayın.

    >[!NOTE]
    > Erişim gözden geçirmesi sona ermeden önce yanıtınızı istediğiniz zaman değiştirebilirsiniz. Yanıtınızı değiştirmek istiyorsanız, satırı seçin ve yanıtı güncelleştirin. Örneğin, daha önce reddedilen bir kullanıcıyı onaylayabilir veya daha önce onaylanmış bir kullanıcıyı reddedebilirsiniz.

    >[!IMPORTANT]
    > - Bir kullanıcının erişimi reddedildiyse, bu kişiler hemen kaldırılmaz. Bunlar, gözden geçirme süresi sona erdiğinde veya [Otomatik Uygula](complete-access-review.md#apply-the-changes) etkinse bir yönetici incelemeyi durdurduklarında kaldırılır.
    > - Birden çok gözden geçiren varsa, son gönderilen yanıt kaydedilir. Yöneticinin iki gözden geçiren – Gamze ve Bob 'u aldığı bir örnek düşünün. Çiğdem önce erişim gözden geçirmeyi açar ve kullanıcının erişim isteğini onaylar. İnceleme dönemi bitmeden önce, Bob erişim gözden geçirmesini açar ve daha önce gamze tarafından onaylanan istek için erişimi reddeder. Erişimi reddeten son karar, kaydedilen yanıttır.

### <a name="approve-or-deny-access-based-on-recommendations"></a>Önerilere göre erişimi onaylayın veya reddedin

Erişim incelemelerini sizin için daha kolay ve hızlı hale getirmek için, tek bir tıklama ile kabul etmenizi sağlayacak öneriler de sağlarız. Öneriler, kullanıcının oturum açma etkinliğine göre oluşturulur.

1. Sayfanın altındaki mavi çubukta **öneriyi kabul et**' e tıklayın.

    ![Önerileri kabul et düğmesini gösteren erişim gözden geçirme listesini aç](./media/perform-access-review/accept-recommendations.png)

    Önerilen eylemlerin bir özetini görürsünüz.

    ![Önerilen eylemlerin özetini görüntüleyen pencere](./media/perform-access-review/accept-recommendations-summary.png)

1. Önerileri kabul etmek için **Tamam** ' ı tıklatın.

## <a name="perform-access-review-using-my-access-new"></a>Erişimimi kullanarak erişim incelemesi gerçekleştir (yeni)

Erişimimde güncelleştirilmiş Kullanıcı arabirimiyle ilgili yeni gözden geçiren deneyime birkaç farklı yolla ulaşabilirsiniz:

### <a name="my-apps-portal"></a>Uygulamalarım portalı

1. Konumundaki uygulamamda oturum açın [https://myapps.microsoft.com](https://myapps.microsoft.com) .

    ![İzinleriniz olan uygulamaları listeleme uygulamalarım portalı](./media/perform-access-review/myapps-access-panel.png)

2. Bekleyen erişim incelemelerinin listesini görmek için **erişim İncelemeleri** kutucuğuna tıklayın.

    > [!NOTE]
    > **Erişim gözden geçirmeleri** kutucuğunda görünmüyorsa, bu kuruluş için gerçekleştirilecek bir erişim incelemesi yoktur ve şu anda hiçbir işlem yapmanız gerekmez.

![Önizleme sırasında yeni deneyim kullanılabilir başlığı bulunan uygulamalar ve gruplar için bekleyen erişim İncelemeleri listesi](./media/perform-access-review/banner.png)

3. Dene 'ye tıklayın **!** sayfanın üst kısmındaki başlıkta. Bu, sizi yeni erişim deneyimime götürür.
  
### <a name="email"></a>E-posta

  >[!IMPORTANT]
> E-posta alınırken gecikmeler olabilir ve bu durum 24 saate kadar sürebilir. azure-noreply@microsoft.comTüm e-postaları aldığınızdan emin olmak için beyaz liste.

   1. Microsoft 'un, erişimi incelemenizi isteyen bir e-posta bulun. Aşağıda örnek bir e-posta iletisi görebilirsiniz:

   ![Bir gruba erişimi gözden geçirmek için Microsoft 'tan örnek e-posta](./media/perform-access-review/access-review-email-preview.png)

   2. Erişim gözden geçirmesini açmak için **Incelemeye başla** bağlantısına tıklayın.

>[!NOTE]
>Başlat İnceleme ' ye tıkladığınızda **uygulamalarım** , uygulamalar **portalından**yukarıdaki bölümde listelenen adımları izleyin.

### <a name="navigate-to-my-access-directly"></a>Erişimime doğrudan git

Ayrıca, erişiminizi açmak için tarayıcınızı kullanarak bekleyen erişim incelemelerinizi görüntüleyebilirsiniz.

1. Erişimimde şurada oturum açın:https://myaccess.microsoft.com/

2. Size atanan bekleyen erişim incelemelerinin listesini görmek için sol taraftaki menüden **erişim gözden geçirmeleri** ' nı seçin.

   ![menüdeki erişim İncelemeleri](./media/perform-access-review/access-review-menu.png)

### <a name="approve-or-deny-access-for-one-or-more-users"></a>Bir veya daha fazla kullanıcı için erişimi onaylama veya reddetme

Erişim grupları ve uygulamalar altında erişimimi açtıktan sonra şunları görebilirsiniz:

- **Ad** Erişim gözden geçirmesi adı.
- **Son tarih** İnceleme için son tarih. Bu tarihten sonra kullanıcılar gözden geçirilmekte olan gruptan veya uygulamadan kaldırılabilir.
- **Kaynak** Gözden geçirme kapsamındaki kaynağın adı.
- **Ilerleme durumu** Bu erişim incelemesinin Toplam Kullanıcı sayısı üzerinde incelenen Kullanıcı sayısı.

Başlamak için erişim gözden geçirmesi adına tıklayın.

![Uygulamalar ve gruplar için bekleyen erişim İncelemeleri listesi](./media/perform-access-review/access-reviews-list-preview.png)

Açıldıktan sonra erişim gözden geçirmesi için kapsamdaki kullanıcıların listesini görürsünüz. İstek kendi erişiminizi gözden geçirmek ise, sayfa farklı görünür. Daha fazla bilgi için bkz. [gruplar veya uygulamalar için kendinize erişimi gözden geçirme](review-your-access.md).

Erişimi onaylamak veya reddetmek için kullanabileceğiniz iki yol vardır:

- Bir veya daha fazla kullanıcının erişimini el ile onaylayabilir veya reddedebilirsiniz.
- Sistem önerilerini kabul edebilirsiniz.

#### <a name="manually-approve-or-deny-access-for-one-or-more-users"></a>Bir veya daha fazla kullanıcının erişimini el ile onaylama veya reddetme

1. Kullanıcı listesini gözden geçirin ve devam eden erişimleri onaylama veya reddetme seçeneklerinden birini belirleyin.
2. Adlarının yanındaki daireye tıklayarak bir veya daha fazla kullanıcı seçin.
3. Yukarıdaki çubukta **Onayla** veya **Reddet** ' i seçin.
    - Emin değilseniz, **bilinmiyor**' a tıklayabilirsiniz. Kullanıcı, erişiminin tutulmasını alır ve seçiminiz denetim günlüklerine kaydedilir. Sağladığınız tüm bilgilerin diğer gözden geçirenler tarafından kullanılabilir olacağını aklınızda bulundurmanız önemlidir. Açıklamaları okuyabilir ve isteği gözden geçirdiklerinde onları hesaba götürebilirler.

    ![Gözden geçirilmesi gereken kullanıcıları listelemek için açık erişim gözden geçirme](./media/perform-access-review/user-list-preview.png)

4. Erişim gözden geçirmesi Yöneticisi, kararınız için **neden** kutusunda bir neden belirtmeniz gerekebilir. Bir neden gerekli olmasa bile. Kararınız için bir neden de sağlayabilirsiniz ve dahil ettiğiniz bilgiler, gözden geçirilmek üzere diğer onaylayanlar tarafından kullanılabilir olacaktır.

5. **Gönder**' e tıklayın.
    - Erişim incelemesi sonlanana kadar yanıtınızı dilediğiniz zaman değiştirebilirsiniz. Yanıtınızı değiştirmek istiyorsanız, satırı seçin ve yanıtı güncelleştirin. Örneğin, daha önce reddedilen bir kullanıcıyı onaylayabilir veya daha önce onaylanmış bir kullanıcıyı reddedebilirsiniz.

 >[!IMPORTANT]
 > - Bir kullanıcının erişimi reddedildiyse, bu kişiler hemen kaldırılmaz. İnceleme süresi sona erdiğinde veya bir yönetici incelemeyi durdurduklarında bunlar kaldırılır. 
 > - Birden çok gözden geçiren varsa, son gönderilen yanıt kaydedilir. Yöneticinin iki gözden geçiren – Gamze ve Bob 'u aldığı bir örnek düşünün. Çiğdem önce erişim gözden geçirmeyi açar ve kullanıcının erişim isteğini onaylar. İnceleme dönemi bitmeden önce, Bob erişim gözden geçirmesini açar ve daha önce gamze tarafından onaylanan istek için erişimi reddeder. Erişimi reddeten son karar, kaydedilen yanıttır.

#### <a name="approve-or-deny-access-based-on-recommendations"></a>Önerilere göre erişimi onaylayın veya reddedin

Erişim incelemelerini sizin için daha kolay ve hızlı hale getirmek için, tek bir tıklama ile kabul etmenizi sağlayacak öneriler de sağlarız. Öneriler, kullanıcının oturum açma etkinliğine göre oluşturulur.

1. Bir veya daha fazla kullanıcı seçin ve ardından **önerileri kabul et**' e tıklayın.

    ![Önerileri kabul et düğmesini gösteren erişim gözden geçirme listesini aç](./media/perform-access-review/accept-recommendations-preview.png)

1. Önerileri kabul etmek için **Gönder** ' e tıklayın.

Tüm kullanıcılar için önerileri kabul etmek için, hiç birinin seçili olmadığından emin olun ve üstteki çubukta **önerileri kabul et** düğmesine tıklayın.

>[!NOTE]
>Önerileri kabul ettiğinizde önceki kararlar değiştirilmez.

## <a name="next-steps"></a>Sonraki adımlar

- [Grupların veya uygulamaların erişim incelemesini tamamlar](complete-access-review.md)

