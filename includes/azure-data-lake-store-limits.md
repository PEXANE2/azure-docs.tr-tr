---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: twooley
ms.service: storage
ms.topic: include
ms.date: 09/30/2020
ms.author: twooley
ms.custom: include file
ms.openlocfilehash: 358de5761f3535b5f1e75d82539ee8e3ecdbd289
ms.sourcegitcommit: 67e8e1caa8427c1d78f6426c70bf8339a8b4e01d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91665636"
---
**Azure Data Lake Storage 2.** adanmış bir hizmet veya depolama hesabı türü değil. Bu, büyük veri analizi için ayrılmış olan en son yetenekler sürümüdür.  Bu özellikler, genel amaçlı bir v2 veya BlockBlobStorage Storage hesabında kullanılabilir ve hesabın **hiyerarşik ad alanı** özelliğini etkinleştirerek bunları elde edebilirsiniz. Ölçek hedefleri için, bu makalelere bakın. 

- [BLOB depolama için hedefleri ölçeklendirin](https://docs.microsoft.com/azure/storage/blobs/scalability-targets#scale-targets-for-blob-storage).
- [Standart depolama hesapları Için ölçek hedefleri](https://docs.microsoft.com/azure/storage/common/scalability-targets-standard-account?toc=/azure/storage/blobs/toc.json#scale-targets-for-standard-storage-accounts).

**Azure Data Lake Storage 1.** adanmış bir hizmettir. Bu, büyük veri analizi iş yükleri için kurumsal çapta bir hiper ölçekli depodur. İşlemsel ve araştırmacı analizler için tek bir yerde herhangi bir boyut, tür ve Alım hızının verilerini yakalamak üzere Data Lake Storage 1. kullanabilirsiniz. Data Lake Storage 1. hesabında depolayabilmeniz için veri miktarına yönelik bir sınır yoktur.

| **Kaynak** | **Sınırlı** | **Açıklamalar** |
| --- | --- | --- |
| Abonelik başına en fazla Data Lake Storage 1. hesabı sayısı (bölge başına) |10 | Bu sınıra yönelik bir artış istemek için desteğe başvurun. |
| En fazla erişim ACL sayısı, dosya veya klasör başına |32 | Bu bir sabit sınır. Daha az girişe erişimi yönetmek için grupları kullanın. |
| Dosya veya klasör başına en fazla varsayılan ACL sayısı |32 | Bu bir sabit sınır. Daha az girişe erişimi yönetmek için grupları kullanın. |
