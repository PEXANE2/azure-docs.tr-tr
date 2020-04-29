---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/30/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 167d50e5c7f3049f46fd8540630e47044240809f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81536599"
---
[Azure dosyaları](../articles/storage/files/storage-files-introduction.md) , sunucu ileti bloğu (SMB) üzerinden [Şirket içi Active Directory Domain Services (AD DS)](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) (Önizleme) ve [Azure Active Directory Domain Services (Azure AD DS)](../articles/active-directory-domain-services/overview.md)üzerinden kimlik tabanlı kimlik doğrulamasını destekler. Bu makale, Azure dosya paylaşımlarının, SMB üzerinden Azure dosya paylaşımlarına kimlik tabanlı erişimi desteklemek için şirket içinde veya Azure 'da etki alanı hizmetlerini nasıl kullanabileceği konusunda odaklanır. Azure dosya paylaşımlarınız için kimlik tabanlı erişimin etkinleştirilmesi, var olan dizin hizmetinizi değiştirmeden mevcut dosya sunucularını Azure dosya paylaşımlarıyla değiştirmenize izin verir. böylece, paylaşımlara sorunsuz Kullanıcı erişimi sağlayabilirsiniz. 

Azure dosyaları, Kullanıcı erişimi üzerinde hem paylaşıma hem de dizin/dosya düzeylerine yönelik yetkilendirmeyi zorlar. Paylaşma düzeyi izin ataması, [rol tabanlı erişim denetimi (RBAC)](../articles/role-based-access-control/overview.md) modeliyle yönetilen Azure Active Directory (Azure AD) kullanıcıları veya grupları üzerinde gerçekleştirilebilir. RBAC ile, dosya erişimi için kullandığınız kimlik bilgileri kullanılabilir olmalıdır veya Azure AD ile eşitlenmelidir. Azure dosya paylaşımında okuma erişimi sağlamak için, Azure AD 'de kullanıcılara veya gruplara depolama dosya veri SMB paylaşma okuyucusu gibi yerleşik RBAC rolleri atayabilirsiniz.

Dizin/dosya düzeyinde Azure dosyaları, tüm Windows dosya sunucuları gibi [Windows DACL 'lerini](https://docs.microsoft.com/windows/win32/secauthz/access-control-lists) koruma, devralma ve zorunlu tutmayı destekler. Mevcut dosya paylaşımınız ve Azure dosya paylaşımlarınız arasında SMB üzerinden veri kopyalarken Windows DACL 'Lerini tutmayı seçebilirsiniz. Yetkilendirmeyi zorunlu kılabilirsiniz veya değil, Azure dosya paylaşımlarını kullanarak ACL 'Leri verilerle birlikte yedekleyebilirsiniz. 
