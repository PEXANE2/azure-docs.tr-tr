---
title: Azure API Yönetimi ilkesi ifadeleri | Microsoft Dokümanlar
description: Azure API Yönetimi'nde ilke ifadeleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/22/2019
ms.author: apimpm
ms.openlocfilehash: 6614e70d130abe46067c657bda3ccdd7000caddc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244010"
---
# <a name="api-management-policy-expressions"></a>API Yönetimi ilkesi ifadeleri
Bu makalede C# 7'de ilke ifadeleri sözdizimi anlatılmaktadır. Her ifade, örtülü olarak sağlanan [bağlam](api-management-policy-expressions.md#ContextVariables) değişkenine ve .NET Framework türlerinin izin verilen [alt kümesine](api-management-policy-expressions.md#CLRTypes) erişebilir.

Daha fazla bilgi için:

- Bağlam bilgilerini arka uç hizmetinize nasıl sağlayacağınızı görün. Bu bilgileri sağlamak için [Sorgu dizesini ve](api-management-transformation-policies.md#SetQueryStringParameter) http üstbilgi ilkelerini [ayarlayın'](api-management-transformation-policies.md#SetHTTPheader) ı ayarlayın.
- Belirteç taleplerine dayalı işlemlere erişimi ön yetkilendirmek için [JWT doğrula](api-management-access-restriction-policies.md#ValidateJWT) ilkesini nasıl kullanacağız bakın.
- İlkelerin nasıl değerlendirildiğini ve bu değerlendirmelerin sonuçlarını görmek için [API Denetçisi](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) izlemesinin nasıl kullanılacağını görün.
- API Yönetimi yanıt önbelleğini yapılandırmak için [önbellekten Ve Depo'dan](api-management-caching-policies.md#GetFromCache) [önbellek ilkelerine](api-management-caching-policies.md#StoreToCache) yönelik ifadeleri nasıl kullanacağını görün. Desteklenen hizmetin `Cache-Control` yönergesi tarafından belirtildiği gibi arka uç hizmetinin yanıt önbelleğe alma yla eşleşen bir süre ayarlayın.
- İçerik filtrelemenin nasıl gerçekleştirildiğini görün. [Denetim akışı](api-management-advanced-policies.md#choose) ve [Set gövde](api-management-transformation-policies.md#SetBody) ilkeleri kullanarak arka uçtan alınan yanıttan veri öğelerini kaldırın.
- İlke bildirimlerini indirmek için [api-management-samples/policies](https://github.com/Azure/api-management-samples/tree/master/policies) GitHub repo'ya bakın.


## <a name="syntax"></a><a name="Syntax"></a>Sözdizimi
Tek ifade ifadeleri, iyi `@(expression)`biçimlendirilmiş bir C# ifadesinin bulunduğu yere `expression` eklenmiştir.

Çok ifadeli ifadeler `@{expression}`. Çok ifadeli ifadeler deki tüm kod `return` yolları bir deyimle sona ermelidir.

## <a name="examples"></a><a name="PolicyExpressionsExamples"></a>Örnekler

```
@(true)

@((1+1).ToString())

@("Hi There".Length)

@(Regex.Match(context.Response.Headers.GetValueOrDefault("Cache-Control",""), @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value)

@(context.Variables.ContainsKey("maxAge") ? int.Parse((string)context.Variables["maxAge"]) : 3600)

@{
  string[] value;
  if (context.Request.Headers.TryGetValue("Authorization", out value))
  {
      if(value != null && value.Length > 0)
      {
          return Encoding.UTF8.GetString(Convert.FromBase64String(value[0]));
      }
  }
  return null;

}
```

## <a name="usage"></a><a name="PolicyExpressionsUsage"></a>Kullanım
İfadeler, herhangi bir API Yönetimi [ilkelerinde](api-management-policies.md) öznitelik değerleri veya metin değerleri olarak kullanılabilir (ilke başvurusu aksini belirtmediği sürece).

> [!IMPORTANT]
> İlke ifadeleri kullandığınızda, ilke tanımlandığında ilke ifadelerinin yalnızca sınırlı bir şekilde doğrulanması vardır. İfadeler çalışma zamanında ağ geçidi tarafından yürütülür, ilke ifadeleri tarafından oluşturulan tüm özel durumlar çalışma zamanı hatasına neden olur.

## <a name="net-framework-types-allowed-in-policy-expressions"></a><a name="CLRTypes"></a>.NET Framework türleri ilke ifadelerinde izin verilen
Aşağıdaki tabloda .NET Framework türleri ve ilke ifadelerinde izin verilen üyeleri listelenir.

|Tür|Desteklenen üyeler|
|--------------|-----------------------|
|Newtonsoft.Json.Formatting|Tümü|
|Newtonsoft.Json.JsonConvert|SerializeObject, DeserializeObject|
|Newtonsoft.Json.Linq.Extensions|Tümü|
|Newtonsoft.Json.Linq.JArray|Tümü|
|Newtonsoft.Json.Linq.JConstructor|Tümü|
|Newtonsoft.Json.Linq.JContainer|Tümü|
|Newtonsoft.Json.Linq.JObject|Tümü|
|Newtonsoft.Json.Linq.JEmlak|Tümü|
|Newtonsoft.Json.Linq.JRaw|Tümü|
|Newtonsoft.Json.Linq.JToken|Tümü|
|Newtonsoft.Json.Linq.JTokenType|Tümü|
|Newtonsoft.Json.Linq.JValue|Tümü|
|Array|Tümü|
|System.BitConverter|Tümü|
|System.Boolean|Tümü|
|System.Byte|Tümü|
|Char|Tümü|
|System.Collections.Generic.Dictionary<TKey, TValue>|Tümü|
|System.Collections.Generic.HashSet\<T>|Tümü|
|System.Collections.Generic.ICollection\<T>|Tümü|
|System.Collections.Generic.IDictionary<TKey, TValue>|Tümü|
|System.Collections.Generic.Imenumerable\<T>|Tümü|
|System.Collections.Generic.IEnumerator\<T>|Tümü|
|System.Collections.Generic.IList\<T>|Tümü|
|System.Collections.Generic.IReadOnlyCollection\<T>|Tümü|
|System.Collections.Generic.IReadOnlyDictionary<TKey, TValue>|Tümü|
|System.Collections.Generic.ISet\<T>|Tümü|
|System.Collections.Generic.KeyValuePair<TKey, TValue>|Tümü|
|System.Collections.Generic.List\<T>|Tümü|
|System.Collections.Generic.Queue\<T>|Tümü|
|System.Collections.Generic.Stack\<T>|Tümü|
|Convert|Tümü|
|Datetime|(Constructor), Add, AddDays, AddHours, AddMilliseconds, AddMinutes, AddMonths, AddSeconds, AddTicks, AddYears, Tarih, DayOfYear, DayInMonth, Saat, IsDaylightSavingTime, IsLeapYear, MaxValue, Millisaniye, Dakika, MinValue, Ay, Şimdi , Ayrıştırmak, İkinci, Çıkarma, Keneler, TimeOfDay, Bugün, ToString, UtcNow, Yıl|
|System.DateTimeKind|Utc|
|System.DateTimeOffset|Tümü|
|Decimal|Tümü|
|Double|Tümü|
|Exception|Tümü|
|System.Guid|Tümü|
|Sistem.Int16|Tümü|
|Sistem.Int32|Tümü|
|Sistem.Int64|Tümü|
|System.IO.StringReader|Tümü|
|System.IO.StringWriter|Tümü|
|System.Linq.Sayısal|Tümü|
|Math|Tümü|
|System.MidpointRounding|Tümü|
|System.Net.WebUtility|Tümü|
|Nullable|Tümü|
|System.Random|Tümü|
|System.SByte|Tümü|
|System.Security.Cryptography.Asimetrik Algoritma|Tümü|
|System.Security.Cryptography.CipherMode|Tümü|
|System.Security.Cryptography.HashAlgorithm|Tümü|
|System.Security.Cryptography.HashAlgorithmName|Tümü|
|System.Security.Cryptography.HMAC|Tümü|
|System.Security.Cryptography.HMACMD5|Tümü|
|System.Security.Cryptography.HMACSHA1|Tümü|
|System.Security.Cryptography.HMACSHA256|Tümü|
|System.Security.Cryptography.HMACSHA384|Tümü|
|System.Security.Cryptography.HMACSHA512|Tümü|
|System.Security.Cryptography.KeyedHashAlgorithm|Tümü|
|System.Security.Cryptography.MD5|Tümü|
|System.Security.Cryptography.Oid|Tümü|
|System.Security.Cryptography.PaddingMode|Tümü|
|System.Security.Cryptography.RNGCryptoServiceProvider|Tümü|
|System.Security.Cryptography.RSA|Tümü|
|System.Security.Cryptography.RSAEncryptionPadding|Tümü|
|System.Security.Cryptography.RSASignaturePadding|Tümü|
|System.Security.Cryptography.SHA1|Tümü|
|System.Security.Cryptography.SHA1Yönetilen|Tümü|
|System.Security.Cryptography.SHA256|Tümü|
|System.Security.Cryptography.SHA256Yönetilen|Tümü|
|System.Security.Cryptography.SHA384|Tümü|
|System.Security.Cryptography.SHA384Yönetilen|Tümü|
|System.Security.Cryptography.SHA512|Tümü|
|System.Security.Cryptography.SHA512Yönetilen|Tümü|
|System.Security.Cryptography.SymmetricAlgorithm|Tümü|
|System.Security.Cryptography.X509Certificates.PublicKey|Tümü|
|System.Security.Cryptography.X509Certificates.RSACertificateExtensions|Tümü|
|System.Security.Cryptography.X509Certificates.X500DistinguishedName|Adı|
|System.Security.Cryptography.X509Certificates.X509Certificate|Tümü|
|System.Security.Cryptography.X509Certificates.X509Certificate2|Tümü|
|System.Security.Cryptography.X509Certificates.X509ContentType|Tümü|
|System.Security.Cryptography.X509Certificates.X509NameType|Tümü|
|System.Single|Tümü|
|System|Tümü|
|System.StringComparer|Tümü|
|System.StringKarşılaştırma|Tümü|
|System.StringSplitOptions|Tümü|
|System.Text.Kodlama|Tümü|
|System.Text.RegularExpressions.Capture|Dizin, Uzunluk, Değer|
|System.Text.RegularExpressions.CaptureCollection|Sayım, Öğe|
|System.Text.RegularExpressions.Group|Yakalar, Başarı|
|System.Text.RegularExpressions.GroupCollection|Sayım, Öğe|
|System.Text.RegularExpressions.Match|Boş, Gruplar, Sonuç|
|System.Text.RegularExpressions.Regex|(Constructor), IsMatch, Match, Maçlar, Değiştir, Unescape, Split|
|System.Text.RegularExpressions.RegexOptions|Tümü|
|Stringbuilder|Tümü|
|Timespan|Tümü|
|System.TimeZone|Tümü|
|System.TimeZoneInfo.AdjustmentRule|Tümü|
|System.TimeZoneInfo.TransitionTime|Tümü|
|System.TimeZoneInfo|Tümü|
|System.Tuple|Tümü|
|Sistem.UInt16|Tümü|
|Sistem.UInt32|Tümü|
|Sistem.UInt64|Tümü|
|Uri|Tümü|
|System.UriPartial|Tümü|
|System.Xml.Linq.Extensions|Tümü|
|System.Xml.Linq.XAttribute|Tümü|
|System.Xml.Linq.XCData|Tümü|
|System.Xml.Linq.XYorum|Tümü|
|System.Xml.Linq.XContainer|Tümü|
|System.Xml.Linq.XDeclaration|Tümü|
|System.Xml.Linq.XBelgesi|Hariç: Yük|
|System.Xml.Linq.XDocumentType|Tümü|
|System.Xml.Linq.XElement|Tümü|
|System.Xml.Linq.XName|Tümü|
|System.Xml.Linq.XNamespace|Tümü|
|System.Xml.Linq.XNode|Tümü|
|System.Xml.Linq.XNodeDocumentOrderComparer|Tümü|
|System.Xml.Linq.XNodeEqualityKarşılaştırılabilir|Tümü|
|System.Xml.Linq.XObject|Tümü|
|System.Xml.Linq.XProcessingInstruction|Tümü|
|System.Xml.Linq.XText|Tümü|
|System.Xml.XmlNodeType|Tümü|

## <a name="context-variable"></a><a name="ContextVariables"></a>Bağlam değişkeni
Adlandırılmış `context` bir değişken örtülü olarak her ilke [ifadesinde](api-management-policy-expressions.md#Syntax)kullanılabilir. Üyeleri, ilgili bilgileri `\request`sağlar. Tüm `context` üyeler salt okunur.

|Bağlam Değişkeni|İzin verilen yöntemler, özellikler ve parametre değerleri|
|----------------------|-------------------------------------------------------|
|bağlam|[Api](#ref-context-api): [IApi](#ref-iapi)<br /><br /> [Dağıtım](#ref-context-deployment)<br /><br /> Geçen: TimeSpan - Zaman damgası değeri ve geçerli zaman arasındaki zaman aralığı<br /><br /> [LastError](#ref-context-lasterror)<br /><br /> [İşlem](#ref-context-operation)<br /><br /> [Ürün](#ref-context-product)<br /><br /> [İstek](#ref-context-request)<br /><br /> RequestId: Guid - benzersiz istek tanımlayıcısı<br /><br /> [Yanıt](#ref-context-response)<br /><br /> [Abonelik](#ref-context-subscription)<br /><br /> Zaman damgası: DateTime - istek alındığı zaman noktası<br /><br /> İzleme: bool - izlemenin kapalı veya kapalı olup olmadığını gösterir <br /><br /> [Kullanıcı](#ref-context-user)<br /><br /> [Değişkenler](#ref-context-variables): IReadOnlyDictionary<dize, nesne><br /><br /> void Trace(mesaj: string)|
|<a id="ref-context-api"></a>Bağlam. Apı|Id: string<br /><br /> IsCurrentRevision: bool<br /><br />  Adı: string<br /><br /> Yol: string<br /><br /> Revizyon: string<br /><br /> ServiceUrl: [IUrl](#ref-iurl)<br /><br /> Sürüm: string |
|<a id="ref-context-deployment"></a>Bağlam. Dağıtım|Bölge: string<br /><br /> ServiceName: string<br /><br /> Sertifikalar: IReadOnlyDictionary<dizesi, X509Certificate2>|
|<a id="ref-context-lasterror"></a>Bağlam. Son Hata|Kaynak: string<br /><br /> Sebep: string<br /><br /> İleti: string<br /><br /> Kapsam: string<br /><br /> Bölüm: string<br /><br /> Yol: string<br /><br /> PolicyId: dize<br /><br /> Bağlam hakkında daha fazla bilgi için. LastError, bkz. [Hata işleme.](api-management-error-handling-policies.md)|
|<a id="ref-context-operation"></a>Bağlam. Işlem|Id: string<br /><br /> Yöntem: string<br /><br /> Adı: string<br /><br /> UrlTemplate: dize|
|<a id="ref-context-product"></a>Bağlam. Ürün|Apis: [IApi'<](#ref-iapi) Imumerable\><br /><br /> Onay Gerekli: bool<br /><br /> Gruplar: [IGroup'<](#ref-igroup)\><br /><br /> Id: string<br /><br /> Adı: string<br /><br /> Durum: enum ProductState {NotPublished, Published}<br /><br /> SubscriptionLimit: int?<br /><br /> Abonelik Gerekli: bool|
|<a id="ref-context-request"></a>Bağlam. Istek|Gövde: [IMessageBody](#ref-imessagebody) veya `null` istek bir gövdeye sahip değilse.<br /><br /> Sertifika: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> [Üstbilgi](#ref-context-request-headers): IReadOnlyDictionary<dize, string[]><br /><br /> IpAddress: dize<br /><br /> Eşleşen Parametreler: IReadOnlyDictionary<dize, dize><br /><br /> Yöntem: string<br /><br /> OrijinalUrl: [IUrl](#ref-iurl)<br /><br /> Url: [IUrl](#ref-iurl)|
|<a id="ref-context-request-headers"></a>dize bağlamı. Request.Headers.GetValueOrDefault(üstbilgiAdı: string, defaultValue: string)|headerName: string<br /><br /> defaultValue: dize<br /><br /> Virgülden ayrılmış istek üstbilgi `defaultValue` değerlerini veya üstbilgi bulunamazsa döndürür.|
|<a id="ref-context-response"></a>Bağlam. Yanıt|Gövde: [IMessageBody](#ref-imessagebody)<br /><br /> [Üstbilgi](#ref-context-response-headers): IReadOnlyDictionary<dize, string[]><br /><br /> StatusCode: int<br /><br /> StatusReason: dize|
|<a id="ref-context-response-headers"></a>dize bağlamı. Response.Headers.GetValueOrDefault(headerName: string, defaultValue: string)|headerName: string<br /><br /> defaultValue: dize<br /><br /> Virgülden ayrılmış yanıt üstbilgi `defaultValue` değerlerini veya üstbilgi bulunamazsa döndürür.|
|<a id="ref-context-subscription"></a>Bağlam. Abonelik|Oluşturulan Saat: DateTime<br /><br /> Bitiş Tarihi: DateTime?<br /><br /> Id: string<br /><br /> Anahtar: string<br /><br /> Adı: string<br /><br /> PrimaryKey: dize<br /><br /> SecondaryKey: dize<br /><br /> Başlangıç Tarihi: DateTime?|
|<a id="ref-context-user"></a>Bağlam. Kullanıcı|E-posta: string<br /><br /> NameName: string<br /><br /> Gruplar: [IGroup'<](#ref-igroup)\><br /><br /> Id: string<br /><br /> Kimlikler: [IUserIdentity](#ref-iuseridentity)<Tanımlanabilir\><br /><br /> Soyadı: string<br /><br /> Not: string<br /><br /> Kayıt Tarihi: DateTime|
|<a id="ref-iapi"></a>IApi|Id: string<br /><br /> Adı: string<br /><br /> Yol: string<br /><br /> Protokoller: Imumerable<dize\><br /><br /> ServiceUrl: [IUrl](#ref-iurl)<br /><br /> AbonelikKeyParameterNames: [ISubscriptionKeyParameterNames](#ref-isubscriptionkeyparameternames)|
|<a id="ref-igroup"></a>IGroup|Id: string<br /><br /> Adı: string|
|<a id="ref-imessagebody"></a>IMessageBody|<T\>(preserveContent: bool = false): Where T: string, bayt[],JObject, JToken, JArray, XNode, XElement, XDocument<br /><br /> Ve `context.Request.Body.As<T>` `context.Response.Body.As<T>` yöntemler, bir istek ve yanıt iletisi `T`gövdesini belirli bir türde okumak için kullanılır. Varsayılan olarak yöntem özgün ileti gövdesi akışını kullanır ve döndükten sonra kullanılamaz hale getirir. Yöntemin gövde akışının bir kopyası üzerinde çalışmasını sağlayarak `preserveContent` bunu önlemek `true`için parametreyi . Bir örnek görmek için [buraya](api-management-transformation-policies.md#SetBody) gidin.|
|<a id="ref-iurl"></a>IUrl|Ana bilgisayar: string<br /><br /> Yol: string<br /><br /> Bağlantı noktası: int<br /><br /> [Sorgu](#ref-iurl-query): IReadOnlyDictionary<dize, string[]><br /><br /> QueryString: string<br /><br /> Şema: string|
|<a id="ref-iuseridentity"></a>IUserIdentity|Id: string<br /><br /> Sağlayıcı: string|
|<a id="ref-isubscriptionkeyparameternames"></a>ISubscriptionKeyParameterNames|Üstbilgi: string<br /><br /> Sorgu: string|
|<a id="ref-iurl-query"></a>string IUrl.Query.GetValueOrDefault(queryParameterName: string, defaultValue: string)|queryParameterName: string<br /><br /> defaultValue: dize<br /><br /> Virgülden ayrılmış sorgu parametre `defaultValue` değerlerini veya parametre bulunamazsa döndürür.|
|<a id="ref-context-variables"></a>T bağlamı. Variables.GetValueOrDefault<T\>(variableName: string, defaultValue: T)|variableName: string<br /><br /> defaultValue: T<br /><br /> Değişken değeri dökümunu `T` `defaultValue` türe veya değişken bulunamazsa döndürür.<br /><br /> Belirtilen tür döndürülen değişkenin gerçek türüyle eşleşmiyorsa, bu yöntem bir özel durum oluşturur.|
|BasicAuthCredentials AsBasic(giriş: bu dize)|giriş: string<br /><br /> Giriş parametresi geçerli bir HTTP Temel Kimlik Doğrulama yetkilendirme isteği üstbilgi değeri `BasicAuthCredentials`içeriyorsa, yöntem tür bir nesne döndürür; aksi takdirde yöntem null döndürür.|
|bool TryParseBasic(giriş: bu dize, sonuç: BasicAuthCredentials çıktı)|giriş: string<br /><br /> sonuç: dışarı BasicAuthCredentials<br /><br /> Giriş parametresi istek üstbilgisinde geçerli bir HTTP Temel Kimlik Doğrulama `true` yetkilendirme değeri içeriyorsa yöntem `BasicAuthCredentials`döndürür ve sonuç parametresi bir tür değeri içerir; aksi takdirde `false`yöntem döndürür.|
|BasicAuth Kimlik Bilgileri|Şifre: string<br /><br /> UserId: string|
|Jwt AsJwt(giriş: bu dize)|giriş: string<br /><br /> Giriş parametresi geçerli bir JWT belirteç değeri içeriyorsa, yöntem bir tür `Jwt`nesnesi döndürür; aksi takdirde `null`yöntem döndürür.|
|bool TryParseJwt(giriş: bu dize, sonuç: Jwt çıkış)|giriş: string<br /><br /> sonuç: dışarı Jwt<br /><br /> Giriş parametresi geçerli bir JWT belirteç değeri `true` içeriyorsa, yöntem döndürür ve sonuç parametresi bir tür `Jwt`değeri içerir; aksi takdirde `false`yöntem döndürür.|
|Jwt|Algoritma: dize<br /><br /> İzleyici: Ayrılmaz<dize\><br /><br /> İddialar: IReadOnlyDictionary<dize, string[]><br /><br /> Sona Erme Zamanı: DateTime?<br /><br /> Id: string<br /><br /> Veren: string<br /><br /> İhraç: DateTime?<br /><br /> NotBefore: DateTime?<br /><br /> Konu: string<br /><br /> Türü: string|
|string Jwt.Claims.GetValueOrDefault(claimName: string, defaultValue: string)|claimName: string<br /><br /> defaultValue: dize<br /><br /> Virgülden ayrılmış talep `defaultValue` değerlerini veya üstbilgi bulunamazsa döndürür.|
|byte[] Encrypt(giriş: bu bayt[], alg: string, key:byte[], iv:byte[])|giriş - düz metin şifrelenecek<br /><br />alg - simetrik şifreleme algoritmasının adı<br /><br />anahtar - şifreleme anahtarı<br /><br />iv - başbaşlatma vektörü<br /><br />Şifreli düz metni döndürür.|
|byte[] Encrypt(giriş: bu bayt[], alg: System.Security.Cryptography.SymmetricAlgorithm)|giriş - düz metin şifrelenecek<br /><br />alg - şifreleme algoritması<br /><br />Şifreli düz metni döndürür.|
|byte[] Encrypt(giriş: bu bayt[], alg: System.Security.Cryptography.SymmetricAlgorithm, key:byte[], iv:byte[])|giriş - düz metin şifrelenecek<br /><br />alg - şifreleme algoritması<br /><br />anahtar - şifreleme anahtarı<br /><br />iv - başbaşlatma vektörü<br /><br />Şifreli düz metni döndürür.|
|byte[] Decrypt(giriş: bu bayt[], alg: string, key:byte[], iv:byte[])|giriş - cypher metin şifresi çözülecek<br /><br />alg - simetrik şifreleme algoritmasının adı<br /><br />anahtar - şifreleme anahtarı<br /><br />iv - başbaşlatma vektörü<br /><br />Düz metni döndürür.|
|byte[] Decrypt(giriş: bu bayt[], alg: System.Security.Cryptography.SymmetricAlgorithm)|giriş - cypher metin şifresi çözülecek<br /><br />alg - şifreleme algoritması<br /><br />Düz metni döndürür.|
|byte[] Decrypt(giriş: bu bayt[], alg: System.Security.Cryptography.SymmetricAlgorithm, key:byte[], iv:byte[])|giriş - cypher metin şifresi çözülecek<br /><br />alg - şifreleme algoritması<br /><br />anahtar - şifreleme anahtarı<br /><br />iv - başbaşlatma vektörü<br /><br />Düz metni döndürür.|
|bool VerifyNoRevocation(giriş: bu System.Security.Cryptography.X509Certificates.X509Certificate2)|Sertifika iptal durumunu denetlemeden X.509 zincir doğrulaması gerçekleştirir.<br /><br />giriş - sertifika nesnesi<br /><br />Doğrulama `true` başarılı olursa döndürür; `false` doğrulama başarısız olursa.|


## <a name="next-steps"></a>Sonraki adımlar

İlkelerle çalışan daha fazla bilgi için bkz:

+ [API Yönetiminde İlkeler](api-management-howto-policies.md)
+ [API'leri Dönüştür](transform-api.md)
+ [İlke](api-management-policy-reference.md) deyimlerinin ve ayarlarının tam listesi için İlke Başvurusu
+ [İlke örnekleri](policy-samples.md)
