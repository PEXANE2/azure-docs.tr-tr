---
title: Hizmetten hizmete kimlik doğrulaması-Data Lake Storage 1.-Azure
description: Azure Active Directory kullanarak Azure Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması elde etme hakkında bilgi edinin.
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 74ad40eb7f7483bb010cf8eb002776893c50a256
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82688194"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Azure Active Directory kullanarak Azure Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)
> * [Hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)
> 
>  

Azure Data Lake Storage 1., kimlik doğrulaması için Azure Active Directory kullanır. Data Lake Storage 1. ile birlikte çalışarak bir uygulamayı yazmadan önce, Azure Active Directory (Azure AD) ile uygulamanızın kimliğini nasıl doğrulayacağınıza karar vermelisiniz. Kullanılabilen iki ana seçenek şunlardır:

* Son kullanıcı kimlik doğrulaması 
* Hizmetten hizmete kimlik doğrulaması (Bu makale) 

Her iki seçenek de, Data Lake Storage 1. yapılan her bir isteğe bağlı olan bir OAuth 2,0 belirteciyle birlikte sağlanmış olur.

Bu makale, **hizmetten hizmete kimlik doğrulaması için bir Azure AD Web uygulaması**oluşturma hakkında bilgi sağlar. Son Kullanıcı kimlik doğrulaması için Azure AD uygulama yapılandırması yönergeleri için [Azure Active Directory kullanarak Data Lake Storage 1. Ile Son Kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)' na bakın.

## <a name="prerequisites"></a>Ön koşullar
* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>1. Adım: Active Directory Web uygulaması oluşturma

Azure Active Directory kullanarak Azure Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması için bir Azure AD Web uygulaması oluşturun ve yapılandırın. Yönergeler için bkz. [Azure AD uygulaması oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).

Önceki bağlantıdaki yönergeleri takip ederken, aşağıdaki ekran görüntüsünde gösterildiği gibi uygulama türü için **Web uygulaması/API** ' yi seçtiğinizden emin olun:

![Web uygulaması oluşturma](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "Web uygulaması oluşturma")

## <a name="step-2-get-application-id-authentication-key-and-tenant-id"></a>2. Adım: uygulama KIMLIĞI, kimlik doğrulama anahtarı ve kiracı KIMLIĞI al
Programlı olarak oturum açtığınızda, uygulamanız için KIMLIĞE ihtiyacınız vardır. Uygulama kendi kimlik bilgileri altında çalışıyorsa, bir kimlik doğrulama anahtarına de ihtiyacınız vardır.

* Uygulamanız için uygulama KIMLIĞI ve kimlik doğrulama anahtarı (istemci gizli anahtarı da denir) alma hakkında yönergeler için bkz. [uygulama kimliği ve kimlik doğrulama anahtarı alma](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).

* Kiracı KIMLIĞINI alma hakkında yönergeler için bkz. [KIRACı kimliğini alma](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).

## <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-storage-gen1-account-file-or-folder"></a>3. Adım: Azure AD uygulamasını Azure Data Lake Storage 1. hesap dosyasına veya klasörüne atama


