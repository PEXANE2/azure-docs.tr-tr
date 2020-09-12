---
title: Azure Kaynak taşıyıcısı ile bölgeler arasında Azure VM 'Leri taşırken hedef ayarları değiştirme
description: Azure Kaynak taşıyıcısı ile bölgeler arasında Azure VM 'Leri taşırken hedef ayarları değiştirmeyi öğrenin.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: c58fdd38c4221c03778b2c769620bf7911aaded8
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89670780"
---
# <a name="modify-target-settings"></a>Hedef ayarlarını değiştir

Bu makalede, [Azure Kaynak taşıyıcısı](overview.md)ile Azure bölgeleri arasında kaynakları taşırken hedef ayarların nasıl değiştirileceği açıklanır.


## <a name="modify-vm-settings"></a>VM ayarlarını değiştir

Azure VM 'lerini ve ilişkili kaynakları taşırken, hedef ayarları değiştirebilirsiniz. 

- Hedef ayarlarını yalnızca taşıma koleksiyonu doğrulandıktan sonra değiştirmenizi öneririz.
- Hazırlama işlemi tamamlandıktan sonra bazı hedef özellikleri düzenleme için kullanılamaz olabileceğinden, kaynakları hazırlamadan önce ayarları değiştirmenizi öneririz.

Ancak
- Kaynak kaynağı taşıyorsanız, genellikle taşıma işlemini başlat işlemini başlatana kadar hedef ayarları değiştirebilirsiniz.
- Kaynak bölgede var olan bir kaynağı atarsanız, taşıma işlemi tamamlanana kadar hedef ayarlarını değiştirebilirsiniz.

### <a name="settings-you-can-modify"></a>Değiştirebileceğiniz ayarlar

Değiştirebileceğiniz yapılandırma ayarları tabloda özetlenmiştir.

