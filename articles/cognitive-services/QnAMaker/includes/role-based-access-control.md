---
title: include dosyası
description: include dosyası
ms.topic: include
ms.custom: include file
ms.date: 03/11/2020
ms.openlocfilehash: b34d3ed2481600cde7ffb3275d635b8a57926bd6
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83665954"
---
Aşağıdaki roller işbirliği için verilmiştir:

|Rol|İşlevler|API erişimi|API izinleri|
|--|--|--|--|
|Sahip|Tümü|Kimlik doğrulama anahtarı|Tümü|
|Katılımcı|Rollere yeni üye ekleyebilme dışında tümü|Kimlik doğrulama anahtarı|Rollere yeni üye ekleyebilme dışında tümü|
|Soru-Cevap Oluşturma okuma<br>okuyamaz|Dışarı aktar/Indir<br>Test et|Taşıyıcı belirteci|1. KB API indirin<br>2. Kullanıcı API 'SI için KBs listesini listeleyin<br>3. Bilgi Bankası ayrıntıları alın<br>4. değişiklikleri indirin<br>Yanıt oluştur |
|Soru-Cevap Oluşturma Düzenleyicisi<br>(okuma/yazma)|Dışarı aktar/Indir<br>Test et<br>Güncelleştirme KB<br>Dışarı aktarma KB<br>İçeri aktarma KB<br>KB değiştirme<br>KB Oluşturma|Taşıyıcı belirteci|1. KB API oluştur<br>2. güncelleştirme KB API<br>3. KB API 'sini Değiştir<br>4. değişiklikleri Değiştir<br>5. "API 'YI eğitme" [yeni hizmet modeli V5 içinde]|
|Bilişsel hizmet kullanıcısı<br>(okuma/yazma/yayımlama)|Tümü|Taşıyıcı belirteci|Tümü|