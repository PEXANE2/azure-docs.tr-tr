---
title: Hata kodu başvurusu
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C hizmeti tarafından döndürülebilen hata kodlarının listesi.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: troubleshooting
ms.date: 01/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 5b09b3c19ab1c5b23e56e25afc1d9631cd1caa68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188673"
---
# <a name="error-codes-azure-active-directory-b2c"></a>Hata kodları: Azure Active Directory B2C

Aşağıdaki hatalar Azure Active Directory B2C hizmeti tarafından döndürülebilir.

| Hata kodu | İleti |
| ---------- | ------- |
| `AADB2C90002` | CORS kaynak{0}' ' bir 404 bulunamadı döndü. |
| `AADB2C90006` | Uri '{0}' nin yeniden yönlendirilen talebi müşteri{1}kimliği için kayıtlı değildir ' . |
| `AADB2C90007` | Müşteri kimliği ' '{0}ile ilişkili uygulamanın kayıtlı yönlendirme URI'leri yoktur. |
| `AADB2C90008` | İstek bir istemci kimlik parametresi içermez. |
| `AADB2C90010` | İstek bir kapsam parametresi içermez. |
| `AADB2C90011` | İstemede{0}verilen müşteri kimliği ' ' ile{1}eşleşmez' ilkesine kaydedilir. |
| `AADB2C90012` | Talepte{0}sağlanan kapsam ' ' desteklenmez. |
| `AADB2C90013` | İstenilen yanıt{0}türü ' ' ' sağlanan istek desteklenmez. |
| `AADB2C90014` | İstenilen yanıt{0}modu ' ' ' sağlanan istek desteklenmez. |
| `AADB2C90016` | İstenen istemci sit türü ' '{0}{1}beklenen tür ' ile eşleşmiyor. |
| `AADB2C90017` | İstekte sağlanan istemci iddiası geçersizdir:{0} |
| `AADB2C90018` | Müşteri kimliği{0}' ' belirtilen istekte kiracı{1}' kayıtlı değildir. |
| `AADB2C90019` | Id ' '{0}ile anahtar{1}konteynerinde kiracı ' ' geçerli bir anahtarı yok. Sebep: {2}. |
| `AADB2C90021` | Teknik profil{0}' ' ' '{1}in '{2}' ' ' politikasında kiracı ' bulunmamaktadır. |
| `AADB2C90022` | ''{0}in ' in'in{1}' ilkesi için meta verileri döndürülemez. |
| `AADB2C90023` | Profil{0}' ' ' gerekli meta{1}veri anahtarı ' 'içermez. |
| `AADB2C90025` | Profil{0}' '{1}in policy{2}' ' in tenant '{3}' ' ' gerekli şifreleme anahtarı ' içermez. |
| `AADB2C90027` | ' '{0}için belirtilen temel kimlik bilgileri geçersizdir. Kimlik bilgilerinin doğru olup olmadığını ve kaynak tarafından erişim verildiğini denetleyin. |
| `AADB2C90028` | ' '{0}için belirtilen istemci sertifikası geçersizdir. Sertifikanın doğru olup olmadığını, özel bir anahtar içerdiğini ve kaynak tarafından erişim verildiğini denetleyin. |
| `AADB2C90031` | İlke{0}' ' ' varsayılan bir kullanıcı yolculuğu belirtmez. İlkenin veya ebeveynlerinin varsayılan bir kullanıcı yolculuğunu güvenilen bir parti bölümünün parçası olarak belirttiğinden emin olun. |
| `AADB2C90035` | Hizmet geçici olarak kullanılamıyor. Lütfen birkaç dakika sonra tekrar deneyin. |
| `AADB2C90036` | İstek, kullanıcıyı oturum açma sonrası göndermek için yönlendirmek için bir URI içermez. post_logout_redirect_uri parametre alanında bir URI belirtin. |
| `AADB2C90037` | İstek işlenirken bir hata oluştu. Lütfen erişmeye çalıştığınız sitenin yöneticisiyle iletişime geçin. |
| `AADB2C90039` | İstek bir istemci iddiası içerir, ancak{0}sağlanan ilke ' ' in tenant '{1}' ' RelyingPartyPolicy'de bir client_secret eksiktir. |
| `AADB2C90040` | Kullanıcı yolculuğu{0}' ' ' bir gönderme talebi adımı içermez. |
| `AADB2C90043` | İstekte yer alan istem geçersiz değerleri içerir. Beklenen 'yok', 'giriş', 'rıza' veya 'select_account'. |
| `AADB2C90044` | İddia '{0}' ' ' ' ile{1}talep çözümleyicisi tarafından desteklenmez. |
| `AADB2C90046` | Şu anki durumunuzu yüklemekte sorun yaşıyoruz. Oturumunuza en baştan başlamayı deneyebilirsiniz. |
| `AADB2C90047` | Kaynak '{0}' ' ' yüklenmesini engelleyen komut dosyası hataları içerir. |
| `AADB2C90048` | Sunucuda işlenmemiş bir özel durum oluştu. |
| `AADB2C90051` | Uygun talep sağlayıcılar bulunamadı. |
| `AADB2C90052` | Geçersiz kullanıcı adı veya parola. |
| `AADB2C90053` | Belirtilen kimlik bilgisine sahip bir kullanıcı bulunamadı. |
| `AADB2C90054` | Geçersiz kullanıcı adı veya parola. |
| `AADB2C90055` | Scope '{0}' ' istekte sağlanan birhttps://example.com/calendar.readkaynak belirtmelidir, gibi ' ' . |
| `AADB2C90057` | Sağlanan uygulama OAuth Örtülü akışına izin verecek şekilde yapılandırılmamıştır. |
| `AADB2C90058` | Sağlanan uygulama ortak istemcilere izin verecek şekilde yapılandırılmamıştır. |
| `AADB2C90067` | Posta giriş idamesi{0}URI ' ' nin geçersiz bir biçimi vardır. ' 'https://example.com/returnveya yerel istemciler için IETF yerel istemcisi URI 'urn:ietf:wg:oauth:2.0:oob' gibi https tabanlı bir URL belirtin. |
| `AADB2C90068` | Kimlikli olarak sağlanan{0}uygulama ' ' ' bu hizmete karşı geçerli değildir. Lütfen B2C portalı üzerinden oluşturulan bir uygulamayı kullanın ve yeniden deneyin. |
| `AADB2C90075` | Talep değişimi{0}' ' '{1}' ' ' '{2}' '{3}ile http hata yanıtı ' ' ve Reason ' ' ile döndürülür. |
| `AADB2C90077` | Kullanıcının varolan bir oturumu yoktur ve istek istem parametresi '{0}'. |
| `AADB2C90079` | İstemciler, gizli bir ödeneği nialırken client_secret göndermelidir. |
| `AADB2C90080` | Sağlanan hibenin süresi doldu. Lütfen yeniden kimlik doğrulayın ve yeniden deneyin. Geçerli zaman: {0}, Grant {1}verilen süre: , {2}Grant sürgülü pencere son kullanma süresi: . |
| `AADB2C90081` | Belirtilen client_secret bu istemci için beklenen değerle eşleşmiyor. Lütfen client_secret düzeltin ve tekrar deneyin. |
| `AADB2C90083` | İstek gerekli parametre eksik: {0}. |
| `AADB2C90084` | Kamu müşterileri, kamuya açık bir hibeyi geri alırken client_secret göndermemelidir. |
| `AADB2C90085` | Hizmet bir iç hatayla karşılaştı. Lütfen yeniden kimlik doğrula ve yeniden deneyin. |
| `AADB2C90086` | Sağlanan grant_type [{0}] desteklenmez. |
| `AADB2C90087` | Sağlanan hibe protokol bitiş noktasının bu sürümü için verilmeyen. |
| `AADB2C90088` | Sağlanan hibe bu bitiş noktası için verilmedi. Gerçek Değer {0} : ve Beklenen Değer :{1} |
| `AADB2C90092` | Sağlanan kimlikli uygulama{0}' ' '{1}kiracı için devre dışı bırakılır. Lütfen uygulamayı etkinleştirin ve yeniden deneyin. |
| `AADB2C90107` | Kimlik ' '{0}' içeren uygulama, istekte openid kapsamı sağlanamadığı veya uygulama için yetkili olmadığı için kimlik belirteci alamaz. |
| `AADB2C90108` | ' ' '{0}' ' ' bir beklenen bir CpimIssuerTechnicalProfileReferenceId belirtmez. |
| `AADB2C90110` | Kapsam parametresi, 'id_token' içeren bir response_type talep ederken 'openid' içermelidir. |
| `AADB2C90111` | Hesabınız kilitlendi. Kilidini açmak için destek kişinize başvurun ve yeniden deneyin. |
| `AADB2C90114` | Yetkisiz kullanımı önlemek için hesabınız geçici olarak kilitlenir. Daha sonra tekrar deneyin. |
| `AADB2C90115` | 'Kod' response_type talep ederken, kapsam parametresi erişim belirteçleri için bir kaynak veya istemci kimliği ve kimlik belirteçleri için 'openid' içermelidir. Ayrıca, yenileme belirteçleri için 'offline_access' ekleyin. |
| `AADB2C90117` | ''{0}kapsamında sağlanan istek desteklenmez. |
| `AADB2C90118` | Kullanıcı parolasını unuttu. |
| `AADB2C90120` | Talepte belirtilen maksimum{0}yaş parametresi ' ' geçersizdir. Maksimum yaş ' ' ve{1}' '{2}' kapsayıcı arasında bir karşımat olmalıdır. |
| `AADB2C90122` | ' '{0}için giriş ' ' isteği alınan HTTP istek doğrulama başarısız oldu. Girişin < veya & gibi karakterler içermediğinden emin olun. |
| `AADB2C90128` | Bu hibeyle ilişkili hesap artık yok. Lütfen yeniden kimlik doğrula ve yeniden deneyin. |
| `AADB2C90129` | Sağlanan ödenek iptal edildi. Lütfen yeniden kimlik doğrula ve yeniden deneyin. |
| `AADB2C90145` | Doğrulanmamış telefon numarası bulunamadı ve ilke bir kullanıcının girilen numaraya izin vermiyor. |
| `AADB2C90146` | ''{0}istekte sağlanan kapsam, desteklenmeyen bir erişim belirteci için birden fazla kaynak belirtir. |
| `AADB2C90149` | Script{0}' ' ' ' yüklemek için başarısız oldu. |
| `AADB2C90151` | Kullanıcı, çok faktörlü kimlik doğrulama için yeniden denemeler için maksimum sayıyı aşmıştır. |
| `AADB2C90152` | Çok faktörlü bir anket isteği hizmetten yanıt alamadı. |
| `AADB2C90154` | Çok faktörlü doğrulama isteği hizmetten oturum kimliği alınamadı. |
| `AADB2C90155` | Çok faktörlü doğrulama isteği neden{0}'' ile başarısız oldu. |
| `AADB2C90156` | Çok faktörlü doğrulama isteği neden '{0}'ile başarısız oldu. |
| `AADB2C90157` | Kullanıcı, kendi kendini ileri süren bir adım için yeniden denemeler için maksimum sayıyı aşmıştır. |
| `AADB2C90158` | Kendi kendini ileri süren bir doğrulama isteği{0}neden '' ile başarısız oldu. |
| `AADB2C90159` | Kendi kendini ileri süren bir doğrulama{0}isteği neden '' ile başarısız oldu. |
| `AADB2C90161` | Kendi kendini ileri süren bir gönderme{0}yanıtı neden '' ile başarısız oldu. |
| `AADB2C90165` | ID ' '{0}ile başlatan SAML iletisi durumda bulunamaz. |
| `AADB2C90168` | HTTP-Yönlendirme isteği, imzalı bir istek için{0}gerekli parametre ' ' 'yi içermez. |
| `AADB2C90178` | İmza sertifikası{0}' ' ' özel anahtarı yoktur. |
| `AADB2C90182` | Sağlanan code_verifier ilişkili code_challenge eşleşmiyor |
| `AADB2C90183` | Sağlanan code_verifier geçersizdir |
| `AADB2C90184` | Sağlanan code_challenge_method desteklenmez. Desteklenen değerler düz veya S256 |
| `AADB2C90188` | SAML teknik profili{0}' ' ' ' bir{1}PartnerEntity URL ', ancak meta{2}veri alma neden ' 'ile başarısız olduğunu belirtir. |
| `AADB2C90194` | Talep{0}' ' ' için belirtilen taşıyıcı belirteci mevcut taleplerde mevcut değildir. Mevcut iddialar{1}' '. |
| `AADB2C90205` | Bu uygulama, işlemi gerçekleştirmek için bu web kaynağına karşı yeterli izinlere sahip değildir. |
| `AADB2C90206` | Bir zaman dışında istemci başlatma oluştu. |
| `AADB2C90208` | Sağlanan id_token_hint parametresi süresi doldu. Lütfen başka bir belirteç sağlayın ve yeniden deneyin. |
| `AADB2C90209` | Sağlanan id_token_hint parametresi kabul edilmiş bir hedef kitle içermez. Geçerli izleyici değerleri: '{0}'. Lütfen başka bir belirteç sağlayın ve yeniden deneyin. |
| `AADB2C90210` | Sağlanan id_token_hint parametresi doğrulanamadı. Lütfen başka bir belirteç sağlayın ve yeniden deneyin. |
| `AADB2C90211` | İstek eksik bir durum çerezi içeriyordu. |
| `AADB2C90212` | İstek geçersiz bir durum çerezi içeriyordu. |
| `AADB2C90220` | Kiracıdaki anahtar kapsayıcı{0}' ' ' '{1}depolama tanımlayıcısı ile ' var, ancak geçerli bir sertifika içermiyor. Sertifikanın süresi dolmuş olabilir veya sertifikanız gelecekte etkin hale gelebilir (nbf). |
| `AADB2C90223` | CORS kaynağını dezenfekte eden bir hata oluştu. |
| `AADB2C90224` | Uygulama için kaynak sahibi akışı etkinleştirildi. |
| `AADB2C90225` | İstekte sağlanan kullanıcı adı veya parola geçersizdir. |
| `AADB2C90226` | Belirtilen belirteç değişimi yalnızca HTTP POST üzerinden desteklenir. |
| `AADB2C90232` | Sağlanan id_token_hint parametresi kabul edilen bir veren içermez. Geçerli verenler:{0}' '. Lütfen başka bir belirteç sağlayın ve yeniden deneyin. |
| `AADB2C90233` | Sağlanan id_token_hint parametresi imza doğrulamabaşarısız oldu. Lütfen başka bir belirteç sağlayın ve yeniden deneyin. |
| `AADB2C90235` | Sağlanan id_token süresi doldu. Lütfen başka bir belirteç sağlayın ve yeniden deneyin. |
| `AADB2C90237` | Sağlanan id_token geçerli bir hedef kitle içermez. Geçerli izleyici değerleri: '{0}'. Lütfen başka bir belirteç sağlayın ve yeniden deneyin. |
| `AADB2C90238` | Sağlanan id_token geçerli bir veren içermez. Geçerli veren değerleri:{0}' '. Lütfen başka bir belirteç sağlayın ve yeniden deneyin. |
| `AADB2C90239` | Sağlanan id_token başarısız imza doğrulaması. Lütfen başka bir belirteç sağlayın ve yeniden deneyin. |
| `AADB2C90240` | Sağlanan id_token bozuk ve ayrıştırılamaz. Lütfen başka bir belirteç sağlayın ve yeniden deneyin. |
| `AADB2C90242` | SAML teknik profili{0}' ' nedenden dolayı yüklenemeyen PartnerEntity CDATA'yı belirtir '{1}' . |
| `AADB2C90243` | IDP'nin istemci anahtarı/sırrı düzgün şekilde yapılandırılmamıştır. |
| `AADB2C90244` | Şu anda çok fazla istek var. Lütfen biraz bekleyin ve tekrar deneyin. |
| `AADB2C90248` | Kaynak sahibi akışı yalnızca B2C yönetici portalı üzerinden oluşturulan uygulamalar tarafından kullanılabilir. |
| `AADB2C90250` | Genel giriş bitiş noktası desteklenmez. |
| `AADB2C90255` | Teknik profilde belirtilen talep{0}değişimi ' ' beklendiği gibi tamamlanmadı. Oturumunuza en baştan başlamayı deneyebilirsiniz. |
| `AADB2C90261` | Talep değişimi{0}' ' '{1}adımda belirtilir ' ' döndürülen HTTP hata yanıtı ayrıştıılamadı. |
| `AADB2C90272` | id_token_hint parametresi istekte belirtilmedi. Lütfen belirteç sağlayın ve yeniden deneyin. |
| `AADB2C90273` | Geçersiz bir yanıt alındı{0}: ' ' |
| `AADB2C90274` | Sağlayıcı meta verileri tek bir oturum açma hizmeti belirtmez veya bitiş noktası bağlama 'urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect' veya 'urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST' değildir. |
| `AADB2C90276` | İstek, ':{0}' teknik{1}profil{2}' ' ' için '{3}' '{4}' ' ' denetim ayarı ile tutarlı değildir . ' kiracı ' ' |
| `AADB2C90277` | ' kullanıcı yolculuğunun '{0}{1}' ' '{2}ilkesi ' ' ' ' ' ' nin düzenleme adımı bir içerik tanımı referansı içermez. |
| `AADB2C90279` | Sağlanan müşteri kimliği{0}' ' ' hibe veren müşteri kimliği ile eşleşmez. |
| `AADB2C90284` | Tanımlayıcı ' '{0}' ile yapılan başvuruya izin verilmedi ve yerel hesaplar için kullanılamaz. |
| `AADB2C90285` | Tanımlayıcı ile uygulama ' '{0}' bulunamadı. |
| `AADB2C90288` | UserJourney with{0}id ' ' '{1}' ' ' '{2}' ile kiracının{3}belirteci itfası için ' ' ' poliçesinde veya temel ilkelerinden herhangi birinde yok. |
| `AADB2C90289` | Kimlik sağlayıcısına bağlanan bir hatayla karşılaştık. Lütfen daha sonra tekrar deneyin. |
| `AADB2C90296` | Uygulama doğru yapılandırılmadı. Lütfen erişmeye çalıştığınız sitenin yöneticisiyle iletişime geçin. |
| `AADB2C99005` | İstek, geçersiz bir karakter '{0}'içeren geçersiz bir kapsam parametresi içerir. |
| `AADB2C99006` | Azure AD B2C, uygulama kimliği '{0}ile uzantı uygulamasını bulamıyor. Daha https://go.microsoft.com/fwlink/?linkid=851224 fazla bilgi için lütfen ziyaret edin. |
| `AADB2C99011` | Meta veri değeri{0}' ' ' '{1}' teknik{2}profil ' ' ' 'de belirtilmemiştir. |
| `AADB2C99013` | Verilen grant_type [{0}] ve{1}token_type [ ] kombinasyonu desteklenmez. |
| `AADB2C99015` | Profil{0}' '{1}in policy{2}' ' in tenant ' ' ' kaynak sahibi nin parola kimlik bilgisi akışı için gerekli tüm Giriş Talepleri eksiktir. |
