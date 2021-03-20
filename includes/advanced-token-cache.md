---
title: include dosyası
description: include dosyası
services: active-directory
author: kalyankrishna1
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.date: 11/05/2020
ms.author: kkrishna
ms.openlocfilehash: 174946667885debc348370ef2c6f93206890e9c1
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94628016"
---
Arka plan uygulamalarının, API 'Lerin ve hizmetlerin kendi devamsızlık aşamasında Kullanıcı adına işlem yapmaya devam etmek için erişim belirteci önbelleğini kullanmasına izin vermek üzere MSAL 'ın belirteç önbelleği uygulamasını kullanabilirsiniz. Bu durum, Kullanıcı ön uç Web uygulamasından çıktıktan sonra, arka plan uygulamalarının ve hizmetlerinin Kullanıcı adına çalışmaya devam etmesi gerekiyorsa yararlıdır.

Günümüzde, çoğu arka plan işlemi, kimlik doğrulaması yapmak veya yeniden kimlik doğrulamak için mevcut kalmadan bir kullanıcının verileriyle çalışması gerektiğinde [uygulama izinlerini](/graph/auth/auth-concepts#microsoft-graph-permissions) kullanır. Uygulama izinleri genellikle, ayrıcalık yükseltmesi gerektiren yönetici onayı gerektirdiğinden, geliştirici, kullanıcının uygulama için ilk olarak kendisi için kabul ettiği izni elde etmek istemediği için gereksiz bir duymayla karşılaşıldı.

GitHub 'daki Bu kod örneği, arka plan uygulamalarından MSAL 'un belirteç önbelleğine erişerek bu gereksiz bir şekilde nasıl kaçınmayı gösterir:

 [Arka plan uygulamaları, API 'Leri ve hizmetlerinden oturum açmış kullanıcının belirteç önbelleğine erişme](https://github.com/Azure-Samples/ms-identity-dotnet-advanced-token-cache)