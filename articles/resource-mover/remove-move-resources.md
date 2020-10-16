---
title: Azure Kaynak taşıyıcısı içindeki bir taşıma koleksiyonundan kaynakları kaldırma
description: Azure Kaynak taşıyıcısı içindeki bir taşıma koleksiyonundan kaynakları kaldırmayı öğrenin.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/08/2020
ms.author: raynew
ms.openlocfilehash: 38a633a7a11ac29271231679e7075920e1f33a70
ms.sourcegitcommit: ba7fafe5b3f84b053ecbeeddfb0d3ff07e509e40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2020
ms.locfileid: "91945952"
---
# <a name="manage-move-collections-and-resource-groups"></a>Taşıma koleksiyonlarını ve kaynak gruplarını yönetme

Bu makalede, [Azure Kaynak taşıyıcısı](overview.md)' de bir taşıma koleksiyonundan kaynakları kaldırma veya taşıma koleksiyonu/kaynak grubunu kaldırma açıklanır. Azure bölgeleri arasında Azure kaynakları taşırken taşıma koleksiyonları kullanılır.

## <a name="remove-a-resource-portal"></a>Kaynak kaldırma (portal)

Kaynak taşıyıcısı portalındaki bir taşıma koleksiyonundaki kaynakları şu şekilde kaldırabilirsiniz:

1. **Bölgeler arasında**, koleksiyondan kaldırmak istediğiniz tüm kaynakları seçin ve **Kaldır**' ı seçin. 

    ![Kaldırmak için seçilecek düğme](./media/remove-move-resources/portal-select-resources.png)

2. **Kaynakları kaldır**' da **Kaldır**' a tıklayın.

    ![Bir taşıma koleksiyonundan kaynakları kaldırmak için seçilecek düğme](./media/remove-move-resources/remove-portal.png)

## <a name="remove-a-move-collectionresource-group-portal"></a>Bir taşıma koleksiyonunu/kaynak grubunu kaldırma (portal)

Portalda bir taşıma koleksiyonunu/kaynak grubunu kaldırabilirsiniz.

1. Koleksiyondan kaynakları kaldırmak için yukarıdaki yordamdaki yönergeleri izleyin. Bir kaynak grubunu kaldırıyorsanız, kaynağın hiç kaynak içermediğinden emin olun.
2. Taşıma koleksiyonunu veya kaynak grubunu silin.  

## <a name="remove-a-resource-powershell"></a>Kaynak kaldırma (PowerShell)

PowerShell kullanarak bir kaynağı (bizim örneğimizde PSDemoVM makineleri) aşağıdaki gibi kaldırın:

```azurepowershell-interactive
# Remove a resource using the resource ID
Remove-AzResourceMoverMoveResource -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus - Name PSDemoVM
```
**Beklenen çıkış** 
 ![ Taşıma koleksiyonundan kaynak kaldırıldıktan sonra çıkış metni](./media/remove-move-resources/remove-resource.png)



## <a name="remove-a-collection-powershell"></a>Bir koleksiyonu kaldırma (PowerShell)

PowerShell kullanarak tüm taşıma koleksiyonunu aşağıdaki gibi kaldırın:

1. PowerShell kullanarak koleksiyondaki kaynakları kaldırmak için yukarıdaki yönergeleri izleyin.
2. Çalıştır:

    ```azurepowershell-interactive
    # Remove a resource using the resource ID
    Remove-AzResourceMoverMoveResource -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus 
    ```
    **Beklenen çıkış** ![ Taşıma koleksiyonu kaldırıldıktan sonra çıkış metni](./media/remove-move-resources/remove-collection.png)

## <a name="vm-resource-state-after-removing"></a>Kaldırdıktan sonra VM kaynağı durumu

Bir taşıma koleksiyonundan bir VM kaynağını kaldırdığınızda ne olacağı, tabloda özetlenen kaynak durumuna bağlıdır.

