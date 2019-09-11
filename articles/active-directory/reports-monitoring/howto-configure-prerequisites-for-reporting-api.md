---
title: Azure Active Directory Raporlama API 'sine erişim önkoşulları | Microsoft Docs
description: Azure AD Raporlama API 'sine erişim önkoşulları hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 08/30/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: f7b6fab4a4a36691bbdeb11975c7a93b97ab86cb
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70241648"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Azure Active Directory Raporlama API 'sine erişim önkoşulları

[Azure Active Directory (Azure AD) raporlama API'leri](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview), bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Bu API 'Leri, programlama dilleri ve araçlarından çağırabilirsiniz.

Raporlama API 'SI, Web API 'Lerine erişim yetkisi vermek için [OAuth](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) kullanır.

Raporlama API 'sine erişiminizi hazırlamak için şunları yapmanız gerekir:

1. [Rol Atama](#assign-roles)
2. [Uygulamayı kaydetme](#register-an-application)
3. [İzin verme](#grant-permissions)
4. [Yapılandırma ayarlarını topla](#gather-configuration-settings)

## <a name="assign-roles"></a>Rol atama

API aracılığıyla raporlama verilerine erişim sağlamak için, aşağıdaki rollerden birine atanmış olması gerekir:

- Güvenlik Okuyucusu

- Güvenlik Yöneticisi

- Genel Yönetici


## <a name="register-an-application"></a>Bir uygulamayı kaydet

Raporlama API 'sine bir betik kullanarak erişiyorsanız bile kayıt gereklidir. Kayıt, yetkilendirme çağrıları için gerekli olan bir **uygulama kimliği**sağlar ve kodunuzun belirteçleri almasını sağlar.

Dizininizi Azure AD Raporlama API 'sine erişecek şekilde yapılandırmak için, Azure AD kiracınızda **genel yönetici** dizini rolünün da üyesi olan bir Azure yönetici hesabıyla [Azure Portal](https://portal.azure.com) oturum açmalısınız.

> [!IMPORTANT]
> Yönetici ayrıcalıklarına sahip kimlik bilgileri altında çalışan uygulamalar çok güçlü olabilir, bu nedenle lütfen uygulamanın KIMLIĞI ve gizli kimlik bilgilerini güvenli bir konumda tutmayı unutmayın.
> 

**Bir Azure AD uygulamasını kaydetmek için:**

1. [Azure Portal](https://portal.azure.com)sol gezinti bölmesinden **Azure Active Directory** ' i seçin.
   
    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. **Azure Active Directory** sayfasında **uygulama kayıtları**' i seçin.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. **Uygulama kayıtları** sayfasından **Yeni kayıt**' yi seçin.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. **Uygulama kaydetme** sayfası:

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. **Ad** metin kutusuna yazın `Reporting API application`.

    b. **Desteklenen hesap türü**için **yalnızca bu kuruluştaki hesaplar**' ı seçin.

    c. **Yeniden yönlendirme URL 'si** ' nde **Web** metin kutusu `https://localhost`seçin yazın.

    d. **Kaydol**’u seçin. 


## <a name="grant-permissions"></a>İzinleri verme 

Erişmek istediğiniz API 'ye bağlı olarak, uygulamanıza aşağıdaki izinleri vermeniz gerekir:  

| API | İzin |
| --- | --- |
| Windows Azure Active Directory | Dizin verilerini okuma |
| Microsoft Graph | Tüm denetim günlüğü verilerini okuyun |


![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/36.png)

Aşağıdaki bölümde her iki API için de adımlar listelenmektedir. API 'lerden birine erişmek istemiyorsanız ilgili adımları atlayabilirsiniz.

**API 'Leri kullanmak için uygulamanıza izin vermek için:**


1. **API izinleri** ' ni seçin ve **izin ekleyin**. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/05.png)

2. **API Izinleri iste sayfasında**, **Eski API** **Azure Active Directory grafiğini**destekler ' i bulun. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/06.png)

3. **Gerekli izinler** sayfasında **Uygulama izinleri**' ni, **Dizin** onay kutusu dizini ' ni genişletin **. ReadAll**' u seçin.  **Izin Ekle**' yi seçin.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/07.png)

4. **Raporlama API 'Si uygulaması-API izinleri** sayfasında, yönetici izni **ver**' i seçin. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/08.png)

5. Not: **Microsoft Graph** , API kaydı sırasında varsayılan olarak eklenir.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/15.png)

## <a name="gather-configuration-settings"></a>Yapılandırma ayarlarını topla 

Bu bölümde, dizininizden aşağıdaki ayarların nasıl alınacağı gösterilmektedir:

- Etki alanı adı
- İstemci Kimliği
- İstemci parolası

Raporlama API 'sine yapılan çağrıları yapılandırırken bu değerlere ihtiyacınız vardır. 

### <a name="get-your-domain-name"></a>Etki alanı adınızı alın

**Etki alanı adınızı almak için:**

1. [Azure Portal](https://portal.azure.com), sol gezinti bölmesinde **Azure Active Directory**' i seçin.
   
    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. **Azure Active Directory** sayfasında, **özel etki alanı adları**' nı seçin.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Etki alanı adınızı etki alanları listesinden kopyalayın.


### <a name="get-your-applications-client-id"></a>Uygulamanızın istemci KIMLIĞINI alın

**Uygulamanızın istemci KIMLIĞINI almak için:**

1. [Azure Portal](https://portal.azure.com), sol gezinti bölmesinde **Azure Active Directory**' e tıklayın.
   
    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Uygulama **kayıtları** sayfasından uygulamanızı seçin.

3. Uygulama sayfasından **uygulama kimliği** ' ne gidin ve **kopyalamak Için tıklayın ' ı**seçin.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/11.png) 


### <a name="get-your-applications-client-secret"></a>Uygulamanızın istemci gizliliğini alın
 Denetim günlüklerine erişmeye veya API kullanarak oturum açmaya çalışırken hatalardan kaçının.

**Uygulamanızın istemci gizli anahtarını almak için:**

1. [Azure Portal](https://portal.azure.com), sol gezinti bölmesinde **Azure Active Directory**' e tıklayın.
   
    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2.  Uygulama **kayıtları** sayfasından uygulamanızı seçin.

3.  **API uygulaması** sayfasında **Sertifikalar ve gizlilikler** ' ı seçin, **Istemci gizli** dizileri bölümünde **+ yeni istemci parolası**' na tıklayın. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/12.png)

5. **İstemci parolası Ekle** sayfasında şunları ekleyin:

    a. **Açıklama** metin kutusuna yazın `Reporting API`.

    b. **Süresi sona erdiğinde** **2 yıl içinde**öğesini seçin.

    c. **Kaydet**’e tıklayın.

    d. Anahtar değerini kopyalayın.

## <a name="troubleshoot-errors-in-the-reporting-api"></a>Raporlama API 'sindeki hataların sorunlarını giderme

Bu bölümde, MS Graph API kullanarak etkinlik raporlarına ve bunların çözünürlüğüne yönelik adımlara erişirken karşılaşabileceğiniz ortak hata iletileri listelenmektedir.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>Microsoft Graph v2 uç noktasına erişirken HTTP iç sunucu hatası 500

Şu anda Microsoft Graph v2 uç noktasını desteklemiyoruz-Microsoft Graph v1 uç noktasını kullanarak etkinlik günlüklerine erişebildiğinizden emin olun.

### <a name="error-failed-to-get-user-roles-from-ad-graph"></a>Hata: AD grafiğinden Kullanıcı rolleri alınamadı

 Graph Explorer 'ı kullanarak oturum açmaya çalışırken bir hata oluşmasını önlemek için, Graph Explorer Kullanıcı arabirimindeki oturum açma düğmelerini kullanarak hesabınızda oturum açın. 

![Graph Gezgini](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-ad-graph"></a>Hata: AD grafiğinden Premium lisans denetimi yapılamadı 

Graph Explorer kullanarak oturum açma erişimiyle çalışmaya çalışırken bu hata iletisiyle karşılaşırsanız, sol gezinti bölmesinde hesabınızın altındaki **Izinleri Değiştir** ' i seçin ve **Görevler. ReadWrite** ve **Dizin. Read. All**' ı seçin. 

![İzin Kullanıcı arabirimini değiştirme](./media/troubleshoot-graph-api/modify-permissions.png)


### <a name="error-tenant-is-not-b2c-or-tenant-doesnt-have-premium-license"></a>Hata: Kiracı B2C değil veya kiracı Premium lisansa sahip değil

Oturum açma raporlarına erişmek için Azure Active Directory Premium 1 (P1) lisansı gerekir. Oturum açma işlemleri sırasında bu hata iletisini görürseniz, kiracınızın bir Azure AD P1 lisansıyla lisanslanmasını sağlayın.

### <a name="error-the-allowed-roles-does-not-include-user"></a>Hata: İzin verilen roller Kullanıcı içermez. 

 Denetim günlüklerine erişmeye veya API kullanarak oturum açmaya çalışırken hatalardan kaçının. Hesabınızın Azure Active Directory kiracınızdaki **güvenlik okuyucusu** veya **rapor okuyucu** rolünün bir parçası olduğundan emin olun.

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Hata: Uygulamada AAD ' Read Directory Data ' izni eksik 

### <a name="error-application-missing-msgraph-api-read-all-audit-log-data-permission"></a>Hata: Uygulama eksik MSGraph API 'SI ' tüm denetim günlüğü verilerini oku ' izni

Uygulamanızın doğru izin kümesiyle çalıştığından emin olmak için [Azure Active Directory Raporlama API 'sine erişmek Için önkoşulların](howto-configure-prerequisites-for-reporting-api.md) içindeki adımları izleyin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Sertifikalarla Azure Active Directory Raporlama API 'sini kullanarak veri al](tutorial-access-api-with-certificates.md)
* [API başvurusunu denetle](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Oturum açma Etkinliği raporu API başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
