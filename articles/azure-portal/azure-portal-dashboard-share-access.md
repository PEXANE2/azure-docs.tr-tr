---
title: Rol Tabanlı Erişim Denetimi'ni kullanarak Azure portal panolarını paylaşın
description: Bu makalede, Rol Tabanlı Erişim Denetimi'ni kullanarak Azure portalında bir pano nasıl paylaşılalışekilde açıklanmaktadır.
services: azure-portal
documentationcenter: ''
author: mgblythe
manager: mtillman
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 03/23/2020
ms.author: mblythe
ms.openlocfilehash: 4eef5a9e3f010e19871471d007ff2a0cc24d3834
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461387"
---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Rol Tabanlı Erişim Denetimi kullanarak Azure panolarını paylaşma

Bir panoyu yapılandırıldıktan sonra yayımlayabilir ve kuruluşunuzdaki diğer kullanıcılarla paylaşabilirsiniz. Azure [Role Tabanlı Erişim Denetimi](../role-based-access-control/role-assignments-portal.md) 'ni (RBAC) kullanarak başkalarının panonuzu görüntülemesine izin verirsiniz. Bir role bir kullanıcı veya kullanıcı grubu atama. Bu rol, bu kullanıcıların yayımlanmış panoyu görüntüleyip görüntüleyemeyeceğini veya değiştirip değiştiremeyeceğini tanımlar.

Yayınlanan tüm panolar Azure kaynakları olarak uygulanır. Bunlar aboneliğinizde yönetilebilir öğeler olarak bulunur ve bir kaynak grubunda bulunur. Erişim denetimi açısından bakıldığında, panolar sanal makine veya depolama hesabı gibi diğer kaynaklardan farklı değildir.

> [!TIP]
> Panodaki tek tek döşeme, görüntüledikleri kaynaklara bağlı olarak kendi erişim denetimi gereksinimlerini uygular. Tek tek döşemelerle ilgili verileri korurken bir panoyu geniş ölçüde paylaşabilirsiniz.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Panolar için erişim denetimini anlama

Role-Based Access Control (RBAC) ile, kullanıcıları üç farklı kapsam düzeyindeki rollere atayabilirsiniz:

* aboneliği
* kaynak grubu
* kaynak

Atadığınız izinler, kaynak için abonelikten devralır. Yayımlanmış pano bir kaynaktır. Yayımlanmış pano için geçerli olan abonelik için rollere atanmış kullanıcılar zaten olabilir.

Bir Azure aboneliğiniz olduğunu ve ekibinizin çeşitli üyelerine abonelik için *sahip*, *katkıda bulunan*veya *okuyucu* rolleri atandığını varsayalım. Sahip veya katkıda bulunan kullanıcılar abonelik içindeki panoları listeleyebilir, görüntüleyebilir, oluşturabilir, değiştirebilir veya silebilir. Okuyucu olan kullanıcılar panoları listeleyebilir ve görüntüleyebilir, ancak bunları değiştiremez veya silemez. Okuyucu erişimi olan kullanıcılar, sorun giderme gibi yayımlanmış bir panoda yerel düzenleme yapabilir, ancak bu değişiklikleri sunucuya geri yayımlayamazlar. Onlar kendileri için pano özel bir kopyasını yapabilirsiniz.

Ayrıca, birkaç pano içeren kaynak grubuna veya tek bir panoya izin atayabilirsiniz. Örneğin, bir kullanıcı grubunun abonelik genelinde sınırlı izinlere sahip olmasına, ancak belirli bir panoya daha fazla erişmeye karar verebilirsiniz. Bu kullanıcıları bu pano için bir role atayın.

## <a name="publish-dashboard"></a>Panoyu yayımlama

Aboneliğinizdeki bir kullanıcı grubuyla paylaşmak istediğiniz bir pano yapılandırdığınızı varsayalım. Aşağıdaki adımlar, bir panoyu Depolama Yöneticileri adlı bir grupla nasıl paylaşılabildiğini gösterir. Grubuna istediğiniz ismi verebilirsiniz. Daha fazla bilgi için Azure [Etkin Dizini'ndeki grupları yönetme'ye](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)bakın.

Erişim atamadan önce, panoyu yayımlamanız gerekir.

1. Panoda **Paylaş'ı**seçin.

    ![panonuz için paylaşımı seçin](./media/azure-portal-dashboard-share-access/share-dashboard-for-access-control.png)

1. **Paylaşım + erişim denetiminde,** **Yayımla'yı**seçin.

    ![panonuzu yayımlayın](./media/azure-portal-dashboard-share-access/publish-dashboard-for-access-control.png)

     Varsayılan olarak, paylaşım **panonuzu pano**adı verilen bir kaynak grubuna yayımlar. Farklı bir kaynak grubu seçmek için onay kutusunu temizleyin.

Panonuz artık yayınlandı. Abonelikten devralınan izinler uygunsa, daha fazla bir şey yapmanız gerekmez. Kuruluşunuzdaki diğer kullanıcılar, abonelik düzeyi rollerine bağlı olarak panoya erişebilir ve panoyu değiştirebilir.

## <a name="assign-access-to-a-dashboard"></a>Panoya erişim atama

Bu pano için bir rol için bir kullanıcı grubu atayabilirsiniz.

1. Panoyu yayımladıktan sonra Paylaşım + erişim **denetimine**erişmek için **Paylaş** veya **Paylaş'ı Paylaş** seçeneğini seçin.

1. **Paylaşım + erişim denetiminde,** kullanıcıları **yönet'i**seçin.

    ![bir pano için kullanıcıları yönetme](./media/azure-portal-dashboard-share-access/manage-users-for-access-control.png)

1. Bu pano için zaten bir rol atanmış varolan kullanıcıları görmek için **Rol atamaları'nı** seçin.

1. Yeni bir kullanıcı veya grup eklemek için Ekle **ve** ardından **rol ataması ekle'yi**seçin.

    ![panoya erişmek için kullanıcı ekleme](./media/azure-portal-dashboard-share-access/manage-users-existing-users.png)

1. Verilecek izinleri temsil eden rolü seçin. Bu **örnekiçin, Katılımcı'yı**seçin.

1. Role atamak için kullanıcıyı veya grubu seçin. Listede aradığınız kullanıcıyı veya grubu görmüyorsanız, arama kutusunu kullanın. Kullanılabilir gruplar listeniz, Etkin Dizini'nde oluşturduğunuz gruplara bağlıdır.

1. Kullanıcı veya grup eklemeyi bitirdikten sonra **Kaydet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Rollerin listesi için Azure [kaynakları için Yerleşik rollere](../role-based-access-control/built-in-roles.md)bakın.
* Kaynakları yönetme hakkında bilgi edinmek için [Azure portalını kullanarak Azure kaynaklarını yönet'e](resource-group-portal.md)bakın.

