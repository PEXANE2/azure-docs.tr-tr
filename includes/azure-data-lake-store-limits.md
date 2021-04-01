---
title: include dosyası
description: include dosyası
services: storage
author: twooley
ms.service: storage
ms.topic: include
ms.date: 09/30/2020
ms.author: twooley
ms.custom: include file
ms.openlocfilehash: 7098f23e9b5b6f56fbbe761335afc65375aea680
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96509354"
---
**Azure Data Lake Storage 2.** adanmış bir hizmet veya depolama hesabı türü değil. Bu, büyük veri analizi için ayrılmış olan en son yetenekler sürümüdür.  Bu özellikler, genel amaçlı bir v2 veya BlockBlobStorage Storage hesabında kullanılabilir ve hesabın **hiyerarşik ad alanı** özelliğini etkinleştirerek bunları elde edebilirsiniz. Ölçek hedefleri için, bu makalelere bakın. 

- [BLOB depolama için hedefleri ölçeklendirin](../articles/storage/blobs/scalability-targets.md#scale-targets-for-blob-storage).
- [Standart depolama hesapları Için ölçek hedefleri](../articles/storage/common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#scale-targets-for-standard-storage-accounts).

**Azure Data Lake Storage 1.** adanmış bir hizmettir. Bu, büyük veri analizi iş yükleri için kurumsal çapta bir hiper ölçekli depodur. İşlemsel ve araştırmacı analizler için tek bir yerde herhangi bir boyut, tür ve Alım hızının verilerini yakalamak üzere Data Lake Storage 1. kullanabilirsiniz. Data Lake Storage 1. hesabında depolayabilmeniz için veri miktarına yönelik bir sınır yoktur.

| **Kaynak** | **Sınırlı** | **Açıklamalar** |
| --- | --- | --- |
| Abonelik başına en fazla Data Lake Storage 1. hesabı sayısı (bölge başına) |10 | Bu sınıra yönelik bir artış istemek için desteğe başvurun. |
| En fazla erişim ACL sayısı, dosya veya klasör başına |32 | Bu bir sabit sınır. Daha az girişe erişimi yönetmek için grupları kullanın. |
| Dosya veya klasör başına en fazla varsayılan ACL sayısı |32 | Bu bir sabit sınır. Daha az girişe erişimi yönetmek için grupları kullanın. |