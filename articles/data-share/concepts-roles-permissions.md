---
title: Azure Veri Paylaşımı rolleri ve gereksinimleri
description: Azure veri paylaşımının kullanıldığı verileri paylaşmak ve almak için gereken izinler hakkında bilgi edinin.
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: ed4b4d9c1de1e9024e8ea86d4661b42d6c68b0ae
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96461001"
---
# <a name="roles-and-requirements-for-azure-data-share"></a>Azure Veri Paylaşımı rolleri ve gereksinimleri 

Bu makalede, Azure veri paylaşma hizmeti 'ni kullanarak veri paylaşmak ve almak için gereken roller ve izinler açıklanmaktadır. 

## <a name="roles-and-requirements"></a>Roller ve gereksinimler

Azure veri paylaşma hizmeti ile veri sağlayıcısı ile tüketici arasında kimlik bilgileri alışverişi yapmadan veri paylaşabilirsiniz. Azure Data Share hizmeti, Azure veri deposunda kimlik doğrulamak için Yönetilen kimlikler (daha önce Mssıs olarak bilinir) kullanır. 

Azure veri paylaşımının kaynağına ait yönetilen kimliğe Azure veri deposuna erişim verilmesi gerekir. Daha sonra Azure veri paylaşımı hizmeti, anlık görüntü tabanlı paylaşıma yönelik verileri okumak ve yazmak ve yerinde paylaşım için sembolik bağlantı kurmak üzere bu yönetilen kimliği kullanır. 

Bir Azure veri deposundan veri paylaşmak veya almak için, kullanıcının en azından aşağıdaki izinlere ihtiyacı vardır. SQL tabanlı paylaşım için ek izinler gereklidir.

* Azure veri deposuna yazma izni. Genellikle, bu izin **katkıda** bulunan rolünde bulunur.
* Azure veri deposunda rol ataması oluşturma izni. Genellikle, rol atamaları oluşturma izni **sahip** rolü, Kullanıcı erişimi yönetici rolü veya Microsoft. Authorization/role atamaları/yazma izni atanmış özel bir rol içinde bulunur. Veri paylaşımının yönetilen kimliği zaten Azure veri deposuna erişim izni verildiyse, bu izin gerekli değildir. Gerekli rol için aşağıdaki tabloya bakın.

Aşağıda, veri paylaşımının kaynak yönetimli kimliğine atanan rollerin özeti verilmiştir:

|**Veri deposu türü**|**Kaynak veri deposu Veri Sağlayıcısı**|**Veri tüketicisi hedef veri deposu**|
|---|---|---|
|Azure Blob Depolama| Depolama Blob Verileri Okuyucusu | Depolama Blob Verileri Katkıda Bulunanı
|Azure Data Lake Gen1 | Sahip | Desteklenmiyor
|Azure Data Lake Gen2 | Depolama Blob Verileri Okuyucusu | Depolama Blob Verileri Katkıda Bulunanı
|Azure Veri Gezgini Kümesi | Katkıda Bulunan | Katkıda Bulunan
|

SQL tabanlı paylaşım için, Azure SQL veritabanı 'nda Azure veri paylaşımı kaynağıyla aynı ada sahip bir dış sağlayıcıdan bir SQL kullanıcısının oluşturulması gerekir. Bu kullanıcıyı oluşturmak için yönetici izni Azure Active Directory gereklidir. SQL kullanıcısının gerektirdiği iznin özeti aşağıda verilmiştir.

|**SQL veritabanı türü**|**Veri Sağlayıcısı SQL Kullanıcı Izni**|**Veri tüketicisi SQL Kullanıcı Izni**|
|---|---|---|
|Azure SQL Veritabanı | db_datareader | db_datareader, db_datawriter, db_ddladmin
|Azure Synapse Analytics | db_datareader | db_datareader, db_datawriter, db_ddladmin
|

### <a name="data-provider"></a>Veri sağlayıcısı

Azure veri paylaşımında bir veri kümesi eklemek için, sağlayıcı veri paylaşımının kaynağına yönetilen kimliğin kaynak Azure veri deposuna erişim verilmesi gerekir. Örneğin, depolama hesabı durumunda, veri paylaşımının kaynak olarak yönetilen kimliği, Depolama Blobu veri okuyucusu rolüne sahiptir. 

Bu, Kullanıcı Azure portal aracılığıyla veri kümesi eklerken ve Kullanıcı uygun izne sahip olduğunda Azure veri paylaşma hizmeti tarafından otomatik olarak gerçekleştirilir. Örneğin, Kullanıcı Azure veri deposunun sahibidir veya Microsoft. Authorization/role atama/yazma izninin atandığı özel bir rolün üyesidir. 

Alternatif olarak, Kullanıcı Azure veri deposunun sahibine sahip olabilir. veri paylaşımının kaynak yönetilen kimliğini Azure veri deposuna el ile ekleyin. Bu eylemin veri paylaşma kaynağı başına yalnızca bir kez gerçekleştirilmesi gerekir.

Veri paylaşımının yönetilen kimliği için el ile bir rol ataması oluşturmak için aşağıdaki adımları izleyin.  

