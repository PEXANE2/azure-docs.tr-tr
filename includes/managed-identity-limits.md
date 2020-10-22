---
title: include dosyası
description: include dosyası
services: active-directory
author: daveba
ms.service: active-directory
ms.subservice: msi
ms.topic: include
ms.date: 05/31/2018
ms.author: daveba
ms.custom: include file
ms.openlocfilehash: 6e5885e076222cd23ba127f3be41c1218f327ca0
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92379722"
---
- Her yönetilen kimlik, [Azure ad hizmeti sınırları ve kısıtlamaları](../articles/active-directory/enterprise-users/directory-service-limits-restrictions.md)bölümünde açıklandığı gibi BIR Azure AD kiracısındaki nesne kotası sınırına doğru sayılır.
-   Yönetilen kimliklerin oluşturulabilen hız Aşağıdaki sınırlara sahiptir:

    1. Azure bölgesi başına Azure AD kiracısı: 20 saniye başına 200 oluşturma işlemi.
    2. Azure bölgesi başına Azure aboneliği başına: 20 saniye başına 40 oluşturma işlemi.

- Kullanıcı tarafından atanan Yönetilen kimlikler oluşturduğunuzda yalnızca alfasayısal karakterler (0-9, a-z ve A-Z) ve kısa çizgi (-) desteklenir. Bir sanal makineye veya sanal makine ölçek kümesinin düzgün şekilde çalışması için, ad 24 karakterle sınırlıdır.
