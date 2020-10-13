---
title: dosya dahil etme
description: dosya dahil etme
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 02/14/2020
ms.topic: include
ms.openlocfilehash: 5eb2517b4b71b61dd70406f345c005d007d9e3ef
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91535975"
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