---
title: Azure Active Directory ile dayanıklı kimlik ve erişim yönetimi oluşturma
description: Mimarlar, BT yöneticileri ve geliştiricilere, kimlik sistemlerinin kesintiye uğramasını sağlamak için esnekliği oluşturmaya yönelik bir kılavuz.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 11/30/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39021fecb79f6b8e7536daad592378ed998760a7
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96454355"
---
# <a name="building-resilience-into-identity-and-access-management-with-azure-active-directory"></a>Azure Active Directory ile kimlik ve erişim yönetimine esnekliği oluşturma

Kimlik ve erişim yönetimi (ıAM), kimliklerin ve bunlara erişimlerin yönetimini kolaylaştıran bir dizi işlem, ilke ve teknolojidir. Bu, sisteminizdeki Kullanıcı ve diğer hesapların kimlik doğrulama ve yetkilendirme işlemlerini destekleyen birçok bileşeni içerir.

IAM esnekliği, sistem bileşenlerine müdahale etme ve iş, Kullanıcı, müşteri ve işlemlerinizin en az etkiyle kurtarma olanağıdır. Bağımlılıkları, karmaşıklığı ve hata noktalarını azaltma, kapsamlı hata işleme sağlamak esnekliği uygulamanızı artırır.

Kesintiye uğramak, ıAM sistemlerinizin herhangi bir bileşeninden gelebilir. Dayanıklı bir ıAM sistemi oluşturmak için kesintiler olduğunu ve planı planlayabileceğini varsayın. 

IAM çözümünüzün esnekliği planlarken, aşağıdaki öğeleri göz önünde bulundurun: 

* IAM sisteminize güvenen uygulamalarınız.

* Kimlik doğrulamanın kullanacağı ortak altyapılar, telekomünikasyon şirketleri, Internet hizmet sağlayıcıları ve ortak anahtar sağlayıcıları da dahil olmak üzere.

* Bulut ve şirket içi kimlik sağlayıcılarınız.

* IAM ' i ve bunları bağlayan API 'Leri kullanan diğer hizmetler.

* Sisteminizdeki diğer şirket içi bileşenler.

Her ne kadar kaynak, algılama ve planlamayla ilgili önemli olan durumlar önemlidir. Bununla birlikte, ek kimlik sistemleri ve bunların sonuç bağımlılıklarını ve karmaşıklığını eklemek, esnekliği uygulamanızı artırmak yerine azaltabilir.

Sistemlerinizde daha fazla esnekliği oluşturmak için aşağıdaki makaleleri gözden geçirin:

* [IAM altyapınızda esnekliği derleme](resilience-in-infrastructure.md)

* [Uygulamalarınızda ıAM esnekliği oluşturma](resilience-app-development-overview.md)

* [Müşteri kimlik ve erişim yönetimi (CıHAR) sistemlerinizde esnekliği oluşturma](resilience-b2c.md)
