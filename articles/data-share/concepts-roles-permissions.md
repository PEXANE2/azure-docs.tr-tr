---
title: Azure Veri Paylaşımı rolleri ve gereksinimleri
description: Azure Veri Paylaşımı'nı kullanarak veri paylaşmak ve almak için gereken izinler hakkında bilgi edinin.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 36a492f6a3e86cfb2fc9505550cc2d9f4746e070
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265512"
---
# <a name="roles-and-requirements-for-azure-data-share"></a>Azure Veri Paylaşımı rolleri ve gereksinimleri 

Bu makalede, Azure Veri Paylaşımı hizmetini kullanarak verileri paylaşmak ve almak için gereken roller ve izinler açıklanmaktadır. 

## <a name="roles-and-requirements"></a>Roller ve gereksinimler

Azure Veri Paylaşımı hizmeti yle, veri sağlayıcısı ve tüketici arasında kimlik bilgileri alışverişinde bulunmadan verileri paylaşabilirsiniz. Azure Veri Paylaşımı hizmeti, Azure veri deposuna kimlik doğrulamak için Yönetilen Kimlikler'i (önceden MSIs olarak bilinir) kullanır. 

Azure Veri Paylaşımı kaynağının yönetilen kimliğine Azure veri deposuna erişim izni verilmesi gerekir. Azure Veri Paylaşımı hizmeti daha sonra bu yönetilen kimliği anlık görüntü tabanlı paylaşım için veri okumak ve yazmak ve yerinde paylaşım için sembolik bağlantı oluşturmak için kullanır. 

Bir Azure veri deposundan veri paylaşmak veya almak için, kullanıcının en az aşağıdaki izinlere ihtiyacı vardır. SQL tabanlı paylaşım için ek izinler gereklidir.
* Azure veri deposuna yazma izni. Genellikle, bu izin **Katılımcı** rolünde bulunur.
* Azure veri deposunda rol ataması oluşturma izni. Tipik olarak, Yönetici **rolünde,** Kullanıcı Erişim Yöneticisi rolünde veya Microsoft.Authorization/role assignments/write izni atanan özel bir rolde rol atamaları oluşturma izni bulunur. Veri paylaşımı kaynağının yönetilen kimliği azure veri deposuna zaten erişim izni verilmişse, bu izin gerekmez. Gerekli rol için aşağıdaki tabloya bakın.

Aşağıda, Veri Paylaşımı kaynağının yönetilen kimliğine atanan rollerin bir özeti verilmiştir:

| |  |  |
|---|---|---|
|**Veri Deposu Türü**|**Veri Sağlayıcı Kaynak Veri Deposu**|**Veri Tüketici Hedef Veri Deposu**|
|Azure Blob Depolama| Depolama Blob Veri Okuyucu | Depolama Blob Veri Katılımcısı
|Azure Veri Gölü Gen1 | Sahip | Desteklenmiyor
|Azure Veri Gölü Gen2 | Depolama Blob Veri Okuyucu | Depolama Blob Veri Katılımcısı
|Azure SQL Sunucusu | SQL DB Katılımcısı | SQL DB Katılımcısı
|Azure Veri Gezgini Kümesi | Katılımcı | Katılımcı
|

SQL tabanlı paylaşım için, SQL veritabanındaki bir dış sağlayıcıdan Azure Veri Paylaşımı kaynağıyla aynı ada sahip bir SQL kullanıcısının oluşturulması gerekir. Aşağıda, SQL kullanıcısı tarafından istenen iznin bir özeti verilmiştir.

| |  |  |
|---|---|---|
|**SQL Veritabanı Türü**|**Veri Sağlayıcısı SQL Kullanıcı İzni**|**Veri Tüketici SQL Kullanıcı İzni**|
|Azure SQL Veritabanı | db_datareader | db_datareader, db_datawriter, db_ddladmin
|Azure Synapse Analytics (eski adı SQL DW) | db_datareader | db_datareader, db_datawriter, db_ddladmin
|


### <a name="data-provider"></a>Veri sağlayıcısı 
Azure Veri Paylaşımı'na bir veri kümesi eklemek için, sağlayıcı veri paylaşım kaynağının yönetilen kimliğine kaynak Azure veri deposuna erişim izni verilmesi gerekir. Örneğin, depolama hesabı durumunda, veri paylaşımı kaynağının yönetilen kimliğine Depolama Blob Veri Okuyucu rolü verilir. 

Bu işlem, kullanıcı Azure portalı üzerinden veri kümesi eklendiğinde ve kullanıcı uygun izne sahipse Azure Veri Paylaşımı hizmeti tarafından otomatik olarak yapılır. Örneğin, kullanıcı Azure veri deposunun sahibidir veya Microsoft.Authorization/role assignments/write izni atanmış olan özel bir rolün üyesidir. 

