---
title: Azure Veri Kutusu Ağ Geçidi Genel Kullanılabilirlik sürüm notları| Microsoft Dokümanlar
description: Genel kullanılabilirlik sürümü çalıştıran Azure Veri Kutusu Ağ Geçidi için kritik açık sorunları ve çözümleri açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/26/2019
ms.author: alkohli
ms.openlocfilehash: f4ee3a5bd754335ab1c7f124671e9c37307a6a28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265408"
---
# <a name="azure-data-box-edgeazure-data-box-gateway-general-availability-release-notes"></a>Azure Veri Kutusu Kenarı/Azure Veri Kutusu Ağ Geçidi Genel Kullanılabilirlik sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, Azure Veri Kutusu Kenarı ve Azure Veri Kutusu Ağ Geçidi için Genel Kullanılabilirlik (GA) sürümü için kritik açık sorunları ve çözülmüş sorunları tanımlar.

Sürüm notları sürekli olarak güncelleştirilir ve geçici çözüm gerektiren kritik sorunlar keşfedildikçe eklenir. Veri Kutusu Kenarı/Veri Kutusu Ağ Geçidi'ni dağıtmadan önce, sürüm notlarında yer alan bilgileri dikkatle inceleyin.

GA sürümü yazılım sürümlerine karşılık gelir:

- **Veri Kutusu Ağ Geçidi 1903 (1.5.814.447)**
- **Veri Kutusu Kenar 1903 (1.5.814.447)**


## <a name="whats-new"></a>Yenilikler

- **Yeni sanal disk görüntüleri** - Yeni VHDX ve VMDK artık Azure portalında mevcuttur. Yeni Data Box Ağ Geçidi GA aygıtlarını sağlamak, yapılandırmak ve dağıtmak için bu görüntüleri indirin. Önceki önizleme sürümlerinde oluşturulan Veri Kutusu Ağ Geçidi aygıtları bu sürüme güncelleştirilemez. Daha fazla bilgi için [Azure Veri Kutusu Ağ Geçidi'ni dağıtmaya hazırla'ya](data-box-gateway-deploy-prep.md)gidin.
- **NFS desteği** - NFS desteği şu anda önizlemede dir ve Veri Kutusu Kenarı ve Veri Kutusu Ağ Geçidi aygıtlarına erişen v3.0 ve v4.1 istemcileri için kullanılabilir.
- **Depolama esnekliği** - Veri Kutusu Kenarı aygıtınız, Depolama esnekliği özelliğiyle tek bir veri diskinin arızalanmasına dayanabilir. Bu özellik şu anda önizleme sürümündedir. Yerel web Kullanıcı Aracı'ndaki **Depolama ayarlarında** **Esnek** seçeneğini seçerek depolama esnekliğini etkinleştirebilirsiniz.


## <a name="known-issues-in-ga-release"></a>GA sürümünde bilinen sorunlar

Aşağıdaki tablo, Veri Kutusu Ağ Geçidi sürümü için bilinen sorunların bir özetini sağlar.

| Hayır. | Özellik | Sorun | Geçici çözüm/yorumlar |
| --- | --- | --- | --- |
| **1.** |Dosya türleri | Aşağıdaki dosya türleri desteklenmez: karakter dosyaları, blok dosyaları, soketler, borular, sembolik bağlantılar.  |Bu dosyaların kopyalanması, NFS paylaşımında oluşturulan 0 uzunlukta ki dosyalarla sonuçlanır. Bu dosyalar bir hata durumunda kalır ve aynı zamanda *error.xml*olarak bildirilir. <br> Dizinlere olan sembolik bağlantılar, dizinlerin çevrimdışı olarak işaretlenmemiş olmasıyla sonuçlanır. Sonuç olarak, dizinlerin çevrimdışı olduğunu ve ilişkili tüm içeriğin tamamen Azure'a yüklendiğini gösteren dizinlerde gri çapraz ı göremeyebilirsiniz. |
| **2.** |Silme | Bu sürümdeki bir hata nedeniyle, bir NFS payı silinirse, paylaşım silinmeyebilir. Paylaşım durumu *Silme'yi*görüntüler.  |Bu, yalnızca paylaşım desteklenmeyen bir dosya adı kullandığında oluşur. |
| **3.** |Kopyala | Hata ile veri kopyalama başarısız olur: İstenen işlem bir dosya sistemi sınırlaması nedeniyle tamamlanamadı.  |Dosya boyutu 128 KB'den büyük olan Alternatif Veri Akışı (ADS) desteklenmez.   |


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Kutusu Ağ Geçidi'ni dağıtmaya hazırlanın.](data-box-gateway-deploy-prep.md)
- [Azure Veri Kutusu Kenarı dağıtmaya hazırlanın.](data-box-edge-deploy-prep.md)
