---
title: Azure ve bağımsız Hizmet Kumaş ı kümelerine genel bakış
description: Windows Server veya Linux çalıştıran tüm VM'lerde veya bilgisayarlarda Service Fabric kümeleri oluşturabilirsiniz. Bu, Service Fabric uygulamalarını şirket içinde, Microsoft Azure'a veya herhangi bir bulut sağlayıcısına bağlı windows server veya Linux bilgisayarları kümesine sahip olduğunuz herhangi bir ortamda dağıtabileceğiniz ve çalıştırabileceğiniz anlamına gelir.
author: dkkapur
ms.topic: conceptual
ms.date: 01/07/2020
ms.author: dekapur
ms.custom: sfrev
ms.openlocfilehash: a3627effe10039ded5007f9dd060bf1865929040
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75751148"
---
# <a name="comparing-azure-and-standalone-service-fabric-clusters-on-windows-server-and-linux"></a>Windows Server ve Linux'ta Azure ve bağımsız Hizmet Dokusu kümelerini karşılaştırma

Service Fabric kümesi, mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal veya fiziksel makineler kümesidir. Kümenin bir parçası olan bir makine veya VM'ye küme düğümü denir. Kümeler binlerce düğüme ölçeklenebilir. Kümeye yeni düğümler eklerseniz, Service Fabric artan düğüm sayısı boyunca hizmet bölümü yinelemelerini ve örneklerini yeniden dengeler. Genel uygulama performansı artırır ve belleğe erişim için çekişme azalır. Kümedeki düğümler verimli bir şekilde kullanılıyorsa, kümedeki düğüm sayısını azaltabilirsiniz. Service Fabric, her düğümdeki donanımı daha iyi kullanmak için bölüm yinelemelerini ve örnekleri azalan düğüm sayısı boyunca yeniden dengeler.

Service Fabric, Windows Server veya Linux çalıştıran herhangi bir VM veya bilgisayarda Service Fabric kümelerinin oluşturulmasına olanak tanır. Bu, Service Fabric uygulamalarını, şirket içinde Microsoft Azure veya herhangi bir bulut sağlayıcısıyla bağlantılı bir dizi Windows Server veya Linux bilgisayarının bulunduğu herhangi bir ortamda dağıtabileceğiniz ve çalıştırabileceğiniz anlamına gelir.

## <a name="benefits-of-clusters-on-azure"></a>Azure'da kümelerin avantajları

Azure'da, kümenin işlemlerini ve yönetimini daha kolay ve daha güvenilir hale getiren diğer Azure özellikleri ve hizmetleriyle entegrasyon sağlarız.

* **Azure portalı:** Azure portalı küme oluşturmayı ve yönetmeyi kolaylaştırır.
* **Azure Kaynak Yöneticisi:** Azure Kaynak Yöneticisi'nin kullanımı, küme tarafından birim olarak kullanılan tüm kaynakların kolay yönetilmesini sağlar ve maliyet izleme ve faturalandırmayı kolaylaştırır.
* **Azure Kaynağı Olarak Hizmet Kumaş Kümesi** Hizmet Kumaşı kümesi bir Azure kaynağıdır, böylece Azure'daki diğer kaynaklarda yaptığınız gibi modelleyebilirsiniz.
* **Azure Altyapısıyla Tümleştirme** Service Fabric, uygulamalarınızın kullanılabilirliğini ve güvenilirliğini artırmak için işletim sistemi, ağ ve diğer yükseltmeler için temel Azure altyapısıyla eşgüdüm sağlar.  
* **Tanılama:** Azure'da, Azure tanılama ve Azure Monitör günlükleriyle tümleştirme sağlıyoruz.
* **Otomatik ölçekleme:** Azure'daki kümeler için, Sanal Makine ölçek kümeleri nedeniyle yerleşik otomatik ölçeklendirme işlevselliği sağlarız. Şirket içi ve diğer bulut ortamlarında, Service Fabric'in ölçeklendirme kümeleri için ortaya çıkardığı API'leri kullanarak kendi otomatik ölçekleme özelliğinizi oluşturmanız veya ölçeklendirmeniz gerekir.

## <a name="benefits-of-standalone-clusters"></a>Bağımsız kümelerin faydaları

* Kümenizi barındıracak herhangi bir bulut sağlayıcısıseçmekte özgürsunuz.
* Service Fabric uygulamaları, yazıldıktan sonra, en az değişiklik olmadan birden fazla barındırma ortamlarında çalıştırılabilir.
* Bina Hizmet Kumaş uygulamaları bilgisi bir barındırma ortamından diğerine taşır.
* Service Fabric kümelerini çalıştırma ve yönetme operasyonel deneyimi bir ortamdan diğerine taşınır.
* Geniş müşteri erişimi, barındırma ortamı kısıtlamaları yla sınırsızdır.
* Bir veri merkezi veya bulut sağlayıcısında kesinti varsa, hizmetleri başka bir dağıtım ortamına taşıyabileceğiniziçin, yaygın kesintilere karşı ekstra bir güvenilirlik ve koruma katmanı vardır.

## <a name="next-steps"></a>Sonraki adımlar

* Azure'da [Hizmet Kumaşı kümelerine](service-fabric-azure-clusters-overview.md) genel bakışı okuyun
* Service Fabric [bağımsız kümelerinin](service-fabric-standalone-clusters-overview.md) genel görünümünü okuyun
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin