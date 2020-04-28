---
title: 'Azure AD Connect: bildirime dayalı sağlama Ifadeleri | Microsoft Docs'
description: Bildirim temelli sağlama ifadelerini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: e3ea53c8-3801-4acf-a297-0fb9bb1bf11d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: cdc7c9dba49bf37db1f039d43b0450c65884c74b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "60245497"
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect eşitleme: bildirim temelli sağlama Ifadelerini anlama
İlk olarak Forefront Identity Manager 2010 ' de tanıtılan bildirim temelli sağlama üzerinde Azure AD Connect eşitleme derlemeleri. Derlenmiş kod yazmak zorunda kalmadan, tüm kimlik Tümleştirme iş mantığınızı uygulamanıza olanak tanır.

Bildirim temelli sağlamanın önemli bir parçası, öznitelik akışlarında kullanılan ifade dilidir. Kullanılan dil, uygulamalar için Microsoft® Visual Basic® alt kümesidir (VBA). Bu dil Microsoft Office kullanılır ve VBScript deneyimi olan kullanıcılar da bunu tanır. Bildirim temelli sağlama Ifade dili yalnızca işlevleri kullanıyor ve yapılandırılmış bir dil değil. Yöntem veya deyim yok. İşlevler, Express program akışına iç içe geçmiş.

Daha ayrıntılı bilgi için bkz. [Office 2013 için Visual Basic for Applications Dil başvurusuna hoş geldiniz](https://msdn.microsoft.com/library/gg264383.aspx).

Öznitelikler kesin olarak türdedir. Bir işlev yalnızca doğru türdeki öznitelikleri kabul eder. Ayrıca büyük/küçük harfe duyarlıdır. Her iki işlev adı ve öznitelik adı uygun bir büyük harfe sahip olmalı veya bir hata oluşur.

## <a name="language-definitions-and-identifiers"></a>Dil tanımları ve tanımlayıcıları
* İşlevlerde bir ad, parantez içinde bağımsız değişkenler gelir: fonksiyonadı (bağımsız değişken 1, bağımsız değişken N).
* Öznitelikler köşeli ayraç ile tanımlanır: [Öznitelikno]
* Parametreler yüzde işaretleri ile tanımlanır:% ParameterName%
* Dize sabitleri tırnak içine alınır: Örneğin, "contoso" (Not: düz tırnak ("") ve akıllı tırnak ("") kullanılmalıdır)
* Sayısal değerler tırnak işareti olmadan ifade edilir ve Decimal olması beklenir. Onaltılık değerlere &H ön eki eklenir. Örneğin, 98052 &HFF
* Boole değerleri sabitler: true, false ile ifade edilir.
* Yerleşik sabitler ve sabit değerler yalnızca kendi adlarıyla ifade edilir: NULL, CRLF, ıgnorethisflow

### <a name="functions"></a>İşlevler
Bildirime dayalı sağlama, öznitelik değerlerini dönüştürme olasılığını sağlamak için birçok işlevi kullanır. Bu işlevler, bir işlevin sonucunun başka bir işleve geçirilmesi için iç içe geçebilir.

`Function1(Function2(Function3()))`

İşlevlerin tamamı listesi [işlev başvurusunda](reference-connect-sync-functions-reference.md)bulunabilir.

### <a name="parameters"></a>Parametreler
Bir parametre, bir bağlayıcı ya da PowerShell kullanan bir yönetici tarafından tanımlanır. Parametreler genellikle sistemden sisteme farklı değerler içerir, örneğin kullanıcının bulunduğu etki alanının adı. Bu parametreler, öznitelik akışlarında kullanılabilir.

Active Directory Bağlayıcısı gelen eşitleme kuralları için aşağıdaki parametreleri sağladı:

| Parametre Adı | Açıklama |
| --- | --- |
| Domain. NetBIOS |Şu anda içeri aktarılmakta olan etki alanının NetBIOS biçimi, örneğin FABRIKAMSALES |
| Domain. FQDN |Şu anda içeri aktarılmakta olan etki alanının FQDN biçimi, örneğin sales.fabrikam.com |
| Domain. LDAP |Şu anda içeri aktarılmakta olan etki alanının LDAP biçimi; örneğin, DC = Sales, DC = Fabrikam, DC = com |
| Forest. NetBIOS |Şu anda içeri aktarılmakta olan orman adının NetBIOS biçimi, örneğin FABRIKAMCORP |
| Forest. FQDN |Şu anda içeri aktarılmakta olan orman adının FQDN biçimi, örneğin fabrikam.com |
| Forest. LDAP |Şu anda içeri aktarılmakta olan orman adının LDAP biçimi; örneğin, DC = Fabrikam, DC = com |

Sistem şu anda çalışmakta olan bağlayıcının tanımlayıcısını almak için kullanılan aşağıdaki parametreyi sağlar:  
`Connector.ID`

Meta veri deposu özniteliği etki alanını, kullanıcının bulunduğu etki alanının NetBIOS adıyla dolduran bir örnek aşağıda verilmiştir:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>İşleçler
Aşağıdaki işleçler kullanılabilir:

* **Karşılaştırma**: <, <=,  <>, =, >, >=
* **Matematik**: +,-, \*,-
* **Dize**: & (Birleştir)
* **Mantıksal**:  &&  (ve), | | veya
* **Değerlendirme sırası**: ()

İşleçler soldan sağa değerlendirilir ve aynı değerlendirme önceliğine sahiptir. Diğer bir deyişle, \* (çarpan)-(çıkarma) öncesinde değerlendirilmez. 2\*(5 + 3), 2\*5 + 3 ile aynı değildir. Köşeli ayraçlar (), soldan sağa değerlendirme sırası uygun olmadığında değerlendirme sırasını değiştirmek için kullanılır.

## <a name="multi-valued-attributes"></a>Çoklu değerli öznitelikler
İşlevler hem tek değerli hem de birden çok değerli öznitelikler üzerinde çalışabilir. Çoklu değerli özniteliklerde, işlev her değer üzerinde çalışır ve her değere aynı işlevi uygular.

Örneğin:  
`Trim([proxyAddresses])`ProxyAddress özniteliğinde her bir değerin kırpılıp bir bölümünü yapın.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`İle @-signher değer için etki alanını ile @contoso.comdeğiştirin.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`SIP-Address ' i bulun ve değerlerden kaldırın.

## <a name="next-steps"></a>Sonraki adımlar
* [Bildirim temelli sağlamayı anlamak](concept-azure-ad-connect-sync-declarative-provisioning.md)için yapılandırma modeli hakkında daha fazla bilgi edinin.
* [Varsayılan yapılandırmayı anlamak için](concept-azure-ad-connect-sync-default-configuration.md)bildirim temelli sağlama 'nın nasıl kullanıldığını görün.
* [Varsayılan yapılandırmada değişiklik yapma konusunda](how-to-connect-sync-change-the-configuration.md)bildirim temelli sağlama kullanarak pratik bir değişiklik yapma konusuna bakın.

**Genel Bakış konuları**

* [Azure AD Connect eşitleme: eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

**Başvuru konuları**

* [Azure AD Connect Sync: Işlevler başvurusu](reference-connect-sync-functions-reference.md)

