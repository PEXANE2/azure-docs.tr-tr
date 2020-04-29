---
title: Silinmiş bir depolama hesabını kurtarma
description: Silinen bir depolama hesabını kurtarmayı öğrenin
author: todmccoy
manager: dcscontentpm
ms.service: storage
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: rogarana
ms.subservice: files
services: storage
tags: ''
ms.openlocfilehash: 05465d4a03335ac607ba8981116c66fd6dac9416
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78252624"
---
# <a name="how-to-recover-a-deleted-storage-account"></a>Silinmiş bir depolama hesabını kurtarma

Azure depolama, otomatik çoğaltmalar aracılığıyla veri dayanıklılığı sağlar ancak kullanıcıların veya uygulama kodunun yanlışlıkla veya kötü amaçlı olarak verileri bozmasını engellemez. Uygulama veya Kullanıcı hatası örnekleri sırasında verilerin aslına uygunluğunu korumak, verileri bir denetim günlüğü ile ikincil bir depolama konumuna kopyalama gibi daha gelişmiş teknikler gerektirir.

Aşağıdaki tabloda, çoğaltma stratejisine bağlı olarak depolama hesabı kurtarma kapsamına genel bakış sunulmaktadır.

| |LRS|ZRS|GRS|RA-GRS|
|---|---|---|---|---|
|Depolama hesabı Azure Resource Manager|Yes|Yes|Yes|Yes|
|Depolama hesabı klasik|Yes|Yes|Yes|Yes|

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
