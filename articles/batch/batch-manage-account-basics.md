---
title: Hesabınızı yönetin- Azure Toplu İş | Microsoft Dokümanlar
description: Azure Toplu İş hesabının nelerden oluştuğunu öğrenin
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
ms.date: 03/30/2020
ms.author: labrenne
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ecae47f6aa0ab3f179632467b7da7805f06162d6
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397255"
---
# <a name="manage-your-batch-account"></a>Toplu İşlem hesabınızı yönetme

Bir Batch hesabı Batch hizmeti dahilinde benzersiz şekilde tanımlanan bir varlıktır. Tüm işlemler bir Batch hesabıyla ilişkilendirilir.

[Azure portalını](batch-account-create-portal.md) kullanarak ya da [Toplu Yönetim .NET kitaplığı](batch-management-dotnet.md) ile olduğu gibi program aracılığıyla bir Azure Batch hesabı oluşturabilirsiniz. Hesabı oluştururken işle ilgili giriş veya çıkış verilerini veya uygulamaları kaydetmek için bir Azure depolama hesabı ilişkilendirebilirsiniz.

Tek bir Batch hesabında birden fazla Batch iş yükü çalıştırabilir ya da iş yüklerinizi aynı abonelik ve farklı Azure bölgelerindeki Batch hesapları arasında dağıtabilirsiniz.

## <a name="components-of-the-batch-account"></a>Toplu İşlem hesabının bileşenleri

Toplu İşlem hesabı, Azure'da büyük ölçekli paralel ve yüksek performanslı bilgi işlem (HPC) toplu işlerini verimli bir şekilde çalıştırmanızı sağlar. Yönettiğiniz hesap içinde:

- Çalıştırdığınız uygulamalar

- Havuzlar içinde havuz ve düğüm tahsisi

- Görev sayısı ve türleri 

- Verilerin girişi ve çıktısı. Görevleri yönetmek için ek yazılım yüklemeniz gerekmez.

- Toplu İş hesabı oluşturduğunuzda, bu hesaba bir ad atamanız istenir. Bu ad onun kimliğidir ve bir kez atandıktan sonra değiştirilemez.

- Bir hesabın adını değiştirmek için hesabı silmeniz ve yeni bir Toplu İşlem hesabı oluşturmanız gerekir.

- Hesap, kullanmak istediğiniz abonelik içinde oluşturulur.

- Aboneliğinizdeki herhangi bir Toplu Iş hesabından birincil ve ikincil hesap anahtarlarını tanımlamak ve almak için hesabı kullanın.

- Hesap, havuz tahsisi ve çekirdek kotalar hakkında bilgi tutar.  

- Hesap konum bilgilerini içerir.

- Hesap, depolama hesabınızı tanımlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalını](batch-account-create-portal.md)kullanarak toplu iş hesabı oluşturun.
- [Toplu İşlem Yönetimi .NET kitaplığı](batch-management-dotnet.md)gibi programlı bir Toplu Iş hesabı oluşturun.
- [Azure Toplu İş havuzunda bilgi düğümleri için uzaktan erişimi yapılandırma](pool-endpoint-configuration.md)veya devre dışı bırakın.
- [Toplu işlem düğümlerinde iş hazırlama ve iş bırakma görevlerini çalıştırma](batch-job-prep-release.md)