###  <a name="remove-vm-state"></a>VM durumunu kaldır
**Kaynak durumu** | **'Nın** | **Ağ**
--- | --- | --- 
**Taşıma koleksiyonuna eklendi** | Taşıma koleksiyonundan Sil. | Taşıma koleksiyonundan Sil. 
**Bağımlılıklar çözümlendi/hazırlık bekliyor** | Taşıma koleksiyonundan Sil  | Taşıma koleksiyonundan Sil. 
**Hazırlama devam ediyor**<br/> (veya devam eden başka bir durum) | Silme işlemi hata vererek başarısız oldu.  | Silme işlemi hata vererek başarısız oldu.
**Hazırlama başarısız oldu** | Taşıma koleksiyonundan silin.<br/>Çoğaltma diskleri dahil olmak üzere hedef bölgede oluşturulan her şeyi silin. <br/><br/> Taşıma sırasında oluşturulan altyapı kaynaklarının el ile silinmesi gerekir. | Taşıma koleksiyonundan silin.  
**Taşımayı başlatma beklemede** | Taşıma koleksiyonundan Sil.<br/><br/> VM, çoğaltma diskleri vb. dahil olmak üzere hedef bölgede oluşturulan her şeyi silin.  <br/><br/> Taşıma sırasında oluşturulan altyapı kaynaklarının el ile silinmesi gerekir. | Taşıma koleksiyonundan Sil.
**Taşıma başlatılamadı** | Taşıma koleksiyonundan Sil.<br/><br/> VM, çoğaltma diskleri vb. dahil olmak üzere hedef bölgede oluşturulan her şeyi silin.  <br/><br/> Taşıma sırasında oluşturulan altyapı kaynaklarının el ile silinmesi gerekir. | Taşıma koleksiyonundan Sil.
**Tamamlama bekleniyor** | Hedef kaynakların önce silinmesi için taşımayı atmenizi öneririz.<br/><br/> Kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz. | Hedef kaynakların önce silinmesi için taşımayı atmenizi öneririz.<br/><br/> Kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz. 
**Kayıt başarısız oldu** | Önce hedef kaynakların silinmesi için öğesini atmenizi öneririz.<br/><br/> Kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz. | Hedef kaynakların önce silinmesi için taşımayı atmenizi öneririz.<br/><br/> Kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz.
**Atma tamamlandı** | Kaynak, **başlatma taşıma bekleme** durumuna geri döner.<br/><br/> Hedef VM, çoğaltma diskleri, kasa vb. üzerinde oluşturulan her türlü şeyle birlikte taşıma koleksiyonundan silinir.  <br/><br/> Taşıma sırasında oluşturulan altyapı kaynaklarının el ile silinmesi gerekir. <br/><br/> Taşıma sırasında oluşturulan altyapı kaynaklarının el ile silinmesi gerekir. |  Kaynak, **başlatma taşıma bekleme** durumuna geri döner.<br/><br/> Taşıma koleksiyonundan silinir.
**Atma başarısız oldu** | Hedef kaynakları önce silinmeden önce, taşımaları atlanmasını öneririz.<br/><br/> Bundan sonra kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz. | Hedef kaynakları önce silinmeden önce, taşımaları atlanmasını öneririz.<br/><br/> Bundan sonra kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz.
**Kaynağı silme bekleniyor** | Taşıma koleksiyonundan silinir.<br/><br/> Hedef bölgede oluşturulan herhangi bir şeyi silmez.  | Taşıma koleksiyonundan silinir.<br/><br/> Hedef bölgede oluşturulan herhangi bir şeyi silmez.
**Kaynak silme başarısız** | Taşıma koleksiyonundan silinir.<br/><br/> Hedef bölgede oluşturulan herhangi bir şeyi silmez. | Taşıma koleksiyonundan silinir.<br/><br/> Hedef bölgede oluşturulan herhangi bir şeyi silmez.

## <a name="sql-resource-state-after-removing"></a>Kaldırdıktan sonra SQL kaynak durumu

Bir Azure SQL kaynağını bir taşıma koleksiyonundan kaldırdığınızda ne olacağı, tabloda özetlenen kaynak durumuna bağlıdır.

**Kaynak durumu** | **SQL** 
--- | --- 
**Taşıma koleksiyonuna eklendi** | Taşıma koleksiyonundan Sil. 
**Bağımlılıklar çözümlendi/hazırlık bekliyor** | Taşıma koleksiyonundan Sil 
**Hazırlama devam ediyor**<br/> (veya devam eden başka bir durum)  | Silme işlemi hata vererek başarısız oldu. 
**Hazırlama başarısız oldu** | Taşıma koleksiyonundan Sil<br/><br/>Hedef bölgede oluşturulan her şeyi silin. 
**Taşımayı başlatma beklemede** |  Taşıma koleksiyonundan Sil<br/><br/>Hedef bölgede oluşturulan her şeyi silin. SQL veritabanı bu noktada bulunur ve silinecek. 
**Taşıma başlatılamadı** | Taşıma koleksiyonundan Sil<br/><br/>Hedef bölgede oluşturulan her şeyi silin. SQL veritabanı bu noktada var ve silinmelidir. 
**Tamamlama bekleniyor** | Hedef kaynakların önce silinmesi için taşımayı atmenizi öneririz.<br/><br/> Kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz.
**Kayıt başarısız oldu** | Hedef kaynakların önce silinmesi için taşımayı atmenizi öneririz.<br/><br/> Kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz. 
**Atma tamamlandı** |  Kaynak, **başlatma taşıma bekleme** durumuna geri döner.<br/><br/> SQL veritabanları da dahil olmak üzere, hedefte oluşturulan her türlü şeyle birlikte taşıma koleksiyonundan silinir. 
**Atma başarısız oldu** | Hedef kaynakları önce silinmeden önce, taşımaları atlanmasını öneririz.<br/><br/> Bundan sonra kaynak, **başlatma taşıma bekleme** durumuna geri döner ve buradan devam edebilirsiniz. 
**Kaynağı silme bekleniyor** | Taşıma koleksiyonundan silinir.<br/><br/> Hedef bölgede oluşturulan herhangi bir şeyi silmez. 
**Kaynak silme başarısız** | Taşıma koleksiyonundan silinir.<br/><br/> Hedef bölgede oluşturulan herhangi bir şeyi silmez. 

## <a name="next-steps"></a>Sonraki adımlar

Kaynak taşıyıcısı ile [BIR VM](tutorial-move-region-virtual-machines.md) 'yi başka bir bölgeye taşımayı deneyin.