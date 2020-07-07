---
title: Azure API Management katmanları için özellik tabanlı karşılaştırma | Microsoft Docs
description: Bu makalede, sunduğu özelliklere göre API Management katmanları karşılaştırılır.
services: api-management
documentationcenter: ''
author: vladvino
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/01/2019
ms.author: apimpm
ms.openlocfilehash: c0cabab6f867df918c4ac86d035918c483e12da9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82203177"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Azure API Management katmanlarının Özellik tabanlı karşılaştırması

Her API Management [fiyatlandırma katmanı](https://aka.ms/apimpricing) , ayrı bir özellik kümesi ve birim [kapasitesi](api-management-capacity.md)başına sunulur. Aşağıdaki tabloda, katmanların her birinde kullanılabilen temel özellikler özetlenmektedir. Bazı özellikler farklı çalışabilir veya katmana bağlı olarak farklı yeteneklere sahip olabilir. Bu gibi durumlarda, bu ayrı özellikleri açıklayan belge makalelerinde farklılıklar çağrılır.

> [!IMPORTANT]
> Geliştirici katmanının üretim dışı kullanım örnekleri ve değerlendirmeleri için olduğunu lütfen unutmayın. SLA sunmaz.

| Özellik                                                                                      | Tüketim | Geliştirici | Temel | Standart | Premium |
| -------------------------------------------------------------------------------------------- | ----------- | --------- | ----- | -------- | ------- |
| Azure AD tümleştirmesi<sup>1</sup>                                                             | No          | Evet       | No    | Yes      | Yes     |
| Sanal ağ (VNet) desteği                                                               | No          | Evet       | Hayır    | Hayır       | Evet     |
| Çok bölgeli dağıtım                                                                      | Hayır          | Hayır        | Hayır    | Hayır       | Evet     |
| Birden çok özel etki alanı adı                                                                 | Hayır          | Hayır        | Hayır    | Hayır       | Evet     |
| Geliştirici portalı<sup>2</sup>                                                                 | No          | Yes       | Yes   | Yes      | Yes     |
| Yerleşik önbellek                                                                               | No          | Yes       | Yes   | Yes      | Yes     |
| Yerleşik analiz                                                                           | No          | Yes       | Yes   | Yes      | Yes     |
| [Şirket içinde barındırılan ağ geçidi](self-hosted-gateway-overview.md)<sup>3</sup>                           | No          | Evet       | Hayır    | Hayır       | Evet     |
| [TLS ayarları](api-management-howto-manage-protocols-ciphers.md)                             | Yes         | Yes       | Yes   | Yes      | Yes     |
| [Dış önbellek](https://aka.ms/apimbyoc)                                                    | Yes         | Yes       | Yes   | Yes      | Yes     |
| [İstemci sertifikası kimlik doğrulaması](api-management-howto-mutual-certificates-for-clients.md) | Yes         | Yes       | Yes   | Yes      | Yes     |
| [Yedekleme ve Geri Yükleme](api-management-howto-disaster-recovery-backup-restore.md)               | No          | Yes       | Yes   | Yes      | Yes     |
| [Git üzerinden yönetim](api-management-configuration-repository-git.md)                        | No          | Yes       | Yes   | Yes      | Yes     |
| Doğrudan yönetim API 'SI                                                                        | No          | Yes       | Yes   | Yes      | Yes     |
| Azure Izleyici günlükleri ve ölçümleri                                                               | Hayır          | Evet       | Yes   | Yes      | Yes     |
| Statik IP                                                                                    | Hayır          | Evet       | Yes   | Yes      | Yes     |

<sup>1</sup> Azure AD 'nin (ve Azure AD B2C) kullanımını, Geliştirici Portalında Kullanıcı oturum açması için bir kimlik sağlayıcısı olarak sunar.<br/>
<sup>2</sup> Kullanıcı, gruplar, sorunlar, uygulamalar ve e-posta şablonları ve bildirimler gibi ilgili işlevler dahil.<br/>
<sup>3</sup> geliştirici katmanında barındırılan ağ geçitlerinde tek ağ geçidi düğümüyle sınırlı.<br/>
