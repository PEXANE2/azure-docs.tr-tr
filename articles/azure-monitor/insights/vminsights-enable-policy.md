---
title: Azure Ilkesini kullanarak VM'ler için Azure İzleyici etkinleştirme
description: Azure Ilkesi kullanarak birden çok Azure sanal makinesi veya sanal makine ölçek kümesi için VM'ler için Azure İzleyici nasıl etkinleştirebileceğinizi açıklar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: 2c292ee601114a58e38b9e509efa53be2d3c93d6
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87328379"
---
# <a name="enable-azure-monitor-for-vms-by-using-azure-policy"></a>Azure Ilkesini kullanarak VM'ler için Azure İzleyici etkinleştirme
Bu makalede, Azure Policy kullanılarak Azure sanal makineler veya Azure Arc (Önizleme) ile bağlantılı karma sanal makine için VM'ler için Azure İzleyici nasıl etkinleştirileceği açıklanır. Azure Ilkesi, Azure ortamınızda VM'ler için Azure İzleyici için gerekli aracıları yükleyen ve her sanal makine oluşturulduğu sürece VM 'Ler için izlemeyi otomatik olarak etkinleştiren ilke tanımlarını atamanıza olanak tanır. VM'ler için Azure İzleyici, ortamınızdaki uyumsuz VM 'Leri keşfetmenizi ve düzeltmenizi sağlayan bir özellik sunar. Doğrudan Azure Ilkesiyle çalışmak yerine bu özelliği kullanın.

Azure Ilkesi hakkında bilgi sahibi değilseniz Azure [ilkesi 'ni kullanarak Azure izleyici 'yi ölçeğe göre dağıtma](../platform/deploy-scale.md)konusuna kısa bir giriş alın.

