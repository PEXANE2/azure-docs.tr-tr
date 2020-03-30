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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74795855"
---
| Kaynak | Hedef | Sabit sınır |
|----------|--------------|------------|
| Bölge başına Depolama Eşitleme Hizmetleri | 20 Depolama Eşitleme Hizmetleri | Evet |
| Depolama Eşitleme Hizmeti başına eşitleme grupları | 100 eşitleme grubu | Evet |
| Depolama Eşitleme Hizmeti başına kayıtlı sunucular | 99 sunucu | Evet |
| Eşitleme grubu başına bulut uç noktaları | 1 bulut bitiş noktası | Evet |
| Eşitleme grubu başına sunucu uç noktaları | 50 sunucu bitiş noktası | Hayır |
| Sunucu başına sunucu uç noktaları | 30 sunucu bitiş noktası | Evet |
| Eşitleme grubuna göre dosya sistemi nesneleri (dizinler ve dosyalar) | 100 milyon nesne | Hayır |
| Bir dizinde maksimum dosya sistemi nesnesi (dizin ve dosya) sayısı | 5 milyon nesne | Evet |
| Maksimum nesne (dizinler ve dosyalar) güvenlik tanımlayıcı boyutu | 64 İçli Köfte | Evet |
| Dosya boyutu | 100 GiB | Hayır |
| Bir dosyanın katmanlandırılmak için minimum dosya boyutu | V9: Dosya sistemi küme boyutuna (çift dosya sistemi küme boyutu) göre. Örneğin, dosya sistemi küme boyutu 4kb ise, minimum dosya boyutu 8kb olacaktır.<br> V8 ve üzeri: 64 KiB  | Evet |

> [!Note]  
> Azure Dosya Eşitleme bitiş noktası, Azure dosya paylaşımı boyutuna kadar ölçeklenebilir. Azure dosya paylaşımı boyut sınırına ulaşılırsa, eşitleme çalışamaz.
