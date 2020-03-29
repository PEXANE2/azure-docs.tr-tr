---
title: B2B davetiye e-postasının öğeleri - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory B2B işbirliği daveti e-posta şablonu
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f93586d46aa01116990f8f02f344c6952d3c1b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65768362"
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email---azure-active-directory"></a>B2B işbirliği daveti e-postasının öğeleri - Azure Active Directory

Davet e-postaları, Iş ortaklarını Azure AD'deki B2B işbirliği kullanıcıları olarak gemiye getirmek için önemli bir bileşendir. Bunları alıcının güvenini artırmak için kullanabilirsiniz. alıcının daveti kabul etmek için **Başlat düğmesini** seçmekte rahat hissettiğinden emin olmak için e-postaya meşruiyet ve sosyal kanıt ekleyebilirsiniz. Bu güven, paylaşım sürtünmesini azaltmak için önemli bir araçtır. Ve ayrıca e-posta harika görünmesini istiyorum!

![B2B davet e-postasını gösteren ekran görüntüsü](media/invitation-email-elements/invitation-email.png)

## <a name="explaining-the-email"></a>E-postayı açıklama
E-postanın birkaç öğesine bakalım, böylece yeteneklerini en iyi nasıl kullanacağınızı biliyorsunuz.

### <a name="subject"></a>Özne
E-postanın konusu aşağıdaki şablonu izler: &lt;Kiracı adı&gt; kuruluşuna davetlisiniz

### <a name="from-address"></a>Adresten
From adresi için LinkedIn benzeri bir desen kullanıyoruz.  Davetlinin kim olduğunu ve hangi şirketten geldiğini açık olmalı ve e-postanın microsoft e-posta adresinden geldiğini açıklığa kavuşturmalısınız. Biçimi: Microsoft Davetiyeler <invites@microsoft.com> &lt;veya kiracı adı&gt; &lt;&gt; (Microsoft üzerinden) <invites@microsoft.com>gelen davetlinin Ekran adı.

### <a name="reply-to"></a>Yanıtla
Yanıtlayan e-posta, hazır olduğunda davetlinin e-postasına ayarlanır, böylece e-postayı yanıtlamak davetliye bir e-posta gönderir.

### <a name="branding"></a>Markalama
Kiracınızdan gelen davet e-postaları, kiracınız için ayarlamış olabileceğiniz şirket markasını kullanır. Bu özellikten yararlanmak istiyorsanız, yapılandırma nın nasıl olduğu yla ilgili ayrıntılar [aşağıda](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) verebilirsiniz. Banner logosu e-postada görünür. En iyi sonuçlar için görüntü boyutunu ve kalite yönergelerini [buradan](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) takip edin. Buna ek olarak, şirket adı da eylem çağrısında gösterir.

### <a name="call-to-action"></a>Harekete geçme çağrısı
Eylem çağrısı iki bölümden oluşur: alıcının postayı neden aldığını ve alıcıdan bu konuda ne yapması istendiğini açıklamak.
- "Neden" bölümü aşağıdaki desen kullanılarak ele alınabilir: Kiracı adı &lt;&gt; kuruluşundaki uygulamalara erişmek için davet edildiniz

- "Sizden ne yapmanız isteniyor" bölümü Başlat **düğmesinin** varlığıyla gösterilir. Alıcı davete gerek kalmadan eklendiğinde, bu düğme görünmüyor.

### <a name="inviters-information"></a>Davetli bilgileri
Davetlinin ekran adı e-postada yer alan. Ayrıca, Azure REKLAM hesabınız için bir profil resmi ayarladıysanız, davet edilen e-postada bu resim de yer alacaktır. Her ikisi de alıcınızın e-postaya olan güvenini artırmak için tasarlanmıştır.

Profil resminizi henüz ayarlamadıysanız, resmin yerine davetlinin baş harflerini içeren bir simge gösterilir:

  ![Daveti davet eden intisat baş harfleri görüntülenirken gösteren ekran görüntüsü](media/invitation-email-elements/inviters-initials.png)

### <a name="body"></a>Gövde
Gövde, [konuk kullanıcıyı dizine, gruba veya uygulamaya davet](add-users-administrator.md) ederken veya davet [API'sini kullanarak](customize-invitation-api.md)davet edenin oluşturduğu iletiyi içerir. Bir metin alanıdır, bu nedenle güvenlik nedenleriyle HTML etiketlerini işlemez.

  ![Davet e-postasının gövdesini gösteren ekran görüntüsü](media/invitation-email-elements/invitation-email-body.png)

### <a name="footer-section"></a>Altbilgi bölümü
Altbilgi Microsoft şirket markasını içerir ve alıcıya e-postanın izlenmeyen bir diğer addan gönderilip gönderilmediği hakkında bilgi verir. 

Özel durumlar:

- Davet edenin davet edilen kirada e-posta adresi yok

  ![Davet eden bir davetsahibinin davet edilen kirada e-postası olmadığında ekran görüntüsü](media/invitation-email-elements/inviter-no-email.png)


- Alıcının daveti kullanmasına gerek yoktur

  ![Alıcının daveti kullanmasına gerek olmadığında ekran görüntüsü](media/invitation-email-elements/when-recipient-doesnt-redeem.png)

## <a name="how-the-language-is-determined"></a>Dil nasıl belirlenir?
Davet e-postasında konuk kullanıcıya sunulan dil aşağıdaki ayarlarla belirlenir. Bu ayarlar öncelik sırasına göre listelenir. Ayar yapılandırılmamışsa, listedeki bir sonraki ayar dili belirler. 
- Davet API'si kullanılırsa [davet edilenUserMessageInfo](https://docs.microsoft.com/graph/api/resources/invitedusermessageinfo?view=graph-rest-1.0) nesnesinin **iletiDili** özelliği
-   Konuğun [kullanıcı nesnesinde](https://docs.microsoft.com/graph/api/resources/user?view=graph-rest-1.0) belirtilen **tercih edilenDil** özelliği
-   Konuk kullanıcının ev kiracısının özelliklerinde ayarlanan **Bildirim dili** (yalnızca Azure AD kiracıları için)
-   Kaynak kiracının özelliklerinde ayarlanan **Bildirim dili**

Bu ayarların hiçbiri yapılandırılmamışsa, dil varsayılan olarak İngilizce (ABD) olarak tanımlanır.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir](what-is-b2b.md)
- [Azure Active Directory yöneticileri B2B işbirliği kullanıcılarını nasıl ekler?](add-users-administrator.md)
- [Bilgi çalışanları B2B işbirliği kullanıcılarını nasıl ekler?](add-users-information-worker.md)
- [B2B işbirliği daveti itfa](redemption-experience.md)
- [B2B işbirliği kullanıcılarını davetsiz ekleme](add-user-without-invite.md)
