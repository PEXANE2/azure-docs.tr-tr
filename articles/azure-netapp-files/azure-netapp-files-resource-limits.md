---
title: Azure NetApp Files için kaynak sınırları | Microsoft Docs
description: Azure NetApp Files kaynaklarına yönelik limitleri ve kaynak sınırı artışını nasıl isteyeceğini açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: b-juche
ms.openlocfilehash: ac660b20d519e49e832e979603f763fa672757a5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80637393"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Azure NetApp Files için kaynak sınırları

Azure NetApp Files için kaynak sınırlarını anlamak, birimlerinizi yönetmenize yardımcı olur.

## <a name="resource-limits"></a>Kaynak sınırları

Aşağıdaki tabloda Azure NetApp Files için kaynak sınırları açıklanmaktadır:

|  Kaynak  |  Varsayılan limit  |  Destek isteği aracılığıyla ayarlanabilir  |
|----------------|---------------------|--------------------------------------|
|  Azure bölgesi başına NetApp hesabı sayısı   |  10    |  Yes   |
|  NetApp hesabı başına kapasite havuzlarının sayısı   |    25     |   Yes   |
|  Kapasite havuzu başına birim sayısı     |    500   |    Yes     |
|  Birim başına anlık görüntü sayısı       |    255     |    Hayır        |
|  Azure sanal ağı başına Azure NetApp Files (Microsoft. NetApp/birimler) için temsilci seçilen alt ağ sayısı    |   1   |    Hayır    |
|  Bir VNet 'te kullanılan IP sayısı (anında eşlenen sanal ağlar dahil) Azure NetApp Files   |    1000   |    Hayır   |
|  Tek bir kapasite havuzunun en küçük boyutu   |  4 TiB     |    Hayır  |
|  Tek bir kapasite havuzunun en büyük boyutu    |  500 TiB   |   Hayır   |
|  Tek bir birimin en küçük boyutu    |    100 GiB    |    Hayır    |
|  Tek bir birimin en büyük boyutu     |    100 TiB    |    Hayır    |
|  Tek bir dosyanın en büyük boyutu     |    16 TiB    |    Hayır    |    
|  Tek bir dizinin en büyük boyutu      |    320 MB    |    Hayır    |    
|  Birim başına en fazla dosya sayısı ([maxfiles](#maxfiles))     |    100.000.000    |    Yes    |    

Daha fazla bilgi için bkz. [Kapasite Yönetimi SSS](azure-netapp-files-faqs.md#capacity-management-faqs).

## <a name="maxfiles-limits"></a>Maxfiles limitleri<a name="maxfiles"></a> 

Azure NetApp Files birimlerde *maxfiles*adlı bir sınır vardır. Maxfiles sınırı, bir birimin içerebileceği dosya sayısıdır. Bir Azure NetApp Files birimi için maxfiles limiti, birimin boyutuna (Kota) göre dizinlenir. Bir birim için maxfiles sınırı, sağlanan birim boyutu başına 20.000.000 dosya hızında artar veya azalır. 

Hizmet, bir birimin sağlanan boyutuna bağlı olarak maxfiles sınırını dinamik olarak ayarlar. Örneğin, başlangıçta 1 TiB boyutuyla yapılandırılan bir birimin maxfiles sınırı 20.000.000 olur. Birimin boyutuyla ilgili sonraki değişiklikler, aşağıdaki kurallara göre maxfiles sınırının otomatik olarak yeniden okundu olarak oluşmasına neden olur: 

|    Birim boyutu (Kota)     |  Maxfiles limitini otomatik olarak ayarlama    |
|----------------------------|-------------------|
|    < 1 TiB                 |    20.000.000     |
|    >= 1 TiB, ancak < 2 TiB    |    40.000.000     |
|    >= 2 TiB ancak < 3 TiB    |    60.000.000     |
|    >= 3 TiB ancak < 4 TiB    |    80.000.000     |
|    >= 4 TiB                |    100.000.000    |

Herhangi bir birim boyutu için, maxfiles limitini 100.000.000 ötesine yükseltmek üzere bir [destek isteği](#limit_increase) başlatabilirsiniz.

## <a name="request-limit-increase"></a>İstek sınırı artışı<a name="limit_increase"></a> 

Yukarıdaki tablodan ayarlanabilir sınırları artırmak için bir Azure destek isteği oluşturabilirsiniz. 

Azure portal gezinti düzleminden: 

1. **Yardım ve destek**' e tıklayın.
2. **+ Yeni destek isteği**' ne tıklayın.
3. Temel bilgiler sekmesinde, aşağıdaki bilgileri sağlayın: 
    1. Sorun türü: **hizmet ve abonelik sınırlarını (kotalar)** seçin.
    2. Abonelikler: kotanın artması gereken kaynak için aboneliği seçin.
    3. Kota türü: **Depolama: Azure NetApp Files sınırlarını**seçin.
    4. **İleri: çözümler**' e tıklayın.
4. Ayrıntılar sekmesinde:
    1. Açıklama kutusunda, ilgili kaynak türü için aşağıdaki bilgileri sağlayın:

        |  Kaynak  |    Üst kaynaklar      |    İstenen yeni sınırlar     |    Kota artışı nedeni       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Hesap |  *Abonelik KIMLIĞI*   |  *İstenen yeni en büyük **Hesap** numarası*    |  *İstekte hangi senaryo veya kullanım örneği istendi?*  |
        |  Havuz    |  *Abonelik KIMLIĞI, hesap URI 'SI*  |  *İstenen yeni en büyük **Havuz** numarası*   |  *İstekte hangi senaryo veya kullanım örneği istendi?*  |
        |  Birim  |  *Abonelik KIMLIĞI, hesap URI 'si, havuz URI 'SI*   |  *İstenen yeni en yüksek **birim** numarası*     |  *İstekte hangi senaryo veya kullanım örneği istendi?*  |
        |  Maxfiles  |  *Abonelik KIMLIĞI, hesap URI 'SI, havuz URI 'si, birim URI 'SI*   |  *İstenen yeni en yüksek **maxfiles** numarası*     |  *İstekte hangi senaryo veya kullanım örneği istendi?*  |    

    2. Uygun destek yöntemini belirtin ve sözleşme bilgilerinizi sağlayın.

    3. Ileri ' ye tıklayın, isteği oluşturmak için **+ Oluştur** ' a tıklayın. 


## <a name="next-steps"></a>Sonraki adımlar  

- [Azure NetApp Files’ın depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)
- [Azure NetApp Files için maliyet modeli](azure-netapp-files-cost-model.md)
