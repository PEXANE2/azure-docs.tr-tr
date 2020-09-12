---
title: Azure ön kapıda özel HTTPS 'yi etkinleştirmek için izin verilen CA
description: Özel bir etki alanında HTTPS 'yi etkinleştirmek için kendi sertifikanızı kullanıyorsanız, bu sertifikayı oluşturmak için izin verilen bir sertifika yetkilisini (CA) kullanmanız gerekir.
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
ms.openlocfilehash: 7bdef37561687b49b030d8237472c0d35f945c13
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89399132"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door"></a>Azure ön kapıda özel HTTPS 'yi etkinleştirmek için izin verilen sertifika yetkilileri

Azure ön kapısının özel etki alanı için [kendi sertifikanızı kullanarak https özelliğini etkinleştirdiğinizde](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), TLS/SSL sertifikanızı oluşturmak için izin verilen bir sertifika YETKILISI (CA) kullanmanız gerekir. Aksi takdirde, izin verilmeyen bir CA veya otomatik olarak imzalanan bir sertifika kullanıyorsanız isteğiniz reddedilir.

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]
