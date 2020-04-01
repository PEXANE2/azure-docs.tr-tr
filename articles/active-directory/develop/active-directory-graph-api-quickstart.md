---
title: Azure AD Grafiği API'si nasıl kullanılır?
description: Azure Etkin Dizin (Azure AD) Grafik API'si, OData REST API uç noktaları aracılığıyla Azure AD'ye programlı erişim sağlar. Uygulamalar, dizin verileri ve nesneler üzerinde oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini gerçekleştirmek için Azure AD Graph API'sını kullanabilir.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 9dc268a9-32e8-402c-a43f-02b183c295c5
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/28/2019
ms.author: ryanwi
ms.reviewer: sureshja
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 701aadcbfe03c82f6a4c341ef9698f8a1bf1a347
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80476595"
---
# <a name="how-to-use-the-azure-ad-graph-api"></a>Nasıl kullanılır: Azure AD Grafiği API'sini kullanın

> [!IMPORTANT]
> Azure Active Directory (Azure AD) kaynaklarına erişmek için Azure AD Graph API yerine [Microsoft Graph'ı](https://developer.microsoft.com/graph) kullanmanızı şiddetle öneririz. Geliştirme çalışmalarımız şu anda Microsoft Graph üzerine yoğunlaşmıştır ve Azure AD Graph API’si için başka bir geliştirme planlanmamaktadır. Azure AD Grafiği API'sinin hala uygun olabileceği çok sınırlı sayıda senaryo vardır; daha fazla bilgi için [Microsoft Graph veya Azure AD Graph](https://developer.microsoft.com/office/blogs/microsoft-graph-or-azure-ad-graph/) blog gönderisine bakın ve Azure AD Grafiği uygulamalarını Microsoft [Graph'a geçirin.](https://docs.microsoft.com/graph/migrate-azure-ad-graph-overview)

Azure AD Grafiği API'si, OData REST API uç noktaları aracılığıyla Azure AD'ye programlı erişim sağlar. Uygulamalar, dizin verileri ve nesneler üzerinde oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini gerçekleştirmek için Azure AD Graph API'sını kullanabilir. Örneğin, yeni bir kullanıcı oluşturmak, kullanıcının özelliklerini görüntülemek veya güncellemek, kullanıcının parolasını değiştirmek, rol tabanlı erişim için grup üyeliğini denetlemek, kullanıcıyı devre dışı kalımveya silmek için Azure AD Graph API'yi kullanabilirsiniz. Azure AD Grafiği API özellikleri ve uygulama senaryoları hakkında daha fazla bilgi için [Azure AD Grafiği API'si](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) ve [Azure AD Grafiği API ön koşulları'na](https://msdn.microsoft.com/library/hh974476.aspx)bakın. Azure AD Graph API yalnızca iş veya okul/kuruluş hesaplarıyla çalışır.

Bu makale, Azure AD Grafiği API'si için geçerlidir. Microsoft Graph API ile ilgili benzer bilgiler için [bkz.](https://developer.microsoft.com/graph/docs/concepts/use_the_api)

## <a name="how-to-construct-a-graph-api-url"></a>Grafik API URL'si oluşturma

Grafik API'sinde, CRUD işlemlerini gerçekleştirmek istediğiniz dizin verilerine ve nesnelere (diğer bir deyişle, kaynaklar veya varlıklar) erişmek için, Açık Veri (OData) Protokolü'ne dayalı URL'ler kullanabilirsiniz. Grafik API'sinde kullanılan URL'ler dört ana bölümden oluşur: hizmet kökü, kiracı tanımlayıcısı, `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`kaynak yolu ve sorgu dize seçenekleri: . Aşağıdaki URL örneğini ele `https://graph.windows.net/contoso.com/groups?api-version=1.6`alalım: .

* **Hizmet Kökü**: Azure AD Grafiği API'sinde hizmet kökü her zaman https://graph.windows.net.
* **Kiracı tanımlayıcısı**: Bu bölüm, bir önceki örnekte, contoso.com doğrulanmış (kayıtlı) alan adı olabilir. Ayrıca bir kiracı nesne kimliği veya "myorganization" veya "ben" takma olabilir. Daha fazla bilgi için Azure [AD Grafiği API'deki varlıkları ve işlemleri ele alma](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)bilgisine bakın.
* **Kaynak yolu**: URL'nin bu bölümü, etkileşimde alınacak kaynağı (kullanıcılar, gruplar, belirli bir kullanıcı veya belirli bir grup, vb.) tanımlar Yukarıdaki örnekte, bu kaynak kümesini ele almak için en üst düzey "gruplar" yer alıyor. "users/{objectId}" veya "users/userPrincipalName" gibi belirli bir varlığa da hitap edebilirsiniz.
* **Sorgu parametreleri**: Soru işareti (?) kaynak yolu bölümünü sorgu parametreleri bölümünden ayırır. Azure AD Grafiği API'deki tüm isteklerde "api sürümü" sorgu parametresi gereklidir. Azure AD Grafiği API'si ayrıca aşağıdaki OData sorgu seçeneklerini de destekler: **$filter**, **$orderby,** **$expand,** **$top**, ve **$format.** Aşağıdaki sorgu seçenekleri şu anda desteklenmez: **$count,** **$inlinecount**ve **$skip.** Daha fazla bilgi için Azure [AD Grafiği API'sinde Desteklenen Sorgular, Filtreler ve Sayfalama Seçenekleri'ne](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options)bakın.

## <a name="graph-api-versions"></a>Grafik API sürümleri

"Api sürümü" sorgu parametresinde Grafik API isteğinin sürümünü belirtirsiniz. Sürüm 1.5 ve sonrası için sayısal sürüm değeri kullanırsınız; api-version=1.6. Önceki sürümler için, YYYY-MM-DD biçimine uyan bir tarih dizesini kullanırsınız; örneğin, api-version=2013-11-08. Önizleme özellikleri için "beta" dizesini kullanın; örneğin, api-version=beta. Grafik API sürümleri arasındaki farklar hakkında daha fazla bilgi için [Azure AD Grafiği API sürümüne](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning)bakın.

## <a name="graph-api-metadata"></a>Grafik API meta verileri

Azure AD Graph API meta veri dosyasını döndürmek için, URL'deki kiracı tanımlayıcıdan sonra "$metadata" kesimini ekleyin Örneğin, `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`aşağıdaki URL bir demo şirketi için meta verileri döndürür: . Meta verileri görmek için bu URL'yi bir web tarayıcısının adres çubuğuna girebilirsiniz. Döndürülen CSDL meta veri belgesi, istediğiniz Grafik API sürümütarafından açığa çıkarılan varlıkları ve karmaşık türleri, özelliklerini ve işlevleri ve eylemleri açıklar. Api sürüm parametresi atlayarak en son sürümü için meta verileri döndürür.

## <a name="common-queries"></a>Sık sorulan sorular

[Azure AD Grafiği API yaygın sorguları,](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) dizininizdeki üst düzey kaynaklara erişmek için kullanılabilecek sorgular ve dizininizde işlemleri gerçekleştirmek için sorgular da dahil olmak üzere Azure REKLAM Grafiği ile kullanılabilecek yaygın sorguları listeler.

Örneğin, `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` şirket bilgilerini dizin contoso.com için döndürür.

Veya `https://graph.windows.net/contoso.com/users?api-version=1.6` contoso.com dizindeki tüm kullanıcı nesnelerini listeler.

## <a name="using-the-azure-ad-graph-explorer"></a>Azure AD Grafiği Gezgini'ni kullanma
Uygulamanızı oluştururken dizin verilerini sorgulamak için Azure AD Graph API'si için Azure AD Graph Explorer'ı kullanabilirsiniz.

Aşağıdaki ekran görüntüsü, Azure AD Graph Explorer'da gezinmek, oturum açın ve `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` oturum açmış kullanıcı nın dizinindeki tüm kullanıcıları görüntülemek için girseniz göreceğiniz çıktıdır:

![Azure AD Grafiği API Gezgini'nde örnek çıktı](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Azure AD Grafiği Gezgini'ni yükleyin** [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/): Aracı yüklemek için . Azure AD Grafik Gezgini'ni kiracınıza karşı çalıştırmak için Azure REKLAM hesap kimlik bilgilerinizle **Oturum Açma** ve oturum açma'yı tıklatın. Azure AD Graph Explorer'ı kendi kiracınıza karşı çalıştırınsa, oturum açma sırasında sizin veya yöneticinizin onay alması gerekir. Office 365 aboneliğiniz varsa, otomatik olarak bir Azure AD kiracınız olur. Office 365'te oturum açtırdığınız kimlik bilgileri aslında Azure AD hesaplarıdır ve bu kimlik bilgilerini Azure AD Graph Explorer ile kullanabilirsiniz.

**Sorgu çalıştır**: Sorguyu çalıştırmak için, sorgunuzu istek metin kutusuna yazın ve **GET** tuşuna basın veya **enter** tuşuna tıklayın. Sonuçlar yanıt kutusunda görüntülenir. Örneğin, `https://graph.windows.net/myorganization/groups?api-version=1.6` oturum açmış kullanıcı dizinindeki tüm grup nesnelerini listeler.

Azure AD Grafiği Gezgini'nin aşağıdaki özelliklerine ve sınırlamalarını not edin:

* Kaynak kümelerinde otomatik tamamlama özelliği. Bu işlevselliği görmek için istek metin kutusunu (şirket URL'sinin göründüğü yer) tıklatın. Açılan listeden bir kaynak kümesi seçebilirsiniz.
* Geçmiş iste.
* Takma adlarını ele alan "ben" ve "organizasyonum"u destekler. Örneğin, oturum açmış `https://graph.windows.net/me?api-version=1.6` kullanıcının kullanıcı nesnesini döndürmek veya `https://graph.windows.net/myorganization/users?api-version=1.6` oturum açmış kullanıcı dizinindeki tüm kullanıcıları döndürmek için kullanabilirsiniz.
* Kendi dizininize karşı tam CRUD `GET` `PATCH` operasyonlarını `DELETE`destekler, `POST`ve .
* Yanıt üstbilgi bölümü. Bu bölüm, sorguları çalıştırırken oluşan sorunları gidermeye yardımcı olmak için kullanılabilir.
* Genişletme ve daraltma özellikleriyle yanıt için bir JSON görüntüleyici.
* Küçük resim fotoğrafını görüntülemek veya yüklemek için destek yok.

## <a name="using-fiddler-to-write-to-the-directory"></a>Dizin yazmak için Fiddler kullanma

Bu Quickstart kılavuzunun amaçları doğrultusunda, Azure AD dizininize karşı 'yazma' işlemleri gerçekleştirme alıştırması yapmak için Fiddler Web Hata Ayıklayıcı'yı kullanabilirsiniz. Örneğin, bir kullanıcının profil fotoğrafını alabilir ve yükleyebilirsiniz (Azure AD Graph Explorer ile mümkün değildir). Daha fazla bilgi ve Fiddler [https://www.telerik.com/fiddler](https://www.telerik.com/fiddler)yüklemek için bkz.

Aşağıdaki örnekte, Azure AD dizininizde yeni bir güvenlik grubu 'MyTestGroup' oluşturmak için Fiddler Web Hata Ayıklayıcı'yı kullanırsınız.

**Erişim belirteci edinin**: Azure AD Grafiği'ne erişmek için müşterilerin önce Azure AD'ye başarılı bir şekilde kimlik doğrulaması yapılması gerekir. Daha fazla bilgi için [Azure AD için Kimlik Doğrulama senaryolarına](authentication-scenarios.md)bakın.

**Sorgu oluşturma ve çalıştırma**: Aşağıdaki adımları tamamlayın:

1. Fiddler Web Debugger'ı açın ve **Besteci** sekmesine geçin.
2. Yeni bir güvenlik grubu oluşturmak istediğinizden, açılan menüden HTTP yöntemi olarak **Gönder'i** seçin. Bir grup nesnesi üzerindeki işlemler ve izinler hakkında daha fazla bilgi için Azure [AD Graph REST API başvurusundaki](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) [Grup'a](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#group-entity) bakın.
3. **Post'un**yanındaki alana, aşağıdaki istek URL'sini yazın: `https://graph.windows.net/{mytenantdomain}/groups?api-version=1.6`.
   
   > [!NOTE]
   > {mytenantdomain} yerine kendi Azure AD dizininizin etki alanı adı vermelisiniz.

4. Post pull-down'Un hemen altındaki alana aşağıdaki HTTP üstbilgisini yazın:
   
    ```
   Host: graph.windows.net
   Authorization: Bearer <your access token>
   Content-Type: application/json
   ```
   
   > [!NOTE]
   > Erişim &lt;belirtecinizi&gt; Azure REKLAM dizininiz için erişim belirteciyle değiştirin.

5. İstek **gövde** alanına aşağıdaki JSON'u yazın:
   
    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
   ```
   
    Grup oluşturma hakkında daha fazla bilgi için [bkz.](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup)

Grafik tarafından ortaya çıkarılan Azure AD varlıkları ve türleri hakkında daha fazla bilgi ve Grafik ile üzerlerinde gerçekleştirilebilecek işlemler hakkında bilgi için Azure [AD Graph REST API başvurusuna](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD [Grafiği API'si](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) hakkında daha fazla bilgi edinin
* [Azure AD Grafiği API izni kapsamları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) hakkında daha fazla bilgi edinin
