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
ms.openlocfilehash: b248bbb526baf355faf2564358884fd83422b037
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77565103"
---
[Azure Dosyaları,](../articles/storage/files/storage-files-introduction.md) [Active Directory (AD) (önizleme)](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) ve [Azure Active Directory Domain Services (Azure AD DS) (GA)](../articles/active-directory-domain-services/overview.md) aracılığıyla Sunucu İleti Bloğu (SMB) üzerinden kimlik tabanlı kimlik doğrulamayı destekler. Bu makalede, Azure Dosyaları'nın, Kobİ üzerinden Azure Dosyalarına kimlik tabanlı erişimi desteklemek için şirket içi veya Azure'da etki alanı hizmetlerinden nasıl yararlanabileceği üzerinde duruluyor. Bu, varolan dosya sunucularınızı Azure Dosyaları ile kolayca değiştirmenize ve paylaşımlara sorunsuz kullanıcı erişimi sağlamak için varolan dizin hizmetinizi kullanmaya devam etmenizi sağlar. 

Azure Files, kullanıcının hem paylaşım hem de dizin/dosya düzeyine erişimi ne kadar yetkilendirme yi zorlar. Paylaşım düzeyi izin ataması, Azure REKLAM kullanıcılarına veya tipik [rol tabanlı erişim denetimi (RBAC)](../articles/role-based-access-control/overview.md) modeli yle yönetilen gruplara atanabilir. RBAC ile, dosya erişimi için kullandığınız kimlik bilgileri kullanılabilir veya Azure AD ile eşitlenmelidir. Azure dosya paylaşımına okuma erişimi sağlamak için Azure AD'deki kullanıcılara veya gruplara Depolama Dosyası Verileri SMB Pay Reader gibi yerleşik RBAC rolleri atayabilirsiniz.

Dizin/dosya düzeyinde Azure Dosyaları, windows [datl'lerini](https://docs.microsoft.com/windows/win32/secauthz/access-control-lists) tüm Windows dosya sunucularında olduğu gibi korumayı, devralmayı ve zorlamayı destekler. Verileri Bir dosya paylaşımından SPK üzerinden Azure Dosyaları'na kopyalarsanız veya tam tersi, Windows DACL'ları saklamayı seçebilirsiniz. Yetkilendirmeyi zorlamayı planlasanız da planlamasanız da, Verilerinizle birlikte ALAK'ları yedeklemek için Azure Dosyalarından yararlanabilirsiniz. 
