---
title: Yaygın kimlik doğrulama hatalarıyla ilgili sorunları giderme | Azure Marketi
description: Bulut İş Ortağı Portalı API 'Leri kullanılırken ortak kimlik doğrulama hatalarıyla ilgili yardım sağlar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: d8fd1eb4bef987b4a8605e4be780512a914ec8b5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81256000"
---
# <a name="troubleshooting-common-authentication-errors"></a>Yaygın kimlik doğrulama hatalarında sorun giderme

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri iş ortağı merkezi ile tümleşiktir ve teklifleriniz iş ortağı merkezi 'ne geçirildikten sonra çalışmaya devam edecektir. Tümleştirme küçük değişiklikler sunar. İş Ortağı Merkezi 'ne geçişten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

Bu makalede, Bulut İş Ortağı Portalı API 'Leri kullanılırken ortak kimlik doğrulama hatalarıyla ilgili yardım sağlanmaktadır.

## <a name="unauthorized-error"></a>Yetkisiz hata

Sürekli olarak hata alırsanız `401 unauthorized` , geçerli bir erişim belirteciniz olduğunu doğrulayın.  Daha önce yapmadıysanız, [kaynaklara erişebilen Azure Active Directory bir uygulama ve hizmet sorumlusu oluşturmak için Portal kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)bölümünde açıklandığı gibi temel bir Azure Active Directory (Azure AD) uygulaması ve hizmet sorumlusu oluşturun. Daha sonra, erişimi doğrulamak için uygulamayı veya basit bir HTTP POST isteğini kullanın.  Aşağıdaki görüntüde gösterildiği gibi, erişim belirtecini almak için kiracı KIMLIĞI, uygulama KIMLIĞI, nesne KIMLIĞI ve gizli anahtarı dahil edersiniz:

![401 hatası sorunlarını giderme](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-401-error.jpg)


## <a name="forbidden-error"></a>Yasak hatası

Bir `403 forbidden` hata alırsanız, bulut iş ortağı portalı yayımcı hesabınıza doğru hizmet sorumlusu eklendiğinden emin olun.
Hizmet sorumlunuzu portala eklemek için [Önkoşullar](./cloud-partner-portal-api-prerequisites.md) sayfasındaki adımları izleyin.

Doğru hizmet sorumlusu eklendiyse, diğer tüm bilgileri doğrulayın. Portalda girilen nesne KIMLIĞINE yakın bir uyarı ödeyin. Azure Active Directory uygulama kaydı sayfasında iki nesne kimliği vardır ve yerel nesne kimliğini kullanmanız gerekir. Uygulamanızın **uygulama kayıtları** sayfasına giderek ve **yerel dizinde yönetilen uygulama**altındaki uygulama adına tıklayarak doğru değeri bulabilirsiniz. Bu sizi, aşağıdaki şekilde gösterildiği gibi **Özellikler** sayfasında doğru nesne kimliğini bulabileceğiniz uygulamanın yerel özelliklerine götürür. Ayrıca, hizmet sorumlusunu eklerken ve API çağrısını yaptığınızda doğru Yayımcı KIMLIĞINI kullandığınızdan emin olun.

![403 hatası sorunlarını giderme](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-403-error.jpg)
