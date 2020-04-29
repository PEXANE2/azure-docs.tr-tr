---
title: Azure ön kapıda özel HTTPS 'yi etkinleştirmek için izin verilen CA
description: Özel bir etki alanında HTTPS 'yi etkinleştirmek için kendi sertifikanızı kullanıyorsanız, bu sertifikayı oluşturmak için izin verilen bir sertifika yetkilisini (CA) kullanmanız gerekir.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.assetid: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2018
ms.author: sharadag
ms.openlocfilehash: 611f5730afed4c3a84b81d6acfd33b633c532bbc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80874679"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door"></a>Azure ön kapıda özel HTTPS 'yi etkinleştirmek için izin verilen sertifika yetkilileri

Azure ön kapısının özel etki alanı için [kendi sertifikanızı kullanarak https özelliğini etkinleştirdiğinizde](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), TLS/SSL sertifikanızı oluşturmak için izin verilen bir sertifika YETKILISI (CA) kullanmanız gerekir. Aksi takdirde, izin verilmeyen bir CA veya otomatik olarak imzalanan bir sertifika kullanıyorsanız isteğiniz reddedilir.

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]
