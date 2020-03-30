---
title: 'Azure AD Connect: Tasarım kavramları | Microsoft Dokümanlar'
description: Bu konu, belirli uygulama tasarım alanlarını ayrıntılarıyla
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 4114a6c0-f96a-493c-be74-1153666ce6c9
ms.service: active-directory
ms.custom: azure-ad-connect
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 08/10/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb41e14a7ecf41a2698a063c3067a98d8acf8f07
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253890"
---
# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Tasarım kavramları
Bu belgenin amacı, Azure AD Connect'in uygulama tasarımı sırasında düşünülmesi gereken alanları açıklamaktır. Bu belge belirli alanlarda derin bir dalış ve bu kavramlar kısaca diğer belgelerde de açıklanmıştır.

## <a name="sourceanchor"></a>kaynakÇapa
SourceAnchor *özniteliği, bir nesnenin ömrü boyunca değişmez bir öznitelik*olarak tanımlanır. Bir nesneyi şirket içinde ve Azure AD'de aynı nesne olarak benzersiz olarak tanımlar. Öznitelik de **değişmezId** denir ve iki ad değiştirilebilir kullanılır.

"Değiştirilemez" sözcüğü bu belge için önemlidir. Bu özniteliğin değeri ayarlandıktan sonra değiştirilemeyeceğinden, senaryonuzu destekleyen bir tasarım seçmek önemlidir.

Öznitelik aşağıdaki senaryolar için kullanılır:

* Yeni bir eşitleme motoru sunucusu inşa edildiğinde veya olağanüstü durum kurtarma senaryosundan sonra yeniden oluşturulduğunda, bu öznitelik Azure AD'deki varolan nesneleri şirket içi nesnelerle bağlar.
* Yalnızca bulut kimliğinden eşitlenmiş bir kimlik modeline geçerseniz, bu özellik nesnelerin Azure AD'deki varolan nesneleri şirket içi nesnelerle "sert eşleştirmesine" olanak tanır.
* Federasyon kullanıyorsanız, bu öznitelik **userPrincipalName** ile birlikte bir kullanıcıyı benzersiz olarak tanımlamak için yapılan claim'de kullanılır.

Bu konu sadece kullanıcılarla ilgili olarak sourceAnchor hakkında konuşuyor. Aynı kurallar tüm nesne türleri için geçerlidir, ancak bu sorun genellikle bir sorun sadece kullanıcılar içindir.

### <a name="selecting-a-good-sourceanchor-attribute"></a>İyi bir kaynakSeçmeÇapa özniteliği
Öznitelik değeri aşağıdaki kurallara uymalıdır:

* Uzunluğu 60 karakterden az
  * A-z, A-Z veya 0-9 olmayan karakterler kodlanır ve 3 karakter olarak sayılır
* Özel bir karakter içermez: &#92; ! # $ % & * + / = ? ^ &#96; { } | ~ < >  ( ) ' ( ) ( : , [ ] " \@ _
* Genel olarak benzersiz olması gerekir
* Bir dize, tamsayı veya ikili olmalıdır
* Bunlar değişebildiği için kullanıcının adına dayalı olmamalıdır
* Büyük/küçük harf duyarlı olmamalı ve duruma göre değişebilecek değerlerden kaçınmalı
* Nesne oluşturulduğunda atanmalı

Seçili kaynakÇapa tür dizesi değilse, azure AD Connect Base64Özel karakter görünmemesini sağlamak için öznitelik değerini encode. ADFS'den başka bir federasyon sunucusu kullanıyorsanız, sunucunuzun özniteliği de Base64Encode'a dayandırdığından emin olun.

Kaynak Çapa özniteliği büyük/küçük harf duyarlıdır. "JohnDoe"nun değeri "johndoe" ile aynı şey değildir. Ancak, yalnızca bir fark olan iki farklı nesneniz olmamalıdır.

Şirket içinde tek bir orman varsa, kullanmanız gereken öznitelik **objectGUID'dir.** Bu aynı zamanda Azure AD Connect'te ifade ayarlarını ve DirSync tarafından kullanılan özniteliği kullandığınızda kullanılan özniteliktir.