**Kaynak** | **Seçenekler** 
--- | --- | --- 
**VM adı** | Seçenekler:<br/><br/> -Hedef bölgede aynı ada sahip yeni bir VM oluşturun.<br/><br/> -Hedef bölgede farklı bir ada sahip yeni bir VM oluşturun.<br/><br/> -Hedef bölgede var olan bir VM 'yi kullanın.<br/><br/> Değişiklik yaptığınız ayarların özel durumu ile yeni bir sanal makine oluşturursanız, yeni hedef VM kaynakla aynı ayarlara atanır.
**VM kullanılabilirlik bölgesi** | Hedef VM 'nin yerleştirileceği kullanılabilirlik bölgesi. Bu, kaynak ayarlarını değiştirmek istemiyorsanız veya VM 'yi bir kullanılabilirlik alanına yerleştirmek istemiyorsanız, **yok** olarak işaretlenebilir.
**VM SKU** | Hedef VM için kullanılacak [VM türü](https://azure.microsoft.com/pricing/details/virtual-machines/series/) (hedef bölgede kullanılabilir).<br/><br/> Seçilen hedef VM, kaynak VM 'den küçük olmamalıdır.
**Ağ kaynakları** | Sanal ağlar (VNet) için seçenekler/ağ güvenlik grupları/ağ arabirimleri:<br/><br/> -Hedef bölgede aynı ada sahip yeni bir kaynak oluşturun.<br/><br/> -Hedef bölgede farklı bir ada sahip yeni bir kaynak oluşturun.<br/><br/> -Hedef bölgede var olan bir ağ kaynağını kullanın.<br/><br/> Yeni bir hedef kaynak oluşturursanız, değiştirdiğiniz ayarların dışında, kaynak kaynakla aynı ayarları atanır.
**Genel IP adresi adı** | Adı belirtin.
**Genel IP adresi SKU 'SU** | [SKU 'yu](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#sku)belirtin.
**Genel IP adresi bölgesi** | Standart genel IP adresleri için [bölgeyi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#standard) belirtin.<br/><br/> Bölgenin yedekli olmasını istiyorsanız **bölge**olarak girin.
**Yük dengeleyici adı** | Adı belirtin.
**Yük dengeleyici SKU 'SU** | Temel veya standart. Standart kullanmanızı öneririz.
**Yük dengeleyici bölgesi** | Yük Dengeleyici için bir bölge belirtin. <br/><br/> Bölgenin yedekli olmasını istiyorsanız **bölge**olarak girin.
**Kaynak bağımlılıkları** | Her bağımlılık için seçenekler:<br/><br/>-Kaynak, hedef bölgeye taşınacak kaynak bağımlı kaynakları kullanır.<br/><br/> -Kaynak, hedef bölgede bulunan farklı bağımlı kaynakları kullanır. Bu durumda, hedef bölgedeki herhangi bir benzer kaynak arasından seçim yapabilirsiniz.

### <a name="edit-vm-target-settings"></a>VM hedef ayarlarını Düzenle

Kaynak bölgeden hedefe bağımlı kaynaklar istemiyorsanız, birkaç seçeneğiniz vardır:

- Hedef bölgede yeni bir kaynak oluşturun. Farklı ayarlar belirtmediğiniz sürece, yeni kaynak kaynak kaynakla aynı ayarlara sahip olur.
- Hedef bölgede var olan bir kaynağı kullanın.

Tam davranış, kaynak türüne bağlıdır. Hedef ayarları değiştirme hakkında [daha fazla bilgi edinin](modify-target-settings.md) .

Kaynak taşıma koleksiyonundaki **hedef yapılandırma** girişini kullanarak bir kaynağın hedef ayarlarını değiştirirsiniz. 

Bir ayarı değiştirmek için: 

1. **Bölgeler arası** sayfasında > **hedef yapılandırma** sütununda, kaynak girişinin bağlantısına tıklayın.
2. **Yapılandırma ayarları**' nda, hedef bölgede yenı bir VM oluşturabilirsiniz.
3. Hedef VM 'ye yeni bir kullanılabilirlik alanı, kullanılabilirlik kümesi veya SKU atayın. **Kullanılabilirlik bölgesi** ve **SKU 'su**.

Değişiklikler yalnızca düzenlemekte olduğunuz kaynak için yapılır. Herhangi bir bağımlı kaynağı ayrı olarak güncelleştirmeniz gerekir.


## <a name="modify-sql-settings"></a>SQL ayarlarını değiştir

Azure SQL veritabanı kaynaklarını taşırken, taşıma için hedef ayarları değiştirebilirsiniz. 

- SQL veritabanı için:
    - Hedef yapılandırma ayarlarını taşımaya hazırlanmadan önce değiştirmenizi öneririz.
    - Hedef veritabanı için ayarları ve veritabanı için bölge yedekliği ayarlarını değiştirebilirsiniz.
- Elastik havuzlar için:
    -  Taşımayı başlatmadan önce hedef yapılandırmayı dilediğiniz zaman değiştirebilirsiniz.
    - Havuz için hedef elastik havuzu ve bölge yedeklemesini değiştirebilirsiniz. 

### <a name="sql-settings-you-can-modify"></a>Değiştirebileceğiniz SQL ayarları

**Ayar** | **SQL veritabanı** | **Elastik havuz**
--- | --- | ---
**Ad** | Hedef bölgede aynı ada sahip yeni bir veritabanı oluşturun.<br/><br/> Hedef bölgede farklı bir adla yeni bir veritabanı oluşturun.<br/><br/> Hedef bölgede var olan bir veritabanını kullanın. | Hedef bölgede aynı ada sahip yeni bir elastik havuz oluşturun.<br/><br/> Hedef bölgede farklı bir adla yeni bir elastik havuz oluşturun.<br/><br/> Hedef bölgede mevcut bir elastik havuzu kullanın.
**Bölge yedekliliği** | Bölge yedekliliği destekleyen bir bölgeden olmayan bir bölgeye taşımak için, bölge ayarında **devre dışı bırak** yazın.<br/><br/> Bölge yedekliliği desteklemeyen bir bölgeden geçiş yapmak için bölge ayarında **Etkinleştir** yazın. | Bölge yedekliliği destekleyen bir bölgeden olmayan bir bölgeye taşımak için, bölge ayarında **devre dışı bırak** yazın.<br/><br/> Bölge yedekliliği desteklemeyen bir bölgeden geçiş yapmak için bölge ayarında **Etkinleştir** yazın.

### <a name="edit-sql-target-settings"></a>SQL hedef ayarlarını Düzenle

Azure SQL veritabanı kaynağı için hedef ayarları aşağıdaki gibi değiştirirsiniz: 

1. **Bölgeler arasında**, değiştirmek istediğiniz kaynak için **hedef yapılandırma** girişine tıklayın.
2. **Yapılandırma ayarları**' nda, yukarıdaki tabloda özetlenen hedef ayarları belirtin.

## <a name="next-steps"></a>Sonraki adımlar

[Bir Azure VM](tutorial-move-region-virtual-machines.md) 'yi başka bir bölgeye taşıyın.
