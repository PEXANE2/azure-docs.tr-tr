---
title: Active Directory kullanarak sonuçları kırpmaya yönelik güvenlik filtreleri Azure Search
description: Güvenlik filtrelerini ve Azure Active Directory (AAD) kimliklerini kullanarak Azure Search içerikte erişim denetimi.
author: brjohnstmsft
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 11/07/2017
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 8bcc1dcd1d86c0ca18ed03dc60834884a42a39c9
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70186523"
---
# <a name="security-filters-for-trimming-azure-search-results-using-active-directory-identities"></a>Active Directory kimliklerini kullanarak Azure Search sonuçları kırpma için güvenlik filtreleri

Bu makalede, Kullanıcı grubu üyeliğine göre arama sonuçlarını kırpmak için Azure Search filtreleriyle birlikte Azure Active Directory (AAD) güvenlik kimliklerinin birlikte nasıl kullanılacağı gösterilmektedir.

Bu makale aşağıdaki görevleri kapsar:
> [!div class="checklist"]
> - AAD grupları ve kullanıcıları oluşturma
> - Kullanıcıyı oluşturduğunuz grupla ilişkilendirin
> - Yeni grupları önbelleğe al
> - İlişkili gruplarıyla belge dizini oluştur
> - Grup tanımlayıcıları filtresiyle bir arama isteği verme
> 
> [!NOTE]
> Bu makaledeki örnek kod parçacıkları ' de C#yazılmıştır. Tam kaynak kodunu [GitHub](https://aka.ms/search-dotnet-howto)'da bulabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

Azure Search içindeki dizininiz, belgeye okuma erişimi olan grup kimliklerinin listesini depolamak için bir [güvenlik alanı](search-security-trimming-for-azure-search.md) içermelidir. Bu kullanım örneği, güvenli kılınabilir bir öğe (örneğin, bir okul uygulaması) ve bu öğeye kimlerin erişimi olduğunu belirten bir güvenlik alanı (Admissions personeli) arasında bire bir yazışma olduğunu varsayar.

AAD 'de Kullanıcı, Grup ve ilişkilendirme oluşturmak için bu kılavuzda AAD yönetici izinlerinizin olması gerekir.

Uygulamanız Ayrıca, aşağıdaki yordamda açıklandığı gibi AAD ile kayıtlı olmalıdır.

### <a name="register-your-application-with-aad"></a>Uygulamanızı AAD ile kaydetme

Bu adım, Kullanıcı ve grup hesaplarının oturum açma işlemlerini kabul etmek amacıyla uygulamanızı AAD ile tümleştirir. Kuruluşunuzda bir AAD yöneticisi değilseniz, aşağıdaki adımları gerçekleştirmek için [Yeni bir kiracı oluşturmanız](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) gerekebilir.

