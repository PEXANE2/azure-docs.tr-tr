---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 01/27/2021
ms.author: rogarana
ms.openlocfilehash: 7da7c2fbb49a9dd936762b23f3c251d2142c52fd
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2021
ms.locfileid: "99221775"
---
| Kaynak | Standart dosya paylaşımları\* | Premium dosya paylaşımları |
|----------|---------------|------------------------------------------|
| Dosya paylaşımının en küçük boyutu | En az; Kullandıkça öde | 100 GiB; oluşturulamadı |
| Dosya paylaşımının en büyük boyutu | 100 TiB \* \* , 5 Tib | 100 TiB |
| Dosya paylaşımındaki bir dosyanın en büyük boyutu | 4 TiB | 4 TiB |
| Dosya paylaşımındaki en fazla dosya sayısı | Sınır yok | Sınır yok |
| Paylaşma başına maksimum ıOPS | \* \* 100ms 'de 10.000 ıops, 1.000 ıops veya 100 isteği | 100.000 ıOPS |
| Dosya paylaşma başına en fazla depolanan erişim ilkesi sayısı | 5 | 5 |
| Tek bir dosya paylaşımının hedef performansı | en fazla 300 MIB/sn \* \* , en fazla 60 MIB/sn,  | Bkz. Premium dosya paylaşma giriş ve çıkış değerleri|
| Tek bir dosya paylaşımında maksimum çıkış | Bkz. standart dosya paylaşma hedef işleme | En fazla 6.204 MIB/sn |
| Tek bir dosya paylaşımının en büyük girişi | Bkz. standart dosya paylaşma hedef işleme | En fazla 4.136 MIB/sn |
| Dosya veya dizin başına en fazla açık tanıtıcı | 2.000 açık tanıtıcı | 2.000 açık tanıtıcı |
| Maksimum paylaşılan anlık görüntü sayısı | 200 paylaşma anlık görüntüleri | 200 paylaşma anlık görüntüleri |
| En fazla nesne (Dizin ve dosya) adı uzunluğu | 2.048 karakter | 2.048 karakter |
| En fazla yol adı bileşeni (\A\B\C\D yolunda her bir bir bileşendir) | 255 karakter | 255 karakter |
| Sabit bağlantı sınırı (yalnızca NFS) | Yok | 178 |
| En fazla SMB çok kanallı kanal sayısı | YOK | 4 |

\* Standart dosya paylaşımları için sınırlar Standart dosya paylaşımları için kullanılabilen her üç katmanda geçerlidir: işlem için iyileştirilmiş, sık erişimli ve seyrek.

\*\* Standart dosya paylaşımlarında varsayılan değer 5 TiB 'dir ve standart dosya paylaşımlarının ölçeğini 100 TiB 'ye kadar artırma hakkında daha fazla bilgi için bkz. [büyük dosya paylaşımlarını etkinleştirme ve oluşturma](../articles/storage/files/storage-files-how-to-create-large-file-share.md) .
