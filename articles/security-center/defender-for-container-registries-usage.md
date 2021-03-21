---
title: Azure Defender 'ı kapsayıcı kayıt defterleri için kullanma
description: Linux ile barındırılan kayıt defterlerinde Linux görüntülerini taramak için Azure Defender 'ı kapsayıcı kayıt defterleri ile kullanma hakkında bilgi edinin
author: memildin
ms.author: memildin
ms.date: 10/21/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: ee4992e41e792b570d8937edfe31efb4c651d742
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102100738"
---
# <a name="use-azure-defender-for-container-registries-to-scan-your-images-for-vulnerabilities"></a>Güvenlik açıkları için görüntülerinizi taramak üzere kapsayıcı kayıt defterleri için Azure Defender kullanma

Bu sayfada, yerleşik güvenlik açığı tarayıcısının Azure Resource Manager tabanlı Azure Container Registry depolanan kapsayıcı görüntülerini taramak için nasıl kullanılacağı açıklanmaktadır.

**Kapsayıcı kayıt defterleri için Azure Defender** etkinleştirildiğinde, kayıt defterinize gönderdiğiniz her görüntü anında taranır. Ayrıca, son 30 gün içinde çekilen tüm görüntü de taranır. 

Tarayıcı güvenlik merkezi 'ne güvenlik açıkları bildirdiğinde Güvenlik Merkezi, bulguları ve ilgili bilgileri öneriler olarak sunar. Ayrıca, bulgular düzeltme adımları, ilgili cileri, CVSS puanları ve daha fazlası gibi ilgili bilgileri içerir. Bir veya daha fazla abonelik veya belirli bir kayıt defteri için tanımlanan güvenlik açıklarını görüntüleyebilirsiniz.


## <a name="identify-vulnerabilities-in-images-in-azure-container-registries"></a>Azure Container Registry 'deki görüntülerde güvenlik açıklarını tanımla 

Azure Resource Manager tabanlı Azure Container Registry depolanan görüntülerin güvenlik açığı taramasını etkinleştirmek için:

1. Aboneliğiniz için **Azure Defender 'ı kapsayıcı kayıt defterleri için** etkinleştirin. Güvenlik Merkezi artık kayıt defterlerinden görüntüleri taramaya hazırdır.

    >[!NOTE]
    > Bu özellik görüntü başına ücretlendirilir.

1. Görüntü taramaları her gönderim veya içeri aktarma üzerinde tetiklenir ve görüntü son 30 gün içinde çekiliyorsa. 

    Tarama tamamlandığında (genellikle yaklaşık 2 dakika sonra, ancak 15 dakikaya kadar), bulgular Güvenlik Merkezi önerileri olarak kullanılabilir.

