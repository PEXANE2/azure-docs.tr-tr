---
title: Kapsayıcı gereksinimleri ve önerileri
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/18/2019
ms.author: dapine
ms.openlocfilehash: 47c889a428ec2916dc8cad73e30ef5ff630372a7
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71129810"
---
> [!NOTE]
> Gereksinimler ve öneriler, 29 satır ve toplam 803 karakter içeren taranmış bir iş harfinin 8 MB 'lik bir görüntüsünü kullanarak saniyede tek bir istek ile kıyaslamayı temel alır.

#### <a name="readtabread"></a>[Okuma](#tab/read)

Aşağıdaki tabloda, her bir okuma kapsayıcısı için kaynakların en düşük ve önerilen ayırması açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen |TPS<br>(En düşük, en yüksek)|
|-----------|---------|-------------|--|
| Okuma | 1 çekirdek, 8 GB bellek, 0,24 TPS | 8 çekirdek, 16 GB bellek, 1,17 TPS | 0,24, 1,17 |

#### <a name="recognize-texttabrecognize-text"></a>[Metin tanıma](#tab/recognize-text)

Aşağıdaki tabloda her bir Metin Tanıma kapsayıcısı için kaynakların en düşük ve önerilen ayırması açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen |TPS<br>(En düşük, en yüksek)|
|-----------|---------|-------------|--|
| Metin Tanıma | 1 çekirdek, 8 GB bellek, 0,12 TPS | 8 çekirdek, 16 GB bellek, 0,60 TPS | 0,12, 0,60 |

***

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.
* TPS-saniye başına işlem.

Çekirdek ve bellek, `--cpus` `docker run` komutunun bir parçası `--memory` olarak kullanılan ve ayarlarına karşılık gelir.
