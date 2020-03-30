---
title: Kullanıcı kaydı ve ürün aboneliği nasıl devrlenir?
description: Azure API Yönetimi'nde kullanıcı kaydı ve ürün aboneliğini üçüncü bir tarafa nasıl devredin öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.assetid: 8b7ad5ee-a873-4966-a400-7e508bbbe158
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/04/2019
ms.author: apimpm
ms.openlocfilehash: a69babdf2fffb4cb9d963f1806f3c85755e50294
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74454347"
---
# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Kullanıcı kaydı ve ürün aboneliği nasıl devrlenir?

Temsilcilik, geliştirici portalındaki yerleşik işlevselliği kullanmanın aksine, mevcut web sitenizi geliştirici oturum açma/kaydolma ve ürünlere abonelik işlemlerini işlemek için kullanmanıza olanak tanır. Web sitenizin kullanıcı verilerine sahip olmasını ve bu adımların doğrulanmasını özel bir şekilde gerçekleştirmesini sağlar.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="delegating-developer-sign-in-and-sign-up"></a><a name="delegate-signin-up"> </a>Geliştirici oturum açma ve kaydolma atama

Geliştiriciyi temsilci olarak vermek, oturum açmak ve mevcut web sitenize kaydolmak için sitenizde özel bir temsilci bitiş noktası oluşturmanız gerekir. API Yönetimi geliştirici portalından başlatılan bu tür bir istek için giriş noktası olarak hareket etmesi gerekir.

Son iş akışı aşağıdaki gibi olacaktır:

1. Geliştirici oturum açma düğmesine tıklar veya API Yönetimi geliştirici portalındaki bağlantıya kaydolun
2. Tarayıcı temsilci bitiş noktasına yönlendirilir
3. Buna karşılık temsilci bitiş noktası kullanıcıdan oturum açmasını veya kaydolmasını isteyen Kullanıcı Arabirimi'ne yönlendirir veya sunar
4. Başarı üzerine, kullanıcı başlattıkları API Yönetimi geliştirici portalı sayfasına geri yönlendirilir

Başlamak için, önce api yönetimini, istekleri temsilci bitiş noktanız üzerinden yönlendirmek için ayarlayalım. Azure portalında, API Yönetimi kaynağınızda **Güvenlik'i** arayın ve ardından **Temsilci** öğesini tıklatın. 'Temsilci oturum açma & kaydol' seçeneğini etkinleştirmek için onay kutusunu tıklatın.

![Heyet sayfası][api-management-delegation-signin-up]

* Özel temsilcibitiş noktanızın URL'sinin ne olacağına karar verin ve **Temsilci bitiş noktası URL** alanına girin. 
* Temsilci kimlik doğrulama anahtarı alanında, isteğin gerçekten Azure API Yönetimi'nden geldiğinden emin olmak için doğrulama için size sağlanan bir imzayı hesaplamak için kullanılacak bir sır girin. API Yönetimi'nin sizin için rastgele bir anahtar oluşturması için **oluştur** düğmesini tıklatabilirsiniz.

Şimdi **delegasyon bitiş noktasını**oluşturmanız gerekir. Bir dizi eylem gerçekleştirmesi gerekiyor:

1. Aşağıdaki formda bir istek alın:
   
   > *http:\//www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl={KAYNAK SAYFANIN URL'si}&salt={string}&sig={string}*
   > 
   > 
   
    Oturum açma / kaydolma durumu için sorgu parametreleri:
   
   * **operasyon**: ne tür bir delegasyon isteği olduğunu tanımlar - bu durumda yalnızca **SignIn** olabilir
   * **returnUrl**: kullanıcının bir oturum açma veya kaydol bağlantısını tıklattığı sayfanın URL'si
   * **tuz**: bir güvenlik karma hesaplamak için kullanılan özel bir tuz dizesi
   * **sig**: kendi hesaplanmış karma ile karşılaştırmak için kullanılacak bir hesaplanmış güvenlik karma
