---
title: Azure API Management ilke ifadeleri | Microsoft Docs
description: Azure API Management 'de ilke ifadeleri hakkında bilgi edinin. Örneklere bakın ve kullanılabilir ek kaynakları görüntüleyin.
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
ms.openlocfilehash: 7117ffcbaf4eba8d83a6e968f4fed7422673610b
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87844250"
---
# <a name="api-management-policy-expressions"></a>API Management İlkesi ifadeleri
Bu makalede C# 7 ' de ilke ifadeleri sözdizimi anlatılmaktadır. Her bir ifadenin örtük olarak sağlanmış [bağlam](api-management-policy-expressions.md#ContextVariables) değişkenine ve .NET Framework türlerin izin verilen bir [alt kümesine](api-management-policy-expressions.md#CLRTypes) erişimi vardır.

Daha fazla bilgi için:

- Bkz. arka uç hizmetinize bağlam bilgilerini sağlama. [Sorgu dizesi ayarla parametresini](api-management-transformation-policies.md#SetQueryStringParameter) kullanın ve bu bilgileri sağlamak için [http üst bilgi ilkelerini ayarlayın](api-management-transformation-policies.md#SetHTTPheader) .
- Belirteç taleplerine göre işlemlere erişimi önceden yetkilendirmek için bkz. [JWT](api-management-access-restriction-policies.md#ValidateJWT) ilkesini nasıl kullanacağınızı öğrenin.
- İlkelerin nasıl değerlendirileceğini ve bu değerlendirmelerinin sonuçlarını görmek için [API denetçisi](./api-management-howto-api-inspector.md) izlemesini nasıl kullanacağınızı öğrenin.
- API Management yanıtı önbelleğe almayı yapılandırmak için, bkz. bir [önbellekten al](api-management-caching-policies.md#GetFromCache) ve [önbellek ilkelerine depola](api-management-caching-policies.md#StoreToCache) ilkeleri kullanma. Desteklenen hizmetin yönergesinde belirtilen arka uç hizmetinin yanıt önbelleklemesi ile eşleşen bir süre ayarlayın `Cache-Control` .
- Bkz. içerik filtrelemeyi gerçekleştirme. [Denetim akışını](api-management-advanced-policies.md#choose) ve [gövde ilkelerini ayarla](api-management-transformation-policies.md#SetBody) ' yı kullanarak arka uçta alınan yanıttan veri öğelerini kaldırın.
- İlke deyimlerini indirmek için bkz. [api-Management-Samples/policies](https://github.com/Azure/api-management-samples/tree/master/policies) GitHub deposu.


## <a name="syntax"></a><a name="Syntax"></a>Sözdizimi
Tek deyim ifadeleri içine alınmıştır `@(expression)` , burada `expression` iyi biçimlendirilmiş bir C# ifade deyimidir.

Çoklu deyim ifadeleri içine alınmıştır `@{expression}` . Çok deyimli ifadeler içindeki tüm kod yollarının bir ifadesiyle bitmesi gerekir `return` .

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
İfadeler, herhangi [bir API Management ilkesinde](api-management-policies.md) öznitelik değerleri veya metin değerleri olarak kullanılabilir (ilke başvurusu aksini belirtmedikleri sürece).

> [!IMPORTANT]
> İlke ifadelerini kullandığınızda, ilke tanımlandığında ilke ifadelerinin yalnızca sınırlı doğrulaması vardır. İfadeler çalışma zamanında ağ geçidi tarafından yürütülür, ilke ifadeleri tarafından oluşturulan tüm özel durumlar bir çalışma zamanı hatasına neden oluşur.

## <a name="net-framework-types-allowed-in-policy-expressions"></a><a name="CLRTypes"></a>İlke ifadelerinde izin verilen .NET Framework türleri
Aşağıdaki tablo, ilke ifadelerinde izin verilen .NET Framework türlerini ve üyelerini listelemektedir.

|Tür|Desteklenen Üyeler|
|--------------|-----------------------|
|Üzerinde Newtonsoft.Js. Biçime|Tümü|
|Newtonsoft.Json.JsonConvert|SerializeObject, DeserializeObject|
|Üzerinde Newtonsoft.Js. LINQ. Extensions|Tümü|
|Üzerinde Newtonsoft.Js. LINQ. JArray|Tümü|
|Üzerinde Newtonsoft.Js. LINQ. JConstructor|Tümü|
|Üzerinde Newtonsoft.Js. LINQ. JContainer|Tümü|
|Üzerinde Newtonsoft.Js. LINQ. JObject|Tümü|
|Üzerinde Newtonsoft.Js. LINQ. JProperty|Tümü|
|Üzerinde Newtonsoft.Js. LINQ. JRaw|Tümü|
|Üzerinde Newtonsoft.Js. LINQ. JToken|Tümü|
|Üzerinde Newtonsoft.Js. LINQ. JTokenType|Tümü|
|Üzerinde Newtonsoft.Js. LINQ. JValue|Tümü|
|System. Array|Tümü|
|System. BitConverter|Tümü|
|System. Boolean|Tümü|
|System. Byte|Tümü|
|System. Char|Tümü|
|System. Collections. Generic. Dictionary<TKey, TValue>|Tümü|
|System. Collections. Generic. HashSet\<T>|Tümü|
|System. Collections. Generic. ICollection\<T>|Tümü|
|System. Collections. Generic. IDictionary<TKey, TValue>|Tümü|
|System. Collections. Generic. IEnumerable\<T>|Tümü|
|System. Collections. Generic. IEnumerator\<T>|Tümü|
|System. Collections. Generic. IList\<T>|Tümü|
|System. Collections. Generic. IReadOnlyCollection\<T>|Tümü|
|System. Collections. Generic. IReadOnlyDictionary<TKey, TValue>|Tümü|
|System. Collections. Generic. Iset\<T>|Tümü|
|System. Collections. Generic. KeyValuePair<TKey, TValue>|Tümü|
|System. Collections. Generic. List\<T>|Tümü|
|System. Collections. Generic. Queue\<T>|Tümü|
|System. Collections. Generic. Stack\<T>|Tümü|
|System. Convert|Tümü|
|System. DateTime|(Oluşturucu), ekleme, Addgün, AddHours, AddMilliseconds, AddMinutes, Addaylar, AddSeconds, AddTicks, Addyear, Date, Day, DayOfWeek, DayOfYear, DaysInMonth, Hour, IsDaylightSavingTime, IsLeapYear, MaxValue, milisaniyeye, dakika, MinValue, month, Now, Parse, Second, Subtract, ticks, TimeOfDay, bugün, ToString, UtcNow, Year|
|System. DateTimeKind|UTC|
|System. DateTimeOffset|Tümü|
|System. Decimal|Tümü|
|System. Double|Tümü|
|System. Exception|Tümü|
|System. Guid|Tümü|
|System. Int16|Tümü|
|System. Int32|Tümü|
|System. Int64|Tümü|
|System. ıO. StringReader|Tümü|
|System. ıO. StringWriter|Tümü|
|System. LINQ. sıralanabilir|Tümü|
|System. Math|Tümü|
|System. MidpointRounding|Tümü|
|System .net. Webugiyi|Tümü|
|System. Nullable|Tümü|
|System. Random|Tümü|
|System. SByte|Tümü|
|System. Security. Cryptography. AsymmetricAlgorithm|Tümü|
|System. Security. Cryptography. CipherMode|Tümü|
|System. Security. Cryptography. HashAlgorithm|Tümü|
|System. Security. Cryptography. HashAlgorithmName|Tümü|
|System. Security. Cryptography. HMAC|Tümü|
|System. Security. Cryptography. HMACMD5|Tümü|
|System. Security. Cryptography. HMACSHA1|Tümü|
|System. Security. Cryptography. HMACSHA256|Tümü|
|System. Security. Cryptography. HMACSHA384|Tümü|
|System. Security. Cryptography. HMACSHA512|Tümü|
|System. Security. Cryptography. KeyedHashAlgorithm|Tümü|
|System. Security. Cryptography. MD5|Tümü|
|System. Security. Cryptography. OID|Tümü|
|System. Security. Cryptography. doldurma Ingmode|Tümü|
|System. Security. Cryptography. RNGCryptoServiceProvider|Tümü|
|System. Security. Cryptography. RSA|Tümü|
|System. Security. Cryptography. RSAEncryptionPadding|Tümü|
|System. Security. Cryptography. Rsatifturepekleniyor|Tümü|
|System. Security. Cryptography. SHA1|Tümü|
|System. Security. Cryptography. SHA1Managed|Tümü|
|System. Security. Cryptography. SHA256|Tümü|
|System. Security. Cryptography. SHA256Managed|Tümü|
|System. Security. Cryptography. SHA384|Tümü|
|System. Security. Cryptography. SHA384Managed|Tümü|
|System. Security. Cryptography. SHA512 olur|Tümü|
|System. Security. Cryptography. SHA512Managed|Tümü|
|System. Security. Cryptography. SymmetricAlgorithm|Tümü|
|System. Security. Cryptography. X509Certificates. PublicKey|Tümü|
|System. Security. Cryptography. X509Certificates. RSACertificateExtensions|Tümü|
|System. Security. Cryptography. X509Certificates. X500DistinguishedName|Ad|
|System. Security. Cryptography. X509Certificates. X509Certificate|Tümü|
|System. Security. Cryptography. X509Certificates. X509Certificate2|Tümü|
|System. Security. Cryptography. X509Certificates. X509ContentType|Tümü|
|System. Security. Cryptography. X509Certificates. X509NameType|Tümü|
|System. Single|Tümü|
|System. String|Tümü|
|System. StringComparer|Tümü|
|System. StringComparison|Tümü|
|System. Stringsptoptions|Tümü|
|System. Text. Encoding|Tümü|
|System. Text. RegularExpressions. Capture|Dizin, uzunluk, değer|
|System. Text. RegularExpressions. CaptureCollection|Sayı, öğe|
|System. Text. RegularExpressions. Group|Yakalamalar, başarılı|
|System. Text. RegularExpressions. GroupCollection|Sayı, öğe|
|System. Text. RegularExpressions. Match|Boş, gruplar, sonuç|
|System. Text. RegularExpressions. Regex|(Oluşturucu), IsMatch, Eşleştir, eşleşmeler, Değiştir, Unkaçış, Böl|
|System. Text. RegularExpressions. RegexOptions|Tümü|
|System. Text. StringBuilder|Tümü|
|System. TimeSpan|Tümü|
|System. TimeZone|Tümü|
|System. TimeZoneInfo. AdjustmentRule|Tümü|
|System. TimeZoneInfo. geçişli Tiontime|Tümü|
|System. TimeZoneInfo|Tümü|
|System. Tuple|Tümü|
|System. UInt16|Tümü|
|System. UInt32|Tümü|
|System. UInt64|Tümü|
|System. Uri|Tümü|
|System. UriPartial|Tümü|
|System.Xml. LINQ. Extensions|Tümü|
|System.Xml. LINQ. XAttribute|Tümü|
|System.Xml. LINQ. XCData|Tümü|
|System.Xml. LINQ. XComment|Tümü|
|System.Xml. LINQ. XContainer|Tümü|
|System.Xml. LINQ. XDeclaration|Tümü|
|System.Xml. LINQ. XDocument|Tümü, şunun dışında: yükleme|
|System.Xml. LINQ. XDocumentType|Tümü|
|System.Xml. LINQ. XElement|Tümü|
|System.Xml. LINQ. XName|Tümü|
|System.Xml. LINQ. XNamespace|Tümü|
|System.Xml. LINQ. XNode|Tümü|
|System.Xml. LINQ. XNodeDocumentOrderComparer|Tümü|
|System.Xml. LINQ. XNodeEqualityComparer|Tümü|
|System.Xml. LINQ. XObject|Tümü|
|System.Xml. LINQ. XProcessingInstruction|Tümü|
|System.Xml. LINQ. XText|Tümü|
|System.Xml.XmlNodeType|Tümü|

## <a name="context-variable"></a><a name="ContextVariables"></a>Bağlam değişkeni
Adlı bir değişken `context` , her ilke [ifadesinde](api-management-policy-expressions.md#Syntax)örtülü olarak kullanılabilir. Üyeleri ile ilgili bilgiler sağlar `\request` . Tüm `context` Üyeler salt okunurdur.

|Bağlam değişkeni|İzin verilen Yöntemler, Özellikler ve parametre değerleri|
|----------------------|-------------------------------------------------------|
|bağlam|[API](#ref-context-api): [ıapi](#ref-iapi)<br /><br /> [Dağıtım](#ref-context-deployment)<br /><br /> Geçen: zaman damgası ve geçerli saat değeri arasındaki TimeSpan-Time aralığı<br /><br /> [LastError](#ref-context-lasterror)<br /><br /> [İşlem](#ref-context-operation)<br /><br /> [Ürün](#ref-context-product)<br /><br /> [İstek](#ref-context-request)<br /><br /> RequestId: Guid-benzersiz istek tanımlayıcısı<br /><br /> [Response](#ref-context-response)<br /><br /> [Abonelik](#ref-context-subscription)<br /><br /> Zaman damgası: DateTime-isteğin alındığı zaman içinde nokta<br /><br /> İzleme: bool-izlemenin açık veya kapalı olup olmadığını gösterir <br /><br /> [Kullanıcı](#ref-context-user)<br /><br /> [Değişkenler](#ref-context-variables): IReadOnlyDictionary<dize, nesne><br /><br /> void trace (ileti: dize)|
|<a id="ref-context-api"></a>bağlam. 'Sindeki|Kimlik: dize<br /><br /> IsCurrentRevision: bool<br /><br />  Ad: dize<br /><br /> Yol: dize<br /><br /> Düzeltme: dize<br /><br /> ServiceUrl: [Iurl](#ref-iurl)<br /><br /> Sürüm: dize |
|<a id="ref-context-deployment"></a>bağlam. Dağıtmak|Bölge: dize<br /><br /> HizmetAdı: dize<br /><br /> Sertifikalar: IReadOnlyDictionary<dize, X509Certificate2>|
|<a id="ref-context-lasterror"></a>bağlam. LastError|Kaynak: dize<br /><br /> Neden: dize<br /><br /> İleti: dize<br /><br /> Kapsam: dize<br /><br /> Bölüm: dize<br /><br /> Yol: dize<br /><br /> PolicyId: dize<br /><br /> Bağlam hakkında daha fazla bilgi için. LastError, bkz. [hata işleme](api-management-error-handling-policies.md).|
|<a id="ref-context-operation"></a>bağlam. Çalışmasını|Kimlik: dize<br /><br /> Yöntem: dize<br /><br /> Ad: dize<br /><br /> UrlTemplate: dize|
|<a id="ref-context-product"></a>bağlam. Ürünüyle|API 'ler: IEnumerable<[ıapi](#ref-iapi)\><br /><br /> ApprovalRequired: bool<br /><br /> Gruplar: IEnumerable<[Igroup](#ref-igroup)\><br /><br /> Kimlik: dize<br /><br /> Ad: dize<br /><br /> Durum: enum ProductState {Notyayýmlandý}<br /><br /> SubscriptionLimit: int?<br /><br /> Abonelik gerekli: bool|
|<a id="ref-context-request"></a>bağlam. İsteyen|Body: [ımessagebody](#ref-imessagebody) veya `null` isteğin gövdesi yok.<br /><br /> Sertifika: System. Security. Cryptography. X509Certificates. X509Certificate2<br /><br /> [Üstbilgiler](#ref-context-request-headers): IReadOnlyDictionary<dize, dize [] ><br /><br /> IPAddress: dize<br /><br /> MatchedParameters: IReadOnlyDictionary<dize, dize><br /><br /> Yöntem: dize<br /><br /> OriginalUrl: [Iurl](#ref-iurl)<br /><br /> URL: [Iurl](#ref-iurl)|
|<a id="ref-context-request-headers"></a>dize bağlamı. Request. Headers. GetValueOrDefault (headerName: String, defaultValue: String)|headerName: dize<br /><br /> defaultValue: dize<br /><br /> Virgülle ayrılmış istek üst bilgisi değerlerini döndürür veya `defaultValue` üst bilgi bulunamadı.|
|<a id="ref-context-response"></a>bağlam. Yanıtıyla|Gövde: [ımessagebody](#ref-imessagebody)<br /><br /> [Üstbilgiler](#ref-context-response-headers): IReadOnlyDictionary<dize, dize [] ><br /><br /> StatusCode: int<br /><br /> StatusReason: dize|
|<a id="ref-context-response-headers"></a>dize bağlamı. Response. Headers. GetValueOrDefault (headerName: String, defaultValue: String)|headerName: dize<br /><br /> defaultValue: dize<br /><br /> Virgülle ayrılmış yanıt üst bilgisi değerlerini döndürür veya `defaultValue` üst bilgi bulunamadı.|
|<a id="ref-context-subscription"></a>bağlam. Aboneliğiniz|CreatedTime: DateTime<br /><br /> EndDate: DateTime<br /><br /> Kimlik: dize<br /><br /> Anahtar: dize<br /><br /> Ad: dize<br /><br /> PrimaryKey: String<br /><br /> SecondaryKey: dize<br /><br /> StartDate: TarihSaat?|
|<a id="ref-context-user"></a>bağlam. Kullanıcısını|E-posta: dize<br /><br /> FirstName: dize<br /><br /> Gruplar: IEnumerable<[Igroup](#ref-igroup)\><br /><br /> Kimlik: dize<br /><br /> Kimlikler: IEnumerable<[ıuserıdentity](#ref-iuseridentity)\><br /><br /> LastName: dize<br /><br /> Note: dize<br /><br /> RegistrationDate: DateTime|
|<a id="ref-iapi"></a>Iapi|Kimlik: dize<br /><br /> Ad: dize<br /><br /> Yol: dize<br /><br /> Protokoller: IEnumerable<dizesi\><br /><br /> ServiceUrl: [Iurl](#ref-iurl)<br /><br /> SubscriptionKeyParameterNames: [ISubscriptionKeyParameterNames](#ref-isubscriptionkeyparameternames)|
|<a id="ref-igroup"></a>Igroup|Kimlik: dize<br /><br /> Ad: dize|
|<a id="ref-imessagebody"></a>Imessagebody|<T \> (preservecontent: bool = false): burada t: String, Byte [], JObject, JToken, JArray, XNode, XElement, XDocument<br /><br /> `context.Request.Body.As<T>`Ve `context.Response.Body.As<T>` yöntemleri, belirli bir türdeki bir istek ve yanıt iletisi gövdelerini okumak için kullanılır `T` . Varsayılan olarak, yöntemi özgün ileti gövdesi akışını kullanır ve çağrıldıktan sonra kullanılamaz hale getirir. Yöntemin gövde akışının bir kopyası üzerinde çalışmasını sağlayarak bunu önlemek için `preserveContent` parametresini olarak ayarlayın `true` . Bir örnek görmek için [buraya](api-management-transformation-policies.md#SetBody) gidin.|
|<a id="ref-iurl"></a>Iurl|Ana bilgisayar: dize<br /><br /> Yol: dize<br /><br /> Bağlantı noktası: int<br /><br /> [Sorgu](#ref-iurl-query): IReadOnlyDictionary<dize, dize [] ><br /><br /> QueryString: String<br /><br /> Düzen: dize|
|<a id="ref-iuseridentity"></a>Iuserıdentity|Kimlik: dize<br /><br /> Sağlayıcı: dize|
|<a id="ref-isubscriptionkeyparameternames"></a>ISubscriptionKeyParameterNames|Üstbilgi: dize<br /><br /> Sorgu: dize|
|<a id="ref-iurl-query"></a>String Iurl. Query. GetValueOrDefault (queryParameterName: String, defaultValue: String)|queryParameterName: dize<br /><br /> defaultValue: dize<br /><br /> Virgülle ayrılmış sorgu parametresi değerlerini döndürür veya `defaultValue` parametre bulunmazsa.|
|<a id="ref-context-variables"></a>T bağlamı. Variables. GetValueOrDefault<T \> (VariableName: String, DefaultValue: T)|variableName: dize<br /><br /> defaultValue: T<br /><br /> Değişken değeri türüne dönüştürme `T` veya `defaultValue` değişken bulunamazsa döndürür.<br /><br /> Bu yöntem, belirtilen tür döndürülen değişkenin gerçek türüyle eşleşmiyorsa bir özel durum oluşturur.|
|BasicAuthCredentials AsBasic (giriş: Bu dize)|Giriş: dize<br /><br /> Giriş parametresi geçerli bir HTTP temel kimlik doğrulaması yetkilendirme isteği üst bilgisi değeri içeriyorsa, yöntemi türünde bir nesne döndürür `BasicAuthCredentials` ; Aksi takdirde yöntem null döndürür.|
|bool TryParseBasic (giriş: Bu dize, sonuç: Out BasicAuthCredentials)|Giriş: dize<br /><br /> Sonuç: Out BasicAuthCredentials<br /><br /> Giriş parametresi istek üstbilgisinde geçerli bir HTTP temel kimlik doğrulaması yetkilendirme değeri içeriyorsa, yöntem döner `true` ve sonuç parametresi türünde bir değer içerir `BasicAuthCredentials` ; Aksi takdirde Yöntem döndürülür `false` .|
|BasicAuthCredentials|Parola: dize<br /><br /> UserID: String|
|JWT AsJwt (giriş: Bu dize)|Giriş: dize<br /><br /> Giriş parametresi geçerli bir JWT belirteci değeri içeriyorsa, yöntemi türünde bir nesne döndürür `Jwt` ; Aksi takdirde Yöntem döndürülür `null` .|
|bool TryParseJwt (giriş: Bu dize, sonuç: Out JWT)|Giriş: dize<br /><br /> Sonuç: Out JWT<br /><br /> Giriş parametresi geçerli bir JWT belirteci değeri içeriyorsa, yöntemi döner `true` ve sonuç parametresi türünde bir değer içerir `Jwt` ; Aksi takdirde Yöntem döndürülür `false` .|
|JWT|Algoritma: dize<br /><br /> İzleyiciler: IEnumerable<dize\><br /><br /> Talepler: IReadOnlyDictionary<dize, dize [] ><br /><br /> ExpirationTime: DateTime<br /><br /> Kimlik: dize<br /><br /> Veren: dize<br /><br /> Issuedat: DateTime<br /><br /> NotBefore: DateTime?<br /><br /> Subject: dize<br /><br /> Tür: dize|
|String JWT. Claim. GetValueOrDefault (claimName: String, defaultValue: String)|claimName: dize<br /><br /> defaultValue: dize<br /><br /> Virgülle ayrılmış talep değerlerini döndürür veya `defaultValue` üst bilgi bulunmazsa.|
|Byte [] Encrypt (Input: this Byte [], alg: String, Key: Byte [], IV: Byte [])|şifrelenecek giriş düz metin<br /><br />alg-simetrik şifreleme algoritmasının adı<br /><br />anahtar şifreleme anahtarı<br /><br />IV-başlatma vektörü<br /><br />Şifrelenmiş düz metin döndürür.|
|Byte [] şifreleyin (input: Bu Byte [], alg: System. Security. Cryptography. SymmetricAlgorithm)|şifrelenecek giriş düz metin<br /><br />alg-şifreleme algoritması<br /><br />Şifrelenmiş düz metin döndürür.|
|Byte [] şifreleyin (input: Bu Byte [], alg: System. Security. Cryptography. SymmetricAlgorithm, Key: Byte [], IV: Byte [])|şifrelenecek giriş düz metin<br /><br />alg-şifreleme algoritması<br /><br />anahtar şifreleme anahtarı<br /><br />IV-başlatma vektörü<br /><br />Şifrelenmiş düz metin döndürür.|
|Byte [] şifre çözme (giriş: Bu Byte [], alg: dize, anahtar: Byte [], IV: Byte [])|Şifresi çözülecek Input-şifresi üzerinde anlaşılamadı metni<br /><br />alg-simetrik şifreleme algoritmasının adı<br /><br />anahtar şifreleme anahtarı<br /><br />IV-başlatma vektörü<br /><br />Düz metin döndürür.|
|Byte [] şifre çözme (giriş: Bu Byte [], alg: System. Security. Cryptography. SymmetricAlgorithm)|Şifresi çözülecek Input-şifresi üzerinde anlaşılamadı metni<br /><br />alg-şifreleme algoritması<br /><br />Düz metin döndürür.|
|Byte [] şifre çözme (giriş: Bu Byte [], alg: System. Security. Cryptography. SymmetricAlgorithm, Key: Byte [], IV: Byte [])|Şifresi çözülecek Input-şifresi üzerinde anlaşılamadı metni<br /><br />alg-şifreleme algoritması<br /><br />anahtar şifreleme anahtarı<br /><br />IV-başlatma vektörü<br /><br />Düz metin döndürür.|
|bool Verifynoiptali (giriş: Bu System. Security. Cryptography. X509Certificates. X509Certificate2)|Sertifika iptal durumunu denetlemeden bir X. 509.440 zinciri doğrulaması gerçekleştirir.<br /><br />Giriş-sertifika nesnesi<br /><br />`true`Doğrulamanın başarılı olup olmadığını döndürür; `false` doğrulama başarısız olursa.|


## <a name="next-steps"></a>Sonraki adımlar

İlkelerle çalışma hakkında daha fazla bilgi için bkz.

+ [API Management ilkeler](api-management-howto-policies.md)
+ [API dönüştürme](transform-api.md)
+ İlke deyimlerinin ve ayarlarının tam listesi için [Ilke başvurusu](./api-management-policies.md)
+ [İlke örnekleri](policy-samples.md)
