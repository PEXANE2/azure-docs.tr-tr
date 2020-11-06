---
title: Azure AD 'ye genel istemci uygulaması kaydetme-FHIR için Azure API
description: Bu makalede, Azure 'da FHıR API 'SI dağıtmaya yönelik olarak Azure Active Directory bir genel istemci uygulamasının nasıl kaydedileceği açıklanmaktadır.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: ff7bb6084839af56b5f6e874b39929021f23f8a1
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93398088"
---
# <a name="register-a-public-client-application-in-azure-active-directory"></a>Ortak istemci uygulamasını Azure Active Directory kaydetme

Bu makalede, Azure Active Directory bir ortak uygulamayı nasıl kaydedeceğinizi öğreneceksiniz.  

İstemci uygulama kayıtları, kimlik doğrulayabilecek ve Kullanıcı adına API izinleri sorabileceğiniz uygulamaların Azure Active Directory temsilleridir. Ortak istemciler, gizli dizileri gizli tutabilecek mobil uygulamalar ve tek sayfalı JavaScript uygulamaları gibi uygulamalardır. Yordam, [gizli bir istemciyi kaydetmeye](register-confidential-azure-ad-client-app.md)benzerdir, ancak ortak istemcilerin uygulama gizli dizisi tutmak için güvenilir olmaması gerektiğinden, bir tane eklemeniz gerekmez.

Hızlı başlangıç, [Microsoft Identity platformu ile bir uygulamanın nasıl kaydedileceği](../active-directory/develop/quickstart-register-app.md)hakkında genel bilgiler sağlar.

## <a name="app-registrations-in-azure-portal"></a>Azure portal Uygulama kayıtları

1. [Azure portalda](https://portal.azure.com) sol taraftaki gezinti panelinden **Azure Active Directory** ’ye tıklayın.

2. **Azure Active Directory** dikey penceresinde **uygulama kayıtları** ' e tıklayın:

    ![Azure portal. Yeni uygulama kaydı.](media/how-to-aad/portal-aad-new-app-registration.png)

3. **Yeni kayda** tıklayın.

## <a name="application-registration-overview"></a>Uygulama kaydına genel bakış

1. Uygulamaya bir görünen ad verin.

2. Yanıt URL 'SI girin. Yanıt URL 'SI, kimlik doğrulama kodlarının istemci uygulamasına dönecektir. Daha sonra, daha fazla yanıt URL 'Si ekleyebilir ve var olanları düzenleyebilirsiniz.

    ![Azure portal. Yeni genel uygulama kaydı.](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-NAME.png)


[Masaüstü](../active-directory/develop/scenario-desktop-app-registration.md), [Mobil](../active-directory/develop/scenario-mobile-app-registration.md) veya [tek sayfalı](../active-directory/develop/scenario-spa-app-registration.md) uygulamanızı ortak uygulama olarak yapılandırmak için:

1. [Azure Portal](https://portal.azure.com), **uygulama kayıtları** ' de Uygulamanızı seçin ve **kimlik doğrulaması** ' nı seçin.

2. **Gelişmiş ayarlar**  >  **varsayılan istemci türü** ' nü seçin. **Uygulamayı ortak istemci olarak değerlendir** için **Evet** ' i seçin.

3. Tek sayfalı bir uygulama için, örtük akışı etkinleştirmek üzere belirteçleri ve **kimlik belirteçlerini** **erişim** ' i seçin.

   - Uygulamanız kullanıcıların oturumunu açarsa, **Kimlik belirteçleri** ' ni seçin.
   - Uygulamanızın de korumalı bir Web API 'SI çağırması gerekiyorsa, **erişim belirteçleri** ' ni seçin.

## <a name="api-permissions"></a>API izinleri

[Gizli istemci uygulamasına](register-confidential-azure-ad-client-app.md)benzer şekilde, bu uygulamanın kullanıcı adına ISTEYEBILMESI gereken API izinlerini seçmeniz gerekir:

1. **API izinlerini** açın.

    FHıR için Azure API kullanıyorsanız, **Kuruluşumun kullandığı API** 'Ler altında Azure sağlık API 'Lerini arayarak Azure sağlık API 'lerine bir izin ekleyeceksiniz. Bunu yalnızca [FHıR Için Azure API 'sini zaten dağıttıysanız](fhir-paas-powershell-quickstart.md)bulabilirsiniz.

    
    Farklı bir kaynak uygulamasına başvuruyorsam, daha önce **API 'Lerim** altında oluşturduğunuz [Fhır API kaynak uygulaması kaydınızı](register-resource-azure-ad-client-app.md) seçin:

    ![Azure portal. Yeni genel API izinleri-FHıR varsayılan için Azure API](media/public-client-app/api-permissions.png)


2. Uygulamanın isteyebilmesini istediğiniz izinleri seçin: ![ Azure Portal. Uygulama izinleri](media/public-client-app/app-permissions.png)

## <a name="validate-fhir-server-authority"></a>FHıR sunucu yetkilisini doğrula
Bu makaleye ve FHıR sunucunuza kaydettiğiniz uygulama aynı Azure AD kiracısında ise, sonraki adımlara devam etmeniz iyi bir uygulamadır.

İstemci uygulamanızı FHıR sunucunuzdaki farklı bir Azure AD kiracısında yapılandırırsanız, **yetkiyi** güncelleştirmeniz gerekir. FHıR için Azure API 'de, ayarlar--> kimlik doğrulaması altında yetkiyi ayarlarsınız. Yetkilinizi olarak ayarlayın **https://login.microsoftonline.com/\<TENANT-ID>** .

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Active Directory bir ortak istemci uygulamasını nasıl kaydedeceğinizi öğrendiniz. Ardından, Postman kullanarak FHıR sunucunuza erişimi test edin.
 
>[!div class="nextstepaction"]
>[Postman ile FHıR için Azure API 'sine erişme](access-fhir-postman-tutorial.md)