---
title: 'Hizmete kimlik doğrulama: Azure Active Directory ile Azure Veri Gölü Depolama Gen1 | Microsoft Dokümanlar'
description: Azure Active Directory'yi kullanarak Azure Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması nasıl elde edilenöğrenin
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 3fbf2f2540e8f1ca84aad2759b9a1fc790e4065d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66241370"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Azure Etkin Dizini kullanarak Azure Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)
> * [Hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)
> 
>  

Azure Veri Gölü Depolama Gen1 kimlik doğrulaması için Azure Etkin Dizini kullanır. Veri Gölü Depolama Gen1 ile çalışan bir uygulama yazmadan önce, Azure Etkin Dizini (Azure AD) ile uygulamanızın kimliğini nasıl doğruladığını belirlemeniz gerekir. Mevcut iki ana seçenek şunlardır:

* Son kullanıcı kimlik doğrulaması 
* Hizmete hizmet kimlik doğrulaması (bu makale) 

Her iki seçenek de uygulamanızın Veri Gölü Depolama Gen1'e yapılan her isteğe iliştirilen bir OAuth 2.0 belirteci yle birlikte sunulmasıyla sonuçlanır.

Bu makalede, **hizmete kimlik doğrulaması için**bir Azure AD web uygulamasının nasıl oluşturulacağı anlatılıyor. Son kullanıcı kimlik doğrulaması için Azure AD uygulama yapılandırması ile ilgili talimatlar için, [Azure Etkin Dizini'ni kullanarak Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması'na](data-lake-store-end-user-authenticate-using-active-directory.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>Adım 1: Active Directory web uygulaması oluşturma

Azure Active Directory'yi kullanarak Azure Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması için bir Azure AD web uygulaması oluşturun ve yapılandırma. Yönergeler için [bkz.](../active-directory/develop/howto-create-service-principal-portal.md)

Yukarıdaki bağlantıdaki yönergeleri takip ederken, aşağıdaki ekran görüntüsünde gösterildiği gibi, uygulama türü için **Web App / API** seçtiğinizden emin olun:

![Web uygulaması oluşturma](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "Web uygulaması oluşturma")

## <a name="step-2-get-application-id-authentication-key-and-tenant-id"></a>Adım 2: Uygulama kimliği, kimlik doğrulama anahtarı ve kiracı kimliği alma
Programlı olarak oturum açarken, başvurunuz için kimlik gerekir. Uygulama kendi kimlik bilgileri altında çalışıyorsa, kimlik doğrulama anahtarıda da ihtiyacınız vardır.

* Uygulamanız için uygulama kimliği ve kimlik doğrulama anahtarının (istemci sırrı olarak da adlandırılır) nasıl alınacağına ilişkin talimatlar için başvuru [kimliğini ve kimlik doğrulama anahtarını al'a](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in)bakın.

* Kiracı kimliğini niçin alacağına ilişkin talimatlar için [bkz.](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in)

## <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-storage-gen1-account-file-or-folder"></a>Adım 3: Azure AD uygulamasını Azure Veri Gölü Depolama Gen1 hesap dosyasına veya klasörüne atama


1. [Azure portalında](https://portal.azure.com)oturum açın. Daha önce oluşturduğunuz Azure Etkin Dizin uygulamasıyla ilişkilendirmek istediğiniz Veri Gölü Depolama Gen1 hesabını açın.
2. Veri Gölü Depolama Gen1 hesap bıçak, **Veri Gezgini'ni**tıklatın.
   
    ![Veri Gölü Depolama Gen1 hesabında dizin oluşturma](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "Veri Gölü hesabında dizin oluşturma")
3. Veri **Gezgini** bısınız' da, Azure AD uygulamasına erişim sağlamak istediğiniz dosyaveya klasörü tıklatın ve ardından **Access'i**tıklatın. Bir dosyaya erişimi yapılandırmak için Dosya **Önizleme** çubuğundan **Eriş'i** tıklatmanız gerekir.
   
    ![Data Lake dosya sisteminde Aç'lar ayarlama](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "Data Lake dosya sisteminde Aç'lar ayarlama")
4. **Access** blade, köke zaten atanmış olan standart erişimi ve özel erişimi listeler. Özel düzeyde AK eklemek için **Ekle** simgesini tıklatın.
   
    ![Standart ve özel erişimi listele](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "Standart ve özel erişimi listele")
5. **Özel Erişim Ekle** baçı açmak için **Ekle** simgesini tıklatın. Bu bıçakta, **Kullanıcı veya Grubu Seç'i**ve ardından Kullanıcı veya **Grup** seç'i tıklatın ve daha önce oluşturduğunuz Azure Etkin Dizin uygulamasını arayın. Arama yapacak çok sayıda grubunüz varsa, grup adına filtre uygulayabilmek için üstteki metin kutusunu kullanın. Eklemek istediğiniz grubu tıklatın ve sonra **Seç'i**tıklatın.
   
    ![Grup ekleme](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "Grup ekleme")
6. **İzinleri Seçin'i**tıklatın, izinleri seçin ve izinleri varsayılan ACL olarak atamak, ACL'ye veya her ikisine erişmek isteyip istemediğiniiz. **Tamam**'a tıklayın.
   
    ![Gruplandırmak için izin atama](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "Gruplandırmak için izin atama")
   
    Veri Gölü Depolama Gen1 ve Varsayılan/Erişim Aç'ları'ndaki izinler hakkında daha fazla bilgi için Veri [Gölü Depolama Gen1'de Erişim Denetimi'ne](data-lake-store-access-control.md)bakın.
7. Özel **Erişim Ekle** çubuğunda **Tamam'ı**tıklatın. İlişkili izinlere sahip yeni eklenen grup **Access** blade'inde listelenir.
   
    ![Gruplandırmak için izin atama](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "Gruplandırmak için izin atama")

> [!NOTE]
> Azure Active Directory uygulamanızı belirli bir klasörle sınırlamayı planlıyorsanız, .NET SDK üzerinden dosya oluşturma erişimini etkinleştirmek için aynı Azure Active dizin uygulaması **Execute** iznini köke vermeniz gerekir.

> [!NOTE]
> Bir Veri Gölü Depolama Gen1 hesabı oluşturmak için SDK'ları kullanmak istiyorsanız, Veri Gölü Depolama Gen1 hesabını oluşturduğunuz Kaynak Grubu'na rol olarak Azure AD web uygulamasını atamanız gerekir.
> 
>

## <a name="step-4-get-the-oauth-20-token-endpoint-only-for-java-based-applications"></a>Adım 4: OAuth 2.0 belirteç bitiş noktasını alın (yalnızca Java tabanlı uygulamalar için)

1. [Azure portalında](https://portal.azure.com) oturum açın ve sol bölmeden Active Directory'yi tıklatın.

2. Sol bölmeden Uygulama **kayıtlarını**tıklatın.

3. Uygulama kayıtları bıçağının üst kısmından **Uç Noktaları'nı**tıklatın.

    ![OAuth belirteç bitiş noktası](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint.png "OAuth belirteç bitiş noktası")

4. Uç noktalar listesinden OAuth 2.0 belirteci bitiş noktasını kopyalayın.

    ![OAuth belirteç bitiş noktası](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint-1.png "OAuth belirteç bitiş noktası")   

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir Azure AD web uygulaması oluşturdunuz ve .NET SDK, Java, Python, REST API, vb. kullanarak yazarolduğunuz istemci uygulamalarınızda ihtiyacınız olan bilgileri topladınız. Şimdi, önce Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmak ve ardından mağazadaki diğer işlemleri gerçekleştirmek için Azure AD yerel uygulamasının nasıl kullanılacağı hakkında aşağıdaki makalelere geçebilirsiniz.

* [Java kullanarak Data Lake Storage Gen1 ile servise hizmet kimlik doğrulaması](data-lake-store-service-to-service-authenticate-java.md)
* [.NET SDK kullanarak Veri Gölü Depolama Gen1 ile servise doğru doğrulama](data-lake-store-service-to-service-authenticate-net-sdk.md)
* [Python kullanarak Veri Gölü Depolama Gen1 ile servise hizmet kimlik doğrulaması](data-lake-store-service-to-service-authenticate-python.md)
* [REST API kullanarak Veri Gölü Depolama Gen1 ile servise hizmet kimlik doğrulaması](data-lake-store-service-to-service-authenticate-rest-api.md)


