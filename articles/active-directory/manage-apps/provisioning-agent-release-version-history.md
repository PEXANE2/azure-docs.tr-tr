---
title: 'Azure AD Connect sağlama Aracısı: Sürüm sürümü geçmişi | Microsoft Docs'
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
ms.date: 04/05/2019
ms.subservice: app-mgmt
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: de6ef5dedb0779536cfbc1e6a3440e748ff78524
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70862115"
---
# <a name="azure-ad-connect-provisioning-agent-version-release-history"></a>Azure AD Connect sağlama Aracısı: Sürüm yayınlama geçmişi
Bu makalede, yayımlanan Azure AD Connect sağlama aracısının sürümleri ve özellikleri listelenmektedir. Azure AD ekibi, sağlama aracısını yeni özellikler ve işlevlerle düzenli olarak güncelleştirir. Yeni bir sürüm yayınlandığında sağlama aracıları otomatik olarak güncelleştirilir. 

En son özelliklere ve hata düzeltmelerine sahip olduğunuzdan emin olmak için aracılarınız için otomatik güncelleştirmeyi etkinleştirmenizi öneririz. Microsoft, en son aracı sürümü ve bir sürümü için doğrudan destek sağlar.

## <a name="11670"></a>1.1.67.0

### <a name="release-status"></a>Yayın durumu

9 Eylül 2019: Otomatik güncelleştirme için yayınlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Hazırlama Aracısı sorunlarını gidermek için ek izleme ve günlüğe kaydetme yapılandırma olanağı
* Eşitleme performansını geliştirmek için eşlemede yapılandırılan yalnızca bu AD özniteliklerini getirme olanağı

### <a name="fixed-issues"></a>Düzeltilen sorunlar

* AD bağlantı hatalarıyla ilgili sorunlar oluşursa aracının yanıt vermeyen bir duruma oluştuğu bir hata düzeltildi
* İkili veriler Active Directory okunmadığında soruna neden olan bir hata düzeltildi
* Bir hata düzeltildi-aracıda bulut hibrit kimlik hizmeti ile güveni yenileyemedi

## <a name="11300"></a>1.1.30.0

### <a name="release-status"></a>Yayın durumu

23 Ocak 2019: İndirilmek üzere yayınlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Daha iyi performans, kararlılık ve güvenilirlik için bağlayıcı mimarisi & sağlama Aracısı 
* UI denetimli Yükleme Sihirbazı kullanarak Basitleştirilmiş sağlama Aracısı yapılandırması 
* Otomatik aracı güncelleştirmeleri için destek eklendi

