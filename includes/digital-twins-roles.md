---
title: include dosyası
description: include dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/15/2020
ms.custom: include file
ms.openlocfilehash: 864d693f2919922b5035f963103b2ce98600a51c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76044960"
---
Aşağıdaki tabloda Azure Digital Twins'te kullanılabilen roller açıklanmaktadır:

| **Rol** | **Açıklama** | **Tanımlayıcı** |
| --- | --- | --- |
| Alan Yöneticisi | *Belirtilen*alan ve altındaki tüm düğümler için CREATE , *READ*, *UPDATE*ve *DELETE* izni. Genel izin. | 98e44ad7-28d4-4007-853b-b9968ad132d1 |
| Kullanıcı Yöneticisi| Kullanıcılar ve kullanıcıyla ilgili nesneler için *CREATE,* *READ,* *UPDATE*ve *DELETE* izni. Boşluklar için izin *OKUYUN.* | dfaac54c-f583-4dd2-b45d-8d4bbc0aa1ac |
| Aygıt Yöneticisi | Aygıtlar ve aygıtla ilgili nesneler için *CREATE,* *READ,* *UPDATE*ve *DELETE* izni. Boşluklar için izin *OKUYUN.* | 3cdfde07-bc16-40d9-yatak3-66d49a8f52ae |
| Anahtar Yönetici | *Erişim*anahtarları için CREATE , *READ*, *UPDATE*ve *DELETE* izni. Boşluklar için izin *OKUYUN.* | 5a0b1afc-e118-4068-969f-b50efb8e5da6 |
| Belirteç Yöneticisi |  Erişim anahtarları için *OKUMA* ve *GÜNCELLEME* izni. Boşluklar için izin *OKUYUN.* | 38a3bb21-5424-43b4-b0bf-78ee28840c3 |
| Kullanıcı |  Karşılık gelen ilgili nesneleri içeren boşluklar, sensörler ve kullanıcılar için *okuma* izni. | b1ffdb77-c635-4e7e-ad25-948237d85b30 |
| Destek Uzmanı |  Erişim anahtarları dışındaki her şey için *OKUMA* İzNi. | 6e46958b-dc62-4e7c-990c-c3da2e030969 |
| Cihaz Yükleyici | İlgili nesneleri içeren aygıtlar ve sensörler için *OKUMA* ve *GÜNCELLEME* izni. Boşluklar için izin *OKUYUN.* | b16dd9fe-4efe-467b-8c8c-720e2ff8817c |
| Ağ Geçidi Cihazı | Sensörler için izin *oluşturun.* İlgili ilgili nesneleri içeren aygıtlar ve sensörler için *READ* izni. | d4c69766-e9bd-4e61-bfc1-d8b6e686c7a8 |