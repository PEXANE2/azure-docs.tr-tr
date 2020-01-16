---
title: Azure Güvenlik Merkezi 'nde bulut Yerel bilgi işlem için tehdit algılama | Microsoft Docs
description: Bu makalede, Azure Güvenlik Merkezi 'nde bulunan bulut Yerel işlem uyarıları sunulmaktadır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5aa5efcf-9f6f-4aa1-9f72-d651c6a7c9cd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: a85ec565077ac229cd24bb7b9cf753015aa56ebf
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76024864"
---
# <a name="threat-detection-for-cloud-native-computing-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde bulut Yerel bilgi işlem için tehdit algılama

Yerel bir çözüm olarak, Azure Güvenlik Merkezi 'nin birden çok hedef arasında saldırı yöntemi tanımlaması için iç günlüklere benzersiz görünürlüğü vardır. Bu makalede, aşağıdaki Azure hizmetleri için kullanılabilen uyarılar sunulmaktadır:

* [Azure App Service](#app-services)
* [Azure kapsayıcıları](#azure-containers) 

> [!NOTE]
> Bu hizmetler şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılabilir değildir.

## Azure App Service<a name="app-services"></a>

Güvenlik Merkezi, App Service üzerinde çalışan uygulamaları hedefleyen saldırıları belirlemek için bulutun ölçeğini kullanır. Saldırganlar, zayıf yanları bulmak ve yararlanmak için Web uygulamalarını araştırma. Belirli ortamlara yönlendirilmeden önce, Azure 'da çalışan uygulamalara yönelik istekler, incelendikleri ve günlüğe kaydedildiği çeşitli ağ geçitleri aracılığıyla yapılır. Bu veriler daha sonra kötüye kullanım ve saldırganlar tanımlamak ve daha sonra kullanılacak yeni desenler öğrenmek için kullanılır.

Güvenlik Merkezi, Azure 'un bulut sağlayıcısı olarak sahip olduğu görünürlüğü kullanarak, birden çok hedef üzerinde saldırı yöntemini tanımlamak üzere iç günlükleri App Service analiz eder. Örneğin, metodolojide, yaygın tarama ve dağıtılmış saldırılar bulunur. Bu tür bir saldırı genellikle IP 'nin küçük bir alt kümesinden gelir ve birden fazla konaktaki benzer uç noktalara gezinme düzenlerini gösterir. Saldırılar, güvenlik açığı bulunan bir sayfa veya eklenti arıyor ve tek bir konağın açısından belirlenemiyor.

Windows tabanlı bir App Service planı çalıştırıyorsanız, güvenlik merkezi 'nin temel alınan korumalı makinelere ve sanal makinelere da erişimi vardır. Yukarıda bahsedilen günlük verileriyle birlikte, altyapı hikayeye, Müşteri makinelerinden muaf olmak üzere yeni bir saldırıya karşı çok daha fazla bilgi verebilir. Bu nedenle, güvenlik merkezi bir Web uygulaması kullanıldıktan sonra dağıtılsa bile, devam eden saldırıları tespit edebilir.

Azure App Service uyarılarının listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azureappserv)bakın.

## Azure kapsayıcıları<a name="azure-containers"></a>

Güvenlik Merkezi, Kapsayıcılı ortamlarınız için gerçek zamanlı tehdit algılama sağlar ve şüpheli etkinlikler için uyarı oluşturur. Bu bilgileri kullanarak güvenlik sorunlarını hızlı bir şekilde çözebilir ve kapsayıcılarınızın güvenlik düzeyini artırabilirsiniz.

Farklı düzeylerde tehditler tespit ediyoruz: 

* **Ana bilgisayar düzeyi** -Güvenlik Merkezi 'nin Aracısı (Standart katmanda mevcuttur. Ayrıntılar için [fiyatlandırma](security-center-pricing.md) ) Linux 'u şüpheli etkinlikler için izler. Aracı, düğüm veya üzerinde çalışan bir kapsayıcıdan kaynaklanan şüpheli etkinlikler için uyarıları tetikler. Web kabuğu algılama ve bilinen şüpheli IP adresleriyle bağlantı gibi etkinliklere örnek olarak verilebilir.

    Kapsayıcılı ortamınızın güvenliğine daha ayrıntılı bir bakış için, aracı kapsayıcıya özgü Analizi izler. Ayrıcalıklı kapsayıcı oluşturma, API sunucularına şüpheli erişim ve bir Docker kapsayıcısı içinde çalışan Secure Shell (SSH) sunucuları gibi olaylar için uyarı tetikleyecektir.

    >[!NOTE]
    > Aracılarınıza aracıları yüklememeyi seçerseniz, tehdit algılama avantajları ve uyarıların yalnızca bir alt kümesini alacaksınız. Hala kötü amaçlı sunucularla ağ analizi ve iletişimlerle ilgili uyarılar alacaksınız.

    Konak düzeyi uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-containerhost)bakın.


* **Aks küme düzeyi**Için Kubernetes denetim günlükleri analizini temel alan tehdit algılama izlemesi vardır. **Aracısız** izlemeyi etkinleştirmek Için, **fiyatlandırma & ayarları** sayfasından Kubernetes seçeneğini aboneliğinize ekleyin (bkz. [fiyatlandırma](security-center-pricing.md)). Bu düzeyde uyarı oluşturmak için, güvenlik merkezi AKS tarafından yönetilen Hizmetleri AKS tarafından alınan günlükleri kullanarak izler. Bu düzeydeki olay örnekleri, sunulan Kubernetes panoları, yüksek ayrıcalıklı rollerin oluşturulmasını ve hassas takmaları oluşturmayı içerir.

    >[!NOTE]
    > Güvenlik Merkezi, Azure Kubernetes hizmet eylemleri ve abonelik ayarlarında Kubernetes seçeneği etkinleştirildikten sonra gerçekleşen dağıtımlar için algılama uyarıları oluşturur. 

    AKS küme düzeyi uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-akscluster)bakın.

Ayrıca, güvenlik araştırmacıları küresel takımımız tehdidi sürekli olarak izler. Bulundukları gibi kapsayıcıya özgü uyarıları ve güvenlik açıklarını ekler.

## <a name="next-steps"></a>Sonraki adımlar

* App Service planları hakkında daha fazla bilgi için bkz. [App Service planları](https://azure.microsoft.com/pricing/details/app-service/plans/).