---
title: Erişim incelemeleriyle & uygulamalara erişiminizi gözden geçirme-Azure AD
description: Azure Active Directory erişim gözden geçirmeleriyle gruplara veya uygulamalara kendi erişiminizi incelemeyi öğrenin.
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
ms.openlocfilehash: 33d946c47a17bb537c7644937547ad479b4637e5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85077926"
---
# <a name="review-access-for-yourself-to-groups-or-applications-in-azure-ad-access-reviews"></a>Azure AD erişim incelemeleriyle ilgili grupları veya uygulamaları kendiniz için erişimi gözden geçirin

Azure Active Directory (Azure AD), kuruluşların Azure AD erişim gözden geçirmeleri adlı bir özellik ile Azure AD ve diğer Microsoft çevrimiçi hizmetlerindeki gruplara veya uygulamalara erişimi nasıl yöneteceğini basitleştirir.

Bu makalede, bir grup veya uygulamaya kendi erişiminizi nasıl gözden geçibileceğinizi açıklar.

## <a name="review-your-access-using-my-apps"></a>Uygulamalarımı kullanarak erişiminizi gözden geçirme

Erişim gözden geçirmesi gerçekleştirmeye yönelik ilk adım, erişim incelemesini bulup açmak.

>[!IMPORTANT]
> E-posta alınırken gecikmeler olabilir ve bu durum 24 saate kadar sürebilir. azure-noreply@microsoft.comTüm e-postaları aldığınızdan emin olmak için beyaz liste.

