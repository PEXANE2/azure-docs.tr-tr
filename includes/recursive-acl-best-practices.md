---
title: include dosyası
description: include dosyası
services: storage
author: normesta
ms.service: storage
ms.topic: include
ms.date: 02/03/2021
ms.author: normesta
ms.custom: include file
ms.openlocfilehash: b0be074c995fcc62f63a4a2ebf4149d3040e7b6b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750698"
---
## <a name="best-practices"></a>En iyi uygulamalar

Bu bölümde, ACL 'Leri yinelemeli olarak ayarlamaya yönelik bazı en iyi yöntem yönergeleri sunulmaktadır. 

#### <a name="handling-runtime-errors"></a>Çalışma zamanı hatalarını işleme

Birçok nedenden dolayı bir çalışma zamanı hatası oluşabilir (örneğin: bir kesinti veya bir istemci bağlantı sorunu). Bir çalışma zamanı hatasıyla karşılaşırsanız, özyinelemeli ACL işlemini yeniden başlatın. ACL 'Ler, olumsuz bir etkiye neden olmadan öğelere yeniden uygulanabilir. 

#### <a name="handling-permission-errors-403"></a>İzin hatalarını işleme (403)

Özyinelemeli bir ACL işlemi çalıştırırken bir erişim denetimi özel durumuyla karşılaşırsanız, AD [güvenlik sorumlusu](../articles/role-based-access-control/overview.md#security-principal) , Dizin hiyerarşisindeki bir veya daha fazla alt öğeye bir ACL uygulamak için yeterli izne sahip olmayabilir. Bir izin hatası oluştuğunda, işlem duraklar ve devamlılık belirteci sağlanır. İzin sorununu giderip kalan veri kümesini işlemek için devamlılık belirtecini kullanın. Zaten başarıyla işlenen dizinlerin ve dosyaların yeniden işlenmesi gerekmez. Özyinelemeli ACL işlemini de yeniden başlatmayı seçebilirsiniz. ACL 'Ler, olumsuz bir etkiye neden olmadan öğelere yeniden uygulanabilir. 

#### <a name="credentials"></a>Kimlik bilgileri 

Hedef depolama hesabı veya kapsayıcısının kapsamındaki [Depolama Blobu veri sahibi](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner) rolü atanmış BIR Azure AD güvenlik sorumlusu sağlamanızı öneririz. 

#### <a name="performance"></a>Performans 

Gecikme süresini azaltmak için, özyinelemeli ACL işlemini depolama hesabınızla aynı bölgede bulunan bir Azure sanal makinesinde (VM) çalıştırmanızı öneririz. 

#### <a name="acl-limits"></a>ACL sınırları

Bir dizin veya dosyaya uygulayabileceğiniz en fazla ACL sayısı 32 erişim ACL 'Lerine ve 32 varsayılan ACL 'Lerine sahiptir. Daha fazla bilgi için bkz. [Azure Data Lake Storage 2. Nesil'de erişim denetimi](../articles/storage/blobs/data-lake-storage-access-control.md).