2. İsteğin Azure API Yönetimi'nden geldiğini doğrulayın (isteğe bağlı, ancak güvenlik için önerilir)
   
   * **ReturnUrl** ve **tuz** sorgu parametrelerini temel alan bir dize HMAC-SHA512 karma sını hesaplayın[(aşağıda verilen örnek kod):]
     
     > HMAC(**tuz** + '\n' + **returnUrl**)
     > 
     > 
   * Yukarıda hesaplanmış karma ile **sig** sorgu parametresinin değerini karşılaştırın. İki hashes eşleşirse, bir sonraki adıma geçin, aksi takdirde isteği reddedin.
3. Oturum açma/kaydolma için bir istek aldığınızı doğrulayın: **işlem** sorgu parametresi "**SignIn**" olarak ayarlanır.
4. Oturum açmak veya kaydolmak için kullanıcıya Kullanıcıya Kullanıcıya Kullanıcıya kullanıcıya oturum açma
5. Kullanıcı kaydoluyorsa, API Yönetimi'nde onlar için karşılık gelen bir hesap oluşturmanız gerekir. API Management REST API ile [bir kullanıcı oluşturun.] Bunu yaparken, kullanıcı kimliğini kullanıcı mağazanızdakiyle aynı değere veya takip edebilirsiniz bir kimlik için ayarladığınızdan emin olun.
6. Kullanıcı başarıyla doğrulandığında:
   
   * API Management REST API aracılığıyla [tek oturum açma (SSO) belirteci isteyin]
   * yukarıdaki API çağrısından aldığınız SSO URL'sine bir returnUrl sorgu parametresi ekleyin:
     
     > örneğin,https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 
     > 
     > 
   * kullanıcıyı yukarıdaki üretilen URL'ye yönlendirin

**SignIn** işlemine ek olarak, önceki adımları izleyerek ve aşağıdaki işlemlerden birini kullanarak hesap yönetimini de gerçekleştirebilirsiniz:

* **Changepassword**
* **ChangeProfile**
* **Kapanış Hesabı**

Hesap yönetimi işlemleri için aşağıdaki sorgu parametrelerini geçmeniz gerekir.

* **operation**: ne tür bir delegasyon isteği olduğunu tanımlar (ChangePassword, ChangeProfile veya CloseAccount)
* **userId**: yönetilen hesabın kullanıcı kimliği
* **tuz**: bir güvenlik karma hesaplamak için kullanılan özel bir tuz dizesi
* **sig**: kendi hesaplanmış karma ile karşılaştırmak için kullanılacak bir hesaplanmış güvenlik karma

## <a name="delegating-product-subscription"></a><a name="delegate-product-subscription"> </a>Ürün aboneliğini alegating
Ürün aboneliğini atamak, kullanıcı oturum açma/-yukarı atamaya benzer şekilde çalışır. Son iş akışı aşağıdaki gibi olacaktır:

1. Geliştirici, API Yönetimi geliştirici portalında bir ürün seçer ve Abone Ol düğmesini tıklatıyor.
2. Tarayıcı temsilci bitiş noktasına yönlendirilir.
3. Temsilci bitiş noktası gerekli ürün abonelik adımlarını gerçekleştirir. Adımları tasarlamak size kalmış. Bunlar arasında fatura bilgileri istemek için başka bir sayfaya yönlendirme, ek sorular sorma veya yalnızca bilgileri depolamave herhangi bir kullanıcı eylemi gerektirmemesi yer alabilir.

İşlevselliği etkinleştirmek **için, Temsilci** sayfasında **Temsilci ürün aboneliğini**tıklatın.

Ardından, delegasyon bitiş noktasının aşağıdaki eylemleri yaptığından emin olun:

