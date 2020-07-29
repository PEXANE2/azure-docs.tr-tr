---
title: Azure Geçişi aleti mimarisi
description: Sunucu değerlendirmesi ve geçişte kullanılan Azure geçişi gerecine genel bakış sağlar.
ms.topic: conceptual
ms.date: 06/09/2020
ms.openlocfilehash: 0752f7afa7ff8d25f938084fd9e6e863d885f9aa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84770909"
---
# <a name="azure-migrate-appliance-architecture"></a>Azure Geçişi aleti mimarisi

Bu makalede Azure geçiş gereci mimarisi ve süreçler açıklanmaktadır. Azure geçişi gereci, Azure 'a geçiş için VM 'Leri ve fiziksel sunucuları bulmayı sağlayan, şirket içinde dağıtılan hafif bir gereç. 

## <a name="deployment-scenarios"></a>Dağıtım senaryoları

Azure geçişi gereci aşağıdaki senaryolarda kullanılır.

**Senaryo** | **Araç** | **Kullanıldığı yerler** 
--- | --- | ---
**VMware VM değerlendirmesi** | Azure geçişi: Sunucu değerlendirmesi | VMware VM 'lerini bulun.<br/><br/> Makine uygulamalarını ve bağımlılıklarını bulun.<br/><br/> Makine meta verilerini ve performans meta verilerini toplayın ve Azure 'a gönderin.
**VMware VM geçişi (aracısız)** | Azure geçişi: sunucu geçişi | VMware VM 'lerini bulma<br/><br/>  VMware VM 'lerini [aracısız geçişle](server-migrate-overview.md)çoğaltın.
**Hyper-V VM değerlendirmesi** | Azure geçişi: Sunucu değerlendirmesi | Hyper-V VM 'lerini bulun.<br/><br/> Makine meta verilerini ve performans meta verilerini toplayın ve Azure 'a gönderin.
**Fiziksel makine** |  Azure geçişi: Sunucu değerlendirmesi |  Fiziksel sunucuları bulun.<br/><br/> Makine meta verilerini ve performans meta verilerini toplayın ve Azure 'a gönderin.

## <a name="appliance-components"></a>Gereç bileşenleri

Gereç çok sayıda bileşene sahiptir.

- **Yönetim uygulaması**: Bu, Gereç dağıtımı sırasında Kullanıcı girişi için bir Web uygulamasıdır. Makineleri Azure 'a geçiş için değerlendirmek için kullanılır.
- **Keşif Aracısı**: aracı makine yapılandırma verilerini toplar. Makineleri Azure 'a geçiş için değerlendirmek için kullanılır. 
- **Değerlendirme Aracısı**: aracı performans verilerini toplar. Makineleri Azure 'a geçiş için değerlendirmek için kullanılır.
- **DRA Aracısı**: VM çoğaltmasını düzenleyin ve çoğaltılan makineler ile Azure arasındaki iletişimi koordine edin. Yalnızca VMware VM 'Leri aracısız geçiş kullanılarak Azure 'a çoğaltılırken kullanılır.
- **Ağ geçidi**: çoğaltılan verileri Azure 'a gönderir. Yalnızca VMware VM 'Leri aracısız geçiş kullanılarak Azure 'a çoğaltılırken kullanılır.
- **Otomatik güncelleştirme hizmeti**: gereç bileşenlerini güncelleştirir (24 saatte bir çalışır).



## <a name="appliance-deployment"></a>Gereç dağıtımı

- Azure geçişi gereci, [Hyper-V](how-to-set-up-appliance-hyper-v.md) veya [VMware](how-to-set-up-appliance-vmware.md) için bir şablon kullanılarak veya [VMware/Hyper-V](deploy-appliance-script.md)için bir PowerShell betiği yükleyicisi kullanılarak veya [fiziksel sunucular](how-to-set-up-appliance-physical.md)için ayarlanabilir. 
- Gereç desteği gereksinimleri ve dağıtım önkoşulları, [gereç desteği matrisinde](migrate-appliance.md)özetlenir.


## <a name="appliance-registration"></a>Gereç kaydı

Gereç kurulumu sırasında gereci Azure geçişi ile kaydedersiniz ve tabloda özetlenen eylemler oluşur.

