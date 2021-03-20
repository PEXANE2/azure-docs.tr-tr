---
title: Veri saklama ilkesine genel bakış-Azure SQL Edge
description: Azure SQL Edge 'de veri bekletme ilkesi hakkında bilgi edinin
keywords: SQL Edge, veri saklama
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/04/2020
ms.openlocfilehash: bb059a946c03f41e5b65944eec67070f84ee6b08
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "90976347"
---
# <a name="data-retention-overview"></a>Veri saklamaya genel bakış

Bağlı IoT cihazlarındaki verilerin toplanması ve depolanması, işletimsel ve iş öngörülerini sağlamak için önemlidir. Ancak, bu cihazlardan kaynaklanan verilerin hacmi verildiğinde, kuruluşların sürdürmek istedikleri veri miktarını ve ayrıntı düzeyini dikkatle planlayıp planmaları önemli hale gelir. Tüm verilerin her ayrıntı düzeyinde bekletilmesini tercih ederken her zaman pratik değildir. Ayrıca, tutulabileceği veri hacmi, IoT veya uç cihazlarda bulunan depolama miktarı ile sınırlıdır. 

Azure SQL Edge veritabanı yöneticileri, bir SQL Edge veritabanında ve temel tablolarında veri bekletme ilkesi tanımlayabilir. Veri saklama ilkesi tanımlandıktan sonra, Kullanıcı tablolarından kullanılmayan (eski) verileri temizlemek için bir arka plan sistem görevi çalıştırılır. 

> [!Note]
> Tablodan Temizlenen veriler kurtarılamaz. Temizlenen verileri kurtarmanın mümkün olan tek yolu, veritabanını eski bir yedekten geri yüklemektir.

Hızlı Başlangıçlar:

- [Veri saklama ilkelerini etkinleştirme ve devre dışı bırakma](data-retention-enable-disable.md)
- [Geçmiş verilerini bekletme ilkesiyle yönetme](data-retention-cleanup.md)

## <a name="how-data-retention-works"></a>Veri saklama nasıl kullanılır

Veri bekletmesini yapılandırmak için DDL deyimlerini kullanabilirsiniz. Daha fazla bilgi için [veri bekletme Ilkelerini etkinleştirin ve devre dışı bırakın](data-retention-enable-disable.md). Artık kullanılmayan kayıtları otomatik olarak silmek için, veri saklama işleminin önce hem veritabanı hem de söz konusu veritabanı içinde temizlenmesini istediğiniz tablolar için etkinleştirilmesi gerekir. 

Veri saklama bir tablo için yapılandırıldıktan sonra bir arka plan görevi, bir tablodaki eski kayıtları tanımlamak ve bu kayıtları silmek için çalışır. Bir nedenden dolayı, görevlerin otomatik temizlenmesi çalışmaz veya silme işlemleri devam etmez, Bu tablolarda el ile temizleme işlemi gerçekleştirilebilir. Otomatik ve el ile temizlik hakkında daha fazla bilgi için [otomatik ve El Ile Temizleme](data-retention-cleanup.md)bölümüne bakın.

## <a name="limitations-and-restrictions"></a>Sınırlamalar ve kısıtlamalar

- Veri saklama, etkinleştirilirse, veritabanı tam yedeklemeden geri yüklendiğinde veya yeniden iliştirilmediğinde otomatik olarak devre dışıdır. 
- Zamana bağlı bir geçmiş tablosu için veri saklama etkinleştirilemez
- Veri saklama filtresi koomn değiştirilemez. Sütunu değiştirmek için tabloda veri bekletmeyi devre dışı bırakın.  

## <a name="next-steps"></a>Sonraki Adımlar

- [SQL Edge 'de ONNX ile Machine Learning ve yapay zeka](onnx-overview.md).
- [IoT Edge kullanarak SQL Edge ile uçtan uca IoT çözümü oluşturma](tutorial-deploy-azure-resources.md).
- [Azure SQL Edge 'de veri akışı](stream-data.md)
