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
ms.openlocfilehash: e2a1eb8524c9d9a4b0ae603da3c05fbb20900111
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95995514"
---
Azure dosya paylaşımları için çok kanallı SMB Şu anda aşağıdaki kısıtlamalara sahiptir:
- Yalnızca yerel olarak yedekli dosya depolama hesaplarıyla birlikte kullanılabilir.
- Yalnızca Windows istemcileri için desteklenir. 
- En fazla kanal sayısı dördü.
- Doğrudan Erişimli SMB desteklenmez.
- Azure dosyaları için şirket içi Active Directory Domain Services (AD DS) veya Azure AD DS [kimlik tabanlı kimlik doğrulaması](../articles/storage/files/storage-files-active-directory-overview.md) etkinleştirilmiş depolama hesapları IÇIN, SMB istemcileri, dizin ve dosyalardaki NTFS izinlerini yapılandırmak Için Windows Dosya Gezgini 'ni kullanamaz.
    - İzinleri yapılandırmak için bunun yerine Windows [ıacl 'leri](/windows-server/administration/windows-commands/icacls) aracını veya [set-ACL](/powershell/module/microsoft.powershell.security/set-acl?view=powershell-7) komutunu kullanın.

