---
title: 'Azure AD Connect eşitleme: Varsayılan yapılandırmayı değiştirme | Microsoft Dokümanlar'
description: Azure AD Connect eşitlemenin varsayılan yapılandırmasını değiştirmek için en iyi uygulamaları sağlar.
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
ms.topic: conceptual
ms.date: 08/29/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 940a35d89996b1eb9600fe4214863d2b5304750e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60242152"
---
# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Connect eşitlemi: Varsayılan yapılandırmayı değiştirmek için en iyi uygulamalar
Bu konunun amacı, Desteklenen ve desteklenmeyen Azure AD Connect eşitleme değişikliklerini açıklamaktır.

Azure AD Connect tarafından oluşturulan yapılandırma, şirket içi Active Directory ile Azure AD senkronize eden çoğu ortam için "olduğu gibi" çalışır. Ancak, bazı durumlarda, belirli bir gereksinimi veya gereksinimi karşılamak için yapılandırmaya bazı değişiklikler uygulamak gerekir.

## <a name="changes-to-the-service-account"></a>Hizmet hesabında yapılan değişiklikler
Azure AD Connect eşitlemi, yükleme sihirbazı tarafından oluşturulan bir hizmet hesabı altında çalışıyor. Bu hizmet hesabı eşitleme tarafından kullanılan veritabanının şifreleme anahtarlarını tutar. 127 karakter uzunluğunda parolayla oluşturulur ve parolanın süresi dolmayacak şekilde ayarlanır.

* Hizmet hesabının parolasını değiştirmek veya sıfırlamak **desteklenmez.** Bunu yapmak şifreleme anahtarlarını yok eder ve hizmet veritabanına erişemez ve başlatılamaz.

## <a name="changes-to-the-scheduler"></a>Zamanlayıcıda yapılan değişiklikler
Build 1.1 (Şubat 2016) sürümlerinden başlayarak [zamanlayıcıyı](how-to-connect-sync-feature-scheduler.md) varsayılan 30 dakikadan farklı bir eşitleme döngüsüne sahip olacak şekilde yapılandırabilirsiniz.

## <a name="changes-to-synchronization-rules"></a>Eşitleme Kurallarında Yapılan Değişiklikler
Yükleme sihirbazı, en yaygın senaryolar için çalışması gereken bir yapılandırma sağlar. Yapılandırmada değişiklik yapmanız gerekiyorsa, desteklenen bir yapılandırmaya sahip olmak için bu kurallara uymanız gerekir.

> [!WARNING]
> Varsayılan eşitleme kurallarında değişiklik yaparsanız, bu değişiklikler Azure AD Connect bir sonraki güncelleştirilmesinde üzerine yazılır ve beklenmeyen ve büyük olasılıkla istenmeyen eşitleme sonuçlarına neden olur.

* Varsayılan doğrudan öznitelik akışları kuruluşunuz için uygun değilse [öznitelik akışlarını değiştirebilirsiniz.](how-to-connect-sync-change-the-configuration.md#other-common-attribute-flow-changes)
* [Bir öznitelik akışı ve](how-to-connect-sync-change-the-configuration.md#do-not-flow-an-attribute) Azure AD'de varolan öznitelik değerlerini kaldırmak istemiyorsanız, bu senaryo için bir kural oluşturmanız gerekir.
* [İstenmeyen Eşitleme Kuralını](#disable-an-unwanted-sync-rule) siler yerine devre dışı bırakın. Silinen bir kural yükseltme sırasında yeniden oluşturulur.
* [Kutudan çıkma kuralını değiştirmek](#change-an-out-of-box-rule)için, özgün kuralın bir kopyasını oluşturmalı ve kutu dansı kuralını devre dışı açmalısınız. Eşitleme Kuralı Düzenleyicisi ister ve size yardımcı olur.
* Eşitleme Kuralları Düzenleyicisi'ni kullanarak özel eşitleme kurallarınızı dışa aktarın. Editör, olağanüstü durum kurtarma senaryosunda bunları kolayca yeniden oluşturmak için kullanabileceğiniz bir PowerShell komut dosyası sağlar.

> [!WARNING]
> Kutunun dışında eşitleme kurallarının parmak izi vardır. Bu kurallarda bir değişiklik yaparsanız, parmak izi artık eşleşmiyor. Azure AD Connect'in yeni bir sürümü uygulamaya çalıştığınızda gelecekte sorunlar la karşılamayabilirsiniz. Yalnızca bu makalede açıklanan şekilde değişiklik yapın.

### <a name="disable-an-unwanted-sync-rule"></a>İstenmeyen Eşitleme Kuralını devre dışı
Kutu dansı eşitleme kuralını silmeyin. Bir sonraki yükseltme sırasında yeniden oluşturulur.

Bazı durumlarda, yükleme sihirbazı topolojiniz için çalışmayan bir yapılandırma üretti. Örneğin, bir hesap kaynağı orman topolojiniz varsa ancak Exchange şemasıyla hesap ormanındaki şema süresini genişlettiyseniz, hesap ormanı ve kaynak ormanı için Exchange kuralları oluşturulur. Bu durumda, Exchange için Eşitleme Kuralıdevre dışı kalmanız gerekir.

![Devre dışı eşitleme kuralı](./media/how-to-connect-sync-best-practices-changing-default-configuration/exchangedisabledrule.png)

Yukarıdaki resimde, yükleme sihirbazı hesap ormanında eski bir Exchange 2003 şeması buldu. Bu şema uzantısı, Fabrikam'ın ortamında kaynak ormanı kullanılmadan önce eklenmiştir. Eski Exchange uygulamasından hiçbir özniteliklerin eşitlenmediğinden emin olmak için eşitleme kuralı gösterildiği gibi devre dışı bırakılmalı.

### <a name="change-an-out-of-box-rule"></a>Kutudan çıkma kuralını değiştirme
Kutudan çıkma kuralını değiştirmeniz gereken tek zaman, birleştirme kuralını değiştirmeniz gerektiğidir. Bir öznitelik akışını değiştirmeniz gerekiyorsa, kutu dan sıcağa göre daha yüksek önceliğe sahip bir eşitleme kuralı oluşturmanız gerekir. Pratik olarak klonlamak için gereken tek kural AD - **User Join kuralıdır.** Daha yüksek bir öncelik kuralıyla diğer tüm kuralları geçersiz kılabilirsiniz.

Kutudan çıkarma kuralında değişiklik yapmanız gerekiyorsa, kutudan çıkma kuralının bir kopyasını oluşturmanız ve özgün kuralı devre dışı atmanız gerekir. Sonra klonlanmış kural değişiklikleri yapın. Eşitleme Kuralı Düzenleyicisi bu adımlarda size yardımcı oluyor. Kutudan çıkmış bir kuralı açtığınızda, bu iletişim kutusu yla sunulur:  
![Kutu kuralıdışında uyarı](./media/how-to-connect-sync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Kuralın bir kopyasını oluşturmak için **Evet'i** seçin. Klonlanmış kural daha sonra açılır.  
![Klonlanmış kural](./media/how-to-connect-sync-best-practices-changing-default-configuration/clonedrule.png)

Bu klonlanmış kuralda, kapsam, birleştirme ve dönüşümlerde gerekli değişiklikleri yapın.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
