---
title: Silinmiş bir depolama hesabını kurtarma
description: Silinen bir depolama hesabını nasıl kurtarılacın
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252624"
---
# <a name="how-to-recover-a-deleted-storage-account"></a>Silinmiş bir depolama hesabını kurtarma

Azure Depolama, otomatik yinelemeler aracılığıyla veri esnekliği sağlar, ancak kullanıcıların veya uygulama kodunun yanlışlıkla veya kötü amaçlı olarak verileri bozmasını engellemez. Uygulama veya kullanıcı hatası örnekleri sırasında veri doğruluğunun korunması, verileri denetim günlüğüyle ikincil bir depolama konumuna kopyalamak gibi daha gelişmiş teknikler gerektirir.

Aşağıdaki tablo, çoğaltma stratejisine bağlı olarak Depolama Hesabı Kurtarma kapsamının genel görünümünü sağlar.

| |LRS|ZRS|GRS|RA - GRS|
|---|---|---|---|---|
|Depolama Hesabı Azure Kaynak Yöneticisi|Evet|Evet|Evet|Evet|
|Depolama Hesabı Klasik|Evet|Evet|Evet|Evet|

Aşağıdaki bilgileri toplayın ve Microsoft Destek ile bir destek isteği dosyalayın:

* Depolama hesabı adı
* Silme tarihi
* Depolama hesabı bölgesi
* Depolama hesabı nasıl silindi?
* Depolama hesabı hangi yöntem silindi? (Portal, PowerShell, vb.)

Önemli Noktalar

* Çöp toplama işlemini gerçekleştirmek için depolama hizmetinin silinme tarihinden itibaren genellikle 15 gün kadar sürebilir, bu nedenle depolama hesaplarıkurtarma bir SLA ile kurtarılamayabilir.
* Microsoft Destek, Kapsayıcı/Hesabı en iyi şekilde kurtarmaya çalışır ve kurtarmayı garanti edemez.

> [!NOTE]
> Hesap yeniden oluşturulduysa kurtarma başarılı olmayabilir. Hesabı zaten yeniden oluşturduysanız, kurtarma girişiminde bulunulmadan önce hesabı silmeniz gerekir.
