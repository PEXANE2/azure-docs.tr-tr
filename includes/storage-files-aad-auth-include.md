---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/30/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: d470160a9b54af8751371aa4ca58202646c8fab8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84263386"
---
[Azure dosyaları](../articles/storage/files/storage-files-introduction.md) , [Şirket içi Active Directory Domain Services (AD DS)](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) ve [Azure Active Directory Domain Services (Azure AD DS)](../articles/active-directory-domain-services/overview.md)aracılığıyla sunucu ileti bloğu (SMB) üzerinden kimlik tabanlı kimlik doğrulamasını destekler. Bu makale, Azure dosya paylaşımlarının, SMB üzerinden Azure dosya paylaşımlarına kimlik tabanlı erişimi desteklemek için şirket içinde veya Azure 'da etki alanı hizmetlerini nasıl kullanabileceği konusunda odaklanır. Azure dosya paylaşımlarınız için kimlik tabanlı erişimin etkinleştirilmesi, var olan dizin hizmetinizi değiştirmeden mevcut dosya sunucularını Azure dosya paylaşımlarıyla değiştirmenize izin verir. böylece, paylaşımlara sorunsuz Kullanıcı erişimi sağlayabilirsiniz. 

Azure dosyaları, Kullanıcı erişimi üzerinde hem paylaşıma hem de dizin/dosya düzeylerine yönelik yetkilendirmeyi zorlar. Paylaşma düzeyi izin ataması, [rol tabanlı erişim denetimi (RBAC)](../articles/role-based-access-control/overview.md) modeliyle yönetilen Azure Active Directory (Azure AD) kullanıcıları veya grupları üzerinde gerçekleştirilebilir. RBAC ile, dosya erişimi için kullandığınız kimlik bilgileri kullanılabilir olmalıdır veya Azure AD ile eşitlenmelidir. Azure dosya paylaşımında okuma erişimi sağlamak için, Azure AD 'de kullanıcılara veya gruplara depolama dosya veri SMB paylaşma okuyucusu gibi yerleşik RBAC rolleri atayabilirsiniz.

Dizin/dosya düzeyinde Azure dosyaları, tüm Windows dosya sunucuları gibi [Windows DACL 'lerini](https://docs.microsoft.com/windows/win32/secauthz/access-control-lists) koruma, devralma ve zorunlu tutmayı destekler. Mevcut dosya paylaşımınız ve Azure dosya paylaşımlarınız arasında SMB üzerinden veri kopyalarken Windows DACL 'Lerini tutmayı seçebilirsiniz. Yetkilendirmeyi zorunlu kılabilirsiniz veya değil, Azure dosya paylaşımlarını kullanarak ACL 'Leri verilerle birlikte yedekleyebilirsiniz. 
