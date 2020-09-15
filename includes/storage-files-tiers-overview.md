---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/27/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a2493ce764f2aed2aca1d555f98b738185159f7a
ms.sourcegitcommit: 51df05f27adb8f3ce67ad11d75cb0ee0b016dc5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90064837"
---
Azure dosyaları, kendi senaryolarınızın performans ve fiyat gereksinimlerine yönelik olarak paylaşımlarınızı uyarlamanızı sağlamak için dört farklı depolama, Premium, işlem için iyileştirilmiş, sık erişimli ve seyrek katman katmanları sunar:

- **Premium**: Premium dosya paylaşımları katı hal sürücüleri (SSD 'ler) tarafından desteklenir ve **FileStorage depolama hesabı** türüne dağıtılır. Premium dosya paylaşımları, GÇ yoğun iş yükleri için çoğu GÇ işlemleri için tek basamaklı milisaniye içinde tutarlı yüksek performans ve düşük gecikme sağlar. Premium dosya paylaşımları veritabanları, Web sitesi barındırma ve geliştirme ortamları gibi çok çeşitli iş yükleri için uygundur. 
- **İşlem için iyileştirilmiş**: işlem için iyileştirilmiş dosya paylaşımları Premium dosya paylaşımları tarafından sunulan gecikmeye gerek olmayan işlem ağır iş yüklerini etkinleştirir. İşlem için iyileştirilmiş dosya paylaşımları, sabit disk sürücüleri (HDD 'Ler) tarafından desteklenen standart depolama donanımında sunulur ve **genel amaçlı sürüm 2 (GPv2) depolama hesabı** türünde dağıtılır. İşlem için iyileştirilmiş, "standart" olarak adlandırılmıştı, ancak bu, katmanın kendisi yerine depolama medyası türü anlamına gelir (Standart depolama donanımında olduklarından, sık ve seyrek da "standart" katmanlarıdır).
- **Sık**erişimli: sık erişimli dosya paylaşımları, takım paylaşımları ve Azure dosya eşitleme gibi genel amaçlı dosya paylaşım senaryolarında en iyi duruma getirilmiş depolama sunar. Etkin dosya paylaşımları, HDD 'Ler tarafından desteklenen standart depolama donanımında sunulur ve **genel amaçlı sürüm 2 (GPv2) depolama hesabı** türünde dağıtılır.
- Seyrek **erişimli: seyrek**erişimli dosya paylaşımları, çevrimiçi arşiv depolama senaryoları için iyileştirilmiş uygun maliyetli depolama sunar. Azure Dosya Eşitleme, daha düşük dalgalanma iş yükleri için de uygun olabilir. Seyrek Erişimli dosya paylaşımları, HDD 'Ler tarafından desteklenen standart depolama donanımında sunulur ve **genel amaçlı sürüm 2 (GPv2) depolama hesabı** türünde dağıtılır.

Premium dosya paylaşımları yalnızca sağlanan faturalandırma modelinde kullanılabilir. Premium dosya paylaşımları için sağlanan faturalandırma modeli hakkında daha fazla bilgi için bkz. [Premium dosya paylaşımları için sağlamayı anlama](../articles/storage/files/storage-files-planning.md#understanding-provisioning-for-premium-file-shares). İşlem için iyileştirilmiş, sık ve seyrek erişimli dosya paylaşımları dahil olmak üzere standart dosya paylaşımları, Kullandıkça Öde faturalandırmasıyla sunulmaktadır.

Sık erişimli ve seyrek erişimli dosya paylaşımları, tüm Azure genel bölgelerinde kullanılabilir. İşlem için iyileştirilmiş dosya paylaşımları, Ulusal bulut bölgeleri dahil olmak üzere tüm Azure bölgelerinde kullanılabilir.

Sık erişimli veya seyrek erişimli bir dosya paylaşma dağıtmak için bkz. sık erişimli veya seyrek erişimli [dosya paylaşma oluşturma](../articles/storage/files/storage-how-to-create-file-share.md#create-a-hot-or-cool-file-share). 