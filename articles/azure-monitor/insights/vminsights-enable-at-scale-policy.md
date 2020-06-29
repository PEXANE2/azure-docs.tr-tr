---
title: Azure Ilkesini kullanarak VM'ler için Azure İzleyici etkinleştirme
description: Bu makalede, Azure Ilkesi kullanarak birden çok Azure sanal makinesi veya sanal makine ölçek kümesi için VM'ler için Azure İzleyici nasıl etkinleştirileceği açıklanır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/25/2020
ms.openlocfilehash: 7d3c4e0f4bd34f996bb39426af39a692a6f79c5c
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85507186"
---
# <a name="enable-azure-monitor-for-vms-by-using-azure-policy"></a>Azure Ilkesini kullanarak VM'ler için Azure İzleyici etkinleştirme

Bu makalede Azure Ilkesi kullanılarak Azure sanal makineleri, Azure sanal makine ölçek kümeleri ve Azure Arc makinelerinde VM'ler için Azure İzleyici nasıl etkinleştirileceği açıklanır. Bu işlemin sonunda, Log Analytics ve bağımlılık aracılarını ve uyumlu olmayan tanımlanmış sanal makineleri etkinleştirmeyi başarıyla yapılandırmış olursunuz.

Tüm Azure sanal makinelerinizin veya sanal makine ölçek kümelerinin VM'ler için Azure İzleyici bulmasını, yönetmesini ve etkinleştirmesini sağlamak için Azure Ilkesi veya Azure PowerShell kullanabilirsiniz. Azure Ilkesi, yeni sağlanan VM 'lerin tutarlı uyumluluk ve otomatik olarak etkinleştirilmesini sağlamak üzere aboneliklerinizi etkili bir şekilde yönetmek üzere ilke tanımlarını yönetebilmeniz için önerdiğimiz yöntemdir. Bu ilke tanımları:

* Log Analytics aracısını ve bağımlılık aracısını dağıtın.
* Uyumluluk sonuçlarını raporla.
* Uyumsuz VM 'Ler için düzeltin.

