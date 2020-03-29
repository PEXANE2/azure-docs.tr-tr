---
title: Azure Güvenlik Merkezi'nde kapsayıcılarınızın güvenliğini izleme
description: Azure Güvenlik Merkezi'nden kapsayıcılarınızın güvenlik duruşunu nasıl kontrol edebilirsiniz öğrenin
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: memildin
ms.openlocfilehash: 330cbc3f28f5e549d5a21417c3d7ccc1e5444769
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919541"
---
# <a name="monitoring-the-security-of-your-containers"></a>Konteynerlerinizin güvenliğini izleme

Bu sayfa, kavramlar bölümünde Kapsayıcı Güvenlik [makalesinde](container-security.md) açıklanan kapsayıcı güvenlik özelliklerinin nasıl kullanılacağını açıklamaktadır.

Azure Güvenlik Merkezi, kapsayıcı güvenliğinin aşağıdaki üç yönünü kapsar:

- **Güvenlik Açığı yönetimi** - Güvenlik Merkezi'nin standart fiyatlandırma katmanındaysanız [(fiyatlandırmaya](/azure/security-center/security-center-pricing)bakın), yeni bir resim her itildiğinde ARM tabanlı Azure Konteyner Kayıt Defterinizi tekleyebilirsiniz. Tarayıcı (Qualys tarafından desteklenmektedir) Güvenlik Merkezi önerileri olarak bulgular sunar.
    Ayrıntılı talimatlar için, aşağıdaki [güvenlik açıkları için konteyner kayıt defterlerinizi tarama ya](#scanning-your-arm-based-container-registries-for-vulnerabilities) da

- **Konteynerlerinizin Docker ana bilgisayarlarını sertleştirme** - Security Center, Docker çalıştıran IaaS Linux VM'lerde veya diğer Linux makinelerinde barındırılan yönetilmeyen konteynerleri bulur ve konteynerlerin yapılandırmalarını Sürekli Olarak Internet Security (CIS) Docker Benchmark Merkezi ile karşılaştırır. Güvenlik Merkezi, kapsayıcılarınız denetimlerin hiçbirini karşılamazsa sizi uyarır. Yanlış yapılandırmalar nedeniyle güvenlik riskleri için sürekli izleme herhangi bir güvenlik programının önemli bir bileşenidir. 
    Ayrıntılı talimatlar için, [aşağıdaki konteynerlerin Docker ana bilgisayarlarını sertleştirme](#hardening-your-containers-docker-hosts) ye bakın.

- **Azure Kubernetes Hizmet kümelerinizi sertleştirme** - Güvenlik Merkezi, Azure Kubernetes Hizmet kümelerinizin yapılandırmasında güvenlik açıkları bulduğunda öneriler sağlar. Görünebilecek özel önerilerin ayrıntıları için [Kubernetes Hizmet önerilerine](recommendations-reference.md#recs-containers)bakın.

- **Çalışma zamanı koruması** - Güvenlik Merkezi'nin standart fiyatlandırma katmanındaysanız, konteynerleştirilmiş ortamlarınız için gerçek zamanlı tehdit koruması elde elabilirsiniz. Güvenlik Merkezi, ana bilgisayar ve AKS küme düzeyindeki şüpheli etkinlikler için uyarılar oluşturur. Görünebilecek ilgili güvenlik uyarılarının ayrıntıları için, [Azure Kubernetes Hizmet kümeleri için Uyarılar](alerts-reference.md#alerts-akscluster) ve [kapsayıcılar için Uyarılar -uyarılar](alerts-reference.md#alerts-containerhost) başvuru tablosunun üst düzey bölümlerine ev sahipliği yapan) bakın.

## <a name="scanning-your-arm-based-container-registries-for-vulnerabilities"></a>ARM tabanlı konteyner kayıt defterlerinizi güvenlik açıkları için tarama 

1. Azure Konteyner Kayıt Defteri resimlerinizin güvenlik açığı taramalarını etkinleştirmek için:

    1. Azure Güvenlik Merkezi'nin standart fiyatlandırma katmanında olduğunuzdan emin olun.

    1. Fiyatlandırma **& ayarları** sayfasından aboneliğiniz için isteğe bağlı ![Konteyner Kayıt Defterleri paketini etkinleştirin: Konteyner Kayıt Defterleri paketini etkinleştirme](media/monitor-container-security/enabling-container-registries-bundle.png)

        Güvenlik Merkezi artık kayıt defterine itilen görüntüleri talmaya hazır. 

        >[!NOTE]
        >Bu özellik görüntü başına yüklenir.


1. Görüntünün teşbini tetiklemek için, resmin kayıt defterine itin. 

    Taramalar tamamlandığında (genellikle yaklaşık 10 dakika sonra), bulgular Güvenlik Merkezi önerilerinde kullanılabilir.
    

1. Bulguları görüntülemek için **Öneriler** sayfasına gidin. Sorunlar bulunduysa, aşağıdaki öneriyi görürsünüz:

    ![Sorunları düzeltmek için öneri ](media/monitor-container-security/acr-finding.png)


1. Öneriyi seçin. 
    Tavsiye ayrıntıları sayfası ek bilgilerle açılır. Bu bilgiler, güvenlik açığı olan görüntülerin ("Etkilenen kaynaklar") ve düzeltme adımlarını içeren kayıt defterlerinin listesini içerir. 

1. İçinde hassas depoları olan depoları görmek için belirli bir kayıt defteri seçin.

    ![Kayıt defteri seçin](media/monitor-container-security/acr-finding-select-registry.png)

    Kayıt defteri ayrıntıları sayfası, etkilenen depoların listesiyle açılır.

1. İçinde savunmasız görüntüler bulunan depoları görmek için belirli bir depo seçin.

    ![Depo seçme](media/monitor-container-security/acr-finding-select-repository.png)

    Depo ayrıntıları sayfası açılır. Bu bulguların şiddeti bir değerlendirme ile birlikte savunmasız görüntüleri listeler.

1. Güvenlik açıklarını görmek için belirli bir resim seçin.

    ![Görüntüleri seçin](media/monitor-container-security/acr-finding-select-image.png)

    Seçili görüntüiçin bulgular listesi açılır.

    ![Bulgular listesi](media/monitor-container-security/acr-findings.png)

1. Bir bulgu hakkında daha fazla bilgi edinmek için bulguyu seçin. 

    Bulgular ayrıntıları bölmesi açılır.

    [![Bulgular ayrıntıları bölmesi](media/monitor-container-security/acr-finding-details-pane.png)](media/monitor-container-security/acr-finding-details-pane.png#lightbox)

    Bu bölme, sorunun ayrıntılı bir açıklamasını ve tehditleri azaltmaya yardımcı olmak için dış kaynaklara bağlantılar içerir.

1. Bu bölmenin düzeltme bölümündeki adımları izleyin.

1. Güvenlik sorununu düzeltmek için gereken adımları attığınızda, kayıt defterinizdeki resmi değiştirin:

    1. Güncelleştirilmiş görüntüyü itin. Bu bir tazyik tetikler. 
    
    1. "Azure Konteyner Kayıt Defteri resimlerindeki güvenlik açıkları düzeltilmeli" önerisi için öneriler sayfasını kontrol edin. 
    
        Öneri hala görünüyorsa ve işlediğiniz resim hala savunmasız görüntüler listesinde görünüyorsa, düzeltme adımlarını yeniden denetleyin.

    1. Güncelleştirilmiş görüntünün itildiğinden, tarandığından ve artık öneride görünmediğinden emin olduğunuzda, "eski" savunmasız görüntüyü kayıt defterinizden silin.


## <a name="hardening-your-containers-docker-hosts"></a>Konteynerlerinizin Docker ev sahiplerini sertleştirme

Güvenlik Merkezi, Docker ana bilgisayarlarınızın yapılandırmasını sürekli olarak izler ve endüstri standartlarını yansıtan güvenlik önerileri oluşturur.

Azure Güvenlik Merkezi'nin kapsayıcılarınızın Docker ana bilgisayarları için güvenlik önerilerini görüntülemek için:

1. Güvenlik Merkezi gezinti çubuğundan, **Uygulamaları & İşlem'i** açın ve **Kapsayıcılar** sekmesini seçin.

1. İsteğe bağlı olarak, kapsayıcı kaynaklarınızın listesini kapsayıcı ana bilgisayarlarına süzün.

    ![Kapsayıcı kaynakları filtresi](media/monitor-container-security/container-resources-filter.png)

1. Konteyner ana bilgisayar makineleriniz listesinden, daha fazla araştırmak için birini seçin.

    ![Konteyner ana bilgisayar önerileri](media/monitor-container-security/container-resources-filtered-to-hosts.png)

    **Kapsayıcı ana bilgisayar bilgileri sayfası,** ana bilgisayar ayrıntıları ve öneriler listesiyle açılır.

1. Öneriler listesinden, daha fazla araştırmak için bir öneri seçin.

    ![Konteyner ana bilgisayar öneri listesi](media/monitor-container-security/container-host-rec.png)

1. İsteğe bağlı olarak, açıklama, bilgi, tehdit ve düzeltme adımlarını okuyun. 

1. Sayfanın altındaki **Eyleme Al'ı** seçin.

    [![Eylem düğmesini ele al](media/monitor-container-security/host-security-take-action-button.png)](media/monitor-container-security/host-security-take-action.png#lightbox)

    Log Analytics, çalışmaya hazır özel bir işlemle açılır. Varsayılan özel sorgu, sorunları çözmenize yardımcı olacak yönergelerle birlikte değerlendirilen tüm başarısız kuralların bir listesini içerir.

    [![Log Analytics eylemi](media/monitor-container-security/log-analytics-for-action-small.png)](media/monitor-container-security/log-analytics-for-action.png#lightbox)

1. Sorgu parametrelerini değiştirin ve ana bilgisayarınız için hazır olduğundan emin olduğunuzda **Çalıştır'ı** seçin. 



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Güvenlik Merkezi'nin kapsayıcı güvenlik özelliklerini nasıl kullanacağınızı öğrendiniz. 

Diğer ilgili materyaller için aşağıdaki sayfalara bakın: 

- [Konteynerler için Güvenlik Merkezi önerileri](recommendations-reference.md#recs-containers)
- [AKS küme düzeyi için uyarılar](alerts-reference.md#alerts-akscluster)
- [Kapsayıcı ana bilgisayar düzeyi için uyarılar](alerts-reference.md#alerts-containerhost)
