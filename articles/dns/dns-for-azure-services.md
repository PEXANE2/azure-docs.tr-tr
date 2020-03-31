---
title: Azure DNS'yi diğer Azure hizmetleriyle kullanma
description: Bu öğrenme yolunda, diğer Azure hizmetlerinin adlarını çözmek için Azure DNS'nin nasıl kullanılacağı nasıI kullanılacağı nasıI başlayın
services: dns
documentationcenter: na
author: rohinkoul
manager: kumudD
tags: azure dns
ms.assetid: e9b5eb94-7984-4640-9930-564bb9e82b78
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: rohink
ms.openlocfilehash: fa2c1ced6405c967ca33562d6215b304b8507e5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76937247"
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Azure DNS diğer Azure hizmetleriyle nasıl çalışır?

Azure DNS, barındırılan bir DNS yönetimi ve ad çözümleme hizmetidir. Azure'da dağıttığınız diğer uygulamalar ve hizmetler için ortak DNS adları oluşturmak için kullanabilirsiniz. Özel etki alanınızda bir Azure hizmeti için ad oluşturmak kolaydır. Hizmetiniz için doğru türe ait bir kayıt eklemeniz.

* Dinamik olarak ayrılan IP adresleri için, Azure'un hizmetiniz için oluşturduğu DNS adı ile eşlenen bir DNS CNAME kaydı oluşturabilirsiniz. DNS standartları, bölge tepe leri için CNAME kaydı nı kullanmanızı engeller. Bunun yerine bir takma ad kaydı kullanabilirsiniz. Daha fazla bilgi için [Bkz. Öğretici: Azure Genel IP adresine başvurmak için takma ad kaydını yapılandırın.](tutorial-alias-pip.md)
* Statik olarak ayrılan IP adresleri için, bölge tepesinde çıplak bir *etki alanı* adı içeren herhangi bir ad kullanarak bir DNS A kaydı oluşturabilirsiniz.

Aşağıdaki tablo, çeşitli Azure hizmetleri için kullanabileceğiniz desteklenen kayıt türlerini özetleyin. Tabloda görüldüğü gibi, Azure DNS Yalnızca Internet'e bakan ağ kaynakları için DNS kayıtlarını destekler. Azure DNS, dahili, özel adreslerin ad çözümlenmesi için kullanılamaz.

| Azure hizmeti | Ağ arabirimi | Açıklama |
| --- | --- | --- |
| Azure Application Gateway |[Ön uç genel IP](dns-custom-domain.md#public-ip-address) |Bir DNS A veya CNAME kaydı oluşturabilirsiniz. |
| Azure Load Balancer |[Ön uç genel IP](dns-custom-domain.md#public-ip-address) |Bir DNS A veya CNAME kaydı oluşturabilirsiniz. Yük Dengeleyicidinamik olarak atanmış bir IPv6 genel IP adresine sahip olabilir. IPv6 adresi için CNAME kaydı oluşturun. |
| Azure Traffic Manager |Genel ad |Trafik Yöneticisi profilinize atanan trafficmanager.net adı eşleyen bir takma ad kaydı oluşturabilirsiniz. Daha fazla bilgi için [Bkz. Öğretici: Trafik Yöneticisi ile apeks alan adlarını desteklemek için bir takma ad kaydı yapılandırın.](tutorial-alias-tm.md) |
| Azure Cloud Services |[Genel IP](dns-custom-domain.md#public-ip-address) |Statik olarak ayrılan IP adresleri için bir DNS A kaydı oluşturabilirsiniz. Dinamik olarak ayrılan IP adresleri için, *cloudapp.net* adına eşlenen bir CNAME kaydı oluşturmanız gerekir.|
| Azure App Service | [Harici IP](dns-custom-domain.md#app-service-web-apps) |Harici IP adresleri için bir DNS A kaydı oluşturabilirsiniz. Aksi takdirde, azurewebsites.net adına eşleyen bir CNAME kaydı oluşturmanız gerekir. Daha fazla bilgi için, [bir Azure uygulamasına özel bir etki alanı adını](../app-service/app-service-web-tutorial-custom-domain.md)Haritala'ya bakın. |
| Azure Kaynak Yöneticisi VM'leri |[Genel IP](dns-custom-domain.md#public-ip-address) |Kaynak Yöneticisi VM'lerin ortak IP adresleri olabilir. Ortak IP adresine sahip bir VM, yük dengeleyicinin arkasında da olabilir. Ortak adres için bir DNS A, CNAME veya diğer ad kaydı oluşturabilirsiniz. Bu özel adı, yük bakiyesindeki VIP'yi atlamak için kullanabilirsiniz. |
| Klasik VM'ler |[Genel IP](dns-custom-domain.md#public-ip-address) |PowerShell veya CLI kullanılarak oluşturulan klasik VM'ler dinamik veya statik (ayrılmış) sanal bir adresle yapılandırılabilir. Sırasıyla bir DNS CNAME veya A kaydı oluşturabilirsiniz. |
