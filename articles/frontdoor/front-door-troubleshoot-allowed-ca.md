---
title: Azure Ön Kapı'da özel HTTPS etkinleştirmek için CA'ya izin verildi
description: ÖZEL bir etki alanında HTTPS'yi etkinleştirmek için kendi sertifikanızı kullanıyorsanız, bunu oluşturmak için izin verilen bir sertifika yetkilisini (CA) kullanmanız gerekir.
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
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80874679"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door"></a>Azure Ön Kapı'da özel HTTPS etkinleştirme için sertifika yetkililerine izin

Azure Ön Kapı özel etki alanı için, [kendi sertifikanızı kullanarak HTTPS özelliğini etkinleştirdiğinizde](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate)TLS/SSL sertifikanızı oluşturmak için izin verilen bir sertifika yetkilisini (CA) kullanmanız gerekir. Aksi takdirde, izin verilmeyen bir CA veya kendi imzalı sertifika kullanırsanız, isteğiniz reddedilir.

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]
