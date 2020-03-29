---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/19/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: cdcbe993bd1100b2060a1f8d38eb82ac97121c0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74851646"
---
Depolama hesabı kullanımınıza bağlı olarak Azure Depolama için faturalandırılırsınız. Depolama hesabındaki tüm nesneler bir grup halinde faturalandırılır. 

Depolama maliyetleri aşağıdaki faktörlere göre hesaplanır: 

* **Bölge,** hesabınızın bulunduğu coğrafi bölgeyle ifade eder.
* **Hesap türü,** kullanmakta olduğunuz depolama hesabı türünü ifade eder. 
* **Access katmanı,** genel amaçlı v2 veya Blob depolama hesabınız için belirttiğiniz veri kullanım deseni anlamına gelir.
* Depolama **Kapasitesi,** verileri depolamak için depolama hesap tahsisinizin ne kadarını kullandığınızı ifade eder.
* **Çoğaltma,** verilerinizin aynı anda kaç kopyasının tutulur ve hangi konumlarda saklanmasını belirler.
* **İşlemler,** Azure Depolama'daki tüm okuma ve yazma işlemlerini ifade eder.
* **Veri çıkış,** bir Azure bölgesinden aktarılan tüm verileri ifade eder. Depolama hesabınızdaki verilere aynı bölgede çalışmayan bir uygulama tarafından erişildiğinde, veri çıkış için ücretlendirilirsiniz. Çıkış ücretlerini sınırlamak için verilerinizi ve hizmetlerinizi aynı bölgede gruplandırmak için kaynak gruplarını kullanma hakkında bilgi için [bkz.](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group) 

[Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası hesap türüne, depolama kapasitesine, çoğaltmaya ve işlemlere bağlı olarak ayrıntılı fiyatladırma bilgileri sağlar. [Veri Aktarımları Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/data-transfers/), veri çıkışı için ayrıntılı fiyatlandırma bilgileri sağlar. Maliyetlerinizi tahmin etmeye yardımcı olması için [Azure Storage Fiyatlandırması Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?scenario=data-management)’nı kullanabilirsiniz.

