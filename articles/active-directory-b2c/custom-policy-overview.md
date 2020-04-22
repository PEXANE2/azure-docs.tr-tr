---
title: Azure Active Directory B2C özel ilkeleri | Microsoft Dokümanlar
description: Azure Active Directory B2C özel ilkeleri hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: f18f44208b97ab5bc8d9cd9ff01d604c62deb963
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678155"
---
# <a name="custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeler

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Özel ilkeler, Azure Etkin Dizin B2C (Azure AD B2C) kiracınızın davranışını tanımlayan yapılandırma dosyalarıdır. Kullanıcı akışları, en yaygın kimlik görevleri için Azure AD B2C portalında önceden tanımlanır. Özel ilkeler, birçok farklı görevi tamamlamak için bir kimlik geliştiricisi tarafından tam olarak düzenlenebilir.

## <a name="comparing-user-flows-and-custom-policies"></a>Kullanıcı akışlarını ve özel ilkeleri karşılaştırma

| | Kullanıcı akışları | Özel ilkeler |
|-|-------------------|-----------------|
| Hedef kullanıcılar | Kimlik uzmanlığı olan veya olmayan tüm uygulama geliştiricileri. | Kimlik uzmanları, sistem entegratörleri, danışmanlar ve şirket içi kimlik ekipleri. OpenID Connect akışları ile rahatlar ve kimlik sağlayıcılarını ve talep tabanlı kimlik doğrulamayı anlarlar. |
| Yapılandırma yöntemi | Kullanıcı dostu kullanıcı arabirimine (UI) sahip Azure portalı. | Doğrudan XML dosyalarını düzenleyip Azure portalına yükleyin. |
| UI özelleştirme | HTML, CSS ve JavaScript dahil olmak üzere tam kullanıcı bir iki arama birimi özelleştirme.<br><br>Özel dizeleri ile çok dilli destek. | Aynı |
| Öznitelik özelleştirme | Standart ve özel öznitelikler. | Aynı |
| Belirteç ve oturum yönetimi | Özel belirteç ve birden çok oturum seçeneği. | Aynı |
| Kimlik Sağlayıcıları | Azure Active Directory kiracılarıyla federasyon gibi önceden tanımlanmış yerel veya sosyal sağlayıcı lar ve çoğu OIDC kimlik sağlayıcısı. | Standartlara dayalı OIDC, OAUTH ve SAML.  Kimlik doğrulama, REST API'leri ile tümleştirme kullanılarak da mümkündür. |
| Kimlik Görevleri | Yerel veya birçok sosyal hesapla kaydolma veya kaydolma.<br><br>Self servis parola sıfırlama.<br><br>Profil editi.<br><br>Çok Faktörlü Kimlik Doğrulama.<br><br>Belirteçleri ve oturumları özelleştirin.<br><br>Erişim belirteç akışları. | Özel kimlik sağlayıcılarını kullanarak kullanıcı akışlarıyla aynı görevleri tamamlayın veya özel kapsamlar kullanın.<br><br>Kayıt sırasında başka bir sistemde bir kullanıcı hesabı sağlama.<br><br>Kendi e-posta servis sağlayıcınızı kullanarak hoş geldiniz e-postası gönderin.<br><br>Azure AD B2C dışında bir kullanıcı mağazası kullanın.<br><br>Bir API kullanarak güvenilir bir sistemle kullanıcı sağlanan bilgileri doğrulayın. |

## <a name="policy-files"></a>İlke dosyaları

Bu üç tür ilke dosyası kullanılır:

- **Temel dosya** - tanımların çoğunu içerir. Sorun giderme ve ilkelerinizin uzun süreli bakımına yardımcı olmak için bu dosyada en az sayıda değişiklik yapmanız önerilir.
- **Uzantılar dosyası** - kiracınız için benzersiz yapılandırma değişikliklerini tutar.
- **Güvenerek Taraf (RP) dosyası** - Doğrudan uygulama veya hizmet tarafından çağrılan tek görev odaklı dosya (Ayrıca, Güvenen Taraf olarak da bilinir). Her benzersiz görev kendi RP gerektirir ve marka gereksinimlerine bağlı olarak, sayı "uygulama toplamı x toplam kullanım örnekleri sayısı" olabilir.

