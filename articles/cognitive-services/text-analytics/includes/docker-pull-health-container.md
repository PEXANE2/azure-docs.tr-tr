---
title: Sistem durumu kapsayıcısı için Docker çekme
titleSuffix: Azure Cognitive Services
description: Sistem durumu kapsayıcısı için Metin Analizi Docker Pull komutu
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: b28049f1854494b61d63824334b986d814a641cd
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91309328"
---
Kapsayıcıya erişim istemek için bilişsel [Hizmetler kapsayıcıları istek formunu](https://aka.ms/csgate) doldurun ve iletin.
Form, siz, şirketiniz ve kapsayıcısını kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Formu gönderdikten sonra, Azure bilişsel hizmetler ekibi, özel kapsayıcı kayıt defterine erişim ölçütlerini karşıladığınızdan emin olmak için bunu inceler.

> [!IMPORTANT]
> * Formunda, bir Azure aboneliği KIMLIĞIYLE ilişkili bir e-posta adresi kullanmanız gerekir.
> * Kapsayıcıyı çalıştırmak için kullandığınız Azure kaynağının, onaylanan Azure abonelik KIMLIĞIYLE oluşturulmuş olması gerekir. 
> * Microsoft 'tan uygulamanızın durumuyla ilgili güncelleştirmeler için e-postanızı (hem gelen kutusu hem de önemsiz klasörler) denetleyin.

Bilişsel hizmetler kapsayıcıları için özel kapsayıcı kayıt defterimize bağlanmak üzere ekleme e-postanıza girilen kimlik bilgileriyle Docker Login komutunu kullanın.


```Docker
docker login containerpreview.azurecr.io -u <username> -p <password>
```

[`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)Özel kapsayıcı kayıt Defterimizden bu kapsayıcı görüntüsünü indirmek için komutunu kullanın.

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-healthcare:latest
```
