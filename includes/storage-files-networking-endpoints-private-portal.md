---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 5/11/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b941084c8a196081c2443364ed3fb52868386670
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84465066"
---
Özel bir uç nokta oluşturmak istediğiniz depolama hesabına gidin. Depolama hesabının içindekiler tablosunda **Özel uç nokta bağlantıları**' nı ve ardından **+ Özel uç noktasını** seçerek yeni bir özel uç nokta oluşturun. 

[![Depolama hesabı İçindekiler tablosundaki özel uç nokta bağlantıları öğesinin ekran görüntüsü](media/storage-files-networking-endpoints-private-portal/create-private-endpoint-0.png)](media/storage-files-networking-endpoints-private-portal/create-private-endpoint-0.png#lightbox)

Ortaya çıkan sihirbazda, tamamlanacak birden çok sayfa vardır.

**Temel bilgiler** dikey penceresinde, Özel uç noktanız için istenen kaynak grubunu, adı ve bölgeyi seçin. Bunlar istediğiniz gibi olabilir, ancak içinde özel uç nokta oluşturmak istediğiniz sanal ağ ile aynı bölgede özel uç nokta oluşturmanız gerekir, ancak bu, depolama hesabını herhangi bir şekilde eşleştirmek zorunda kalmaz.

![Özel uç nokta oluştur bölümünün temel bilgiler bölümünün ekran görüntüsü](media/storage-files-networking-endpoints-private-portal/create-private-endpoint-1.png)

**Kaynak** dikey penceresinde, **Dizinimde bir Azure kaynağına bağlanmak**için radyo düğmesini seçin. **Kaynak**türü altında, kaynak türü için **Microsoft. Storage/storageaccounts** ' ı seçin. **Kaynak** alanı, bağlanmak istediğiniz Azure dosya paylaşımının bulunduğu depolama hesabıdır. Azure dosyaları için olduğundan, hedef alt kaynak **Dosya**.

**Yapılandırma** dikey penceresi, Özel uç noktanızı eklemek istediğiniz belirli sanal ağı ve alt ağı seçmenize olanak sağlar. Hizmet uç noktanızı eklemiş olduğunuz alt ağdan farklı bir alt ağ seçmelisiniz. Yapılandırma dikey penceresi ayrıca özel DNS bölgesi oluşturma/güncelleştirme bilgilerini içerir. Varsayılan bölgeyi kullanmanızı öneririz `privatelink.file.core.windows.net` .

![Yapılandırma bölümünün ekran görüntüsü](media/storage-files-networking-endpoints-private-portal/create-private-endpoint-2.png)

Özel uç noktayı oluşturmak için **gözden geçir + oluştur** ' a tıklayın. 