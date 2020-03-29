---
title: Azure DevTest Labs ve DevOps Entegrasyonu | Microsoft Dokümanlar
description: Kurumsal bir ortamda sürekli tümleştirme (CI)/ sürekli teslimat (CD) boru hatları içinde Azure DevTest Labs laboratuvarlarını nasıl kullanacağınızı öğrenin.
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: spelluru
ms.openlocfilehash: 62c44bfea28d47d7c32aa7ef440a40d45c314683
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67078930"
---
# <a name="integration-of-azure-devtest-labs-and-azure-devops"></a>Azure DevTest Labs ve Azure DevOps entegrasyonu
DevOps, yazılım geliştirme (Dev) ile bir sistem için operasyonlar (Ops) entegre eden bir yazılım geliştirme metodolojisidir. Bu sistem, iş hedefleriyle uyumlu yeni özellikler, güncelleştirmeler ve düzeltmeler sunabilir. Bu metodoloji, hedeflere, kullanım kalıplarını ve müşteri geri bildirimlerine dayalı yeni özellikler tasarlamaktan her şeyi kapsar; sorunlar oluştuğunda sistemi düzeltmek, kurtarmak ve sertleştirmek. Bu metodolojinin kolayca tanımlanan bir bileşeni sürekli entegrasyon (CI)/ sürekli teslimat (CD) boru hattıdır. BIR CI/CD ardışık işlem, sistemi üretmek için oluşturma, sınama ve dağıtımı içeren bir dizi adım aracılığıyla bir commit'den bilgi, kod ve kaynak alır. Bu makalede, bir kuruluş ortamında bir boru hattı içindeki laboratuvarları etkili bir şekilde kullanmanın farklı yolları üzerinde duruluyor. 

## <a name="benefits-of-using-labs-in-devops-workflow"></a>DevOps iş akışında laboratuvar kullanmanın faydaları 

### <a name="focused-access"></a>Odaklanmış erişim 
Bir laboratuarı bileşen olarak kullanmak, belirli bir ekosistemin sınırlı bir grup insanla ilişki yemesine olanak tanır. Genellikle, ortak bir alanda veya belirli bir özellikte çalışan bir ekip veya grup, onlara atanmış bir laboratuara sahip olur.   

### <a name="infrastructure-replication-in-the-cloud"></a>Bulutta altyapı çoğaltma 
Geliştirici, kaynak kodu ve araçları içeren bir geliştirme kutusu içeren bir geliştirme ekosistemini hızla ayarlayabilir. Geliştirici, üretim yapılandırması ile neredeyse aynı olan bir ortam da oluşturabilir. Bu daha hızlı iç döngü geliştirme ile yardımcı olur. 

### <a name="pre-production"></a>Üretim öncesi 
CI/CD ardışık boru hattında bir laboratuvara sahip olmak, birden fazla farklı üretim öncesi ortamın veya makinelerin eşzamanlı test için aynı anda çalışmasını kolaylaştırır. Yapı aracıları gibi farklı destek altyapıları bir laboratuvar içinde dağıtılabilir ve yönetilebilir. 

## <a name="devops-with-devtest-labs"></a>DevTest Labs ile DevOps 

### <a name="development--operation"></a>Geliştirme / Operasyon 
Laboratuvar, özellik alanında çalışan bir ekibe odaklanmalıdır. Bu ortak odak, araçlar, komut dosyaları veya Kaynak Yöneticisi şablonları gibi alana özgü kaynakların paylaşılmasına olanak tanır. Olumsuz etkileri daha küçük bir grupla sınırlandırırken daha hızlı değişikliklere olanak sağlar. Bu paylaşılan kaynaklar, geliştiricinin gerekli tüm kod, araç ve yapılandırmayla geliştirme VM'leri oluşturmasına olanak sağlar. Bunlar dinamik olarak oluşturulabilir veya özelleştirmelerle temel görüntüler oluşturan bir sisteme sahip olabilir. Geliştirici yalnızca VM'ler oluşturmakla kalmamış, aynı zamanda laboratuvarda uygun Azure kaynaklarını oluşturmak için gerekli şablonları temel alan DevTest Labs ortamları oluşturabilir. Herhangi bir değişiklik veya yıkıcı çalışma başka kimseyi etkilemeden laboratuvar ortamına karşı yapılabilir. Ürünün müşterinin makinesine yüklenmiş bağımsız bir sistem olduğu senaryoyu düşünün. Bu senaryoda, DevTest Labs, kodların daha hızlı iç döngü testi için yapıları kullanarak ek yazılım yüklemeyi ve müşteri yapılandırmalarını önceden oluşturmayı içeren VM oluşturmayı geliştirmiştir. 
  
