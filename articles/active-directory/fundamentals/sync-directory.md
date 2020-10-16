---
title: Azure Active Directory ile dizin eşitleme
description: Bu eşitleme modelini elde etmek için mimari yönergeler
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 862d17948cb09c18f9372f8b8b981e5efa6be71b
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114316"
---
# <a name="directory-synchronization"></a>Dizin eşitlemesi

Birçok kuruluşun hem şirket içi hem de bulut bileşenlerini çevreleyen bir karma altyapısı vardır. Kullanıcıların kimliklerini yerel ve bulut dizinleri arasında eşitleme, kullanıcıların tek bir kimlik bilgileri kümesiyle kaynaklara erişmesini sağlar. 

Eşitleme, şu işlemdir 

* belirli koşullara göre nesne oluşturma
* nesnenin güncel tutulması
* koşullar artık karşılanamadığında nesne kaldırılıyor. 

Şirket içi sağlama, şirket içi kaynaklardan (Active Directory benzer) Azure Active Directory (Azure AD) için sağlamayı içerir. 

## <a name="use-when"></a>Şu durumlarda kullanın

Şirket içi Active Directory ortamlarınızdaki kimlik verilerini Azure AD 'ye eşitlemeniz gerekir.

![mimari diyagram](./media/authentication-patterns/dir-sync-auth.png)

## <a name="components-of-system"></a>Sistem bileşenleri

* **Kullanıcı**: Azure ad kullanarak bir uygulamaya erişir.

* **Web tarayıcısı**: kullanıcının uygulamanın dış URL 'sine erişmek için etkileşimde bulunduğu bileşen.

* **Uygulama**: kimlik doğrulaması ve yetkilendirme AMACıYLA Azure AD 'nin kullanımına dayalı Web uygulaması.

* **Azure AD**: kimlik bilgilerini kuruluşun şirket içi dizininden Azure AD Connect aracılığıyla eşitler. 

* **Azure AD Connect**: şirket kimliği altyapılarına Microsoft Azure AD bağlanmak için bir araç. Sihirbaz ve kılavuzlu deneyimler, bağlantı için gerekli olan önkoşulları ve bileşenleri dağıtmanıza ve yapılandırmanıza yardımcı olur ve Active Directory 'den Azure AD 'ye oturum açın. 

* **Active Directory**: Active Directory, çoğu Windows Server işletim sisteminde yer alan bir dizin hizmetidir. Active Directory Domain Services çalıştıran sunuculara (AD DS) etki alanı denetleyicileri denir. Bunlar, etki alanındaki tüm kullanıcıları ve bilgisayarları doğrular ve yetkilendirirler.

## <a name="implement-directory-synchronization-with-azure-ad"></a>Azure AD ile dizin eşitlemeyi uygulama

* [Kimlik sağlama nedir?](https://docs.microsoft.com/azure/active-directory/cloud-provisioning/what-is-provisioning) 

* [Karma kimlik dizini tümleştirme araçları](https://docs.microsoft.com/azure/active-directory/hybrid/plan-hybrid-identity-design-considerations-tools-comparison) 

* [Azure AD Connect yükleme yol haritası](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-roadmap)
