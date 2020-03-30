---
title: Azure API Yönetimi katmanlarının özellik tabanlı karşılaştırılması | Microsoft Dokümanlar
description: Bu makalede, sundukları özelliklere göre API Yönetimi katmanları karşılaştırılır.
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
ms.openlocfilehash: 2e84138419986ef1033ab076b3745187812e91b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335896"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Azure API Yönetimi katmanlarının özellik tabanlı karşılaştırılması

Her API Yönetimi [fiyatlandırma katmanı,](https://aka.ms/apimpricing) birim [başına](api-management-capacity.md)farklı bir özellik kümesi sunar. Aşağıdaki tablo, katmanların her birinde bulunan temel özellikleri özetleyin. Bazı özellikler farklı çalışabilir veya katmana bağlı olarak farklı özelliklere sahip olabilir. Bu gibi durumlarda, bu bireysel özellikleri açıklayan dokümantasyon makalelerinde farklılıklar belirtilir.

> [!IMPORTANT]
> Geliştirici katmanının üretim dışı kullanım durumları ve değerlendirmeler için olduğunu lütfen unutmayın. SLA sunmuyor.

| Özellik                                                                                      | Tüketim | Geliştirici | Temel | Standart | Premium |
| -------------------------------------------------------------------------------------------- | ----------- | --------- | ----- | -------- | ------- |
| Azure AD<sup>tümleştirmesi 1</sup>                                                             | Hayır          | Evet       | Hayır    | Evet      | Evet     |
| Sanal Ağ (VNet) desteği                                                               | Hayır          | Evet       | Hayır    | Hayır       | Evet     |
| Çok bölgeli dağıtım                                                                      | Hayır          | Hayır        | Hayır    | Hayır       | Evet     |
| Birden çok özel alan adı                                                                 | Hayır          | Hayır        | Hayır    | Hayır       | Evet     |
| Geliştirici portalı<sup>2</sup>                                                                 | Hayır          | Evet       | Evet   | Evet      | Evet     |
| Dahili önbellek                                                                               | Hayır          | Evet       | Evet   | Evet      | Evet     |
| Yerleşik analitik                                                                           | Hayır          | Evet       | Evet   | Evet      | Evet     |
| [Kendi kendine barındırılan ağ geçidi](self-hosted-gateway-overview.md)<sup>3</sup>                           | Hayır          | Evet       | Hayır    | Hayır       | Evet     |
| [TLS ayarları](api-management-howto-manage-protocols-ciphers.md)                             | Evet         | Evet       | Evet   | Evet      | Evet     |
| [Dış önbellek](https://aka.ms/apimbyoc)                                                    | Evet         | Evet       | Evet   | Evet      | Evet     |
| [İstemci sertifikası kimlik doğrulaması](api-management-howto-mutual-certificates-for-clients.md) | Evet         | Evet       | Evet   | Evet      | Evet     |
| [Yedekleme ve geri yükleme](api-management-howto-disaster-recovery-backup-restore.md)               | Hayır          | Evet       | Evet   | Evet      | Evet     |
| [Git üzerinde Yönetim](api-management-configuration-repository-git.md)                        | Hayır          | Evet       | Evet   | Evet      | Evet     |
| Doğrudan yönetim API'si                                                                        | Hayır          | Evet       | Evet   | Evet      | Evet     |
| Azure Monitör günlükleri ve ölçümleri                                                               | Hayır          | Evet       | Evet   | Evet      | Evet     |
| Statik IP                                                                                    | Hayır          | Evet       | Evet   | Evet      | Evet     |

<sup>1</sup> Geliştirici portalında kullanıcı oturum açma için bir kimlik sağlayıcısı olarak Azure AD'nin (ve Azure AD B2C) kullanımını sağlar.<br/>
<sup>2</sup> Kullanıcılar, gruplar, sorunlar, uygulamalar ve e-posta şablonları ve bildirimler gibi ilgili işlevler de dahil olmak üzere.<br/>
<sup>3</sup> Tek bir ağ geçidi düğümü ile kendi kendine barındırılan ağ geçidi dağıtımı ile sınırlıdır.<br/>
