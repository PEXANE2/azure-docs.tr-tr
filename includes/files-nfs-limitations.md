---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/04/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 4b708e80bf335ba8bdce074285857a6f8b77b972
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "103439222"
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
- Birincil gruba atanan izinlere güvenen iyisidir. Bazen, kullanıcının birincil olmayan grubuna ayrılan izinler, bilinen bir hata nedeniyle erişime erişim izni verebilir.

### <a name="azure-storage-features-not-yet-supported"></a>Azure depolama özellikleri henüz desteklenmiyor

Ayrıca, aşağıdaki Azure dosyaları özellikleri NFS paylaşımlarında kullanılamaz:

- Kimlik tabanlı kimlik doğrulaması
- Azure Backup desteği
- Anlık Görüntüler
- Geçici silme
- Aktarım için tam şifreleme desteği (Ayrıntılar için bkz. [NFS güvenliği](../articles/storage/files/storage-files-compare-protocols.md#security))
- Azure Dosya Eşitleme (yalnızca NFS 4,1 tarafından desteklenmeyen Windows istemcileri için kullanılabilir)
