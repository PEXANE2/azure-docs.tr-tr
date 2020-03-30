---
title: Bulut İş Ortağı Portalı API Başvurusu | Azure Marketi
description: Pazar api işlemlerinin açıklaması, kullanımı ön koşullar ve listesi.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 296c667876807bdd05a6281de461df76207b5490
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288555"
---
<a name="cloud-partner-portal-api-reference"></a>Bulut İş Ortağı Portalı API Başvurusu
==================================

Cloud Partner Portal REST API'leri, iş yüklerinin, tekliflerin ve yayıncı profillerinin programlı olarak alınmasına ve manipüle edilebilmesine olanak tanır. API'ler, işleme zamanında doğru izinleri zorlamak için rol tabanlı erişim denetimini (RBAC) kullanır.

Bu başvuru, Bulut İş Ortağı Portalı REST API'leri için teknik ayrıntıları sağlar. Bu belgedeki yük örnekleri yalnızca başvuru amaçlıdır ve yeni işlevler eklendikçe değiştirilebilir.


<a name="prerequisites-and-considerations"></a>Ön koşullar ve değerlendirmeler
-------------------------------

API'leri kullanmadan önce şunları gözden geçirmelisiniz:

- Hesabınıza bir hizmet yöneticisinin nasıl ekleyeceğinizi öğrenmek ve kimlik doğrulama için Azure Etkin Dizin (Azure AD) erişim jetonu almak için [Önkoşullar](./cloud-partner-portal-api-prerequisites.md) makalesi. 
- İki [eşzamanlılık denetimi.](./cloud-partner-portal-api-concurrency-control.md)
bu API'leri aramak için stratejiler mevcuttur.
- Sürüm ve hata işleme gibi ek API [değerlendirmeleri.](./cloud-partner-portal-api-considerations.md)


<a name="common-tasks"></a>Genel görevler
------------
Bu başvuru, aşağıdaki ortak görevleri gerçekleştirmek için API'leri ayrıntılarıyla anlatır.


### <a name="offers"></a>Teklifler

-   [Tüm teklifleri alın](./cloud-partner-portal-api-retrieve-offers.md)
-   [Belirli bir teklifi alma](./cloud-partner-portal-api-retrieve-specific-offer.md)
-   [Teklif durumunu alma](./cloud-partner-portal-api-retrieve-offer-status.md)
-   [Teklif oluşturma](./cloud-partner-portal-api-creating-offer.md)
-   [Teklif yayımlama](./cloud-partner-portal-api-publish-offer.md)

### <a name="operations"></a>İşlemler

-   [İşlemleri alma](./cloud-partner-portal-api-retrieve-operations.md)
-   [İşlemleri iptal etme](./cloud-partner-portal-api-cancel-operations.md)

### <a name="publish-an-app"></a>Uygulama yayımlama

-   [Canlı yayına geçme](./cloud-partner-portal-api-go-live.md)

### <a name="other-tasks"></a>Diğer görevler

-   [Sanal makine teklifleri için fiyatlandırma yı ayarlama](./cloud-partner-portal-api-setting-price.md)

### <a name="troubleshooting"></a>Sorun giderme

-   [Kimlik doğrulama hatalarını giderme](./cloud-partner-portal-api-troubleshooting-authentication-errors.md)