Alternatif olarak, kullanıcı Azure veri deposunun sahibine sahip olabilir, veri paylaşımı kaynağının yönetilen kimliğini Azure veri deposuna el ile ekleyebilir. Bu eylemin yalnızca veri paylaşımı kaynağı başına bir kez gerçekleştirilmesi gerekir.

Veri paylaşımı kaynağının yönetilen kimliği için bir rol ataması oluşturmak için aşağıdaki adımları izleyin:

1. Azure veri deposuna gidin.
1. **Erişim Denetimi 'ni (IAM)** seçin.
1. **Rol ataması ekle'yi**seçin.
1. *Role*altında, yukarıdaki rol atama tablosundaki rolü seçin (örneğin, depolama hesabı için *Depolama Blob Veri Okuyucu'yu*seçin).
1. *Select*, Azure Veri Paylaşımı kaynağınızın adını yazın.
1. *Kaydet*'e tıklayın.

SQL tabanlı kaynaklarda, yukarıdaki adımlara ek olarak, SQL veritabanındaki bir dış sağlayıcıdan Azure Veri Paylaşımı kaynağıyla aynı ada sahip bir SQL kullanıcısının oluşturulması gerekir. Bu *kullanıcıya db_datareader* izni nin verilmesi gerekir. SQL tabanlı paylaşım için diğer ön koşullarla birlikte örnek bir komut [dosyası, veri öğreticinizi paylaş'ta](share-your-data.md) bulunabilir. 

### <a name="data-consumer"></a>Veri tüketicisi
Veri almak için, tüketici veri paylaşım kaynağının yönetilen kimliğine hedef Azure veri deposuna erişim izni verilmesi gerekir. Örneğin, depolama hesabı durumunda, veri paylaşımı kaynağının yönetilen kimliğine Depolama Blob Veri Katılımcısı rolü verilir. 

Bu işlem, kullanıcının Azure portalı üzerinden bir hedef veri deposu belirtmesi ve kullanıcının uygun izni varsa Azure Veri Paylaşımı hizmeti tarafından otomatik olarak yapılır. Örneğin, kullanıcı Azure veri deposunun sahibidir veya Microsoft.Authorization/role assignments/write izni atanmış olan özel bir rolün üyesidir. 

Alternatif olarak, kullanıcı Azure veri deposunun sahibine sahip olabilir, veri paylaşımı kaynağının yönetilen kimliğini Azure veri deposuna el ile ekleyebilir. Bu eylemin yalnızca veri paylaşımı kaynağı başına bir kez gerçekleştirilmesi gerekir.

Veri paylaşımı kaynağının yönetilen kimliği için el ile bir rol ataması oluşturmak için aşağıdaki adımları izleyin:

1. Azure veri deposuna gidin.
1. **Erişim Denetimi 'ni (IAM)** seçin.
1. **Rol ataması ekle'yi**seçin.
1. *Role*altında, yukarıdaki rol atama tablosundaki rolü seçin (örneğin, depolama hesabı için *Depolama Blob Veri Okuyucu'yu*seçin).
1. *Select*, Azure Veri Paylaşımı kaynağınızın adını yazın.
1. *Kaydet*'e tıklayın.

SQL tabanlı hedef için, yukarıdaki adımlara ek olarak, SQL veritabanındaki bir dış sağlayıcıdan Azure Veri Paylaşımı kaynağıyla aynı ada sahip bir SQL kullanıcısının oluşturulması gerekir. Bu kullanıcıya *db_datareader, db_datawriter db_ddladmin* izni verilmesi gerekir. SQL tabanlı paylaşım için diğer ön koşullarla birlikte örnek bir komut dosyası [kabul ve veri](subscribe-to-data-share.md) öğretici almak bulunabilir. 

REST API'lerini kullanarak veri paylaşıyorsanız, bu rol atamalarını el ile oluşturmanız gerekir. 

Rol ataması nasıl ekleyeceğiniz hakkında daha fazla bilgi edinmek için [bu belgelere bakın.](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment) 

## <a name="resource-provider-registration"></a>Kaynak sağlayıcı kaydı 

Azure Veri Paylaşımı davetini Azure kiracınızda ilk kez görüntülemek için Microsoft.DataShare kaynak sağlayıcısını Azure aboneliğinize el ile kaydetmeniz gerekebilir. Microsoft.DataShare kaynak sağlayıcısını Azure Aboneliğinize kaydetmek için aşağıdaki adımları izleyin. Kaynak sağlayıcısını kaydetmek için *Katılımcı'nın* Azure aboneliğine erişmeniz gerekir.

1. Azure portalında **Abonelikler'e**gidin.
1. Azure Veri Paylaşımı için kullandığınız aboneliği seçin.
1. Kaynak **Sağlayıcıları'na**tıklayın.
1. Microsoft.DataShare'i arayın.
1. **Kaydol'u**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure'daki roller hakkında daha fazla bilgi edinin - [Rol tanımlarını anlama](../role-based-access-control/role-definitions.md)