**Eylem** | **Ayrıntılar** | **İzinler**
--- | --- | ---
**Kaynak sağlayıcılarını Kaydet** | Bu kaynak sağlayıcıları, Gereç kurulumu sırasında seçtiğiniz aboneliğe kaydedilir: Microsoft. OffAzure, Microsoft. Migrate ve Microsoft. Keykasası.<br/><br/> Kaynak sağlayıcısı kaydı, aboneliğinizi kaynak sağlayıcısıyla çalışacak şekilde yapılandırır. | Kaynak sağlayıcılarını kaydetmek için abonelikte bir katkıda bulunan veya sahip rolü gerekir.
**Azure AD uygulaması oluşturma-iletişim** | Azure geçişi, Gereç üzerinde çalışan aracılar ve Azure üzerinde çalışan ilgili hizmetleri arasında iletişim için bir Azure Active Directory (Azure AD) uygulaması oluşturur.<br/><br/> Bu uygulamanın herhangi bir kaynakta Azure Resource Manager çağrısı yapma ayrıcalıkları yoktur veya erişim izni yoktur. | Uygulamayı oluşturmak için Azure geçişi için [Bu izinlere](tutorial-prepare-vmware.md#assign-permissions-to-create-azure-ad-apps) ihtiyacınız vardır.
**Azure AD uygulamaları oluşturma-Anahtar Kasası** | Bu uygulama yalnızca VMware VM 'lerinin Azure 'a aracısız geçişi için oluşturulmuştur.<br/><br/> Bu, özel olarak, kullanıcının aracısız geçiş için Kullanıcı aboneliğinde oluşturulan anahtar kasasına erişmek için kullanılır.<br/><br/> Bu, gereçden bulma başlatıldığında Azure Anahtar Kasası 'nda (müşterinin kiracısında oluşturulur | Uygulamayı oluşturmak için Azure geçişi için [Bu izinlere](tutorial-prepare-vmware.md#assign-permissions-to-create-a-key-vault) ihtiyacınız vardır.



## <a name="collected-data"></a>Toplanan veriler

Tüm dağıtım senaryoları için istemci tarafından toplanan veriler [gereç desteği matrisinde](migrate-appliance.md)özetlenir.

## <a name="discovery-and-collection-process"></a>Bulma ve toplama işlemi

![Mimari](./media/migrate-appliance-architecture/architecture.png)

Gereç, aşağıdaki işlemi kullanarak vCenter sunucularıyla ve Hyper-V konaklarıyla/kümesiyle iletişim kurar.

1. **Bulmayı Başlat**:
    - Hyper-V gereci üzerinde bulmayı başlattığınızda, WinRM bağlantı noktaları 5985 (HTTP) ve 5986 (HTTPS) üzerindeki Hyper-V konaklarıyla iletişim kurar.
    - VMware gereci üzerinde bulmayı başlattığınızda, varsayılan olarak TCP bağlantı noktası 443 üzerindeki vCenter Server ile iletişim kurar. VCenter sunucusu farklı bir bağlantı noktasını dinliyorsa, bunu gereç Web uygulamasında yapılandırabilirsiniz.
2. **Meta verileri ve performans verilerini toplayın**:
    - Gereç, 5985 ve 5986 bağlantı noktalarında Hyper-V konağı üzerinden Hyper-V VM verilerini toplamak için bir Genel Bilgi Modeli (CıM) oturumu kullanır.
    - Gereç, vCenter Server VMware VM verilerini toplamak için varsayılan olarak bağlantı noktası 443 ile iletişim kurar.
3. **Veri Gönder**: gereç, toplanan verileri Azure geçişi sunucu değerlendirmesini ve Azure geçişi sunucu geçişini SSL bağlantı noktası 443 üzerinden gönderir. Gereç Internet üzerinden Azure 'a bağlanabilir veya ExpressRoute 'u ortak/Microsoft eşlemesi ile birlikte kullanabilirsiniz.
    - Performans verileri için, Gereç gerçek zamanlı kullanım verilerini toplar.
        - Performans verileri her bir performans ölçümü için VMware için 20 saniyede bir ve Hyper-V için her 30 saniyede bir toplanır.
        - Toplanan veriler 10 dakika boyunca tek bir veri noktası oluşturmak için toplanır.
        - En yüksek kullanım değeri tüm 20/30 saniyelik veri noktalarından seçilir ve değerlendirme hesaplaması için Azure 'a gönderilir.
        - Değerlendirme özelliklerinde belirtilen yüzdebirlik değerine (50./90./yüzde/sn) göre, on dakikalık noktaları artan düzende sıralanır ve değerlendirmeyi hesaplamak için uygun yüzdebirlik değeri kullanılır
    - Sunucu geçişi için, Gereç VM verilerini toplamaya başlar ve bunu Azure 'a çoğaltır.
4. **Değerlendirin ve geçirin**: artık Azure geçişi sunucu değerlendirmesini kullanarak gereç tarafından toplanan meta verilerden değerlendirmeler oluşturabilirsiniz. Ayrıca, Azure geçişi sunucu geçişini kullanarak VMware VM 'Leri geçirmeyi daha az VM çoğaltmasını düzenlemek için de başlatabilirsiniz.





## <a name="appliance-upgrades"></a>Gereç yükseltmeleri

Gereç üzerinde çalışan Azure geçiş aracıları güncelleştirildiğinden, Gereç yükseltilir. Otomatik güncelleştirme, Gereç üzerinde varsayılan olarak etkinleştirildiğinden bu otomatik olarak gerçekleşir. Aracıları el ile güncelleştirmek için bu varsayılan ayarı değiştirebilirsiniz.

HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\AzureAppliance "otomatik güncelleştirme" anahtarını 0 (DWORD) olarak ayarlayarak, kayıt defterinde otomatik güncelleştirmeyi devre dışı bırakabilirsiniz.

 

## <a name="next-steps"></a>Sonraki adımlar

Gereç destek matrisini [gözden geçirin](migrate-appliance.md) .

