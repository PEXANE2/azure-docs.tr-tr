---
title: Etkin Dizin kullanarak sonuçları kırpmak için güvenlik filtreleri
titleSuffix: Azure Cognitive Search
description: Güvenlik filtrelerini ve Azure Etkin Dizin (AAD) kimliklerini kullanarak Azure Bilişsel Arama içeriğinde erişim denetimi.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 01280b6ee9dda15af3c0fc707a385501580c624c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72794313"
---
# <a name="security-filters-for-trimming-azure-cognitive-search-results-using-active-directory-identities"></a>Etkin Dizin kimliklerini kullanarak Azure Bilişsel Arama sonuçlarını kırpmak için güvenlik filtreleri

Bu makalede, kullanıcı grubu üyeliğine dayalı arama sonuçlarını kırpmak için Azure Bilişsel Arama'daki filtrelerle birlikte Azure Etkin Dizin (AAD) güvenlik kimliklerinin nasıl kullanılacağı gösterilmelidir.

Bu makale aşağıdaki görevleri kapsar:
> [!div class="checklist"]
> - AAD grupları ve kullanıcıları oluşturma
> - Kullanıcıyı oluşturduğunuz grupla ilişkilendirme
> - Önbellek yeni gruplar
> - İlişkili gruplarla dizin belgeleri
> - Grup tanımlayıcıları filtresiyle arama isteği verme
> 
> [!NOTE]
> Bu makaledeki örnek kod parçacıkları C# olarak yazılmıştır. Tam kaynak kodunu [GitHub](https://aka.ms/search-dotnet-howto)'da bulabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

Azure Bilişsel Arama'daki dizininizin, belgeye erişimi okuyan grup kimliklerinin listesini depolamak için bir [güvenlik alanına](search-security-trimming-for-azure-search.md) sahip olması gerekir. Bu kullanım örneği, bir ekürilebilir öğe (bireyin üniversite başvurusu gibi) ile bu maddeye kimlerin erişimi olduğunu belirten bir güvenlik alanı (kabul personeli) arasında bire bir yazışma olduğunu varsayar.

AAD'de kullanıcı, grup ve çağrışım oluşturmak için bu izbölümünde gerekli olan AAD yönetici izinlerine sahip olmalısınız.

Başvurunuzun, aşağıdaki yordamda açıklandığı şekilde AAD'ye de kaydedilmesi gerekir.

### <a name="register-your-application-with-aad"></a>Başvurunuzu AAD'ye kaydedin

Bu adım, kullanıcı ve grup hesaplarının oturum açmalarını kabul etmek amacıyla uygulamanızı AAD ile bütünleştirir. Kuruluşunuzdaki bir AAD yöneticisi değilseniz, aşağıdaki adımları gerçekleştirmek için [yeni bir kiracı oluşturmanız](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) gerekebilir.