Birden çok ormanA sahipseniz ve kullanıcıları ormanlar ve etki alanları arasında taşımıyorsa, **objectGUID** bu durumda bile kullanmak için iyi bir özelliktir.

Kullanıcıları ormanlar ve etki alanları arasında taşırsanız, taşıma sırasında kullanıcılarla birlikte değişmeyen veya kullanıcılarla birlikte taşınabilen bir öznitelik bulmanız gerekir. Önerilen bir yaklaşım sentetik bir özellik tanıtmaktır. GUID gibi görünen bir şeyi tutabilecek bir öznitelik uygun olacaktır. Nesne oluşturma sırasında, yeni bir GUID oluşturulur ve kullanıcı üzerinde damgalanır. **NesneGUID'i** temel alan bu değeri oluşturmak ve ADDS'te seçili özniteliği güncelleştirmek için eşitleme motoru sunucusunda özel bir eşitleme kuralı oluşturulabilir. Nesneyi taşırken, bu değerin içeriğini de kopyaladiğinizden emin olun.

Başka bir çözüm, değişmediğini bildiğiniz varolan bir özniteliği seçmektir. Sık kullanılan öznitelikler **employeeID**içerir. Harfleri içeren bir öznitelik düşünüyorsanız, özniteliğin değeri için büyük /büyük harf e-postanın değişme olasılığını olmadığından emin olun. Kullanılmaması gereken kötü öznitelikler, kullanıcının adı ile bu öznitelikleri içerir. Bir evlilik veya boşanma, adı bu öznitelik için izin verilmez değişmesi bekleniyor. Bu **da, userPrincipalName,** **mail**ve **targetAddress** gibi özniteliklerin Azure AD Connect yükleme sihirbazında seçilmesinin bile mümkün olmamasının nedenlerinden biridir. Bu öznitelikler,\@kaynakÇapa'da izin verilmeyen " " karakterini de içerir.

### <a name="changing-the-sourceanchor-attribute"></a>KaynakÇapa özniteliğini değiştirme
Kaynak Çapa özniteliği değeri, nesne Azure AD'da oluşturulduktan ve kimlik eşitlendikten sonra değiştirilemez.

Bu nedenle, aşağıdaki kısıtlamalar Azure AD Connect için geçerlidir:

* SourceAnchor özniteliği yalnızca ilk yükleme sırasında ayarlanabilir. Yükleme sihirbazını yeniden çalıştırSanız, bu seçenek salt okunur. Bu ayarı değiştirmeniz gerekiyorsa, kaldırmanız ve yeniden yüklemeniz gerekir.
* Başka bir Azure AD Connect sunucusu yüklerseniz, daha önce kullanılan kaynakÇapa özniteliğini seçmeniz gerekir. Daha önce DirSync kullanıyorsanız ve Azure AD Connect'e geçtiyseniz, DirSync tarafından kullanılan öznitelik bu olduğundan **objectGUID'i** kullanmanız gerekir.
* KaynakAnchor değeri, nesne Azure AD'ye dışa aktarıldıktan sonra değiştirilirse, Azure AD Connect eşitleme bir hata yapar ve sorun giderilmeden ve kaynak Çapa kaynağında yeniden değiştirilmeden önce bu nesneüzerinde daha fazla değişiklik yapılmasına izin vermez Dizin.

## <a name="using-ms-ds-consistencyguid-as-sourceanchor"></a>ms-DS-ConsistencyGuid'i kaynak Olarak KullanmaÇapa
Varsayılan olarak, Azure AD Connect (sürüm 1.1.486.0 ve üzeri) kaynakÇapa özniteliği olarak objectGUID'i kullanır. ObjectGUID sistem tarafından oluşturulur. Şirket içi AD nesneleri oluştururken değerini belirtemezsiniz. Bölüm [kaynağıAnchor](#sourceanchor)açıklandığı gibi, kaynakÇapa değerini belirtmeniz gereken senaryolar vardır. Senaryolar sizin için geçerliyse, kaynak Çapa özniteliği olarak yapılandırılabilir bir AD özniteliği (örneğin, ms-DS-ConsistencyGuid) kullanmanız gerekir.

