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
ms.openlocfilehash: 4c33952ed56399b94a7db1088cb60919a4f88137
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108982"
---
Kapsayıcıya erişim istemek için bilişsel [Hizmetler kapsayıcıları istek formunu](https://aka.ms/cognitivegate) doldurun ve iletin.

[!INCLUDE [Request access to the container registry](../../../../includes/cognitive-services-containers-request-access-only.md)]

Bilişsel hizmetler kapsayıcıları için özel kapsayıcı kayıt defterimize bağlanmak üzere ekleme e-postanıza girilen kimlik bilgileriyle Docker Login komutunu kullanın.

```bash
docker login containerpreview.azurecr.io -u <username> -p <password>
```

[`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)Özel kapsayıcı kayıt Defterimizden bu kapsayıcı görüntüsünü indirmek için komutunu kullanın.

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-healthcare:latest
```
