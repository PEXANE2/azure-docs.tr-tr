---
title: Azure Active Directory raporlama API için ön koşullar | Microsoft Dokümanlar
description: Azure AD raporlama API'sine erişmek için ön koşullar hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/04/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 12abfc0d345c937ae886f9bfacfb8ce30227cc45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78399308"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Azure Etkin Dizin raporlama API'sine erişmek için ön koşullar

[Azure Active Directory (Azure AD) raporlama API'leri](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-reporting-api), bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Bu API'leri programlama dillerinden ve araçlarından arayabilirsiniz.

Raporlama API'si, web API'lerine erişim yetkisi vermek için [OAuth'u](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) kullanır.

Raporlama API'sine erişiminizi hazırlamak için şunları yapmanız gerekir:

1. [Rolleri atama](#assign-roles)
2. [Bir uygulamayı kaydetme](#register-an-application)
3. [İzinleri verme](#grant-permissions)
4. [Yapılandırma ayarlarını toplama](#gather-configuration-settings)

## <a name="assign-roles"></a>Rolleri atama

API üzerinden raporlama verilerine erişmek için aşağıdaki rollerden birine atanmış olmanız gerekir:

- Güvenlik Okuyucu

- Güvenlik Yöneticisi

- Genel Yönetici


## <a name="register-an-application"></a>Bir uygulamayı kaydetme

Bir komut dosyası kullanarak raporlama API'sına erişiyor sanız bile kayıt gereklidir. Kayıt, yetkilendirme çağrıları için gerekli olan ve kodunuzu belirteçleri almasını sağlayan bir **Uygulama Kimliği**verir.

Dizininizi Azure AD raporlama API'sine erişmek üzere yapılandırmak için, Azure REKLAM kiracınızdaki **Genel Yönetici** dizin rolünün de üyesi olan bir Azure yönetici hesabıyla [Azure portalında](https://portal.azure.com) oturum açmanız gerekir.

> [!IMPORTANT]
> Yönetici ayrıcalıkları olan kimlik bilgileri altında çalışan uygulamalar çok güçlü olabilir, bu nedenle lütfen uygulamanın kimliğini ve gizli kimlik bilgilerini güvenli bir yerde sakladığından emin olun.
> 

**Azure AD uygulamasını kaydetmek için:**

1. Azure [portalında,](https://portal.azure.com)sol daki gezinti bölmesinden **Azure Etkin Dizin'i** seçin.
   
    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Azure **Etkin Dizin** sayfasında **Uygulama kayıtlarını**seçin.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. Uygulama **kayıtları** sayfasından **Yeni kayıt'ı**seçin.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. **Bir Başvuru Kaydı** sayfası:

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. **Ad** metin kutusuna, `Reporting API application`yazın.

    b. **Desteklenen hesap türü için,** yalnızca bu **kuruluşta Hesaplar'ı**seçin.

    c. Yönlendirme **URL'sinde** **Web** textbox'ı seçin, yazın. `https://localhost`

    d. **Kaydol**’u seçin. 


## <a name="grant-permissions"></a>İzinleri verme 

Erişmek istediğiniz API'ye bağlı olarak, uygulamanız için aşağıdaki izinleri vermeniz gerekir:  

| API | İzin |
| --- | --- |
| Windows Azure Active Directory | Dizin verilerini oku |
| Microsoft Graph | Tüm denetim günlüğü verilerini okuyun |


![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/36.png)

Aşağıdaki bölümde her iki API için adımlar listelenir. API'lerden birine erişmek istemiyorsanız, ilgili adımları atlayabilirsiniz.

**API'leri kullanmak için başvuru izni vermek için:**


1. **API izinlerini** seçin ve **ardından izin ekleyin.** 

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/05.png)

2. İstek **API izinleri sayfasında,** **Destek eski API** **Azure Etkin Dizin Grafiği'ni**bulun. 

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/06.png)

3. Gerekli **İzinler** **sayfasında, Uygulama İzinleri'ni**seçin, **Dizin** onay kutusunu **genişletin.ReadAll**.  **İzin Ekle'yi**seçin.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/07.png)

4. Raporlama **API Uygulaması - API İzinleri** sayfasında, **Yönetici yi bağışla'yı**seçin. 

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/08.png)

5. Not: **ApI** Kaydı sırasında Microsoft Graph varsayılan olarak eklenir.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/15.png)

## <a name="gather-configuration-settings"></a>Yapılandırma ayarlarını toplama 

Bu bölümde, dizininizden aşağıdaki ayarları nasıl alacağınızı gösterilmektedir:

- Etki alanı adı
- İstemci Kimliği
- Gizli anahtar

Raporlama API'sine yapılandırılabiliyorken bu değerlere ihtiyacınız var. 

### <a name="get-your-domain-name"></a>Etki alanı adınızı alın

**Alan adınızı almak için:**

1. Sol daki gezinti bölmesinde [bulunan Azure portalında](https://portal.azure.com) **Azure Etkin Dizin'i**seçin.
   
    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Azure **Etkin Dizin** sayfasında **Özel alan adlarını**seçin.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Etki alanı adınızı etki alanları listesinden kopyalayın.


### <a name="get-your-applications-client-id"></a>Uygulamanızın istemci kimliğini alma

**Başvurunuzun istemci kimliğini almak için:**

1. Sol daki gezinti bölmesinde [bulunan Azure portalında](https://portal.azure.com) **Azure Etkin Dizini'ni**tıklatın.
   
    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. **Uygulama Kayıtları** sayfasından başvurunuzu seçin.

3. Uygulama sayfasından Uygulama **Kimliği'ne** gidin ve **kopyalamak için Tıklayın'ı**seçin.

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/11.png) 


### <a name="get-your-applications-client-secret"></a>Uygulamanızın istemcisi sırrını alın
 API'yi kullanarak denetim günlüklerine veya oturum açmaya çalışırken hatalardan kaçının.

**Uygulamanızın istemci sırrını almak için:**

1. Sol daki gezinti bölmesinde [bulunan Azure portalında](https://portal.azure.com) **Azure Etkin Dizini'ni**tıklatın.
   
    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2.  **Uygulama Kayıtları** sayfasından başvurunuzu seçin.

3.  **API Uygulama** sayfasında, **Müşteri Sırları** bölümünde **Sertifikalar ve Sırlar'ı** seçin, **+ Yeni İstemci Sırrı'nı**tıklatın. 

    ![Uygulamayı kaydetme](./media/howto-configure-prerequisites-for-reporting-api/12.png)

5. **İstemci gizli** sayfasında şunları ekleyin:

    a. **Açıklama** metin kutusuna, `Reporting API`yazın.

    b. **Sona Erme**tarihi itibariyle , **2 yıl içinde**seçin.

    c. **Kaydet**'e tıklayın.

    d. Anahtar değerini kopyalayın.

## <a name="troubleshoot-errors-in-the-reporting-api"></a>Raporlama API'sindeki sorun giderme hataları

Bu bölümde, Microsoft Graph API'sini kullanarak etkinlik raporlarına erişirken karşılaşabileceğiniz yaygın hata iletileri ve bunların çözümlemesi için adımlar listelenebilir.

### <a name="error-failed-to-get-user-roles-from-microsoft-graph"></a>Hata: Microsoft Graph'tan kullanıcı rolleri alınamadı

 Graph Explorer'ı kullanarak oturum açmaya çalışırken hata almamak için Grafik Explorer UI'deki her iki oturum açma düğmesini kullanarak hesabınızda oturum açın. 

![Graph Gezgini](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-microsoft-graph"></a>Hata: Microsoft Graph'tan premium lisans denetimi başarısız oldu 

Graph Explorer'ı kullanarak oturum açma lara erişmeye çalışırken bu hata iletisiyle karşınıza çıkarsa, sol navigasyonda hesabınızın altındaki **İzinleri Değiştir'i** seçin ve **Tasks.ReadWrite** ve **Directory.Read.All'ı**seçin. 

![İzinleri değiştirme UI](./media/troubleshoot-graph-api/modify-permissions.png)

### <a name="error-tenant-is-not-b2c-or-tenant-doesnt-have-premium-license"></a>Hata: Kiracı B2C veya kiracı prim lisansı yok değildir

Oturum açma raporlarına erişmek için Azure Active Directory 1 (P1) lisansı gerekir. Oturum açma işlemlerine erişirken bu hata iletisi görüyorsanız, kiracınızın Azure AD P1 lisansıyla lisans aldığından emin olun.

### <a name="error-the-allowed-roles-does-not-include-user"></a>Hata: İzin verilen roller Kullanıcı içermez. 

 API'yi kullanarak denetim günlüklerine veya oturum açmaya çalışırken hatalardan kaçının. Azure Etkin Dizin kiracınızda hesabınızın **Güvenlik Okuyucuveya** **Rapor Okuyucu** rolünün bir parçası olduğundan emin olun.

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Hata: Uygulama eksik AAD 'Okuma dizin verileri' izni 

### <a name="error-application-missing-microsoft-graph-api-read-all-audit-log-data-permission"></a>Hata: Microsoft Graph API'si eksik uygulama 'Tüm denetim günlüğü verilerini okuma' izni

Uygulamanızın doğru izin kümesiyle çalıştığını sağlamak [için Azure Active Directory raporlama API'sine erişmek için Ön Koşullar'daki](howto-configure-prerequisites-for-reporting-api.md) adımları izleyin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Sertifikalarla Azure Active Directory raporlama API’sini kullanarak veri alma](tutorial-access-api-with-certificates.md)
* [Denetim API başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Oturum açma faaliyet raporu API başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
