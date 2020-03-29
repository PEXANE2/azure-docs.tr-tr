---
title: Yönetici onayı iş akışını yapılandırma - Azure Etkin Dizin | Microsoft Dokümanlar
description: Son kullanıcıların yönetici onayı gerektiren uygulamalara erişim istemeleri için nasıl bir yol yapılandırıştırmayı öğrenin.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: mimart
ms.reviewer: luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 83b3f0d97daf0b4ac17f74981119b380d1776d97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75430210"
---
# <a name="configure-the-admin-consent-workflow-preview"></a>Yönetici onayı iş akışını yapılandırma (önizleme)

Bu makalede, son kullanıcılara yönetici onayı gerektiren uygulamalara erişim isteme leri için bir yol sağlayan yönetici onayı iş akışı (önizleme) özelliğinin nasıl etkinleştirilen açıklanmaktadır.

Yönetici onayı iş akışı olmadan, kullanıcı nın rızasının engellendiği kiracıdaki bir kullanıcı, kuruluş verilerine erişmek için izin gerektiren herhangi bir uygulamaya erişmeye çalıştıklarında engellenir. Kullanıcı, uygulamaya yetkisiz olduklarını belirten genel bir hata iletisi görür ve yöneticilerinden yardım ister. Ancak genellikle, kullanıcı kiminle iletişim kuracağını bilmez, bu nedenle uygulamada ya vazgeçer ya da yeni bir yerel hesap oluşturur. Bir yönetici bilgilendirilse bile, yöneticinin erişim izni vermesi ne yardımcı olmak ve kullanıcılarına bildirmek için her zaman kolaylaştırılmış bir işlem yoktur.
 
Yönetici onayı iş akışı, yöneticilere yönetici onayı gerektiren uygulamalara erişim sağlamak için güvenli bir yol sağlar. Bir kullanıcı bir uygulamaya erişmeye çalıştığında ancak onay veremediğinde, yönetici onayı için bir istek gönderebilir. İstek, gözden geçiren olarak atanan yöneticilere e-posta yoluyla gönderilir. Bir gözden geçiren istek üzerine işlem alır ve kullanıcı eylem bildirilir.

İstekleri onaylamak için gözden geçirenin global yönetici, bulut uygulama yöneticisi veya uygulama yöneticisi olması gerekir. Gözden geçirenin bu yönetici rollerinden birine atanmış olması gerekir; sadece bir gözden geçiren olarak atamak onların ayrıcalıklarını yükseltmek değildir.

## <a name="enable-the-admin-consent-workflow"></a>Yönetici onayı iş akışını etkinleştirme

Yönetici onayı iş akışını etkinleştirmek ve gözden geçirenleri seçmek için:

