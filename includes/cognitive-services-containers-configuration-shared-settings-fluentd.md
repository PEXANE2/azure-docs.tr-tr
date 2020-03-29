---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: b08516b35a864eae6d15c4c5c928f0550c64c239
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67712493"
---
Fluentd, birleşik günlüğe kaydetme için açık kaynak kodlu bir veri toplayıcısidir. Ayarlar, `Fluentd` kapsayıcının [Akıcı](https://www.fluentd.org) bir sunucuya bağlantısını yönetir. Kapsayıcı, kapsayıcınızın günlükler ve isteğe bağlı olarak akıcı bir sunucuya metrik veri yazmasına olanak tanıyan bir Akıcı günlük sağlayıcısı içerir.

Aşağıdaki tabloda `Fluentd` bölüm altında desteklenen yapılandırma ayarları açıklanmaktadır.

| Adı | Veri türü | Açıklama |
|------|-----------|-------------|
| `Host` | Dize | Fluentd sunucusunun IP adresi veya DNS ana bilgisayar adı. |
| `Port` | Tamsayı | Fluentd sunucusunun bağlantı noktası.<br/> Varsayılan değer 24224'dür. |
| `HeartbeatMs` | Tamsayı | Kalp atışı aralığı, milisaniye cinsinden. Bu aralık sona ermeden önce olay trafiği gönderilmemişse, Akıcı sunucuya bir sinyal gönderilir. Varsayılan değer 60000 milisaniyedir (1 dakika). |
| `SendBufferSize` | Tamsayı | İleti işlemleri için ayrılan ağ arabelleği alanı, baytlar içinde. Varsayılan değer 32768 bayt (32 kilobayt) 'dır. |
| `TlsConnectionEstablishmentTimeoutMs` | Tamsayı | Zaman alakart, milisaniye cinsinden, Fluentd sunucusuyla bir SSL/TLS bağlantısı kurmak için. Varsayılan değer 10000 milisaniye (10 saniye) olur.<br/> Yanlış `UseTLS` olarak ayarlanmışsa, bu değer yoksayılır. |
| `UseTLS` | Boole | Kapsayıcının Fluentd sunucusuyla iletişim kurmak için SSL/TLS kullanıp kullanmaması gerektiğini gösterir. Varsayılan değer false'tur. |