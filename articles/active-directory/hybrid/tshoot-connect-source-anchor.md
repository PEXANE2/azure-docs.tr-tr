---
title: 'Azure AD Connect: Yükleme sırasında Kaynak Bağlantı Sorunlarını Giderme | Microsoft Dokümanlar'
description: Bu konu, yükleme sırasında kaynak bağlantı noktasıyla ilgili sorunları nasıl gidereceklerine yönelik adımlar sağlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "62114165"
---
# <a name="troubleshooting-source-anchor-issues-during-installation"></a>Yükleme sırasında Kaynak Bağlantı Sorunları Giderme
Bu makalede, yükleme sırasında oluşabilecek farklı kaynak bağlantı sorunları açıklanabilir ve bu sorunları çözmek için yollar sunar.

## <a name="invalid-source-anchor-in-azure-active-directory"></a>Azure Etkin Dizininde Geçersiz Kaynak Çapa

### <a name="custom-installation"></a>Özel Kurulum

Azure AD Connect, özel yükleme sırasında Azure Active Directory'deki kaynak bağlantı ilkesini okur. İlke Azure Etkin Dizini'nde varsa, Azure AD Connect müşteri tarafından geçersiz kılınmadığı sürece aynı ilkeyi uygular. Sihirbaz, hangi özniteliğin okunduğunu bildirir. Ayrıca, kaynak bağlantı ilkesini geçersiz kılmaya çalışırsanız sihirbaz uyarır.

Bu okuma işlemi sırasında, Azure Etkin Dizin'deki kaynak bağlantı ilkesinin beklenmeyen olması mümkündür. Bu durumda, Azure AD Connect hangi kaynak çapanın kullanılacağını bilmiyor ve el ile geçersiz kılmaya ihtiyacı var.</br>
![Beklen -medik](media/tshoot-connect-source-anchor/source1.png)

Bu sorunu gidermek için, belirli bir öznitelik seçerek kaynak çapa el ile geçersiz kılabilirsiniz. Seçmek için hangi öznitelik emin seniz ve sadece bu seçeneği ile devam edin. Emin değilseniz, rehberlik için [Microsoft desteğine](https://support.microsoft.com/contactus/) başvurun. Kaynak bağlantı ilkesini değiştirirseniz, şirket içi kullanıcılarınızla ilişkili Azure kaynakları arasındaki ilişkiyi bozabilir.</br>
![Beklen -medik](media/tshoot-connect-source-anchor/source2.png)

### <a name="express-installation"></a>Ekspres Kurulum
Azure AD Connect, ekspres yükleme sırasında Azure Active Directory'deki kaynak bağlantı ilkesini okur. İlke Azure Etkin Dizini'nde varsa, Azure AD Connect aynı ilkeyi uygular. El ile geçersiz kılma seçeneği yoktur.

Bu okuma işlemi sırasında, Azure Etkin Dizin'deki kaynak bağlantı ilkesinin beklenmeyen olması mümkündür. Bu durumda, Azure AD Connect kaynak çapanın ne olması gerektiğini bilmez.</br>
![Beklen -medik](media/tshoot-connect-source-anchor/source3.png)

Bu sorunu gidermek için, özel modu kullanarak yeniden yüklemeniz ve belirli bir öznitelik seçerek kaynak bağlantı sını el ile geçersiz kılmanız gerekir. Seçmek için hangi öznitelik emin seniz ve sadece bu seçeneği ile devam edin. Emin değilseniz, rehberlik için [Microsoft desteğine](https://support.microsoft.com/contactus/) başvurun. Kaynak bağlantı ilkesini değiştirirseniz, şirket içi kullanıcılarınızla ilişkili Azure kaynakları arasındaki ilişkiyi bozabilir.

### <a name="invalid-source-anchor-in-sync-engine"></a>Eşitleme Motorunda Geçersiz Kaynak Çapa
Yükleme sırasında, Azure AD Connect'in eşitleme altyapısını geçersiz bir kaynak çapa kullanarak yapılandırmaya çalıştığı mümkündür. Bu işlem büyük olasılıkla bir ürün sorunudur ve Azure AD Connect'in yüklenmesi başarısız olur. Bu sorunla karşılaştıysanız [Microsoft desteğine](https://support.microsoft.com/contactus/) başvurun.</br>
![Beklen -medik](media/tshoot-connect-source-anchor/source4.png)


## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.