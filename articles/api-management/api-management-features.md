---
title: Azure API Management katmanları için özellik tabanlı karşılaştırma | Microsoft Docs
description: API Management katmanlarını, sundukları özelliklere göre karşılaştırın. Her fiyatlandırma katmanında kullanılabilen temel özellikleri özetleyen bir tablo görüntüleyin.
services: api-management
documentationcenter: ''
author: vladvino
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/15/2020
ms.author: apimpm
ms.openlocfilehash: 262d12e1b0ff74570b44b7794f8b55619a9afa97
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87905151"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Azure API Management katmanlarının Özellik tabanlı karşılaştırması

Her API Management [fiyatlandırma katmanı](https://aka.ms/apimpricing) , ayrı bir özellik kümesi ve birim [kapasitesi](api-management-capacity.md)başına sunulur. Aşağıdaki tabloda, katmanların her birinde kullanılabilen temel özellikler özetlenmektedir. Bazı özellikler farklı çalışabilir veya katmana bağlı olarak farklı yeteneklere sahip olabilir. Bu gibi durumlarda, bu ayrı özellikleri açıklayan belge makalelerinde farklılıklar çağrılır.

> [!IMPORTANT]
> Geliştirici katmanının üretim dışı kullanım örnekleri ve değerlendirmeleri için olduğunu lütfen unutmayın. SLA sunmaz.

| Özellik                                                                                      | Tüketim | Geliştirici | Temel | Standart | Premium |
| -------------------------------------------------------------------------------------------- | ----------- | --------- | ----- | -------- | ------- |
| Azure AD tümleştirmesi<sup>1</sup>                                                             | Hayır          | Evet       | Hayır    | Evet      | Evet     |
| Sanal ağ (VNet) desteği                                                               | Hayır          | Evet       | Hayır    | Hayır       | Evet     |
| Çok bölgeli dağıtım                                                                      | Hayır          | Hayır        | Hayır    | Hayır       | Evet     |
| Birden çok özel etki alanı adı                                                                 | Hayır          | Evet        | Hayır    | Hayır       | Evet     |
| Geliştirici portalı<sup>2</sup>                                                                 | Hayır          | Evet       | Evet   | Evet      | Evet     |
| Yerleşik önbellek                                                                               | Hayır          | Evet       | Evet   | Evet      | Evet     |
| Yerleşik analiz                                                                           | Hayır          | Evet       | Evet   | Evet      | Evet     |
| [Şirket içinde barındırılan ağ geçidi](self-hosted-gateway-overview.md)<sup>3</sup>                           | Hayır          | Evet       | Hayır    | Hayır       | Evet     |
| [TLS ayarları](api-management-howto-manage-protocols-ciphers.md)                             | Evet         | Evet       | Evet   | Evet      | Evet     |
| [Dış önbellek](https://aka.ms/apimbyoc)                                                    | Evet         | Evet       | Evet   | Evet      | Evet     |
| [İstemci sertifikası kimlik doğrulaması](api-management-howto-mutual-certificates-for-clients.md) | Evet         | Evet       | Evet   | Evet      | Evet     |
| [Yedekleme ve Geri Yükleme](api-management-howto-disaster-recovery-backup-restore.md)               | Hayır          | Evet       | Evet   | Evet      | Evet     |
| [Git üzerinden yönetim](api-management-configuration-repository-git.md)                        | Hayır          | Evet       | Evet   | Evet      | Evet     |
| Doğrudan yönetim API 'SI                                                                        | Hayır          | Evet       | Evet   | Evet      | Evet     |
| Azure Izleyici günlükleri ve ölçümleri                                                               | Evet         | Evet       | Evet   | Evet      | Evet     |
| Statik IP                                                                                    | Hayır          | Evet       | Evet   | Evet      | Evet     |

<sup>1</sup> Azure AD 'nin (ve Azure AD B2C) kullanımını, Geliştirici Portalında Kullanıcı oturum açması için bir kimlik sağlayıcısı olarak sunar.<br/>
<sup>2</sup> Kullanıcı, gruplar, sorunlar, uygulamalar ve e-posta şablonları ve bildirimler gibi ilgili işlevler dahil.<br/>
<sup>3</sup> geliştirici katmanında barındırılan ağ geçitlerinde tek ağ geçidi düğümüyle sınırlı.<br/>
