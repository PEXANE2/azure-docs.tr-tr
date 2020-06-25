---
title: Azure SQL Edge için sürüm notları
description: Azure SQL Edge görüntülerinde nelerin yeni olduğunu veya değiştiğini açıklayan sürüm notları
keywords: sürüm notları SQL Edge
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
ms.subservice: ''
author: VasiyaKrishnan
ms.author: vakrishn
ms.reviewer: sstein
ms.date: 06/05/2020
ms.openlocfilehash: d5d60db2cc0c35b908454be3f00716db6d2da945
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85361651"
---
# <a name="azure-sql-edge-release-notes"></a>Azure SQL Edge sürüm notları 

Bu makalede, yenilikler ve her yeni Azure SQL Edge derlemesi ile nelerin değiştiğini açıklanmaktadır.

## <a name="ctp-21"></a>CTP 2,1 
### <a name="sql-engine-build-number---15020001545"></a>SQL altyapısı derleme numarası-15.0.2000.1545
### <a name="fixes"></a>Düzeltmeler
1. ARM 'de CPUıD sorununu işlemek için ONNX modelleriyle tahmın etme 
2. TSQL akışının başlangıcında hata yolunun işlenmesini geliştirmek için onarma 
3. Veri olmadığında iş ölçümlerinde hatalı eşik gecikmesi değerini onarın. 
4. Bağdaştırıcı, toplu işler arasında değişken şemasına sahip olduğunda çıkış bağdaştırıcısındaki sorunu düzeltir.  

## <a name="ctp-20"></a>CTP 2,0 
### <a name="sql-engine-build-number---15020001401"></a>SQL altyapısı derleme numarası-15.0.2000.1401
### <a name="whats-new"></a>Yenilikler
1.  Ürün adı ' Azure SQL Edge ' olarak güncelleştirildi
1.  Date_bucket işlevi

    i.  Tarih, saat, tarih saat türü desteği
3.  ONNX ile tahmın edin
    
    i.  ONNX için çalışma zamanı parametresi gereklidir 
    
4.  TSQL akış desteği (sınırlı Önizleme) 
 
### <a name="known-issues"></a>Bilinen Sorunlar

1. <b>Sorun:</b> Zamanlama sorunu nedeniyle başlatmada dacpac uygulama ile ilgili olası sorunlar.

    <b>Geçici çözüm:</b> SQL Server veya kapsayıcının yeniden başlatılması, dacpac 'yi uygulamayı yeniden deneyecek ve sorunu çözmelidir
### <a name="request-support"></a>Destek iste
1. Destek [sayfasında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)destek isteyebilirsiniz.

4. Aşağıdaki alanların seçildiğinden emin olun: 
    * Sorun türü-Teknik 
    * Hizmet-IoT Edge
    * Sorun türü-sorunum bir IoT Edge modülüyle ilişkili
    * Sorun alt türü-Azure SQL Edge

   ![Örnek destek bileti](media/get-support/support-ticket.png)

## <a name="ctp-15"></a>CTP 1,5
### <a name="sql-engine-build-number---15020001331"></a>SQL altyapısı derleme numarası-15.0.2000.1331
### <a name="whats-new"></a>Yenilikler
1. Date_bucket işlevi
    
    i. DateTimeOffset türü desteği
2. ONNX modelleriyle tahmın edin

    i. nvarchar desteği
 
## <a name="ctp-14"></a>CTP 1,4
### <a name="sql-engine-build-number---15020001247"></a>SQL altyapısı derleme numarası-15.0.2000.1247
### <a name="whats-new"></a>Yenilikler
1.  ONNX modelleriyle tahmın edin
 
    i.  Varchar desteği
    
    ii. ONNX çalışma zamanı 1,0 sürümüne geçiş 
2.  Özellik desteği-aşağıdaki özellikler etkinleştirilmiştir:

    i.   CDC desteği

    ii.  Sıkıştırma içeren geçmiş tablosu

    iii. Günlük okuma için daha yüksek ölçek faktörü

    IV.  Batch modu, aşağı doğru filtrele

    v.   İleriye yönelik iyileştirmeleri okuyun
 
## <a name="ctp-13"></a>CTP 1,3
### <a name="sql-engine-build-number---15020001147"></a>SQL altyapısı derleme numarası-15.0.2000.1147
### <a name="whats-new"></a>Yenilikler
1. Azure ıOT Portal dağıtımı 

    i.   AMD64 ve ARM görüntülerini dağıtma desteği

    ii.  Akış işi oluşturma desteği

    iii. Dacpac dağıtımı
2. ONNX modelleriyle tahmın edin

    i. Sayısal tür desteği
3. Özellik desteği-aşağıdaki özellikler etkinleştirilmiştir:

    i.  Toplam sütun deposu taramasına aşağı itme

    ii. Mutlu-go-yuvarlak taramalar
4. Ayak izi ve bellek tüketimi azaltma işi
