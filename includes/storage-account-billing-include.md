---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/19/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: cdcbe993bd1100b2060a1f8d38eb82ac97121c0d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74851646"
---
Azure depolama için depolama hesabı kullanımınıza göre faturalandırılırsınız. Depolama hesabındaki tüm nesneler bir grup halinde faturalandırılır. 

Depolama maliyetleri aşağıdaki etkenlere göre hesaplanır: 

* **Bölge** , hesabınızın dayandığı coğrafi bölgeyi ifade eder.
* **Hesap türü** , kullanmakta olduğunuz depolama hesabının türünü belirtir. 
* **Erişim katmanı** , genel amaçlı v2 veya blob depolama hesabınız için belirttiğiniz veri kullanımı modelini ifade eder.
* Depolama **kapasitesi** , veri depolamak için kullandığınız depolama hesabı servis biriminden ne kadarının olduğunu ifade eder.
* **Çoğaltma** , verilerinizin kaç kopyasının aynı anda ve hangi konumlarda tutulmasını belirler.
* **İşlemler** , Azure depolama 'ya yönelik tüm okuma ve yazma işlemlerine başvurur.
* **Veri** çıkışı, bir Azure bölgesinden aktarılan tüm verileri ifade eder. Depolama hesabınızdaki verilere aynı bölgede çalışmayan bir uygulama tarafından erişildiğinde, veri çıkışı için ücretlendirilirsiniz. Kaynak gruplarını, çıkış ücretlerini sınırlandırmak için aynı bölgedeki veri ve hizmetlerinizi gruplandırmak üzere kullanma hakkında bilgi için bkz. [Azure Kaynak grubu nedir?](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group). 

[Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası hesap türüne, depolama kapasitesine, çoğaltmaya ve işlemlere bağlı olarak ayrıntılı fiyatladırma bilgileri sağlar. [Veri Aktarımları Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/data-transfers/), veri çıkışı için ayrıntılı fiyatlandırma bilgileri sağlar. Maliyetlerinizi tahmin etmeye yardımcı olması için [Azure Storage Fiyatlandırması Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?scenario=data-management)’nı kullanabilirsiniz.