Bu görevleri Azure PowerShell veya bir Azure Resource Manager şablonuyla gerçekleştirmeye ilgileniyorsanız, bkz. [Azure PowerShell veya Azure Resource Manager şablonlarını kullanarak VM'ler için Azure izleyici etkinleştirme](vminsights-enable-at-scale-powershell.md).

## <a name="prerequisites"></a>Ön koşullar
Azure VM 'lerinizi ve sanal makine ölçek kümelerinizi VM 'Ler için Azure Izlemeye eklemek üzere Ilke kullanmadan önce, izleme verilerinizi depolamak için kullanacağınız çalışma alanında Vminsıghts çözümünü etkinleştirmeniz gerekir. Bu görev, **diğer ekleme seçenekleri** sekmesindeki Azure Izleyici 'deki **Başlarken** sayfasından tamamlanabilir.  **Bir çalışma alanı Yapılandır**' ı seçin, bu işlem yapılandırılacak çalışma alanını seçmenizi ister.

![Çalışma alanını yapılandırma](media/vminsights-enable-at-scale-policy/configure-workspace.png)

Ayrıca, **ilkeyi kullanarak etkinleştir** ' i seçerek çalışma alanınızı yapılandırabilir ve ardından **çalışma alanını Yapılandır** araç çubuğunu seçebilirsiniz.  Bu işlem, çalışma alanının, Ilke kullanarak etkinleştirdiğiniz VM 'Ler ve sanal makine ölçek kümeleri tarafından gönderilen izleme verilerinin depolanmasını sağlayan seçili çalışma alanına Vminsıghts çözümünü yükler. 

![İlkeyi kullanarak etkinleştir](media/vminsights-enable-at-scale-policy/enable-using-policy.png)

## <a name="manage-policy-coverage-feature-overview"></a>Ilke kapsamını yönetme özelliğine genel bakış

VM'ler için Azure İzleyici Ilke kapsamı, daha önce bahsedilen ilke tanımlarını içeren **etkinleştirme VM'ler için Azure izleyici** girişim ölçeğini ölçeklendirmeyi, yönetmeyi ve etkinleştirmeyi basitleştirir. Bu özelliğe erişmek için VM'ler için Azure İzleyici **Başlarken** sekmesinden **diğer ekleme seçeneklerini** belirleyin. **VM'ler için Azure izleyici ilkesi** kapsamı sayfasını açmak Için **ilke kapsamını Yönet** ' i seçin.

![VM 'lerden Azure Izleyici Başlarken sekmesi](./media/vminsights-enable-at-scale-policy/get-started-page.png)

Buradan, yönetim gruplarınız ve abonelikleriniz genelinde girişim için kapsamı denetleyebilir ve yönetebilirsiniz. Yönetim gruplarının ve aboneliklerinin her birinde kaç tane VM olduğunu ve bunların uyumluluk durumunu anlayabilirsiniz.

![VM'ler için Azure İzleyici Ilke sayfasını yönet](media/vminsights-enable-at-scale-policy/manage-policy-page-01.png)

Bu bilgiler, bir merkezi konumdan VM'ler için Azure İzleyici için idare senaryonuzu planlayıp yürütgetirmenize yardımcı olmak için yararlıdır. Azure Ilkesi, bir bir ilke veya girişim bir kapsama atandığında, bu yeni sayfayla birlikte ilke veya girişim atanmamış ve yerinde atayabileceğiniz bir uyumluluk görünümü sağlar. Atama, görüntüleme ve düzenleme gibi tüm eylemler doğrudan Azure Ilkesine yeniden yönlendirme. **VM'ler için Azure izleyici Ilkesi kapsamı** sayfası, yalnızca girişim **etkinleştirme VM'ler için Azure izleyici**için genişletilmiş ve tümleşik bir deneyimdir.

Bu sayfadan Ayrıca, Log Analytics çalışma alanınızı, *Vminsıghts* çözümünü yükleyecek VM'ler için Azure izleyici için yapılandırabilirsiniz.

![VM'ler için Azure İzleyici çalışma alanını Yapılandır](media/vminsights-enable-at-scale-policy/manage-policy-page-02.png)

Bu seçenek, herhangi bir ilke eylemleriyle ilgili değildir. VM'ler için Azure İzleyici etkinleştirmek için gereken [önkoşulları](vminsights-enable-overview.md) yerine getirmek için kolay bir yol sağlamak için kullanılabilir.  

### <a name="what-information-is-available-on-this-page"></a>Bu sayfada hangi bilgiler kullanılabilir?

Aşağıdaki tablo, ilke kapsamı sayfasında sunulan bilgilerin dökümünü ve nasıl yorumlanacağını sağlar.

| İşlev | Açıklama | 
|----------|-------------| 
| **Kapsam** | Yönetim grubu hiyerarşisinde ayrıntıya gidebilme olanağı sunan veya bu erişimi devralınmış olan yönetim grubu ve abonelikler.|
| **Role** | Kapsam, okuyucu, sahip veya katkıda bulunan olabilir. Bazı durumlarda, abonelik erişimi olduğunu ancak ait olduğu yönetim grubuna değil, aboneliğe erişiminizin olabileceğini göstermek için boş görünebilir. Diğer sütunlardaki bilgiler, rolünüze bağlı olarak değişir. Rol, hangi verileri görebileceğinize ve ilke veya girişim atama, bunları düzenlemeyle veya uyumluluğu görüntüleme açısından gerçekleştirebileceğiniz eylemler konusunda bir anahtardır. |
| **Toplam VM sayısı** | Bu kapsamdaki sanal makine sayısı. Bir yönetim grubu için, abonelikler veya alt yönetim grubu altında iç içe geçmiş sanal makinelerin toplamıdır. |
| **Atama kapsamı** | İlke veya girişim kapsamındaki VM 'lerin yüzdesi. |
| **Atama durumu** | İlke veya girişim atamalarınızın durumu hakkında bilgi. |
| **Uyumlu VM 'Ler** | İlke veya girişim kapsamında uyumlu olan sanal makinelerin sayısı. Girişim **etkinleştirme VM'ler için Azure izleyici**için, bu hem Log Analytics Aracısı hem de bağımlılık aracısına sahip olan sanal makinelerin sayısıdır. Bazı durumlarda, hiçbir atama, VM yok veya yeterli izin olmadığından boş görünebilir. Bilgi, **uyumluluk durumu**altında sunulmaktadır. |
| **Uyumluluk** | Genel uyumluluk numarası, tüm ayrı kaynakların toplamına ayrılan ayrı kaynakların toplamıdır. |
| **Uyumluluk Durumu** | İlkenize veya girişim atamalarınızın uyumluluk durumuyla ilgili bilgiler.|

İlkeyi veya girişimi atadığınızda, atamada seçilen kapsam listelenen kapsam veya bunun bir alt kümesi olabilir. Örneğin, bir yönetim grubu (kapsam kapsamı) değil, abonelik (ilke kapsamı) için bir atama oluşturmuş olabilirsiniz. Bu durumda, **atama kapsamının** değeri, ilke veya girişim kapsamındaki VM 'leri, kapsam kapsamındaki VM 'ler tarafından ayrılmış olarak gösterir. Başka bir durumda, bazı VM 'Leri, kaynak gruplarını veya ilke kapsamından bir aboneliği dışınınız olabilir. Değer boşsa, ilke veya girişim yok ya da izniniz yok anlamına gelir. Bilgi, **atama durumu**altında sağlanır.

## <a name="enable-by-using-azure-policy"></a>Azure Ilkesi 'ni kullanarak etkinleştirme

VM'ler için Azure İzleyici, kiracınızda Azure Ilkesini kullanarak etkinleştirmek için:

- Girişimi bir kapsama atayın: yönetim grubu, abonelik veya kaynak grubu.
- Uyumluluk sonuçlarını gözden geçirin ve düzeltin.

Azure Ilkesi atama hakkında daha fazla bilgi için bkz. [Azure ilkesine genel bakış](../../governance/policy/overview.md#assignments) ve devam etmeden önce [Yönetim gruplarına genel bakış](../../governance/management-groups/overview.md) konusunu inceleyin.

### <a name="policies-for-azure-vms"></a>Azure VM 'Ler için ilkeler

Bir Azure VM 'nin ilke tanımları aşağıdaki tabloda listelenmiştir.

|Adı |Açıklama |Tür |
|-----|------------|-----|
|VM'ler için Azure İzleyici etkinleştir |Belirtilen kapsamdaki (Yönetim grubu, abonelik veya kaynak grubu) sanal makineler için Azure Izleyicisini etkinleştirin. Log Analytics çalışma alanını parametre olarak alır. |Girişim |
|Denetim bağımlılık Aracısı dağıtımı – VM görüntüsü (OS) listelenmemiş |VM görüntüsü (OS) listede tanımlanmamışsa ve aracı yüklü değilse, VM 'Leri uyumsuz olarak raporlar. |İlke |
|Denetim Log Analytics aracı dağıtımı – VM görüntüsü (OS) listelenmemiş |VM görüntüsü (OS) listede tanımlanmamışsa ve aracı yüklü değilse, VM 'Leri uyumsuz olarak raporlar. |İlke |
|Linux VM 'Ler için bağımlılık aracısını dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Linux VM 'Ler için bağımlılık aracısını dağıtın. |İlke |
|Windows VM 'Leri için bağımlılık aracısını dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Windows VM 'Ler için bağımlılık Aracısı 'nı dağıtın. |İlke |
|Linux sanal makineleri için Log Analytics aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Linux VM 'Leri için Log Analytics Aracısı dağıtın. |İlke |
|Windows VM 'Leri için Log Analytics aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Windows VM 'Leri için Log Analytics Aracısı dağıtın. |İlke |


### <a name="policies-for-hybrid-azure-arc-machines"></a>Karma Azure yay makineleri için ilkeler

Karma Azure Arc makinelerinde ilke tanımları aşağıdaki tabloda listelenmiştir.

|Adı |Açıklama |Tür |
|-----|------------|-----|
| [Önizleme]: Log Analytics Aracı, Linux Azure yay makinelerinizde yüklü olmalıdır |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, karma Azure yay makinelerini Linux VM 'Ler için uyumsuz olarak raporlar. |İlke |
| [Önizleme]: aracı Log Analytics Windows Azure Arc makinelerinizde yüklü olmalıdır |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, karma Azure yay makinelerini Windows VM 'Ler için uyumsuz olarak raporlar. |İlke |
| [Önizleme]: karma Linux Azure yay makinelerine bağımlılık aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Linux karma Azure yay makineleri için bağımlılık aracısını dağıtın. |İlke |
| [Önizleme]: karma Windows Azure yay makinelerine bağımlılık aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Windows karma Azure yay makineleri için bağımlılık Aracısı 'nı dağıtın. |İlke |
| [Önizleme]: Linux Azure yay makinelerine Log Analytics aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Linux karma Azure yay makineleri için Log Analytics Aracısı dağıtın. |İlke |
| [Önizleme]: Windows Azure yay makinelerine Log Analytics aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Windows karma Azure yay makineleri için Log Analytics Aracısı dağıtın. |İlke |


### <a name="policies-for-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümeleri için ilkeler

Bir Azure sanal makine ölçek kümesinin ilke tanımları aşağıdaki tabloda listelenmiştir.

|Adı |Açıklama |Tür |
|-----|------------|-----|
|Sanal Makine Ölçek Kümeleri için Azure Izleyicisini etkinleştir |Belirtilen kapsamdaki (Yönetim grubu, abonelik veya kaynak grubu) sanal makine ölçek kümeleri için Azure Izleyicisini etkinleştirin. Log Analytics çalışma alanını parametre olarak alır. Note: ölçek ayarlama yükseltme ilkeniz el Ile olarak ayarlandıysa, uzantıyı üzerinde yükseltme çağırarak küme içindeki tüm VM 'lere uygulayın. CLı 'de, bu `az vmss update-instances` . |Girişim |
|Sanal makine ölçek kümelerinde denetim bağımlılığı Aracısı dağıtımı-VM görüntüsü (OS) listelenmemiş |VM görüntüsü (OS) listede tanımlanmamışsa ve aracı yüklü değilse, sanal makine ölçek kümesini uyumsuz olarak bildirir. |İlke |
|Sanal makine ölçek kümelerinde denetim Log Analytics Aracısı dağıtımı-VM görüntüsü (OS) listelenmemiş |VM görüntüsü (OS) listede tanımlanmamışsa ve aracı yüklü değilse, sanal makine ölçek kümesini uyumsuz olarak bildirir. |İlke |
|Linux sanal makine ölçek kümeleri için bağımlılık aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Linux sanal makine ölçek kümeleri için bağımlılık aracısını dağıtın. |İlke |
|Windows sanal makine ölçek kümeleri için bağımlılık aracısını dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Windows sanal makine ölçek kümeleri için bağımlılık aracısını dağıtın. |İlke |
|Linux sanal makine ölçek kümeleri için Log Analytics aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Linux sanal makine ölçek kümeleri için Log Analytics Aracısı dağıtın. |İlke |
|Windows sanal makine ölçek kümeleri için Log Analytics aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Windows sanal makine ölçek kümeleri için Log Analytics Aracısı dağıtın. |İlke |

Tek başına ilke (girişim ile birlikte verilmez) burada açıklanmıştır:

|Adı |Açıklama |Tür |
|-----|------------|-----|
|VM için Log Analytics çalışma alanını denetle – rapor uyumsuzluğu |İlke veya girişim atamasında belirtilen Log Analytics çalışma alanına oturum açtıklarında VM 'Leri uyumsuz olarak raporla. |İlke |

### <a name="assign-the-azure-monitor-initiative"></a>Azure Izleyici girişim atama

**VM'ler için Azure izleyici Ilkesi kapsamı** sayfasından ilke atamasını oluşturmak için aşağıdaki adımları izleyin. Bu adımların nasıl tamamlandığını anlamak için, bkz. [Azure Portal ilke ataması oluşturma](../../governance/policy/assign-policy-portal.md).

İlkeyi veya girişimi atadığınızda, atamadaki seçili kapsam burada listelenen kapsam veya bunun bir alt kümesi olabilir. Örneğin, yönetim grubu (kapsam kapsamı) değil, abonelik (ilke kapsamı) için bir atama oluşturmuş olabilirsiniz. Bu durumda, kapsam yüzdesi, ilke veya girişim kapsamındaki VM 'Leri kapsam kapsamındaki VM 'Ler tarafından ayrılmış olarak gösterir. Başka bir durumda, bazı VM 'Leri veya kaynak gruplarını veya ilke kapsamından bir aboneliği dışınınız olabilir. Boşsa, ilke veya girişim yok ya da izniniz yok anlamına gelir. Bilgi, **atama durumu**altında sağlanır.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Azure portal, **İzle**' yi seçin. 

3. **Öngörüler** bölümünde **sanal makineler** ' i seçin.
 
4. **Kullanmaya** başlayın sekmesini seçin. Sayfasında, **Ilke kapsamını Yönet**' i seçin.

5. Bir yönetim grubu veya tablodan bir abonelik seçin. Üç nokta (...) simgesini seçerek **kapsam** seçin. Örnekte, bir kapsam, ilke atamasını zorlama için bir sanal makine gruplandırması ile sınırlandırır.

6. **Azure ilke ataması** sayfasında, girişim **Etkinleştir VM'ler için Azure izleyici**önceden doldurulur. 
    **Atama adı** kutusu, girişim adı ile otomatik olarak doldurulur, ancak bunu değiştirebilirsiniz. İsteğe bağlı bir açıklama da ekleyebilirsiniz. **Atanan** kutusu, oturum açan kim temel alınarak otomatik olarak doldurulur. Bu değer isteğe bağlıdır.

7. Seçim Kapsamdan bir veya daha fazla kaynağı kaldırmak için **Dışlamalar**' i seçin.

8. Desteklenen bölgenin **Log Analytics çalışma alanı** açılır listesinde bir çalışma alanı seçin.

   > [!NOTE]
   > Çalışma alanı atama kapsamının ötesinde, ilke atamasının asıl KIMLIĞINE *Log Analytics katkıda bulunan* izinleri verin. Bunu yapmazsanız, erişim izni vermek gibi bir dağıtım hatası görebilirsiniz `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...` , [yönetilen kimliği el ile yapılandırmayı](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity)gözden geçirin.
   > 
   >  Atanmakta olan girişim, *Deployifnotexists* efektli bir ilke Içerdiğinden, **yönetilen kimlik** onay kutusu seçilidir.
    
9. **Kimlik konumunu Yönet** açılan listesinde uygun bölgeyi seçin.

10. **Ata**'yı seçin.

Atamayı oluşturduktan sonra, **VM'ler için Azure izleyici Ilke kapsamı** sayfası, değişiklikleri yansıtacak şekilde **atama kapsamını**, **atama durumunu**, **uyumlu VM 'leri**ve **uyumluluk durumunu** güncelleştirir. 

Aşağıdaki matris, girişim için olası her uyumluluk durumunu eşler.  

| Uyumluluk durumu | Açıklama | 
|------------------|-------------|
| **Uyumlu** | Kapsamdaki tüm sanal makinelerin kendisine dağıtılan Log Analytics ve bağımlılık aracıları vardır.|
| **Uyumlu değil** | Kapsamdaki tüm VM 'Lerin bunlara dağıtılan Log Analytics ve bağımlılık aracıları yoktur ve düzeltme gerekebilir.|
| **Başlatılmadı** | Yeni bir atama eklendi. |
| **İne** | Yönetim grubu için yeterli ayrıcalıklara sahip değilsiniz. <sup>1</sup> | 
| **Adet** | Hiçbir ilke atanmadı. | 

<sup>1</sup> yönetim grubuna erişiminiz yoksa, bir sahibe erişim sağlamasını isteyin. Ya da, uyumluluğu görüntüleyin ve atamaları alt yönetim grupları ya da abonelikler aracılığıyla yönetin. 

Aşağıdaki tablo girişim için olası her atama durumunu eşler.

| Atama durumu | Açıklama | 
|------------------|-------------|
| **Başarılı** | Kapsamdaki tüm sanal makinelerin kendisine dağıtılan Log Analytics ve bağımlılık aracıları vardır.|
| **Uyarı** | Abonelik bir yönetim grubu altında değil.|
| **Başlatılmadı** | Yeni bir atama eklendi. |
| **İne** | Yönetim grubu için yeterli ayrıcalıklara sahip değilsiniz. <sup>1</sup> | 
| **Adet** | VM yok veya bir ilke atanmamış. | 
| **Eylem** | Bir ilke atayın veya bir atamayı düzenleyin. | 

<sup>1</sup> yönetim grubuna erişiminiz yoksa, bir sahibe erişim sağlamasını isteyin. Ya da, uyumluluğu görüntüleyin ve atamaları alt yönetim grupları ya da abonelikler aracılığıyla yönetin.

## <a name="review-and-remediate-the-compliance-results"></a>Uyumluluk sonuçlarını gözden geçirin ve düzeltin

Aşağıdaki örnek bir Azure VM içindir, ancak aynı zamanda sanal makine ölçek kümeleri için de geçerlidir. Uyumluluk sonuçlarının nasıl inceleyeceğinizi öğrenmek için bkz. [uyumsuzluk sonuçlarını belirleme](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). **VM'ler için Azure izleyici Ilkesi kapsamı** sayfasında, tablodan bir yönetim grubu veya bir abonelik seçin. Üç nokta (...) simgesini seçerek **uyumluluğu görüntüle** ' yi seçin.   

![Azure VM 'Leri için ilke uyumluluğu](./media/vminsights-enable-at-scale-policy/policy-view-compliance.png)

Girişim ile birlikte gelen ilkelerin sonuçlarına bağlı olarak, VM 'Ler aşağıdaki senaryolarda uyumsuz olarak bildirilir:

* Log Analytics Aracısı veya bağımlılık Aracısı dağıtılmadı.  
    Bu senaryo, var olan VM 'Lere sahip bir kapsam için tipik bir noktadır. Bunu azaltmak için, uyumsuz bir ilkede [Düzeltme görevleri oluşturarak](../../governance/policy/how-to/remediate-resources.md) gerekli aracıları dağıtın.  
    - Linux VM 'Ler için bağımlılık aracısını dağıtma
    - Windows VM 'Leri için bağımlılık aracısını dağıtma
    - Linux sanal makineleri için Log Analytics aracısı dağıtma
    - Windows VM 'Leri için Log Analytics aracısı dağıtma

* VM görüntüsü (OS), ilke tanımında tanımlı değil.  
    Dağıtım ilkesinin ölçütleri yalnızca iyi bilinen Azure VM görüntülerinden dağıtılan VM 'Leri içerir. VM işletim sisteminin desteklenip desteklenmediğini görmek için belgelere bakın. Desteklenmiyorsa, dağıtım ilkesini çoğaltın ve görüntüyü uyumlu hale getirmek için güncelleştirin veya değiştirin.  
    - Denetim bağımlılık Aracısı dağıtımı – VM görüntüsü (OS) listelenmemiş
    - Denetim Log Analytics aracı dağıtımı – VM görüntüsü (OS) listelenmemiş

* VM 'Ler belirtilen Log Analytics çalışma alanında oturum değil.  
    Girişim kapsamındaki bazı VM 'Ler, ilke atamasında belirtilenden farklı bir Log Analytics çalışma alanında oturum açmak mümkündür. Bu ilke, hangi VM 'Lerin uyumsuz bir çalışma alanına rapor olduğunu belirleyen bir araçtır.  
    - VM için Log Analytics çalışma alanını denetle – rapor uyumsuzluğu

## <a name="edit-an-initiative-assignment"></a>Girişim atamasını düzenleme

Bir yönetim grubuna veya aboneliğine bir girişim atadıktan sonra, aşağıdaki özellikleri değiştirmek için onu düzenleyebilirsiniz:

- Atama adı
- Açıklama
- Atayan
- Log Analytics çalışma alanı
- Özel durumlar

## <a name="next-steps"></a>Sonraki adımlar

Artık sanal makineleriniz için izleme etkin olduğuna göre, bu bilgiler VM'ler için Azure İzleyici analiz için kullanılabilir. 

- Bulunan uygulama bağımlılıklarını görüntülemek için bkz. [VM'ler için Azure izleyici haritasını görüntüleme](vminsights-maps.md). 

- VM performanlarınızın performans sorunlarını ve genel kullanımını belirlemek için bkz. [Azure VM performansını görüntüleme](vminsights-performance.md). 
