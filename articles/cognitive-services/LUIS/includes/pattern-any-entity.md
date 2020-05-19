---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 02/14/2020
ms.topic: include
ms.author: diberry
ms.openlocfilehash: 8aef4a5eeeec27bfb2ca3edf9ff6823b88431319
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83591853"
---
pattern.any varlığı, ifade nedeniyle varlığın sonunun konuşmanın geri kalanından ayrılmasının zor olduğu durumlarda serbest biçimli verileri bulmanızı sağlar.

Insan kaynakları uygulaması, çalışanların şirket formlarını bulmasına yardımcı olur.

|İfade|
|--|
|Where is **HRF-123456**? (HRF-123456 nerede?)|
|Who authored **HRF-123234**? (HRF-123234'ü kim yazdı?)|
|**HRF-456098** is published in French? (HRF-456098 Fransızca mı yayımlandı?)|

Ancak her formun hem bir biçimlendirilmiş adı (yukarıdaki tabloda kullanılan) hem de kolay adı (`Request relocation from employee new to the company 2018 version 5` gibi) vardır.

Formun kolay adı konuşmada şu şekilde olur:

|İfade|
|--|
|Where is **Request relocation from employee new to the company 2018 version 5**? (Yeni şirket çalışanı taşınma talep formu 2018 sürüm 5 nerede?)|
|Who authored **"Request relocation from employee new to the company 2018 version 5"**? (Yeni şirket çalışanı taşınma talep formu 2018 sürüm 5'i kim yazdı?)|
|**Request relocation from employee new to the company 2018 version 5** is published in French? (Yeni şirket çalışanı taşınma talep formu 2018 sürüm 5 Fransızca mı yayımlandı?)|

Sözcüklerin uzunluklarının değişmesi, LUIS'in varlığın sonunu belirleme konusunda karışıklık yaşamasına neden olabilir. Pattern.any varlığını bir desen içinde kullanmak, form adının başını ve sonunu belirterek LUIS'in form adını doğru şekilde ayıklamasına yardımcı olmanızı sağlar.

|Konuşma şablonu örneği|
|--|
|Where is {FormName}[?] ({FormName} nerede[?])|
|Who authored {FormName}[?] ({FormName} kim yazdı[?])|
|{FormName} is published in French[?] ({FormName} Fransızca mı yayımlandı[?])|

Gözden geçirme stili. tüm [başvuru](../reference-entity-pattern-any.md) bilgileri