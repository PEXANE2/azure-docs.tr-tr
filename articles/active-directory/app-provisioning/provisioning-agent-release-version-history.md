---
title: 'Azure AD Connect sağlama Aracısı: sürüm sürümü geçmişi | Microsoft Docs'
description: Bu makalede Azure AD Connect sağlama aracısının tüm sürümleri listelenir ve yeni özellikler ve düzeltilen sorunlar açıklanmaktadır
services: active-directory
documentationcenter: ''
author: cmmdesai
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/04/2020
ms.subservice: app-provisioning
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: 164d156be030aa4a177ea77ed61782780338abb0
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77522382"
---
# <a name="azure-ad-connect-provisioning-agent-version-release-history"></a>Azure AD Connect sağlama Aracısı: sürüm yayınlama geçmişi
Bu makalede, yayımlanan Azure Active Directory Connect sağlama aracısının sürümleri ve özellikleri listelenmektedir. Azure AD ekibi, sağlama aracısını yeni özellikler ve işlevlerle düzenli olarak güncelleştirir. Yeni bir sürüm yayınlandığında sağlama Aracısı otomatik olarak güncelleştirilir. 

En son özelliklere ve hata düzeltmelerine sahip olduğunuzdan emin olmak için aracılarınız için otomatik güncelleştirmeyi etkinleştirmenizi öneririz. Microsoft, en son aracı sürümü ve bir sürümü için doğrudan destek sağlar.

## <a name="11960"></a>1.1.96.0

### <a name="release-status"></a>Yayın durumu

4 Aralık 2019: indirme için yayınlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Kullanıcıyı eşitlemeye yönelik [Azure AD Connect bulut sağlama](../cloud-provisioning/what-is-cloud-provisioning.md) , şirket içi Active Directory Ile Azure AD arasında iletişim kurma ve veri gruplama için destek içerir


## <a name="11670"></a>1.1.67.0

### <a name="release-status"></a>Yayın durumu

9 Eylül 2019: otomatik güncelleştirme için yayınlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Hazırlama Aracısı sorunlarını gidermek için ek izleme ve günlüğe kaydetme yapılandırma olanağı
* Yalnızca eşleme performansını geliştirmek için eşlemede yapılandırılmış Azure AD özniteliklerini getirme olanağı

### <a name="fixed-issues"></a>Düzeltilen sorunlar

* Azure AD bağlantı hatalarıyla ilgili sorunlar oluşursa, aracıdaki bir hata düzeltilmedi durumuna geçti
* İkili veriler Azure Active Directory okunmadığında soruna neden olan bir hata düzeltildi
* Aracıdaki bir hata düzeltildiğinde bulut hibrit kimlik hizmeti ile güveni yenileyemedi

## <a name="11300"></a>1.1.30.0

### <a name="release-status"></a>Yayın durumu

23 Ocak 2019: indirilmek üzere yayınlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Daha iyi performans, kararlılık ve güvenilirlik için sağlama Aracısı ve bağlayıcı mimarisi kullanıma alınıyor 
* UI tabanlı yükleme sihirbazını kullanarak sağlama Aracısı yapılandırması basitleşme 
* Otomatik aracı güncelleştirmeleri için destek eklendi

