---
title: RBAC kullanarak Azure portal panoları paylaşma | Microsoft Docs
description: Bu makalede rol tabanlı Access Control kullanarak Azure portal bir panonun nasıl paylaşılacağını açıklanmaktadır.
services: azure-portal
documentationcenter: ''
author: mgblythe
manager: mtillman
editor: tysonn
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2016
ms.author: mblythe
ms.openlocfilehash: ce94a35ebcbf5d8cf3d176f05ac75ea5da5d8d99
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76310756"
---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Rol Tabanlı Erişim Denetimi kullanarak Azure panolarını paylaşma

Bir panoyu yapılandırdıktan sonra yayımlayabilir ve kuruluşunuzdaki diğer kullanıcılarla paylaşabilirsiniz. Başkalarının Azure [rol tabanlı Access Control](../role-based-access-control/role-assignments-portal.md)kullanarak panonuzu görüntülemesine izin verebilirsiniz. Bir kullanıcı veya kullanıcı grubunu bir role atarsınız ve bu rol, kullanıcıların yayımlanan panoyu görüntüleyip görüntüleyemeyeceğini veya değiştiremeyeceklerini tanımlar. 

Yayımlanan tüm panolar Azure kaynakları olarak uygulanır. Bu, aboneliğinizde yönetilebilir öğeler olarak oldukları ve bir kaynak grubunda yer aldığı anlamına gelir.  Bir erişim denetimi perspektifinden, panolar, sanal makine veya depolama hesabı gibi diğer kaynaklardan farklı değildir.

> [!TIP]
> Panodaki tek Kutucuklar, görüntülenen kaynaklara göre kendi erişim denetimi gereksinimlerini zorunlu tutar.  Bu nedenle, yaygın olarak paylaşılan bir pano tasarlayabilirken, tek tek kutucuklarda verileri korurken.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Panolar için erişim denetimini anlama
Rol tabanlı Access Control (RBAC) ile, kullanıcıları rollere üç farklı kapsam düzeyinde atayabilirsiniz:

* aboneliği
* kaynak grubu
* resource

Atadığınız izinler abonelikten kaynağa kadar devralınır. Yayınlanan Pano bir kaynaktır. Bu nedenle, yayımlanmış Pano için de çalışan, aboneliğin rollerine atanmış kullanıcılarınız zaten olabilir. 

Bir örnek aşağıda verilmiştir.  Azure aboneliğiniz olduğunu ve takımınızın çeşitli üyelerine abonelik için **sahip**, **katkıda bulunan**veya **okuyucu** rollerinin atandığını varsayalım. Sahip veya katkıda bulunanlar olan kullanıcılar, abonelik içinde panoları listeleyebiliyor, görüntüleyebilir, oluşturabilir, değiştirebilir veya silebilir.  Okuyucular olan kullanıcılar panoları listeleyebilir ve görüntüleyebilir, ancak bunları değiştiremez veya silemez.  Okuyucu erişimi olan kullanıcılar yayımlanmış bir panoda (örneğin, bir sorun giderirken) yerel düzenlemeler yapabilir, ancak bu değişiklikleri sunucuya geri yayımlayamaz.  Bu kişiler için panonun özel bir kopyasını oluşturma seçeneği vardır

Bununla birlikte, çeşitli panolar veya tek bir Pano içeren kaynak grubuna izinler de atayabilirsiniz. Örneğin, bir Kullanıcı grubunun abonelik genelinde sınırlı izinleri olması ve belirli bir panoya daha fazla erişimi olması gerektiğine karar verebilirsiniz. Bu kullanıcıları bu pano için bir role atarsınız. 

## <a name="publish-dashboard"></a>Panoyu yayımlama
Aboneliğinizdeki bir kullanıcı grubuyla paylaşmak istediğiniz bir panoyu yapılandırmayı tamamladığınızı varsayalım. Aşağıdaki adımlarda, depolama yöneticileri adlı özelleştirilmiş bir grup verilmiştir ancak grubunuza istediğiniz her şeyi adlandırabilirsiniz. Active Directory grubu oluşturma ve bu gruba kullanıcı ekleme hakkında daha fazla bilgi için bkz. [Azure Active Directory grupları yönetme](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

1. Panoda, **paylaşma**' yı seçin.
   
     ![paylaşma Seç](./media/azure-portal-dashboard-share-access/select-share.png)
2. Erişim atamadan önce Panoyu yayımlamanız gerekir. Varsayılan olarak, pano, **panolar**adlı bir kaynak grubuna yayımlanır. **Yayımla**’yı seçin.
   
     ![publish](./media/azure-portal-dashboard-share-access/publish.png)

Panonuz artık yayımlandı. Abonelikten devralınan izinler uygunsa, daha fazla şey yapmanız gerekmez. Kuruluşunuzdaki diğer kullanıcılar, abonelik düzeyi rolüne göre panoya erişebilir ve panoyu değiştirebilir. Bununla birlikte, bu öğreticide, bir kullanıcı grubunu bu pano için bir role atayalim.

## <a name="assign-access-to-a-dashboard"></a>Panoya erişim atama
1. Panoyu yayımladıktan sonra **Kullanıcıları Yönet**' i seçin.
   
     ![Kullanıcıları yönetme](./media/azure-portal-dashboard-share-access/manage-users.png)
2. Bu Pano için zaten bir rolün atandığı mevcut kullanıcıların listesini görürsünüz. Mevcut kullanıcılar listeniz aşağıdaki görüntüden farklı olacaktır. Büyük olasılıkla, atamalar abonelikten devralınır. Yeni bir kullanıcı veya grup eklemek için **Ekle**' yi seçin.
   
     ![Kullanıcı Ekle](./media/azure-portal-dashboard-share-access/existing-users.png)
3. Vermek istediğiniz izinleri temsil eden rolü seçin. Bu örnek için **katkıda bulunan**' ı seçin.
   
     ![Rol Seç](./media/azure-portal-dashboard-share-access/select-role.png)
4. Role atamak istediğiniz kullanıcı veya grubu seçin. Listede Aradığınız kullanıcıyı veya grubu görmüyorsanız, arama kutusunu kullanın. Kullanılabilir gruplar listeniz, Active Directory oluşturduğunuz gruplara bağlıdır.
   
     ![Kullanıcı Seç](./media/azure-portal-dashboard-share-access/select-user.png) 
5. Kullanıcı veya grup eklemeyi bitirdiğinizde **Tamam**' ı seçin. 
6. Yeni atama, kullanıcı listesine eklenir. **Erişiminin** **Devralındığı**yerine **atanmış** olarak listelendiğini unutmayın.
   
     ![atanan roller](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## <a name="next-steps"></a>Sonraki adımlar
* Rollerin listesi için bkz. [RBAC: yerleşik roller](../role-based-access-control/built-in-roles.md).
* Kaynakları yönetme hakkında bilgi edinmek için bkz. [Azure kaynaklarını Portal üzerinden yönetme](resource-group-portal.md).

