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
ms.openlocfilehash: b9452baf0301233a1cd6e05621a89d013d32c9e0
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90906105"
---
Kapsayıcıya erişim istemek için bilişsel [Hizmetler kapsayıcıları istek formunu](https://aka.ms/cognitivegate) doldurun ve iletin.
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