1. [Aşağıda açıklandığı gibi bulguları görüntüleyin ve düzeltin](#view-and-remediate-findings).

## <a name="identify-vulnerabilities-in-images-in-other-container-registries"></a>Diğer kapsayıcı kayıt defterlerinde görüntülerde güvenlik açıklarını tanımla 

1. Docker Hub veya Microsoft Container Registry kayıt defterinize görüntü getirmek için ACR araçlarını kullanın.  İçeri aktarma işlemi tamamlandığında, içeri aktarılan görüntüler Azure Defender tarafından taranır. 

    Kapsayıcı [görüntülerini kapsayıcı kayıt defterine Içeri aktarma](../container-registry/container-registry-import-images.md) hakkında daha fazla bilgi edinin

    Tarama tamamlandığında (genellikle yaklaşık 2 dakika sonra, ancak 15 dakikaya kadar), bulgular Güvenlik Merkezi önerileri olarak kullanılabilir.

1. [Aşağıda açıklandığı gibi bulguları görüntüleyin ve düzeltin](#view-and-remediate-findings).


## <a name="view-and-remediate-findings"></a>Bulguları görüntüleyin ve düzeltin

1. Bulguları görüntülemek için **öneriler** sayfasına gidin. Sorun bulunursa **Azure Container Registry görüntülerinin düzeltilme önerisi hakkında güvenlik açıklarını** görürsünüz

    ![Sorunları düzeltme önerisi ](media/monitor-container-security/acr-finding.png)

1. Öneriyi seçin. 

    Öneri ayrıntıları sayfası ek bilgilerle açılır. Bu bilgiler, güvenlik açığı bulunan görüntüler ("etkilenen kaynaklar") ve düzeltme adımlarını içeren kayıt defterlerinin listesini içerir. 

1. Güvenlik açığı bulunan depolara sahip olan depolar içindeki depoları görmek için belirli bir kayıt defteri seçin.

    ![Kayıt defteri seçin](media/monitor-container-security/acr-finding-select-registry.png)

    Kayıt defteri ayrıntıları sayfası, etkilenen depoların listesiyle birlikte açılır.

1. Güvenlik açığı bulunan görüntülere sahip olan depolar içindeki depoları görmek için belirli bir depoyu seçin.

    ![Depo seçme](media/monitor-container-security/acr-finding-select-repository.png)

    Depo ayrıntıları sayfası açılır. Bu, güvenlik açığı bulunan görüntüleri bulgularda önem derecesi değerlendirmesi ile birlikte listeler.

1. Güvenlik açıklarını görmek için belirli bir görüntü seçin.

    ![Görüntü Seç](media/monitor-container-security/acr-finding-select-image.png)

    Seçili görüntü için bulguları listesi açılır.

    ![Bulguları listesi](media/monitor-container-security/acr-findings.png)

1. Bulma hakkında daha fazla bilgi edinmek için bulma ' yı seçin. 

    Bulguları ayrıntıları bölmesi açılır.

    [![Bulguları ayrıntıları bölmesi](media/monitor-container-security/acr-finding-details-pane.png)](media/monitor-container-security/acr-finding-details-pane.png#lightbox)

    Bu bölme, sorunun ayrıntılı bir açıklamasını ve tehditleri azaltmaya yardımcı olmak için dış kaynakların bağlantılarını içerir.

1. Bu bölmenin düzeltme bölümündeki adımları izleyin.

1. Güvenlik sorununu düzeltmek için gereken adımları tamamladığınızda, kayıt defterinizin görüntüsünü değiştirin:

    1. Güncelleştirilmiş görüntüyü gönderin. Bu, bir taramayı tetikler. 
    
    1. Öneri "Azure Container Registry görüntülerde güvenlik açıkları düzeltilmelidir" önerisi için öneriler sayfasını kontrol edin. 
    
        Öneri hala görünüyorsa ve yaptığınız görüntü, güvenlik açığı bulunan görüntüler listesinde görünmeye devam ediyorsa, düzeltme adımlarını yeniden kontrol edin.

    1. Güncelleştirilmiş görüntünün itildiği, tarandığı ve artık öneri içinde görünmediğinden emin olduğunuzda, "eski" savunmasız görüntüyü Kayıt defterinizden silin.


## <a name="disable-specific-findings-preview"></a>Belirli bulguları devre dışı bırak (Önizleme)

> [!NOTE]
> [!INCLUDE [Legalese](../../includes/security-center-preview-legal-text.md)]

Bir bulmayı yok saymanız gereken bir kuruluş varsa, bunu düzeltmek yerine isteğe bağlı olarak devre dışı bırakabilirsiniz. Devre dışı bulgular, güvenli puanınızı etkilemez veya istenmeyen gürültü oluşturmaz.

Bir bulma, devre dışı bırakma kurallarında tanımladığınız ölçütlerle eşleştiğinde, bu, bulguları listesinde görünmez. Tipik senaryolar şunlardır:

- Ortalamanın altında önem derecesine sahip bulguları devre dışı bırak
- Patchable olmayan bulguları devre dışı bırak
- CVSS puanı 6,5 ile bulguları devre dışı bırakın
- Güvenlik denetiminde veya kategoride belirli bir metinle bulguları devre dışı bırakın (örneğin, "RedHat", "CentOS güvenlik güncelleştirmesi")

> [!IMPORTANT]
> Bir kural oluşturmak için Azure Ilkesinde bir ilkeyi düzenleme izinlerinizin olması gerekir.
>
> Azure [Ilkesinde Azure RBAC izinleri](../governance/policy/overview.md#azure-rbac-permissions-in-azure-policy)hakkında daha fazla bilgi edinin.

Aşağıdaki ölçütlerden herhangi birini kullanabilirsiniz: 

- KIMLIK bulma 
- Kategori
- Güvenlik denetimi 
- CVSS v3 puanları
- Önem derecesi 
- Patchable durumu 

Bir kural oluşturmak için:

1. **Azure Container Registry görüntülerde güvenlik açıklarına** yönelik öneriler ayrıntısı sayfasında, **kuralı devre dışı bırak**' ı seçin.
1. İlgili kapsamı seçin.
1. Ölçütlerinizi tanımlayın.
1. **Kural Uygula**' yı seçin.

    :::image type="content" source="./media/defender-for-container-registries-usage/new-disable-rule-for-registry-finding.png" alt-text="Kayıt defterindeki VA bulguları için devre dışı bırakma kuralı oluşturma":::

1. Bir kuralı görüntülemek, geçersiz kılmak veya silmek için: 
    1. **Kuralı devre dışı bırak** seçeneğini belirleyin.
    1. Kapsam listesinden, etkin kuralların bulunduğu abonelikler **kural uygulandı** olarak gösterilir.
        :::image type="content" source="./media/remediate-vulnerability-findings-vm/modify-rule.png" alt-text="Mevcut bir kuralı değiştirme veya silme":::
    1. Kuralı görüntülemek veya silmek için üç nokta menüsünü ("...") seçin.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Defender hakkında daha fazla bilgi](azure-defender.md)