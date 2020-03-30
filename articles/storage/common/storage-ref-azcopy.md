---
title: azcopy | Microsoft Dokümanlar
description: Bu makalede, azcopy komutu için referans bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 5da3a2e5d003a191bff66af6599cae4d34ab60c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74038081"
---
# <a name="azcopy"></a>azcopy

AzCopy, verileri Azure Depolama'ya giren ve çıkan bir komut satırı aracıdır.

## <a name="synopsis"></a>Özet

Komutların genel biçimi: `azcopy [command] [arguments] --[flag-name]=[flag-value]`.

Sorunları bildirmek veya araç hakkında daha [https://github.com/Azure/azure-storage-azcopy](https://github.com/Azure/azure-storage-azcopy)fazla bilgi edinmek için bkz.

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve Blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="options"></a>Seçenekler

**--kap-mbps uint32**   Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.

**-h, --yardım** Azcopy için yardım
      
**--çıkış türü**  Komutçıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer 'metin'dir. (varsayılan "metin")

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [azcopy ortamı](storage-ref-azcopy-bench.md)
- [azcopy kopya](storage-ref-azcopy-copy.md)
- [azcopy belge](storage-ref-azcopy-doc.md)
- [azcopy ortamı](storage-ref-azcopy-env.md)
- [azcopy işleri](storage-ref-azcopy-jobs.md)
- [azcopy işleri temizleme](storage-ref-azcopy-jobs-clean.md)
- [azcopy iş listesi](storage-ref-azcopy-jobs-list.md)
- [azcopy işleri kaldırma](storage-ref-azcopy-jobs-remove.md)
- [azcopy işleri sürdürme](storage-ref-azcopy-jobs-resume.md)
- [azcopy işleri gösterme](storage-ref-azcopy-jobs-show.md)
- [azcopy listesi](storage-ref-azcopy-list.md)
- [azcopy oturum açma](storage-ref-azcopy-login.md)
- [azcopy oturumu kapatma](storage-ref-azcopy-logout.md)
- [azcopy oluşturma](storage-ref-azcopy-make.md)
- [azcopy kaldırma](storage-ref-azcopy-remove.md)
- [azcopy eşitleme](storage-ref-azcopy-sync.md)
