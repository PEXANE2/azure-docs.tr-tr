---
title: 'Azure AD Connect Provisioning Aracısı: Sürüm sürüm geçmişi | Microsoft Dokümanlar'
description: Bu makalede, Azure AD Bağlantı Sağlama Aracısı'nın tüm sürümleri listelenir ve yeni özellikler ve sabit sorunları açıklar
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
ms.date: 02/26/2020
ms.subservice: app-provisioning
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: 559bca4f5020cebe06be7f24f7af5ec2e94bec0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183253"
---
# <a name="azure-ad-connect-provisioning-agent-version-release-history"></a>Azure AD Connect Provisioning Aracısı: Sürüm sürüm geçmişi
Bu makalede, azure Active Directory Connect Provisioning Agent'ın yayımlanmış sürümleri ve özellikleri listelenmiştir. Azure AD ekibi, Sağlama Aracısını düzenli olarak yeni özellikler ve işlevlerle güncelleştirir. Yeni bir sürüm yayımlandığında, Sağlama Aracısı otomatik olarak güncelleştirilir. 

Microsoft, en son aracı sürümü ve daha önce bir sürüm için doğrudan destek sağlar.

## <a name="11960"></a>1.1.96.0

### <a name="release-status"></a>Yayın durumu

Aralık 4, 2019: İndiriçin yayımlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Şirket içi Active Directory'den Azure AD'ye kullanıcı, kişi ve grup verilerini eşitlemek için [Azure AD Connect bulut sağlama](../cloud-provisioning/what-is-cloud-provisioning.md) desteği içerir


## <a name="11670"></a>1.1.67.0

### <a name="release-status"></a>Yayın durumu

9 Eylül 2019: Otomatik güncelleme için yayımlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Madde hata ayıklama Aracısı sorunları için ek izleme ve günlük yapılandırma yeteneği
* Eşitleme performansını artırmak için eşlemede yapılandırılan yalnızca bu Azure AD özniteliklerini alma özelliği

### <a name="fixed-issues"></a>Düzeltilen sorunlar

* Azure AD bağlantı hatalarıyla ilgili sorunlar varsa, aracının yanıt vermeyen durumuna girdiği bir hata düzeltildi
* Azure Etkin Dizini'nden ikili veriler okunduğunda sorunlara neden olan bir hata düzeltildi
* Aracının bulut karma kimlik hizmetiyle güven tazelemediği bir hata düzeltildi

## <a name="11300"></a>1.1.30.0

### <a name="release-status"></a>Yayın durumu

Ocak 23, 2019: İndiriçin yayımlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Daha iyi performans, kararlılık ve güvenilirlik için Provizyon Aracısı ve konektör mimarisini yeniledi 
* Kullanıcı Aracısı tabanlı yükleme sihirbazını kullanarak Sağlama Aracısı yapılandırmasını basitleştirdi 
* Otomatik aracı güncelleştirmeleri için destek eklendi

