---
title: Bulut İş Ortağı Portalı API Başvurusu | Azure Marketi
description: Pazar api işlemlerinin açıklaması, kullanımı ön koşullar ve listesi.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: b9f698ea81830aaa8761c05012cf6843d07ad5a4
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256425"
---
# <a name="cloud-partner-portal-api-reference"></a>Bulut İş Ortağı Portalı API Başvurusu

> [!NOTE]
> Bulut İş Ortağı Portalı API'leri İş Ortağı Merkezi ile entegre edilmiştir ve teklifleriniz İş Ortağı Merkezi'ne geçtikten sonra çalışmaya devam edecektir. Entegrasyon küçük değişiklikler sunar. Kodunuzu Ortak Merkezi'ne geçişten sonra çalışmaya devam etmesini sağlamak için bu belgede listelenen [CPP API'lerinde yapılan değişiklikleri](#changes-to-cpp-apis-after-the-migration-to-partner-center) gözden geçirin.

Cloud Partner Portal REST API'leri, iş yüklerinin, tekliflerin ve yayıncı profillerinin programlı olarak alınmasına ve manipüle edilebilmesine olanak tanır. API'ler, işleme zamanında doğru izinleri zorlamak için rol tabanlı erişim denetimini (RBAC) kullanır.

Bu başvuru, Bulut İş Ortağı Portalı REST API'leri için teknik ayrıntıları sağlar. Bu belgedeki yük örnekleri yalnızca başvuru amaçlıdır ve yeni işlevler eklendikçe değiştirilebilir.

## <a name="prerequisites-and-considerations"></a>Ön koşullar ve değerlendirmeler

API'leri kullanmadan önce şunları gözden geçirmelisiniz:

- Hesabınıza bir hizmet yöneticisinin nasıl ekleyeceğinizi öğrenmek ve kimlik doğrulama için Azure Etkin Dizin (Azure AD) erişim jetonu almak için [Önkoşullar](./cloud-partner-portal-api-prerequisites.md) makalesi.
- Bu API'leri aramak için kullanılabilen iki [eşzamanlılık kontrol](./cloud-partner-portal-api-concurrency-control.md) stratejileri.
- Sürüm ve hata işleme gibi ek API [değerlendirmeleri.](./cloud-partner-portal-api-considerations.md)

## <a name="changes-to-cpp-apis-after-the-migration-to-partner-center"></a>Ortak Merkezi'ne geçişten sonra CPP API'leri'nde yapılan değişiklikler

| **API** | **Açıklamayı değiştir** | **Etki** |
| ------- | ---------------------- | ---------- |
| POST Yayınla, GoLive, İptal Et | Geçirilen teklifler için yanıt üstbilgisi farklı bir biçime sahip olur, ancak işlem durumunu almak için göreli bir yolu göstererek aynı şekilde çalışmaya devam edecektir. | İlgili POST isteklerinden herhangi birini teklif için gönderirken, Konum üstbilgisi teklifin geçiş durumuna bağlı olarak iki biçimden birine sahip olacaktır:<ul><li>Geçirilmeyen teklifler<br>`/api/operations/{PublisherId}${offerId}$2$preview?api-version=2017-10-31`</li><li>Geçirilen teklifler<br>`/api/publishers/{PublisherId}/offers/{offereId}/operations/408a4835-0000-1000-0000-000000000000?api-version=2017-10-31`</li> |
| GET Operasyonu | Yanıtta daha önce desteklenen 'bildirim-e-posta' alanı olan teklif türleri için, bu alan amortismana kesilir ve artık geçirilen teklifler için iade edilmez. | Geçirilen teklifler için artık isteklerde belirtilen e-postalar listesine bildirim göndermeyiz. Bunun yerine, API hizmeti e-posta göndermek için İş Ortağı Merkezi'ndeki bildirim e-posta süreciyle uyumlu olacaktır. Özellikle, bildirimler İş Ortağı Merkezi'ndeki Hesap ayarlarınızın Satıcı iletişim bilgileri bölümünde ayarlanan e-posta adresine gönderilecektir.<br><br>Bildirimler için doğru e-postanın sağlandığından emin olmak için lütfen İş Ortağı Merkezi'ndeki [Hesap ayarlarınızın](https://partner.microsoft.com/dashboard/account/management) Satıcı iletişim bilgileri bölümünde ayarlanan e-posta adresini gözden geçirin.  |

## <a name="common-tasks"></a>Genel görevler

Bu başvuru, aşağıdaki ortak görevleri gerçekleştirmek için API'leri ayrıntılarıyla anlatır.

### <a name="offers"></a>Teklifler

- [Tüm teklifleri alın](./cloud-partner-portal-api-retrieve-offers.md)
- [Belirli bir teklifi alma](./cloud-partner-portal-api-retrieve-specific-offer.md)
- [Teklif durumunu alma](./cloud-partner-portal-api-retrieve-offer-status.md)
- [Teklif oluşturma](./cloud-partner-portal-api-creating-offer.md)
- [Teklif yayımlama](./cloud-partner-portal-api-publish-offer.md)

### <a name="operations"></a>İşlemler

- [İşlemleri alma](./cloud-partner-portal-api-retrieve-operations.md)
- [İşlemleri iptal etme](./cloud-partner-portal-api-cancel-operations.md)

### <a name="publish-an-app"></a>Uygulama yayımlama

- [Canlı yayına geçme](./cloud-partner-portal-api-go-live.md)

### <a name="other-tasks"></a>Diğer görevler

- [Sanal makine teklifleri için fiyatlandırma yı ayarlama](./cloud-partner-portal-api-setting-price.md)

### <a name="troubleshooting"></a>Sorun giderme

- [Kimlik doğrulama hatalarını giderme](./cloud-partner-portal-api-troubleshooting-authentication-errors.md)
