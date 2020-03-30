---
title: Azure Veri Gölü Depolama Gen2 Hiyerarşik Ad Alanı
description: Azure Veri Gölü Depolama Gen2 için hiyerarşik ad alanı kavramını açıklar
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: normesta
ms.reviewer: jamesbak
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 6ce94590a1d0de6941c27d972bdd1c4194080e95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77153086"
---
# <a name="azure-data-lake-storage-gen2-hierarchical-namespace"></a>Azure Veri Gölü Depolama Gen2 hiyerarşik ad alanı

Azure Veri Gölü Depolama Gen2'nin nesne depolama ölçeğinde ve fiyatlarda dosya sistemi performansı sağlamasına olanak tanıyan önemli bir **mekanizma, hiyerarşik**bir ad alanının eklenmesidir. Bu, bir hesaptaki nesnelerin/dosyaların toplanmasının bilgisayarınızdaki dosya sisteminin düzenlendiği gibi dizinler ve iç içe geçen alt dizinler hiyerarşisi içinde düzenlenmesini sağlar. Hiyerarşik ad alanı etkinken, bir depolama hesabı analitik altyapılara ve çerçevelere aşina olan dosya sistemi anlambilimiyle nesne depolamanın ölçeklenebilirliğini ve maliyet etkinliğini sağlama yeteneğine sahip olur.

## <a name="the-benefits-of-a-hierarchical-namespace"></a>Hiyerarşik ad alanının yararları

Aşağıdaki avantajlar, blob verileri üzerinde hiyerarşik bir ad alanı uygulayan dosya sistemleriyle ilişkilidir:

- **Atomik dizin manipülasyonu:** Nesne depoları, yol parçalarını belirtmek için nesne adına kesik (/) katıştırma kuralını benimseyerek bir dizin hiyerarşisine yakındır. Bu kural nesneleri düzenlemek için çalışırken, dizinleri taşıma, yeniden adlandırma veya silme gibi eylemler için hiçbir yardım sağlamaz. Gerçek dizinler olmadan, uygulamalar dizin düzeyinde görevler elde etmek için potansiyel olarak milyonlarca tek tek blob'u işlemelidir. Bunun aksine, hiyerarşik bir ad alanı bu görevleri tek bir girişi (üst dizini) güncelleştirerek işler.

    Bu dramatik optimizasyon, özellikle birçok büyük veri analizi çerçevesi için önemlidir. Hive, Kıvılcım, vb gibi araçlar genellikle geçici konumlara çıktı yazmak ve daha sonra işin sonunda konumu yeniden adlandırmak. Hiyerarşik bir ad alanı olmadan, bu yeniden adlandırma genellikle analiz işleminin kendisinden daha uzun sürebilir. Daha düşük iş gecikmesi, analitik iş yükleri için daha düşük toplam sahip olma maliyetine (TCO) eşittir.

- **Tanıdık Arayüz Stili:** Dosya sistemleri hem geliştiriciler hem de kullanıcılar tarafından iyi anlaşılır. Veri Gölü Depolama Gen2 tarafından maruz dosya sistemi arayüzü bilgisayarlar tarafından kullanılan aynı paradigma, büyük ve küçük olduğu gibi buluta hareket zaman yeni bir depolama paradigması öğrenmek için gerek yoktur.

Nesne depolarının tarihsel olarak hiyerarşik bir ad alanını desteklememiş olmasının nedenlerinden biri, hiyerarşik ad alanının ölçeği sınırlamış olmasıdır. Ancak, Veri Gölü Depolama Gen2 hiyerarşik ad alanı doğrusal ölçekler ve veri kapasitesi veya performansı düşürmez.

## <a name="deciding-whether-to-enable-a-hierarchical-namespace"></a>Hiyerarşik ad alanını etkinleştirmeye karar verme

Hesabınızda hiyerarşik bir ad alanı etkinleştirdikten sonra, hesabı düz bir ad alanına geri döndüremezsiniz. Bu nedenle, nesne deposu iş yüklerinizin yapısına dayalı hiyerarşik bir ad alanı etkinleştirmek için mantıklı olup olmadığını düşünün.

Bazı iş yükleri hiyerarşik bir ad alanı etkinleştirerek herhangi bir yarar elde olmayabilir. Örnekler arasında yedeklemeler, görüntü depolama alanı ve nesne organizasyonunun nesnelerin kendilerinden ayrı olarak depolandığı diğer uygulamalar (örneğin: ayrı bir veritabanında). 

Ayrıca, Blob depolama özellikleri ve Azure hizmet ekosistemi desteği büyümeye devam ederken, hiyerarşik ad alanına sahip hesaplarda henüz desteklenmeyen bazı özellikler ve Azure hizmetleri vardır. [Bkz. Bilinen Sorunlar](data-lake-storage-known-issues.md). 

Genel olarak, dizinleri işleyen dosya sistemleri için tasarlanmış depolama iş yükleri için hiyerarşik bir ad alanını açmanızı öneririz. Bu, öncelikle analitik işleme için olan tüm iş yüklerini içerir. Yüksek derecede organizasyon gerektiren veri kümeleri, hiyerarşik bir ad alanı etkinleştirerek de yarar sağlar.

Hiyerarşik bir ad alanını etkinleştirme nedenleri bir TCO analizi ile belirlenir. Genel olarak konuşursak, depolama hızlandırması nedeniyle iş yükü gecikmesindeki iyileştirmeler daha kısa bir süre için hesaplama kaynakları gerektirir. Hiyerarşik ad alanı tarafından etkinleştirilen atomik dizin işlemesi nedeniyle birçok iş yükü için gecikme sonu geliştirilebilir. Birçok iş yükünde, bilgi işlem kaynağı toplam maliyetin %85'> temsil eder ve bu nedenle iş yükü gecikmesinde bile mütevazı bir azalma önemli miktarda TCO tasarrufuna eşittir. Hiyerarşik bir ad alanının etkinleştirilmesinin depolama maliyetlerini artırdığı durumlarda bile, düşük işlem maliyetleri nedeniyle TCO yine de düşürülür.

Veri depolama fiyatları, işlem fiyatları ve depolama kapasitesi rezervasyon fiyatlandırmasındaki farklılıkları hiyerarşik ad alanına sahip hesaplar ile hiyerarşik ad alanı arasındaki farklılıkları analiz etmek için [Bkz.](https://azure.microsoft.com/pricing/details/storage/data-lake/)

## <a name="next-steps"></a>Sonraki adımlar

- [Depolama hesabı oluşturma](./data-lake-storage-quickstart-create-account.md)
