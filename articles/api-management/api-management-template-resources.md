---
title: Azure API Yönetimi şablon kaynakları | Microsoft Dokümanlar
description: Azure API Yönetimi'nde geliştirici portal şablonlarında kullanılmak üzere kullanılabilen kaynak türleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 51a1b4c6-a9fd-4524-9e0e-03a9800c3e94
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: c3b6123c63bf530463379a175745ef86baf2c5a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249509"
---
# <a name="azure-api-management-template-resources"></a>Azure API Yönetimi şablon kaynakları
Azure API Yönetimi, geliştirici portalı şablonlarında kullanılmak üzere aşağıdaki kaynak türlerini sağlar.  
  
-   [Yaylı çalgılar](#strings)  
  
-   [Glyph kaynakları](#glyphs)  

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="string-resources"></a><a name="strings"></a>Yaylı çalgılar  
 API Yönetimi, geliştirici portalında kullanılmak üzere kapsamlı bir dize kaynakları kümesi sağlar. Bu kaynaklar API Yönetimi tarafından desteklenen tüm dillerde yerelleştirilmiştir. Varsayılan şablon kümesi, bu kaynakları sayfa üstbilgileri, etiketler ve geliştirici portalında görüntülenen sabit dizeleri için kullanır. Şablonlarınızda bir dize kaynağı kullanmak için, aşağıdaki örnekte gösterildiği gibi, kaynak dize önekisini ve ardından dize adını sağlayın.  
  
```  
{% localized "Prefix|Name" %}  
  
```  
  
 Aşağıdaki örnek Ürün listesi şablonundan gelir ve **Ürünleri** sayfanın üst kısmında görüntüler.  
  
```  
<h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
  
```  
  
Aşağıdaki yerelleştirme seçenekleri desteklenir:

| Yerel Ayar    | Dil               |
|-----------|------------------------|
| "tr"      | "İngilizce"              |
| "cs"      | "Čeština"              |
| "de"      | "Deutsch"              |
| "es"      | "Español"              |
| "fr"      | "Français"             |
| "hu"      | "Magyar"               |
| "o"      | "Italiano"             |
| "ja-JP"   | "Japonca"                |
| "ko"      | "2010"                |
| "nl"      | "Nederlands"           |
| "pl"      | "Polski"               |
| "pt-br"   | "Português (Brezilya)"   |
| "pt-pt"   | "Português (Portekiz)" |
| "ru"      | "Русский"              |
| "sv"      | "Svenska"              |
| "tr"      | "Türkçe"               |
| "zh-hans" | "Japonca(japonca)"           |
| "zh-hant" | "Japonca(Japonca)"           |

 Geliştirici portalı şablonlarınızda kullanılabilecek dize kaynakları için aşağıdaki tablolara bakın. Tablo adını, bu tablodaki dize kaynakları için önek olarak kullanın.  
  
-   [ApisStrings](#ApisStrings)  
  
-   [UygulamaListStrings](#ApplicationListStrings)  
  
-   [AppDetailsStrings](#AppDetailsStrings)  
  
-   [AppStrings](#AppStrings)  
  
-   [Ortak Kaynaklar](#CommonResources)  
  
-   [CommonStrings](#CommonStrings)  
  
-   [Belgeler](#Documentation)  
  
-   [ErrorPageStrings](#ErrorPageStrings)  
  
-   [IssuesStrings](#IssuesStrings)  
  
-   [NotFoundStrings](#NotFoundStrings)  
  
-   [ÜrünDetailsStrings](#ProductDetailsStrings)  
  
-   [ÜrünlerStrings](#ProductsStrings)  
  
-   [SağlayıcıInfoStrings](#ProviderInfoStrings)  
  
-   [SigninResources](#SigninResources)  
  
-   [SigninStrings](#SigninStrings)  
  
-   [Kayıt KatıRlar](#SignupStrings)  
  
-   [AbonelikListStrings](#SubscriptionListStrings)  
  
-   [AbonelikStrings](#SubscriptionStrings)  
  
-   [UpdateProfileStrings](#UpdateProfileStrings)  
  
-   [UserProfile](#UserProfile)  
  
###  <a name="apisstrings"></a><a name="ApisStrings"></a>ApisStrings  
  
|Adı|Metin|  
|----------|----------|  
|SayfaBaşlığıApis|API'ler|  
  
###  <a name="appdetailsstrings"></a><a name="AppDetailsStrings"></a>AppDetailsStrings  
  
|Adı|Metin|  
|----------|----------|  
|WebApplicationsDetailsBaşlık|Uygulama önizlemesi|  
|WebApplicationsRequirementsHeader|Gereksinimler|  
|WebApplicationsScreenshotAlt|Ekran görüntüsü|  
|WebApplicationsScreenshotsHeader|Ekran görüntüleri|  
  
###  <a name="applicationliststrings"></a><a name="ApplicationListStrings"></a>UygulamaListStrings  
  
|Adı|Metin|  
|----------|----------|  
|WebDevelopersAppDeleteConfirmation|Uygulamayı kaldırmak istediğinizden emin misiniz?|  
|WebDevelopersAppNotPublished|Yayınlanmadı|  
|WebDevelopersAppNotSubmitted|Gönderilmez|  
|WebDevelopersAppTableKategoriHeader|Kategori|  
|WebDevelopersAppTableNameHeader|Adı|  
|WebDevelopersAppTableStateHeader|Durum|  
|WebDevelopersEditLink|Düzenle|  
|WebDevelopersRegisterAppLink|Uygulamayı kaydetme|  
|WebDevelopersRemoveLink|Kaldır|  
|WebDevelopersSubmitLink|Gönder|  
|WebDevelopersYourApplicationsHeader|Uygulamalarınız|  
  
###  <a name="appstrings"></a><a name="AppStrings"></a>AppStrings  
  
|Adı|Metin|  
|----------|----------|  
|WebApplicationsHeader|Uygulamalar|  
  
###  <a name="commonresources"></a><a name="CommonResources"></a>Ortak Kaynaklar  
  
|Adı|Metin|  
|----------|----------|  
|NoItemsToDisplay|Sonuç bulunamadı.|  
|GenelÖzel Mesaj|Yolunda olmayan bir şeyler var. Geçici bir aksaklık ya da böcek olabilir. Lütfen, tekrar dene.|  
|GeneralJsonExceptionMessage|Yolunda olmayan bir şeyler var. Geçici bir aksaklık ya da böcek olabilir. Lütfen, sayfayı yeniden yükleyin ve yeniden deneyin.|  
|ConfirmationMessageUnsavedChanges|Bazı kaydedilmemiş değişiklikler vardır. Değişiklikleri iptal edip atmak istediğinizden emin misiniz?|  
|AzureActiveDirectory|Azure Active Directory|  
|httplargeRequestMessage|Http İstek Vücut çok büyük.|  
  
###  <a name="commonstrings"></a><a name="CommonStrings"></a>CommonStrings  
  
|Adı|Metin|  
|----------|----------|  
|ButtonLabelİptal|İptal|  
|ButtonLabelKaydet|Kaydet|  
|GenelÖzel Mesaj|Yolunda olmayan bir şeyler var. Geçici bir aksaklık ya da böcek olabilir. Lütfen, tekrar dene.|  
|NoItemsToDisplay|Görüntülenecek öğe yok.|  
|PagerButtonLabelİlk|İlk|  
|PagerButtonLabelSon|Son|  
|PagerButtonLabelSonraki|Sonraki|  
|PagerButtonLabelÖnceki|Önceki|  
|PagerLabelPageNOfM|Sayfa {0}/{1}|  
|ŞifreTooShort|Şifre çok kısa|  
|EmailAsPassword|E-postanızı parolanız olarak kullanmayın|  
|PasswordsameasUserName|Parolanız kullanıcı adınızı içeremez|  
|PasswordTwoCharacterClasses|Farklı karakter sınıfları kullanma|  
|PasswordTooManyTekrarlar|Çok fazla tekrar|  
|Şifre Dizileri Bulundu|Parolanız diziler içerir|  
|PagerLabelPageSize|Sayfa boyutu|  
|PerdeEtiket Yükleme|Yükleniyor...|  
|TablePlaceholderNothingToDisplay|Seçili dönem ve kapsam için veri yok|  
|ButtonLabelClose|Kapat|  
  
###  <a name="documentation"></a><a name="Documentation"></a>Belge  
  
|Adı|Metin|  
|----------|----------|  
|WebDokümantasyonGeçersizHeaderErrorMessage|Geçersiz üstbilgi{0}' '|  
|WebDokümantasyonGeçersizİstekErrorMessage|Geçersiz İstek URL'si|  
|TextboxLabelAccessToken|Erişim belirteci *|  
|DropdownOptionPrimaryKeyFormat|İlköğretim-{0}|  
|DropdownOptionSecondaryKeyFormat|İkincil-{0}|  
|WebDocumentationSubscriptionKeyText|Abonelik anahtarınız|  
|WebDocumentationTemplatesAddHeaders|Gerekli HTTP üstbilgilerini ekleme|  
|WebDocumentationTemplatesBasicAuthSample|Temel Yetkilendirme Örneği|  
|WebDocumentationTemplatesCurlForBasicAuth|Temel Yetkilendirme kullanımı için: --user {username}:{password}|  
|WebDocumentationTemplatesCurlValuesForPath|Yol parametreleri ({...}olarak gösterilir), abonelik anahtarınız ve sorgu parametreleri için değerler için değerleri belirtin|  
|WebDocumentationTemplatesDeveloperKey|Abonelik anahtarınızı belirtin|  
|WebDocumentationTemplatesJavaApache|Bu örnek, HTTP Bileşenleri'nden Apache HTTP istemcisini kullanır (http://hc.apache.org/httpcomponents-client-ga/)|  
|WebDocumentationTemplatesİsteğe BağlıParams|Gerektiğinde isteğe bağlı parametreler için değerleri belirtin|  
|WebDocumentationTemplatesPhpPackage|Bu örnek, HTTP_Request2 paketini kullanır. (daha fazla bilgi için:https://pear.php.net/package/HTTP_Request2)|  
|WebDocumentationTemplatesPythonValuesForPath|Yol parametreleri için değerleri belirtin ({...}olarak gösterilir) ve gerekirse istek gövdesi|  
|WebDocumentationTemplatesRequestBody|İstek gövdesini belirtin|  
|WebDocumentationTemplatesGerekliParams|Aşağıdaki gerekli parametreler için değerleri belirtin|  
|WebDocumentationTemplatesValuesForPath|Yol parametreleri için değerleri belirtin ({...}olarak gösterilmiştir)|  
|OAuth2AuthorizationEndpointDescription|Yetkilendirme bitiş noktası, kaynak sahibiyle etkileşim kurmak ve yetkilendirme hibesi almak için kullanılır.|  
|OAuth2AuthorizationEndpointName|Yetkilendirme bitiş noktası|  
|OAuth2TokenEndpointAçıklama|Belirteç bitiş noktası, yetkilendirme hibesini sunarak veya belirteç lerini yenileyerek bir erişim belirteci elde etmek için istemci tarafından kullanılır.|  
|OAuth2TokenEndpointName|Belirteç bitiş noktası|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Description|<\> p İstemci, kaynak sahibinin kullanıcı aracısını yetkilendirme bitiş noktasına yönlendirerek akışı başlatır.  İstemci, istemci tanımlayıcısını, istenen kapsamı, yerel durumu ve yetkilendirme sunucusunun erişim verildikten (veya reddedildikten) sonra kullanıcı aracısını geri göndereceği bir yeniden yönlendirme URI'sini içerir.     </p\> <\> p Yetkilendirme sunucusu kaynak sahibinin kimliğini (kullanıcı aracısı aracılığıyla) doğrular ve kaynak sahibinin istemcinin erişim isteğini hibe edip etmediğini veya reddettiğini belirler.     </p\> <\> p Kaynak sahibinin erişim sağladığını varsayarsak, yetkilendirme sunucusu kullanıcı aracısını daha önce sağlanan yeniden yönlendirme URI'yi kullanarak istemciye yönlendirir (istekte veya istemci kaydı sırasında).  Yeniden yönlendirme URI bir yetkilendirme kodu ve istemci tarafından daha önce sağlanan herhangi bir yerel durum içerir.     </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ErrorDescription|<\> p Kullanıcı isteğin geçersiz olup olmadığını niçin erişim isteğini reddederse, istemci yönlendirmeye eklenen aşağıdaki parametreleri kullanarak bilgilendirilir: </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Name|Yetkilendirme isteği|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_RequestDescription|<\> p İstemci uygulaması, OAuth işlemini başlatmak için kullanıcıyı yetkilendirme bitiş noktasına göndermelidir.          Yetkilendirme bitiş noktasında, kullanıcı kimlik doğrulaması verir ve ardından uygulamaya erişimi verir veya reddeder.     </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ResponseDescription|<\> p Kaynak sahibinin erişim sağladığını varsayarsak, yetkilendirme sunucusu kullanıcı aracısını daha önce sağlanan yeniden yönlendirme URI'yi kullanarak istemciye geri yönlendirir (istekte veya istemci kaydı sırasında).  Yeniden yönlendirme URI bir yetkilendirme kodu ve istemci tarafından daha önce sağlanan herhangi bir yerel durum içerir. </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_Description|<\> p İstemci, önceki adımda alınan yetkilendirme kodunu ekleyerek yetkilendirme sunucusunun belirteç bitiş noktasından bir erişim belirteci ister.  İstek tevekkül ederken, istemci yetkilendirme sunucusuyla kimlik doğrulaması sağlar.  İstemci, doğrulama için yetkilendirme kodunu elde etmek için kullanılan yeniden yönlendirme URI'yi içerir. </p\> <\> p Yetkilendirme sunucusu istemcinin kimliğini doğrular, yetkilendirme kodunu doğrular ve URI'nin aldığı yeniden yönlendirmenin istemciyi adım (C) olarak yönlendirmek için kullanılan URI ile eşleşmesini sağlar.  Geçerliyse, yetkilendirme sunucusu erişim belirteci ve isteğe bağlı olarak yenileme belirteci ile yanıt verir. </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ErrorDescription|<\> p İstek istemcikimlik doğrulaması başarısız olduysa veya geçersizse, yetkilendirme sunucusu http 400 (Kötü İstek) durum koduyla yanıt verir (aksi belirtilmedikçe) ve yanıtla birlikte aşağıdaki parametreleri içerir. </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_RequestDescription|<\> p İstemci, http istek varlık gövdesinde UTF-8 karakter kodlaması ile "application/x-www-form-urlencoded" biçimini kullanarak aşağıdaki parametreleri göndererek belirteç bitiş noktasına bir istekte bulunuyor. </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ResponseDescription|<\> p Yetkilendirme sunucusu erişim belirteci ve isteğe bağlı yenileme belirteci sorunları ve 200 (Tamam) durum kodu ile HTTP yanıtının varlık gövdesine aşağıdaki parametreleri ekleyerek yanıtı oluşturuyor. </p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_Description|<\> p İstemci yetkilendirme sunucusuyla kimlik doğrular ve belirteç bitiş noktasından bir erişim belirteci ister. </p\> <\> p yetkilendirme sunucusu istemcinin kimliğini doğrular ve geçerliyse erişim belirteci sağlar. </p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ErrorDescription|<\> p İstek istemci kimlik doğrulaması başarısız olursa veya geçersiz ise yetkilendirme sunucusu http 400 (Kötü İstek) durum kodu yla yanıt verir (aksi belirtilmedikçe) ve yanıtla birlikte aşağıdaki parametreleri içerir. </p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_RequestDescription|<\> p İstemci, HTTP istek varlık gövdesinde UTF-8 karakter kodlaması ile "application/x-www-form-urlencoded" biçimini kullanarak aşağıdaki parametreleri ekleyerek belirteç bitiş noktasına bir istekte bulunuyor. </p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ResponseDescription|<\> p Erişim belirteci isteği geçerli ve yetkiliyse, yetkilendirme sunucusu bir erişim belirteci ve isteğe bağlı yenileme belirteci yayınlar ve 200 (Tamam) durum koduyla HTTP yanıtının varlık gövdesine aşağıdaki parametreleri ekleyerek yanıtı sağlar. </p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_Description|<\> p İstemci, kaynak sahibinin kullanıcı aracısını yetkilendirme bitiş noktasına yönlendirerek akışı başlatır.  İstemci, istemci tanımlayıcısını, istenen kapsamı, yerel durumu ve yetkilendirme sunucusunun erişim verildikten (veya reddedildikten) sonra kullanıcı aracısını geri göndereceği bir yeniden yönlendirme URI'sini içerir. </p\> <\> p Yetkilendirme sunucusu kaynak sahibinin kimliğini doğrular (kullanıcı aracısı aracılığıyla) ve kaynak sahibinin istemcinin erişim isteğini hibe edip etmediğini veya reddedip etmediğini belirler. </p\> <\> p Kaynak sahibinin erişim sağladığını varsayarsak, yetkilendirme sunucusu daha önce sağlanan yeniden yönlendirme URI'yi kullanarak kullanıcı aracısını istemciye yönlendirir.  Yeniden yönlendirme URI URI parçasında erişim belirteci içerir. </p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ErrorDescription|<\> p Kaynak sahibi erişim isteğini reddederse veya istek eksik veya geçersiz yeniden yönlendirme URI dışında nedenlerle başarısız olursa, yetkilendirme sunucusu "application/x-www-form-urlencoded" biçimini kullanarak yeniden yönlendirme URI'nin parça bileşenine aşağıdaki parametreleri ekleyerek istemciyi bilgilendirir. </p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_RequestDescription|<\> p İstemci uygulaması, OAuth işlemini başlatmak için kullanıcıyı yetkilendirme bitiş noktasına göndermelidir.      Yetkilendirme bitiş noktasında, kullanıcı kimlik doğrulaması verir ve ardından uygulamaya erişimi verir veya reddeder. </p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ResponseDescription|<\> p Kaynak sahibi erişim isteğini verirse, yetkilendirme sunucusu erişim belirteci verir ve "application/x-www-form-urlencoded" biçimini kullanarak yeniden yönlendirme URI'nin parça bileşenine aşağıdaki parametreleri ekleyerek istemciye teslim eder. </p\>|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Description|Yetkilendirme kodu akışı, kimlik bilgilerinin gizliliğini koruyabilen istemciler için optimize edilmiştir (örneğin, PHP, Java, Python, Ruby, ASP.NET, vb. kullanılarak uygulanan web sunucusu uygulamaları.|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Name|Yetkilendirme Kodu hibe|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Description|İstemci kimlik bilgileri akışı, istemcinin (uygulamanızın) denetimi altındaki korumalı kaynaklara erişim talep ettiği durumlarda uygundur. İstemci bir kaynak sahibi olarak kabul edilir, bu nedenle son kullanıcı etkileşimi gerekmez.|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Name|İstemci Kimlik Bilgileri hibesi|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Description|Örtük akış, belirli bir yeniden yönlendirme URI'yi çalıştırtığı bilinen kimlik bilgilerinin gizliliğini koruyamayan istemciler için optimize edilebiyi optimize eder. Bu istemciler genellikle JavaScript gibi bir komut dosyası dili kullanılarak bir tarayıcıda uygulanır.|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Name|Örtülü hibe|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Description|Kaynak sahibi parola kimlik bilgileri akışı, kaynak sahibinin istemciyle (uygulamanızla) aygıt işletim sistemi veya yüksek ayrıcalıklı bir uygulama gibi güven ilişkisi olduğu durumlarda uygundur. Bu akış, kaynak sahibinin kimlik bilgilerini (genellikle etkileşimli bir form kullanarak kullanıcı adı ve parola) alabilen istemciler için uygundur.|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Name|Kaynak Sahibi Şifre Kimlik Bilgileri hibesi|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_Description|<\> p Kaynak sahibi istemciye kullanıcı adı ve parolasını sağlar. </p\> <\> p İstemci, kaynak sahibinden alınan kimlik bilgilerini ekleyerek yetkilendirme sunucusunun belirteç bitiş noktasından bir erişim belirteci ister.  İstek tevekkül ederken, istemci yetkilendirme sunucusuyla kimlik doğrulaması sağlar. </p\> <\> p Yetkilendirme sunucusu istemcinin kimliğini doğrular ve kaynak sahibikimlik bilgilerini doğrular ve geçerliyse, bir erişim belirteci verir. </p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ErrorDescription|<\> p İstek istemci kimlik doğrulaması başarısız olursa veya geçersiz ise yetkilendirme sunucusu http 400 (Kötü İstek) durum kodu yla yanıt verir (aksi belirtilmedikçe) ve yanıtla birlikte aşağıdaki parametreleri içerir. </p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_RequestDescription|<\> p İstemci, HTTP istek varlık gövdesinde UTF-8 karakter kodlaması ile "application/x-www-form-urlencoded" biçimini kullanarak aşağıdaki parametreleri ekleyerek belirteç bitiş noktasına bir istekte bulunuyor. </p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ResponseDescription|<\> p Erişim belirteci isteği geçerli ve yetkiliyse, yetkilendirme sunucusu bir erişim belirteci ve isteğe bağlı yenileme belirteci yayınlar ve 200 (Tamam) durum koduyla HTTP yanıtının varlık gövdesine aşağıdaki parametreleri ekleyerek yanıtı sağlar. </p\>|  
|OAuth2Step_AccessTokenRequest_Name|Erişim belirteç isteği|  
|OAuth2Step_AuthorizationRequest_Name|Yetkilendirme isteği|  
|OAuth2AccessToken_AuthorizationCodeGrant_TokenResponse|Gerekli. Yetkilendirme sunucusu tarafından verilen erişim belirteci.|  
|OAuth2AccessToken_ClientCredentialsGrant_TokenResponse|Gerekli. Yetkilendirme sunucusu tarafından verilen erişim belirteci.|  
|OAuth2AccessToken_ImplicitGrant_AuthorizationResponse|Gerekli. Yetkilendirme sunucusu tarafından verilen erişim belirteci.|  
|OAuth2AccessToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|Gerekli. Yetkilendirme sunucusu tarafından verilen erişim belirteci.|  
|OAuth2ClientId_AuthorizationCodeGrant_AuthorizationRequest|Gerekli. İstemci tanımlayıcısı.|  
|OAuth2ClientId_AuthorizationCodeGrant_TokenRequest|İstemci yetkilendirme sunucusu ile kimlik doğrulaması değilse GEREKLI.|  
|OAuth2ClientId_ImplicitGrant_AuthorizationRequest|Gerekli. İstemci tanımlayıcısı.|  
|OAuth2Code_AuthorizationCodeGrant_AuthorizationResponse|Gerekli. Yetkilendirme sunucusu tarafından oluşturulan yetkilendirme kodu.|  
|OAuth2Code_AuthorizationCodeGrant_TokenRequest|Gerekli. Yetkilendirme sunucusundan alınan yetkilendirme kodu.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_AuthorizationErrorResponse|Isteğe bağlı. Ek bilgi sağlayan insan tarafından okunabilir ASCII metni.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_TokenErrorResponse|Isteğe bağlı. Ek bilgi sağlayan insan tarafından okunabilir ASCII metni.|  
|OAuth2ErrorDescription_ClientCredentialsGrant_TokenErrorResponse|Isteğe bağlı. Ek bilgi sağlayan insan tarafından okunabilir ASCII metni.|  
|OAuth2ErrorDescription_ImplicitGrant_AuthorizationErrorResponse|Isteğe bağlı. Ek bilgi sağlayan insan tarafından okunabilir ASCII metni.|  
|OAuth2ErrorDescription_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|Isteğe bağlı. Ek bilgi sağlayan insan tarafından okunabilir ASCII metni.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_AuthorizationErrorResponse|Isteğe bağlı. Bir URI hata hakkında bilgi ile insan tarafından okunabilir bir web sayfası tanımlayan.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_TokenErrorResponse|Isteğe bağlı. Bir URI hata hakkında bilgi ile insan tarafından okunabilir bir web sayfası tanımlayan.|  
|OAuth2ErrorUri_ClientCredentialsGrant_TokenErrorResponse|Isteğe bağlı. Bir URI hata hakkında bilgi ile insan tarafından okunabilir bir web sayfası tanımlayan.|  
|OAuth2ErrorUri_ImplicitGrant_AuthorizationErrorResponse|Isteğe bağlı. Bir URI hata hakkında bilgi ile insan tarafından okunabilir bir web sayfası tanımlayan.|  
|OAuth2ErrorUri_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|Isteğe bağlı. Bir URI hata hakkında bilgi ile insan tarafından okunabilir bir web sayfası tanımlayan.|  
|OAuth2Error_AuthorizationCodeGrant_AuthorizationErrorResponse|Gerekli. Aşağıdakilerden tek bir ASCII hata kodu: invalid_request, unauthorized_client, access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_AuthorizationCodeGrant_TokenErrorResponse|Gerekli. Aşağıdakilerden tek bir ASCII hata kodu: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ClientCredentialsGrant_TokenErrorResponse|Gerekli. Aşağıdakilerden tek bir ASCII hata kodu: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ImplicitGrant_AuthorizationErrorResponse|Gerekli. Aşağıdakilerden tek bir ASCII hata kodu: invalid_request, unauthorized_client, access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|Gerekli. Aşağıdakilerden tek bir ASCII hata kodu: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2ExpiresIn_AuthorizationCodeGrant_TokenResponse|Önerilen. Erişim jetonunun saniyecinsinden ömrü.|  
|OAuth2ExpiresIn_ClientCredentialsGrant_TokenResponse|Önerilen. Erişim jetonunun saniyecinsinden ömrü.|  
|OAuth2ExpiresIn_ImplicitGrant_AuthorizationResponse|Önerilen. Erişim jetonunun saniyecinsinden ömrü.|  
|OAuth2ExpiresIn_ResourceOwnerPasswordCredentialsGrant_TokenResponse|Önerilen. Erişim jetonunun saniyecinsinden ömrü.|  
|OAuth2GrantType_AuthorizationCodeGrant_TokenRequest|Gerekli. Değer "authorization_code" olarak ayarlanmalıdır.|  
|OAuth2GrantType_ClientCredentialsGrant_TokenRequest|Gerekli. Değer "client_credentials" olarak ayarlanmalıdır.|  
|OAuth2GrantType_ResourceOwnerPasswordCredentialsGrant_TokenRequest|Gerekli. Değer "parola" olarak ayarlanmalıdır.|  
|OAuth2Password_ResourceOwnerPasswordCredentialsGrant_TokenRequest|Gerekli. Kaynak sahibi parolası.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_AuthorizationRequest|Isteğe bağlı. Yeniden yönlendirme bitiş noktası URI mutlak bir URI olmalıdır.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_TokenRequest|"redirect_uri" parametresi yetkilendirme isteğine dahil edildiyse ve değerleri aynı ysa gerekli.|  
|OAuth2RedirectUri_ImplicitGrant_AuthorizationRequest|Isteğe bağlı. Yeniden yönlendirme bitiş noktası URI mutlak bir URI olmalıdır.|  
|OAuth2RefreshToken_AuthorizationCodeGrant_TokenResponse|Isteğe bağlı. Yeni erişim belirteçleri elde etmek için kullanılabilecek yenileme belirteci.|  
|OAuth2RefreshToken_ClientCredentialsGrant_TokenResponse|Isteğe bağlı. Yeni erişim belirteçleri elde etmek için kullanılabilecek yenileme belirteci.|  
|OAuth2RefreshToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|Isteğe bağlı. Yeni erişim belirteçleri elde etmek için kullanılabilecek yenileme belirteci.|  
|OAuth2ResponseType_AuthorizationCodeGrant_AuthorizationRequest|Gerekli. Değer "kod" olarak ayarlanmalıdır.|  
|OAuth2ResponseType_ImplicitGrant_AuthorizationRequest|Gerekli. Değer "belirteç" olarak ayarlanmalıdır.|  
|OAuth2Scope_AuthorizationCodeGrant_AuthorizationRequest|Isteğe bağlı. Erişim isteğinin kapsamı.|  
|OAuth2Scope_AuthorizationCodeGrant_TokenResponse|İstemci tarafından talep edilen kapsamla aynıysa, İstEĞE BAĞLI; aksi takdirde, GEREKLİDİr.|  
|OAuth2Scope_ClientCredentialsGrant_TokenRequest|Isteğe bağlı. Erişim isteğinin kapsamı.|  
|OAuth2Scope_ClientCredentialsGrant_TokenResponse|İsteğE BAĞLı, istemci tarafından talep edilen kapsam ile aynı ise; aksi takdirde, GEREKLİDİr.|  
|OAuth2Scope_ImplicitGrant_AuthorizationRequest|Isteğe bağlı. Erişim isteğinin kapsamı.|  
|OAuth2Scope_ImplicitGrant_AuthorizationResponse|İstemci tarafından talep edilen kapsamla aynıysa, İstEĞE BAĞLI; aksi takdirde, GEREKLİDİr.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenRequest|Isteğe bağlı. Erişim isteğinin kapsamı.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenResponse|İsteğE BAĞLı, istemci tarafından talep edilen kapsam ile aynı ise; aksi takdirde, GEREKLİDİr.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationErrorResponse|İstemci yetkilendirme isteğinde "durum" parametresi varsa GEREKLI.  İstemciden alınan tam değer.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationRequest|Önerilen. İstem ve geri çağırma arasındaki durumu korumak için istemci tarafından kullanılan opak bir değer.  Yetkilendirme sunucusu, kullanıcı aracısını istemciye geri yönlendirirken bu değeri içerir.  Parametre, yer ler arası istek sahteciliğini önlemek için kullanılmalıdır.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationResponse|İstemci yetkilendirme isteğinde "durum" parametresi varsa GEREKLI.  İstemciden alınan tam değer.|  
|OAuth2State_ImplicitGrant_AuthorizationErrorResponse|İstemci yetkilendirme isteğinde "durum" parametresi varsa GEREKLI.  İstemciden alınan tam değer.|  
|OAuth2State_ImplicitGrant_AuthorizationRequest|Önerilen. İstem ve geri çağırma arasındaki durumu korumak için istemci tarafından kullanılan opak bir değer.  Yetkilendirme sunucusu, kullanıcı aracısını istemciye geri yönlendirirken bu değeri içerir.  Parametre, yer ler arası istek sahteciliğini önlemek için kullanılmalıdır.|  
|OAuth2State_ImplicitGrant_AuthorizationResponse|İstemci yetkilendirme isteğinde "durum" parametresi varsa GEREKLI.  İstemciden alınan tam değer.|  
|OAuth2TokenType_AuthorizationCodeGrant_TokenResponse|Gerekli. Verilen belirteç türü.|  
|OAuth2TokenType_ClientCredentialsGrant_TokenResponse|Gerekli. Verilen belirteç türü.|  
|OAuth2TokenType_ImplicitGrant_AuthorizationResponse|Gerekli. Verilen belirteç türü.|  
|OAuth2TokenType_ResourceOwnerPasswordCredentialsGrant_TokenResponse|Gerekli. Verilen belirteç türü.|  
|OAuth2UserName_ResourceOwnerPasswordCredentialsGrant_TokenRequest|Gerekli. Kaynak sahibi kullanıcı adı.|  
|OAuth2DesteksizTokenType|Belirteç{0}türü ' ' ' desteklenmez.|  
|OAuth2Geçersiz Durum|Yetkilendirme sunucusundan geçersiz yanıt|  
|OAuth2GrantType_AuthorizationCode|Yetkilendirme kodu|  
|OAuth2GrantType_Implicit|Örtülü|  
|OAuth2GrantType_ClientCredentials|İstemci kimlik bilgileri|  
|OAuth2GrantType_ResourceOwnerPassword|Kaynak sahibi parolası|  
|WebDocumentation302Kod|302 Bulundu|  
|WebDocumentation400Code|400 (Kötü istek)|  
|OAuth2SendingMethod_AuthHeader|Yetkilendirme üstbilgi|  
|OAuth2SendingMethod_QueryParam|Sorgu parametresi|  
|OAuth2AuthorizationServerGeneralException|Erişime izin verirken bir hata oluştu{0}|  
|OAuth2AuthorizationServerCommunicationException|Yetkilendirme sunucusuna bir HTTP bağlantısı kurulamadı veya beklenmedik bir şekilde kapatıldı.|  
|WebDocumentationOAuth2GenelHataMesajı|Beklenmeyen hata oluştu.|  
|YetkilendirmeServerCommunicationException|Yetkilendirme sunucusu iletişimi özel durumu oluştu. Lütfen yöneticiye başvurun.|  
|TextblockAbonelikKeyHeaderAçıklama|Bu API'ye erişim sağlayan abonelik anahtarı. <bulunan bir href ='/developer'\>Profil\></a .|  
|TextblockOAuthHeaderAçıklama|OAuth 2.0 erişim belirteci\> {0} <\>i< elde edilen . Desteklenen hibe türleri:\> {1} <i\></i .|  
|TextblockContentTypeHeaderDescription|API'ye gönderilen gövdenin ortam türü.|  
|ErrorMessageApiAncak'a Ulaşılamıyor|Aramaya çalıştığınız API'ye şu anda erişilemez. Lütfen burada</a\>\><API yayımcısına başvurun.|  
|ErrorMessageApiTimedout|Aramaya çalıştığınız API'nin yanıtı geri alması normalden daha uzun sürüyor. Lütfen burada</a\>\><API yayımcısına başvurun.|  
|BadRequestParameterBeklenen|"'{0}' parametresi bekleniyor"|  
|TooltipTextDoubleClickToSelectAll|Tümünü seçmek için çift tıklatın.|  
|TooltipTextHideRevealSecret|Göster/Gizle|  
|ButtonLinkOpenConsole|Deneyin|  
|BölümBaşlığıİstek Gövdesi|İstek gövdesi|  
|BölümBaşlığıİstek Parametreleri|İstek parametreleri|  
|BölümBaşlığıRequestUrl|İstek URL'si|  
|BölümBaşlığıYanıtı|Yanıt|  
|BölümBaşlığıİstek Headers|İstek üst bilgileri|  
|FormLabelAltmetinİsteğe Bağlı|isteğe bağlı|  
|BölümBaşlığıKod Örnekleri|Kod örnekleri|  
|TextblockOpenidConnectHeaderAçıklama|OpenID Connect kimlik belirteci\> {0} <\>i</i elde . Desteklenen hibe türleri:\> {1} <i\></i .|  
  
###  <a name="errorpagestrings"></a><a name="ErrorPageStrings"></a>ErrorPageStrings  
  
|Adı|Metin|  
|----------|----------|  
|LinkLabelBack|seçin|  
|LinkLabelAna Sayfa|ana sayfa|  
|LinkLabelSendUsEmail|Bize bir e-posta gönderin|  
|Sayfa BaşlığıHatası|Üzgünüz, istenen sayfaya hizmet veren bir sorun vardı|  
|TextblockPotentialCauseIntermittentIssue|Bu, zaten gitmiş bir aralıklı veri erişim sorunu olabilir.|  
|TextblockPotentialCauseOldLink|Tıkladığınız bağlantı eski olabilir ve artık doğru konumu işaret etmeyebilir.|  
|TextblockPotentialCauseTeknikProblem|Bizim tarafımızda teknik bir sorun olabilir.|  
|TextblockPotentialSolutionRefresh|Sayfayı yenilemeyi deneyin.|  
|TextblockPotentialSolutionStartOver|Baştan {0}başlayalım.|  
|TextblockPotentialSolutionTryAgain|Gidin {0} ve gerçekleştirdiğiniz eylemi yeniden deneyin.|  
|TextReportProblem|{0}neyin yanlış gittiğini tarif ediyor ve en kısa zamanda bakacağız.|  
|BaşlıkPotansiyel Neden|Olası neden|  
|TitlePotentialSolution|Muhtemelen sadece geçici bir sorun, denemek için birkaç şey|  
  
###  <a name="issuesstrings"></a><a name="IssuesStrings"></a>IssuesStrings  
  
|Adı|Metin|  
|----------|----------|  
|WebIssuesIndexTitle|Sorunlar|  
|WebIssuesNoActiveSubscriptions|Etkin aboneliğiniz yok. Bir sorunu bildirmek için bir ürün için abone olmanız gerekir.|  
|WebIssuesNotSignin|Sen oturum etmedin. Lütfen {0} bir sorunu bildirmek veya yorum göndermek için.|  
|WebIssuesReportIssueButton|Rapor Sorunu|  
|WebIssuesSignin|oturum aç|  
|WebIssuesStatusReportedby|Durum: {0} &#124; Tarafından Bildirilen{1}|  
  
###  <a name="notfoundstrings"></a><a name="NotFoundStrings"></a>NotFoundStrings  
  
|Adı|Metin|  
|----------|----------|  
|LinkLabelAna Sayfa|ana sayfa|  
|LinkLabelSendUsEmail|bize bir e-posta göndermek|  
|PageTitleNotFound|Üzgünüz, aradığınız sayfayı bulamıyoruz|  
|TextblockPotentialCauseMisspelledUrl|Url'yi yazdıysanız yanlış yazmış olabilirsiniz.|  
|TextblockPotentialCauseOldLink|Tıkladığınız bağlantı eski olabilir ve artık doğru konumu işaret etmeyebilir.|  
|TextblockPotentialSolutionRetype|URL'yi yeniden yazmayı deneyin.|  
|TextblockPotentialSolutionStartOver|Baştan {0}başlayalım.|  
|TextReportProblem|{0}neyin yanlış gittiğini tarif ediyor ve en kısa zamanda bakacağız.|  
|BaşlıkPotansiyel Neden|Olası neden|  
|TitlePotentialSolution|Potansiyel çözüm|  
  
###  <a name="productdetailsstrings"></a><a name="ProductDetailsStrings"></a>ÜrünDetailsStrings  
  
|Adı|Metin|  
|----------|----------|  
|WebProductsAgreement|Ürün'e {0} abone olarak, ben kabul `<a data-toggle='modal' href='#legal-terms'\>Terms of Use</a\>`ediyorum .|  
|WebProductsLegalTermsLink|Kullanım Koşulları|  
|WebProductsAbone Düğmesi|Abone Ol|  
|WebProductsUsageLimitsHeader|Kullanım sınırları|  
|WebProductsYouAreNotAbone|Bu ürüne abonesiniz.|  
|WebProductsYouRequestedAbonelik|Bu ürüne abonelik istediniz.|  
|ErrorYouneedtoagreeLegalTerms|Devam etmeden önce Kullanım Koşullarını kabul etmelisiniz.|  
|ButtonLabelAddSubscription|Abonelik ekleme|  
|LinkLabelChangeSubscriptionName|Değiştir|  
|ButtonLabelOnay|Confirm|  
|TextblockMultipleSubscriptionsSayısı|Bu {0} ürüne abonelikleri var:|  
|TextblockTekAboneliksayısı|Bu {0} ürüne abonesiniz:|  
|TextblockSingleApisSayısı|Bu ürün {0} API içerir:|  
|TextblockMultipleApisSayısı|Bu ürün {0} API'ler içerir:|  
|TextblockHeaderAbone|Ürüne abone ol|  
|TextblockAbonelikAçıklama|Yeni bir abonelik aşağıdaki gibi oluşturulur:|  
|TextblockSubscriptionLimitReached|Aboneliksınırına ulaşıldı.|  
  
###  <a name="productsstrings"></a><a name="ProductsStrings"></a>ÜrünlerStrings  
  
|Adı|Metin|  
|----------|----------|  
|Sayfa Başlığı Ürünler|Ürünler|  
  
###  <a name="providerinfostrings"></a><a name="ProviderInfoStrings"></a>SağlayıcıInfoStrings  
  
|Adı|Metin|  
|----------|----------|  
|TextboxExternalIdentitiesDevre Dışı|Oturum açma şu anda yöneticiler tarafından devre dışı bırakılır.|  
|TextboxExternalIdentitiesSigninInvitation|Alternatif olarak, oturum|  
|TextboxExternalIdentitiesSigninInvitationPrimary|Oturum şu şekilde oturum açın:|  
  
###  <a name="signinresources"></a><a name="SigninResources"></a>SigninResources  
  
|Adı|Metin|  
|----------|----------|  
|PrincipalNotFound|Asıl sorumlu bulunamadı veya imza geçersiz|  
|ErrorSsoAuthenticationFailed|SSO kimlik doğrulaması başarısız oldu|  
|ErrorSsoAuthenticationFailedDetailed|Verilen geçersiz belirteç veya imza doğrulanamaz.|  
|HataSsoTokenGeçersiz|SSO belirteci geçersizdir|  
|DoğrulamaHatasıSpecificEmailZaten Var|E-posta ' '{0}' zaten kayıtlı|  
|DoğrulamaHatasıSpecificEmailGeçersiz|E-posta ' '{0}' ' geçersizdir|  
|DoğrulamaHatasıParolaGeçersiz|Parola geçersizdir. Lütfen hataları düzeltin ve yeniden deneyin.|  
|PropertyTooShort|{0}çok kısa|  
|WebAuthenticationAddresserEmailInvalidErrorMessage|Geçersiz e-posta adresi.|  
|DoğrulamaMessageNewPasswordConfirmationGerekli|Yeni parolayı onaylama|  
|DoğrulamaHatasıParolaOnayı Gerekli|Parolanın boş olduğunu onaylayın|  
|WebAuthenticationEmailChangeNotice|Onay e-postasını değiştir {0}in' e doğru yolda. Yeni e-posta adresinizi onaylamak için lütfen içindeki talimatları uygulayın. E-posta önümüzdeki birkaç dakika içinde gelen kutunuza ulaşmazsa, lütfen önemsiz e-posta klasörünüzü kontrol edin.|  
|WebAuthenticationEmailChangeNoticeHeader|E-posta değiştirme isteğiniz başarıyla işlendi|  
|WebAuthenticationEmailChangeNoticeTitle|İstenilen e-posta değişikliği|  
|WebAuthenticationEmailHasBeenRevertedNotice|E-posta zaten var. İstek geri alındı|  
|DoğrulamaErrorEmailAlreadyExists|E-posta zaten var|  
|DoğrulamaHatasıEmailGeçersiz|Geçersiz e-posta adresi|  
|TextboxLabelEmail|Email|  
|DoğrulamaHatasıEmailGerekli|E-posta gereklidir.|  
|WebAuthenticationErrorNoticeHeader|Hata|  
|WebAuthenticationFieldLengthErrorMessage|{0}maksimum uzunluk olmalıdır{1}|  
|TextboxLabelEmailİlkName|Ad|  
|DoğrulamaHatasıİlkNameGerekli|Ad gereklidir.|  
|DoğrulamaHatasıFirstNameGeçersiz|Geçersiz ad|  
|BildirimGeçersizDavetiyeToken|Onay bağlantılarının sadece 48 saat boyunca geçerli olduğunu lütfen unutmayın. Bu zaman dilimi içindeyseniz, lütfen bağlantınızın doğru olduğundan emin olun. Bağlantınızın süresi dolduysa, lütfen onaylamaya çalıştığınız eylemi tekrarlayın.|  
|NoticeHeaderGeçersiz DavetiyeToken|Geçersiz davet belirteci|  
|BildirimBaşlığıGeçersiz DavetiyeToken|Onay hatası|  
|WebAuthenticationLastNameInvalidErrorMessage|Geçersiz soyadı|  
|TextboxLabelEmailLastName|Soyadı|  
|DoğrulamaErrorLastNameGerekli|Soyadı gereklidir.|  
|WebAuthenticationLinkExpiredNotice|Size gönderilen onay bağlantısının süresi doldu. `<a href={0}?token={1}>Resend confirmation email.</a\>`|  
|BildirimPasswordResetLinkGeçersiz OrExpired|Parola sıfırlama bağlantınız geçersiz veya süresi doldu.|  
|WebAuthenticationLinkExpiredNoticeTitle|Gönderilen bağlantı|  
|WebAuthenticationNewPasswordLabel|Yeni parola|  
|DoğrulamaMessageNewPasswordGerekli|Yeni parola gereklidir.|  
|TextboxLabelBildirimlerSenderEmail|Bildirim gönderen e-posta|  
|TextboxLabelOrganizasyonAdı|Kuruluş adı|  
|WebAuthenticationOrganizationGerekliErrorMessage|Kuruluş adı boş|  
|WebAuthenticationPasswordChangedNotice|Parolanız başarıyla güncellendi|  
|WebAuthenticationPasswordChangedNoticeTitle|Parola güncellendi|  
|WebAuthenticationPasswordCompareErrorMessage|Parolalar eşleşmiyor|  
|WebAuthenticationPasswordConfirmLabel|Parolayı onayla|  
|DoğrulamaHatasıParolaGeçersiz Ayrıntılı|Parola çok zayıf.|  
|WebAuthenticationPasswordLabel|Parola|  
|DoğrulamaHatasıŞifre Gerekli|Parola gereklidir.|  
|WebAuthenticationPasswordResetSendNotice|Parola onay e-postasını {0}değiştirin. Şifre değiştirme işlemine devam etmek için lütfen e-postadaki talimatları uygulayın.|  
|WebAuthenticationPasswordResetSendNoticeHeader|Parola sıfırlama isteğiniz başarıyla işlendi|  
|WebAuthenticationPasswordResetSendNoticeTitle|İstenilen parola sıfırlama|  
|WebAuthenticationRequestNotFoundNotice|İstek bulunamadı|  
|WebAuthenticationSenderEmailRequiredErrorMessage|Bildirim gönderen e-posta boş|  
|WebAuthenticationSigninPasswordLabel|Lütfen bir parola girerek değişikliği onaylayın|  
|WebAuthenticationSignupConfirmNotice|Kayıt onay e-postası {0}.<\> br / Hesabınızı etkinleştirmek için e-posta\> içindeki talimatları izleyin.<br / E-posta önümüzdeki birkaç dakika içinde gelen kutunuzda gelmezse, lütfen önemsiz e-posta klasörünüzü kontrol edin.|  
|WebAuthenticationSignupConfirmNoticeHeader|Hesabınız başarıyla oluşturuldu|  
|WebAuthenticationSignupConfirmNoticeRepeatHeader|Kayıt onay e-postası tekrar gönderildi|  
|WebAuthenticationSignupConfirmNoticeTitle|Oluşturulan hesap|  
|WebAuthenticationTokenRequiredErrorMessage|Belirteç boş|  
|WebAuthenticationUserAlreadyRegisteredNotice|Bu e-posta ile bir kullanıcı zaten sistemde kayıtlı gibi görünüyor. Parolanızı unuttuysanız, lütfen geri yüklemeyi deneyin veya destek ekibimizle iletişime geçin.|  
|WebAuthenticationUserAlreadyRegisteredNoticeHeader|Kullanıcı zaten kayıtlı|  
|WebAuthenticationUserAlreadyRegisteredNoticeTitle|Zaten kayıtlı|  
|ButtonLabelChangePassword|Parolayı değiştir|  
|ButtonLabelChangeAccountInfo|Hesap bilgilerini değiştirme|  
|ButtonLabelCloseAccount|Hesabı kapat|  
|WebAuthenticationInvalidCaptchaErrorMessage|Girilen metin resimdeki metinle eşleşmiyor. Lütfen tekrar deneyin.|  
|DoğrulamaHatasıcredentialsGeçersiz|E-posta veya parola geçersizdir. Lütfen hataları düzeltin ve yeniden deneyin.|  
|WebAuthenticationRequestIsValid|İstek geçerli değil|  
|WebAuthenticationUserIsNotConfirm|Lütfen oturum açmadan önce kaydınızı onaylayın.|  
|WebAuthenticationGeçersizEmailBiçimli|E-posta geçersizdir:{0}|  
|WebAuthenticationUserNotFound|Kullanıcı bulunamadı|  
|WebAuthenticationTenantNotRegistered|Hesabınız, bu portala erişme yetkisi olmayan bir Azure Etkin Dizin kiracısına aittir.|  
|WebAuthenticationAuthenticationFailed|Kimlik doğrulama başarısız oldu.|  
|WebAuthenticationGooglePlusNotEnabled|Kimlik doğrulama başarısız oldu. Uygulamayı yetkilendirdiyseniz, Google kimlik doğrulamasının doğru şekilde yapılandırıldığından emin olmak için lütfen yöneticiyle iletişime geçin.|  
|DoğrulamaErrorAllowedTenantIsGerekli|İzin Verilen Kiracı gereklidir|  
|DoğrulamaHatasıKiracıDeğilGeçerli|Azure Etkin Dizin{0}kiracı ' ' ' geçerli değildir.|  
|WebAuthenticationActiveDirectoryTitle|Azure Active Directory|  
|WebAuthenticationLoginUsingYourProvider|{0} Hesabınızı kullanarak giriş yapın|  
|WebAuthenticationUserLimitNotice|Bu hizmet, izin verilen maksimum kullanıcı sayısına ulaşmıştır. Lütfen `<a href="mailto:{0}"\>contact the administrator</a\>` hizmetlerini yükseltmek ve kullanıcı kaydını yeniden etkinleştirmek için.|  
|WebAuthenticationUserLimitNoticeHeader|Kullanıcı kaydı devre dışı|  
|WebAuthenticationUserLimitNoticeTitle|Kullanıcı kaydı devre dışı|  
|WebAuthenticationKullanıcıRegistrationDisabledNotice|Kullanıcıların kaydı yönetici tarafından devre dışı bırakıldı. Lütfen harici kimlik sağlayıcıile giriş yapın.|  
|WebAuthenticationUserRegistrationDisabledNoticeHeader|Kullanıcı kaydı devre dışı|  
|WebAuthenticationUserRegistrationDisabledNoticeTitle|Kullanıcı kaydı devre dışı|  
|WebAuthenticationSignupPendingConfirmationNotice|Hesabınızın oluşturulmasını tamamlamadan önce e-posta adresinizi doğrulamamız gerekir. E-posta {0}gönderdik. Hesabınızı etkinleştirmek için lütfen e-postanın içindeki talimatları uygulayın. E-posta önümüzdeki birkaç dakika içinde gelmezse, lütfen önemsiz e-posta klasörünüzü kontrol edin.|  
|WebAuthenticationSignupPendingConfirmationAccountFoundNotice|E-posta adresi {0}için doğrulanmamış bir hesap bulduk. Hesabınızın oluşturulmasını tamamlamak için e-posta adresinizi doğrulamamız gerekir. E-posta {0}gönderdik. Hesabınızı etkinleştirmek için lütfen e-postanın içindeki talimatları uygulayın. E-posta önümüzdeki birkaç dakika içinde gelmezse, lütfen önemsiz e-posta klasörünüzü kontrol edin|  
|WebAuthenticationSignupConfirmationAlmostDone|Neredeyse Bitti|  
|WebAuthenticationSignupConfirmationEmailSent|E-posta {0}gönderdik. Hesabınızı etkinleştirmek için lütfen e-postanın içindeki talimatları uygulayın. E-posta önümüzdeki birkaç dakika içinde gelmezse, lütfen önemsiz e-posta klasörünüzü kontrol edin.|  
|WebAuthenticationEmailSentNotificationMessage|Başarıyla gönderilen e-posta{0}|  
|WebAuthenticationNoAadTenantYapılı|Hizmet için yapılandırılan Azure Etkin Dizin kiracısı yok.|  
|CheckboxLabelKullanıcıRegistrationŞartlarGerekli|Ben katılıyorum `<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use</a\>`.|  
|TextblockUserRegistrationTermsProvided|Lütfen gözden geçirin`<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use.</a\>`|  
|DialogHeadingTermsOfUse|Kullanım Koşulları|  
|DoğrulamaMessageConsentNotAccepted|Devam etmeden önce Kullanım Koşullarını kabul etmelisiniz.|  
  
###  <a name="signinstrings"></a><a name="SigninStrings"></a>SigninStrings  
  
|Adı|Metin|  
|----------|----------|  
|WebAuthenticationForgotPassword|Parolanızı mı unuttunuz?|  
|WebAuthenticationIfAdministrator|Yöneticiyseniz oturum `<a href="{0}"\>here</a\>`açmanız gerekir.|  
|WebAuthenticationNotAMember|Henüz üye değil misiniz? `<a href="/signup"\>Sign up now</a\>`|  
|WebAuthenticationRemember|Beni bu bilgisayarda hatırla.|  
|WebAuthenticationSigininWithPassword|Kullanıcı adınız ve şifrenizle oturum açın|  
|WebAuthenticationSigninTitle|Oturum aç|  
|WebAuthenticationSignUpNow|Hemen kaydolun|  
  
###  <a name="signupstrings"></a><a name="SignupStrings"></a>Kayıt KatıRlar  
  
|Adı|Metin|  
|----------|----------|  
|SayfaBaşlığıSignup|Kaydolma|  
|WebAuthenticationAlreadyAMember|Zaten üye misiniz?|  
|WebAuthenticationCreateNewAccount|Yeni bir API Yönetimi hesabı oluşturma|  
|WebAuthenticationSigninNow|Şimdi oturum açın|  
|ButtonLabelSignup|Kaydolma|  
  
###  <a name="subscriptionliststrings"></a><a name="SubscriptionListStrings"></a>AbonelikListStrings  
  
|Adı|Metin|  
|----------|----------|  
|Abonelikİptal Onayı|Bu aboneliği iptal etmek istediğinizden emin misiniz?|  
|AbonelikYenileme Onayı|Bu aboneliği yenilemek istediğinizden emin misiniz?|  
|WebDevelopersManageSubscriptions|Abonelikleri yönetme|  
|WebDevelopersPrimaryKey|Birincil anahtar|  
|WebDevelopersRegenerateLink|Yeniden Oluştur|  
|WebDevelopersSecondaryKey|İkincil anahtar|  
|ButtonLabelShowKey|Göster|  
|ButtonLabelRenewSubscription|Yenile|  
|WebDevelopersSubscriptionRequested|İstenilen{0}|  
|WebDevelopersSubscriptionRequestedState|İstekte bulunan|  
|WebDevelopersSubscriptionTableNameHeader|Adı|  
|WebDevelopersSubscriptionTableStateHeader|Durum|  
|WebDevelopersUsageStatisticsLink|Analitik raporları|  
|WebDevelopersYourSubscriptions|Abonelikleriniz|  
|AbonelikEmlakEtiketİsteTarihi|İstenilen|  
|AbonelikPropertyLabelBaşlangıç Tarihi|Başladı|  
|SayfaBaşlığıRenameAbonelik|Aboneliği yeniden adlandır|  
|AbonelikPropertyLabelName|Abonelik adı|  
  
###  <a name="subscriptionstrings"></a><a name="SubscriptionStrings"></a>AbonelikStrings  
  
|Adı|Metin|  
|----------|----------|  
|BölümBaşlığıCloseAccount|Hesabınızı kapatmak mı arıyorsunuz?|  
|PageTitleDeveloperProfile|Profil|  
|ButtonLabelHideKey|Gizle|  
|ButtonLabelRegenerateKey|Yeniden Oluştur|  
|InformationMessageKeyWasRegenerated|Bu anahtarı yenilemek istediğinizden emin misiniz?|  
|ButtonLabelShowKey|Göster|  
  
###  <a name="updateprofilestrings"></a><a name="UpdateProfileStrings"></a>UpdateProfileStrings  
  
|Adı|Metin|  
|----------|----------|  
|ButtonLabelUpdateProfile|Profili güncelleştir|  
|PageTitleUpdateProfile|Hesap bilgilerini güncelleştir|  
  
###  <a name="userprofile"></a><a name="UserProfile"></a>Kullanıcı Profili  
  
|Adı|Metin|  
|----------|----------|  
|ButtonLabelChangeAccountInfo|Hesap bilgilerini değiştirme|  
|ButtonLabelChangePassword|Parolayı değiştir|  
|ButtonLabelCloseAccount|Hesabı kapat|  
|TextboxLabelEmail|Email|  
|TextboxLabelEmailİlkName|Ad|  
|TextboxLabelEmailLastName|Soyadı|  
|TextboxLabelBildirimlerSenderEmail|Bildirim gönderen e-posta|  
|TextboxLabelOrganizasyonAdı|Kuruluş adı|  
|AbonelikStateActive|Etkin|  
|AbonelikStateCancelled|İptal Edildi|  
|AbonelikSüresi Doldu|Süresi doldu|  
|AbonelikStateReddedildi|Reddedilen|  
|AbonelikStateRequested|İstekte bulunan|  
|AbonelikStateAskıya|Askıya alındı|  
|Varsayılan Abonelik Adı Şablonu|{0}(varsayılan)|  
|AbonelikAdı Şablonu|Geliştirici erişimi #{0}|  
|TextboxLabelAbonelikName|Abonelik adı|  
|DoğrulamaMessageSubscriptionNameGerekli|Abonelik adı boş olamaz.|  
|ApiManagementUserLimitReached|Bu hizmet, izin verilen maksimum kullanıcı sayısına ulaşmıştır. Lütfen daha yüksek bir fiyatlandırma katmanına yükseltin.|  
  
##  <a name="glyph-resources"></a><a name="glyphs"></a>Glyph kaynakları  
 API Yönetimi geliştirici portalı şablonları [Bootstrap gelen Glyphicons gelen glifleri](https://getbootstrap.com/components/#glyphicons)kullanabilirsiniz. Bu glifler kümesi, [Glyphicon](https://glyphicons.com/) Halflings kümesinden yazı tipi biçiminde 250'den fazla glifler içerir. Bu kümeden bir glifler kullanmak için aşağıdaki sözdizimini kullanın.  
  
```html  
<span class="glyphicon glyphicon-user">  
```  
  
 Gliflerin tam listesi için [Bootstrap'daki Glyphicons](https://getbootstrap.com/components/#glyphicons)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar
Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](api-management-developer-portal-templates.md)bakın.