1. [**Uygulama Kayıt Portalı**](https://apps.dev.microsoft.com) >  **Yakınsama uygulamasına** > gidin**Bir uygulama ekleyin**.
2. Uygulamanız için bir ad girin, ardından **Oluştur'u**tıklatın. 
3. Yeni kaydedilmiş başvurunuzu Uygulamalarım sayfasından seçin.
4. Platform > **ekle**platform **>** uygulama kayıt sayfasında, **Web API**seçin.
5. Hala uygulama kayıt sayfasında, **Microsoft Graph İzinleri** > **Ekle**> gidin.
6. İzinleri Seç'te, aşağıdaki devredilen izinleri ekleyin ve ardından **Tamam'ı**tıklatın:

   + **Directory.ReadWrite.All**
   + **Group.ReadWrite.All**
   + **Kullanıcı.ReadWrite.All**

Microsoft Graph, REST API aracılığıyla AAD'ye programlı erişimsağlayan bir API sağlar. Bu izbin kod örneği, gruplar, kullanıcılar ve ilişkilendirmeler oluşturmak için Microsoft Graph API'yi aramak için izinleri kullanır. API'ler, daha hızlı filtreleme için grup tanımlayıcılarını önbelleğe almak için de kullanılır.

## <a name="create-users-and-groups"></a>Kullanıcı ve gruplar oluşturma

Yerleşik bir uygulamaya arama ekliyorsanız, AAD'de varolan kullanıcı ve grup tanımlayıcılarınız olabilir. Bu durumda, sonraki üç adımı atlayabilirsiniz. 

Ancak, varolan kullanıcılarınız yoksa, güvenlik ilkelerini oluşturmak için Microsoft Graph API'lerini kullanabilirsiniz. Aşağıdaki kod parçacıkları, Azure Bilişsel Arama dizininizdeki güvenlik alanı için veri değerleri haline gelen tanımlayıcıların nasıl oluşturacağınızı gösterir. Varsayımsal üniversite kabul başvurumuzda, bu kabul personeli için güvenlik tanımlayıcıları olacaktır.

Kullanıcı ve grup üyeliği, özellikle büyük kuruluşlarda çok akıcı olabilir. Kullanıcı ve grup kimliklerini oluşturan kod, kuruluş üyeliğindeki değişiklikleri almak için yeterince sık çalışmalıdır. Benzer şekilde, Azure Bilişsel Arama dizininizde de izin verilen kullanıcıların ve kaynakların geçerli durumunu yansıtmak için benzer bir güncelleştirme zamanlaması gerekir.

### <a name="step-1-create-aad-group"></a>Adım 1: [AAD Grubu](https://docs.microsoft.com/graph/api/group-post-groups?view=graph-rest-1.0) Oluşturma 
```csharp
// Instantiate graph client 
GraphServiceClient graph = new GraphServiceClient(new DelegateAuthenticationProvider(...));
Group group = new Group()
{
    DisplayName = "My First Prog Group",
    SecurityEnabled = true,
    MailEnabled = false,
    MailNickname = "group1"
}; 
Group newGroup = await graph.Groups.Request().AddAsync(group);
```
   
### <a name="step-2-create-aad-user"></a>Adım 2: [AAD Kullanıcısı](https://docs.microsoft.com/graph/api/user-post-users?view=graph-rest-1.0) Oluşturma
```csharp
User user = new User()
{
    GivenName = "First User",
    Surname = "User1",
    MailNickname = "User1",
    DisplayName = "First User",
    UserPrincipalName = "User1@FirstUser.com",
    PasswordProfile = new PasswordProfile() { Password = "********" },
    AccountEnabled = true
};
User newUser = await graph.Users.Request().AddAsync(user);
```

### <a name="step-3-associate-user-and-group"></a>Adım 3: Ortak kullanıcı ve grup
```csharp
await graph.Groups[newGroup.Id].Members.References.Request().AddAsync(newUser);
```

### <a name="step-4-cache-the-groups-identifiers"></a>Adım 4: Grupları tanımlayıcıları önbelleğe alan
İsteğe bağlı olarak, ağ gecikmesini azaltmak için, bir arama isteği yayımlandığında grupların önbellekten döndürülecek ve AAD'ye bir gidiş dönüş kaydedilebilmek için kullanıcı grubu ilişkilendirmelerini önbelleğe alabilirsiniz. Birden çok kullanıcıya tek bir Http isteği göndermek ve önbelleği oluşturmak için [AAD Toplu İş API'sini](https://developer.microsoft.com/graph/docs/concepts/json_batching) kullanabilirsiniz.

Microsoft Graph, yüksek sayıdaisteği işlemek üzere tasarlanmıştır. Çok sayıda istek oluşursa, Microsoft Graph HTTP durum kodu 429 ile isteği başarısız olur. Daha fazla bilgi için [Microsoft Graph azaltma'ya](https://developer.microsoft.com/graph/docs/concepts/throttling)bakın.

## <a name="index-document-with-their-permitted-groups"></a>İzin verilen grupları içeren dizin belgesi

Azure Bilişsel Arama'daki sorgu işlemleri, Azure Bilişsel Arama dizini üzerinden yürütülür. Bu adımda, bir dizin oluşturma işlemi, güvenlik filtresi olarak kullanılan tanımlayıcılar da dahil olmak üzere aranabilir verileri bir dizin içine aktarın. 

Azure Bilişsel Arama, kullanıcı kimliklerini doğrulamaz veya bir kullanıcının görüntüleme iznine sahip olduğu içeriği belirlemek için mantık sağlamaz. Güvenlik kırpma için kullanım örneği, hassas bir belge ile arama dizinine bozulmamış olarak içe aktarılan bu belgeye erişimi olan grup tanımlayıcısı arasındaki ilişkiyi sağladığınızı varsayar. 

Varsayımsal örnekte, Azure Bilişsel Arama dizinindeki PUT isteğinin gövdesi, başvuru sahibinin üniversite kompozisyonunu veya transkriptini ve bu içeriği görüntüleme iznine sahip grup tanımlayıcısını içerir. 

Bu gözden geçirme için kod örneğinde kullanılan genel örnekte, dizin eylemi aşağıdaki gibi görünebilir:

```csharp
var actions = new IndexAction<SecuredFiles>[]
              {
                  IndexAction.Upload(
                  new SecuredFiles()
                  {
                      FileId = "1",
                      Name = "secured_file_a",
                      GroupIds = new[] { groups[0] }
                  }),
              ...
             };

var batch = IndexBatch.New(actions);

_indexClient.Documents.Index(batch);  
```

## <a name="issue-a-search-request"></a>Arama isteği verme

Güvenlik kırpma amacıyla, dizindeki güvenlik alanınızdaki değerler, arama sonuçlarına belgeleri dahil etmek veya hariç taramak için kullanılan statik değerlerdir. Örneğin, Kabul için grup tanımlayıcısı "A11B22C33D44-E55F66G77-H88I99JK" ise, dosyalanan güvenlikte bu tanımlayıcıyı içeren bir Azure Bilişsel Arama dizinindeki tüm belgeler, talep sahibine geri gönderilen arama sonuçlarına dahil edilir (veya hariç tutulur).

Arama sonuçlarında iade edilen belgeleri, isteği veren kullanıcı gruplarına göre filtrelemek için aşağıdaki adımları gözden geçirin.

### <a name="step-1-retrieve-users-group-identifiers"></a>Adım 1: Kullanıcının grup tanımlayıcılarını alma

Kullanıcıgrupları önceden önbelleğe alınmamışsa veya önbelleğin süresi dolmuşsa, [grup](https://docs.microsoft.com/graph/api/directoryobject-getmembergroups?view=graph-rest-1.0) isteğini
```csharp
private static void RefreshCacheIfRequired(string user)
{
    if (!_groupsCache.ContainsKey(user))
    {
        var groups = GetGroupIdsForUser(user).Result;
        _groupsCache[user] = groups;
    }
}

private static async Task<List<string>> GetGroupIdsForUser(string userPrincipalName)
{
    List<string> groups = new List<string>();
    var allUserGroupsRequest = graph.Users[userPrincipalName].GetMemberGroups(true).Request();

    while (allUserGroupsRequest != null) 
    {
        IDirectoryObjectGetMemberGroupsRequestBuilder allUserGroups = await allUserGroupsRequest.PostAsync();
        groups = allUserGroups.ToList();
        allUserGroupsRequest = allUserGroups.NextPageRequest;
    }
    return groups;
}
``` 

### <a name="step-2-compose-the-search-request"></a>Adım 2: Arama isteği oluşturma

Kullanıcının grup üyeliğine sahip olduğunuzu varsayarsak, arama isteğini uygun filtre değerleriyle birlikte düzenleyebilirsiniz.

```csharp
string filter = String.Format("groupIds/any(p:search.in(p, '{0}'))", string.Join(",", groups.Select(g => g.ToString())));
SearchParameters parameters = new SearchParameters()
             {
                 Filter = filter,
                 Select = new[] { "application essays" }
             };

DocumentSearchResult<SecuredFiles> results = _indexClient.Documents.Search<SecuredFiles>("*", parameters);
```
### <a name="step-3-handle-the-results"></a>Adım 3: Sonuçları işleme

Yanıt, kullanıcının görüntüleme izni ne sahip olduğundan oluşan filtreuygulanmış bir belge listesi içerir. Arama sonuçları sayfasını nasıl oluşturabileceğinize bağlı olarak, filtre uygulanmış sonuç kümesini yansıtacak görsel ipuçları eklemek isteyebilirsiniz.

## <a name="conclusion"></a>Sonuç

Bu izlemede, Azure Bilişsel Arama sonuçlarındaki belgeleri filtrelemek için AAD oturum açma tekniklerini kullanarak istekte sağlanan filtreyle eşleşmeyan belgelerin sonuçlarını kırparak öğrendiniz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama filtrelerini kullanarak kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search.md)
+ [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
+ [Azure Bilişsel Arama işlemlerinde veri güvenliği ve erişim denetimi](search-security-overview.md)
