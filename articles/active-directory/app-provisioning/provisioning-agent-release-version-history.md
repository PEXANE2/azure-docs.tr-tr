---
title: 'Azure AD Connect sağlama Aracısı: sürüm sürümü geçmişi | Microsoft Docs'
description: Bu makalede Azure AD Connect sağlama aracısının tüm sürümleri listelenir ve yeni özellikler ve düzeltilen sorunlar açıklanmaktadır
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.topic: reference
ms.workload: identity
ms.date: 02/26/2020
ms.subservice: app-provisioning
ms.author: kenwith
ms.reviewer: celested
ms.openlocfilehash: 9e05d1a85f17800ddf4d77e4e4acba6396a8da47
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84781574"
---
# <a name="azure-ad-connect-provisioning-agent-version-release-history"></a>Azure AD Connect sağlama Aracısı: sürüm yayınlama geçmişi

Bu makalede, yayımlanan Azure Active Directory Connect sağlama aracısının sürümleri ve özellikleri listelenmektedir. Azure AD ekibi, sağlama aracısını yeni özellikler ve işlevlerle düzenli olarak güncelleştirir. Yeni bir sürüm yayınlandığında sağlama Aracısı otomatik olarak güncelleştirilir. 

Microsoft, en son aracı sürümü ve bir sürümü için doğrudan destek sağlar.

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

