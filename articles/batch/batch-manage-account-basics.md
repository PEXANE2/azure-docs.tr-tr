---
title: Hesabınızı yönetme
description: Azure Batch hesabını neyin kapsacağınızı öğrenin
ms.topic: conceptual
ms.date: 03/30/2020
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ce0c612e3434249c0c7d078949b10e0b9f6b1f10
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82116018"
---
# <a name="manage-your-batch-account"></a>Batch hesabınızı yönetme

Bir Batch hesabı Batch hizmeti dahilinde benzersiz şekilde tanımlanan bir varlıktır. Tüm işlemler bir Batch hesabıyla ilişkilendirilir.

[Azure portalını](batch-account-create-portal.md) kullanarak ya da [Toplu Yönetim .NET kitaplığı](batch-management-dotnet.md) ile olduğu gibi program aracılığıyla bir Azure Batch hesabı oluşturabilirsiniz. Hesabı oluştururken işle ilgili giriş veya çıkış verilerini veya uygulamaları kaydetmek için bir Azure depolama hesabı ilişkilendirebilirsiniz.

Tek bir Batch hesabında birden fazla Batch iş yükü çalıştırabilir ya da iş yüklerinizi aynı abonelik ve farklı Azure bölgelerindeki Batch hesapları arasında dağıtabilirsiniz.

## <a name="components-of-the-batch-account"></a>Batch hesabının bileşenleri

Batch hesabı, Azure 'da büyük ölçekli paralel ve yüksek performanslı bilgi işlem (HPC) toplu işlerini verimli bir şekilde çalıştırmanızı sağlar. Yönettiğiniz hesap dahilinde:

- Çalıştırdığınız uygulamalar

- Havuzlar içindeki havuzların ve düğümlerin ayrılması

- Görev sayısı ve türleri 

- Verilerin girişi ve çıkışı. Görevleri yönetmek için ek yazılım yüklemeniz gerekmez.

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

