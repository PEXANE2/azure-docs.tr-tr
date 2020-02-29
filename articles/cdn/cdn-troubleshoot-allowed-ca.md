---
title: Azure CDN 'de özel HTTPS 'yi etkinleştirmek için izin verilen CA
description: Özel bir etki alanında HTTPS 'yi etkinleştirmek için kendi sertifikanızı kullanıyorsanız, bu sertifikayı oluşturmak için izin verilen bir sertifika yetkilisini (CA) kullanmanız gerekir.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 7b71611d43bc2d4de4c3e609462906c44fba0443
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77919983"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Azure CDN 'de özel HTTPS 'yi etkinleştirmek için izin verilen sertifika yetkilileri

Azure Content Delivery Network (CDN) özel etki alanı için [kendi sertifikanızı kullanarak https özelliğini etkinleştirdiğinizde](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates) belirli sertifika gereksinimlerini karşılamanız gerekir. **Microsoft profilinden Azure CDN Standard** , aşağıdaki listede onaylanan sertifika YETKILILERINDEN (CA) birinden bir sertifika gerektirir. Onaylanmamış bir CA 'dan bir sertifika veya otomatik olarak imzalanan bir sertifika kullanılıyorsa, istek reddedilir. Verizon profillerden Verizon ve **Azure CDN Premium** **'dan Azure CDN Standart** , geçerli bir CA 'dan geçerli herhangi bir sertifikayı kabul eder.

> [!NOTE]
> Özel etki alanı HTTPS özelliğini etkinleştirmek için kendi sertifikanızı kullanma seçeneği, **Akamai profillerinden Azure CDN Standart** *için kullanılamaz.* 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

