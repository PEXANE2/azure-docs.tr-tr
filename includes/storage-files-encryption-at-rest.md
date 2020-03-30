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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597870"
---
Azure Dosyalarında depolanan tüm veriler, Azure depolama hizmeti şifrelemesi (SSE) kullanılarak istirahatte şifrelenir. Depolama hizmeti şifrelemesi Windows'daki BitLocker'a benzer şekilde çalışır: veriler dosya sistemi düzeyinin altında şifrelenir. Veriler Azure dosya paylaşımının dosya sisteminin altında şifrelenmiş olduğundan, diske kodlandığı için, Azure dosya paylaşımını okumak veya yazmak için istemcinin temel anahtarına erişmeniz gerekmez.

Varsayılan olarak, Azure Dosyaları'nda depolanan veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Microsoft tarafından yönetilen anahtarlarla Microsoft, verileri şifrelemek/çözmek için anahtarları tutar ve bunları düzenli olarak döndürmekten sorumludur. Ayrıca, döndürme işlemi üzerinde kontrol sağlar kendi anahtarları, yönetmek için seçebilirsiniz. Dosya paylaşımlarınızı müşteri tarafından yönetilen anahtarlarla şifrelemeyi seçerseniz, Azure Dosyaları müşterilerinizden gelen okuma ve yazma isteklerini yerine getirmek için anahtarlarınıza erişmeye yetkilidir. Müşteri tarafından yönetilen anahtarlarla bu yetkilendirmeyi istediğiniz zaman iptal edebilirsiniz, ancak bu, Azure dosya paylaşımınıza Artık SMB veya FileREST API üzerinden erişilmeyeceğini zedebilirsiniz.

Azure Files, Azure Blob depolama gibi diğer Azure depolama hizmetleriyle aynı şifreleme düzenini kullanır. Azure depolama hizmeti şifrelemesi (SSE) hakkında daha fazla bilgi edinmek [için, veriler için azure depolama şifrelemesine](../articles/storage/common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)bakın.