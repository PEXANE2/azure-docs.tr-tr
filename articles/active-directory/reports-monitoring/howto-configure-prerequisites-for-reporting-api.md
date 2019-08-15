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
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: f925a86504d68fd08b83c63e4da8b37b4aa25f85
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68989911"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Azure Active Directory Raporlama API 'sine erişim önkoşulları

[Azure Active Directory (Azure AD) raporlama API'leri](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview), bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz.

Raporlama API 'SI, Web API 'Lerine erişim yetkisi vermek için [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) kullanır.

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

Raporlama API 'sine bir betik kullanarak erişiyorsanız bile bir uygulamayı kaydetmeniz gerekir. Bu size, yetkilendirme çağrıları için gerekli olan ve kodunuzun belirteçleri almasını sağlayan bir **uygulama kimliği**sağlar.

Dizininizi Azure AD Raporlama API 'sine erişecek şekilde yapılandırmak için, Azure AD kiracınızda **genel yönetici** dizini rolünün da üyesi olan bir Azure yönetici hesabıyla [Azure Portal](https://portal.azure.com) oturum açmalısınız.

> [!IMPORTANT]
> Yönetici ayrıcalıklarına sahip kimlik bilgileri altında çalışan uygulamalar çok güçlü olabilir, bu nedenle lütfen uygulamanın KIMLIĞI ve gizli kimlik bilgilerini güvenli bir konumda tutmayı unutmayın.
> 

**Bir Azure AD uygulamasını kaydetmek için:**

1. [Azure Portal](https://portal.azure.com)sol gezinti bölmesinden **Azure Active Directory** ' i seçin.
   
    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. **Azure Active Directory** sayfasında **uygulama kayıtları**' i seçin.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. **Uygulama kayıtları** sayfasından **Yeni uygulama kaydı**' nı seçin.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. **Oluştur** sayfasında, aşağıdaki adımları uygulayın:

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. **Ad** metin kutusuna yazın `Reporting API application`.

    b. **Uygulama türü**olarak **Web uygulaması/API**' yi seçin.

    c. **Oturum açma URL 'si** metin kutusuna yazın `https://localhost`.

    d. **Oluştur**’u seçin. 


## <a name="grant-permissions"></a>İzinleri verme 

Erişmek istediğiniz API 'ye bağlı olarak, uygulamanıza aşağıdaki izinleri vermeniz gerekir:  

| API | İzin |
| --- | --- |
| Windows Azure Active Directory | Dizin verilerini okuma |
| Microsoft Graph | Tüm denetim günlüğü verilerini okuyun |


![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/36.png)

Aşağıdaki bölümde her iki API için de adımlar listelenmektedir. API 'lerden birine erişmek istemiyorsanız ilgili adımları atlayabilirsiniz.

**API 'Leri kullanmak için uygulamanıza izin vermek için:**

1. Uygulama **kayıtları** sayfasından uygulamanızı seçin ve **Ayarlar**' ı seçin. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/05.png)

2. **Ayarlar** sayfasında, **gerekli izinler**' i seçin. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/06.png)

3. **Gerekli izinler** sayfasında, **API** listesinde, **Windows Azure Active Directory**' a tıklayın. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/07.png)

4. **Erişimi etkinleştir** sayfasında, **Dizin verilerini oku** ' nı seçin ve **oturum aç ' ın işaretini kaldırın ve kullanıcı profilini okuyun**. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/08.png)

5. Üstteki araç çubuğunda **Kaydet**' e tıklayın.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/15.png)

6. **Gerekli izinler** sayfasında, üstteki araç çubuğundan **Ekle**' ye tıklayın.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/32.png)

7. **API erişimi ekle** sayfasında, **bir API seçin**' e tıklayın.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/31.png)

8. **BIR API seçin** sayfasında, **Microsoft Graph**' a ve ardından **Seç**' e tıklayın.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/33.png)

9. **Erişimi etkinleştir** sayfasında, **tüm denetim günlüğü verilerini oku**' nı seçin ve ardından **Seç**' e tıklayın.  

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/34.png)

10. **API erişimi ekle** sayfasında **bitti**' ye tıklayın.  

11. **Gerekli izinler** sayfasında, üstteki araç çubuğundan. **Izin ver**' e tıklayın ve ardından **Evet**' e tıklayın.

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/17.png)


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
Uygulamanızın istemci gizli anahtarını almak için yeni bir anahtar oluşturmanız ve bu değeri daha sonra almak mümkün olmadığından yeni anahtarı kaydettikten sonra değerini kaydetmeniz gerekir.

