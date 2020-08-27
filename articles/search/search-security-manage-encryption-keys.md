---
title: Müşteri tarafından yönetilen anahtarları kullanarak bekleyen şifreleme
titleSuffix: Azure Cognitive Search
description: Azure Key Vault içinde oluşturduğunuz ve yönettiğiniz anahtarları kullanarak Azure Bilişsel Arama Dizin ve eş anlamlı haritalar üzerinde sunucu tarafı şifrelemeyi tamamlar.
manager: nitinme
author: NatiNimni
ms.author: natinimn
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 08/01/2020
ms.custom: references_regions
ms.openlocfilehash: 2dc7458dd905ff84455927c81b4ea93765d4f5cb
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88928828"
---
# <a name="configure-customer-managed-keys-for-data-encryption-in-azure-cognitive-search"></a>Azure Bilişsel Arama veri şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma

Azure Bilişsel Arama, [hizmet tarafından yönetilen anahtarlarla](../security/fundamentals/encryption-atrest.md#azure-encryption-at-rest-components)bekleyen dizinli içeriği otomatik olarak şifreler. Daha fazla koruma gerekiyorsa, Azure Key Vault ' de oluşturduğunuz ve yönettiğiniz anahtarları kullanarak, varsayılan şifrelemeyi ek bir şifreleme katmanıyla birlikte tamamlayabilirsiniz. Bu makale, CMK şifrelemesini ayarlama adımlarında size yol gösterir.

CMK şifrelemesi [Azure Key Vault](../key-vault/general/overview.md)bağımlıdır. Kendi şifreleme anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya şifreleme anahtarları oluşturmak için Azure Key Vault API 'Lerini kullanabilirsiniz. Azure Key Vault ile [günlüğe kaydetmeyi etkinleştirirseniz](../key-vault/general/logging.md)anahtar kullanımını da denetleyebilirsiniz.  

Müşteri tarafından yönetilen anahtarlarla şifreleme, bu nesneler oluşturulduğunda tek tek dizinlere veya eş anlamlı haritalara uygulanır ve arama hizmeti düzeyinin kendisi üzerinde belirtilmez. Yalnızca yeni nesneler şifrelenebilir. Zaten var olan içeriği şifrelenemez.

Anahtarların tümünün aynı anahtar kasasında olması gerekmez. Tek bir arama hizmeti, her biri farklı anahtar kasalarında depolanan, her biri kendi müşteri tarafından yönetilen şifreleme anahtarlarıyla şifrelenen birden çok şifrelenmiş dizini veya eş anlamlı haritaları barındırabilirler. Ayrıca, aynı hizmette, müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenmemiş dizinler ve eş anlamlı haritalar da olabilir. 

## <a name="double-encryption"></a>Çift şifreleme

1 Ağustos 2020 ve belirli bölgelerde oluşturulan hizmetler için CMK şifrelemesi [kapsamı, geçici](search-security-overview.md#double-encryption)diskler içerir ve şu anda şu bölgelerde kullanılabilir: 

+ Batı ABD 2
+ Doğu ABD
+ Orta Güney ABD
+ US Gov Virginia
+ US Gov Arizona

Farklı bir bölge veya 1 Ağustos 'Tan önce oluşturulmuş bir hizmet kullanıyorsanız CMK şifrelemeniz, hizmet tarafından kullanılan geçici diskler hariç yalnızca veri diski ile sınırlıdır.

## <a name="prerequisites"></a>Önkoşullar

Bu örnekte aşağıdaki hizmetler ve hizmetler kullanılır. 

+ [Bir Azure bilişsel arama hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). 

+ Azure Bilişsel Arama ile aynı abonelikte [bir Azure Key Vault kaynağı oluşturun](../key-vault/secrets/quick-create-portal.md#create-a-vault) veya mevcut bir kasayı bulun. Bu özelliğin aynı abonelik gereksinimi vardır.

+ Yapılandırma görevleri için [Azure PowerShell](/powershell/azure/) veya [Azure CLI](/cli/azure/install-azure-cli) kullanılır.

+ [Postman](search-get-started-postman.md), [Azure POWERSHELL](./search-get-started-powershell.md) ve [.NET SDK önizlemesi](https://aka.ms/search-sdk-preview) , şifreleme anahtar parametresi içeren dizinler ve eş anlamlı eşlemeler oluşturan REST API çağırmak için kullanılabilir. Şu anda dizinlere veya eş anlamlı haritalara anahtar eklemek için bir portal desteği yoktur.

>[!Note]
> Müşteri tarafından yönetilen anahtarlarla şifrelemenin doğası nedeniyle, Azure Anahtar Kasası anahtarınız silinirse Azure Bilişsel Arama verilerinizi alamaz. Yanlışlıkla Key Vault anahtar silmeleri nedeniyle oluşan veri kaybını engellemek için, anahtar kasasında geçici silme ve Temizleme korumasının etkinleştirilmesi gerekir. Geçici silme varsayılan olarak etkindir, bu nedenle yalnızca kasıtlı olarak devre dışı bırakılmışsa sorunlarla karşılaşırsınız. Temizleme koruması varsayılan olarak etkin değildir, ancak Azure Bilişsel Arama CMK şifrelemesi için bu gereklidir. Daha fazla bilgi için bkz. [geçici-silme](../key-vault/general/soft-delete-overview.md) ve [Temizleme koruması](../key-vault/general/soft-delete-overview.md#purge-protection) genel bakış.

## <a name="1---enable-key-recovery"></a>1-anahtar kurtarmayı etkinleştir

Anahtar kasasında **geçici silme** ve **Temizleme koruması** etkinleştirilmiş olmalıdır. Portalı veya aşağıdaki PowerShell veya Azure CLı komutlarını kullanarak bu özellikleri ayarlayabilirsiniz.

### <a name="using-powershell"></a>PowerShell’i kullanma

1. `Connect-AzAccount`Azure kimlik bilgilerinizi kurmak için ' i çalıştırın.

1. Anahtar kasanıza bağlanmak ve geçerli bir adla değiştirmek için aşağıdaki komutu çalıştırın `<vault_name>` :

   ```powershell
   $resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "<vault_name>").ResourceId
   ```

1. Azure Key Vault, geçici silme etkin ile oluşturulur. Kasanızda devre dışıysa, aşağıdaki komutu çalıştırın:

   ```powershell
   $resource.Properties | Add-Member -MemberType NoteProperty -Name "enableSoftDelete" -Value 'true'
   ```

1. Temizleme korumasını etkinleştir:

   ```powershell
   $resource.Properties | Add-Member -MemberType NoteProperty -Name "enablePurgeProtection" -Value 'true'
   ```

1. Güncelleştirmelerinizi kaydedin:

   ```powershell
   Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
   ```

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

```azurecli-interactive
az keyvault update -n <vault_name> -g <resource_group> --enable-soft-delete --enable-purge-protection
```

## <a name="2---create-a-new-key"></a>2-yeni bir anahtar oluşturma

Azure Bilişsel Arama içeriğini şifrelemek için mevcut bir anahtar kullanıyorsanız, bu adımı atlayın.

1. [Azure Portal oturum açın](https://portal.azure.com) ve Anahtar Kasası genel bakış sayfasını açın.

1. Sol gezinti bölmesinden **anahtarlar** ayarını seçin ve **+ Oluştur/içeri aktar**' a tıklayın.

1. **Anahtar oluştur** bölmesinde, **Seçenekler**listesinden, anahtar oluşturmak için kullanmak istediğiniz yöntemi seçin. Yeni bir anahtar **oluşturabilir** , var olan bir anahtarı **yükleyebilir** veya bir anahtarın yedeğini seçmek için **yedekleme geri yükleme** ' yi kullanabilirsiniz.

1. Anahtarınız için bir **ad** girin ve isteğe bağlı olarak diğer anahtar özelliklerini seçin.

1. Dağıtımı başlatmak için **Oluştur** düğmesine tıklayın.

Anahtar tanımlayıcısını bir yere unutmayın: Bu, anahtar **değer URI 'si**, **anahtar adı**ve **anahtar sürümünden**oluşur. Azure Bilişsel Arama 'de şifrelenmiş bir dizin tanımlamanız için bunlara ihtiyacınız olacaktır.
 
![Yeni bir anahtar kasası anahtarı oluştur](./media/search-manage-encryption-keys/create-new-key-vault-key.png "Yeni bir anahtar kasası anahtarı oluştur")

## <a name="3---create-a-service-identity"></a>3-hizmet kimliği oluşturma

Arama hizmetinize bir kimlik atamak, arama hizmetinize Key Vault erişim izni vermenizi sağlar. Arama hizmetiniz, Azure Anahtar Kasası 'nda kimlik doğrulaması yapmak için kimliğini kullanacaktır.

Azure Bilişsel Arama, kimlik atamanın iki yolunu destekler: yönetilen bir kimlik veya dışarıdan yönetilen bir Azure Active Directory uygulaması. 

Mümkünse, yönetilen bir kimlik kullanın. Bu, arama hizmetinize bir kimlik atamanın en kolay yoludur ve çoğu senaryoda çalışır. Dizinler ve eş anlamlı haritalar için birden çok anahtar kullanıyorsanız veya çözümünüz kimlik tabanlı kimlik doğrulamasını niteleyen dağıtılmış bir mimaride ise, bu makalenin sonunda açıklanan Gelişmiş [dışarıdan yönetilen Azure Active Directory yaklaşımını](#aad-app) kullanın.

 Genel olarak, yönetilen bir kimlik, arama hizmetinizin kimlik bilgilerini kodda depolamadan Azure Key Vault kimlik doğrulaması yapmasına olanak sağlar. Bu tür yönetilen kimliğin yaşam döngüsü, yalnızca tek bir yönetilen kimliğe sahip olabilen arama hizmetinizin yaşam döngüsüne bağlıdır. [Yönetilen kimlikler hakkında daha fazla bilgi edinin](../active-directory/managed-identities-azure-resources/overview.md).

1. [Azure Portal oturum açın](https://portal.azure.com) ve arama hizmetine genel bakış sayfasını açın. 

1. Sol gezinti bölmesindeki **kimlik** ' e tıklayın, durumunu **Açık**olarak değiştirin ve **Kaydet**' e tıklayın.

![Yönetilen bir kimliği etkinleştirme](./media/search-enable-msi/enable-identity-portal.png "Bir kimlik kimliğini etkinleştirme")

## <a name="4---grant-key-access-permissions"></a>4-anahtar erişim izinleri verme

Arama hizmetinizin Key Vault anahtarınızı kullanmasını sağlamak için, arama hizmetinize belirli erişim izinlerini vermeniz gerekir.

Erişim izinleri belirli bir zamanda iptal edilebilir. Bu Anahtar Kasası 'nı kullanan herhangi bir arama hizmeti dizini veya eş anlamlı eşleme, iptal edildikten sonra kullanılamaz hale gelir. Anahtar Kasası erişim izinlerinin daha sonraki bir zamanda geri yüklenmesi, Dizin \ eş anlamlı eşleme erişimini geri yükler. Daha fazla bilgi için bkz. [bir anahtar kasasına güvenli erişim](../key-vault/general/secure-your-key-vault.md).

1. [Azure Portal oturum açın](https://portal.azure.com) ve Anahtar Kasası genel bakış sayfasını açın. 

1. Sol gezinti bölmesinden **erişim ilkeleri** ayarını seçin ve **+ Yeni Ekle**' ye tıklayın.

   ![Yeni Anahtar Kasası erişim ilkesi Ekle](./media/search-manage-encryption-keys/add-new-key-vault-access-policy.png "Yeni Anahtar Kasası erişim ilkesi Ekle")

1. **Sorumlu Seç** ' e tıklayın ve Azure bilişsel arama hizmetinizi seçin. Yönetilen kimliği etkinleştirdikten sonra adı veya görüntülenen nesne KIMLIĞINE göre arama yapabilirsiniz.

   ![Anahtar Kasası erişim ilkesi sorumlusunu seçin](./media/search-manage-encryption-keys/select-key-vault-access-policy-principal.png "Anahtar Kasası erişim ilkesi sorumlusunu seçin")

1. **Anahtar izinleri** ' ne tıklayın ve *Al*, *sarmalama tuşu* ve *sarmalama*anahtarı ' nı seçin. Gerekli izinleri hızlıca seçmek için *Azure Data Lake Storage veya Azure depolama* şablonunu kullanabilirsiniz.

   Azure Bilişsel Arama aşağıdaki [erişim izinleriyle](../key-vault/keys/about-keys.md#key-operations)verilmelidir:

   * *Get* -arama hizmetinizin Key Vault anahtarınızın ortak parçalarını almasına izin verir
   * *Anahtar Sarla* -arama hizmetinizin iç şifreleme anahtarını korumak için anahtarınızı kullanmasına izin verir
   * *Anahtar sarmalaması geri al* -arama hizmetinizin iç şifreleme anahtarını sarmalamak için anahtarınızı kullanmasına izin verir

   ![Anahtar Kasası erişim ilkesi anahtar izinlerini seçin](./media/search-manage-encryption-keys/select-key-vault-access-policy-key-permissions.png "Anahtar Kasası erişim ilkesi anahtar izinlerini seçin")

1. **Gizli izinler**için *Al*' ı seçin.

1. **Sertifika izinleri**için *Al*' ı seçin.

1. **Tamam** ' a tıklayın ve erişim Ilkesi değişikliklerini **kaydedin** .

> [!Important]
> Azure Bilişsel Arama şifrelenmiş içerik, belirli bir **sürüme**sahip belirli bir Azure Key Vault anahtarı kullanacak şekilde yapılandırılmıştır. Anahtarı veya sürümü değiştirirseniz, önceki key\version. silinmeden **önce** dizin veya eş anlamlı haritanın yeni key\version kullanacak şekilde güncellenmesi gerekir Bunun başarısız olması, anahtar erişimi kaybolduktan sonra içeriğin şifresini çözemeyecek şekilde dizin veya eş anlamlı haritanın kullanılamamasına neden olur.   

## <a name="5---encrypt-content"></a>5-içerik şifreleyin

Bir dizin veya eş anlamlı eşleme üzerinde müşteri tarafından yönetilen anahtar eklemek için [arama REST API](/rest/api/searchservice/) veya SDK 'yı kullanmanız gerekir. Portal, eş anlamlı haritalar veya şifreleme özellikleri sunmaz. Geçerli bir API kullandığınızda, her iki dizin ve eş anlamlı eşleme, üst düzey bir **encryptionKey** özelliğini destekler. 

Anahtar **Kasası URI**'sini, anahtar **adını** ve Anahtar Kasası anahtarınızın **anahtar sürümünü** kullanarak, aşağıdaki gibi bir **encryptionKey** tanımı oluşturun:

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
> Bu Anahtar Kasası ayrıntılarının hiçbiri gizli kabul edilmez ve Azure portal ' deki ilgili Azure Key Vault anahtarı sayfasına göz atarak kolayca alınabilir.

Yönetilen bir kimlik kullanmak yerine Key Vault kimlik doğrulaması için AAD uygulaması kullanıyorsanız, şifreleme anahtarınıza AAD uygulama **erişimi kimlik bilgilerini** ekleyin: 
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
Burada tek fark, Dizin tanımının bir parçası olarak şifreleme anahtarı ayrıntılarını belirtirken, REST API aracılığıyla yeni dizin oluşturma ayrıntıları [(Azure Bilişsel Arama REST API)](/rest/api/searchservice/create-index)bulunabilir. 

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
Artık dizin oluşturma isteğini gönderebilir ve sonra dizini normal olarak kullanmaya başlayabilirsiniz.

## <a name="example-synonym-map-encryption"></a>Örnek: eş anlamlı eşleme şifrelemesi

REST API aracılığıyla yeni bir eş anlamlı eşleme oluşturma ayrıntıları, burada tek fark, eş anlamlı eşleme tanımının bir parçası olarak şifreleme anahtarı ayrıntılarını belirtirken, [eş anlamlı harita oluşturma (Azure Bilişsel Arama REST API)](/rest/api/searchservice/create-synonym-map)' de bulunabilir: 

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
Artık eş anlamlı harita oluşturma isteğini gönderebilir ve normal olarak kullanmaya başlayabilirsiniz.

>[!Important] 
> **EncryptionKey** , mevcut Azure bilişsel arama dizinlerine veya eş anlamlı haritalara eklenemediğinden, üç Anahtar Kasası ayrıntılarının (örneğin, anahtar sürümünü güncelleştirme) her biri için farklı değerler sunarak güncelleştirilmiş olabilir. Yeni bir Key Vault anahtarına veya yeni bir anahtar sürümüne geçiş yaparken, önce anahtarı kullanan tüm Azure Bilişsel Arama dizini veya eş anlamlı Haritası, önceki key\version. silinmeden **önce** yeni key\version kullanacak şekilde güncellenmelidir Bunun başarısız olması, anahtar erişimi kaybolduktan sonra içeriğin şifresini çözemeyeceği için dizin veya eş anlamlı haritanın kullanılamaz hale gelmesine neden olur.   
> Anahtar Kasası erişim izinlerini daha sonra geri yüklemek, içerik erişimini geri yükler.

## <a name="advanced-use-an-externally-managed-azure-active-directory-application"></a><a name="aad-app"></a> Gelişmiş: dışarıdan yönetilen Azure Active Directory uygulaması kullanma

Yönetilen bir kimlik mümkün olmadığında Azure Bilişsel Arama hizmetiniz için güvenlik sorumlusu olan bir Azure Active Directory uygulaması oluşturabilirsiniz. Özellikle, yönetilen bir kimlik şu koşullarda önemli değildir:

* Anahtar kasasına doğrudan arama hizmeti erişim izinleri verilemez (örneğin, arama hizmeti, Azure Key Vault farklı bir Active Directory kiracısında ise).

* Tek bir arama hizmeti, her anahtar kasasının kimlik doğrulaması için **farklı bir kimlik** kullanması gereken farklı bir anahtar kasasından farklı bir anahtar kullanan birden çok şifrelenmiş ındexes\eş anlamlı haritaları barındırmak için gereklidir. Farklı anahtar kasalarını yönetmek için farklı bir kimlik kullanmak bir gereksinim değildir, yukarıdaki yönetilen kimlik seçeneğini kullanmayı düşünün.  

Azure Bilişsel Arama, bu tür topolojilerle uyum sağlamak için, arama hizmetiniz ve Key Vault arasında kimlik doğrulaması için Azure Active Directory (AAD) uygulamalarının kullanımını destekler.    
Portalda bir AAD uygulaması oluşturmak için:

1. [Azure Active Directory uygulaması oluşturun](../active-directory/develop/howto-create-service-principal-portal.md).

1. Şifrelenmiş bir dizin oluşturmak için gerekli olacak şekilde [, uygulama kimliği ve kimlik doğrulama anahtarını alın](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in) . **Uygulama kimliği** ve **kimlik doğrulama anahtarı**sağlamanız gereken değerler.

>[!Important]
> Yönetilen kimlik yerine kimlik doğrulaması için bir AAD uygulaması kullanmaya karar verirken, Azure Bilişsel Arama tarafından sizin adınıza AAD uygulamanızı yönetmek için yetkilendirilmemiş olduğunu göz önünde bulundurun ve uygulama kimlik doğrulama anahtarının düzenli olarak dönüşü gibi AAD uygulamanızı yönetmeniz gerekir.
> Bir AAD uygulamasını veya kimlik doğrulama anahtarını değiştirirken, bu uygulamayı kullanan tüm Azure Bilişsel Arama dizini veya eş anlamlı Haritası, önceki uygulamayı veya yetkilendirme anahtarını silmeden **önce** ve Key Vault erişiminizi iptal etmeden önce yeni uygulama ID\key kullanacak şekilde güncelleştirilmeleri gerekir.
> Bunun başarısız olması, anahtar erişimi kaybolduktan sonra içeriğin şifresini çözemeyeceği için dizin veya eş anlamlı haritanın kullanılamaz hale gelmesine neden olur.

## <a name="work-with-encrypted-content"></a>Şifrelenmiş içerikle çalışma

CMK şifrelemesi sayesinde, ek şifreleme/şifre çözme işi nedeniyle hem dizin oluşturma hem de sorgular için gecikme fark edeceksiniz. Azure Bilişsel Arama şifreleme etkinliğini günlüğe almaz, ancak Anahtar Kasası günlüğü aracılığıyla anahtar erişimini izleyebilirsiniz. Anahtar Kasası kurulumu 'nun bir parçası olarak [günlük kaydını etkinleştirmenizi](../key-vault/general/logging.md) öneririz.

Anahtar dönüşünün zaman içinde oluşması beklenmektedir. Anahtarları her döndürdüğünüzde, bu sırayı izlemeniz önemlidir:

1. [Bir dizin veya eş anlamlı eşleme tarafından kullanılan anahtarı belirleme](search-security-get-encryption-keys.md).
1. [Anahtar Kasası 'nda yeni bir anahtar oluşturun](../key-vault/keys/quick-create-portal.md), ancak özgün anahtarı kullanılabilir olarak bırakın.
1. Yeni değerleri kullanmak için bir dizin veya eş anlamlı haritadaki [encryptionKey özelliklerini güncelleştirin](/rest/api/searchservice/update-index) . Yalnızca bu özellikle oluşturulan nesneler, farklı bir değer kullanacak şekilde güncelleştirilemeyebilir.
1. Anahtar kasasındaki önceki anahtarı devre dışı bırakın veya silin. Yeni anahtarın kullanıldığını doğrulamak için anahtar erişimini izleyin.

Performans nedenleriyle, arama hizmeti anahtarı birkaç saate kadar önbelleğe alır. Anahtarı yeni bir sağlamadan devre dışı bırakır veya silerseniz, sorgular önbelleğin süresi dolana kadar geçici olarak çalışmaya devam edecektir. Ancak, arama hizmeti içeriğin şifresini çözemeyecek şekilde şu iletiyi alırsınız: "erişim yasak. Kullanılan sorgu anahtarı iptal edilmiş olabilir, lütfen yeniden deneyin. " 

## <a name="next-steps"></a>Sonraki adımlar

Azure Güvenlik mimarisi hakkında bilginiz varsa, [Azure Güvenlik belgelerini](../security/index.yml)gözden geçirin ve özellikle bu makalede:

> [!div class="nextstepaction"]
> [Bekleyen veri şifreleme](../security/fundamentals/encryption-atrest.md)