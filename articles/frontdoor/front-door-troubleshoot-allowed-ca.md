---
title: Azure ön kapıda özel HTTPS 'yi etkinleştirmek için izin verilen CA
description: Azure ön kapılı özel etki alanında HTTPS 'yi etkinleştirmek için kendi sertifikanızı kullanıyorsanız, bunu oluşturmak için izin verilen bir sertifika yetkilisi (CA) kullanmanız gerekir.
services: frontdoor
documentationcenter: ''
author: duongau
ms.assetid: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2018
ms.author: duau
ms.openlocfilehash: 973df2505eefc2a46aa105b874f32b61fe6e8b36
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91269819"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door"></a>Azure ön kapıda özel HTTPS 'yi etkinleştirmek için izin verilen sertifika yetkilileri

Azure ön kapısının özel etki alanı için [kendi sertifikanızı kullanarak https özelliğini etkinleştirdiğinizde](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), TLS/SSL sertifikanızı oluşturmak için izin verilen bir sertifika YETKILISI (CA) kullanmanız gerekir. Aksi takdirde, izin verilmeyen bir CA veya otomatik olarak imzalanan bir sertifika kullanıyorsanız isteğiniz reddedilir.

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]
