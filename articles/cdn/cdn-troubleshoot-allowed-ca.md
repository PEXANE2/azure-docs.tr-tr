---
title: Azure CDN'de özel HTTPS etkinleştirmek için CA'ya izin verildi
description: ÖZEL bir etki alanında HTTPS'yi etkinleştirmek için kendi sertifikanızı kullanıyorsanız, bunu oluşturmak için izin verilen bir sertifika yetkilisini (CA) kullanmanız gerekir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919983"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Azure CDN'de özel HTTPS etkinleştirme için sertifika yetkililerine izin

Azure İçerik Dağıtım Ağı (CDN) özel etki alanı için [kendi sertifikanızı kullanarak HTTPS özelliğini etkinleştirdiğinizde](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates) belirli sertifika gereksinimlerini karşılamanız gerekir. **Microsoft profilinden gelen Azure CDN Standardı,** aşağıdaki listedeki onaylı sertifika yetkililerinden (CA) bir sertifika gerektirir. Onaylanmamış bir CA'dan bir sertifika veya kendi imzalı bir sertifika kullanılıyorsa, istek reddedilir. **Verizon'dan Azure CDN Standardı** ve **Verizon profillerinden Azure CDN Premium,** geçerli CA'lardan geçerli sertifikaları kabul eder.

> [!NOTE]
> Özel etki alanı HTTPS özelliğini etkinleştirmek için kendi sertifikanızı kullanma **seçeneği, Akamai profillerinden Azure CDN Standardı** için *kullanılamaz.* 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

