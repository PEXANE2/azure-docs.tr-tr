---
title: Azure AD Connect yükleme sorunları | Microsoft Dokümanlar'
description: Bu konu, Azure AD Connect'i yüklerken sorunları nasıl gidereceklerine yönelik adımlar sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e75ad29c5b4a76de5317991995f132c6cb53bbe3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70211849"
---
# <a name="troubleshoot-azure-ad-connect-install-issues"></a>Sorun Giderme: Azure AD Connect yükleme sorunları

## <a name="recommended-steps"></a>**Önerilen Adımlar**
Lütfen hangi [Azure AD Connect yükleme türünün](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-select-installation) sizin için uygun olduğunu kontrol edin. Ekspres kurulum kriterlerini karşılıyorsanız, ekspres kurulumile devam etmenizi şiddetle tavsiye ediyoruz. Ekspres yükleme yüklemeyi bitirmek için gereken en az seçenek sağlar, bu nedenle herhangi bir sorun olasılığı daha azdır. 

Ancak, ekspres yükleme ölçütlerini karşılamıyorsanız ve özel yükleme yapmanız gerekiyorsa, sık karşılaşılan sorunları önlemek için izleyebileceğiniz en iyi uygulamalar şunlardır. Basitlik uğruna sadece seçici seçenekler burada belirtilmiştir:

* AAD Connect'i yüklediğiniz makinede yönetici olduğunuzdan emin olun. Aynı yönetici kimlik bilgilerine sahip makinede oturum açın.

* Varolan SQL Server'ı kullanmak istiyorsanız, "Varolan bir SQL Server'ı kullanın" dışında, aşağıdaki sayfada varsayılan tüm seçeneklerin varsayılan olmasını sağlar. Özel yükleme seçeneklerinin nasıl kullanılacağı hakkında [daha fazla ayrıntı](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-custom) aşağıda verilmiştir. 

    ![Varolan SQL Server'ı Kullanma](media/tshoot-connect-install-issues/tshoot-connect-install-issues/useexistingsqlserver.png)

* Aşağıdaki sayfada, varolan hesapla ilgili izin sorunlarını önlemek için "Yeni AD hesabı oluşturma" seçeneğini belirleyin.

    ![AD Orman Hesabı](media/tshoot-connect-install-issues/tshoot-connect-install-issues/createnewaccount.png)

### <a name="common-issues"></a>**Sık Karşılaşılan Sorunlar**

* [Şirket içi Active Directory ile bağlantı sorunları.](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-adconnectivitytools)

* [Çevrimiçi Azure Etkin Dizini ile bağlantı sorunları.](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-connectivity)

* [Şirket içi Active Directory ile izin sorunları.](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-configure-ad-ds-connector-account)

## <a name="recommended-documents"></a>**Önerilen Belgeler**
* [Azure AD Connect Önkoşulları](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-prerequisites)
* [Azure AD Connect için hangi yükleme türünün kullanılacağını seçme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-select-installation)
* [Hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-express)
* [Azure AD Connect özel yüklemesi](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-custom)
* [Azure AD Connect: Önceki bir sürümden en son sürüme yükseltme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-upgrade-previous-version)
* [Azure AD Connect: Evreleme sunucusu nedir?](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-topologies#staging-server)
* [ADConnectivityTool PowerShell Modülü nedir?](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-adconnectivitytools)

## <a name="next-steps"></a>Sonraki adımlar
- [Azure AD Connect eşitleme.](how-to-connect-sync-whatis.md)
- [Hibrit kimlik nedir?](whatis-hybrid-identity.md)





