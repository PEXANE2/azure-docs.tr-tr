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
ms.openlocfilehash: cac6bc9895f2b8778f2b27cc6b1dff4d4b898ae7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471532"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door"></a>Azure Ön Kapı'da özel HTTPS etkinleştirme için sertifika yetkililerine izin

Azure Ön Kapı özel etki alanı için, [kendi sertifikanızı kullanarak HTTPS özelliğini etkinleştirdiğinizde,](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate)SSL sertifikanızı oluşturmak için izin verilen bir sertifika yetkilisini (CA) kullanmanız gerekir. Aksi takdirde, izin verilmeyen bir CA veya kendi imzalı sertifika kullanırsanız, isteğiniz reddedilir.

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]
