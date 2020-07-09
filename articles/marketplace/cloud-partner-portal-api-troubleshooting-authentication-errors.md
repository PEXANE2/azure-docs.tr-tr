---
title: Yaygın kimlik doğrulama hatalarıyla ilgili sorunları giderme | Azure Marketi
description: Bulut İş Ortağı Portalı API 'Leri kullanılırken ortak kimlik doğrulama hatalarıyla ilgili yardım sağlar.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.openlocfilehash: 8c9bd6dbe54f797090c5f7405ad709db954f8fb9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86115477"
---
# <a name="troubleshooting-common-authentication-errors"></a>Yaygın kimlik doğrulama hatalarında sorun giderme

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri iş ortağı merkezi ile tümleşiktir ve teklifleriniz iş ortağı merkezi 'ne geçirildikten sonra çalışmaya devam edecektir. Tümleştirme küçük değişiklikler sunar. İş Ortağı Merkezi 'ne geçişten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](./cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin.

Bu makalede, Bulut İş Ortağı Portalı API 'Leri kullanılırken ortak kimlik doğrulama hatalarıyla ilgili yardım sağlanmaktadır.

## <a name="unauthorized-error"></a>Yetkisiz hata

Sürekli `401 unauthorized` olarak hata alırsanız, geçerli bir erişim belirteciniz olduğunu doğrulayın.  Daha önce yapmadıysanız, [kaynaklara erişebilen Azure Active Directory bir uygulama ve hizmet sorumlusu oluşturmak için Portal kullanma](../active-directory/develop/howto-create-service-principal-portal.md)bölümünde açıklandığı gibi temel bir Azure Active Directory (Azure AD) uygulaması ve hizmet sorumlusu oluşturun. Daha sonra, erişimi doğrulamak için uygulamayı veya basit bir HTTP POST isteğini kullanın.  Aşağıdaki görüntüde gösterildiği gibi, erişim belirtecini almak için kiracı KIMLIĞI, uygulama KIMLIĞI, nesne KIMLIĞI ve gizli anahtarı dahil edersiniz:

![401 hatası sorunlarını giderme](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-401-error.jpg)


## <a name="forbidden-error"></a>Yasak hatası

Bir `403 forbidden` hata alırsanız, bulut iş ortağı portalı yayımcı hesabınıza doğru hizmet sorumlusu eklendiğinden emin olun.
Hizmet sorumlunuzu portala eklemek için [Önkoşullar](./cloud-partner-portal-api-prerequisites.md) sayfasındaki adımları izleyin.

Doğru hizmet sorumlusu eklendiyse, diğer tüm bilgileri doğrulayın. Portalda girilen nesne KIMLIĞINE yakın bir uyarı ödeyin. Azure Active Directory uygulama kaydı sayfasında iki nesne kimliği vardır ve yerel nesne kimliğini kullanmanız gerekir. Uygulamanızın **uygulama kayıtları** sayfasına giderek ve **yerel dizinde yönetilen uygulama**altındaki uygulama adına tıklayarak doğru değeri bulabilirsiniz. Bu sizi, aşağıdaki şekilde gösterildiği gibi **Özellikler** sayfasında doğru nesne kimliğini bulabileceğiniz uygulamanın yerel özelliklerine götürür. Ayrıca, hizmet sorumlusunu eklerken ve API çağrısını yaptığınızda doğru Yayımcı KIMLIĞINI kullandığınızdan emin olun.

![403 hatası sorunlarını giderme](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-403-error.jpg)
