---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/27/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: ed061234da27716bfcdaf1cc065568908108f90a
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77597870"
---
Azure dosyalarında depolanan tüm veriler, Azure depolama hizmeti şifrelemesi (SSE) kullanılarak Rest 'te şifrelenir. Depolama hizmeti şifrelemesi Windows 'da BitLocker 'a benzer şekilde çalışır: veriler dosya sistemi düzeyinin altında şifrelenir. Veriler, Azure dosya paylaşımının dosya sisteminin altında, diske kodlandığı gibi şifrelendiğinden, Azure dosya paylaşımından okumak veya yazmak için istemcideki temel anahtara erişiminizin olması gerekmez.

Varsayılan olarak, Azure dosyalarında depolanan veriler, Microsoft tarafından yönetilen anahtarlarla şifrelenir. Microsoft tarafından yönetilen anahtarlarla, Microsoft, verileri şifrelemek/şifrelerini çözmek için anahtarları tutar ve bunları düzenli olarak döndürmekten sorumludur. Ayrıca, kendi anahtarlarınızı yönetmeyi seçebilirsiniz ve bu da döndürme sürecinde denetim sağlar. Dosya paylaşımlarınızı müşteri tarafından yönetilen anahtarlarla şifrelemeyi seçerseniz, Azure dosyaları, istemcilerinizden okuma ve yazma isteklerini karşılamak üzere Anahtarlarınıza erişme yetkisine sahiptir. Müşteri tarafından yönetilen anahtarlarla, bu yetkilendirmeyi dilediğiniz zaman iptal edebilirsiniz, ancak Azure dosya paylaşımınızın SMB veya FileREST API 'SI aracılığıyla artık erişilemeyeceği anlamına gelir.

Azure dosyaları, Azure Blob depolama gibi diğer Azure depolama hizmetleriyle aynı şifreleme şemasını kullanır. Azure depolama hizmeti şifrelemesi (SSE) hakkında daha fazla bilgi edinmek için bkz. [bekleyen veriler Için Azure depolama şifrelemesi](../articles/storage/common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).