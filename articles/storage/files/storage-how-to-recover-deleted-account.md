---
title: Azure depolama ile Azure dosya paylaşımının kullanımı | Microsoft Docs
description: Azure dosya paylaşımını Windows ve Windows Server ile kullanmayı öğrenin.
author: todmccoy
manager: dcscontentpm
ms.service: storage
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: rogarana
ms.subservice: files
services: storage
tags: ''
ms.openlocfilehash: e1e581134b4e3821659c43bb7f55003239594bd9
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74233798"
---
# <a name="how-to-recover-a-deleted-storage-account"></a>Silinen bir depolama hesabını kurtarma

Azure depolama, otomatik çoğaltmalar aracılığıyla veri dayanıklılığı sağlar ancak kullanıcıların veya uygulama kodunun yanlışlıkla veya kötü amaçlı olarak verileri bozmasını engellemez. Uygulama veya Kullanıcı hatası örnekleri sırasında verilerin aslına uygunluğunu korumak, verileri bir denetim günlüğü ile ikincil bir depolama konumuna kopyalama gibi daha gelişmiş teknikler gerektirir.

Aşağıdaki tabloda, çoğaltma stratejisine bağlı olarak depolama hesabı kurtarma kapsamına genel bakış sunulmaktadır.

| |LRS|ZRS|GRS|RA-GRS|
|---|---|---|---|---|
|Depolama hesabı Azure Resource Manager|Evet|Evet|Evet|Evet|
|Depolama hesabı klasik|Evet|Evet|Evet|Evet|

Aşağıdaki bilgileri toplayın ve Microsoft Desteği bir destek isteği dosyasına koyun:

* Depolama hesabı adı
* Silme tarihi
* Depolama hesabı bölgesi
* Depolama hesabı nasıl silindi?
* Depolama hesabı hangi yöntemi sildi? (Portal, PowerShell, vb.)

Önemli nokta

* Depolama hizmetinin çöp toplama işlemini gerçekleştirmesi için silinme zamanından genellikle 15 güne kadar sürebilir ve depolama hesapları kurtarması bir SLA ile kurtarılamaz.
* Microsoft Desteği, kapsayıcıyı/hesabı en iyi çaba temelinde kurtarmaya çalışacaktır ve kurtarmayı garanti edemez.

> [!NOTE]
> Hesap yeniden oluşturulduysa kurtarma başarılı olmayabilir. Hesabı zaten yeniden oluşturduysanız, kurtarmaya başlamadan önce onu silmeniz gerekir.
