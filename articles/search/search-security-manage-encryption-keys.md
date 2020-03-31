---
title: Müşteri tarafından yönetilen anahtarları kullanarak istirahatte şifreleme
titleSuffix: Azure Cognitive Search
description: Azure Anahtar Kasası'nda oluşturduğunuz ve yönettiğiniz anahtarları kullanarak Azure Bilişsel Arama'da dizinler ve eşanlamlı haritalar üzerinden sunucu tarafında şifreleme tamam.
manager: nitinme
author: NatiNimni
ms.author: natinimn
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: cb17fe24339ad618229b3456ece15c206f79bdb7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76899940"
---
# <a name="encryption-at-rest-of-content-in-azure-cognitive-search-using-customer-managed-keys-in-azure-key-vault"></a>Azure Anahtar Kasası'nda müşteri tarafından yönetilen anahtarları kullanarak Azure Bilişsel Arama'da içeriğin geri kalanında şifreleme

Varsayılan olarak, Azure Bilişsel Arama, hizmet [tarafından yönetilen anahtarlarla](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest#data-encryption-models)dizin ekinli içeriği parolayla şifreler. Azure Anahtar Kasası'nda oluşturduğunuz ve yönettiğiniz anahtarları kullanarak varsayılan şifrelemeyi ekbir şifreleme katmanı ile tamamlayabilirsiniz. Bu makale, adımları size yol.

Sunucu tarafı şifreleme, [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)ile tümleştirme yoluyla desteklenir. Kendi şifreleme anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya şifreleme anahtarları oluşturmak için Azure Key Vault'un API'lerini kullanabilirsiniz. Azure Key Vault ile anahtar kullanımını da denetleyebilirsiniz. 

Müşteri tarafından yönetilen anahtarlarla şifreleme, bu nesneler oluşturulduğunda dizin veya eşanlamlı eşharita düzeyinde yapılandırılır, arama hizmeti düzeyinde değil. Zaten var olan içeriği şifreleyemezsiniz. 

Anahtarların aynı Anahtar Kasası'nda olması gerekmez. Tek bir arama hizmeti, her biri farklı Anahtar Kasalarında depolanan kendi müşteri tarafından yönetilen şifreleme anahtarlarıyla şifrelenmiş birden çok şifreli dizin veya eş anlamlı eşeği barındırabilir.  Ayrıca, müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenmemiş aynı hizmette dizinler ve eşanlamlı eşlemler de bulabilirsiniz. 

> [!IMPORTANT] 
> Bu özellik REST [API sürümü 2019-05-06](https://docs.microsoft.com/rest/api/searchservice/) ve [.NET SDK sürümü 8.0-önizleme](search-dotnet-sdk-migration-version-9.md)mevcuttur. Azure portalında müşteri tarafından yönetilen şifreleme anahtarlarını yapılandırmak için şu anda destek yok. Arama hizmeti Ocak 2019'dan sonra oluşturulmalıdır ve Ücretsiz (paylaşılan) bir hizmet olamaz.

## <a name="prerequisites"></a>Ön koşullar

Bu örnekte aşağıdaki hizmetler kullanılmıştır. 

+ [Bir Azure Bilişsel Arama hizmeti oluşturun](search-create-service-portal.md) veya geçerli aboneliğiniz altında [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) 

+ [Bir Azure Key Vault kaynağı oluşturun](https://docs.microsoft.com/azure/key-vault/quick-create-portal#create-a-vault) veya aboneliğiniz altında mevcut bir kasa bulun.

+ [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) veya [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) yapılandırma görevleri için kullanılır.

+ [Postacı](search-get-started-postman.md), [Azure PowerShell](search-create-index-rest-api.md) ve [Azure Bilişsel Arama SDK](https://aka.ms/search-sdk-preview) REST API aramak için kullanılabilir. Şu anda müşteri tarafından yönetilen şifreleme için portal desteği yoktur.

>[!Note]
> Müşteri tarafından yönetilen anahtarlar özelliğine sahip şifrelemenin doğası gereği, Azure Anahtar kasa anahtarınız silinirse Azure Bilişsel Arama verilerinizi alamaz. Yanlışlıkla Key Vault anahtar silmeneden veri kaybını önlemek için, kullanılmadan önce Anahtar Vault Yumuşak Silme ve Temizleme Koruma **etkinleştirmelisiniz.** Daha fazla bilgi için Azure [Key Vault yumuşak silme'ye](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)bakın.   

## <a name="1---enable-key-recovery"></a>1 - Anahtar kurtarmayı etkinleştirme

Azure Key Vault kaynağını oluşturduktan sonra, aşağıdaki PowerShell veya Azure CLI komutlarını uygulayarak seçili Anahtar kasasında **Yumuşak Silme** ve **Temizleme Koruması'nı** etkinleştirin:   

```powershell
$resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "<vault_name>").ResourceId

$resource.Properties | Add-Member -MemberType NoteProperty -Name "enableSoftDelete" -Value 'true'

$resource.Properties | Add-Member -MemberType NoteProperty -Name "enablePurgeProtection" -Value 'true'

Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

```azurecli-interactive
az keyvault update -n <vault_name> -g <resource_group> --enable-soft-delete --enable-purge-protection
```

## <a name="2---create-a-new-key"></a>2 - Yeni bir anahtar oluşturma

Azure Bilişsel Arama içeriğini şifrelemek için varolan bir anahtar kullanıyorsanız, bu adımı atlayın.

1. [Azure portalında oturum açın](https://portal.azure.com) ve anahtar kasa panosuna gidin.

1. Sol gezinti bölmesinden **Tuşlar** ayarını seçin ve **+ Oluştur/İçe Aktar'ı**tıklatın.

1. **Seçenekler**listesinden **anahtar** bölmesinde, bir anahtar oluşturmak için kullanmak istediğiniz yöntemi seçin. Yeni bir anahtar **oluşturabilir,** varolan bir anahtarı **yükleyebilir** veya anahtarın yedeğini seçmek için **Yedeklemeyi Geri Yükle'yi** kullanabilirsiniz.

1. Anahtarınız için bir **Ad** girin ve isteğe bağlı olarak diğer önemli özellikleri seçin.

1. Dağıtımı başlatmak için **Oluştur** düğmesini tıklatın.

Anahtar Tanımlayıcı bir not olun - Bu **anahtar değeri Uri**oluşur , anahtar **adı**, ve **anahtar sürümü**. Azure Bilişsel Arama'da şifrelenmiş bir dizin tanımlamak için bunlara ihtiyacınız olacaktır.
 
![Yeni bir anahtar kasası anahtarı oluşturma](./media/search-manage-encryption-keys/create-new-key-vault-key.png "Yeni bir anahtar kasası anahtarı oluşturma")

## <a name="3---create-a-service-identity"></a>3 - Hizmet kimliği oluşturma

Arama hizmetinize bir kimlik atamak, Arama hizmetinize Key Vault erişim izinleri vermenize olanak tanır. Arama hizmetiniz, Azure Anahtar kasası ile kimlik doğrulamak için kimliğini kullanır.

Azure Bilişsel Arama, kimlik atamanın iki yolunu destekler: yönetilen bir kimlik veya harici olarak yönetilen bir Azure Etkin Dizin uygulaması. 

Mümkünse yönetilen bir kimlik kullanın. Arama hizmetinize kimlik atamanın en basit yoludur ve çoğu senaryoda çalışmalıdır. Dizinler ve eşanlamlı haritalar için birden çok anahtar kullanıyorsanız veya çözümünüzün kimlik tabanlı kimlik doğrulamayı diskalifiye eden dağıtılmış bir mimaride yse, bu makalenin sonunda açıklanan [gelişmiş dış tarafından yönetilen Azure Etkin Dizin yaklaşımını](#aad-app) kullanın.

 Genel olarak, yönetilen bir kimlik, arama hizmetinizin kimlik bilgilerini kodda depolamadan Azure Anahtar Kasası'nda kimliğinin doğrulanmasını sağlar. Bu tür yönetilen kimliğin yaşam döngüsü, yalnızca tek bir yönetilen kimliğe sahip olabilecek arama hizmetinizin yaşam döngüsüne bağlıdır. [Yönetilen kimlikler hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

1. Yönetilen bir kimlik oluşturmak için [Azure portalında oturum açın](https://portal.azure.com) ve arama hizmeti panonuzu açın. 

1. Sol gezinti bölmesinde **Kimlik'i** tıklatın, durumunu **Açık**olarak değiştirin ve **Kaydet'i**tıklatın.

![Yönetilen bir kimliği etkinleştirme](./media/search-enable-msi/enable-identity-portal.png "Manged kimliği etkinleştirme")

## <a name="4---grant-key-access-permissions"></a>4 - Anahtar erişim izinlerini verme

Arama hizmetinizin Key Vault anahtarınızı kullanmasını sağlamak için, arama hizmetinize belirli erişim izinleri vermeniz gerekir.

Erişim izinleri herhangi bir zamanda iptal edilebilir. İptal edildikten sonra, bu anahtar kasası kullanan herhangi bir arama hizmeti dizini veya eşanlamlı harita kullanılamaz hale gelecektir. Key vault erişim izinlerini daha sonra geri yüklemek dizin\eş anlamlıharita erişimini geri yükler. Daha fazla bilgi için, [anahtar kasasına Güvenli erişim](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault)bakın.

1. [Azure portalında oturum açın](https://portal.azure.com) ve önemli kasa genel bakış sayfanızı açın. 

1. Sol gezinti bölmesinden **Access ilkeleri** ayarını seçin ve **+Ekle'yi**tıklatın.

   ![Yeni anahtar kasa erişim ilkesi ekleme](./media/search-manage-encryption-keys/add-new-key-vault-access-policy.png "Yeni anahtar kasa erişim ilkesi ekleme")

1. **Anapara Seç'i** tıklatın ve Azure Bilişsel Arama hizmetinizi seçin. Yönetilen kimliği etkinleştirdikten sonra görüntülenen ad veya nesne kimliği ile arama yapabilirsiniz.

   ![Anahtar kasa erişim ilkesi ilkesini seçin](./media/search-manage-encryption-keys/select-key-vault-access-policy-principal.png "Anahtar kasa erişim ilkesi ilkesini seçin")

1. Anahtar **izinlerine** tıklayın ve *Al*, *Anahtarı Aç* ve *Sar't Tuşu'nü*seçin. Gerekli izinleri hızla seçmek için *Azure Veri Gölü Depolama sını veya Azure Depolama* şablonunu kullanabilirsiniz.

   Azure Bilişsel Arama aşağıdaki erişim [izinleri](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-operations)ile verilmelidir:

   * *Get* - arama hizmetinizi Key Vault'ta anahtarınızın ortak bölümlerini almanızı sağlar
   * *Kaydırma Anahtarı* - arama hizmetinizin dahili şifreleme anahtarını korumak için anahtarınızı kullanmasına izin verir
   * *Anahtarı Açma* - dahili şifreleme anahtarını açmak için arama hizmetinizin anahtarınızı kullanmasına izin verir

   ![Anahtar kasa erişim ilkesi anahtar izinlerini seçin](./media/search-manage-encryption-keys/select-key-vault-access-policy-key-permissions.png "Anahtar kasa erişim ilkesi anahtar izinlerini seçin")

1. **Tamam'ı** tıklatın ve erişim ilkesi değişikliklerini **kaydedin.**

> [!Important]
> Azure Bilişsel Arama'daki şifreli içerik, belirli bir Azure Anahtar Kasası anahtarını belirli bir **sürümle**kullanacak şekilde yapılandırılmıştır. Anahtarı veya sürümü değiştirirseniz, önceki tuşu/sürümü silmeden **önce** yeni anahtar\sürümünü kullanmak için dizin veya eşanlamlı eşlemi güncelleştirilmelidir. Bunu yapmamak dizin veya eşanlamlı eşlemeyi kullanılamaz hale getirir, anahtar erişimi kaybolduğunda içeriğin şifresini çözemezsiniz.   

## <a name="5---encrypt-content"></a>5 - İçeriği şifreleme

Müşteri tarafından yönetilen anahtarla şifrelenmiş bir dizin veya eşanlamlı eşlemi oluşturmak, Azure portalı kullanılarak henüz mümkün değildir. Böyle bir dizin veya eşanlamlı eşlemi oluşturmak için Azure Bilişsel Arama REST API'sini kullanın.

Hem dizin hem de eşanlamlı harita, anahtarı belirtmek için kullanılan yeni bir üst düzey **şifrelemeAnahtar** özelliğini destekler. 

Anahtar **vault Uri,** **anahtar adı** ve Key vault anahtarı **anahtar sürümünü** kullanarak, bir **şifrelemeAnahtar** tanımı oluşturabilirsiniz:

```json
{
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
  }
}
```
> [!Note] 
> Bu önemli kasa ayrıntılarının hiçbiri gizli olarak kabul edilmez ve Azure portalındaki ilgili Azure Key Vault anahtar sayfasına göz atarak kolayca alınabilir.

Yönetilen bir kimlik kullanmak yerine Key Vault kimlik doğrulaması için bir AAD uygulaması kullanıyorsanız, Şifreleme anahtarınıza AAD uygulama **erişim kimlik bilgilerini** ekleyin: 
```json
{
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660",
    "accessCredentials": {
      "applicationId": "00000000-0000-0000-0000-000000000000",
      "applicationSecret": "myApplicationSecret"
    }
  }
}
```

## <a name="example-index-encryption"></a>Örnek: Dizin şifreleme
REST API üzerinden yeni bir dizin oluşturmanın ayrıntıları, buradaki tek farkın dizin tanımının bir parçası olarak şifreleme anahtarı ayrıntılarını belirtmek olduğu [Create Index'te (Azure Bilişsel Arama REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index)bulunabilir: 

```json
{
 "name": "hotels",  
 "fields": [
  {"name": "HotelId", "type": "Edm.String", "key": true, "filterable": true},
  {"name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false},
  {"name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene"},
  {"name": "Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
  {"name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
  {"name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true},
  {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true},
  {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true},
  {"name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true},
  {"name": "Location", "type": "Edm.GeographyPoint", "filterable": true, "sortable": true},
 ],
 "encryptionKey": {
   "keyVaultUri": "https://demokeyvault.vault.azure.net",
   "keyVaultKeyName": "myEncryptionKey",
   "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
 }
}
```
Şimdi dizin oluşturma isteğini gönderebilir ve ardından dizini normal olarak kullanmaya başlayabilirsiniz.

## <a name="example-synonym-map-encryption"></a>Örnek: Eş anlamlı harita şifrelemesi

REST API üzerinden yeni bir eş anlamlı harita oluşturmanın ayrıntıları, buradaki tek farkın eşanlamlı harita tanımının bir parçası olarak şifreleme anahtarı ayrıntılarını belirtmek olduğu Eş anlamlı harita [oluştur 'da (Azure Bilişsel Arama REST API)](https://docs.microsoft.com/rest/api/searchservice/create-synonym-map)bulunabilir: 

```json
{   
  "name" : "synonymmap1",  
  "format" : "solr",  
  "synonyms" : "United States, United States of America, USA\n
  Washington, Wash. => WA",
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
  }
}
```
Artık eşanlamlı eş oluşturma isteğini gönderebilir ve sonra normal olarak kullanmaya başlayabilirsiniz.

>[!Important] 
> **EncryptionKey** varolan Azure Bilişsel Arama dizinlerine veya eşanlamlı eşanlamlı eşlemlere eklenemez, ancak üç temel kasa ayrıntılarından herhangi biri için farklı değerler sağlayarak güncellenebilir (örneğin, anahtar sürümü güncelleştirme). Yeni bir Key Vault anahtarına veya yeni bir anahtar sürümüne geçerken, anahtarı kullanan herhangi bir Azure Bilişsel Arama dizini veya eşanlamlı eşlemesi, önceki anahtarı silmeden **önce** yeni anahtar\sürümünü kullanmak için öncelikle güncelleştirilmelidir. Bunu yapmamak, anahtar erişimi kaybolduğunda içeriğin şifresini çözemeyeceğinden, dizin veya eşanlamlı eşlemeyi kullanılamaz hale getirir.   
> Key vault erişim izinlerini daha sonra geri yüklemek içerik erişimini geri yükler.

## <a name="advanced-use-an-externally-managed-azure-active-directory-application"></a><a name="aad-app"></a>Gelişmiş: Harici olarak yönetilen bir Azure Etkin Dizin uygulaması kullanma

Yönetilen bir kimlik mümkün olmadığında, Azure Bilişsel Arama hizmetiniz için bir güvenlik ilkesine sahip bir Azure Etkin Dizin uygulaması oluşturabilirsiniz. Özellikle, yönetilen bir kimlik bu koşullar altında geçerli değildir:

* Arama hizmeti erişim izinlerinizi Doğrudan Key vault'a veremezsiniz (örneğin, arama hizmeti Azure Anahtar Kasası'ndan farklı bir Active Directory kiracısındaysa).

* Her biri farklı bir Anahtar kasasından farklı bir anahtar kullanan ve her anahtar tonozunu kimlik doğrulaması için **farklı bir kimlik** kullanması gereken birden çok şifreli dizin\eş anlamlı eşlemi barındırmak için tek bir arama hizmeti gereklidir. Farklı Anahtar tonozlarını yönetmek için farklı bir kimlik kullanmak bir gereklilik değilse, yukarıdaki yönetilen kimlik seçeneğini kullanmayı düşünün.  

Azure Bilişsel Arama, bu tür topolojileri barındırmak için, arama hizmetiniz ile Key Vault arasında kimlik doğrulama için Azure Active Directory (AAD) uygulamalarını kullanarak destekler.    
Portalda bir AAD uygulaması oluşturmak için:

1. [Azure Etkin Dizin uygulaması oluşturun.](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application)

1. Şifreli dizin oluşturmak için gerekli olacakşekilde [uygulama kimliği ve kimlik doğrulama anahtarını alın.](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) Sağlamanız gereken değerler arasında **uygulama kimliği** ve kimlik **doğrulama anahtarı**yer almaktadır.

>[!Important]
> Yönetilen bir kimlik yerine aad kimlik doğrulama uygulamasını kullanmaya karar verirken, Azure Bilişsel Arama'nın AAD uygulamanızı sizin adınıza yönetme yetkisine sahip olmadığını ve periyodik olarak AAD uygulamanızı yönetmenin size bağlı olduğunu göz önünde bulundurun uygulama kimlik doğrulama anahtarının döndürülme.
> Bir AAD uygulamasını veya kimlik doğrulama anahtarını değiştirirken, bu uygulamayı kullanan herhangi bir Azure Bilişsel Arama dizini veya eşanlamlı eşlemesi, önceki uygulamayı veya yetkilendirme anahtarını silmeden önce ve Anahtar Kasa erişimini iptal etmeden **önce** yeni uygulama kimliği\anahtarını kullanmak için önce güncelleştirilmelidir.
> Bunu yapmamak, anahtar erişimi kaybolduğunda içeriğin şifresini çözemeyeceğinden, dizin veya eşanlamlı eşlemeyi kullanılamaz hale getirir.   

## <a name="next-steps"></a>Sonraki adımlar

Azure güvenlik mimarisini bilmiyorsanız, [Azure Güvenlik belgelerini](https://docs.microsoft.com/azure/security/)ve özellikle bu makaleyi inceleyin:

> [!div class="nextstepaction"]
> [Bekleyen veri şifreleme](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)
