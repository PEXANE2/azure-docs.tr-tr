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
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536599"
---
[Azure Dosyaları,](../articles/storage/files/storage-files-introduction.md) [şirket içi Active Directory Etki Alanı Hizmetleri (AD DS) (önizleme)](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) ve [Azure Active Directory Domain Services (Azure AD DS)](../articles/active-directory-domain-services/overview.md)aracılığıyla Sunucu İleti Bloğu (SMB) üzerinden kimlik tabanlı kimlik doğrulamayı destekler. Bu makalede, Azure dosya paylaşımlarının, SMB üzerinden Azure dosya paylaşımlarına kimlik tabanlı erişimi desteklemek için şirket içi veya Azure'da etki alanı hizmetlerini nasıl kullanabileceği üzerinde duruluyor. Azure dosya paylaşımlarınız için kimlik tabanlı erişimi etkinleştirmek, varolan dosya sunucularınızı mevcut dizin hizmetinizi değiştirmeden Azure dosya paylaşımlarıyla değiştirmenize ve paylaşımlara sorunsuz kullanıcı erişimini sağlamanıza olanak tanır. 

Azure Dosyaları, hem paylaşım hem de dizin/dosya düzeylerine kullanıcı erişimi yetkilendirmesi zorlar. Paylaşım düzeyi izin ataması, [rol tabanlı erişim denetimi (RBAC)](../articles/role-based-access-control/overview.md) modeli aracılığıyla yönetilen Azure Etkin Dizin (Azure AD) kullanıcıları veya gruplarında gerçekleştirilebilir. RBAC ile, dosya erişimi için kullandığınız kimlik bilgileri kullanılabilir veya Azure AD ile eşitlenmelidir. Azure dosya paylaşımına okuma erişimi sağlamak için Azure AD'deki kullanıcılara veya gruplara Depolama Dosyası Verileri SMB Pay Reader gibi yerleşik RBAC rolleri atayabilirsiniz.

Dizin/dosya düzeyinde Azure Dosyaları, windows [datl'lerini](https://docs.microsoft.com/windows/win32/secauthz/access-control-lists) tüm Windows dosya sunucularında olduğu gibi korumayı, devralmayı ve zorlamayı destekler. Varolan dosya paylaşımınız ile Azure dosya paylaşımlarınız arasında SMB üzerinden veri kopyalarken Windows DACL'larını saklamayı seçebilirsiniz. Yetkilendirmeyi zorlamayı planlasanız da istemeseniz de, verilerinizle birlikte ALAK'ları yedeklemek için Azure dosya paylaşımlarını kullanabilirsiniz. 
