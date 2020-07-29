---
title: dosya dahil etme
description: dosya dahil etme
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/12/2018
ms.author: raynew
ms.openlocfilehash: f4cd9cad3813378fcdc3f06e8ab1c28eced4f93c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188896"
---
Name | Ticari URL | Kamu URL 'SI | Açıklama
--- | --- | --- | ---
Azure Active Directory | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | Azure Active Directory kullanarak erişim denetimi ve kimlik yönetimi için kullanılır. 
Backup | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Çoğaltma veri aktarımı ve düzenlemesi için kullanılır.
Çoğaltma | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Çoğaltma yönetimi işlemleri ve düzenleme için kullanılır.
Depolama | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Çoğaltılan verileri depolayan depolama hesabına erişim için kullanılır.
Telemetri (isteğe bağlı) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Telemetri için kullanılır.
Zaman eşitleme | ``time.windows.com`` | ``time.nist.gov`` | Tüm dağıtımlarda sistem ve genel saat arasındaki saat eşitlemesini denetlemek için kullanılır.