1. [**Uygulama kayıt portalı**](https://apps.dev.microsoft.com) >  **yakınsanmış** > uygulama uygulama**Ekle**' ye gidin.
2. Uygulamanız için bir ad girin ve ardından **Oluştur**' a tıklayın. 
3. Uygulamalarım sayfasında yeni kayıtlı uygulamanızı seçin.
4. Uygulama kaydı sayfasında > **platformlar** > **platformu Ekle**' yi seçin.
5. Hala uygulama kaydı sayfasında > **Microsoft Graph izinler** > **Ekle**' ye gidin.
6. Izinleri Seç ' de, aşağıdaki temsilci izinlerini ekleyin ve ardından **Tamam**' a tıklayın:

   + **Directory. ReadWrite. All**
   + **Group. ReadWrite. All**
   + **User. ReadWrite. All**

Microsoft Graph, bir REST API aracılığıyla AAD 'ye programlı erişim sağlayan bir API sağlar. Bu izlenecek yol için kod örneği, grupları, kullanıcıları ve ilişkilendirmeleri oluşturmak için Microsoft Graph API 'sini çağırmak için izinleri kullanır. API 'Ler, daha hızlı filtreleme için Grup tanımlayıcılarını önbelleğe almak için de kullanılır.

## <a name="create-users-and-groups"></a>Kullanıcı ve grup oluşturma

Oluşturulan bir uygulamaya arama ekliyorsanız, AAD 'de mevcut kullanıcı ve grup tanımlayıcılarından olabilirsiniz. Bu durumda, sonraki üç adımı atlayabilirsiniz. 

Ancak, mevcut kullanıcılarınız yoksa güvenlik sorumlularını oluşturmak için Microsoft Graph API 'Leri kullanabilirsiniz. Aşağıdaki kod parçacıkları, Azure Search dizininizdeki güvenlik alanı için veri değerleri haline gelen tanımlayıcıların nasıl oluşturulacağını göstermektedir. Kuramsal üniversite sayede uygulamamızda bu, sayede personeli için güvenlik tanımlayıcıları olacaktır.

Kullanıcı ve grup üyeliği, özellikle büyük kuruluşlarda çok akıcı olabilir. Kullanıcı ve grup kimliklerini oluşturan kodun, kuruluş üyeliğindeki değişiklikleri çekmek için genellikle yeterince çalıştırması gerekir. Benzer şekilde, Azure Search dizininiz izin verilen kullanıcı ve kaynakların geçerli durumunu yansıtmak için benzer bir güncelleştirme zamanlaması gerektirir.

### <a name="step-1-create-aad-grouphttpsdocsmicrosoftcomgraphapigroup-post-groupsviewgraph-rest-10"></a>1\. adım: [AAD grubu](https://docs.microsoft.com/graph/api/group-post-groups?view=graph-rest-1.0) oluştur 
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
   
### <a name="step-2-create-aad-userhttpsdocsmicrosoftcomgraphapiuser-post-usersviewgraph-rest-10"></a>2\. adım: [AAD kullanıcısı](https://docs.microsoft.com/graph/api/user-post-users?view=graph-rest-1.0) oluştur
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

### <a name="step-3-associate-user-and-group"></a>3\. adım: Kullanıcı ve grup ilişkilendir
```csharp
await graph.Groups[newGroup.Id].Members.References.Request().AddAsync(newUser);
```

### <a name="step-4-cache-the-groups-identifiers"></a>4\. Adım: Grup tanımlayıcılarını önbelleğe al
İsteğe bağlı olarak, ağ gecikmesini azaltmak için Kullanıcı grubu ilişkilendirmelerini önbelleğe alarak, bir arama isteği verildiğinde, gruplar önbellekten döndürülür ve AAD 'ye gidiş dönüş kaydedilir. Birden çok kullanıcıya sahip tek bir http isteği göndermek ve önbelleği oluşturmak için [AAD Batch API 'sini](https://developer.microsoft.com/graph/docs/concepts/json_batching) kullanabilirsiniz.

Microsoft Graph, yüksek hacimli istekleri işlemek için tasarlanmıştır. İstek sayısı ortaya çıkarsa, Microsoft Graph HTTP durum kodu 429 ile başarısız olur. Daha fazla bilgi için bkz. [Microsoft Graph azaltma](https://developer.microsoft.com/graph/docs/concepts/throttling).

## <a name="index-document-with-their-permitted-groups"></a>Belge, izin verilen gruplarıyla dizin oluştur

Azure Search sorgu işlemleri Azure Search bir dizin üzerinden yürütülür. Bu adımda, dizin oluşturma işlemi, güvenlik filtreleri olarak kullanılan tanımlayıcılar da dahil olmak üzere aranabilir verileri bir dizine aktarır. 

Azure Search Kullanıcı kimliklerinin kimliğini doğrulamaz veya bir kullanıcının görüntüleme iznine sahip olduğu içeriği oluşturmak için mantık sağlar. Güvenlik kırpması için kullanım örneği, gizli bir belge ile bu belgeye erişim sahibi olan grup tanımlayıcısı arasındaki ilişkiyi, bir arama dizinine bir şekilde içeri aktardığınızı varsayar. 

Kuramsal örnekte, bir Azure Search dizindeki PUT isteğinin gövdesi, bir başvuranın okul esi veya dökümünü, bu içeriği görüntüleme izni olan grup tanımlayıcısıyla birlikte içermelidir. 

Bu izlenecek yol için kod örneğinde kullanılan genel örnekte, Dizin eylemi şöyle görünebilir:

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

Güvenlik kırpma amacıyla, dizindeki güvenlik alanındaki değerler, arama sonuçlarına belge ekleme veya dışlama için kullanılan statik değerlerdir. Örneğin, Admissions için grup tanımlayıcısı "A11B22C33D44-E55F66G77-H88I99JKK" ise, güvenlik alanındaki bu tanımlayıcıya sahip bir Azure Search dizindeki tüm belgeler, istek sahibine geri gönderilen arama sonuçlarına dahil edilir (veya hariç tutulur).

İstek veren kullanıcı gruplarına bağlı olarak arama sonuçlarında döndürülen belgeleri filtrelemek için aşağıdaki adımları gözden geçirin.

### <a name="step-1-retrieve-users-group-identifiers"></a>1\. adım: Kullanıcının grup tanımlayıcılarını alma

Kullanıcının grupları zaten önbelleğe alınmamışsa veya önbelleğin süresi dolmuşsa, [gruplar](https://docs.microsoft.com/graph/api/directoryobject-getmembergroups?view=graph-rest-1.0) isteğini yayınlayın
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

### <a name="step-2-compose-the-search-request"></a>2\. adım: Arama isteğini oluştur

Kullanıcının grup üyeliğine sahip olduğunuz varsayılarak, arama isteğini uygun filtre değerleriyle verebilirsiniz.

```csharp
string filter = String.Format("groupIds/any(p:search.in(p, '{0}'))", string.Join(",", groups.Select(g => g.ToString())));
SearchParameters parameters = new SearchParameters()
             {
                 Filter = filter,
                 Select = new[] { "application essays" }
             };

DocumentSearchResult<SecuredFiles> results = _indexClient.Documents.Search<SecuredFiles>("*", parameters);
```
### <a name="step-3-handle-the-results"></a>3\. adım: Sonuçları işleyin

Yanıt, kullanıcının görüntüleme iznine sahip olduğu öğelerden oluşan filtrelenmiş bir belge listesi içerir. Arama sonuçları sayfasını oluşturma yönteminize bağlı olarak, filtrelenmiş sonuç kümesini yansıtmak için görsel ipuçları dahil etmek isteyebilirsiniz.

## <a name="conclusion"></a>Sonuç

Bu kılavuzda, istekte belirtilen filtreyle eşleşmeyen belgelerin sonuçlarını kırptırabilmeniz için Azure Search sonuçlarında belge filtrelemeye yönelik AAD oturum açma yöntemlerini kullanmayı öğrendiniz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search filtreleri kullanılarak kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search.md)
+ [Azure Search filtreler](search-filters.md)
+ [Azure Search işlemlerinde veri güvenliği ve erişim denetimi](search-security-overview.md)
