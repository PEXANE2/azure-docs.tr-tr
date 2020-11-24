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
ms.openlocfilehash: fcfe05db6a9be1049ca5da06985f31135ac79f3b
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95555373"
---
Azure depolama için depolama hesabı kullanımınıza göre faturalandırılırsınız. Depolama hesabındaki tüm nesneler bir grup halinde faturalandırılır. 

Depolama maliyetleri aşağıdaki etkenlere göre hesaplanır: 

* **Bölge** , hesabınızın dayandığı coğrafi bölgeyi ifade eder.
* **Hesap türü** , kullanmakta olduğunuz depolama hesabının türünü belirtir. 
* **Erişim katmanı** , genel amaçlı v2 veya blob depolama hesabınız için belirttiğiniz veri kullanımı modelini ifade eder.
* Depolama **kapasitesi** , veri depolamak için kullandığınız depolama hesabı servis biriminden ne kadarının olduğunu ifade eder.
* **Çoğaltma** , verilerinizin kaç kopyasının aynı anda ve hangi konumlarda tutulmasını belirler.
* **İşlemler** , Azure depolama 'ya yönelik tüm okuma ve yazma işlemlerine başvurur.
* **Veri** çıkışı, bir Azure bölgesinden aktarılan tüm verileri ifade eder. Depolama hesabınızdaki verilere aynı bölgede çalışmayan bir uygulama tarafından erişildiğinde, veri çıkışı için ücretlendirilirsiniz. Kaynak gruplarını, çıkış ücretlerini sınırlandırmak için aynı bölgedeki veri ve hizmetlerinizi gruplandırmak üzere kullanma hakkında bilgi için bkz. [Azure Kaynak grubu nedir?](/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group). 

[Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası hesap türüne, depolama kapasitesine, çoğaltmaya ve işlemlere bağlı olarak ayrıntılı fiyatladırma bilgileri sağlar. [Veri Aktarımları Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/data-transfers/), veri çıkışı için ayrıntılı fiyatlandırma bilgileri sağlar. Maliyetlerinizi tahmin etmeye yardımcı olması için [Azure Storage Fiyatlandırması Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?scenario=data-management)’nı kullanabilirsiniz.