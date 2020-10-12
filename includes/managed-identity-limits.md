---
title: dosya dahil etme
description: dosya dahil etme
services: active-directory
author: daveba
ms.service: active-directory
ms.subservice: msi
ms.topic: include
ms.date: 05/31/2018
ms.author: daveba
ms.custom: include file
ms.openlocfilehash: dc7849e52b3e00c87d271ee8aef8788d816dff7a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90971027"
---
- Her yönetilen kimlik, [Azure ad hizmeti sınırları ve kısıtlamaları](../articles/active-directory/users-groups-roles/directory-service-limits-restrictions.md)bölümünde açıklandığı gibi BIR Azure AD kiracısındaki nesne kotası sınırına doğru sayılır.
-   Yönetilen kimliklerin oluşturulabilen hız Aşağıdaki sınırlara sahiptir:

    1. Azure bölgesi başına Azure AD kiracısı: 20 saniye başına 200 oluşturma işlemi.
    2. Azure bölgesi başına Azure aboneliği başına: 20 saniye başına 40 oluşturma işlemi.

- Kullanıcı tarafından atanan Yönetilen kimlikler oluşturduğunuzda yalnızca alfasayısal karakterler (0-9, a-z ve A-Z) ve kısa çizgi (-) desteklenir. Bir sanal makineye veya sanal makine ölçek kümesinin düzgün şekilde çalışması için, ad 24 karakterle sınırlıdır.
