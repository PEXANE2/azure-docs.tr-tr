---
title: 'Azure AD Connect: yükleme sırasında kaynak bağlantısı sorunlarını giderme | Microsoft Docs'
description: Bu konuda, yükleme sırasında kaynak bağlantısı ile ilgili sorunları gidermeye yönelik adımlar sağlanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/19/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: fac33a01afc2efc1ab06c4783c11f7a089bb6208
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "62114165"
---
# <a name="troubleshooting-source-anchor-issues-during-installation"></a>Yükleme sırasında kaynak bağlantısı sorunlarını giderme
Bu makalede, yükleme sırasında oluşabilecek farklı kaynak bağlantısı ile ilgili sorunlar açıklanmakta ve bu sorunları çözmeye yönelik yollar sunulmaktadır.

## <a name="invalid-source-anchor-in-azure-active-directory"></a>Azure Active Directory geçersiz kaynak Bağlayıcısı

### <a name="custom-installation"></a>Özel yükleme

Özel yükleme sırasında, Azure AD Connect kaynak bağlantısı ilkesini Azure Active Directory okur. İlke Azure Active Directory varsa, müşteri tarafından geçersiz kılınmadıkça Azure AD Connect aynı ilkeyi uygular. Sihirbaz size hangi özniteliğin okunduğunu bildirir. Ayrıca, kaynak bağlantısı ilkesini geçersiz kılmayı denerseniz sihirbaz sizi uyarır.

Bu okuma işlemi sırasında, Azure Active Directory ' deki kaynak bağlayıcı ilkesi beklenmeyen bir durumda olabilir. Bu durumda, Azure AD Connect kaynak bağlayıcının ne olduğunu bilmez ve el ile geçersiz kılma ihtiyacı vardır.</br>
![bek](media/tshoot-connect-source-anchor/source1.png)

Bu sorunu çözmek için, belirli bir özniteliği seçerek kaynak bağlantısını el ile geçersiz kılabilirsiniz. Yalnızca hangi özniteliğin seçgeldiğinden eminseniz, bu seçenekle devam edin. Emin değilseniz, rehberlik için [Microsoft desteğine](https://support.microsoft.com/contactus/) başvurun. Kaynak Bağlayıcısı ilkesini değiştirirseniz, şirket içi kullanıcılarınız ve bunlarla ilişkili Azure kaynakları arasındaki ilişkilendirmeyi bozabilir.</br>
![bek](media/tshoot-connect-source-anchor/source2.png)

### <a name="express-installation"></a>Hızlı yükleme
Hızlı yükleme sırasında Azure AD Connect kaynak bağlantısı ilkesini Azure Active Directory okur. İlke Azure Active Directory varsa, Azure AD Connect aynı ilkeyi uygular. El ile geçersiz kılma yapmak için bir seçenek yoktur.

Bu okuma işlemi sırasında, Azure Active Directory ' deki kaynak bağlayıcı ilkesi beklenmeyen bir durumda olabilir. Bu durumda, Azure AD Connect kaynak bağlayıcının ne olması gerektiğini bilmez.</br>
![bek](media/tshoot-connect-source-anchor/source3.png)

Bu sorunu çözmek için, özel modu kullanarak yeniden yüklemeniz ve belirli bir özniteliği seçerek kaynak bağlantısını el ile geçersiz kılmanız gerekir. Yalnızca hangi özniteliğin seçgeldiğinden eminseniz, bu seçenekle devam edin. Emin değilseniz, rehberlik için [Microsoft desteğine](https://support.microsoft.com/contactus/) başvurun. Kaynak Bağlayıcısı ilkesini değiştirirseniz, şirket içi kullanıcılarınız ve bunlarla ilişkili Azure kaynakları arasındaki ilişkilendirmeyi bozabilir.

### <a name="invalid-source-anchor-in-sync-engine"></a>Eşitleme altyapısında geçersiz kaynak Bağlayıcısı
Yükleme sırasında, geçersiz bir kaynak Bağlayıcısı kullanarak eşitleme altyapısını yapılandırmaya çalışır Azure AD Connect mümkündür. Bu işlem büyük olasılıkla bir ürün sorunudur ve Azure AD Connect yüklemesi başarısız olur. Bu sorun için ' de çalıştırırsanız [Microsoft desteğine](https://support.microsoft.com/contactus/) başvurun.</br>
![bek](media/tshoot-connect-source-anchor/source4.png)


## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.