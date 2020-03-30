---
title: Kullanıcı geçiş yaklaşımları
titleSuffix: Azure AD B2C
description: Toplu alma veya sorunsuz geçiş yöntemlerini kullanarak kullanıcı hesaplarını başka bir kimlik sağlayıcısından Azure AD B2C'ye geçirin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: b3ee069985fd39288a562d3caafc50b12290c060
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80332341"
---
# <a name="migrate-users-to-azure-ad-b2c"></a>Kullanıcıları Azure AD B2C'ye geçirin

Başka bir kimlik sağlayıcısından Azure Etkin Dizin B2C'ye (Azure AD B2C) geçiş yapmak, varolan kullanıcı hesaplarını geçirmeyi de gerektirebilir. Burada iki geçiş yöntemi tartışılır, *toplu alma* ve *sorunsuz geçiş.* Her iki yaklaşımla da, Azure AD B2C'de kullanıcı hesapları oluşturmak için [Microsoft Graph API'sini](manage-user-accounts-graph-api.md) kullanan bir uygulama veya komut dosyası yazmanız gerekir.

## <a name="bulk-import"></a>Toplu alma

Toplu alma akışında, geçiş uygulamanız her kullanıcı hesabı için şu adımları gerçekleştirir:

1. Geçerli kimlik bilgileri (kullanıcı adı ve parola) da dahil olmak üzere eski kimlik sağlayıcısından kullanıcı hesabını okuyun.
1. Azure AD B2C dizininizde geçerli kimlik bilgileriyle karşılık gelen bir hesap oluşturun.

Bu iki durumdan herhangi birinde toplu alma akışını kullanın:

- Bir kullanıcının düz metin kimlik bilgilerine (kullanıcı adı ve parola) erişebilirsiniz.
- Kimlik bilgileri şifrelenir, ancak şifrelerini çözebilirsiniz.

Programlı olarak kullanıcı hesapları oluşturma hakkında bilgi için Bkz. [Microsoft Graph ile Azure AD B2C kullanıcı hesaplarını yönet.](manage-user-accounts-graph-api.md)

## <a name="seamless-migration"></a>Sorunsuz geçiş

Eski kimlik sağlayıcısındaki düz metin parolalarına erişilemiyorsa sorunsuz geçiş akışını kullanın. Örneğin, şu zaman:

- Parola, karma işlev gibi tek yönlü şifreli biçimde depolanır.
- Parola, eski kimlik sağlayıcısı tarafından erişimediğiniz şekilde depolanır. Örneğin, kimlik sağlayıcısı bir web hizmetini çağırarak kimlik bilgilerini doğruladığında.

Sorunsuz geçiş akışı hala kullanıcı hesaplarının toplu geçiş gerektirir, ancak daha sonra her kullanıcının parolasını ilk oturum açmada ayarlamak için bir [REST API](custom-policy-rest-api-intro.md) (oluşturduğunuz) sorgulamak için özel bir [ilke](custom-policy-get-started.md) kullanır.

Sorunsuz geçiş akışı böylece iki aşamadan vardır: *toplu alma* ve kimlik *bilgileri kümesi.*

### <a name="phase-1-bulk-import"></a>Aşama 1: Toplu alma

1. Geçiş uygulamanız, eski kimlik sağlayıcısının kullanıcı hesaplarını okur.
1. Geçiş uygulaması Azure AD B2C dizininde karşılık gelen kullanıcı hesapları oluşturur, ancak *parolalar ayarlamaz.*

### <a name="phase-2-set-credentials"></a>Aşama 2: Kimlik bilgilerini ayarlama

Hesapların toplu geçişi tamamlandıktan sonra, özel politikanız ve REST API'nız bir kullanıcı aşağıdakileri imzaladığında aşağıdakileri gerçekleştirir:

1. Girilen e-posta adresine karşılık gelen Azure AD B2C kullanıcı hesabını okuyun.
1. Boolean uzantı özniteliğini değerlendirerek hesabın geçiş için işaretlenip işaretlenmediğini denetleyin.
    - Uzantı özniteliği `True`geri dönerse, parolayı eski kimlik sağlayıcısına karşı doğrulamak için REST API'nizi arayın.
      - REST API parolanın yanlış olduğunu belirlerse, kullanıcıya dostça bir hata döndürün.
      - REST API parolanın doğru olduğunu belirlerse, parolayı Azure AD B2C hesabına yazın `False`ve boolean uzantı özniteliğini .
    - Boolean uzantısı özniteliği `False`dönerse, oturum açma işlemine normal olarak devam edin.

Örnek bir özel ilke ve REST API görmek için GitHub'daki [sorunsuz kullanıcı geçişi örneğine](https://aka.ms/b2c-account-seamless-migration) bakın.

![Kullanıcı geçişine sorunsuz geçiş yaklaşımının akış şeması diyagramı](./media/user-migration/diagram-01-seamless-migration.png)<br />*Diyagram: Kesintisiz geçiş akışı*

## <a name="best-practices"></a>En iyi uygulamalar

### <a name="security"></a>Güvenlik

Sorunsuz geçiş yaklaşımı, kullanıcının kimlik bilgilerini eski kimlik sağlayıcısına karşı doğrulamak için kendi özel REST API'nizi kullanır.

**REST API'nizi kaba kuvvet saldırılarına karşı korumalısınız.** Saldırgan, sonunda bir kullanıcının kimlik bilgilerini tahmin etme umuduyla birkaç parola gönderebilir. Bu tür saldırıları yenmek için, oturum açma denemesi sayısı belirli bir eşiği geçtiğinde REST API'nize istekler sunmayı bırakın. Ayrıca, Azure AD B2C ile REST API'niz arasındaki iletişimi de güvence altına alanın. Yeniden üretim için yeniden ifl eksalarınızı nasıl güvenli hale erdireceğimize yönelik bilgi için Secure [RESTful API'ye](secure-rest-api.md)bakın.

