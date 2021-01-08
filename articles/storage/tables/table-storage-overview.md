---
title: Tablo depolamaya giriş - Azure’da nesne depolama | Microsoft Docs
description: Bir NoSQL veri deposu olan Azure Table Storage kullanarak bulutta yapılandırılmış veri depolayın.
services: storage
ms.service: storage
author: tamram
ms.author: tamram
ms.devlang: dotnet
ms.topic: overview
ms.date: 01/07/2021
ms.subservice: tables
ms.openlocfilehash: 292104651c6bb9b63e9d8cbe26fea2bf8c6acbf1
ms.sourcegitcommit: e46f9981626751f129926a2dae327a729228216e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98027749"
---
# <a name="what-is-azure-table-storage-"></a>Azure Tablo depolama nedir? 

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

Azure Tablo depolama, bulutta, ilişkisel olmayan yapılandırılmış verileri (yapılandırılmış NoSQL verileri olarak da bilinir) depolayan ve Şemasız bir tasarıma sahip anahtar/öznitelik deposu sağlayan bir hizmettir. Table Storage şemasız olduğu için uygulamanızın ihtiyaçları geliştikçe verilerinizi kolayca uyarlayabilirsiniz. Tablo Depolama verilerine erişim, çoğu uygulama türü için hızlı ve ekonomik olmanın yanı sıra benzer veri hacimleri için geleneksel SQL’e kıyasla genellikle daha düşük maliyetlidir.

Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri veya hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini Tablo Depolama ile depolayabilirsiniz. Bir tabloda istediğiniz kadar varlık depolayabilirsiniz ve bir depolama hesabı kapasite limitini dolduracak kadar tablo içerebilir.

[!INCLUDE [storage-table-concepts-include](../../../includes/storage-table-concepts-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.

* [.NET’te Azure Tablo Depolama kullanmaya başlama](../../cosmos-db/tutorial-develop-table-dotnet.md)

* Kullanılabilir API’ler ile ilgili eksiksiz bilgiler için Tablo hizmeti başvuru belgelerini görüntüleyin:

    * [.NET için depolama Istemci kitaplığı başvurusu](/dotnet/api/overview/azure/storage)

    * [REST API başvurusu](/rest/api/storageservices/)