## <a name="cicd-pipeline"></a>CI/CD boru hattı 
CI/CD ardışık, DevOps'teki kodu geliştiricinin çekme isteğinden hareket ettiren, varolan kodla bütünleştiren ve üretim ekosistemine dağıtan kritik bileşenlerden biridir. Tüm kaynakların laboratuarda olması gerekmez. Örneğin, bir Jenkins ana bilgisayar daha kalıcı bir kaynak olarak laboratuvar dışında kurulabilir. Laboratuvarları boru hattına entegre etmenin bazı özel örnekleri aşağıda verilmiştir. 

### <a name="build"></a>Oluşturma 
Yapı boru hattı, serbest bırakma boru hattına teslim edilecek şekilde birlikte test edilecek bir bileşen paketi oluşturmaya odaklanmıştır. Laboratuvarlar, yapı aracıları ve diğer destek kaynakları için konum olarak yapı ardışık hattının bir parçası olabilir. Altyapıyı dinamik olarak oluşturma yeteneğine sahip olmak daha fazla denetim sağlar. Bir laboratuvarda birden çok ortama sahip olma özelliği sayesinde, her yapı, belirli yapının kaynaklarını benzersiz olarak tanımlamak için yapı kimliğini ortam bilgilerinin bir parçası olarak kullanırken eş zamanlı olarak çalıştırılabilir.   

Yapı aracıları için, laboratuvarın erişimi kısıtlama yeteneği güvenliği artırır ve kazara bozulma olasılığını azaltır.  

### <a name="test"></a>Test 
DevTest Labs, otomatik ve manuel sınama için kullanılabilecek Azure Kaynağının (VM'ler, ortamlar) oluşturulmasını otomatikleştirmek için bir CI/CD ardışık kuruluşunu sağlar. VM'ler, sınama için gerekli olan farklı özel yapılandırmaları oluşturmak için yapı işlemindeki bilgileri kullanan yapılar veya formüller kullanılarak oluşturulur.   

### <a name="release"></a>Yayınla 
DevTest Labs, kod dağıtılmadan önce sürüm bölümünde doğrulama için yaygın olarak kullanılır. Yapı bölümündeki sınamaya benzer. Üretim kaynakları DevTest Labs içinde dağıtılmamalıdır. 

### <a name="customization"></a>Özelleştirme 
Azure DevOps'lerde, belirli laboratuarlarda VM'lerin ve ortamların manipülasyonuna izin veren varolan görevler vardır. Azure DevOps Hizmetleri CI/CD ardışık hattını yönetmenin bir yolu olsa da, laboratuarı REST API'lerini arama, PowerShell komut dosyalarını çalıştırma veya Azure CLI'yi kullanma olanağını destekleyen herhangi bir sisteme entegre edebilirsiniz. 

Bazı CI/CD boru hattı yöneticileri, Azure ve DevTest Labs içindeki kaynakları yönetebilen mevcut açık kaynak eklentilerine sahipken. Ardışık nedenin belirli gereksinimlerine uyacak şekilde bazı özel komut dosyası kullanmanız gerekebilir.  Bunu göz önünde bulundurarak, bir görevi yürürken, bir hizmet yöneticisi laboratuvara erişmek için uygun rolü kullanır. Hizmet sorumlusunun genellikle katılımcının laboratuvara rol erişimine ihtiyacı vardır. Daha fazla bilgi için azure [DevTests Laboratuvarlarını Azure DevOps sürekli tümleştirme ve teslim boru hattınıza entegre edin.](devtest-lab-integrate-ci-cd-vsts.md) 
 