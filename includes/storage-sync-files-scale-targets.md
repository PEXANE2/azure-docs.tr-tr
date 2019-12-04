---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/05/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 6a053b94813145f9ccd69158d18edb728d5dad61
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795855"
---
| Kaynak | Hedefleyin | Sabit sınır |
|----------|--------------|------------|
| Bölge başına depolama Eşitleme Hizmetleri | 20 depolama eşitleme hizmeti | Yes |
| Depolama eşitleme hizmeti başına eşitleme grupları | 100 eşitleme grupları | Yes |
| Depolama eşitleme hizmeti başına kayıtlı sunucular | 99 sunucu | Yes |
| Eşitleme grubu başına bulut uç noktaları | 1 bulut uç noktası | Yes |
| Eşitleme grubu başına sunucu uç noktaları | 50 sunucu uç noktaları | Hayır |
| Sunucu başına sunucu uç noktaları | 30 sunucu uç noktası | Yes |
| Eşitleme grubu başına dosya sistemi nesneleri (dizinler ve dosyalar) | 100.000.000 nesneleri | Hayır |
| Bir dizindeki en fazla dosya sistemi nesnesi (Dizin ve dosya) sayısı | 5\.000.000 nesneleri | Yes |
| En fazla nesne (dizinler ve dosyalar) güvenlik tanımlayıcısı boyutu | 64 KiB | Yes |
| Dosya boyutu | 100 GiB | Hayır |
| Katman oluşturulacak bir dosya için en küçük dosya boyutu | V9: dosya sistemi kümesi boyutuna (çift dosya sistemi kümesi boyutu) göre. Örneğin, dosya sistemi kümesi boyutu 4kb ise, en küçük dosya boyutu 8 KB olur.<br> V8 ve üzeri: 64 KiB  | Yes |

> [!Note]  
> Azure Dosya Eşitleme uç noktası, bir Azure dosya paylaşımının boyutuna kadar ölçeklendirilebilir. Azure dosya paylaşımının boyut sınırına ulaşıldığında, eşitleme çalışamayacak.
