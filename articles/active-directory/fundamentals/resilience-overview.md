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
ms.openlocfilehash: 7dfd51b0ed43badbc6a4882f619cb718952b0e85
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95919877"
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

* [CıHAR sistemlerinizde derleme esnekliği](resilience-b2c.md)
