---
title: 'Azure AD Connect: Bildirimsel Sağlama İfadeleri | Microsoft Dokümanlar'
description: Bildirimsel sağlama ifadelerini açıklar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60245497"
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect eşitlemi: Bildirimsel Sağlama İfadelerini Anlama
Azure AD Connect eşitleme, ilk olarak Forefront Identity Manager 2010'da tanıtılan bildirimsel sağlama üzerine oluşturur. Derlenmiş kod yazmaya gerek kalmadan tam kimlik tümleştirme iş mantığınızı uygulamanızı sağlar.

Bildirimsel sağlamanın önemli bir parçası öznitelik akışlarında kullanılan ifade dilidir. Kullanılan dil, Microsoft® Visual Basic® uygulamalar için bir alt kümesidir(VBA). Bu dil Microsoft Office'te kullanılır ve VBScript deneyimine sahip kullanıcılar da bunu tanıyacaktır. Bildirimsel Hüküm Veren İfade Dili yalnızca işlevleri kullanır ve yapılandırılmış bir dil değildir. Herhangi bir yöntem veya ifade yok. İşlevler bunun yerine program akışını ifade etmek için iç içe dir.

Daha fazla bilgi için, [Office 2013 için Uygulamalar dil başvurusu için Visual Basic'e hoş geldiniz' e](https://msdn.microsoft.com/library/gg264383.aspx)bakın.

Öznitelikler güçlü bir şekilde yazılır. Bir işlev yalnızca doğru türün özniteliklerini kabul eder. Ayrıca büyük/küçük harf duyarlıdır. Hem işlev adları hem de öznitelik adları uygun kasaya sahip olmalıdır veya bir hata atılır.

## <a name="language-definitions-and-identifiers"></a>Dil tanımları ve tanımlayıcılar
* İşlevler parantez içinde bağımsız değişkenler tarafından izlenen bir ad var: FunctionName (bağımsız değişken 1, bağımsız değişken N).
* Öznitelikler kare ayraçlarla tanımlanır: [öznitelikName]
* Parametreler yüzde işaretleri ile tanımlanır: %ParameterName%
* Dize sabitleri tırnak işaretleriyle çevrilidir: Örneğin, "Contoso" (Not: düz tırnak "" değil, akıllı tırnaklar "" kullanmanız gerekir "")
* Sayısal değerler tırnak işaretleri olmadan ifade edilir ve ondalık olması beklenir. Hexadecimal değerler &H ile önceden belirlenmiştir. Örneğin, 98052, &HFF
* Boolean değerleri sabitlerle ifade edilir: Doğru, Yanlış.
* Yerleşik sabitler ve literals sadece kendi adlarıyla ifade edilir: NULL, CRLF, IgnoreThisFlow

### <a name="functions"></a>İşlevler
Bildirimsel sağlama, öznitelik değerlerini dönüştürme olanağını etkinleştirmek için birçok işlev kullanır. Bu işlevler iç içe geçirilebilir, böylece bir işlevin sonucu başka bir işleve aktarılır.

`Function1(Function2(Function3()))`

Fonksiyonların tam listesi [işlev referans](reference-connect-sync-functions-reference.md)bulunabilir.

### <a name="parameters"></a>Parametreler
Parametre, PowerShell kullanan bir Bağlayıcı veya yönetici tarafından tanımlanır. Parametreler genellikle sistemden sisteme farklı değerler içerir( örneğin kullanıcının bulunduğu etki alanının adı. Bu parametreler öznitelik akışlarında kullanılabilir.

Active Directory Bağlayıcısı gelen Eşitleme Kuralları için aşağıdaki parametreleri sağladı:

| Parametre Adı | Açıklama |
| --- | --- |
| Domain.Netbios |Şu anda ithal edilmekte olan alan adının Netbios formatı, örneğin FABRIKAMSALES |
| Domain.FQDN |Şu anda içe aktarılan etki alanının FQDN biçimi, örneğin sales.fabrikam.com |
| Domain.LDAP |Şu anda ithal edilmekte olan etki alanının LDAP biçimi, örneğin DC=sales,DC=fabrikam,DC=com |
| Forest.Netbios |Şu anda ithal edilmekte olan orman adının Netbios formatı, örneğin FABRIKAMCORP |
| Forest.FQDN |Şu anda içe aktarılmakta olan orman adının FQDN biçimi, örneğin fabrikam.com |
| Orman.LDAP |Şu anda ithal edilmekte olan orman adının LDAP biçimi, örneğin DC=fabrikam,DC=com |

Sistem, şu anda çalışan Bağlayıcı'nın tanımlayıcısını almak için kullanılan aşağıdaki parametreyi sağlar:  
`Connector.ID`

Metaverse öznitelik etki alanını, kullanıcının bulunduğu etki alanının netbios adı ile dolduran bir örnek aşağıda verilmiştir:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>İşleçler
Aşağıdaki işleçler kullanılabilir:

* **Karşılaştırma**: <, <=, <>, =, >, >=
* **Matematik**: +, \*-, , -
* **String**: & (concatenate)
* **Mantıksal**: && (ve), || (veya)
* **Değerlendirme sırası**: ( )

Operatörler soldan sağa değerlendirilir ve aynı değerlendirme önceliğine sahiptir. Yani, \* (çarpan) önce değerlendirilmemiştir - (çıkarma). 2\*(5+3) 2\*5+3 ile aynı değildir. Parantez ( ) soldan sağa değerlendirme sırası uygun olmadığında değerlendirme sırasını değiştirmek için kullanılır.

## <a name="multi-valued-attributes"></a>Çok değerli öznitelikler
İşlevler hem tek değerli hem de çok değerli özniteliklerüzerinde çalışabilir. Çok değerli öznitelikler için, işlev her değer üzerinde çalışır ve her değer için aynı işlevi uygular.

Örnek:  
`Trim([proxyAddresses])`proxyAddress özniteliğindeki her değerin bir Kırpma sını yapın.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Bir ile her @-signdeğer için, @contoso.cometki alanı değiştirin .  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`SIP adresini arayın ve değerlerden kaldırın.

## <a name="next-steps"></a>Sonraki adımlar
* [Bildirimsel Hükmü Anlama'da](concept-azure-ad-connect-sync-declarative-provisioning.md)yapılandırma modeli hakkında daha fazla bilgi edinin.
* [Varsayılan yapılandırmayı anlamada](concept-azure-ad-connect-sync-default-configuration.md)bildirimsel sağlamanın kullanıma hazır olarak nasıl kullanıldığını görün.
* [Varsayılan yapılandırmada değişiklik yapma](how-to-connect-sync-change-the-configuration.md)konusunda bildirimsel hükmü kullanarak pratik bir değişiklik yapma şekline bakın.

**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

**Başvuru konuları**

* [Azure AD Connect eşitlemi: İşlevler Başvurusu](reference-connect-sync-functions-reference.md)

