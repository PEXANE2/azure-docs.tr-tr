---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/05/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 95c553d26a3e79b53106b933c629c5884c3e004c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84466811"
---
| Kaynak | Hedef | Sabit sınır |
|----------|--------------|------------|
| Bölge başına depolama Eşitleme Hizmetleri | 100 depolama Eşitleme Hizmetleri | Evet |
| Depolama eşitleme hizmeti başına eşitleme grupları | 200 eşitleme grupları | Evet |
| Depolama eşitleme hizmeti başına kayıtlı sunucular | 99 sunucu | Evet |
| Eşitleme grubu başına bulut uç noktaları | 1 bulut uç noktası | Evet |
| Eşitleme grubu başına sunucu uç noktaları | 50 sunucu uç noktaları | Hayır |
| Sunucu başına sunucu uç noktaları | 30 sunucu uç noktası | Evet |
| Eşitleme grubu başına dosya sistemi nesneleri (dizinler ve dosyalar) | 100.000.000 nesneleri | Hayır |
| Bir dizindeki en fazla dosya sistemi nesnesi (Dizin ve dosya) sayısı | 5.000.000 nesneleri | Evet |
| En fazla nesne (dizinler ve dosyalar) güvenlik tanımlayıcısı boyutu | 64 KiB | Evet |
| Dosya boyutu | 100 GiB | Hayır |
| Katman oluşturulacak bir dosya için en küçük dosya boyutu | V9: dosya sistemi kümesi boyutuna (çift dosya sistemi kümesi boyutu) göre. Örneğin, dosya sistemi kümesi boyutu 4kb ise, en küçük dosya boyutu 8 KB olur.<br> V8 ve üzeri: 64 KiB  | Evet |

> [!Note]  
> Azure Dosya Eşitleme uç noktası, bir Azure dosya paylaşımının boyutuna kadar ölçeklendirilebilir. Azure dosya paylaşımının boyut sınırına ulaşıldığında, eşitleme çalışamayacak.
