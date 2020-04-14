---
title: Azure CDN'de özel HTTPS etkinleştirmek için CA'ya izin verildi
description: ÖZEL bir etki alanında HTTPS'yi etkinleştirmek için kendi sertifikanızı kullanıyorsanız, bunu oluşturmak için izin verilen bir sertifika yetkilisini (CA) kullanmanız gerekir.
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
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81259985"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Azure CDN'de özel HTTPS etkinleştirme için sertifika yetkililerine izin

Azure İçerik Dağıtım Ağı (CDN) özel etki alanı için [kendi sertifikanızı kullanarak HTTPS özelliğini etkinleştirdiğinizde](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#tlsssl-certificates) belirli sertifika gereksinimlerini karşılamanız gerekir. **Microsoft profilinden gelen Azure CDN Standardı,** aşağıdaki listedeki onaylı sertifika yetkililerinden (CA) bir sertifika gerektirir. Onaylanmamış bir CA'dan bir sertifika veya kendi imzalı bir sertifika kullanılıyorsa, istek reddedilir. **Verizon'dan Azure CDN Standardı** ve **Verizon profillerinden Azure CDN Premium,** geçerli CA'lardan geçerli sertifikaları kabul eder.

> [!NOTE]
> Özel etki alanı HTTPS özelliğini etkinleştirmek için kendi sertifikanızı kullanma **seçeneği, Akamai profillerinden Azure CDN Standardı** için *kullanılamaz.* 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

