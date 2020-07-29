---
title: Diğer Azure hizmetleriyle Azure DNS kullanma
description: Bu öğrenme yolunda, diğer Azure hizmetleri için adları çözümlemek üzere Azure DNS nasıl kullanacağınızı kullanmaya başlayın
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76937247"
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Azure DNS diğer Azure hizmetleriyle nasıl kullanılır

Azure DNS barındırılan bir DNS yönetimi ve ad çözümleme hizmetidir. Bu hizmeti, Azure 'da dağıttığınız diğer uygulamalar ve hizmetler için ortak DNS adları oluşturmak üzere kullanabilirsiniz. Özel etki alanında bir Azure hizmeti için ad oluşturma basittir. Hizmetiniz için doğru türde bir kayıt eklemeniz yeterlidir.

* Dinamik olarak ayrılan IP adresleri için, hizmetiniz için Azure tarafından oluşturulan DNS adıyla eşleşen bir DNS CNAME kaydı oluşturabilirsiniz. DNS standartları, tepesinde bölgesi için bir CNAME kaydı kullanmanızı önler. Bunun yerine bir diğer ad kaydı kullanabilirsiniz. Daha fazla bilgi için bkz. [öğretici: Azure genel IP adresine başvurmak için bir diğer ad kaydı yapılandırma](tutorial-alias-pip.md).
* Statik olarak ayrılan IP adresleri için, tepesinde bölgesinde bir *çıplak etki alanı* adı içeren herhangi bir ad kullanarak bir DNS a kaydı oluşturabilirsiniz.

Aşağıdaki tabloda, çeşitli Azure hizmetleri için kullanabileceğiniz desteklenen kayıt türleri özetlenmektedir. Tabloda gösterildiği gibi, Azure DNS Internet 'e yönelik ağ kaynakları için yalnızca DNS kayıtlarını destekler. Azure DNS iç, özel adreslerin ad çözümlemesi için kullanılamaz.

| Azure hizmeti | Ağ arabirimi | Açıklama |
| --- | --- | --- |
| Azure Application Gateway |[Ön uç genel IP 'si](dns-custom-domain.md#public-ip-address) |DNS A veya CNAME kaydı oluşturabilirsiniz. |
| Azure Load Balancer |[Ön uç genel IP 'si](dns-custom-domain.md#public-ip-address) |DNS A veya CNAME kaydı oluşturabilirsiniz. Load Balancer, dinamik olarak atanan IPv6 genel IP adresine sahip olabilir. Bir IPv6 adresi için CNAME kaydı oluşturun. |
| Azure Traffic Manager |Ortak ad |Traffic Manager profilinize atanan trafficmanager.net adıyla eşleşen bir diğer ad kaydı oluşturabilirsiniz. Daha fazla bilgi için bkz. [öğretici: Traffic Manager ile tepesinde etki alanı adlarını desteklemek için bir diğer ad kaydı yapılandırma](tutorial-alias-tm.md). |
| Azure Cloud Services |[Genel IP](dns-custom-domain.md#public-ip-address) |Statik olarak ayrılan IP adresleri için DNS A kaydı oluşturabilirsiniz. Dinamik olarak ayrılan IP adresleri için *cloudapp.net* adıyla eşleşen bir CNAME kaydı oluşturmanız gerekir.|
| Azure App Service | [Dış IP](dns-custom-domain.md#app-service-web-apps) |Dış IP adresleri için DNS A kaydı oluşturabilirsiniz. Aksi takdirde, azurewebsites.net adıyla eşleyen bir CNAME kaydı oluşturmanız gerekir. Daha fazla bilgi için bkz. [özel etki alanı adını bir Azure uygulamasına eşleme](../app-service/app-service-web-tutorial-custom-domain.md). |
| Azure Resource Manager VM 'Ler |[Genel IP](dns-custom-domain.md#public-ip-address) |Kaynak Yöneticisi VM 'Ler genel IP adreslerine sahip olabilir. Ortak IP adresine sahip bir VM, yük dengeleyicinin arkasında da olabilir. Genel adres için bir DNS A, CNAME veya diğer ad kaydı oluşturabilirsiniz. Bu özel adı, yük dengeleyicide VIP 'yi atlamak için kullanabilirsiniz. |
| Klasik VM'ler |[Genel IP](dns-custom-domain.md#public-ip-address) |PowerShell veya CLı kullanılarak oluşturulan klasik VM 'Ler, dinamik veya statik (ayrılmış) bir sanal adresle yapılandırılabilir. Sırasıyla bir DNS CNAME veya bir kayıt oluşturabilirsiniz. |
