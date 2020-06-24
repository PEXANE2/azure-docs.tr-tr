---
title: Azure CDN 'de özel HTTPS 'yi etkinleştirmek için izin verilen CA
description: Özel bir etki alanında HTTPS 'yi etkinleştirmek için kendi sertifikanızı kullanıyorsanız, bu sertifikayı oluşturmak için izin verilen bir sertifika yetkilisini (CA) kullanmanız gerekir.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/18/2018
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: 29b6cb25e021e86ce6663b4db5c89217aaf70a37
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84887396"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Azure CDN 'de özel HTTPS 'yi etkinleştirmek için izin verilen sertifika yetkilileri

Azure Content Delivery Network (CDN) özel etki alanı için [kendi sertifikanızı kullanarak https özelliğini etkinleştirdiğinizde](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#tlsssl-certificates) belirli sertifika gereksinimlerini karşılamanız gerekir. **Microsoft profilinden Azure CDN Standard** , aşağıdaki listede onaylanan sertifika YETKILILERINDEN (CA) birinden bir sertifika gerektirir. Onaylanmamış bir CA 'dan bir sertifika veya otomatik olarak imzalanan bir sertifika kullanılıyorsa, istek reddedilir. Verizon profillerden Verizon ve **Azure CDN Premium** **'dan Azure CDN Standart** , geçerli bir CA 'dan geçerli herhangi bir sertifikayı kabul eder.

> [!NOTE]
> Özel etki alanı HTTPS özelliğini etkinleştirmek için kendi sertifikanızı kullanma seçeneği, **Akamai profillerinden Azure CDN Standart** *için kullanılamaz.* 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

