---
title: include dosyası
description: include dosyası
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 05/05/2019
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 46bf9851ccf8877885d9f98a023aa95ebdc533bc
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68857517"
---
| Resource | Hedef | Sabit sınır |
|----------|--------------|------------|
| Bölge başına depolama Eşitleme Hizmetleri | 20 depolama eşitleme hizmeti | Evet |
| Depolama eşitleme hizmeti başına eşitleme grupları | 100 eşitleme grupları | Evet |
| Depolama eşitleme hizmeti başına kayıtlı sunucular | 99 sunucu | Evet |
| Eşitleme grubu başına bulut uç noktaları | 1 bulut uç noktası | Evet |
| Eşitleme grubu başına sunucu uç noktaları | 50 sunucu uç noktaları | Hayır |
| Sunucu başına sunucu uç noktaları | 30 sunucu uç noktası | Evet |
| Eşitleme grubu başına dosya sistemi nesneleri (dizinler ve dosyalar) | 25.000.000 nesneleri | Hayır |
| Bir dizindeki en fazla dosya sistemi nesnesi (Dizin ve dosya) sayısı | 5\.000.000 nesneleri | Evet |
| En fazla nesne (dizinler ve dosyalar) güvenlik tanımlayıcısı boyutu | 64 KiB | Evet |
| Dosya boyutu | 100 GiB | Hayır |
| Katman oluşturulacak bir dosya için en küçük dosya boyutu | 64 KiB | Evet |
| Eşzamanlı eşitleme oturumları | V4 Aracısı ve üzeri: Sınır, kullanılabilir sistem kaynaklarına göre farklılık gösterir. <BR> V3 Aracısı: Her işlemci için iki etkin eşitleme oturumu veya sunucu başına en fazla sekiz etkin eşitleme oturumu. | Evet

> [!Note]  
> Azure Dosya Eşitleme uç noktası, bir Azure dosya paylaşımının boyutuna kadar ölçeklendirilebilir. Azure dosya paylaşımının boyut sınırına ulaşıldığında, eşitleme çalışamayacak.