1. [Azure portalında](https://portal.azure.com) genel yönetici olarak oturum açın.
2. Sol navigasyon menüsünün üst kısmındaki **Tüm hizmetler'i** tıklatın. **Azure Etkin Dizin Uzantısı** açılır.
3. Filtre arama kutusunda "**Azure Etkin Dizini**" yazın ve **Azure Etkin Dizin öğesini** seçin.
4. Gezinti menüsünden **Kurumsal uygulamaları**tıklatın. 
5. **Yönet**altında, **Kullanıcı ayarlarını**seçin.
6. **Admin'in onay istekleri (Önizleme)** altında, ayar Kullanıcılar **Evet'e** **izin veremedikleri uygulamalar için yönetici onayı isteyebilirler.**

   ![Yönetici onayı iş akışı ayarlarını yapılandırma](media/configure-admin-consent-workflow/admin-consent-requests-settings.png)
 
6. Aşağıdaki ayarları yapılandırın:

   * **Yönetici onay isteklerini incelemek için kullanıcıları seçin.** Bu iş akışı için gözden geçirenleri, genel yönetici, bulut uygulama yöneticisi ve uygulama yöneticisi rollerine sahip bir kullanıcı kümesinden seçin.
   * **Seçili kullanıcılar istekler için e-posta bildirimleri alır.** İstek yapıldığında gözden geçirenlere e-posta bildirimlerini etkinleştirin veya devre dışı edin.  
   * **Seçili kullanıcılar istek sona erme anımsatıcıları alırsınız.** Bir istek sona ermek üzereyken gözden geçirenlere hatırlatma e-posta bildirimlerini etkinleştirin veya devre dışı kolun.  
   * **İzin isteği (gün) sonra sona erer.** İstekleri ne kadar süreyle geçerli kalacağını belirtin.

7. **Kaydet'i**seçin. Özelliğin etkinleştirilmesi bir saat kadar sürebilir.

> [!NOTE]
> **Yönetici onay isteklerini gözden geçirenler** listesini değiştirerek bu iş akışı için gözden geçirenler ekleyebilir veya kaldırabilirsiniz. Bu özelliğin geçerli bir sınırlaması, gözden geçirenlerin gözden geçiren olarak atandıkları sırada yapılan istekleri gözden geçirme yeteneğini koruyabilmesidir.

## <a name="how-users-request-admin-consent"></a>Kullanıcılar yönetici onayı nasıl talep

Yönetici onayı iş akışı etkinleştirildikten sonra, kullanıcılar yetkisiz oldukları bir uygulama için yönetici onayı isteyebilir. Aşağıdaki adımlar, onay alırken kullanıcının deneyimini açıklar. 

1. Kullanıcı uygulamada oturum açmaya çalışır.

2. **Onay gerekli** ileti görüntülenir. Kullanıcı uygulamaya erişim için bir gerekçe yazdı ve ardından **İstek onayı'nı**seçer.

   ![Yönetici onayı kullanıcı isteği ve gerekçesi](media/configure-admin-consent-workflow/end-user-justification.png)

3. Gönderilen bir **İstek** iletisi, isteğin yöneticiye gönderildiğini onaylar. Kullanıcı birkaç istek gönderirse, yöneticiye yalnızca ilk istek gönderilir.

   ![Yönetici onayı kullanıcı isteği ve gerekçesi](media/configure-admin-consent-workflow/end-user-sent-request.png)

 4. Kullanıcı, isteği onaylandığında, reddedildiğinde veya engellendiğinde bir e-posta bildirimi alır. 

## <a name="review-and-take-action-on-admin-consent-requests"></a>Yönetici onay isteklerini gözden geçirin ve eyleme geçin

Yönetici nin onay isteklerini incelemek ve harekete geçmek için:

1. Yönetici onayı iş akışının kayıtlı gözden geçirenlerinden biri olarak [Azure portalında](https://portal.azure.com) oturum açın.
2. Sol navigasyon menüsünün üst kısmındaki **Tüm hizmetleri** seçin. **Azure Etkin Dizin Uzantısı** açılır.
3. Filtre arama kutusunda "**Azure Etkin Dizini**" yazın ve **Azure Etkin Dizin** öğesini seçin.
4. Gezinti menüsünden **Kurumsal uygulamaları**tıklatın.
5. **Etkinlik**altında, **Yönetici onay isteklerini seçin (Önizleme)**.

   > [!NOTE]
   > Gözden geçirenler yalnızca gözden geçiren olarak atandıktan sonra oluşturulan yönetici isteklerini görür.

1. İstenilen uygulamayı seçin.
2. İstek le ilgili ayrıntıları gözden geçirin:  

   * Kimlerin erişim istediğini ve neden istediğini görmek için **sekme tarafından isteneni** seçin.
   * Uygulama tarafından hangi izinlerin istendiğini görmek için **Gözden Geçir izinlerini ve onayını**seçin.

8. İsteği değerlendirin ve uygun eylemi gerçekleştirin:

   * **İsteği onaylayın.** Bir isteği onaylamak için, yöneticinin başvuruya onay vermesini isteyin. Bir istek onaylandıktan sonra, tüm istekte bulununca, tüm istekte bulununca erişim izni verildiği bildirilir.  
   * **İsteği reddedin.** Bir isteği reddetmek için, tüm istekte bulundurabilecek bir gerekçe sağlamanız gerekir. Bir istek reddedildikten sonra, tüm istekte bulunduranlara uygulamaya erişimlerinin engellendiğini bildirilir. Bir isteği reddetmek, kullanıcıların gelecekte uygulama için yönetici onayı istemesini engellemez.  
   * **İsteği engelleyin.** Bir isteği engellemek için, tüm istekte bulundurabilecek bir yama sağlamanız gerekir. Bir istek engellendikten sonra, tüm istekte bulunduranlara uygulamaya erişimlerinin engellendiğini bildirilir. İsteğin engellenmesi, devre dışı bırakılmış durumda kiracınızdaki uygulama için bir hizmet ana nesnesi oluşturur. Kullanıcılar gelecekte uygulama için yönetici onayı isteyemez.
 
## <a name="email-notifications"></a>E-posta bildirimleri
 
Yapılandırılırsa, tüm gözden geçirenler şu anda e-posta bildirimleri alır:

* Yeni bir istek oluşturuldu
* Bir istek süresi doldu
* İstek son kullanma tarihine yaklaşıyor  
 
İstek edenler e-posta bildirimleri alır:

* Erişim için yeni bir istek gönderirler
* İsteklerinin süresi doldu
* İstekleri reddedildi veya engellendi
* İstekleri onaylandı.
 
## <a name="audit-logs"></a>Denetim günlükleri 
 
Aşağıdaki tabloda, yönetici onayı iş akışı için kullanılabilir senaryolar ve denetim değerleri özetlenmektedir. 

> [!NOTE]
> Denetim aktörünün kullanıcı bağlamı şu anda tüm senaryolarda eksik. Bu, önizleme sürümünde bilinen bir sınırlamadır.


|Senaryo  |Denetim Hizmeti  |Denetim Kategorisi  |Denetim Faaliyeti  |Denetim Aktörü  |Denetim günlüğü sınırlamaları  |
|---------|---------|---------|---------|---------|---------|
|Onay isteği iş akışını etkinleştiren yönetici        |Erişim Gözden Geçirmeleri           |Kullanıcı Yönetimi           |Yönetim ilkesi şablonu oluşturma          |Uygulama bağlamı            |Şu anda kullanıcı bağlamını bulamıyorsunuz            |
|Yönetici onay isteği iş akışını devre dışı bırakma       |Erişim Gözden Geçirmeleri           |Kullanıcı Yönetimi           |Yönetim ilkesi şablonu silme          |Uygulama bağlamı            |Şu anda kullanıcı bağlamını bulamıyorsunuz           |
|Yönetici onay iş akışı yapılandırmalarını güncelliyor        |Erişim Gözden Geçirmeleri           |Kullanıcı Yönetimi           |Yönetim ilkesi şablonu güncelleştirme          |Uygulama bağlamı            |Şu anda kullanıcı bağlamını bulamıyorsunuz           |
|Bir uygulama için yönetici onayı isteği oluşturan son kullanıcı       |Erişim Gözden Geçirmeleri           |İlke         |İstek oluşturma           |Uygulama bağlamı            |Şu anda kullanıcı bağlamını bulamıyorsunuz           |
|Yönetici onayı isteğini onaylayan gözden geçirenler       |Erişim Gözden Geçirmeleri           |Kullanıcı Yönetimi           |İş akışındaki tüm istekleri onaylama          |Uygulama bağlamı            |Şu anda, yönetici onayı verilen kullanıcı bağlamını veya uygulama kimliğini bulamazsınız.           |
|Yönetici onayı isteğini reddeden gözden geçirenler       |Erişim Gözden Geçirmeleri           |Kullanıcı Yönetimi           |İş akışındaki tüm istekleri onaylama          |Uygulama bağlamı            | Şu anda bir yönetici onayı isteğini reddeden aktörün kullanıcı bağlamını bulamazsınız          |

## <a name="faq"></a>SSS 

**Bu iş akışını açtım, ancak işlevselliği sınarken, erişim isteğinde bulunmama izin veren yeni "Onay gerekli" istemlerini neden göremiyorum?**

Özelliği açtıktan sonra, son kullanıcıların güncelleştirmeyi görmesi 60 dakika kadar sürebilir. `https://graph.microsoft.com/beta/settings` API'deki **EnableAdminConsentRequests** değerini görüntüleyerek yapılandırmanın düzgün bir şekilde etkili olduğunu doğrulayabilirsiniz.

**Bir gözden geçiren olarak, neden bekleyen tüm istekleri göremiyorum?**

Gözden geçirenler, yalnızca gözden geçiren olarak atandıktan sonra oluşturulan yönetici isteklerini görebilir. Bu nedenle, yakın zamanda gözden geçiren olarak eklendiyseniz, atamanızdan önce oluşturulan istekleri görmezsiniz.

**Bir gözden geçiren olarak, neden aynı uygulama için birden çok istek görüyorum?**
  
Bir uygulama geliştiricisi, son kullanıcının verilerine erişim istemek için uygulamalarını statik ve dinamik onay kullanacak şekilde yapılandırırsa, iki yönetici onay isteği görürsünüz. Bir istek statik izinleri, diğeri dinamik izinleri temsil eder.

**İstekçi olarak, isteğimin durumunu kontrol edebilir miyim?**  

Hayır, şimdilik istekte bulunabilecek ler yalnızca e-posta bildirimleri yoluyla güncellemeler alabilirsiniz.

**Bir gözden geçiren olarak, uygulamayı onaylamak mümkün mü, ancak herkes için değil mi?**
 
Yönetici onayı verme ve kiracıdaki tüm kullanıcıların uygulamayı kullanmasına izin verme konusunda endişeleriniz varsa, isteği reddetmenizi öneririz. Ardından, kullanıcı ataması gerektirerek ve kullanıcı veya grupları uygulamaya atayarak uygulamaya erişimi kısıtlayarak yönetici onayı verin. Daha fazla bilgi [için, kullanıcı ve grup atama yöntemlerine](methods-for-assigning-users-and-groups.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalara onay verme hakkında daha fazla bilgi için [Azure Active Directory onay çerçevesine](../develop/consent-framework.md)bakın.

[Son kullanıcıların uygulamalara nasıl onay verme sini yapılandırın](configure-user-consent.md)

[Kiracı genelinde yönetici onayı verme](grant-admin-consent.md)

[Microsoft kimlik platformunda izinler ve onay](../develop/active-directory-v2-scopes.md)

[StackOverflow'da Azure REKLAM](https://stackoverflow.com/questions/tagged/azure-active-directory)
