---
title: 'Azure AD Connect eşitleme: varsayılan yapılandırmayı değiştirme | Microsoft Docs'
description: Azure AD Connect eşitleme 'nin varsayılan yapılandırmasını değiştirmek için en iyi yöntemleri sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 7638a031-1635-4942-94c3-fce8f09eed5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/29/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70e91ff8fa3666a2dfc5aaad07be7927852b08bd
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85357707"
---
# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Connect eşitleme: varsayılan yapılandırmayı değiştirmek için En Iyi uygulamalar
Bu konunun amacı, Azure AD Connect eşitlemede desteklenen ve desteklenmeyen değişiklikleri anlatmaktadır.

Azure AD Connect tarafından oluşturulan yapılandırma, şirket içi Active Directory Azure AD ile eşitleyerek birçok ortamda "olduğu gibi" çalışmaktadır. Ancak bazı durumlarda, belirli bir ihtiyacı veya gereksinimi karşılamak için bir yapılandırmaya yapılan bazı değişiklikler uygulamak gereklidir.

## <a name="changes-to-the-service-account"></a>Hizmet hesabındaki değişiklikler
Azure AD Connect eşitleme, Yükleme Sihirbazı tarafından oluşturulan bir hizmet hesabı altında çalışıyor. Bu hizmet hesabı, eşitleme tarafından kullanılan veritabanının şifreleme anahtarlarını barındırır. Bu, 127 karakter uzunluğunda bir parola ile oluşturulur ve parola kullanım dışı olarak ayarlanır.

* Hizmet hesabının parolasının değiştirilmesi veya sıfırlanması **desteklenmez** . Bunun yapılması şifreleme anahtarlarını yok eder ve hizmet veritabanına erişemez ve başlatılamıyor.

## <a name="changes-to-the-scheduler"></a>Zamanlayıcıda yapılan değişiklikler
Build 1,1 (2016 Şubat) sürümünden başlayarak, [Scheduler](how-to-connect-sync-feature-scheduler.md) 'ı varsayılan 30 dakikadan farklı bir eşitleme döngüsüne sahip olacak şekilde yapılandırabilirsiniz.

## <a name="changes-to-synchronization-rules"></a>Eşitleme kurallarında yapılan değişiklikler
Yükleme Sihirbazı, en yaygın senaryolar için çalışması gereken bir yapılandırma sağlar. Yapılandırmada değişiklikler yapmanız gerekirse, hala desteklenen bir yapılandırmaya sahip olması için bu kuralları izlemeniz gerekir.

> [!WARNING]
> Varsayılan eşitleme kurallarında değişiklik yaparsanız, bu değişikliklerin üzerine yazılır ve bu değişiklikler, Azure AD Connect bir sonraki güncelleştirilişinde, beklenmedik ve olası istenmeyen eşitleme sonuçlarına neden olur.

* Varsayılan doğrudan öznitelik akışları kuruluşunuz için uygun değilse, [öznitelik akışlarını değiştirebilirsiniz](how-to-connect-sync-change-the-configuration.md#other-common-attribute-flow-changes) .
* [Bir özniteliği Flow](how-to-connect-sync-change-the-configuration.md#do-not-flow-an-attribute) ve Azure AD 'de var olan öznitelik değerlerini kaldırmak istiyorsanız, bu senaryo için bir kural oluşturmanız gerekir.
* [Istenmeyen eşitleme kuralını silmek yerine devre dışı bırakın](#disable-an-unwanted-sync-rule) . Silinen bir kural, yükseltme sırasında yeniden oluşturulur.
* [Bir hazır kuralını değiştirmek](#change-an-out-of-box-rule)için özgün kuralın bir kopyasını oluşturmanız ve kutudan çıkarma kuralını devre dışı bırakmanız gerekir. Eşitleme kuralı Düzenleyicisi size sorar ve size yardımcı olur.
* Eşitleme kuralları düzenleyicisini kullanarak özel eşitleme kurallarınızı dışarı aktarın. Düzenleyici, bunları olağanüstü durum kurtarma senaryosunda kolayca yeniden oluşturmak için kullanabileceğiniz bir PowerShell betiği sağlar.

> [!WARNING]
> Kullanıma hazır eşitleme kurallarının bir parmak izi vardır. Bu kurallarda değişiklik yaparsanız, parmak izi artık eşleşmesiz olur. Azure AD Connect yeni bir sürümünü uygulamaya çalıştığınızda gelecekte sorunlarla karşılaşabilirsiniz. Yalnızca bu makalede açıklanan şekilde değişiklik yapın.

### <a name="disable-an-unwanted-sync-rule"></a>İstenmeyen eşitleme kuralını devre dışı bırakma
Kullanıma hazır bir eşitleme kuralını silmeyin. Sonraki yükseltme sırasında yeniden oluşturulur.

Bazı durumlarda, Yükleme Sihirbazı topolojiniz için çalışmayan bir yapılandırma üretti. Örneğin, bir hesap-kaynak orman topolojisi varsa ancak şemayı hesap ormanında Exchange şemasına genişlettiyseniz, hesap ormanı ve kaynak ormanı için Exchange kuralları oluşturulur. Bu durumda, Exchange için eşitleme kuralını devre dışı bırakmanız gerekir.

![Eşitleme kuralı devre dışı](./media/how-to-connect-sync-best-practices-changing-default-configuration/exchangedisabledrule.png)

Yukarıdaki resimde, Yükleme Sihirbazı hesap ormanında eski bir Exchange 2003 şeması buldu. Bu şema uzantısı, kaynak ormanı fabrikam ortamında sunulmadan önce eklendi. Eski Exchange uygulamasındaki hiçbir özniteliğin eşitlenmemesini sağlamak için, eşitleme kuralı gösterildiği gibi devre dışı bırakılmalıdır.

### <a name="change-an-out-of-box-rule"></a>Hazır olmayan kuralı değiştirme
Yalnızca bir hazır kuralını değiştirmeniz gereken tek zaman, JOIN kuralını değiştirmeniz gerektiğinde olur. Bir öznitelik akışını değiştirmeniz gerekiyorsa, hazır kurallarından daha yüksek önceliğe sahip bir eşitleme kuralı oluşturmanız gerekir. Kopyalamanız gereken tek kural, **ad-Kullanıcı birleşimi içindeki**kuralıdır. Daha yüksek öncelikli bir kuralla, diğer tüm kuralları geçersiz kılabilirsiniz.

Kullanıma hazır bir kuralda değişiklik yapmanız gerekiyorsa, kutudan çıkan kuralın bir kopyasını oluşturmanız ve özgün kuralı devre dışı bırakmanız gerekir. Ardından klonlanan kuralda değişiklikleri yapın. Eşitleme kuralı Düzenleyicisi size bu adımlarla yardımcı oluyor. Hazır bir kuralı açtığınızda bu iletişim kutusuyla karşılaşırsınız:  
![Uyarı kutudan çıkış kuralı](./media/how-to-connect-sync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Kuralın bir kopyasını oluşturmak için **Evet** ' i seçin. Kopyalanan kural daha sonra açılır.  
![Kopyalanmış kural](./media/how-to-connect-sync-best-practices-changing-default-configuration/clonedrule.png)

Bu kopyalanmış kuralda, kapsam, JOIN ve dönüşümlerdeki tüm gerekli değişiklikleri yapın.

## <a name="next-steps"></a>Sonraki adımlar
**Genel Bakış konuları**

* [Azure AD Connect eşitleme: eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
