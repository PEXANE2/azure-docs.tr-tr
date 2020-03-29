---
title: 'Azure AD Connect: Yükleme türünü seçin | Microsoft Dokümanlar'
description: Bu konu, Azure AD Connect için kullanılacak yükleme türünü seçme konusunda size yol
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 90a624a6b3b4696899af0d8606f653df260cc201
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60348289"
---
# <a name="select-which-installation-type-to-use-for-azure-ad-connect"></a>Azure AD Connect için hangi yükleme türünün kullanılacağını seçme
Azure AD Connect'in yeni yükleme için iki yükleme türü vardır: Express ve özelleştirilmiş. Bu konu, yükleme sırasında hangi seçeneği kullanacağınıza karar vermenize yardımcı olur.

## <a name="express"></a>Express
Express en yaygın seçenektir ve tüm yeni kurulumların yaklaşık %90'ı tarafından kullanılır. En yaygın müşteri senaryoları için çalışan bir yapılandırma sağlamak üzere tasarlanmıştır.

Bu varsayar:

- Şirket içinde tek bir Active Directory ormanın var.
- Yükleme için kullanabileceğiniz bir kurumsal yönetici hesabınız var.
- Şirket içi Active Directory'nizde 100.000'den az nesne var.

Şunları elde elabilirsiniz:

- Tek oturum açma için şirket içi azure AD'ye parola [karma eşitleme.](how-to-connect-password-hash-synchronization.md)
- [Kullanıcıları, grupları, kişileri ve Windows 10 bilgisayarlarını](concept-azure-ad-connect-sync-default-configuration.md)eşitleyen bir yapılandırma.
- Tüm etki alanlarında ve tüm OUs'larda tüm uygun nesnelerin senkronizasyonu.
- [Kullanılabilir](how-to-connect-install-automatic-upgrade.md) en son sürümü her zaman kullandığınızdan emin olmak için otomatik yükseltme etkinleştirilir.

Express'i kullanmaya devam edebileceğiniz seçenekler:

- Tüm OS'leri eşitlemek istemiyorsanız, Express'i kullanmaya devam edebilirsiniz ve son sayfada seç **Eşitleme işlemini başlat...***. Ardından yükleme sihirbazını yeniden çalıştırın ve [yapılandırma seçeneklerindeki](how-to-connect-installation-wizard.md#customize-synchronization-options) OS'leri değiştirin ve zamanlanmış eşitlemeyi etkinleştirin.
- Azure AD Premium'daki Parola yazma gibi özelliklerden birini etkinleştirmek istiyorsunuz. İlk yüklemenin tamamlanması için express'ten geçin. Ardından yükleme sihirbazını yeniden çalıştırın ve [yapılandırma seçeneklerini](how-to-connect-installation-wizard.md#customize-synchronization-options)değiştirin.

## <a name="custom"></a>Özel
Özelleştirilmiş yol, ekspresten çok daha fazla seçenek sunar. Express için önceki bölümde açıklanan yapılandırmanın kuruluşunuz için temsil olmadığı tüm durumlarda kullanılmalıdır.

Şu durumlarda kullanın:

- Active Directory'deki bir kurumsal yönetici hesabına erişiminiz yok.
- Birden fazla ormanın var veya gelecekte birden fazla ormanı senkronize etmeyi planlıyorsun.
- Ormanınızda Connect sunucusundan erişilemeyecek etki alanları vardır.
- Kullanıcı oturum açma için federasyon veya geçiş kimlik doğrulaması kullanmayı planlıyorsunuz.
- 100.000'den fazla nesneniz var ve tam bir SQL Server kullanmanız gerekir.
- Grup tabanlı filtreleme kullanmayı planlıyorsunuz ve yalnızca etki alanı veya OU tabanlı filtreleme kullanmayı planlıyorsunuz.

## <a name="upgrade-from-dirsync"></a>DirSync'ten yükseltme
Şu anda DirSync kullanıyorsanız, varolan yapılandırmanızı yükseltmek için [DirSync'den Yükseltme](how-to-dirsync-upgrade-get-started.md) adımlarını izleyin. İki farklı yükseltme seçeneği vardır:

- Connect'i aynı sunucuya yüklemek için yerinde yükseltme.
- Varolan DirSync sunucusu hala çalışır durumdayken Connect'i yeni bir sunucuya yüklemek için paralel dağıtım.

## <a name="upgrade-from-azure-ad-sync"></a>Azure AD Eşitleme'den yükseltme
Şu anda Azure AD Eşitlemesi kullanıyorsanız, bir Connect sürümünden daha yenisine yükselttiğiniz adımlarla [aynı adımları](how-to-upgrade-previous-version.md) izleyebilirsiniz. İki farklı yükseltme seçeneği vardır:

- Connect'i aynı sunucuya yüklemek için yerinde yükseltme.
- Varolan Azure AD Sync sunucusu hala çalışır durumdayken Connect'i yeni bir sunucuya yüklemek için geçiş geçiş.

## <a name="migrate-from-fim2010-or-mim2016"></a>FIM2010 veya MIM2016'dan geçiş
Şu anda Azure AD Bağlayıcısı ile Forefront Identity Manager 2010 veya Microsoft Identity Manager 2016 kullanıyorsanız, tek seçeneğiniz geçiştir. [Salıncak geçişinde](how-to-upgrade-previous-version.md#swing-migration)açıklanan adımları izleyin. Adımlarda, Azure AD Sync'inherhangi bir şekilde bahsedilmesi yle FIM2010/MIM2016'yı değiştirin.

## <a name="next-steps"></a>Sonraki adımlar
Kullanmayı seçtiğiniz seçene bağlı olarak, ayrıntılı adımlarla makalenizi bulmak için soldaki içerik tablosunu kullanın.
