---
title: Azure NetApp Dosyaları için kaynak sınırları | Microsoft Dokümanlar
description: Azure NetApp Files kaynaklarının sınırlarını ve kaynak limiti artış isteğini açıklar.
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
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637393"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Azure NetApp Files için kaynak sınırları

Azure NetApp Dosyaları için kaynak sınırlarını anlamak, birimlerinizi yönetmenize yardımcı olur.

## <a name="resource-limits"></a>Kaynak sınırları

Aşağıdaki tabloda Azure NetApp Dosyaları için kaynak sınırları açıklanmaktadır:

|  Kaynak  |  Varsayılan limit  |  Destek isteği ile ayarlanabilir  |
|----------------|---------------------|--------------------------------------|
|  Azure bölgesi başına NetApp hesabı sayısı   |  10    |  Evet   |
|  NetApp hesabı başına kapasite havuzu sayısı   |    25     |   Evet   |
|  Kapasite havuzu başına birim sayısı     |    500   |    Evet     |
|  Birim başına anlık görüntü sayısı       |    255     |    Hayır        |
|  Azure Sanal Ağı başına Azure NetApp Dosyalarına (Microsoft.NetApp/birimleri) devredilen alt ağ sayısı    |   1   |    Hayır    |
|  Azure NetApp Dosyaları ile VNet'teki (hemen eşlenen VNet'ler dahil) kullanılan IP sayısı   |    1000   |    Hayır   |
|  Tek kapasiteli havuzun minimum boyutu   |  4 TiB     |    Hayır  |
|  Tek kapasiteli havuzun maksimum boyutu    |  500 TiB   |   Hayır   |
|  Tek bir birimin minimum boyutu    |    100 GiB    |    Hayır    |
|  Tek bir birimin maksimum boyutu     |    100 TiB    |    Hayır    |
|  Tek bir dosyanın maksimum boyutu     |    16 TiB    |    Hayır    |    
|  Tek bir dizin in maksimum boyutu      |    320 MB    |    Hayır    |    
|  Birim başına maksimum dosya sayısı[(maxfiles)](#maxfiles)     |    100 milyon    |    Evet    |    

Daha fazla bilgi için [Kapasite yönetimi SSS'lerine](azure-netapp-files-faqs.md#capacity-management-faqs)bakın.

## <a name="maxfiles-limits"></a>Maxfiles sınırları<a name="maxfiles"></a> 

Azure NetApp Files birimlerinin *maxfiles*adı verilen bir sınırı vardır. En üst dosya sınırı, bir birimin içerebileceği dosya sayısıdır. Azure NetApp Files biriminin en üst dosya sınırı, birimin boyutuna (kotaya) göre dizine dizine eklenir. Bir birim için maksimum dosya sınırı, verilen birim boyutunun TiB başına 20 milyon dosya oranında artar veya azalır. 

Hizmet, verilen boyutuna bağlı olarak bir birim için maksimum dosya sınırını dinamik olarak ayarlar. Örneğin, başlangıçta 1 TiB boyutuyla yapılandırılan bir birim, 20 milyon maksimum dosya sınırına sahip olacaktır. Birim boyutunda sonraki değişiklikler, aşağıdaki kurallara göre maksimum dosya sınırının otomatik olarak yeniden ayarlanmasına neden olur: 

|    Birim boyutu (kota)     |  Maksimum dosya sınırının otomatik olarak yeniden ayarlandırılması    |
|----------------------------|-------------------|
|    < 1 TiB                 |    20 milyon     |
|    >= 1 TiB ama < 2 TiB    |    40 milyon     |
|    >= 2 TiB ama < 3 TiB    |    60 milyon     |
|    >= 3 TiB ama < 4 TiB    |    80 milyon     |
|    >= 4 TiB                |    100 milyon    |

Herhangi bir birim boyutu için, maksimum dosya sınırını 100 milyonun üzerine çıkarmak için bir [destek isteği](#limit_increase) başlatabilirsiniz.

## <a name="request-limit-increase"></a>İstek limiti artışı<a name="limit_increase"></a> 

Yukarıdaki tablodan ayarlanabilir sınırları artırmak için bir Azure destek isteği oluşturabilirsiniz. 

Azure portal navigasyon uçağından: 

1. **Yardım + destek'i**tıklatın.
2. + **Yeni destek isteğine**tıklayın.
3. Temel Bilgiler sekmesinde aşağıdaki bilgileri sağlayın: 
    1. Sorun türü: **Hizmet ve abonelik limitlerini (kotalar)** seçin.
    2. Abonelikler: Kotanın artırılmasını gerekdiren kaynak için aboneliği seçin.
    3. Kota türü: **Depolama'yı seçin: Azure NetApp Dosyaları sınırları.**
    4. Sonraki' ye **tıklayın: Çözümler**.
4. Ayrıntılar sekmesinde:
    1. Açıklama kutusunda, ilgili kaynak türü için aşağıdaki bilgileri sağlayın:

        |  Kaynak  |    Üst kaynaklar      |    İstenen yeni sınırlar     |    Kota artışının nedeni       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Hesap |  *Abonelik Kimliği*   |  *İstenen yeni maksimum **hesap** numarası*    |  *İstek hangi senaryo veya kullanım örneği istendi?*  |
        |  Havuz    |  *Abonelik Kimliği, Hesap URI*  |  *İstenen yeni maksimum **havuz** numarası*   |  *İstek hangi senaryo veya kullanım örneği istendi?*  |
        |  Birim  |  *Abonelik Kimliği, Hesap URI, Havuz URI*   |  *İstenen yeni maksimum **birim** numarası*     |  *İstek hangi senaryo veya kullanım örneği istendi?*  |
        |  Maxfiles  |  *Abonelik Kimliği, Hesap URI, Havuz URI, Volume URI*   |  *İstenen yeni maksimum **maksimum dosya** numarası*     |  *İstek hangi senaryo veya kullanım örneği istendi?*  |    

    2. Uygun destek yöntemini belirtin ve sözleşme bilgilerinizi sağlayın.

    3. **Sonraki'yi tıklatın: İstek** oluşturmak için Gözden Geçir + oluşturun. 


## <a name="next-steps"></a>Sonraki adımlar  

- [Azure NetApp Files’ın depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)
- [Azure NetApp Files için maliyet modeli](azure-netapp-files-cost-model.md)