**Uygulamanızın istemci gizli anahtarını almak için:**

1. [Azure Portal](https://portal.azure.com), sol gezinti bölmesinde **Azure Active Directory**' e tıklayın.
   
    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2.  Uygulama **kayıtları** sayfasından uygulamanızı seçin.

3. Uygulama sayfasında, üstteki araç çubuğundan **Ayarlar**' ı seçin. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/05.png)

4. **Ayarlar** sayfasındaki **API erişimi** bölümünde **anahtarlar**' a tıklayın. 

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/12.png)

5. **Anahtarlar** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Uygulamayı kaydet](./media/howto-configure-prerequisites-for-reporting-api/14.png)

    a. **Açıklama** metin kutusuna yazın `Reporting API`.

    b. **Süresi sona erdiğinde** **2 yıl içinde**öğesini seçin.

    c. **Kaydet**’e tıklayın.

    d. Anahtar değerini kopyalayın.

## <a name="troubleshoot-errors-in-the-reporting-api"></a>Raporlama API 'sindeki hataların sorunlarını giderme

Bu bölümde, MS Graph API kullanarak etkinlik raporlarına ve bunların çözünürlüğüne yönelik adımlara erişirken karşılaşabileceğiniz ortak hata iletileri listelenmektedir.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>Microsoft Graph v2 uç noktasına erişirken HTTP iç sunucu hatası 500

Şu anda Microsoft Graph v2 uç noktasını desteklemiyoruz-Microsoft Graph v1 uç noktasını kullanarak etkinlik günlüklerine erişebildiğinizden emin olun.

### <a name="error-failed-to-get-user-roles-from-ad-graph"></a>Hata: AD grafiğinden Kullanıcı rolleri alınamadı

Bu hata iletisini, Graph Explorer kullanarak oturum açma erişimlerine erişmeye çalışırken alabilirsiniz. Aşağıdaki görüntüde gösterildiği gibi, Graph Explorer Kullanıcı arabirimindeki oturum açma düğmelerinden her ikisini kullanarak hesabınızda oturum açtığınızdan emin olun. 

![Graph Gezgini](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-ad-graph"></a>Hata: AD grafiğinden Premium lisans denetimi yapılamadı 

Graph Explorer kullanarak oturum açma erişimiyle çalışmaya çalışırken bu hata iletisiyle karşılaşırsanız, sol gezinti bölmesinde hesabınızın altındaki **Izinleri Değiştir** ' i seçin ve **Görevler. ReadWrite** ve **Dizin. Read. All**' ı seçin. 

![İzin Kullanıcı arabirimini değiştirme](./media/troubleshoot-graph-api/modify-permissions.png)


### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Hata: Hiçbir kiracı B2C değil veya kiracıda Premium lisansa sahip değil

Oturum açma raporlarına erişmek için Azure Active Directory Premium 1 (P1) lisansı gerekir. Oturum açma işlemleri sırasında bu hata iletisini görürseniz, kiracınızın bir Azure AD P1 lisansıyla lisanslanmasını sağlayın.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Hata: Kullanıcı izin verilen rollerde değil 

API kullanarak denetim günlüklerine veya oturum açma erişimine erişmeye çalışırken bu hata iletisini görürseniz, hesabınızın Azure Active Directory kiracınızdaki **güvenlik okuyucusu** veya **rapor okuyucu** rolünün bir parçası olduğundan emin olun. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Hata: Uygulamada AAD ' Read Directory Data ' izni eksik 

Lütfen uygulamanızın doğru izin kümesiyle çalıştığından emin olmak için [Azure Active Directory Raporlama API 'sine erişmek üzere ön koşullar](howto-configure-prerequisites-for-reporting-api.md) bölümündeki adımları izleyin. 

### <a name="error-application-missing-msgraph-api-read-all-audit-log-data-permission"></a>Hata: Uygulama eksik MSGraph API 'SI ' tüm denetim günlüğü verilerini oku ' izni

Lütfen uygulamanızın doğru izin kümesiyle çalıştığından emin olmak için [Azure Active Directory Raporlama API 'sine erişmek üzere ön koşullar](howto-configure-prerequisites-for-reporting-api.md) bölümündeki adımları izleyin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Sertifikalarla Azure Active Directory Raporlama API 'sini kullanarak veri al](tutorial-access-api-with-certificates.md)
* [API başvurusunu denetle](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Oturum açma Etkinliği raporu API başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