1. Aşağıdaki formda bir istek alın:
   
   > *http:\//www.yourwebsite.com/apimdelegation?operation={operation}&productId={product tosubscribe}&userId={user making request}&salt={string}&sig={string}*
   >
   
    Ürün aboneliği örneği için sorgu parametreleri:
   
   * **operation**: ne tür bir delegasyon isteği olduğunu belirler. Ürün aboneliği istekleri için geçerli seçenekler şunlardır:
     * "Abone ol": Kullanıcıyı sağlanan kimlikle belirli bir ürüne abone etme isteği (aşağıya bakın)
     * "Aboneliği iptal et": Bir kullanıcının ürünaboneliğini iptal etme isteği
     * "Yenileme": aboneliği yenileme isteği (örneğin, süresi doluyor olabilir)
   * **productId**: Kullanıcının abone olmasını istediği ürünün kimliği
   * **subscriptionId**: *Abonelikten çık* ve *yenile* - ürün aboneliğinin kimliği
   * **userId**: kullanıcının kimliği için istek yapılır
   * **tuz**: bir güvenlik karma hesaplamak için kullanılan özel bir tuz dizesi
   * **sig**: kendi hesaplanmış karma ile karşılaştırmak için kullanılacak bir hesaplanmış güvenlik karma

2. İsteğin Azure API Yönetimi'nden geldiğini doğrulayın (isteğe bağlı, ancak güvenlik için önerilir)
   
   * **ProductId,** **userId**ve **tuz** sorgu parametrelerini temel alan bir dize hmac-SHA512 hesaplama:
     
     > HMAC(**tuz** + '\n' + **productId** + '\n' + **userId**)
     > 
     > 
   * Yukarıda hesaplanmış karma ile **sig** sorgu parametresinin değerini karşılaştırın. İki hashes eşleşirse, bir sonraki adıma geçin, aksi takdirde isteği reddedin.
3. Ürün **aboneliğini, işletmede** talep edilen işlem türüne göre işleme ( örneğin, faturalandırma, diğer sorular, vb.)
4. Kullanıcıyı sizin tarafınızdaki ürüne başarıyla abone olarak, [abonelikler için REST API'yi arayarak]kullanıcıyı API Management ürününe abone olun.

## <a name="example-code"></a><a name="delegate-example-code"> </a> Örnek Kod

Bu kod örnekleri nasıl yapılacağını gösterir:

* Yayıncı portalının Temsilci ekranında ayarlanan *temsilci doğrulama anahtarını*alın
* Daha sonra imzayı doğrulamak için kullanılan ve geçirilen returnUrl'nin geçerliliğini kanıtlayan bir HMAC oluşturun.

Aynı kod hafif bir değişiklik ile productId ve userId için çalışır.

**C# kodu returnUrl karma oluşturmak için**

```csharp
using System.Security.Cryptography;

string key = "delegation validation key";
string returnUrl = "returnUrl query parameter";
string salt = "salt query parameter";
string signature;
using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
{
    signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
}
```

**NodeJS kodu returnUrl karma oluşturmak için**

```
var crypto = require('crypto');

var key = 'delegation validation key'; 
var returnUrl = 'returnUrl query parameter';
var salt = 'salt query parameter';

var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
var digest = hmac.update(salt + '\n' + returnUrl).digest();
// change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
// compare signature to sig query parameter

var signature = digest.toString('base64');
```

> [!IMPORTANT]
> Temsilci değişikliklerinin etkili olması için [geliştirici portalını yeniden yayımlamanız](api-management-howto-developer-portal-customize.md#publish) gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Temsilci lik hakkında daha fazla bilgi için aşağıdaki videoya bakın:

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign in and sign up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[tek oturum açma (SSO) belirteci isteyin]: https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/User/GenerateSsoUrl
[bir kullanıcı oluşturma]: https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/user/createorupdate
[abonelikler için REST API'yi arama]: https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/subscription/createorupdate
[Next steps]: #next-steps
[aşağıda verilen örnek kod]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 
