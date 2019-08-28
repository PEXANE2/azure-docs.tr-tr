---
title: Azure AD Graph API kullanma
description: Azure Active Directory (Azure AD) Graph API, OData REST API uç noktaları aracılığıyla Azure AD 'ye programlı erişim sağlar. Uygulamalar, dizin verileri ve nesnelerinde oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini gerçekleştirmek için Azure AD Graph API kullanabilir.
services: active-directory
documentationcenter: n/a
author: rwike77
manager: CelesteDG
editor: ''
tags: ''
ms.assetid: 9dc268a9-32e8-402c-a43f-02b183c295c5
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2019
ms.author: ryanwi
ms.reviewer: sureshja
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ee6c4630205561eb8beb19062520f8ae2a35e1b
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70073904"
---
# <a name="how-to-use-the-azure-ad-graph-api"></a>Nasıl yapılır: Azure AD Graph API’sini kullanma

> [!IMPORTANT]
> Azure Active Directory kaynaklarına erişmek için Azure AD Graph API'si yerine [Microsoft Graph](https://developer.microsoft.com/graph) kullanmanız önemle tavsiye edilir. Geliştirme çalışmalarımız şu anda Microsoft Graph üzerine yoğunlaşmıştır ve Azure AD Graph API’si için başka bir geliştirme planlanmamaktadır. Azure AD Graph API 'nin hala uygun olabileceği çok sınırlı sayıda senaryo vardır; daha fazla bilgi için bkz. [Microsoft Graph veya Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blog gönderisi ve [Azure ad Graph uygulamalarını Microsoft Graph 'ye geçirme](https://docs.microsoft.com/graph/migrate-azure-ad-graph-overview).

Azure Active Directory (Azure AD) Graph API, OData REST API uç noktaları aracılığıyla Azure AD 'ye programlı erişim sağlar. Uygulamalar, dizin verileri ve nesnelerinde oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini gerçekleştirmek için Azure AD Graph API kullanabilir. Örneğin, Azure AD Graph API kullanarak yeni bir kullanıcı oluşturabilir, kullanıcının özelliklerini görüntüleyebilir veya güncelleştirebilir, Kullanıcı parolasını değiştirebilir, rol tabanlı erişim için Grup üyeliğini denetleyebilir, devre dışı bırakabilir veya silebilirsiniz. Azure AD Graph API özellikleri ve uygulama senaryoları hakkında daha fazla bilgi için bkz. [Azure ad Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) ve [Azure AD Graph API önkoşulları](https://msdn.microsoft.com/library/hh974476.aspx). Azure AD Graph API yalnızca iş veya okul/kuruluş hesaplarıyla çalışır.

Bu makale, Azure AD Graph API için geçerlidir. Microsoft Graph API ile ilgili benzer bilgiler için, bkz. [MICROSOFT Graph API 'Sini kullanma](https://developer.microsoft.com/graph/docs/concepts/use_the_api).

## <a name="how-to-construct-a-graph-api-url"></a>Graph API URL 'SI oluşturma

Graph API, CRUD işlemleri gerçekleştirmek istediğiniz dizin verilerine ve nesnelere (diğer bir deyişle, kaynaklarda veya varlıklarda) erişmek için, açık veri (OData) protokolüne göre URL 'Leri kullanabilirsiniz. Graph API ' de kullanılan URL 'Ler dört ana bölümden oluşur: hizmet kökü, kiracı tanımlayıcısı, kaynak yolu ve sorgu dizesi seçenekleri: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Aşağıdaki URL 'nin örneğini alın: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

* **Hizmet kökü**: Azure AD Graph API 'de hizmet kökü her zaman https://graph.windows.net olur.
* **Kiracı tanımlayıcısı**: Bu bölüm, önceki örnekte contoso.com, doğrulanmış (kayıtlı) bir etki alanı adı olabilir. Kiracı nesne KIMLIĞI veya "myorgani" veya "Me" diğer adı da olabilir. Daha fazla bilgi için bkz. [Azure AD Graph API 'de varlıkları ve Işlemleri adresleme](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview).
* **Kaynak yolu**: Bir URL 'nin bu bölümü, (kullanıcılar, gruplar, belirli bir kullanıcı veya belirli bir grup vb.) ile etkileşim edilecek kaynağı tanımlar. Yukarıdaki örnekte, bu kaynak kümesini ele almak için en üst düzey "gruplar" dır. Belirli bir varlığa de (örneğin, "Users/{ObjectID}" veya "Users/userPrincipalName") de bağlayabilirsiniz.
* **Sorgu parametreleri**: Soru işareti (?), sorgu parametreleri bölümünden kaynak yolu bölümünü ayırır. Azure AD Graph API tüm isteklerde "api-Version" sorgu parametresi gereklidir. Azure AD Graph API, aşağıdaki OData sorgu seçeneklerini de destekler: **$Filter**, **$OrderBy**, **$Expand**, **$top**ve **$Format**. Şu sorgu seçenekleri şu anda desteklenmiyor: **$Count**, **$inlinecount**ve **$Skip**. Daha fazla bilgi için bkz. [Azure AD Graph API 'de desteklenen sorgular, filtreler ve sayfalama seçenekleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Graph API sürümleri

Bir Graph API isteği için sürümü "API-Version" sorgu parametresinde belirtirsiniz. Sürüm 1,5 ve sonrası için sayısal bir sürüm değeri kullanırsınız; api-Version = 1.6. Önceki sürümler için YYYY-AA-GG biçiminde uygun bir tarih dizesi kullanırsınız. Örneğin, api-Version = 2013-11-08. Önizleme özellikleri için "Beta" dizesini kullanın; Örneğin, api-version = beta. Graph API sürümleri arasındaki farklılıklar hakkında daha fazla bilgi için bkz. [Azure AD Graph API sürümü oluşturma](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Graph API meta veri

Azure AD Graph API meta veri dosyasını geri döndürmek için, URL 'deki kiracı tanımlayıcısından sonra "$metadata" segmentini ekleyin örneğin, aşağıdaki URL bir demo şirketi için meta verileri döndürür: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Meta verileri görmek için, bu URL 'YI bir Web tarayıcısının adres çubuğuna girebilirsiniz. Döndürülen CSDL meta verileri belgesi, varlıkları ve karmaşık türleri, özelliklerini ve istediğiniz Graph API sürümü tarafından kullanıma sunulan işlevleri ve eylemleri açıklar. Api sürümü parametresini atlama, en son sürüm için meta verileri döndürür.

## <a name="common-queries"></a>Ortak sorgular

[Azure ad Graph API ortak sorgular](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) , dizininizdeki işlemleri gerçekleştirmek için dizininizdeki en üst düzey kaynaklara ve sorgulara erişmek üzere kullanılabilecek sorgular da dahil olmak üzere Azure AD Graph ile kullanılabilen ortak sorguları listeler.

Örneğin, `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` contoso.com dizini için şirket bilgilerini döndürür.

Veya `https://graph.windows.net/contoso.com/users?api-version=1.6` contoso.com dizinindeki tüm kullanıcı nesnelerini listeler.

## <a name="using-the-azure-ad-graph-explorer"></a>Azure AD Graph Gezginini Kullanma
Uygulamanızı oluştururken dizin verilerini sorgulamak için Azure AD Graph API için Azure AD grafik Gezgini ' ni kullanabilirsiniz.

Aşağıdaki ekran görüntüsü, Azure AD Graph Explorer 'a gitmeniz, oturum açmanız ve oturum açan kullanıcının dizinindeki tüm kullanıcıları görüntülemek için girdiğinizde `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` göreceğiniz çıktıdır:

![Azure AD Graph API Gezgini 'nde örnek çıkış](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Azure AD Graph Gezginini yükleyin**: Aracı yüklemek için öğesine [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/)gidin. Azure AD Graph Explorer 'ı kiracınıza karşı çalıştırmak için **oturum aç** ' a tıklayın ve Azure AD hesabı kimlik bilgilerinizle oturum açın. Azure AD Graph Explorer 'ı kendi kiracınızda çalıştırırsanız, sizin ya da yöneticinizin oturum açma sırasında onay olması gerekir. Office 365 aboneliğiniz varsa, otomatik olarak bir Azure AD kiracınız vardır. Office 365 ' te oturum açmak için kullandığınız kimlik bilgileri, aslında Azure AD hesapları ve bu kimlik bilgilerini Azure AD Graph Explorer ile kullanabilirsiniz.

**Bir sorgu çalıştırın**: Bir sorgu çalıştırmak için, istek metin kutusuna sorgunuzu yazın ve **Al** ' a tıklayın veya **ENTER** tuşuna tıklayın. Sonuçlar yanıt kutusunda görüntülenir. Örneğin, `https://graph.windows.net/myorganization/groups?api-version=1.6` oturum açan kullanıcının dizinindeki tüm Grup nesnelerini listeler.

Azure AD Graph Explorer 'ın aşağıdaki özelliklerini ve sınırlamalarını aklınızda edin:

* Kaynak kümelerinde otomatik tamamlama özelliği. Bu işlevi görmek için istek metin kutusuna (Şirket URL 'sinin göründüğü yer) tıklayın. Açılan listeden bir kaynak kümesi seçebilirsiniz.
* İstek geçmişi.
* "Me" ve "myorganleştirme" adresleme diğer adlarını destekler. Örneğin, oturum açmış kullanıcının Kullanıcı `https://graph.windows.net/me?api-version=1.6` nesnesini döndürmek veya `https://graph.windows.net/myorganization/users?api-version=1.6` oturum açan kullanıcının dizinindeki tüm kullanıcıları döndürmek için kullanabilirsiniz.
* `POST`, Ve`GET`kullanarak kendi dizininize karşı tam CRUD işlemlerini destekler. `DELETE` `PATCH`
* Yanıt üst bilgileri bölümü. Bu bölüm, sorguları çalıştırırken oluşan sorunları gidermeye yardımcı olmak için kullanılabilir.
* Genişletme ve daraltma özelliklerine sahip yanıt için bir JSON Görüntüleyicisi.
* Küçük resim fotoğrafı görüntüleme veya karşıya yükleme desteği yoktur.

## <a name="using-fiddler-to-write-to-the-directory"></a>Dizine yazmak için Fiddler kullanma

Bu hızlı başlangıç kılavuzunun amaçları doğrultusunda, Azure AD dizininize yönelik ' yazma ' işlemlerini gerçekleştirmek için Fiddler Web hata ayıklayıcısını kullanabilirsiniz. Örneğin, bir kullanıcının profil fotoğrafını alabilir ve yükleyebilirsiniz (Azure AD Graph Explorer ile mümkün değildir). Daha fazla bilgi edinmek ve Fiddler 'ı yüklemek için [https://www.telerik.com/fiddler](https://www.telerik.com/fiddler)bkz.

Aşağıdaki örnekte, Azure AD dizininizde yeni bir ' MyTestGroup ' güvenlik grubu oluşturmak için Fiddler Web hata ayıklayıcısını kullanacaksınız.

**Erişim belirteci edinin**: Azure AD grafiğine erişmek için, istemcilerin önce Azure AD 'de başarıyla kimlik doğrulaması yapması gerekir. Daha fazla bilgi için bkz. [Azure AD Için kimlik doğrulama senaryoları](authentication-scenarios.md).

**Bir sorgu oluşturun ve çalıştırın**: Aşağıdaki adımları tamamlayın:

1. Fiddler Web hata ayıklayıcısını açın ve **besteci** sekmesine geçin.
2. Yeni bir güvenlik grubu oluşturmak istediğiniz için, açılır menüden HTTP yöntemi olarak **Postala** ' yı seçin. Bir grup nesnesindeki işlemler ve izinler hakkında daha fazla bilgi için bkz. [](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#group-entity) [Azure AD Graph içindeki Grup REST API başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. **Gönderi**' ın yanındaki alana, AŞAĞıDAKI istek URL 'sini yazın: `https://graph.windows.net/{mytenantdomain}/groups?api-version=1.6`.
   
   > [!NOTE]
   > {Mytenantdomain} yerine kendi Azure AD dizininizin etki alanı adını belirtmelisiniz.

4. Aşağı çekme sonrası alanındaki alanda, aşağıdaki HTTP üst bilgisini yazın:
   
    ```
   Host: graph.windows.net
   Authorization: Bearer <your access token>
   Content-Type: application/json
   ```
   
   > [!NOTE]
   > Erişim belirtecinizi&gt; Azure AD dizininiz için erişim belirteci ile değiştirin. &lt;

5. **İstek gövdesi** ALANıNA aşağıdaki JSON yazın:
   
    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
   ```
   
    Grup oluşturma hakkında daha fazla bilgi için bkz. [Grup oluşturma](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Grafik tarafından sunulan Azure AD varlıkları ve türleri hakkında daha fazla bilgi ve grafikle gerçekleştirilebilecek işlemler hakkında bilgi için bkz. [Azure AD Graph REST API başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) hakkında daha fazla bilgi edinin
* [Azure AD Graph API izin kapsamları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) hakkında daha fazla bilgi edinin