1. Azure veri deposuna gidin.
1. **Access Control (IAM)** seçeneğini belirleyin.
1. **Rol ataması Ekle**' yi seçin.
1. *Rol* altında, yukarıdaki rol atama tablosunda bulunan rolü seçin (örneğin, depolama hesabı Için, *Depolama Blobu veri okuyucusu*' nu seçin).
1. *Seç*' in altında, Azure veri paylaşma kaynağınızın adını yazın.
1. *Kaydet*’e tıklayın.

Rol atama hakkında daha fazla bilgi edinmek için [Azure Portal kullanarak Azure rol atamaları ekleme veya kaldırma](../role-based-access-control/role-assignments-portal.md#add-a-role-assignment)bölümüne bakın. REST API 'Lerini kullanarak veri paylaşıyorsanız, [REST API kullanarak Azure rol atamaları ekleme veya kaldırma ' ya](../role-based-access-control/role-assignments-rest.md)başvurarak API kullanarak rol ataması oluşturabilirsiniz. 

SQL tabanlı kaynaklar için SQL kullanıcısının SQL veritabanı 'nda Azure Active Directory kimlik doğrulaması kullanılarak SQL veritabanı 'na bağlanırken Azure veri paylaşma kaynağıyla aynı ada sahip bir dış sağlayıcıdan oluşturulması gerekir. Bu kullanıcıya *db_datareader* izni verilmesi gerekir. SQL tabanlı paylaşıma yönelik diğer önkoşullara birlikte örnek bir betik, [Azure SQL veritabanı veya Azure SYNAPSE Analytics](how-to-share-from-sql.md) öğreticisinde bulunabilir. 

### <a name="data-consumer"></a>Veri tüketicisi
Veri almak için, tüketici veri paylaşımının kaynağına ait yönetilen kimliğin hedef Azure veri deposuna erişim verilmesi gerekir. Örneğin, depolama hesabı durumunda, veri paylaşımının kaynak tarafından yönetilen kimliği, Depolama Blobu veri katılımcısı rolüne sahiptir. 

Kullanıcı Azure portal aracılığıyla bir hedef veri deposu belirtiyorsa ve Kullanıcı uygun izinlere sahipse, bu, Azure veri paylaşma hizmeti tarafından otomatik olarak gerçekleştirilir. Örneğin, Kullanıcı Azure veri deposunun sahibidir veya Microsoft. Authorization/role atama/yazma izni atanmış özel bir rolün üyesidir. 

Alternatif olarak, Kullanıcı Azure veri deposunun sahibine sahip olabilir. veri paylaşımının kaynak yönetilen kimliğini Azure veri deposuna el ile ekleyin. Bu eylemin veri paylaşma kaynağı başına yalnızca bir kez gerçekleştirilmesi gerekir.

Veri paylaşımının yönetilen kimliği için el ile bir rol ataması oluşturmak için aşağıdaki adımları izleyin. 

1. Azure veri deposuna gidin.
1. **Access Control (IAM)** seçeneğini belirleyin.
1. **Rol ataması Ekle**' yi seçin.
1. *Rol* altında, yukarıdaki rol atama tablosunda bulunan rolü seçin (örneğin, depolama hesabı Için, *Depolama Blobu veri okuyucusu*' nu seçin).
1. *Seç*' in altında, Azure veri paylaşma kaynağınızın adını yazın.
1. *Kaydet*’e tıklayın.

Rol atama hakkında daha fazla bilgi edinmek için [Azure Portal kullanarak Azure rol atamaları ekleme veya kaldırma](../role-based-access-control/role-assignments-portal.md#add-a-role-assignment)bölümüne bakın. REST API 'Lerini kullanarak veri alıyorsanız, [REST API kullanarak Azure rol atamaları Ekle veya Kaldır](../role-based-access-control/role-assignments-rest.md)' a başvurarak API kullanarak rol ataması oluşturabilirsiniz. 

SQL tabanlı hedef için, SQL veritabanı Azure Active Directory kimlik doğrulaması kullanılarak SQL veritabanı 'na bağlanırken Azure veri paylaşma kaynağıyla aynı ada sahip SQL veritabanı 'nda bir dış sağlayıcıdan oluşturulması gerekir. Bu kullanıcıya *db_datareader, db_datawriter db_ddladmin* izin verilmesi gerekir. SQL tabanlı paylaşıma yönelik diğer önkoşullara birlikte örnek bir betik, [Azure SQL veritabanı veya Azure SYNAPSE Analytics](how-to-share-from-sql.md) öğreticisinde bulunabilir. 

## <a name="resource-provider-registration"></a>Kaynak sağlayıcısı kaydı 

Aşağıdaki senaryolarda Microsoft. DataShare kaynak sağlayıcısını Azure aboneliğinize el ile kaydetmeniz gerekebilir: 

* Azure kiracınızda Azure Veri Paylaşma davetini ilk kez görüntüleyin
* Azure veri paylaşımından farklı bir Azure aboneliğindeki verileri Azure veri deposundan paylaşma
* Azure Data Share kaynağından farklı bir Azure aboneliğindeki verileri Azure veri deposuna alma

Microsoft. DataShare kaynak sağlayıcısını Azure aboneliğinize kaydetmek için aşağıdaki adımları izleyin. Kaynak sağlayıcısını kaydetmek için Azure aboneliğine *katkıda bulunan* erişime ihtiyacınız vardır.

1. Azure portal **abonelikler**' e gidin.
1. Azure veri paylaşımında kullandığınız aboneliği seçin.
1. **Kaynak sağlayıcıları**' na tıklayın.
1. Microsoft. DataShare için arama yapın.
1. **Kaydet**’e tıklayın.
 
Kaynak sağlayıcısı hakkında daha fazla bilgi edinmek için [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md)' ne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure 'da roller hakkında daha fazla bilgi edinin- [rol tanımlarını anlayın](../role-based-access-control/role-definitions.md)