1. Microsoft 'un, erişimi incelemenizi isteyen bir e-posta bulun. Bir gruba erişiminizi gözden geçirmek için örnek bir e-posta aşağıda verilmiştir.

    ![Bir gruba erişiminizi gözden geçirmek için Microsoft 'tan örnek e-posta](./media/review-your-access/access-review-email.png)

1. Erişim gözden geçirmesini açmak için **erişimi gözden geçir** bağlantısına tıklayın.

E-postanız yoksa, bekleyen erişim incelemelerinizi aşağıdaki adımları izleyerek bulabilirsiniz.

1. Konumundaki uygulamalarım portalında oturum açın [https://myapps.microsoft.com](https://myapps.microsoft.com) .

    ![İzinleriniz olan uygulamaları listeleme uygulamalarım portalı](./media/review-your-access/myapps-access-panel.png)

1. Sayfanın sağ üst köşesinde yer alan ve adınızla varsayılan kuruluşunuzun gösterildiği kullanıcı simgesine tıklayın. Listede birden fazla kuruluş varsa erişim gözden geçirmesi isteğinde bulunan kuruluşu seçin.

1. Sayfanın sağ tarafında, bekleyen erişim incelemelerinin listesini görmek için **erişim İncelemeleri** kutucuğuna tıklayın.

    Kutucuk yoksa ilgili kuruluş için bekleyen erişim gözden geçirmesi yoktur ve herhangi bir işlem yapmanız gerekmez.

    ![Uygulamalarınız ve gruplarınız için bekleyen erişim İncelemeleri listesi](./media/review-your-access/access-reviews-list.png)

1. Gerçekleştirmek istediğiniz erişim incelemesi için **gözden geçirmeyi Başlat** bağlantısına tıklayın.

### <a name="perform-the-access-review"></a>Erişim gözden geçirmesini gerçekleştir

Erişim gözden geçirmesini açtıktan sonra erişiminizi görebilirsiniz.

1. Erişiminizi gözden geçirin ve hâlâ erişime ihtiyacınız olup olmadığına karar verin.

    İstek başkalarının erişimini gözden geçirmektir, sayfa farklı görünür. Daha fazla bilgi için bkz. [gruplara veya uygulamalara erişimi gözden geçirme](perform-access-review.md).

    ![Hala bir gruba erişmeniz gerekip gerekmediğini soran açık erişim gözden geçirme](./media/review-your-access/perform-access-review.png)

1. Erişiminizi korumak için **Evet** ' e tıklayın veya erişiminizi kaldırmak için **Hayır** ' a tıklayın.

1. **Evet**' e tıklarsanız, **neden** kutusunda bir gerekçe belirtmeniz gerekebilir.

    ![Bir gruba erişmeniz gerekip gerekmediğini soran tamamlanmış erişim incelemesi](./media/review-your-access/perform-access-review-submit.png)

1. **Gönder**' e tıklayın.

    Seçiminiz gönderilir ve My Apps portalına dönersiniz.

    Yanıtınızı değiştirmek istiyorsanız, erişim İncelemeleri sayfasını yeniden açın ve yanıtınızı güncelleştirin. Erişim incelemesi sonlanana kadar yanıtınızı dilediğiniz zaman değiştirebilirsiniz.

    > [!NOTE]
    > Artık erişime ihtiyaç duymadığını belirttiyseniz, hemen kaldırılmamıştır. İnceleme sona erdiğinde veya bir yönetici incelemeyi durdurduğu zaman kaldırılır.

## <a name="review-your-own-access-using-my-access-new"></a>Erişimimi kullanarak kendi erişiminizi gözden geçirin (yeni)

Yeni deneyimi erişimimde güncelleştirilmiş Kullanıcı arabirimiyle birkaç farklı şekilde deneyebilirsiniz:

### <a name="my-apps-portal"></a>Uygulamalarım portalı

1. Konumundaki uygulamalarım portalında oturum açın [https://myapps.microsoft.com](https://myapps.microsoft.com) .

    ![İzinleriniz olan uygulamaları listeleme uygulamalarım portalı](./media/review-your-access/myapps-access-panel.png)

2. Bekleyen erişim incelemelerinin listesini görmek için **erişim İncelemeleri** kutucuğuna tıklayın.

    > [!NOTE]
    > **Erişim gözden geçirmeleri** kutucuğunda görünmüyorsa, bu kuruluş için gerçekleştirilecek bir erişim incelemesi yoktur ve şu anda hiçbir işlem yapmanız gerekmez.

3. Dene 'ye tıklayın **!** sayfanın üst kısmındaki başlıkta yeni erişim deneyimime gidebilirsiniz.

    ![Önizleme sırasında yeni deneyim kullanılabilir başlığı bulunan uygulamalar ve gruplar için bekleyen erişim İncelemeleri listesi](./media/review-your-access/banner-your-access.png)

4. **Erişim gözden geçirmesini gerçekleştirme** bölümünde devam et

### <a name="email"></a>E-posta

>[!IMPORTANT]
> E-posta alınırken gecikmeler olabilir ve bu durum 24 saate kadar sürebilir. azure-noreply@microsoft.comTüm e-postaları aldığınızdan emin olmak için beyaz liste.

1. Microsoft 'un, erişimi incelemenizi isteyen bir e-posta bulun. Aşağıda örnek bir e-posta iletisi görebilirsiniz:

 ![Bir gruba erişimi gözden geçirmek için Microsoft 'tan örnek e-posta](./media/review-your-access/access-review-email-preview.png)

2. Erişim gözden geçirmesini açmak için **erişimi gözden geçir** bağlantısına tıklayın.

3. **Erişim gözden geçirmesini gerçekleştirme** bölümünde devam et

>[!NOTE]
>Başlat İnceleme ' ye tıkladığınızda **uygulamalarım** , uygulamalar **portalından**yukarıdaki bölümde listelenen adımları izleyin.

### <a name="directly-at-my-access"></a>Doğrudan erişimimde

Ayrıca, erişiminizi açmak için tarayıcınızı kullanarak bekleyen erişim incelemelerinizi görüntüleyebilirsiniz.

1. Erişimimde şurada oturum açın:https://myaccess.microsoft.com/

2. Size atanan bekleyen erişim incelemelerinin listesini görmek için sol taraftaki menüden **erişim gözden geçirmeleri** ' nı seçin.

   ![menüdeki erişim İncelemeleri](./media/review-your-access/access-review-menu.png)

### <a name="perform-the-access-review"></a>Erişim gözden geçirmesini gerçekleştir

1. Gruplar ve uygulamalar altında şunları görebilirsiniz:
    
    - **Ad** Erişim gözden geçirmesi adı.
    - **Son tarih** İnceleme için son tarih. Bu tarihten sonra kullanıcılar gözden geçirilmekte olan gruptan veya uygulamadan kaldırılabilir.
    - **Kaynak** Gözden geçirme kapsamındaki kaynağın adı.
    - **Ilerleme durumu** Bu erişim incelemesinin Toplam Kullanıcı sayısı üzerinde incelenen Kullanıcı sayısı.
    
2. Başlamak için erişim gözden geçirmesi adına tıklayın.

   ![Uygulamalar ve gruplar için bekleyen erişim İncelemeleri listesi](./media/review-your-access/access-reviews-list-preview.png)

3. Erişiminizi gözden geçirin ve hâlâ erişime ihtiyacınız olup olmadığına karar verin.

    İstek başkalarının erişimini gözden geçirmektir, sayfa farklı görünür. Daha fazla bilgi için bkz. [gruplara veya uygulamalara erişimi gözden geçirme](perform-access-review.md).

    ![Hala bir gruba erişmeniz gerekip gerekmediğini soran açık erişim gözden geçirme](./media/review-your-access/review-access-preview.png)

1. Erişiminizi korumak için **Evet** ' i seçin veya erişiminizi kaldırmak için **Hayır** ' ı seçin.

1. **Evet**' e tıklarsanız, **neden** kutusunda bir gerekçe belirtmeniz gerekebilir.

    ![Bir gruba erişmeniz gerekip gerekmediğini soran tamamlanmış erişim incelemesi](./media/review-your-access/review-access-yes-preview.png)

1. **Gönder**' e tıklayın.

    Seçiminiz gönderilir ve erişim sayfasına dönersiniz.

    Yanıtınızı değiştirmek istiyorsanız, erişim İncelemeleri sayfasını yeniden açın ve yanıtınızı güncelleştirin. Erişim incelemesi sonlanana kadar yanıtınızı dilediğiniz zaman değiştirebilirsiniz.

    > [!NOTE]
    > Artık erişime ihtiyaç duymadığını belirttiyseniz, hemen kaldırılmamıştır. İnceleme sona erdiğinde veya bir yönetici incelemeyi durdurduğu zaman kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Grupların veya uygulamaların erişim incelemesini tamamlar](complete-access-review.md)