Azure AD B2C'deki kullanıcı akışları yukarıda gösterilen dosya desenini takip eder, ancak geliştirici yalnızca RP dosyasını görürken, Azure portalı uzantılar dosyasının arka planında değişiklikler yapar.

Üç tür ilke dosyası olmasına rağmen, yalnızca üç dosyayla sınırlı değilsiniz. Her dosya türünden birden çok dosyanız olabilir. Örneğin, Uzantılar dosyanızda değişiklik yapmak istemiyorsanız, Uzantılar dosyasını daha da genişletmek için bir Uzantı2 dosyası oluşturabilirsiniz.

## <a name="custom-policy-core-concepts"></a>Özel ilke temel kavramları

Azure'daki müşteri kimliği ve erişim yönetimi (CIAM) hizmeti şunları içerir:

- Microsoft Graph kullanılarak erişilebilen ve hem yerel hesaplar hem de federe hesaplar için kullanıcı verilerini tutan bir kullanıcı dizini.
- Kullanıcılar ve varlıklar arasında güveni düzenleyen ve bir kimlik veya erişim yönetimi görevini tamamlamak için aralarındaki talepleri aktaran **Kimlik Deneyimi Çerçevesine** erişim.
- Kimlik belirteçleri, yenileme belirteçleri ve erişim belirteçleri (ve eşdeğer SAML iddiaları) veren ve kaynakları korumak için bunları doğrulayan bir güvenlik belirteci hizmeti (STS).

Azure AD B2C, kimlik görevi elde etmek için kimlik sağlayıcıları, kullanıcılar, diğer sistemler ve sırayla yerel kullanıcı dizini ile etkileşim kurar. Örneğin, bir kullanıcıoturum açın, yeni bir kullanıcı kaydedin veya bir parolayı sıfırlayın. Kimlik Deneyimi Çerçevesi ve bir ilke (kullanıcı yolculuğu veya güven çerçevesi ilkesi olarak da adlandırılır) çok partili güven belirler ve aktörleri, eylemleri, protokolleri ve tamamlanması gereken adımların sırasını açıkça tanımlar.

Kimlik Deneyimi Çerçevesi, OpenID Connect, OAuth, SAML gibi standart protokol biçimlerindeki varlıklar arasında güveni düzenleyen ve örneğin REST API tabanlı sistemden sisteme talep alışverişi gibi standart olmayan birkaç platformda güven düzenleyen tamamen yapılandırılabilir, ilke tabanlı, bulut tabanlı bir Azure platformudur. Çerçeve, HTML ve CSS'yi destekleyen kullanıcı dostu, beyaz etiketli deneyimler oluşturur.

Özel ilke bir veya hiyerarşi zincirinde birbirine başvuran daha fazla XML biçimindeki dosyadan oluşabilir. XML öğeleri, diğer öğelerin yanı sıra, talep şema, talep dönüşümleri, içerik tanımları, talep sağlayıcılar, teknik profiller ve kullanıcı yolculuğu düzenleme adımlarını tanımlar. Özel bir ilke, güvenen bir taraf tarafından çağrıldığızaman Kimlik Deneyimi Çerçevesi tarafından yürütülen bir veya birkaç XML dosyası olarak erişilebilir. Özel ilkeleri yapılandıran geliştiriciler, güvenilir ilişkileri meta veri uç noktalarını, tam talep değişimi tanımlarını içerecek şekilde dikkatli bir şekilde tanımlamalı ve her kimlik sağlayıcısının gerektirdiği sırları, anahtarları ve sertifikaları yapılandırmalıdır.

### <a name="inheritance-model"></a>Kalıtım modeli

Bir uygulama RP ilke dosyasını aradığında, Azure AD B2C'deki Kimlik Deneyimi Çerçevesi, geçerli ilkeyi birleştirmek için temel dosyadaki tüm öğeleri, uzantılar dosyasından ve ardından RP ilkesi dosyasından ekler.  RP dosyasındaki aynı tür ve addaki öğeler uzantıdakiöğeleri geçersiz kılar ve uzantılar tabanı geçersiz kılar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel ilkeleri kullanmaya başlama](custom-policy-get-started.md)
