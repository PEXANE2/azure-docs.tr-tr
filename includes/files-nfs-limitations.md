---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/04/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 372342611265640a2a64100f003880a430d61ca0
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2020
ms.locfileid: "96620961"
---
Önizleme aşamasında, NFS aşağıdaki sınırlamalara sahiptir:

- NFS 4,1 Şu anda yalnızca [protokol belirtiminin](https://tools.ietf.org/html/rfc5661)birçok özelliğini desteklemektedir. Tüm tür temsilciler ve geri çağırma, yükseltme yükseltmeleri ve indirgenme, Kerberos kimlik doğrulaması ve şifreleme gibi bazı özellikler desteklenmez.
- İsteklerinizin çoğunluğu meta veri merkezli ise, okuma/yazma/güncelleştirme işlemlerine kıyasla gecikme daha kötüleşmeyecektir.
- NFS paylaşımının oluşturulması için yeni bir depolama hesabı oluşturulması gerekir.
- Yalnızca yönetim düzlemi REST API 'Leri desteklenir. Veri düzlemi REST API 'Leri kullanılabilir değildir; bu, Depolama Gezgini gibi araçların NFS paylaşımları ile çalışmamasına veya Azure portal NFS paylaşım verilerine gözatmanıza olanak sağlar.
- AzCopy Şu anda desteklenmiyor.
- Yalnızca Premium katmanı için kullanılabilir.
- NFS paylaşımları yalnızca sayısal UID/GID kabul eder. İstemcilerinizin alfasayısal UID/GID göndermesini önlemek için KIMLIK eşlemeyi devre dışı bırakmanız gerekir.
- Paylaşımlar, özel bağlantılar kullanılırken yalnızca tek bir VM 'deki bir depolama hesabından bağlanabilir. Diğer depolama hesaplarından paylaşım bağlama girişimi başarısız olur.

### <a name="azure-storage-features-not-yet-supported"></a>Azure depolama özellikleri henüz desteklenmiyor

Ayrıca, aşağıdaki Azure dosyaları özellikleri NFS paylaşımlarında kullanılamaz:

- Kimlik tabanlı kimlik doğrulaması
- Azure Backup desteği
- Anlık Görüntüler
- Geçici silme
- Aktarım için tam şifreleme desteği (Ayrıntılar için bkz. [NFS güvenliği](../articles/storage/files/storage-files-compare-protocols.md#security))
- Azure Dosya Eşitleme (yalnızca NFS 4,1 tarafından desteklenmeyen Windows istemcileri için kullanılabilir)
