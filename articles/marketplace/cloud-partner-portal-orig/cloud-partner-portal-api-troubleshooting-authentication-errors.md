---
title: Sık karşılaşılan kimlik doğrulama hatalarını giderme | Azure Marketi
description: Bulut İş Ortağı Api'lerini kullanırken yaygın kimlik doğrulama hatalarıyla ilgili yardım sağlar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: d8fd1eb4bef987b4a8605e4be780512a914ec8b5
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256000"
---
# <a name="troubleshooting-common-authentication-errors"></a>Sık karşılaşılan kimlik doğrulama hatalarını giderme

> [!NOTE]
> Bulut İş Ortağı Portalı API'leri İş Ortağı Merkezi ile entegre edilmiştir ve teklifleriniz İş Ortağı Merkezi'ne geçtikten sonra çalışmaya devam edecektir. Entegrasyon küçük değişiklikler sunar. Kodunuzu İş Ortağı Merkezi'ne geçişten sonra çalışmaya devam etmesini sağlamak için [Bulut İş Ortağı Portalı API Başvurusu'nda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

Bu makalede, Bulut İş Ortağı Portal API'leri kullanırken sık kullanılan kimlik doğrulama hataları ile ilgili yardım sağlar.

## <a name="unauthorized-error"></a>Yetkisiz hata

Sürekli olarak hata `401 unauthorized` alırsanız, geçerli bir erişim jetonuna sahip olduğunuzu doğrulayın.  Bunu daha önce yapmadıysanız, [kaynaklara erişebilen bir Azure Active Directory uygulaması ve hizmet ilkesi oluşturmak için Kullanım portalında](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)açıklandığı gibi temel bir Azure Etkin Dizin (Azure AD) uygulaması ve bir hizmet ilkesi oluşturun. Ardından, erişiminizi doğrulamak için uygulamayı veya basit bir HTTP POST isteğini kullanın.  Aşağıdaki resimde gösterildiği gibi erişim belirteci elde etmek için Kiracı Kimliği, Uygulama Kimliği, Nesne Kimliği ve gizli anahtarı içerecektir:

![401 hatasını giderme](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-401-error.jpg)


## <a name="forbidden-error"></a>Yasak hatası

Bir `403 forbidden` hata alırsanız, Bulut İş Ortağı Portalı'ndaki yayıncı hesabınıza doğru hizmet sorumlusunun eklendiğini unutmayın.
Hizmet müdürünüzü portala eklemek için [Önkoşullar](./cloud-partner-portal-api-prerequisites.md) sayfasındaki adımları izleyin.

Doğru hizmet sorumlusu eklendiyse, diğer tüm bilgileri doğrulayın. Portalda girilen Nesne Kimliği'ne dikkat edin. Azure Etkin Dizin uygulaması kayıt sayfasında iki Nesne Kimliği vardır ve yerel Nesne Kimliğini kullanmanız gerekir. Uygulamanızın **Uygulama kayıtları** sayfasına giderek ve **yerel dizinde Yönetilen uygulama**altında uygulama adını tıklayarak doğru değeri bulabilirsiniz. Bu, aşağıdaki şekilde gösterildiği gibi **Özellikler** sayfasında doğru Nesne Kimliğini bulabileceğiniz uygulamanın yerel özelliklerine götürür. Ayrıca, hizmet sorumlusunu eklerken doğru yayımcı kimliğini kullandığınızdan ve API çağrısı yaptığınızdan emin olun.

![403 hatasını giderme](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-403-error.jpg)
