---
title: Azure API Management katmanları için özellik tabanlı karşılaştırma | Microsoft Docs
description: Bu makalede, sunduğu özelliklere göre API Management katmanları karşılaştırılır.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.author: apimpm
ms.openlocfilehash: bc5eab6bb5044e474c386d3e5771d8517e6ba9cf
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70072525"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Azure API Management katmanlarının Özellik tabanlı karşılaştırması

>[!IMPORTANT]
> Geliştirici katmanının üretim dışı kullanım örnekleri ve değerlendirmeleri için olduğunu lütfen unutmayın. SLA sunmaz. 

Her API Management [fiyatlandırma katmanı](https://aka.ms/apimpricing) , ayrı bir özellik kümesi ve birim [kapasitesi](api-management-capacity.md)başına sunulur. Aşağıdaki tabloda, katmanların her birinde kullanılabilen temel özellikler özetlenmektedir. Bazı özellikler farklı çalışabilir veya katmana bağlı olarak farklı yeteneklere sahip olabilir. Bu gibi durumlarda, bu ayrı özellikleri açıklayan belge makalelerinde farklılıklar çağrılır.

| Özellik                                                                                      | Tüketim | Geliştirici      | Temel          | Standart       | Premium        |
| -------------------------------------------------------------------------------------------- | ----------------------------- | -------------- | -------------- | -------------- | -------------- |
| Azure AD tümleştirmesi<sup>1</sup>                                                             | Hayır                            | Evet            | Hayır             | Evet            | Evet            |
| Sanal ağ (VNet) desteği                                                               | Hayır                            | Evet            | Hayır             | Hayır             | Evet            |
| Çok bölgeli dağıtım                                                                      | Hayır                            | Hayır             | Hayır             | Hayır             | Evet            |
| Birden çok özel etki alanı adı                                                                 | Hayır                            | Hayır             | Hayır             | Hayır             | Evet            |
| Geliştirici portalı<sup>2</sup>                                                                 | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| Yerleşik önbellek                                                                               | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| Yerleşik analitik                                                                           | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| [SSL ayarları](api-management-howto-manage-protocols-ciphers.md)                             | Evet                            | Evet            | Evet            | Evet            | Evet            |
| [Dış önbellek](https://aka.ms/apimbyoc)                                                    | Evet                           | Evet            | Evet            | Evet            | Evet            |
| [İstemci sertifikası kimlik doğrulaması](api-management-howto-mutual-certificates-for-clients.md) | Evet                | Evet            | Evet            | Evet            | Evet            |
| [Yedekleme ve geri yükleme](api-management-howto-disaster-recovery-backup-restore.md)               | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| [Git üzerinden yönetim](api-management-configuration-repository-git.md)                        | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| Doğrudan yönetim API 'SI                                                                        | Hayır                            | Evet            | Evet            | Evet            | Evet            |
| Azure Izleyici günlükleri ve ölçümleri                                                               | Hayır                | Evet            | Evet            | Evet            | Evet            |
| Statik IP                                                               | Hayır                | Evet            | Evet            | Evet            | Evet            |

<sup>1</sup> Azure AD 'nin (ve Azure AD B2C) kullanımını, Geliştirici Portalında Kullanıcı oturum açması için bir kimlik sağlayıcısı olarak sunar.<br/>
<sup>2</sup> Kullanıcı, gruplar, sorunlar, uygulamalar ve e-posta şablonları ve bildirimler gibi ilgili işlevler dahil.<br/>
