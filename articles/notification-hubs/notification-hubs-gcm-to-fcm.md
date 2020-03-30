---
title: Azure Bildirim Hub'ları ve Google Firebase Bulut Mesajlaşma (FCM) geçişi
description: Azure Bildirim Hub'larının Google GCM'den FCM geçişine nasıl hitap ettiği açıklanır.
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 04/10/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 04/10/2019
ms.openlocfilehash: 2f2ca4b56445b3f399477e396de579d8a8c539e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127035"
---
# <a name="azure-notification-hubs-and-google-firebase-cloud-messaging-migration"></a>Azure Bildirim Hub'ları ve Google Firebase Bulut Mesajlaşma geçişi

## <a name="current-state"></a>Geçerli durum

Google, Google Bulut Mesajlaşma'dan (GCM) Firebase Cloud Messaging'e (FCM) geçişini duyurduğunda, bizimki gibi hizmetler değişikliği karşılamak için Android cihazlara bildirim gönderme şeklimizi ayarlamak zorunda kaldı.

Hizmet arka uçumuzu güncelledik, ardından gerektiğinde API ve SDK'larımızda güncellemeler yayınladık. Uygulamamızla, müşteri etkisini en aza indirmek için mevcut GCM bildirim şemalarıyla uyumluluğu koruma kararı aldık. Bu, şu anda FCM Eski Modu'nda FCM kullanarak Android cihazlara bildirim gönderdiğimiz anlamına gelir. Sonuç olarak, yeni özellikler ve taşıma biçimi de dahil olmak üzere FCM için gerçek destek eklemek istiyoruz. Bu daha uzun vadeli bir değişikliktir ve mevcut geçiş, varolan uygulamalar ve SDK'larla uyumluluğu korumaya odaklanmıştır. Uygulamanızda (SDK ile birlikte) GCM veya FCM SDK'ları kullanabilirsiniz ve bildirimin doğru bir şekilde gönderilmesini sağlayabiliriz.

Bazı müşteriler son zamanlarda Google'dan bildirimler için GCM bitiş noktası kullanan uygulamalar hakkında uyarı bir e-posta aldı. Bu sadece bir uyarıydı ve hiçbir şey kırılmadı – uygulamanızın Android bildirimleri hala işlenmek üzere Google'a gönderilir ve Google hala bunları işler. Hizmet yapılandırmalarında GCM bitiş noktasını açıkça belirten bazı müşteriler hala amortismana hazır bitiş noktasını kullanıyordu. Bu boşluğu zaten belirlemiştik ve Google e-postayı gönderdiğinde sorunu gidermeye çalışıyorduk.

O azalan bitiş noktasını değiştirdik ve düzeltme dağıtıldı.

## <a name="going-forward"></a>İleriye gitme

Google'ın FCM SSS'si hiçbir şey yapmanız gerekmemektedir. [FCM SSS,](https://developers.google.com/cloud-messaging/faq)Google "istemci SDK'lar ve GCM belirteçleri süresiz çalışmaya devam edeceğini söyledi. Ancak, FCM'ye geçiş yapmadıkça Android uygulamanızda Google Play Hizmetleri'nin en son sürümünü hedef alamazsınız."

Uygulamanız GCM kitaplığını kullanıyorsa, uygulamanızdaki FCM kitaplığına yükseltmek için Google'ın yönergelerini izleyin. SDK'mız her ikisiyle de uyumludur, bu nedenle uygulamanızda bizim tarafımızdaki hiçbir şeyi güncellemeniz gerekmez (SDK sürümümüzden haberdar olduğunuz sürece).

## <a name="questions-and-answers"></a>Sorular ve cevaplar

Müşterilerden duyduğumuz sık sorulan soruların bazı yanıtları aşağıda veda edinebilirsiniz:

**S:** Kesme tarihine göre uyumlu olmak için ne yapmam gerekiyor (Google'ın geçerli kesme tarihi 29 Mayıs'tır ve değişebilir)?

**A:** Hiçbir şey. Mevcut GCM bildirim şemasıyla uyumluluğu koruyacağız. GCM anahtarınız, uygulamanız tarafından kullanılan GCM SDK'ları ve kitaplıkları gibi normal çalışmaya devam edecektir.

Yeni özelliklerden yararlanmak için FCM SDK'larına ve kitaplıklarına yükseltmeye karar verdiyseniz/ne zaman yeni özelliklerden yararlanırsanız, GCM anahtarınız çalışmaya devam eder. İsterseniz FCM tuşunu kullanmaya geçebilirsiniz, ancak yeni Firebase projesini oluştururken mevcut GCM projenize Firebase eklediğinizden emin olabilirsiniz. Bu, uygulamanın GCM SDK'larını ve kitaplıklarını hala kullanan eski sürümlerini çalıştıran müşterilerinizle geriye dönük uyumluluğu garanti eder.

Yeni bir FCM projesi oluşturuyorsanız ve mevcut GCM projesine eklenmiyorsanız, Bildirim Hub'larını yeni FCM sırrıyla güncelledikten sonra, yeni FCM anahtarının eski GCM'ye bağlantısı olmadığı için, mevcut uygulama yüklemelerinize bildirim iletme yeteneğini kaybedersiniz Proje.

**S:** Neden eski GCM uç noktaları kullanılan hakkında bu e-posta alıyorum? Ne yapmam gerekiyor?

**A:** Hiçbir şey. Yeni uç noktalara göç ediyoruz ve yakında bitecek, bu yüzden herhangi bir değişikliğe gerek yok. Hiçbir şey kırık, bizim bir cevapsız bitiş noktası sadece Google'dan uyarı mesajları neden oldu.

**S:** Mevcut kullanıcıları kırmadan yeni FCM SDK'lara ve kitaplıklara nasıl geçiş yapabilirim?

C: İstediğinzaman yükseltin. Google henüz mevcut GCM SDK'ların ve kütüphanelerin herhangi bir amortismanı duyurdu değil. Mevcut kullanıcılarınıza yönelik anında iletme bildirimlerini kesmemeniz için, mevcut GCM projenizle ilişkilendirdiğiniz yeni Firebase projesini oluştururken emin olun. Bu, yeni Firebase sırlarının GCM SDK'lar ve kütüphanelerle uygulamanızın eski sürümlerini çalıştıran kullanıcıların yanı sıra FCM SDK'lar ve kitaplıklarla uygulamanızın yeni kullanıcıları için çalışmasını sağlayacaktır.

**S:** Bildirimlerim için yeni FCM özelliklerini ve şemalarını ne zaman kullanabilirim?

**A:** API ve SDK'larımız için bir güncelleme yayınladıktan sonra, bizi izlemeye devam edin – önümüzdeki aylarda sizin için bir şeyler olmasını bekliyoruz.