> [!NOTE]
> Azure Ilkesini Azure sanal makine ölçek kümeleri ile kullanmak veya Azure sanal makinelerini etkinleştirmek üzere doğrudan Azure Ilkesiyle çalışmak için bkz. Azure [ilkesini kullanarak Azure izleyici 'yi ölçekli olarak dağıtma](../platform/deploy-scale.md#azure-monitor-for-vms-and-virtual-machine-agents).

## <a name="prerequisites"></a>Önkoşullar
- [Log Analytics çalışma alanı oluşturun ve yapılandırın](vminsights-configure-workspace.md).
- Etkinleştirmiş olduğunuz sanal makine veya sanal makine ölçek kümesinin işletim sisteminin desteklendiğinden emin olmak için [desteklenen işletim sistemleri](vminsights-enable-overview.md#supported-operating-systems) bölümüne bakın. 


## <a name="azure-monitor-for-vms-initiative"></a>VM'ler için Azure İzleyici girişim
VM'ler için Azure İzleyici, Azure sanal makinelerine Log Analytics aracısını ve bağımlılık aracısını yüklemek için yerleşik ilke tanımları sağlar. Girişim **etkinleştir VM'ler için Azure izleyici** , bu ilke tanımlarının her birini içerir. Aracıları bu kapsamdaki herhangi bir Windows veya Linux Azure sanal makinesine otomatik olarak yüklemek için bu girişimi bir yönetim grubuna, aboneliğe veya kaynak grubuna atayın.

## <a name="open-policy-coverage-feature"></a>Ilke kapsamı özelliğini aç
**VM'ler için Azure izleyici Ilke kapsamına**erişmek için, Azure Portal **Azure izleyici** menüsündeki **sanal makinelere** gidin. **Diğer ekleme seçeneklerini** belirleyin ve **ilkeyi kullanarak etkinleştir**' in altında **etkinleştirin** .

[![VM 'lerden Azure Izleyici Başlarken sekmesi](./media/vminsights-enable-at-scale-policy/get-started-page.png)](./media/vminsights-enable-at-scale-policy/get-started-page.png#lightbox)

## <a name="create-new-assignment"></a>Yeni atama oluştur
Henüz bir atamanız yoksa, **Ilke ata**' ya tıklayarak yeni bir tane oluşturun.

[![Atama oluştur](media/vminsights-enable-at-scale-policy/create-assignment.png)](media/vminsights-enable-at-scale-policy/create-assignment.png#lightbox)

Bu, seçtiğiniz kapsamla ve **Enable VM'ler için Azure izleyici** girişim tanımıyla birlikte kodlanması dışında, Azure ilkesinde bir girişim atamak için aynı sayfasıdır. İsteğe bağlı olarak **atama adını** değiştirebilir ve bir **Açıklama**ekleyebilirsiniz. Kapsama bir dışlama sağlamak istiyorsanız **Dışlamalar** ' ı seçin. Örneğin, kapsamınız bir yönetim grubu olabilir ve bu yönetim grubunda atamadan dışlanacak bir abonelik belirtebilirsiniz.

[![Girişim ata](media/vminsights-enable-at-scale-policy/assign-initiative.png)](media/vminsights-enable-at-scale-policy/assign-initiative.png#lightbox)

**Parametreler** sayfasında, atamadaki tüm sanal makineler tarafından kullanılacak bir **Log Analytics çalışma alanı** seçin. Farklı sanal makineler için farklı çalışma alanları belirtmek istiyorsanız, her biri kendi kapsamına sahip birden çok atama oluşturmanız gerekir. 

   > [!NOTE]
   > Çalışma alanı atama kapsamının ötesinde, ilke atamasının asıl KIMLIĞINE *Log Analytics katkıda bulunan* izinleri verin. Bunu yapmazsanız, şunun gibi bir dağıtım hatası görebilirsiniz:`The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...`

[![Çalışma alanı](media/vminsights-enable-at-scale-policy/assignment-workspace.png)](media/vminsights-enable-at-scale-policy/assignment-workspace.png#lightbox)

Oluşturmak için **Oluştur** ' a tıklamadan önce atama ayrıntılarını gözden geçirmek Için **gözden geçir + oluştur** ' a tıklayın. Mevcut sanal makineleri etkinleştirmek için büyük olasılıkla birden çok düzeltme görevi olması gerekeceğinden, bu noktada bir düzeltme görevi oluşturmayın. Bkz. [uyumluluk sonuçlarını](#remediate-compliance-results) aşağıdaki şekilde düzeltin.

## <a name="review-compliance"></a>Uyumluluğu gözden geçir
Bir atama oluşturulduktan sonra yönetim gruplarınız ve aboneliklerinizde **VM'ler için Azure izleyici girişim etkinleştirme** kapsamını gözden geçirebilir ve yönetebilirsiniz. Bu, yönetim gruplarının veya aboneliklerinin her birinde kaç tane sanal makinenin mevcut olduğunu ve bunların uyumluluk durumlarını gösterir.

[![VM'ler için Azure İzleyici Ilke sayfasını yönet](media/vminsights-enable-at-scale-policy/manage-policy-page-01.png)](media/vminsights-enable-at-scale-policy/manage-policy-page-01.png#lightbox)


Aşağıdaki tabloda bu görünümdeki bilgilerin açıklaması verilmiştir.

| İşlev | Açıklama | 
|----------|-------------| 
| **Kapsam** | Yönetim grubu hiyerarşisinde ayrıntıya gidebilme olanağı sunan veya bu erişimi devralınmış olan yönetim grubu ve abonelikler.|
| **Role** | Kapsam içinde, bu, okuyucu, sahip veya katkıda bulunan olabilecek roldür. Abonelik erişiminiz varsa ancak ait olduğu yönetim grubuna erişemiyorsanız bu boş olur. Bu rol, hangi verileri görebileceğinizi ve ilke veya girişim (Owner) atama, bunları düzenlemeyle veya uyumluluğu görüntüleme açısından gerçekleştirebileceğiniz eylemleri belirler. |
| **Toplam VM sayısı** | Bu kapsamdaki, durumlarına bakılmaksızın toplam sanal makine sayısı. Bir yönetim grubu için, bu, abonelikler veya alt yönetim grupları altında iç içe geçmiş VM 'lerin toplam toplamıdır. |
| **Atama kapsamı** | Girişim kapsamındaki VM 'lerin yüzdesi. |
| **Atama durumu** | **Başarılı** -kapsamdaki tüm sanal makinelerin kendisine dağıtılan Log Analytics ve bağımlılık aracıları vardır.<br>**Uyarı** -abonelik bir yönetim grubu altında değil.<br>**Başlatılmamış** -yeni bir atama eklendi.<br>**Kilitle** -yönetim grubu için yeterli ayrıcalıklara sahip değilsiniz.<br>**Boş** -VM yok veya bir ilke atanmamış. |
| **Uyumlu VM 'Ler** | Log Analytics Aracısı ve bağımlılık aracısının yüklü olduğu sanal makine sayısı, uyumlu olan sanal makinelerin sayısıdır. Bu, herhangi bir atama olmadığında, kapsamda VM olmadığında veya uygun izinlere sahip olmadığında boş olur. |
| **Uyumluluk** | Genel uyumluluk numarası, tüm ayrı kaynakların toplamına ayrılan ayrı kaynakların toplamıdır. |
| **Uyumluluk durumu** | **Uyumlu** -kapsamdaki sanal makinelerdeki tüm sanal makineler, bunlara dağıtılan Log Analytics ve bağımlılık aracılarını veya kapsama tabi olan kapsamdaki yeni VM 'leri henüz değerlendirilmemiştir.<br>**Uyumlu** değil-değerlendirilen ancak etkinleştirilmeyen ve düzeltme gerektirebilecek VM 'ler vardır.<br>**Başlatılmamış** -yeni bir atama eklendi.<br>**Kilitle** -yönetim grubu için yeterli ayrıcalıklara sahip değilsiniz.<br>**Boş** -ilke atanmamış.  |


Girişimi atadığınızda, atamadaki seçili kapsam listelenen kapsam veya bunun bir alt kümesi olabilir. Örneğin, bir yönetim grubu (kapsam kapsamı) değil, abonelik (ilke kapsamı) için bir atama oluşturmuş olabilirsiniz. Bu durumda, **atama** kapsamının değeri, girişim kapsamındaki VM 'lerin kapsam kapsamındaki VM 'ler tarafından bölündüğü anlamına gelir. Başka bir durumda, bazı VM 'Leri, kaynak gruplarını veya ilke kapsamından bir aboneliği dışınınız olabilir. Değer boşsa, ilke veya girişim yok ya da izniniz yok anlamına gelir. Bilgi, **atama durumu**altında sağlanır.


## <a name="remediate-compliance-results"></a>Uyumluluk sonuçlarını düzelt
Girişim, oluşturulan veya değiştirilen sanal makinelere uygulanır, ancak mevcut VM 'lere uygulanmaz. Atamanız %100 uyumluluğu göstermiyorsa, mevcut VM 'Leri değerlendirmek ve etkinleştirmek için düzeltme görevleri oluşturun, üç noktayı (...) seçerek **uyumluluğu görüntüle** ' yi seçin.

[![Uyumluluğu görüntüle](media/vminsights-enable-at-scale-policy/view-compliance.png)](media/vminsights-enable-at-scale-policy/view-compliance.png#lightbox)

**Uyumluluk** sayfası, belirtilen filtreyle eşleşen atamaları ve uyumlu olup olmadığını listeler. Ayrıntılarını görüntülemek için bir atamaya tıklayın.

[![Azure VM 'Leri için ilke uyumluluğu](./media/vminsights-enable-at-scale-policy/policy-view-compliance.png)](./media/vminsights-enable-at-scale-policy/policy-view-compliance.png#lightbox)

**Girişim uyumluluğu** sayfası, girişim içindeki ilke tanımlarını listeler ve her birinin uyumlu olup olmadığını belirtir.

[![Uyumluluk ayrıntıları](media/vminsights-enable-at-scale-policy/compliance-details.png)](media/vminsights-enable-at-scale-policy/compliance-details.png#lightbox)

Ayrıntılarını görüntülemek için bir ilke tanımına tıklayın. İlke tanımlarının uyumsuz olarak gösterileceği senaryolar aşağıdakileri içerir:

* Log Analytics Aracısı veya bağımlılık Aracısı dağıtılmadı. Azaltmak için bir düzeltme görevi oluşturun.
* VM görüntüsü (OS), ilke tanımında tanımlı değil. Dağıtım ilkesinin ölçütleri yalnızca iyi bilinen Azure VM görüntülerinden dağıtılan VM 'Leri içerir. VM işletim sisteminin desteklenip desteklenmediğini görmek için belgelere bakın.
* VM 'Ler belirtilen Log Analytics çalışma alanına oturum değil. Girişim kapsamındaki bazı VM 'Ler, ilke atamasında belirtilenden farklı bir Log Analytics çalışma alanına bağlanır.

[![İlke uyumluluğu ayrıntıları](media/vminsights-enable-at-scale-policy/policy-compliance-details.png)](media/vminsights-enable-at-scale-policy/policy-compliance-details.png#lightbox)

Uyumluluk sorunlarını azaltmak için bir düzeltme görevi oluşturmak üzere **Düzeltme görevi oluştur**' a tıklayın. 

[![Yeni düzeltme görevi](media/vminsights-enable-at-scale-policy/new-remediation-task.png)](media/vminsights-enable-at-scale-policy/new-remediation-task.png#lightbox)

Düzeltme görevini oluşturmak için **Düzelt** ' e tıklayın ve ardından başlatmak için **düzeltin** . Büyük olasılıkla her ilke tanımı için bir tane olmak üzere birden çok düzeltme görevi oluşturmanız gerekir. Bir girişim için düzeltme görevi oluşturamazsınız.

[![Düzeltmesi](media/vminsights-enable-at-scale-policy/remediation.png)](media/vminsights-enable-at-scale-policy/remediation.png#lightbox)


Düzeltme görevleri tamamlandıktan sonra, sanal makinelerinizin VM'ler için Azure İzleyici için yüklü ve etkinleştirilmiş aracıların uyumlu olması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

Artık sanal makineleriniz için izleme etkin olduğuna göre, bu bilgiler VM'ler için Azure İzleyici analiz için kullanılabilir. 

- Bulunan uygulama bağımlılıklarını görüntülemek için bkz. [VM'ler için Azure izleyici haritasını görüntüleme](vminsights-maps.md). 
- VM performanlarınızın performans sorunlarını ve genel kullanımını belirlemek için bkz. [Azure VM performansını görüntüleme](vminsights-performance.md). 