Azure AD Connect (sürüm 1.1.524.0 ve sonrası) artık kaynakÇapa özniteliği olarak ms-DS-ConsistencyGuid kullanımını kolaylaştırır. Azure AD Connect bu özelliği kullanırken eşitleme kurallarını otomatik olarak şu şekilde yapılandırır:

1. Ms-DS-ConsistencyGuid kullanıcı nesneleri için kaynakÇapa özniteliği olarak kullanın. ObjectGUID diğer nesne türleri için kullanılır.

2. ms-DS-ConsistencyGuid özniteliği doldurulmayan herhangi bir şirket içi AD Kullanıcı nesnesi için Azure AD Connect objectGUID değerini yerinde Active Directory'deki ms-DS-ConsistencyGuid özniteliğine geri yazar. ms-DS-ConsistencyGuid özniteliği doldurulduktan sonra, Azure AD Connect nesneyi Azure AD'ye dışa dışa dışa dışa aktarmaz.

>[!NOTE]
> Şirket içi bir AD nesnesi Azure AD Connect'e alındıktan sonra (diğer bir deyişle, AD Bağlayıcı Sıtkı Alanına aktarıldıktan ve Metaverse'e yansıtıldıktan sonra), kaynağıAnchor değerini artık değiştiremezsiniz. Belirli bir şirket içi AD nesnesinin kaynak Çapa değerini belirtmek için, Azure AD Connect'e aktarılmadan önce ms-DS-ConsistencyGuid özniteliğini yapılandırın.

### <a name="permission-required"></a>İzin gerekli
Bu özelliğin çalışması için, şirket içi Active Directory ile senkronize etmek için kullanılan AD DS hesabının, şirket içi Active Directory'deki ms-DS-ConsistencyGuid özniteliğine yazma izni verilmelidir.

### <a name="how-to-enable-the-consistencyguid-feature---new-installation"></a>ConsistencyGuid özelliği nasıl etkinleştirilir - Yeni yükleme
Yeni yükleme sırasında SourceAnchor olarak ConsistencyGuid kullanımını etkinleştirebilirsiniz. Bu bölüm, hem Ekspres hem de Özel yüklemeyi ayrıntılarıyla kapsar.

  >[!NOTE]
  > Azure AD Connect'in yalnızca daha yeni sürümleri (1.1.524.0 ve sonrası) yeni yükleme sırasında SourceAnchor olarak ConsistencyGuid kullanımını destekler.

### <a name="how-to-enable-the-consistencyguid-feature"></a>ConsistencyGuid özelliği nasıl etkinleştirilir?

#### <a name="express-installation"></a>Ekspres Kurulum
Azure AD Connect ile Express modunu yüklerken, Azure AD Connect sihirbazı aşağıdaki mantığı kullanarak kaynak Çapa özniteliği olarak kullanılacak en uygun AD özniteliğini otomatik olarak belirler:

* İlk olarak, Azure AD Connect sihirbazı, önceki Azure AD Connect yüklemesinde (varsa) kaynak Çapa özniteliği olarak kullanılan AD özniteliğini almak için Azure AD kiracınızı sorgular. Bu bilgiler varsa, Azure AD Connect aynı AD özniteliğini kullanır.

  >[!NOTE]
  > Yalnızca Azure AD Connect'in yalnızca daha yeni sürümleri (1.1.524.0 ve sonrası) yükleme sırasında kullanılan kaynak Çapa özniteliği hakkında Azure AD kiracınızda bilgi depolar. Azure AD Connect'in eski sürümleri nde değildir.

* Kullanılan kaynakÇapa özniteliği hakkında bilgi yoksa, sihirbaz şirket içi Active Directory'nizde ms-DS-TutarlılıkGuid özniteliğinin durumunu denetler. Öznitelik dizindeki herhangi bir nesne üzerinde yapılandırılmamışsa, sihirbaz kaynakÇapa özniteliği olarak ms-DS-ConsistencyGuid kullanır. Öznitelik dizindeki bir veya daha fazla nesne üzerinde yapılandırılırsa, sihirbaz özniteliğin diğer uygulamalar tarafından kullanıldığı sonucuna varır ve kaynakÇapa özniteliği olarak uygun değildir...

