---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/16/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: bbf0530c1a7f1a747d456d87efc106418f23b7ba
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98052963"
---
Azure dosya paylaşımları için çok kanallı SMB Şu anda aşağıdaki kısıtlamalara sahiptir:
- Yalnızca yerel olarak yedekli dosya depolama hesaplarıyla birlikte kullanılabilir.
- Yalnızca Windows istemcileri için desteklenir. 
- En fazla kanal sayısı dördü.
- Doğrudan Erişimli SMB desteklenmez.
- Depolama hesaplarının özel uç noktaları desteklenmez.
- Azure dosyaları için şirket içi Active Directory Domain Services (AD DS) veya Azure AD DS [kimlik tabanlı kimlik doğrulaması](../articles/storage/files/storage-files-active-directory-overview.md) etkinleştirilmiş depolama hesapları IÇIN, SMB istemcileri, dizin ve dosyalardaki NTFS izinlerini yapılandırmak Için Windows Dosya Gezgini 'ni kullanamaz.
    - İzinleri yapılandırmak için bunun yerine Windows [ıacl 'leri](/windows-server/administration/windows-commands/icacls) aracını veya [set-ACL](/powershell/module/microsoft.powershell.security/set-acl?view=powershell-7) komutunu kullanın.

