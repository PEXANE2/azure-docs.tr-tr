---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/15/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 10177dd949ac531027e13cf633b11c16674fd4ab
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94386544"
---
Önizleme aşamasında, NFS aşağıdaki sınırlamalara sahiptir:

- NFS 4,1 Şu anda yalnızca [protokol belirtiminden](https://tools.ietf.org/html/rfc5661)zorunlu özellikleri destekliyor. Tüm türleri temsilci ve geri çağırma, yükseltme yükseltmeleri ve indirgenme ve Kerberos kimlik doğrulaması ve şifreleme gibi isteğe bağlı özellikler desteklenmez.
- İsteklerinizin çoğunluğu meta veri merkezli ise, okuma/yazma/güncelleştirme işlemlerine kıyasla gecikme daha kötüleşmeyecektir.
- NFS paylaşımının oluşturulması için yeni bir depolama hesabı oluşturulması gerekir.
- Yalnızca yönetim düzlemi REST API 'Leri desteklenir. Veri düzlemi REST API 'Leri kullanılabilir değildir; bu, Depolama Gezgini gibi araçların NFS paylaşımları ile çalışmamasına veya Azure portal NFS paylaşım verilerine gözatmanıza olanak sağlar.
- Yalnızca Premium katmanı için kullanılabilir.
- Şu anda yalnızca yerel olarak yedekli depolama (LRS) ile kullanılabilir.

### <a name="azure-storage-features-not-yet-supported"></a>Azure depolama özellikleri henüz desteklenmiyor

Ayrıca, aşağıdaki Azure dosyaları özellikleri NFS paylaşımlarında kullanılamaz:

- Kimlik tabanlı kimlik doğrulaması
- Azure Backup desteği
- Anlık Görüntüler
- Geçici silme
- Aktarım için tam şifreleme desteği (Ayrıntılar için bkz. [NFS güvenliği](../articles/storage/files/storage-files-compare-protocols.md#security))
- Azure Dosya Eşitleme (yalnızca NFS 4,1 tarafından desteklenmeyen Windows istemcileri için kullanılabilir)
