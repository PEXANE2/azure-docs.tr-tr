---
title: Azure Güvenlik Merkezi 'nde kapsayıcılarınızın güvenliğini izleme
description: Azure Güvenlik Merkezi 'nden kapsayıcılarınızın güvenlik duruşunu nasıl denetleyeceğinizi öğrenin
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: memildin
ms.openlocfilehash: 330cbc3f28f5e549d5a21417c3d7ccc1e5444769
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77919541"
---
# <a name="monitoring-the-security-of-your-containers"></a>Kapsayıcılarınızın güvenliğini izleme

Bu sayfada, kavramlar bölümümüzde [kapsayıcı güvenlik makalesinde](container-security.md) açıklanan kapsayıcı güvenlik özelliklerinin nasıl kullanılacağı açıklanmaktadır.

Azure Güvenlik Merkezi, kapsayıcı güvenliğinin aşağıdaki üç yönlerini ele almaktadır:

- **Güvenlik açığı yönetimi** -Güvenlik Merkezi 'nin standart Fiyatlandırma Katmanı (bkz. [fiyatlandırma](/azure/security-center/security-center-pricing)) kullanıyorsanız, her yeni görüntü gönderildiğinde ARM tabanlı Azure Container Registry tarayabilirsiniz. Tarayıcı (Qualys tarafından desteklenir), bulguları Güvenlik Merkezi önerileri olarak sunar.
    Ayrıntılı yönergeler için bkz. aşağıdaki [güvenlik açıkları için kapsayıcı kayıt defterlerinden tarama](#scanning-your-arm-based-container-registries-for-vulnerabilities) .

- **Kapsayıcılarınızın Docker konaklarını sağlamlaştırma** -Güvenlik Merkezi, IaaS Linux sanal makinelerinde veya Docker çalıştıran diğer Linux makinelerde barındırılan yönetilmeyen kapsayıcıları bulur ve kapsayıcıların yapılandırmalarının Internet GÜVENLIĞI (CIS) Docker kıyaslaması için merkezi ile sürekli olarak karşılaştırılmasını ister. Güvenlik Merkezi, kapsayıcılarınız denetimlerden herhangi birini karşılamadığı takdirde sizi uyarır. Güvenlik risklerine yönelik sürekli izleme, yapılandırma hataları nedeniyle herhangi bir güvenlik programının önemli bir bileşenidir. 
    Ayrıntılı yönergeler için bkz. [kapsayıcılarınızın Docker konaklarını artırma](#hardening-your-containers-docker-hosts) .

- **Azure Kubernetes hizmet kümelerinizi sağlamlaştırma** -Güvenlik Merkezi, Azure Kubernetes hizmet kümelerinizin yapılandırmasında güvenlik açıkları bulduğunda öneriler sağlar. Görünebilen belirli önerilerin ayrıntıları için bkz. [Kubernetes hizmet önerileri](recommendations-reference.md#recs-containers).

- **Çalışma zamanı koruması** -Güvenlik Merkezi 'nin standart fiyatlandırma katmanında çalışıyorsanız, Kapsayıcılı ortamlarınız için gerçek zamanlı tehdit koruması alacaksınız. Güvenlik Merkezi, ana bilgisayar ve AKS kümesi düzeyinde şüpheli etkinlikler için uyarı oluşturur. Görünebilen ilgili güvenlik uyarılarının ayrıntıları için uyarılar başvuru tablosunun [Azure Kubernetes hizmet kümeleri](alerts-reference.md#alerts-akscluster) ve [kapsayıcılar için uyarılar-konak düzeyi](alerts-reference.md#alerts-containerhost) bölümlerine bakın.

## <a name="scanning-your-arm-based-container-registries-for-vulnerabilities"></a>Güvenlik açıkları için ARM tabanlı kapsayıcı kayıt defterlerinden tarama 

1. Azure Container Registry görüntülerinizin güvenlik açığı taramasını etkinleştirmek için:

    1. Azure Güvenlik Merkezi 'nin standart fiyatlandırma katmanında olduğunuzdan emin olun.

    1. **Fiyatlandırma & ayarları** sayfasında, aboneliğiniz için Isteğe bağlı kapsayıcı kayıt defterleri grubunu etkinleştirin: kapsayıcı kayıt defterleri paketini etkinleştirmek ![](media/monitor-container-security/enabling-container-registries-bundle.png)

        Güvenlik Merkezi artık kayıt defterine gönderilen görüntüleri taramaya hazırdır. 

        >[!NOTE]
        >Bu özellik görüntü başına ücretlendirilir.


1. Bir görüntünün taramasını tetiklemek için, Kayıt defterinize gönderin. 

    Tarama tamamlandığında (genellikle yaklaşık 10 dakika sonra), güvenlik merkezi önerilerinde bulguları kullanılabilir.
    

1. Bulguları görüntülemek için **öneriler** sayfasına gidin. Sorun bulunursa aşağıdaki öneriyi görürsünüz:

    ![Sorunları düzeltme önerisi ](media/monitor-container-security/acr-finding.png)


1. Öneriyi seçin. 
    Öneri ayrıntıları sayfası ek bilgilerle açılır. Bu bilgiler, güvenlik açığı bulunan görüntüler ("etkilenen kaynaklar") ve düzeltme adımlarını içeren kayıt defterlerinin listesini içerir. 

1. Güvenlik açığı bulunan depolara sahip olan depolar içindeki depoları görmek için belirli bir kayıt defteri seçin.

    ![Kayıt defteri seçin](media/monitor-container-security/acr-finding-select-registry.png)

    Kayıt defteri ayrıntıları sayfası, etkilenen depoların listesiyle birlikte açılır.

1. Güvenlik açığı bulunan görüntülere sahip olan depolar içindeki depoları görmek için belirli bir depoyu seçin.

    ![Bir depo seçin](media/monitor-container-security/acr-finding-select-repository.png)

    Depo ayrıntıları sayfası açılır. Bu, güvenlik açığı bulunan görüntüleri bulgularda önem derecesi değerlendirmesi ile birlikte listeler.

1. Güvenlik açıklarını görmek için belirli bir görüntü seçin.

    ![Görüntü Seç](media/monitor-container-security/acr-finding-select-image.png)

    Seçili görüntü için bulguları listesi açılır.

    ![Bulguları listesi](media/monitor-container-security/acr-findings.png)

1. Bulma hakkında daha fazla bilgi edinmek için bulma ' yı seçin. 

    Bulguları ayrıntıları bölmesi açılır.

    [![bulguları Ayrıntılar bölmesi](media/monitor-container-security/acr-finding-details-pane.png)](media/monitor-container-security/acr-finding-details-pane.png#lightbox)

    Bu bölme, sorunun ayrıntılı bir açıklamasını ve tehditleri azaltmaya yardımcı olmak için dış kaynakların bağlantılarını içerir.

1. Bu bölmenin düzeltme bölümündeki adımları izleyin.

1. Güvenlik sorununu düzeltmek için gereken adımları tamamladığınızda, kayıt defterinizin görüntüsünü değiştirin:

    1. Güncelleştirilmiş görüntüyü gönderin. Bu, bir taramayı tetikler. 
    
    1. Öneri "Azure Container Registry görüntülerde güvenlik açıkları düzeltilmelidir" önerisi için öneriler sayfasını kontrol edin. 
    
        Öneri hala görünüyorsa ve yaptığınız görüntü, güvenlik açığı bulunan görüntüler listesinde görünmeye devam ediyorsa, düzeltme adımlarını yeniden kontrol edin.

    1. Güncelleştirilmiş görüntünün itildiği, tarandığı ve artık öneri içinde görünmediğinden emin olduğunuzda, "eski" savunmasız görüntüyü Kayıt defterinizden silin.


## <a name="hardening-your-containers-docker-hosts"></a>Kapsayıcılarınızın Docker konaklarını sağlamlaştırma

Güvenlik Merkezi, Docker konaklarınızın yapılandırmasını sürekli izler ve sektör standartlarını yansıtan güvenlik önerileri oluşturur.

Azure Güvenlik Merkezi 'nin kapsayıcılarınızın Docker konaklarına yönelik güvenlik önerilerini görüntülemek için:

1. Güvenlik Merkezi gezinti çubuğundan, **işlem & uygulamalar** ' ı açın ve **kapsayıcılar** sekmesini seçin.

1. İsteğe bağlı olarak, kapsayıcı kaynaklarının listesini kapsayıcı Konakları konaklarına filtreleyin.

    ![Kapsayıcı kaynakları filtresi](media/monitor-container-security/container-resources-filter.png)

1. Kapsayıcı ana makinelerinizin listesinden daha fazla araştırmak için bir tane seçin.

    ![Kapsayıcı ana bilgisayar önerileri](media/monitor-container-security/container-resources-filtered-to-hosts.png)

    **Kapsayıcı ana bilgisayar bilgileri sayfası** , konağın ayrıntıları ve öneriler listesi ile açılır.

1. Öneriler listesinden daha fazla araştırma yapmak için bir öneri seçin.

    ![Kapsayıcı konak önerisi listesi](media/monitor-container-security/container-host-rec.png)

1. İsteğe bağlı olarak, açıklama, bilgi, tehditler ve düzeltme adımlarını okuyun. 

1. Sayfanın alt kısmındaki **eylem al** ' ı seçin.

    [![eylem Al düğmesi](media/monitor-container-security/host-security-take-action-button.png)](media/monitor-container-security/host-security-take-action.png#lightbox)

    Log Analytics, çalıştırmaya hazırlamış özel bir işlemle açılır. Varsayılan özel sorgu, sorunları çözmenize yardımcı olacak yönergeler ile birlikte, değerlendirilen tüm başarısız kuralların bir listesini içerir.

    [![Log Analytics eylemi](media/monitor-container-security/log-analytics-for-action-small.png)](media/monitor-container-security/log-analytics-for-action.png#lightbox)

1. Sorgu parametrelerini ince ayar ve konağa hazırlandığından emin olduğunuzda **Çalıştır** ' ı seçin. 



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Güvenlik Merkezi 'nin kapsayıcı güvenlik özelliklerini kullanmayı öğrendiniz. 

Diğer ilgili malzemeler için aşağıdaki sayfalara bakın: 

- [Kapsayıcılar için Güvenlik Merkezi önerileri](recommendations-reference.md#recs-containers)
- [AKS kümesi düzeyi için uyarılar](alerts-reference.md#alerts-akscluster)
- [Kapsayıcı ana bilgisayar düzeyi uyarıları](alerts-reference.md#alerts-containerhost)
