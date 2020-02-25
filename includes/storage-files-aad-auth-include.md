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
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2020
ms.locfileid: "77565103"
---
[Azure dosyaları](../articles/storage/files/storage-files-introduction.md) , sunucu ileti bloğu (SMB) üzerinden [Active Directory (ad)](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) (Önizleme) ve [Azure Active Directory Domain Services (Azure AD DS)](../articles/active-directory-domain-services/overview.md) (GA) üzerinden kimlik tabanlı kimlik doğrulamasını destekler. Bu makalede, Azure dosyalarının SMB üzerinden Azure dosyalarına kimlik tabanlı erişimi desteklemek için şirket içinde ya da Azure 'da etki alanı hizmetlerinden nasıl yararlanacağı ele alınmaktadır. Bu, mevcut Dosya sunucularınızı Azure dosyaları ile kolayca değiştirmenize ve var olan dizin hizmetinizi kullanarak paylaşımlara sorunsuz Kullanıcı erişimi sürdürmeye devam etmenize olanak tanır. 

Azure dosyaları, Kullanıcı erişimi için paylaşıma ve dizin/dosya düzeyine erişimi zorlar. Paylaşma düzeyi izin atama, Azure AD kullanıcılarına veya tipik [rol tabanlı erişim denetimi (RBAC)](../articles/role-based-access-control/overview.md) modeliyle Yönetilen gruplara atanabilir. RBAC ile, dosya erişimi için kullandığınız kimlik bilgileri kullanılabilir olmalıdır veya Azure AD ile eşitlenmelidir. Azure dosya paylaşımında okuma erişimi sağlamak için, Azure AD 'de kullanıcılara veya gruplara depolama dosya veri SMB paylaşma okuyucusu gibi yerleşik RBAC rolleri atayabilirsiniz.

Dizin/dosya düzeyinde Azure dosyaları, tüm Windows dosya sunucuları gibi [Windows DACL 'lerini](https://docs.microsoft.com/windows/win32/secauthz/access-control-lists) koruma, devralma ve zorunlu tutmayı destekler. SMB üzerinden verileri bir dosya paylaşımından Azure dosyalarına kopyalayabilir veya bunun tersini yaparsanız, Windows DACL 'Lerini tutmayı seçebilirsiniz. Yetkilendirmeyi zorunlu olarak planlayıp planlamayacaksanız, Azure dosyalarından yararlanarak, verileri verilerle birlikte ACL 'Leri de yedekleyebilirsiniz. 
