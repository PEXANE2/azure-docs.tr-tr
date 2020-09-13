---
title: Yaygın kimlik doğrulama hatalarında sorun giderme, Azure Marketi
description: Azure Marketi 'nde Bulut İş Ortağı Portalı API 'Leri kullanılırken ortak kimlik doğrulama hatalarıyla ilgili yardım sağlar.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: troubleshooting
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 01af5357e4ae2f4dfb317a0931a8d0bc2b2d54e1
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89487326"
---
# <a name="troubleshooting-common-authentication-errors"></a>Yaygın kimlik doğrulama hatalarında sorun giderme

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri ile tümleşiktir ve Iş Ortağı Merkezi 'nde çalışmaya devam edecektir. Geçiş küçük değişiklikler sunar. Iş Ortağı Merkezi 'ne geçtikten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](./cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin. CPP API 'Leri yalnızca Iş Ortağı Merkezi 'ne geçişten önce tümleştirilmiş mevcut ürünler için kullanılmalıdır; Yeni ürünlerin Iş Ortağı Merkezi gönderme API 'Leri kullanması gerekir.

Bu makalede, Bulut İş Ortağı Portalı API 'Leri kullanılırken ortak kimlik doğrulama hatalarıyla ilgili yardım sağlanmaktadır.

## <a name="unauthorized-error"></a>Yetkisiz hata

Sürekli `401 unauthorized` olarak hata alırsanız, geçerli bir erişim belirteciniz olduğunu doğrulayın.  Daha önce yapmadıysanız, [kaynaklara erişebilen Azure Active Directory bir uygulama ve hizmet sorumlusu oluşturmak için Portal kullanma](../active-directory/develop/howto-create-service-principal-portal.md)bölümünde açıklandığı gibi temel bir Azure Active Directory (Azure AD) uygulaması ve hizmet sorumlusu oluşturun. Daha sonra, erişimi doğrulamak için uygulamayı veya basit bir HTTP POST isteğini kullanın.  Erişim belirtecini almak için kiracı KIMLIĞI, uygulama KIMLIĞI, nesne KIMLIĞI ve gizli anahtar dahil edersiniz.

## <a name="forbidden-error"></a>Yasak hatası

Bir `403 forbidden` hata alırsanız, bulut iş ortağı portalı yayımcı hesabınıza doğru hizmet sorumlusu eklendiğinden emin olun. Hizmet sorumlunuzu portala eklemek için [Önkoşullar](./cloud-partner-portal-api-prerequisites.md) sayfasındaki adımları izleyin.

Doğru hizmet sorumlusu eklendiyse, diğer tüm bilgileri doğrulayın. Portalda girilen nesne KIMLIĞINE yakın bir uyarı ödeyin. Azure Active Directory uygulama kaydı sayfasında iki nesne kimliği vardır ve yerel nesne kimliğini kullanmanız gerekir. Uygulamanızın **uygulama kayıtları** sayfasına giderek ve **yerel dizinde yönetilen uygulama**altındaki uygulama adına tıklayarak doğru değeri bulabilirsiniz. Bu sizi, aşağıdaki şekilde gösterildiği gibi **Özellikler** sayfasında doğru nesne kimliğini bulabileceğiniz uygulamanın yerel özelliklerine götürür. Ayrıca, hizmet sorumlusunu eklerken ve API çağrısını yaptığınızda doğru Yayımcı KIMLIĞINI kullandığınızdan emin olun.
