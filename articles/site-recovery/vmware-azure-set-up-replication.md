---
title: Azure Site Kurtarma ile VMware olağanüstü durum kurtarma için çoğaltma ilkeleri ayarlama| Microsoft Dokümanlar
description: Azure Site Kurtarma ile VMware olağanüstü durum kurtarma için çoğaltma ayarlarını Azure'a nasıl yapılandırıştırılabildiğini açıklar.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: sutalasi
ms.openlocfilehash: 45921bdf802a649b7b802f44d2842a543e44f02b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257127"
---
# <a name="configure-and-manage-replication-policies-for-vmware-disaster-recovery"></a>VMware olağanüstü durum kurtarma için çoğaltma ilkelerini yapılandırma ve yönetme

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md)kullanarak VMware VM'leri Azure'a kopyalarken çoğaltma ilkesinin nasıl yapılandırılabildiğini açıklanmaktadır.

## <a name="create-a-policy"></a>İlke oluşturma

1. **Site Kurtarma Altyapılarını** **Yönet'i** > seçin.
2. **VMware ve Fiziksel makineler için**Çoğaltma **ilkelerini**seçin.
3. **+Çoğaltma ilkesini**tıklatın ve ilke adını belirtin.
4. **RPO eşiğinde,** RPO sınırını belirtin. Sürekli çoğaltma bu sınırı aştığında uyarılar oluşturulur.
5. **Kurtarma noktası bekletme** kısmında, her kurtarma noktası için bekletme süresini saat cinsinden belirtin. Korumalı makineler, bekletme penceresi içindeki herhangi bir noktaya kurtarılabilir. Premium depolama alanına çoğaltılan makineler için 24 saate kadar bekletme desteklenir. Standart depolama için 72 saate kadar desteklenir.
6. **Uygulama tutarlı anlık sıklıktan,** uygulama tutarlı anlık görüntüler içeren kurtarma noktalarının ne sıklıkta (saatlerde) oluşturulması gerektiğini açılır noktadan seçin. Uygulama tutarlılık noktalarının oluşturmasını kapatmak istiyorsanız, açılır açılır durumda "Kapalı" değerini seçin.
7. **Tamam**'a tıklayın. İlke, 30 ila 60 saniye içinde oluşturulur.

Bir çoğaltma ilkesi oluşturduğunuzda, eşleşen bir geri dönüş çoğaltma ilkesi otomatik olarak oluşturulur ve "failback" soneki yle birlikte. İlkeyi oluşturduktan sonra, **ayarları**düzenleme > seçerek bu politikayı değiştirebilirsiniz.

## <a name="associate-a-configuration-server"></a>Yapılandırma sunucusuilişkilendirme

Çoğaltma ilkesini şirket içi yapılandırma sunucunuzla ilişkilendirin.

1. **Ilişkilendir'i**tıklatın ve yapılandırma sunucusunu seçin.

    ![Yapılandırma sunucusunu ilişkilendirme](./media/vmware-azure-set-up-replication/associate1.png)
2. **Tamam**'a tıklayın. Yapılandırma sunucusu, bir ila iki dakika içinde ilişkilendirilir.

    ![Yapılandırma sunucusu ilişkilendirme](./media/vmware-azure-set-up-replication/associate2.png)

## <a name="edit-a-policy"></a>İlkeyi edin

Bir çoğaltma ilkesini oluşturduktan sonra değiştirebilirsiniz.

- İlkedeki değişiklikler, ilkeyi kullanan tüm makinelere uygulanır.
- Çoğaltılan makineleri farklı bir çoğaltma ilkesiyle ilişkilendirmek istiyorsanız, ilgili makineler için korumayı devre dışı bırakıp yeniden etkinleştirmeniz gerekir.

Bir ilkeyi aşağıdaki gibi edin:
1. **Site Kurtarma Altyapısı** > **Çoğaltma İlkelerini** **Yönet'i** > seçin.
2. Değiştirmek istediğiniz çoğaltma ilkesini seçin.
3. **Ayarları Edit'i**tıklatın ve RPO eşiğini/kurtarma noktası bekletme saatlerini/uygulama tutarlı anlık görüntü sıklığı alanlarını gerektiği gibi güncelleştirin.
4. Uygulama tutarlılık noktalarının oluşturmasını kapatmak istiyorsanız, alan App tutarlı anlık görüntü **frekansının**açılır damlasında "Kapalı" değerini seçin.
5. **Kaydet**'e tıklayın. İlke 30 ila 60 saniye içinde güncelleştirilmelidir.



## <a name="disassociate-or-delete-a-replication-policy"></a>Çoğaltma ilkesini ilişkilendirme veya silme

1. Çoğaltma ilkesini seçin.
    a. İlkeyi yapılandırma sunucusundan ayrıştırmak için, çoğaltılan makinelerin ilkeyi kullanmadığından emin olun. Ardından, **Dissociate'yi**tıklatın.
    b. İlkeyi silmek için, bir yapılandırma sunucusuyla ilişkili olmadığından emin olun. Ardından **Sil'i**tıklatın. Silmek 30-60 saniye sürer.
2. **Tamam**'a tıklayın.
