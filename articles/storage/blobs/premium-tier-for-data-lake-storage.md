---
title: Azure Data Lake Storage için Premium katman | Microsoft Docs
description: Azure Data Lake Storage 2. ile Premium performans katmanını kullanın
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.custom: references_regions
ms.date: 10/30/2020
ms.author: normesta
ms.openlocfilehash: dc0b620629b5dce651e02e5fe95b0619c1d42a78
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93131165"
---
# <a name="premium-tier-for-azure-data-lake-storage"></a>Azure Data Lake Storage için Premium katman

Azure Data Lake Storage 2. artık [Premium performans katmanını](storage-blob-performance-tiers.md#premium-performance)desteklemektedir. Premium performans katmanı, düşük tutarlı gecikme süresi gerektiren ve çok sayıda işlem içeren büyük veri analizi uygulamaları ve iş yükleri için idealdir.

## <a name="workloads-that-can-benefit-from-the-premium-performance-tier"></a>Premium performans katmanından faydalanabilir iş yükleri

Örnek iş yükleri arasında etkileşimli iş yükleri, IoT, akış analizi, yapay zeka ve makine öğrenimi bulunur. 

**Etkileşimli iş yükleri** 

Bu iş yükleri, e-ticaret ve eşleme uygulamaları, etkileşimli video uygulamaları vb. gibi anlık güncelleştirmeler ve Kullanıcı geri bildirimi gerektirir. Örneğin, bir e-ticaret uygulamasında daha az sıklıkta görüntülenen öğeler muhtemelen önbelleğe alınmaz. Ancak, bu müşterilerin isteğe bağlı olarak müşteriye anında gösterilmesi gerekir. Diğer bir örnek olarak, veri bilimcileri, analistleri ve geliştiriciler Premium performans katmanını kullanan bir hesapta depolanan veriler üzerinde sorgular çalıştırarak zamana duyarlı içgörüler daha da hızlı bir şekilde türetilebilir. 

**IoT/akış analizi** 

IoT senaryosunda, buluta her saniye çok sayıda küçük yazma işlemi itilmiş olabilir. Büyük miktarlarda veri alınabilir, analiz amaçlarıyla toplanır ve hemen hemen silinir. Premium performans katmanının yüksek alım özellikleri, bu iş yükü türünün verimli olmasını sağlar. 

**Yapay zeka/makine öğrenimi (AI/ML)** 

AI/ML, görseller, konuşma ve metin gibi farklı veri türlerinin tüketimine ve işlenmesiyle ilgilidir. Bu yüksek performanslı bilgi işlem iş yükü, veri analizi için hızlı yanıt ve verimli alma süreleri gerektiren büyük miktarda verilerle ilgilidir. 

## <a name="cost-effectiveness"></a>Maliyet uygunluğu

Premium performans katmanının daha yüksek depolama maliyeti ancak standart performans katmanıyla karşılaştırıldığında daha düşük bir işlem maliyeti vardır. Uygulamalarınız ve iş yükleriniz çok sayıda işlem yürütüle, Premium performans katmanı ekonomik olabilir.

Aşağıdaki tabloda Azure Data Lake Storage için Premium katmanın maliyet verimliliği gösterilmektedir. Her sütun, bir aydaki işlem sayısını temsil eder.  Her satır, okuma işlemleri olan işlemlerin yüzdesini temsil eder. Tablodaki her hücrede, okuma işlemi yüzdesi ve yürütülen işlem sayısı ile ilişkili maliyet azaltma yüzdesi gösterilmektedir. 

Örneğin, hesabınızın Doğu ABD 2 bölgede olduğu varsayılırsa, hesabınıza sahip olan işlem sayısı 90 ' ı aşmaktadır ve bu işlemlerin %70 ' si okuma işlemleri olduğunu varsayarsak, Premium performans katmanı daha düşük maliyetli olur.

> [!div class="mx-imgBorder"]
> ![resim buraya gider](./media/premium-tier-for-data-lake-storage/premium-performance-data-lake-storage-cost-analysis-table.png)

> [!NOTE] 
> Her TB veri için saniye başına işlem sayısına göre maliyet verimliliğini değerlendirmek isterseniz, tablonun en altında görünen sütun başlıklarını kullanabilirsiniz.

Fiyatlandırma hakkında daha fazla bilgi için [Azure Data Lake Storage 2. fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/data-lake/) sayfasına bakın.

## <a name="feature-availability"></a>Özellik kullanılabilirliği 

Bazı BLOB depolama özellikleri kullanılamayabilir veya Premium performans katmanıyla yalnızca kısmi destek alabilir. Tüm liste için, bkz. [Azure Data Lake Storage 2. bulunan BLOB Storage özellikleri](data-lake-storage-supported-blob-storage-features.md). Ardından, işlevsellikten herhangi bir boşluk değerlendirmek için [bilinen sorunların](data-lake-storage-known-issues.md) bir listesini gözden geçirin.

## <a name="enabling-the-premium-performance-tier"></a>Premium performans katmanını etkinleştirme 

**Hiyerarşik ad alanı** ayarı **etkinken** bir blok blobstorage hesabı oluşturarak Azure Data Lake Storage için Premium katmanı kullanabilirsiniz. Tüm rehberlik için bkz. [blok Blobstorage hesap hesabı oluşturma](storage-blob-create-account-block-blob.md) .

Hesabı oluştururken, **Premium** performans seçeneğini ve **blok blobstorage** hesabı türünü seçtiğinizden emin olun.

> [!div class="mx-imgBorder"]
> ![Blok blobstoragehesabı oluştur](./media/premium-tier-for-data-lake-storage/create-block-blob-storage-account.png)

**Depolama hesabı oluştur** sayfasının **Gelişmiş** sekmesinde **hiyerarşik ad alanı** ayarını etkinleştirin. Hesabı oluştururken bu ayarı etkinleştirmeniz gerekir. Daha sonra etkinleştiremezsiniz.

Aşağıdaki görüntüde, bu ayar **depolama hesabı oluştur** sayfasında gösterilmektedir.

> [!div class="mx-imgBorder"]
> ![Hiyerarşik ad alanı ayarı](./media/create-data-lake-storage-account/hierarchical-namespace-feature.png)

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Azure Data Lake Storage için Premium katman aşağıdaki bölgelerde kullanılabilir.

|Bölge|Yedeklilik|
|--|--|
|Doğu ABD|LRS, ZRS|
|Doğu ABD 2|LRS, ZRS|
|Central US|LRS|
|Batı ABD|LRS|
|Batı ABD 2|LRS, ZRS|
|Orta Batı ABD|LRS|
|Orta Güney ABD|LRS|
|Orta Kanada|LRS|
|Doğu Kanada|LRS|
|North Europe|LRS, ZRS|
|West Europe|LRS, ZRS|
|Güney Birleşik Krallık|LRS|
|Batı Birleşik Krallık|LRS|
|Orta Fransa|LRS|
|Doğu Asya|LRS|
|Güney Kore - Orta|LRS|
|Güney Kore - Güney|LRS|
|Orta Hindistan|LRS|
|Batı Hindistan|LRS|
|BAE Kuzey|LRS|
|Doğu Japonya|LRS|
|Batı Japonya|LRS|
|Güneydoğu Asya|LRS, ZRS|
|Doğu Avustralya|LRS, ZRS|
|Avustralya Güneydoğu|LRS|
|Brezilya Güney|LRS|
