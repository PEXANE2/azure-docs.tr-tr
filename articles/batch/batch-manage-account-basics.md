---
title: Hesabınızı yönetin-Azure Batch | Microsoft Docs
description: Azure Batch hesabını neyin kapsacağınızı öğrenin
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/05/2020
ms.author: labrenne
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 72630a2003b63e60ba79882e1861283173840425
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79375193"
---
# <a name="manage-your-batch-account"></a>Batch hesabınızı yönetme

Bir Batch hesabı Batch hizmeti dahilinde benzersiz şekilde tanımlanan bir varlıktır. Tüm işlemler bir Batch hesabıyla ilişkilendirilir.

[Azure portalını](batch-account-create-portal.md) kullanarak ya da [Toplu Yönetim .NET kitaplığı](batch-management-dotnet.md) ile olduğu gibi program aracılığıyla bir Azure Batch hesabı oluşturabilirsiniz. Hesabı oluştururken işle ilgili giriş veya çıkış verilerini veya uygulamaları kaydetmek için bir Azure depolama hesabı ilişkilendirebilirsiniz.

Tek bir Batch hesabında birden fazla Batch iş yükü çalıştırabilir ya da iş yüklerinizi aynı abonelik ve farklı Azure bölgelerindeki Batch hesapları arasında dağıtabilirsiniz.

## <a name="components-of-the-batch-account"></a>Batch hesabının bileşenleri

Batch hesabı, Azure 'da büyük ölçekli paralel ve yüksek performanslı bilgi işlem (HPC) toplu işlerini verimli bir şekilde çalıştırmanızı sağlar. Yönettiğiniz hesap dahilinde:

- çalıştırdığınız uygulamalar

- havuzlar içindeki havuzların ve düğümlerin ayrılması

- görev sayısı ve türleri 

- verilerin girişi ve çıkışı. Görevleri yönetmek için ek yazılım yüklemeniz gerekmez.

- Batch hesabını oluşturduğunuzda buna bir ad atamanız istenir. Bu ad, KIMLIĞI ve atandıktan sonra değiştirilemez.

- Bir hesabın adını değiştirmek için, onu silmeniz ve yeni bir Batch hesabı oluşturmanız gerekir.

- Hesap, kullanmak istediğiniz abonelik içinde oluşturulur.

- Aboneliğinizdeki herhangi bir Batch hesabından birincil ve ikincil hesap anahtarlarını tanımlamak ve almak için hesabını kullanın.

- Hesap, havuz ayırma ve çekirdek kotalar hakkındaki bilgileri saklar.  

- Hesap, konum bilgilerini içerir.

- Hesap, depolama hesabınızı tanımlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Portal](batch-account-create-portal.md)kullanarak bir Batch hesabı oluşturun.
- [Batch yönetimi .NET kitaplığı](batch-management-dotnet.md)ile gibi programlı bir şekilde Batch hesabı oluşturun.
- [Bir Azure Batch havuzundaki işlem düğümlerine uzaktan erişimi yapılandırın veya devre dışı bırakın](pool-endpoint-configuration.md).
- [Toplu işlem düğümlerinde iş hazırlama ve iş bırakma görevlerini çalıştırma](batch-job-prep-release.md)
