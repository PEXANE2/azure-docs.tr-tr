---
title: Genel kullanılabilirlik sürüm notlarını Azure Data Box Gateway | Microsoft Docs
description: Genel kullanılabilirlik sürümü çalıştıran Azure Data Box Gateway için kritik açık sorunları ve çözümleri açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/26/2019
ms.author: alkohli
ms.openlocfilehash: f4ee3a5bd754335ab1c7f124671e9c37307a6a28
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79265408"
---
# <a name="azure-data-box-edgeazure-data-box-gateway-general-availability-release-notes"></a>Azure Data Box Edge/Azure Data Box Gateway genel kullanılabilirlik sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, Azure Data Box Edge ve Azure Data Box Gateway için kritik açık sorunları ve genel kullanılabilirlik (GA) sürümü için çözülmüş sorunları belirler.

Sürüm notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar keşfedildiğinde eklenirler. Data Box Edge/Data Box Gateway dağıtmadan önce, sürüm notlarında bulunan bilgileri dikkatle gözden geçirin.

GA sürümü yazılım sürümlerine karşılık gelir:

- **Data Box Gateway 1903 (1.5.814.447)**
- **Data Box Edge 1903 (1.5.814.447)**


## <a name="whats-new"></a>Yenilikler

- **Yeni sanal disk görüntüleri** -Yeni VHDX ve VMDK artık Azure Portal kullanılabilir. Yeni Data Box Gateway GA cihazları sağlamak, yapılandırmak ve dağıtmak için bu görüntüleri indirin. Önceki önizleme sürümlerinde oluşturulan Data Box Gateway cihazları bu sürüme güncelleştirilemez. Daha fazla bilgi için, [Azure Data Box Gateway dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md)bölümüne gidin.
- **NFS desteği** -NFS desteği şu anda, Data Box Edge ve Data Box Gateway cihazlarına erişen v 3.0 ve v 4.1 istemcilerinde önizleme aşamasındadır ve kullanılabilir.
- **Depolama dayanıklılığı** -Data Box Edge cihazınız, depolama dayanıklılığı özelliği ile bir veri diskinin başarısızlığını gerçekleştirebilir. Bu özellik şu anda önizleme sürümündedir. Yerel Web Kullanıcı arabirimindeki **depolama ayarları** ' nda **dayanıklı seçeneğini belirleyerek** depolama dayanıklılığı sağlayabilirsiniz.


## <a name="known-issues-in-ga-release"></a>GA sürümündeki bilinen sorunlar

Aşağıdaki tabloda, sürüm çalıştıran Data Box Gateway için bilinen sorunların bir özeti verilmiştir.

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamalar |
| --- | --- | --- | --- |
| **1.** |Dosya türleri | Şu dosya türleri desteklenmez: karakter dosyaları, blok dosyaları, yuvalar, kanallar, sembolik bağlantılar.  |Bu dosyaların kopyalanması, NFS paylaşımında oluşturulan 0 uzunluklu dosyalara neden olur. Bu dosyalar hata durumunda kalır ve *hata. xml*içinde de raporlanır. <br> Dizinlerin simgesel bağlantıları, dizinlerde hiçbir şekilde çevrimdışı olarak işaretlenmez. Sonuç olarak, dizinlerin çevrimdışı olduğunu ve ilişkili tüm içeriğin Azure 'a tamamen yüklendiğini gösteren dizinler üzerinde gri bir işlem görmeyebilirsiniz. |
| **2.** |Silme | Bu sürümdeki bir hata nedeniyle, bir NFS paylaşma silinirse, paylaşma silinmeyebilir. Paylaşma durumu, *silme*öğesini görüntüleyecektir.  |Bu yalnızca, paylaşımın desteklenmeyen bir dosya adı kullandığı zaman gerçekleşir. |
| **3.** |Kopyala | Veri kopyalama işlemi hata vererek başarısız oldu: istenen işlem bir dosya sistemi sınırlaması nedeniyle tamamlanamadı.  |128 KB 'den büyük dosya boyutuyla ilişkili alternatif veri akışı (ADS) desteklenmiyor.   |


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Box Gateway dağıtmaya hazırlanın](data-box-gateway-deploy-prep.md).
- [Azure Data Box Edge dağıtmaya hazırlanın](data-box-edge-deploy-prep.md).