1. [Azure Portal](https://portal.azure.com)oturum açın. Daha önce oluşturduğunuz Azure Active Directory uygulamayla ilişkilendirmek istediğiniz Data Lake Storage 1. hesabını açın.
2. Data Lake Storage 1. hesabı dikey penceresinde **Veri Gezgini**' e tıklayın.
   
    ![Data Lake Storage 1. hesapta dizin oluşturma](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "Data Lake hesapta dizin oluşturma")
3. **Veri Gezgini** dikey penceresinde, Azure AD uygulamasına erişim sağlamak istediğiniz dosya veya klasöre tıklayın ve ardından **erişim**' e tıklayın. Bir dosyaya erişimi yapılandırmak için **dosya önizleme** dikey penceresinde **erişim** ' e tıklamanız gerekir.
   
    ![Data Lake dosya sisteminde ACL 'Leri ayarla](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "Data Lake dosya sisteminde ACL 'Leri ayarla")
4. **Erişim** dikey penceresi, köke zaten atanmış standart erişimi ve özel erişimi listeler. Özel düzey ACL 'Ler eklemek için **Ekle** simgesine tıklayın.
   
    ![Standart ve özel erişimi listeleyin](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "Standart ve özel erişimi listeleyin")
5. **Özel erişim Ekle** dikey penceresini açmak için **Ekle** simgesine tıklayın. Bu dikey pencerede **Kullanıcı veya Grup Seç**' e tıklayın ve ardından **Kullanıcı veya grup** dikey penceresinde, daha önce oluşturduğunuz Azure Active Directory uygulamayı arayın. Arama yapılacak çok sayıda grubunuz varsa, Grup adını filtrelemek için üstteki metin kutusunu kullanın. Eklemek istediğiniz gruba tıklayın ve ardından **Seç**' e tıklayın.
   
    ![Grup Ekle](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "Grup Ekle")
6. **Izinleri Seç**' e tıklayın, izinleri seçin ve izinleri varsayılan bir ACL, erişim ACL 'si veya her ikisi olarak atamak isteyip istemediğiniz. **Tamam**'a tıklayın.
   
    ![Gruba izin atama](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "Gruba izin atama")
   
    Data Lake Storage 1. izinler ve varsayılan/erişim ACL 'Leri hakkında daha fazla bilgi için bkz. [Data Lake Storage 1. Access Control](data-lake-store-access-control.md).
7. **Özel erişim Ekle** dikey penceresinde **Tamam**' a tıklayın. Yeni eklenen gruplar, ilişkili izinlerle birlikte **erişim** dikey penceresinde listelenir.
   
    ![Gruba izin atama](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "Gruba izin atama")

> [!NOTE]
> Azure Active Directory uygulamanızı belirli bir klasöre kısıtlamayı planlıyorsanız, .NET SDK aracılığıyla dosya oluşturma erişimini etkinleştirmek için köke aynı Azure Active Directory uygulaması **yürütme** iznini de vermeniz gerekir.

> [!NOTE]
> Data Lake Storage 1. bir hesap oluşturmak için SDK 'Ları kullanmak istiyorsanız, Azure AD Web uygulamasını Data Lake Storage 1. hesabını oluşturduğunuz kaynak grubuna rol olarak atamanız gerekir.
> 
>

## <a name="step-4-get-the-oauth-20-token-endpoint-only-for-java-based-applications"></a>4. Adım: OAuth 2,0 belirteç uç noktasını edinme (yalnızca Java tabanlı uygulamalar için)

1. [Azure Portal](https://portal.azure.com) oturum açın ve sol bölmeden Active Directory ' a tıklayın.

2. Sol bölmeden **uygulama kayıtları**' ye tıklayın.

3. Uygulama kayıtları dikey penceresinin en üstünden **uç noktalar**' a tıklayın.

    ![OAuth belirteci uç noktası](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint.png "OAuth belirteci uç noktası")

4. Uç noktalar listesinden OAuth 2,0 belirteç uç noktasını kopyalayın.

    ![OAuth belirteci uç noktası](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint-1.png "OAuth belirteci uç noktası")   

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir Azure AD Web uygulaması oluşturdunuz ve .NET SDK, Java, Python, REST API vb. kullanarak yazdığınız istemci uygulamalarınızda ihtiyacınız olan bilgileri topladı. Artık Data Lake Storage 1. kimlik doğrulaması yapmak için Azure AD yerel uygulamasının nasıl kullanılacağına ve ardından mağaza üzerinde başka işlemler gerçekleştirmeye yönelik aşağıdaki makalelere geçebilirsiniz.

* [Java kullanarak Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-java.md)
* [.NET SDK kullanarak Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-net-sdk.md)
* [Python kullanarak Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-python.md)
* [REST API kullanarak Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-rest-api.md)


