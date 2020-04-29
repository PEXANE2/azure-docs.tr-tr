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
ms.topic: article
ms.date: 10/18/2018
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: faf51dbb1f1c3c0346b1ae9104494538efcc2ee7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81259985"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Azure CDN 'de özel HTTPS 'yi etkinleştirmek için izin verilen sertifika yetkilileri

Azure Content Delivery Network (CDN) özel etki alanı için [kendi sertifikanızı kullanarak https özelliğini etkinleştirdiğinizde](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#tlsssl-certificates) belirli sertifika gereksinimlerini karşılamanız gerekir. **Microsoft profilinden Azure CDN Standard** , aşağıdaki listede onaylanan sertifika YETKILILERINDEN (CA) birinden bir sertifika gerektirir. Onaylanmamış bir CA 'dan bir sertifika veya otomatik olarak imzalanan bir sertifika kullanılıyorsa, istek reddedilir. Verizon profillerden Verizon ve **Azure CDN Premium** **'dan Azure CDN Standart** , geçerli bir CA 'dan geçerli herhangi bir sertifikayı kabul eder.

> [!NOTE]
> Özel etki alanı HTTPS özelliğini etkinleştirmek için kendi sertifikanızı kullanma seçeneği, **Akamai profillerinden Azure CDN Standart** *için kullanılamaz.* 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

