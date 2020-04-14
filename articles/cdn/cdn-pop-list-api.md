---
title: Retrieve the current POP IP list for Azure CDN| Microsoft Dokümanlar
description: Geçerli POP listesini nasıl alınabildiğini öğrenin.
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
ms.openlocfilehash: a58fd7c6f50cd46ac3c34cd7e5bd329c0007e5f6
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260199"
---
# <a name="retrieve-the-current-pop-ip-list-for-azure-cdn"></a>Azure CDN için geçerli POP IP listesini alma

## <a name="retrieve-the-current-verizon-pop-ip-list-for-azure-cdn"></a>Azure CDN için geçerli Verizon POP IP listesini alın

Verizon'un varlık noktası (POP) sunucuları için IP kümesini almak için REST API'sini kullanabilirsiniz. Bu POP sunucuları, Verizon profilinde Azure İçerik Dağıtım Ağı (CDN) uç noktalarıyla ilişkili başlangıç sunucularına istekte bulunur **(Verizon'dan Azure CDN Standardı** veya **Verizon'dan Azure CDN Premium).** Bu IP kümesinin, bir istemcinin POP'lara istekte bulunmada göreceği IP'lerden farklı olduğunu unutmayın. 

POP listesini almak için REST API işleminin sözdizimi için Edge [Düğümleri - Liste'ye](https://docs.microsoft.com/rest/api/cdn/edgenodes/list)bakın.

## <a name="retrieve-the-current-microsoft-pop-ip-list-for-azure-cdn"></a>Azure CDN için geçerli Microsoft POP IP listesini alma

Yalnızca Microsoft'tan gelen Azure CDN trafiğini kabul etmek için uygulamanızı kilitlemek için arka uçiçin IP ALA'larını ayarlamanız gerekir. Azure CDN tarafından Microsoft'tan gönderilen 'X-Forwarded-Host' üstbilgisi için kabul edilen değerler kümesini de kısıtlayabilirsiniz. Bu adımlar aşağıdaki gibi ayrıntılı olarak açıklanmıştır:

Microsoft'un arka uç IP adres alanından ve Azure'un altyapı hizmetlerinden Azure CDN trafiğini kabul etmek için arka uçlarınız için IP YAPıŞTıRmayı yapılandırın. 

* Microsoft'un IPv4 arka uç IP alanından Azure CDN: 147.243.0.0/16
* Microsoft'un IPv6 arka uç IP alanından Azure CDN: 2a01:111:2050::/44

Microsoft hizmetleri için IP Aralıkları ve Hizmet etiketlerini [burada](https://www.microsoft.com/download/details.aspx?id=56519) bulabilirsiniz


## <a name="typical-use-case"></a>Tipik kullanım örneği

Güvenlik amacıyla, bu IP listesini, başlangıç sunucunuzdaki istekleriyalnızca geçerli bir Verizon POP'tan yapılan istekleri zorlamak için kullanabilirsiniz. Örneğin, bir kişi CDN bitiş noktasının başlangıç sunucusunun ana bilgisayar adını veya IP adresini keşfettiyse, doğrudan kaynak sunucuya istekte bulunarak Azure CDN tarafından sağlanan ölçekleme ve güvenlik özelliklerini atlayabilir. Döndürülen listedeki IP'leri bir başlangıç sunucusunda izin verilen tek IP olarak ayarlayarak, bu senaryo önlenebilir. En son POP listesine sahip olduğundan emin olmak için, günde en az bir kez alın. 

## <a name="next-steps"></a>Sonraki adımlar

REST API hakkında bilgi için [Azure CDN REST API'ye](https://docs.microsoft.com/rest/api/cdn/)bakın.
