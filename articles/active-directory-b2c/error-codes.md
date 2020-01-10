---
title: Hata kodu başvurusu
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C hizmeti tarafından döndürülebilecek hata kodlarının bir listesi.
services: B2C
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: troubleshooting
ms.date: 01/10/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: c48f7bdd51c9b8c173ee0ebed72ed8c7f4fb51fc
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75735085"
---
# <a name="error-codes-azure-active-directory-b2c"></a>Hata kodları: Azure Active Directory B2C

Aşağıdaki hatalar Azure Active Directory B2C hizmeti tarafından döndürülebilir.

| Hata kodu | İleti |
| ---------- | ------- |
| `AADB2C90002` | CORS kaynağı '{0}', bir 404 bulamadı. |
| `AADB2C90006` | İstekte belirtilen '{0}' yeniden yönlendirme URI 'SI '{1}' istemci kimliği için kayıtlı değil. |
| `AADB2C90007` | '{0}' istemci kimliğiyle ilişkili uygulamanın kayıtlı yeniden yönlendirme URI 'Leri yok. |
| `AADB2C90008` | İstek bir istemci kimliği parametresi içermiyor. |
| `AADB2C90010` | İstek bir kapsam parametresi içermiyor. |
| `AADB2C90011` | İstekte belirtilen '{0}' istemci kimliği, ilkede kayıtlı istemci kimliği '{1}' ile eşleşmiyor. |
| `AADB2C90012` | İstekte belirtilen '{0}' kapsamı desteklenmiyor. |
| `AADB2C90013` | İstekte belirtilen '{0}' yanıt türü desteklenmiyor. |
| `AADB2C90014` | İstekte sunulan istenen yanıt modu '{0}' desteklenmiyor. |
| `AADB2C90016` | İstenen istemci onaylama türü '{0}', beklenen '{1}' türü ile eşleşmiyor. |
| `AADB2C90017` | İstekte belirtilen istemci onayı geçersiz: {0} |
| `AADB2C90018` | İstekte belirtilen '{0}' istemci kimliği '{1}' kiracısında kayıtlı değil. |
| `AADB2C90019` | '{1}' kiracısındaki '{0}' kimliğine sahip anahtar kapsayıcısı geçerli bir anahtara sahip değil. Neden: {2}. |
| `AADB2C90021` | '{0}' Teknik profili, '{2}' kiracının '{1}' ilkesinde yok. |
| `AADB2C90022` | '{1}' kiracısındaki '{0}' ilkesinin meta verileri döndürülemiyor. |
| `AADB2C90023` | '{0}' profili, gerekli '{1}' meta veri anahtarını içermiyor. |
| `AADB2C90025` | '{2}' kiracısındaki '{1}' ilkesindeki '{0}' profili, gerekli '{3}' şifreleme anahtarını içermiyor. |
| `AADB2C90027` | '{0}' için belirtilen temel kimlik bilgileri geçersiz. Kimlik bilgilerinin doğru olduğundan ve kaynak tarafından erişimin verildiğinden emin olun. |
| `AADB2C90028` | '{0}' için belirtilen istemci sertifikası geçersiz. Sertifikanın doğru olup olmadığını kontrol edin, özel bir anahtar içerir ve kaynak tarafından erişim izni verildiğini doğrulayın. |
| `AADB2C90031` | '{0}' ilkesi varsayılan bir Kullanıcı yolculuğu belirtmiyor. İlkenin veya üst öğelerinin, bağlı olan taraf bölümünün parçası olarak varsayılan bir Kullanıcı yolculuğu belirttiğinden emin olun. |
| `AADB2C90035` | Hizmet geçici olarak kullanılamıyor. Lütfen birkaç dakika sonra yeniden deneyin. |
| `AADB2C90036` | İstek, kullanıcıyı oturumu kapatma sonrası olarak yönlendirmek için bir URI içermiyor. Post_logout_redirect_uri Parameter alanında bir URI belirtin. |
| `AADB2C90037` | İstek işlenirken bir hata oluştu. Lütfen erişmeye çalıştığınız sitenin yöneticisine başvurun. |
| `AADB2C90039` | İstek bir istemci onaylama işlemi içeriyor, ancak '{1}' kiracısında belirtilen '{0}' ilkesinde, Ilkesinde relyingpartypolicy içinde client_secret eksik. |
| `AADB2C90040` | Kullanıcı yolculuğu '{0}' bir talep Gönder adımı içermiyor. |
| `AADB2C90043` | İstekte yer alan istem geçersiz değerler içeriyor. ' None ', ' login', ' onay ' veya ' select_account ' bekleniyor. |
| `AADB2C90044` | '{0}' talebi, '{1}' talep çözümleyici tarafından desteklenmiyor. |
| `AADB2C90046` | Geçerli durumlarınızı yüklerken sorun yaşıyoruz. Oturumunuzu baştan başlatmayı denemek isteyebilirsiniz. |
| `AADB2C90047` | '{0}' kaynağı, yüklenmesini engellemek için betik hataları içeriyor. |
| `AADB2C90048` | Sunucuda işlenmeyen bir özel durum oluştu. |
| `AADB2C90051` | Uygun talep sağlayıcısı bulunamadı. |
| `AADB2C90052` | Geçersiz Kullanıcı adı veya parola. |
| `AADB2C90053` | Belirtilen kimlik bilgilerine sahip bir Kullanıcı bulunamadı. |
| `AADB2C90054` | Geçersiz Kullanıcı adı veya parola. |
| `AADB2C90055` | İstekte belirtilen '{0}' kapsamının 'https://example.com/calendar.read ' gibi bir kaynak belirtmesi gerekir. |
| `AADB2C90057` | Belirtilen uygulama OAuth örtük akışına izin verecek şekilde yapılandırılmadı. |
| `AADB2C90058` | Belirtilen uygulama, ortak istemcilere izin verecek şekilde yapılandırılmadı. |
| `AADB2C90067` | '{0}' Post kapatma yeniden yönlendirme URI 'sinin biçimi geçersiz. 'https://example.com/return ' veya yerel istemciler için bir https tabanlı URL belirtin ' urn: ietf: WG: OAuth: 2.0: OOB ' IETF yerel istemci URI 'sini kullanın. |
| `AADB2C90068` | '{0}' KIMLIĞINE sahip belirtilen uygulama bu hizmette geçerli değildir. Lütfen B2C portalı aracılığıyla oluşturulan bir uygulamayı kullanın ve yeniden deneyin. |
| `AADB2C90075` | '{1}' adımında belirtilen '{0}' talep değişimi, '{2}' kodu ve '{3}' nedeniyle HTTP hata yanıtı döndürdü. |
| `AADB2C90077` | Kullanıcının mevcut bir oturumu yok ve istek istem parametresi '{0}' değerine sahip. |
| `AADB2C90079` | İstemciler, bir gizli izni benimseme sırasında client_secret göndermelidir. |
| `AADB2C90080` | Belirtilen izin süresi doldu. Lütfen tekrar kimlik doğrulaması yapın ve yeniden deneyin. Geçerli zaman: {0}, verilme Zamanı: {1}, kayan pencerenin sona erme zamanı verme süresi: {2}. |
| `AADB2C90081` | Belirtilen client_secret, bu istemci için beklenen değerle eşleşmiyor. Lütfen client_secret düzeltip yeniden deneyin. |
| `AADB2C90083` | İstekte gerekli parametre eksik: {0}. |
| `AADB2C90084` | Ortak istemciler, genel olarak alınan bir izni benimseyede bir client_secret göndermemelidir. |
| `AADB2C90085` | Hizmet bir iç hatayla karşılaştı. Lütfen yeniden kimlik doğrulaması yapın ve tekrar deneyin. |
| `AADB2C90086` | Sağlanan grant_type [{0}] desteklenmiyor. |
| `AADB2C90087` | Bu protokol uç noktasının bu sürümü için belirtilen izin verilmedi. |
| `AADB2C90088` | Bu uç nokta için belirtilen izin verilmedi. Gerçek değer: {0} ve beklenen değer: {1} |
| `AADB2C90092` | '{0}' KIMLIĞINE sahip belirtilen uygulama '{1}' kiracısı için devre dışı bırakıldı. Lütfen uygulamayı etkinleştirin ve yeniden deneyin. |
| `AADB2C90107` | '{0}' KIMLIĞINE sahip uygulama, istekte OpenID kapsamı sağlanmadığından ya da uygulama için yetkilendirilmemiş olmadığından KIMLIK belirteci alamıyor. |
| `AADB2C90108` | '{0}' düzenleme adımı beklenirken bir CpimIssuerTechnicalProfileReferenceId belirtmiyor. |
| `AADB2C90110` | ' İd_token ' içeren bir response_type istenirken kapsam parametresi ' OpenID ' içermelidir. |
| `AADB2C90111` | Hesabınız kilitlendi. Dosyanın kilidini açmak için destek sorumlunuza başvurun, sonra yeniden deneyin. |
| `AADB2C90114` | Hesabınız yetkisiz kullanımı engellemek için geçici olarak kilitlidir. Daha sonra tekrar deneyin. |
| `AADB2C90115` | ' Kod ' response_type istenirken, kapsam parametresi erişim belirteçleri için bir kaynak veya istemci KIMLIĞI ve KIMLIK belirteçleri için ' OpenID ' içermelidir. Ayrıca, yenileme belirteçleri için ' offline_access ' ekleyin. |
| `AADB2C90117` | İstekte belirtilen '{0}' kapsamı desteklenmiyor. |
| `AADB2C90118` | Kullanıcı parolasını unutmuş. |
| `AADB2C90120` | İstekte belirtilen en fazla yaş parametresi '{0}' geçersiz. Maksimum yaş, '{1}' ve '{2}' dahil olmak üzere bir tamsayı olmalıdır. |
| `AADB2C90122` | İstekte alınan '{0}' girişi için HTTP istek doğrulaması başarısız oldu. Girişin < veya & gibi karakterler içermediğinden emin olun. |
| `AADB2C90128` | Bu izin ile ilişkili hesap artık yok. Lütfen yeniden kimlik doğrulaması yapın ve tekrar deneyin. |
| `AADB2C90129` | Belirtilen izin iptal edildi. Lütfen yeniden kimlik doğrulaması yapın ve tekrar deneyin. |
| `AADB2C90145` | Doğrulanmamış telefon numarası bulunamadı ve ilke, Kullanıcı tarafından girilen bir numaraya izin vermiyor. |
| `AADB2C90146` | İstekte belirtilen '{0}' kapsamı, bir erişim belirteci için birden fazla kaynak belirtiyor ve bu desteklenmiyor. |
| `AADB2C90149` | '{0}' betiği yüklenemedi. |
| `AADB2C90151` | Kullanıcı Multi-Factor Authentication için yeniden deneme sayısı üst sınırını aştı. |
| `AADB2C90152` | Multi-Factor yoklama isteği hizmetten yanıt alamadı. |
| `AADB2C90154` | Multi-Factor Authentication isteği hizmetten bir oturum kimliği alamadı. |
| `AADB2C90155` | Multi-Factor Authentication isteği, '{0}' nedeniyle başarısız oldu. |
| `AADB2C90156` | Multi-Factor doğrulama isteği, '{0}' nedeniyle başarısız oldu. |
| `AADB2C90157` | Kullanıcı, otomatik olarak onaylanan bir adım için yeniden deneme sayısı üst sınırını aştı. |
| `AADB2C90158` | Otomatik olarak onaylanan bir doğrulama isteği, '{0}' nedeniyle başarısız oldu. |
| `AADB2C90159` | Otomatik olarak onaylanan bir doğrulama isteği, '{0}' nedeniyle başarısız oldu. |
| `AADB2C90161` | Kendini onaylanan bir gönderme yanıtı, '{0}' nedeniyle başarısız oldu. |
| `AADB2C90165` | '{0}' kimlikli SAML başlatma iletisi, durumunda bulunamıyor. |
| `AADB2C90168` | HTTP-Redirect isteği imzalı bir istek için gerekli '{0}' parametresini içermiyor. |
| `AADB2C90178` | '{0}' imzalama sertifikasının özel anahtarı yok. |
| `AADB2C90182` | Sağlanan code_verifier ilişkili code_challenge eşleşmiyor |
| `AADB2C90183` | Sağlanan code_verifier geçersiz |
| `AADB2C90184` | Sağlanan code_challenge_method desteklenmiyor. Desteklenen değerler düz veya S256 |
| `AADB2C90188` | SAML Technical profile '{0}', '{1}' öğesinin bir PartnerEntity URL 'sini belirtiyor, ancak meta verilerin getirilmesi '{2}' nedeniyle başarısız oluyor. |
| `AADB2C90194` | Taşıyıcı belirteci için belirtilen '{0}' talebi, kullanılabilir taleplerde mevcut değil. Kullanılabilir talepler '{1}'. |
| `AADB2C90205` | Bu uygulama, işlemi gerçekleştirmek için bu Web kaynağında yeterli izinlere sahip değil. |
| `AADB2C90206` | İstemciyi başlatırken bir zaman aşımı oluştu. |
| `AADB2C90208` | Belirtilen id_token_hint parametresi zaman aşımına uğradı. Lütfen başka bir belirteç girip yeniden deneyin. |
| `AADB2C90209` | Belirtilen id_token_hint parametresi kabul edilen bir hedef kitle içermiyor. Geçerli hedef kitle değerleri: '{0}'. Lütfen başka bir belirteç girip yeniden deneyin. |
| `AADB2C90210` | Belirtilen id_token_hint parametresi doğrulanamadı. Lütfen başka bir belirteç girip yeniden deneyin. |
| `AADB2C90211` | İstek tamamlanmamış bir durum tanımlama bilgisi içeriyor. |
| `AADB2C90212` | İstek geçersiz bir durum tanımlama bilgisi içeriyordu. |
| `AADB2C90220` | '{0}' kiracısındaki '{1}' depolama tanımlayıcısına sahip anahtar kapsayıcısı var, ancak geçerli bir sertifika içermiyor. Sertifikanın geçerliliği bozulmuş olabilir veya sertifikanız gelecekte (NBF) etkin hale gelebilir. |
| `AADB2C90223` | CORS kaynağını temizleme hatası oluştu. |
| `AADB2C90224` | Kaynak sahibi akışı uygulama için etkinleştirilmemiş. |
| `AADB2C90225` | İstekte belirtilen Kullanıcı adı veya parola geçersiz. |
| `AADB2C90226` | Belirtilen belirteç değişimi yalnızca HTTP POST üzerinden desteklenir. |
| `AADB2C90232` | Belirtilen id_token_hint parametresi kabul edilen bir veren içermiyor. Geçerli verenler: '{0}'. Lütfen başka bir belirteç girip yeniden deneyin. |
| `AADB2C90233` | Belirtilen id_token_hint parametresi imza doğrulamasında başarısız oldu. Lütfen başka bir belirteç girip yeniden deneyin. |
| `AADB2C90235` | Belirtilen id_token zaman aşımına uğradı. Lütfen başka bir belirteç girip yeniden deneyin. |
| `AADB2C90237` | Belirtilen id_token geçerli bir hedef kitle içermiyor. Geçerli hedef kitle değerleri: '{0}'. Lütfen başka bir belirteç girip yeniden deneyin. |
| `AADB2C90238` | Belirtilen id_token, geçerli bir veren içermiyor. Geçerli veren değerleri: '{0}'. Lütfen başka bir belirteç girip yeniden deneyin. |
| `AADB2C90239` | Belirtilen id_token imza doğrulaması başarısız oldu. Lütfen başka bir belirteç girip yeniden deneyin. |
| `AADB2C90240` | Belirtilen id_token hatalı biçimlendirilmiş ve ayrıştırılamadı. Lütfen başka bir belirteç girip yeniden deneyin. |
| `AADB2C90242` | '{0}' SAML teknik profili, '{1}' nedeni için yüklenemeyen PartnerEntity CDATA belirtiyor. |
| `AADB2C90243` | IDP 'nin istemci anahtarı/parolası düzgün yapılandırılmamış. |
| `AADB2C90244` | Şu anda çok fazla istek var. Lütfen bir süre bekleyip yeniden deneyin. |
| `AADB2C90248` | Kaynak sahibi akışı yalnızca B2C yönetim portalı aracılığıyla oluşturulan uygulamalar tarafından kullanılabilir. |
| `AADB2C90250` | Genel oturum açma uç noktası desteklenmiyor. |
| `AADB2C90255` | '{0}' Teknik profilinde belirtilen talep değişimi beklendiği gibi tamamlanmadı. Oturumunuzu baştan başlatmayı denemek isteyebilirsiniz. |
| `AADB2C90261` | '{1}' adımında belirtilen '{0}' talep değişimi, ayrıştırılmamış HTTP hata yanıtı döndürdü. |
| `AADB2C90272` | İstekte id_token_hint parametresi belirtilmedi. Lütfen belirteç sağlayın ve yeniden deneyin. |
| `AADB2C90273` | Geçersiz bir yanıt alındı: '{0}' |
| `AADB2C90274` | Sağlayıcı meta verileri tek bir oturum kapatma hizmeti belirtmiyor veya uç nokta bağlaması ' urn: oassıs: names: TC: SAML: 2.0: Bindings: HTTP-redirect ' veya ' urn: oassıs: names: TC: SAML: 2.0: Bindings: HTTP-POST ' değil. |
| `AADB2C90276` | İstek, ilke '{3}' kiracısı '{4}' için '{2}' teknisyen içindeki '{0}{1}' denetim ayarıyla tutarlı değil. |
| `AADB2C90277` | '{2}' ilkesinin '{1}' Kullanıcı yolculuğunun '{0}' düzenleme adımı bir içerik tanımı başvurusu içermiyor. |
| `AADB2C90279` | Belirtilen '{0}' istemci kimliği, izni veren istemci kimliği ile eşleşmiyor. |
| `AADB2C90284` | '{0}' tanımlayıcısına sahip uygulamaya izin verilmedi ve yerel hesaplar için kullanılamaz. |
| `AADB2C90285` | '{0}' tanımlayıcısına sahip uygulama bulunamadı. |
| `AADB2C90288` | '{2}' kiracısı için '{1}' adlı '{0}' kimliğine sahip Useryolculuğu, '{3}' ilkesinde veya temel ilkelerinin hiçbirinde yok. |
| `AADB2C90289` | Kimlik sağlayıcısına bağlanılırken bir hatayla karşılaştık. Lütfen daha sonra tekrar deneyin. |
| `AADB2C90296` | Uygulama doğru yapılandırılmamış. Lütfen erişmeye çalıştığınız sitenin yöneticisine başvurun. |
| `AADB2C99005` | İstek geçersiz '{0}' karakterini içeren geçersiz bir kapsam parametresi içeriyor. |
| `AADB2C99006` | Azure AD B2C, uygulama kimliği '{0}' olan uzantılar uygulamasını bulamıyor. Daha fazla bilgi için lütfen https://go.microsoft.com/fwlink/?linkid=851224 ziyaret edin. |
| `AADB2C99011` | '{2}' ilkesindeki '{1}' teknisyen bölümünde '{0}' meta veri değeri belirtilmedi. |
| `AADB2C99013` | Sağlanan grant_type [{0}] ve token_type [{1}] birleşimi desteklenmez. |
| `AADB2C99015` | '{2}' kiracısındaki '{1}' ilkesindeki '{0}' profilinde, kaynak sahibi parola kimlik bilgisi akışı için gereken tüm ınputclaim eksik. |
