---
title: Sistem durumu kapsayıcısı için Docker çekme
titleSuffix: Azure Cognitive Services
description: Sistem durumu kapsayıcısı için Metin Analizi Docker Pull komutu
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: a0b2c9548f9c1289ae0abd61a72d7146a3bbca29
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94965176"
---
Sistem durumu genel önizlemesi için Metin Analizi erişim istemek için bilişsel [Hizmetler istek formunu](https://aka.ms/csgate) doldurun ve iletin.  Bu uygulama hem kapsayıcı hem de barındırılan Web API genel önizlemesi için geçerlidir.
Form, siz, şirketiniz ve kapsayıcısını kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Formu gönderdikten sonra, Azure bilişsel hizmetler ekibi, özel kapsayıcı kayıt defterine erişim ölçütlerini karşıladığınızdan emin olmak için bunu inceler.

> [!IMPORTANT]
> * Formunda, bir Azure aboneliği KIMLIĞIYLE ilişkili bir e-posta adresi kullanmanız gerekir.
> * Kapsayıcıyı çalıştırmak için kullandığınız Azure kaynağının, onaylanan Azure abonelik KIMLIĞIYLE oluşturulmuş olması gerekir. 
> * Microsoft 'tan uygulamanızın durumuyla ilgili güncelleştirmeler için e-postanızı (hem gelen kutusu hem de önemsiz klasörler) denetleyin.

Onaylandığında, özel kapsayıcı kayıt defterine erişmek için kimlik bilgilerini içeren bir e-posta gönderilir.  Bilişsel hizmetler kapsayıcıları için özel kapsayıcı kayıt defterimize bağlanmak üzere ekleme e-postanıza girilen kimlik bilgileriyle Docker Login komutunu kullanın.


```Docker
docker login containerpreview.azurecr.io -u <username> -p <password>
```

[`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)Özel kapsayıcı kayıt Defterimizden bu kapsayıcı görüntüsünü indirmek için komutunu kullanın.

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-healthcare:latest
```
