---
title: Rol tabanlı Access Control kullanarak Azure portal panoları paylaşma
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
ms.date: 01/29/2020
ms.author: mblythe
ms.openlocfilehash: e8d251cef9e67cb8fc0c11df8ce546383f75a679
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76900818"
---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Rol tabanlı Access Control kullanarak Azure panoları paylaşma

Bir panoyu yapılandırdıktan sonra yayımlayabilir ve kuruluşunuzdaki diğer kullanıcılarla paylaşabilirsiniz. Başkalarının Azure [rol tabanlı Access Control](../role-based-access-control/role-assignments-portal.md) (RBAC) kullanarak panonuzu görüntülemesine izin verebilirsiniz. Bir role Kullanıcı veya Kullanıcı grubu atayın. Bu rol, bu kullanıcıların yayınlanan panoyu görüntüleyip görüntüleyemeyeceğini veya değiştiremeyeceklerini tanımlar.

Yayımlanan tüm panolar Azure kaynakları olarak uygulanır. Abonelikleriniz içinde yönetilebilir öğeler olarak mevcuttur ve bir kaynak grubunda yer alır. Bir erişim denetimi perspektifinden, panolar, sanal makine veya depolama hesabı gibi diğer kaynaklardan farklı değildir.

> [!TIP]
> Panodaki tek Kutucuklar, görüntülenen kaynaklara göre kendi erişim denetimi gereksinimlerini zorunlu tutar. Tek tek bölmelerde verileri korurken bir panoyu büyük ölçüde paylaşabilirsiniz.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Panolar için erişim denetimini anlama

Rol tabanlı Access Control (RBAC) ile, kullanıcıları rollere üç farklı kapsam düzeyinde atayabilirsiniz:

* aboneliği
* kaynak grubu
* resource

Atadığınız izinler abonelikten kaynağa kadar devralınır. Yayınlanan Pano bir kaynaktır. Yayımlanmış Pano için uygulanan aboneliğin rollerine atanmış kullanıcılarınız zaten olabilir.

Azure aboneliğiniz olduğunu ve takımınızın çeşitli üyelerine abonelik için *sahip*, *katkıda bulunan*veya *okuyucu* rollerinin atandığını varsayalım. Sahip veya katkıda bulunanlar olan kullanıcılar, abonelik içinde panoları listeleyebilir, görüntüleyebilir, oluşturabilir, değiştirebilir veya silebilir. Okuyucular olan kullanıcılar panoları listeleyebilir ve görüntüleyebilir, ancak bunları değiştiremez veya silemez. Okuyucu erişimi olan kullanıcılar, yayımlanan bir panoda bir sorunu giderirken, ancak bu değişiklikleri sunucuya geri yayımlayamazlar. Bunlar için panonun özel bir kopyasını oluşturabilir.

Ayrıca, çeşitli panolar veya ayrı bir Pano içeren kaynak grubuna izinler atayabilirsiniz. Örneğin, bir Kullanıcı grubunun abonelik genelinde sınırlı izinleri olması ve belirli bir panoya daha fazla erişimi olması gerektiğine karar verebilirsiniz. Bu kullanıcıları bu pano için bir role atayın.

## <a name="publish-dashboard"></a>Panoyu yayımlama

Aboneliğinizdeki bir kullanıcı grubuyla paylaşmak istediğiniz bir panoyu yapılandıralım. Aşağıdaki adımlarda, bir panonun depolama yöneticileri adlı bir grup ile nasıl paylaşılacağını gösterilmektedir. Grubunuzu dilediğiniz gibi adlandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory grupları yönetme](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Erişim atamadan önce Panoyu yayımlamanız gerekir.

1. Panoda, **paylaşma**' yı seçin.

    ![Panonuz için paylaşma seçin](./media/azure-portal-dashboard-share-access/share-dashboard-for-access-control.png)

1. **Paylaşım ve erişim denetimi**' nde **Yayımla**' yı seçin.

    ![panonuzu yayımlayın](./media/azure-portal-dashboard-share-access/publish-dashboard-for-access-control.png)

     Varsayılan olarak, paylaşım panonuzu **panolar**adlı bir kaynak grubuna yayınlar.

Panonuz artık yayımlandı. Abonelikten devralınan izinler uygunsa, daha fazla şey yapmanız gerekmez. Kuruluşunuzdaki diğer kullanıcılar, abonelik düzeyi rolüne göre panoya erişebilir ve panoyu değiştirebilir.

## <a name="assign-access-to-a-dashboard"></a>Panoya erişim atama

Bu Pano için bir kullanıcı grubunu bir role atayabilirsiniz.

1. Panoyu yayımladıktan sonra, paylaşım ve **erişim denetimi**' nde **Kullanıcıları Yönet**' i seçin.

    ![kullanıcıları bir Pano için yönetme](./media/azure-portal-dashboard-share-access/manage-users-for-access-control.png)

    Bir panodan **Paylaşım + erişim denetimine** erişmek için **Paylaşım** veya paylaşımı **Kaldır** seçeneğini belirleyin.

1. Bu Pano için zaten bir rol atanmış var olan kullanıcıları görmek için **rol atamaları** ' nı seçin.

1. Yeni bir kullanıcı veya grup eklemek için **Ekle**' yi seçin.

    ![panoya erişim için Kullanıcı ekleme](./media/azure-portal-dashboard-share-access/manage-users-existing-users.png)

1. Verilecek izinleri temsil eden rolü seçin. Bu örnek için **katkıda bulunan**' ı seçin.

1. Role atanacak Kullanıcı veya grubu seçin. Listede Aradığınız kullanıcıyı veya grubu görmüyorsanız arama kutusunu kullanın. Kullanılabilir gruplar listeniz, Active Directory oluşturduğunuz gruplara bağlıdır.

1. Kullanıcı veya grup eklemeyi bitirdiğinizde **Tamam**' ı seçin.

    Yeni atama, kullanıcı listesine eklenir. **Erişimi** , **Devralındığı**yerine **atanmış** olarak listelenir.

    ![atanan roller](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## <a name="next-steps"></a>Sonraki adımlar

* Rollerin listesi için bkz. [Azure kaynakları Için yerleşik roller](../role-based-access-control/built-in-roles.md).
* Kaynakları yönetme hakkında bilgi edinmek için, bkz. [Azure Portal kullanarak Azure kaynaklarını yönetme](resource-group-portal.md).

