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
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76044960"
---
Aşağıdaki tabloda, Azure dijital TWINS 'de kullanılabilen roller açıklanmaktadır:

| **Rol** | **Açıklama** | **Tanımlayıcısını** |
| --- | --- | --- |
| Alan Yöneticisi | Belirtilen alan ve altındaki tüm düğümler için *Oluştur*, *Oku*, *Güncelleştir*ve *Sil* izinleri. Küresel izin. | 98e44ad7-28d4-4007-853b-b9968ad132d1 |
| Kullanıcı Yöneticisi| Kullanıcılar ve kullanıcıyla ilgili nesneler için *oluşturma*, *okuma*, *güncelleştirme*ve *silme* izni. Boşluklar için *Oku* izni. | dfaac54c-f583-4dd2-b45d-8d4bbc0aa1ac |
| Cihaz Yöneticisi | Cihazlar ve cihazla ilgili nesneler için *oluşturma*, *okuma*, *güncelleştirme*ve *silme* izni. Boşluklar için *Oku* izni. | 3cdfde07-bc16-40d9-bed3-66d49a8f52ae |
| Anahtar Yöneticisi | Erişim tuşları için *oluşturma*, *okuma*, *güncelleştirme*ve *silme* izni. Boşluklar için *Oku* izni. | 5a0b1afc-e118-4068-969f-b50efb8e5da6 |
| Belirteç Yöneticisi |  Erişim anahtarları için *Oku* ve *Güncelleştir* izinleri. Boşluklar için *Oku* izni. | 38a3bb21-5424-43b4-b0bf-78ee228840c3 |
| Kullanıcı |  İlgili ilgili nesneleri içeren boşluklar, algılayıcılar ve kullanıcılar için *okuma* izni. | b1ffdb77-c635-4e7e-ad25-948237d85b30 |
| Destek uzmanı |  Erişim anahtarları hariç her şey için *Oku* izni. | 6e46958b-dc62-4e7c-990c-c3da2e030969 |
| Cihaz yükleyicisi | İlgili ilgili nesneleri içeren cihazlar ve algılayıcılar için *okuma* ve *güncelleştirme* izni. Boşluklar için *Oku* izni. | b16dd9fe-4efe-467b-8c8c-720e2ff8817c |
| Ağ geçidi cihazı | Algılayıcılar için izin *Oluştur* . İlgili ilgili nesneleri içeren cihazlar ve algılayıcılar için *okuma* izni. | d4c69766-e9bd-4e61-bfc1-d8b6e686c7a8 |