---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/15/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7e009516721473554517d1677bdef7a7451e4007
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90564938"
---
Önizleme aşamasında, NFS aşağıdaki sınırlamalara sahiptir:

- NFS 4,1 Şu anda yalnızca [protokol belirtiminden](https://tools.ietf.org/html/rfc5661)zorunlu özellikleri destekliyor. Tüm türleri temsilci ve geri çağırma, yükseltme yükseltmeleri ve indirgenme ve Kerberos kimlik doğrulaması ve şifreleme gibi isteğe bağlı özellikler desteklenmez.
- İsteklerinizin çoğunluğu meta veri merkezli ise, açma/kapatma işlemlerine kıyasla gecikme daha kötüleşmeyecektir.
- NFS paylaşımının oluşturulması için yeni bir depolama hesabı oluşturulması gerekir.
- Yalnızca yönetim düzlemi REST API 'Leri desteklenir. Veri düzlemi REST API 'Leri kullanılabilir değildir, bu da Depolama Gezgini gibi araçların NFS paylaşımları ile çalışmamasına veya Azure portal NFS paylaşım verilerine gözatmanıza olanak sağlayacak.
- Yalnızca Premium katmanı için kullanılabilir.
- Şu anda yalnızca yerel olarak yedekli depolama ile kullanılabilir.

### <a name="azure-storage-features-not-yet-supported"></a>Azure depolama özellikleri henüz desteklenmiyor

Ayrıca, aşağıdaki Azure dosyaları özellikleri NFS paylaşımlarında kullanılamaz:

- Kimlik tabanlı kimlik doğrulaması
- Azure Backup desteği
- Anlık Görüntüler
- Geçici silme
- Aktarım için tam şifreleme desteği (Ayrıntılar için bkz. [NFS güvenliği](../articles/storage/files/storage-files-compare-protocols.md#security))
- Azure Dosya Eşitleme (yalnızca NFS 4,1 tarafından desteklenmeyen Windows istemcileri için kullanılabilir)