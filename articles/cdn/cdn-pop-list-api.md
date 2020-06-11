---
title: Azure CDN için geçerli POP IP listesini al | Microsoft Docs
description: Geçerli POP listesini almayı öğrenin.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2019
ms.author: allensu
ms.custom: ''
ms.openlocfilehash: 7b98bb446fc400007e4061d09db8084960702943
ms.sourcegitcommit: eeba08c8eaa1d724635dcf3a5e931993c848c633
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84668296"
---
# <a name="retrieve-the-current-pop-ip-list-for-azure-cdn"></a>Azure CDN için geçerli POP IP listesini al

## <a name="retrieve-the-current-verizon-pop-ip-list-for-azure-cdn"></a>Azure CDN için geçerli Verizon POP IP listesini al

Verizon 'in varlık noktası (POP) sunucuları için IP kümesini almak üzere REST API kullanabilirsiniz. Bu POP sunucuları, bir Verizon profilindeki Azure Content Delivery Network (CDN) uç noktaları ile ilişkili ( **Verizon 'Den**Verizon veya Azure CDN Premium**Azure CDN Standart** ) istekleri kaynak olarak sağlar. Bu IP kümesinin, bir istemcinin pop 'Lara istek yaparken göreceği IP 'lerden farklı olduğunu unutmayın. 

POP listesini almak için REST API işleminin sözdizimi için bkz. [Edge Nodes-List](https://docs.microsoft.com/rest/api/cdn/edgenodes/list).

## <a name="retrieve-the-current-microsoft-pop-ip-list-for-azure-cdn"></a>Azure CDN için geçerli Microsoft POP IP listesini al

Uygulamanızı yalnızca Microsoft 'un Azure CDN trafiği kabul edecek şekilde kilitlemek için, arka ucunuz için IP ACL 'Lerini ayarlamanız gerekir. Ayrıca, Microsoft 'tan Azure CDN tarafından gönderilen ' X-Iletilen-Host ' üst bilgisi için kabul edilebilir değerler kümesini kısıtlayabilirsiniz. Bu adımlar aşağıdaki şekilde ayrıntılı olarak verilmiştir:

Microsoft 'un arka uç IP adresi alanından ve Azure 'un altyapı hizmetlerinden gelen Azure CDN trafiği kabul etmek için backenler için IP adresini yapılandırın. 

* Microsoft 'un IPv4 arka uç IP alanından Azure CDN: 147.243.0.0/16
* Microsoft 'un IPv6 arka uç IP alanından Azure CDN: 2a01:111:2050::/44

Hizmet etiketlerini Microsoft 'tan Azure CDN kullanmak için lütfen Azure ön kapısı etiketini kullanın. Microsoft Hizmetleri için IP aralıkları ve hizmet etiketleri [burada](https://www.microsoft.com/download/details.aspx?id=56519) bulunabilir


## <a name="typical-use-case"></a>Tipik kullanım örneği

Güvenlik nedeniyle bu IP listesini, kaynak sunucunuza yönelik isteklerin yalnızca geçerli bir Verizon POP 'tan yapılmasını zorlamak için kullanabilirsiniz. Örneğin, bir CDN uç noktasının kaynak sunucusu için ana bilgisayar adı veya IP adresi tespit edildiğinde, birisi istekleri doğrudan kaynak sunucuya yapabilir, bu nedenle Azure CDN tarafından belirtilen ölçekleme ve güvenlik yeteneklerini atlayarak. Döndürülen listedeki IP 'Leri, bir kaynak sunucuda yalnızca izin verilen IP 'Ler olarak ayarlayarak bu senaryo engellenebilir. En son POP listesine sahip olduğunuzdan emin olmak için, günde en az bir kez alın. 

## <a name="next-steps"></a>Sonraki adımlar

REST API hakkında daha fazla bilgi için bkz. [Azure CDN REST API](https://docs.microsoft.com/rest/api/cdn/).
