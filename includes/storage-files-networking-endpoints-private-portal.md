---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 5/11/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 09723030f0da0252120f66f36347cad2e3a3546a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93375972"
---
Özel bir uç nokta oluşturmak istediğiniz depolama hesabına gidin. Depolama hesabının içindekiler tablosunda **Özel uç nokta bağlantıları**' nı ve ardından **+ Özel uç noktasını** seçerek yeni bir özel uç nokta oluşturun. 

[![Depolama hesabı İçindekiler tablosundaki özel uç nokta bağlantıları öğesinin ekran görüntüsü](media/storage-files-networking-endpoints-private-portal/create-private-endpoint-0.png)](media/storage-files-networking-endpoints-private-portal/create-private-endpoint-0.png#lightbox)

Ortaya çıkan sihirbazda, tamamlanacak birden çok sayfa vardır.

**Temel bilgiler** dikey penceresinde, Özel uç noktanız için istenen kaynak grubunu, adı ve bölgeyi seçin. Bunlar istediğiniz gibi olabilir, ancak içinde özel uç nokta oluşturmak istediğiniz sanal ağ ile aynı bölgede özel uç nokta oluşturmanız gerekir, ancak bu, depolama hesabını herhangi bir şekilde eşleştirmek zorunda kalmaz.

![Özel uç nokta Oluştur bölümünde temel bilgiler bölümünün ekran görüntüsü](media/storage-files-networking-endpoints-private-portal/create-private-endpoint-1.png)

**Kaynak** dikey penceresinde, **Dizinimde bir Azure kaynağına bağlanmak** için radyo düğmesini seçin. **Kaynak** türü altında, kaynak türü için **Microsoft. Storage/storageaccounts** ' ı seçin. **Kaynak** alanı, bağlanmak istediğiniz Azure dosya paylaşımının bulunduğu depolama hesabıdır. Azure dosyaları için olduğundan, hedef alt kaynak **Dosya**.

**Yapılandırma** dikey penceresi, Özel uç noktanızı eklemek istediğiniz belirli sanal ağı ve alt ağı seçmenize olanak sağlar. Hizmet uç noktanızı eklemiş olduğunuz alt ağdan farklı bir alt ağ seçmelisiniz. Yapılandırma dikey penceresi ayrıca özel DNS bölgesi oluşturma/güncelleştirme bilgilerini içerir. Varsayılan bölgeyi kullanmanızı öneririz `privatelink.file.core.windows.net` .

![Yapılandırma bölümünün ekran görüntüsü](media/storage-files-networking-endpoints-private-portal/create-private-endpoint-2.png)

Özel uç noktayı oluşturmak için **gözden geçir + oluştur** ' a tıklayın. 
