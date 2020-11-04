---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 10/13/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 29518fb971649087d8a5afa39d69c7fc2c014f98
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93331076"
---
| Kaynak | Hedef | Sabit sınır |
|----------|--------------|------------|
| Bölge başına depolama Eşitleme Hizmetleri | 100 depolama Eşitleme Hizmetleri | Evet |
| Depolama eşitleme hizmeti başına eşitleme grupları | 200 eşitleme grupları | Evet |
| Depolama eşitleme hizmeti başına kayıtlı sunucular | 99 sunucu | Evet |
| Eşitleme grubu başına bulut uç noktaları | 1 bulut uç noktası | Evet |
| Eşitleme grubu başına sunucu uç noktaları | 100 sunucu uç noktaları | Evet |
| Sunucu başına sunucu uç noktaları | 30 sunucu uç noktası | Evet |
| Eşitleme grubu başına dosya sistemi nesneleri (dizinler ve dosyalar) | 100.000.000 nesneleri | No |
| Bir dizindeki en fazla dosya sistemi nesnesi (Dizin ve dosya) sayısı | 5.000.000 nesneleri | Evet |
| En fazla nesne (dizinler ve dosyalar) güvenlik tanımlayıcısı boyutu | 64 KiB | Evet |
| Dosya boyutu | 100 GiB | No |
| Katman oluşturulacak bir dosya için en küçük dosya boyutu | V9 ve daha yeni: dosya sistemi kümesi boyutuna (çift dosya sistemi kümesi boyutu) göre. Örneğin, dosya sistemi kümesi boyutu 4kb ise, en küçük dosya boyutu 8 KB olur.<br> V8 ve üzeri: 64 KiB  | Evet |

> [!Note]  
> Azure Dosya Eşitleme uç noktası, bir Azure dosya paylaşımının boyutuna kadar ölçeklendirilebilir. Azure dosya paylaşımının boyut sınırına ulaşıldığında, eşitleme çalışamayacak.
