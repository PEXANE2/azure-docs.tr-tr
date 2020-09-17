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
ms.openlocfilehash: 8901f54c4802766eb3f12830d98d29fb47337ae5
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90705223"
---
Önizleme aşamasında, NFS aşağıdaki sınırlamalara sahiptir:

- NFS 4,1 Şu anda yalnızca [protokol belirtiminden](https://tools.ietf.org/html/rfc5661)zorunlu özellikleri destekliyor. Tüm türleri temsilci ve geri çağırma, yükseltme yükseltmeleri ve indirgenme ve Kerberos kimlik doğrulaması ve şifreleme gibi isteğe bağlı özellikler desteklenmez.
- İsteklerinizin çoğunluğu meta veri merkezli ise, okuma/yazma/güncelleştirme işlemlerine kıyasla gecikme daha kötüleşmeyecektir.
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