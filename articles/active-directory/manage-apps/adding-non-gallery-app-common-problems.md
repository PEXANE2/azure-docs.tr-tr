---
title: Galeri dışı uygulama ekleme sorunu | Microsoft Dokümanlar
description: İnsanların özel galeri dışı uygulamalar eklerken karşılaştıkları sık karşılaşılan sorunları anlama
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
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 38a9ef04389318d3588649117c930ff6efa3fe4e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65784489"
---
# <a name="problem-adding-a-non-gallery-application"></a>Galeri dışı uygulama ekleme sorunu

Bu makale, özel **galeri dışı uygulamalar** eklerken insanların karşılaştığı sık karşılaşılan sorunları ve bunları çözmek için neler yapabileceğinizi anlamanıza yardımcı olur. 

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a>"Ekle" düğmesini tıklattım ve uygulamamın görünmesi uzun zaman aldı

Bazı durumlarda, bir uygulamanın dizininize ekledikten sonra görünmesi 1-2 dakika (ve bazen daha uzun sürebilir). Bu beklenen normal performans olmasa da, [Azure portalının](https://portal.azure.com/) sağ üst kısmındaki **Bildirimler** simgesine (çan) tıklayarak ve **Uygulama Oluştur**etiketli Devam **Veya** **Tamamlanmış** bir bildirim arayarak uygulama eklemesinin devam ettiğini görebilirsiniz.

Uygulamanız hiçbir zaman eklenmezveya **Ekle** düğmesini tıklattığınızda bir hatayla karşılaşırsanız, **hata** durumunda bir **Bildirim** görürsünüz. Bir destek mühendisiyle daha fazla bilgi edinmek veya paylaşmak için hata hakkında daha fazla ayrıntı istiyorsanız, portal bildirimi bölümünün [ayrıntılarını nasıl görebilirsiniz](#how-to-see-the-details-of-a-portal-notification) bölümündeki adımları izleyerek hata hakkında daha fazla bilgi görebilirsiniz.

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a>"Ekle" düğmesini tıklattım ve uygulamam görüntülenmedi

Bazen, geçici sorunlar, ağ sorunları veya bir hata nedeniyle, bir uygulama ekleme başarısız olur. Azure portalının sağ üst kısmındaki **Bildirimler** simgesine (çan) tıkladığınızda ve **uygulama oluştur** bildiriminizin yanında kırmızı (!) bir simge gördüğünüzde bunun gerçekleştiğini söyleyebilirsiniz. Bu, uygulamayı oluştururken bir hata olduğunu gösterir.

**Ekle** düğmesini tıklattığınızda bir hatayla karşılaşırsanız, **Hata** durumunda bir **Bildirim** görürsünüz. Bir destek mühendisiyle daha fazla bilgi edinmek veya paylaşmak için hata hakkında daha fazla ayrıntı istiyorsanız, portal bildirimi bölümünün [ayrıntılarını nasıl görebilirsiniz](#how-to-see-the-details-of-a-portal-notification) bölümündeki adımları izleyerek hata hakkında daha fazla bilgi görebilirsiniz.

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a>Uygulamayı ekledikten sonra nasıl ayarlıyorum bilmiyorum

Özel uygulamalar hakkında bilgi edinmek için yardıma ihtiyacınız varsa, [Azure AD Uygulamaları Belge Kitaplığı](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) Azure AD ile tek oturum açma ve nasıl çalıştığı hakkında daha fazla bilgi edinmenize yardımcı olur.

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Portal bildiriminin ayrıntılarını görme

Aşağıdaki adımları izleyerek herhangi bir portal bildiriminin ayrıntılarını görebilirsiniz:

1. Azure portalının sağ üst kısmındaki **Bildirimler** simgesine (çan) tıklayın

2. **Hata** durumundaki herhangi bir bildirimi (yanlarında kırmızı (!) olanlar) seçin.

   >[!NOTE]
   >**Bildirimleri Başarılı** veya Devam **Halinde** tıklatamazsınız.
   >
   >

4. Sorunla ilgili daha fazla ayrıntıyı anlamak için **Bildirim Ayrıntıları** altındaki bilgileri kullanın.

5. Hala yardıma ihtiyacınız varsa, sorununuzla ilgili yardım almak için bu bilgileri bir destek mühendisi veya ürün grubuyla da paylaşabilirsiniz.

6. Destek veya ürün grubu mühendisiyle paylaşmak üzere tüm bildirim ayrıntılarını kopyalamak için **Kopyala hatası** textbox'ın sağındaki **kopya simgesini** tıklatın.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Bir destek mühendisine bildirim ayrıntıları göndererek nasıl yardım alabilirsiniz?

Yardıma ihtiyacınız varsa **aşağıda listelenen tüm ayrıntıları** bir destek mühendisiile paylaşmanız çok önemlidir, böylece bunlar size hızlı bir şekilde yardımcı olabilir. Bunu ekran görüntüsü **alarak** veya **Kopyala hata** textbox'ının sağında bulunan **Kopyala hata simgesine**tıklayarak kolayca yapabilirsiniz.

## <a name="notification-details-explained"></a>Bildirim Detayları Açıklandı

Bildirimler hakkında daha fazla bilgi için aşağıdaki açıklamalara bakın.

### <a name="essential-notification-items"></a>Temel Bildirim Öğeleri

- **Başlık** – bildirimin açıklayıcı başlığı
  *  Örnek – **Uygulama proxy ayarları**

- **Açıklama** – operasyon sonucunda meydana gelen şeyin açıklaması

  *  Örnek – **Girilen dahili url zaten başka bir uygulama tarafından kullanılıyor**

- **Bildirim Kimliği** – bildirimin benzersiz kimliği

  *  Örnek – **istemciBildirim-2adbfc06-2073-4678-a69f-7eb78d96b068**

- **İstemci İstek Kimliği** – tarayıcınız tarafından yapılan özel istek kimliği

  *  Örnek – **302fd775-3329-4670-a9f3-bea37004f0bc**

- **Zaman Damgası UTC** – bildirimin gerçekleştiği zaman damgası, UTC'de

  *  Örnek – **2017-03-23T19:50:43.7583681Z**

- **Dahili İşlem Kimliği** – sistemlerimizdeki hatayı aramak için kullanabileceğimiz dahili kimlik

  *  Örnek – **71a2f329-ca29-402f-aa72-bc00a7aca603**

- **UPN** – işlemi gerçekleştiren kullanıcı

  *  Örnek – **tperkins\@f128.info**

- **Kiracı Kimliği** – işlemi gerçekleştiren kullanıcının, kiracının üyesi olduğu benzersiz kimliği

  *  Örnek – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

- **Kullanıcı nesnekimliği** – işlemi gerçekleştiren kullanıcının benzersiz kimliği

  *  Örnek – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Ayrıntılı Bildirim Öğeleri

- **Görüntü Adı** – **(boş olabilir)** hata için daha ayrıntılı bir ekran adı

  *  Örnek – **Uygulama proxy ayarları**

- **Durum** – bildirimin özel durumu

  *  Örnek – **Başarısız**

- **Nesne Kimliği** - **(boş olabilir)** işlemin gerçekleştirildiği nesne kimliği

  *  Örnek – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

- **Ayrıntılar** – operasyon sonucunda meydana gelen şeyin ayrıntılı açıklaması

  *  Örnek – **İç url `https://bing.com/` zaten kullanımda olduğu için geçersizdir**

- **Kopyalama hatası** – Destek veya ürün grubuyla paylaşmak için tüm bildirim ayrıntılarını kopyalamak için **Kopyala hatası** textbox'ın sağındaki **kopya simgesine** tıklayın 
- engineer (mühendis)

  *  Örnek```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```




