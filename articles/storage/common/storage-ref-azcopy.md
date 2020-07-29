---
title: AzCopy | Microsoft Docs
description: Bu makale AzCopy komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 18972e991f08db7fa9548454a5c5cdc3ff0f552f
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87285195"
---
# <a name="azcopy"></a>azcopy

AzCopy, Azure depolama 'ya ve dışına veri taşınan bir komut satırı aracıdır.

## <a name="synopsis"></a>Özeti

Komutların genel biçimi: `azcopy [command] [arguments] --[flag-name]=[flag-value]` .

Sorunları bildirmek veya araç hakkında daha fazla bilgi edinmek için bkz [https://github.com/Azure/azure-storage-azcopy](https://github.com/Azure/azure-storage-azcopy) ..

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve BLOB Storage ile veri aktarma](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile verileri aktarma](storage-use-azcopy-files.md)
- [AzCopy 'i yapılandırma, iyileştirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="options"></a>Seçenekler

**--Cap-Mbps** (float), saniye başına megabit cinsinden aktarım hızını artırır. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.

**--Yardım** AzCopy için yardım
      
**--komut çıktısının çıkış türü** (dize) biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer: `text`. (varsayılan `text` )

**--Güvenilen-Microsoft-Suffixes** (dize) Azure Active Directory oturum açma belirteçlerinin gönderilebileceği ek etki alanı soneklerini belirler.  Varsayılan değer '*. Core.Windows.net;*' dir. core.chinacloudapi.cn; *. Core.cloudapi.de;*. core.usgovcloudapi.net '. Burada listelenenler varsayılan olarak eklenir. Güvenlik için yalnızca Microsoft Azure etki alanlarını Buraya yerleştirmeniz gerekir. Birden çok girişi noktalı virgülle ayırın.

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