### <a name="user-attributes"></a>Kullanıcı öznitelikleri

Eski kimlik sağlayıcısındaki tüm bilgiler Azure AD B2C dizininize geçirilmemelidir. Geçiş yapmadan önce Azure AD B2C'de depolanması gereken uygun kullanıcı öznitelikleri kümesini belirleyin.

- Azure AD B2C'de **DEPOLAMA** YAP
  - Kullanıcı adı, şifre, e-posta adresleri, telefon numaraları, üyelik numaraları/tanımlayıcılar.
  - Gizlilik ilkesi ve son kullanıcı lisans sözleşmeleri için onay işaretleri.
- Azure AD B2C'de **saklamaYIN**
  - Kredi kartı numaraları, sosyal güvenlik numaraları (SSN), tıbbi kayıtlar veya devlet veya endüstri uyumluluk kuruluşları tarafından düzenlenen diğer veriler gibi hassas veriler.
  - Pazarlama veya iletişim tercihleri, kullanıcı davranışları ve öngörüler.

### <a name="directory-clean-up"></a>Dizin temizleme

Geçiş işlemine başlamadan önce, dizinitemizlemek için fırsat alın.

- Azure AD B2C'de depolanacak kullanıcı öznitelikleri kümesini belirleyin ve yalnızca ihtiyacınız olanı geçirin. Gerekirse, bir kullanıcı hakkında daha fazla veri depolamak için [özel öznitelikler](custom-policy-custom-attributes.md) oluşturabilirsiniz.
- Birden çok kimlik doğrulama kaynağı olan bir ortamdan geçiş yapıyorsunuz (örneğin, her uygulamanın kendi kullanıcı dizini vardır), Azure AD B2C'de birleşik bir hesaba geçiş yaptı.
- Birden çok uygulamanın farklı kullanıcı adları varsa, kimlik koleksiyonunu kullanarak bunların tümlerini Bir Azure AD B2C kullanıcı hesabında saklayabilirsiniz. Parolayla ilgili olarak, kullanıcının bir parola seçmesine ve dizine ayarlamasına izin verin. Örneğin, sorunsuz geçişle, yalnızca seçilen parola Azure AD B2C hesabında depolanmalıdır.
- Geçiş yapmadan önce kullanılmayan kullanıcı hesaplarını kaldırın veya eski hesapları geçirin.

### <a name="password-policy"></a>Parola ilkesi

Geçirdiğiniz hesapların parola gücü Azure AD B2C tarafından uygulanan [güçlü parola gücünden](../active-directory/authentication/concept-sspr-policy.md) daha zayıfsa, güçlü parola gereksinimini devre dışı kullanabilirsiniz. Daha fazla bilgi için [Parola ilkesi özelliğine](manage-user-accounts-graph-api.md#password-policy-property)bakın.

## <a name="next-steps"></a>Sonraki adımlar

GitHub'daki [azure-ad-b2c/user-migration](https://github.com/azure-ad-b2c/user-migration) deposu sorunsuz bir geçiş özel ilkesi örneği ve REST API kodu örneği içerir:

[Sorunsuz kullanıcı geçişi özel ilkesi & REST API kod örneği](https://aka.ms/b2c-account-seamless-migration)