* Bu durumda, sihirbaz kaynakÇapa özniteliği olarak objectGUID kullanarak geri düşer.

* Kaynak Çapa özelliği ne karar verildikten sonra sihirbaz bilgileri Azure AD kiracınızda saklar. Bilgiler, Azure AD Connect'in gelecekteki yüklemesi tarafından kullanılacaktır.

Express yüklemesi tamamlandıktan sonra sihirbaz, Kaynak Bağlantı özelliği olarak hangi özniteliğin seçildiğini bildirir.

![Sihirbaz kaynakAnchor için seçilen AD özniteliği bildirir](./media/plan-connect-design-concepts/consistencyGuid-01.png)

#### <a name="custom-installation"></a>Özel kurulum
Azure AD Connect'i Özel modla yüklerken, Kaynak Çapa özniteliğini yapılandırırken Azure AD Connect sihirbazı iki seçenek sağlar:

![Özel kurulum - sourceAnchor yapılandırması](./media/plan-connect-design-concepts/consistencyGuid-02.png)

| Ayar | Açıklama |
| --- | --- |
| Kaynak bağlantısını benim için Azure yönetsin | Azure AD’nin sizin için özniteliği seçmesini istiyorsanız bu seçeneği belirleyin. Bu seçeneği seçerseniz, Azure AD Connect sihirbazı [Express yüklemesi sırasında kullanılan aynı kaynakÇapa öznitelik seçim mantığını](#express-installation)uygular. Express yüklemesine benzer şekilde sihirbaz, Özel yükleme tamamlandıktan sonra Kaynak Bağlantı özelliği olarak hangi öznitelik alındığını bildirir. |
| Belirli bir öznitelik | SourceAnchor özniteliği olarak mevcut bir AD özniteliğini belirtmek istiyorsanız bu seçeneği belirleyin. |

### <a name="how-to-enable-the-consistencyguid-feature---existing-deployment"></a>ConsistencyGuid özelliği nasıl etkinleştirilir - Varolan dağıtım
Kaynak Çapa özniteliği olarak objectGUID'i kullanan varolan bir Azure AD Connect dağıtımınız varsa, bunun yerine ConsistencyGuid'i kullanmaya geçebilirsiniz.

>[!NOTE]
> Yalnızca Azure AD Connect'in (1.1.552.0 ve sonrası) daha yeni sürümleri Kaynak Çapa özniteliği olarak ObjectGuid'den ConsistencyGuid'e geçişi destekler.

Kaynak Bağlantı Özelliği özniteliği olarak objectGUID'den ConsistencyGuid'e geçmek için:

1. Azure AD Connect sihirbazını başlatın ve Görevler ekranına gitmek için **Yapıl'ı** tıklatın.

2. Kaynak **Çapayı Yapılandır'ı** seçin ve **İleri'yi**tıklatın.

   ![Varolan dağıtım için Tutarlılığı EtkinleştirGuid - adım 2](./media/plan-connect-design-concepts/consistencyguidexistingdeployment01.png)

3. Azure AD Yöneticisi kimlik bilgilerinizi girin ve **İleri'yi**tıklatın.

4. Azure AD Connect sihirbazı, şirket içi Active Directory'nizde ms-DS-ConsistencyGuid özniteliğinin durumunu analiz eder. Öznitelik dizindeki herhangi bir nesne üzerinde yapılandırılmamışsa, Azure AD Connect başka hiçbir uygulamanın şu anda bu özelliği kullanmadığı ve Kaynak Bağlantı özniteliği olarak kullanılmasının güvenli olduğu sonucuna varır. Devam etmek için **İleri** 'ye tıklayın.

   ![Varolan dağıtım için Tutarlılığı EtkinleştirGuid - adım 4](./media/plan-connect-design-concepts/consistencyguidexistingdeployment02.png)

5. **Yapılandırmaya Hazır** ekranında, yapılandırma değişikliğini yapmak için **Yapılandırma'yı** tıklatın.

   ![Varolan dağıtım için Tutarlılığı EtkinleştirGuid - adım 5](./media/plan-connect-design-concepts/consistencyguidexistingdeployment03.png)

6. Yapılandırma tamamlandıktan sonra sihirbaz, ms-DS-ConsistencyGuid'in artık Kaynak Bağlantı Özelliği olarak kullanıldığını gösterir.

   ![Varolan dağıtım için Tutarlılığı EtkinleştirGuid - adım 6](./media/plan-connect-design-concepts/consistencyguidexistingdeployment04.png)

Çözümleme sırasında (adım 4), öznitelik dizindeki bir veya daha fazla nesne üzerinde yapılandırılırsa, sihirbaz özniteliğin başka bir uygulama tarafından kullanıldığı sonucuna varır ve aşağıdaki diyagramda gösterildiği gibi bir hata döndürür. Birincil Azure AD Connect sunucunuzda TutarlılıkGuid özelliğini daha önce etkinleştirdiyseniz ve aynı şeyi hazırlama sunucunuzda da yapmaya çalışıyorsanız, bu hata da oluşabilir.

![Varolan dağıtım için Tutarlılığı EtkinleştirGuid - hata](./media/plan-connect-design-concepts/consistencyguidexistingdeploymenterror.png)

 Özniteliğin diğer varolan uygulamalar tarafından kullanılmadığından eminseniz, Azure AD Connect sihirbazını **/SkipLdapSearch** anahtarıyla yeniden başlatarak hatayı bastırabilirsiniz. Bunu yapmak için aşağıdaki komut isteminde aşağıdaki komut u komutunu çalıştırın:

```
"c:\Program Files\Microsoft Azure Active Directory Connect\AzureADConnect.exe" /SkipLdapSearch
```

### <a name="impact-on-ad-fs-or-third-party-federation-configuration"></a>AD FS veya üçüncü taraf federasyon yapılandırması üzerindeki etkisi
Şirket içi AD FS dağıtımını yönetmek için Azure AD Connect kullanıyorsanız, Azure AD Connect, kaynakÇapa ile aynı AD özniteliğini kullanmak için talep kurallarını otomatik olarak güncelleştirir. Bu, ADFS tarafından oluşturulan Değişmez KIMLIK talebinin Azure AD'ye dışa aktarılan kaynak Çapa değerleriyle tutarlı olmasını sağlar.

AD FS'yi Azure AD Connect dışında yönetiyorsanız veya kimlik doğrulaması için üçüncü taraf federasyon sunucularını kullanıyorsanız, ImmutableID iddiası için talep kurallarını, madde bölümünde açıklandığı gibi Azure [AD'ye](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-management#modclaims)aktarılan kaynakAnchor değerleriyle tutarlı olması için el ile güncelleştirmeniz gerekir. Sihirbaz yükleme tamamlandıktan sonra aşağıdaki uyarıyı döndürür:

![Üçüncü taraf federasyon yapılandırması](./media/plan-connect-design-concepts/consistencyGuid-03.png)

### <a name="adding-new-directories-to-existing-deployment"></a>Varolan dağıtıma yeni dizinler ekleme
Azure AD Connect'i TutarlıGuid özelliği etkinleştirilmiş olarak dağıtdığınızı ve şimdi dağıtıma başka bir dizin eklemek istediğinizi varsayalım. Dizini eklemeyi denediğinizde, Azure AD Connect sihirbazı dizindeki ms-DS TutarlılığıGuid özniteliğinin durumunu denetler. Öznitelik dizindeki bir veya daha fazla nesne üzerinde yapılandırılırsa, sihirbaz özniteliğin diğer uygulamalar tarafından kullanıldığı sonucuna varır ve aşağıdaki diyagramda gösterildiği gibi bir hata döndürür. Özniteliğin varolan uygulamalar tarafından kullanılmadığından eminseniz, Azure AD Connect sihirbazını yukarıda açıklandığı gibi belirtilen **/SkipLdapSearch** anahtarıyla yeniden başlatarak hatayı bastırabilirsiniz veya daha fazla bilgi için Destek'e başvurmanız gerekir.

![Varolan dağıtıma yeni dizinler ekleme](./media/plan-connect-design-concepts/consistencyGuid-04.png)

## <a name="azure-ad-sign-in"></a>Azure AD oturum açma
Şirket içi dizininizi Azure AD ile tümleştirmekle birlikte, eşitleme ayarlarının kullanıcının kimlik doğrulama biçimini nasıl etkileyebileceğini anlamak önemlidir. Azure AD, kullanıcının kimliğini doğrulamak için userPrincipalName (UPN) kullanır. Ancak, kullanıcılarınızı eşitlediğinizde, userPrincipalName değeri için kullanılacak özniteliği dikkatle seçmeniz gerekir.

### <a name="choosing-the-attribute-for-userprincipalname"></a>userPrincipalName için öznitelik seçimi
Azure'da kullanılacak UPN değerini sağlamak için özniteliği seçerken,

* Öznitelik değerleri UPN sözdizimine (RFC 822) uygundur, yani biçim\@kullanıcı adı etki alanı olmalıdır
* Değerlerdeki sonek, Azure AD'deki doğrulanmış özel etki alanlarından biriyle eşleşir

Ekspres ayarlarda, öznitelik için varsayılan seçim userPrincipalName'dir. userPrincipalName özniteliği kullanıcılarınızın Azure'da oturum açmasını istediğiniz değeri içermiyorsa, **Özel Yükleme'yi**seçmeniz gerekir.

### <a name="custom-domain-state-and-upn"></a>Özel etki alanı durumu ve UPN
UPN soneki için doğrulanmış bir etki alanı olduğundan emin olmak önemlidir.

John contoso.com bir kullanıcıdır. Kullanıcıları Azure REKLAM dizininizle senkronize\@ettikten sonra Azure'da oturum açmak için şirket içi UPN john contoso.com'ni kullanmasını contoso.onmicrosoft.com. Bunu yapmak için, kullanıcıları eşitlemeye başlamadan önce Azure AD'de özel bir etki alanı olarak contoso.com eklemeniz ve doğrulamanız gerekir. Örneğin contoso.com John'un UPN soneki Azure AD'de doğrulanmış bir etki alanıyla eşleşmiyorsa, Azure AD UPN sonekinin yerine contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Azure AD için şirket içinde routable olmayan etki alanları ve UPN
Bazı kuruluşların contoso.local veya contoso gibi basit tek etiketli etki alanları gibi routable olmayan etki alanları vardır. Azure AD'de routable olmayan bir etki alanını doğrulayamıyorsunuz. Azure AD Connect, Azure AD'de yalnızca doğrulanmış bir etki alanıyla eşitlenebilir. Bir Azure REKLAM dizini oluşturduğunuzda, örneğin Azure REKLAM'ınız için varsayılan etki alanı haline gelen bir contoso.onmicrosoft.com etki alanı oluşturur. Bu nedenle, varsayılan onmicrosoft.com etki alanıyla eşitlemek istemediğiniz durumlarda, böyle bir senaryoda başka bir routable etki alanını doğrulamak gerekir.

Etki alanları ekleme ve doğrulama hakkında daha fazla bilgi için [özel alan adınızı Azure Etkin Dizin'e ekle'yi](../active-directory-domains-add-azure-portal.md) okuyun.

Azure AD Connect, rahatsız edilemeyen bir etki alanı ortamında çalışıyorsanız algılar ve sizi açık ayarlarla devam etmeniz konusunda uygun şekilde uyarır. Eğer routable olmayan bir etki alanında çalışıyorsanız, o zaman upn, kullanıcıların, çok routable olmayan sonekler olması muhtemeldir. Örneğin, contoso.local altında çalışıyorsanız, Azure AD Connect ekspres ayarları kullanmak yerine özel ayarları kullanmanızı önerir. Özel ayarları kullanarak, kullanıcılar Azure AD ile senkronize edildikten sonra Azure'da oturum açmak için UPN olarak kullanılması gereken özniteliği belirtebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
