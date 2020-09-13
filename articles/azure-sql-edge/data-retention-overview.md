---
title: Veri saklama ilkesine genel bakış-Azure SQL Edge (Önizleme)
description: Azure SQL Edge 'de veri bekletme ilkesi hakkında bilgi edinin (Önizleme)
keywords: SQL Edge, veri saklama
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/04/2020
ms.openlocfilehash: 3649d4f77e5b57ab14accacd87fbaa867ba2742f
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89550725"
---
# <a name="data-retention-policy-overview"></a>Veri saklama ilkesine genel bakış

Bağlı IoT cihazlarındaki verilerin toplanması ve depolanması, işletimsel ve iş öngörülerini sağlamak için önemlidir. Ancak, bu cihazlardan kaynaklanan verilerin hacmi verildiğinde, kuruluşların sürdürmek istedikleri veri miktarını ve ayrıntı düzeyini dikkatle planlayıp planmaları önemli hale gelir. Tüm verilerin her ayrıntı düzeyinde bekletilmesini tercih ederken her zaman pratik değildir. Ayrıca, tutulabileceği veri hacmi, IoT veya uç cihazlarda bulunan depolama miktarı ile sınırlıdır. 

Azure SQL Edge (Önizleme) CTP 2.3, veritabanı yöneticilerinin bir SQL Edge veritabanında ve temel aldığı tablolarda veri bekletme ilkesi tanımlamasına olanak sağlayan yeni bir özellik ekler. Veri saklama ilkesi tanımlandıktan sonra, Kullanıcı tablolarından kullanılmayan (eski) verileri temizlemek için bir arka plan sistem görevi çalıştırılır. 

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

## <a name="next-steps"></a>Sonraki Adımlar

- [SQL Edge 'de ONNX ile Machine Learning ve yapay zeka](onnx-overview.md).
- [IoT Edge kullanarak SQL Edge ile uçtan uca IoT çözümü oluşturma](tutorial-deploy-azure-resources.md).
- [Azure SQL Edge 'de veri akışı](stream-data.md)
