---
title: Azure Ilkesi 'ni kullanarak Azure Izleyici 'yi ölçekli olarak dağıtma
description: Azure Izleyici özelliklerini Azure Ilkesi kullanarak ölçekli olarak dağıtın.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/08/2020
ms.openlocfilehash: a69a58da85cf1ee03046626bb076c5cd44196279
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87828719"
---
# <a name="deploy-azure-monitor-at-scale-using-azure-policy"></a>Azure Ilkesi 'ni kullanarak Azure Izleyici 'yi ölçekli olarak dağıtma
Bazı Azure Izleyici özellikleri bir kez veya sınırlı sayıda yapılandırıldığında, izlemek istediğiniz her kaynak için diğerlerinin tekrarlanması gerekir. Bu makalede, izlemenin tüm Azure kaynaklarınız için tutarlı ve doğru şekilde yapılandırıldığından emin olmak üzere Azure Izleyici 'yi ölçekli olarak uygulamak için Azure Ilkesi kullanma yöntemleri açıklanmaktadır.

Örneğin, tüm mevcut Azure kaynaklarınız için ve oluşturduğunuz her yeni kaynak için bir tanılama ayarı oluşturmanız gerekir. Ayrıca, her bir sanal makine oluşturduğunuzda bir aracının yüklenmiş ve yapılandırılmış olması gerekir. Bu yöntemler Azure Izleyici 'nin tüm özellikleri için kullanılabilir olduğundan, bu eylemleri gerçekleştirmek için PowerShell veya CLı gibi yöntemleri kullanabilirsiniz. Azure Ilkesi 'ni kullanarak bir kaynağı her oluşturduğunuzda veya değiştirdiğinizde uygun yapılandırmayı otomatik olarak gerçekleştirecek mantığa sahip olabilirsiniz.


## <a name="azure-policy"></a>Azure İlkesi
Bu bölümde, en az çabayla tüm Azure aboneliğiniz veya yönetim grubunuz genelinde kurumsal standartları değerlendirmenize ve zorlamanıza olanak sağlayan [Azure ilkesine](../governance/policy/overview.md) kısa bir giriş sunulmaktadır. Tüm ayrıntılar için [Azure İlkesi belgelerine](../governance/policy/overview.md) bakın.

Azure Ilkesi ile oluşturulan tüm kaynaklar için yapılandırma gereksinimlerini belirtebilir ve uyumsuz olan kaynakları tanımlayabilir, kaynakların oluşturulmasını engelleyebilir veya gerekli yapılandırmayı ekleyebilirsiniz. Yeni bir kaynak oluşturmak veya var olan bir kaynağı değiştirmek için, kesintiye uğratan çağrılar yaparak işe yarar. Bir ilke tanımında beklenen özelliklerle eşleşmezse isteği reddetme, uyumsuzluk için bayrak atama veya ilgili bir kaynağı dağıtma gibi bu etkilerle yanıt verebilir. Mevcut kaynakları bir **Deployifnotexists** ile düzeltebilir veya ilke tanımı ' nı **değiştirebilirsiniz** .

Azure Ilkesi, aşağıdaki tablodaki nesnelerden oluşur. Her birinin daha ayrıntılı açıklaması için bkz. [Azure ilke nesneleri](../governance/policy/overview.md#azure-policy-objects) .

| Öğe | Açıklama |
|:---|:---|
| İlke tanımı | Kaynak uyumluluk koşullarını ve bir koşul karşılanırsa gerçekleştirilecek etkiyi açıklar. Bu, belirli bir türdeki veya yalnızca belirli özelliklerle eşleşen kaynakların tüm kaynakları olabilir. Etki, kaynağı uyumluluk veya ilgili bir kaynağı dağıtmaya yönelik olarak işaretlemek olabilir. İlke tanımları, [Azure ilke tanımı yapısı](../governance/policy/concepts/definition-structure.md)bölümünde AÇıKLANDıĞı gibi JSON kullanılarak yazılır. Etkileri [Azure ilke efektlerini anlama](../governance/policy/concepts/effects.md)bölümünde açıklanmaktadır.
| İlke girişimi | Birlikte uygulanması gereken bir ilke tanımları grubu. Örneğin, kaynak günlüklerini bir Log Analytics çalışma alanına göndermek için bir ilke tanımınız olabilir ve başka bir deyişle, kaynak günlüklerini Olay Hub 'larına gönderebilirsiniz. Her iki ilke tanımını da içeren bir girişim oluşturun ve bireysel ilke tanımları yerine girişimi kaynaklara uygulayın. Girişimler, [Azure ilke girişimi yapısında](../governance/policy/concepts/initiative-definition-structure.md)AÇıKLANDıĞı şekilde JSON kullanılarak yazılır. |
| Atama | Bir ilke tanımı veya girişimi bir kapsama atanıncaya kadar etkili olmaz. Örneğin, bu kaynakta oluşturulan tüm kaynaklara uygulamak için bir kaynak grubuna ilke atayın veya bu aboneliği Bu abonelikteki tüm kaynaklara uygulamak için bir aboneliğe uygulayın.  Daha ayrıntılı bilgi için bkz. [Azure ilke atama yapısı](../governance/policy/concepts/assignment-structure.md). |

## <a name="built-in-policy-definitions-for-azure-monitor"></a>Azure Izleyici için yerleşik ilke tanımları
Azure Ilkesi, Azure Izleyici ile ilgili olarak önceden oluşturulmuş çeşitli tanımlar içerir. Bu ilke tanımlarını mevcut aboneliğinize atayabilir veya kendi özel tanımlarınızı oluşturmak için temel olarak kullanabilirsiniz. **İzleme** kategorisindeki yerleşik politiklerin tam bir listesi için bkz. Azure [ilkesi yerleşik Ilke tanımları Azure izleyici](samples/policy-samples.md).

İzleme ile ilgili yerleşik ilke tanımlarını görüntülemek için aşağıdakileri yapın:

1. Azure portal **Azure ilkesi** ' ne gidin.
2. **Tanımlar**' ı seçin.
3. **Tür**için *yerleşik* ' i seçin ve **Kategori**için *izleme*' yi seçin.

  ![Yerleşik ilke tanımları](media/deploy-scale/builtin-policies.png)


## <a name="diagnostic-settings"></a>Tanılama ayarları
[Tanılama ayarları](platform/diagnostic-settings.md) , Azure kaynaklarından gelen kaynak günlüklerini ve ölçümleri, genellikle [günlük sorguları](log-query/log-query-overview.md) ve [günlük uyarıları](platform/alerts-log.md)ile verileri analiz etmenize olanak tanıyan bir Log Analytics çalışma alanına kadar birden fazla konuma toplar. Her kaynak oluşturduğunuzda otomatik olarak bir tanılama ayarı oluşturmak için Ilkeyi kullanın.

Her Azure Kaynak türünün, tanılama ayarında listelenmesi gereken benzersiz bir kategori kümesi vardır. Bu nedenle, her kaynak türü ayrı bir ilke tanımı gerektirir. Bazı kaynak türlerinde, değişiklik yapmadan atayabileceğiniz yerleşik ilke tanımları vardır. Diğer kaynak türleri için özel bir tanım oluşturmanız gerekir.

### <a name="built-in-policy-definitions-for-azure-monitor"></a>Azure Izleyici için yerleşik ilke tanımları
Her kaynak türü için, biri Log Analytics çalışma alanına ve diğeri de Olay Hub 'ına gönderilmek üzere iki yerleşik ilke tanımı vardır. Yalnızca bir konuma ihtiyacınız varsa, bu ilkeyi kaynak türü için atayın. Her ikisine de ihtiyacınız varsa, kaynak için her iki ilke tanımını da atayın.

Örneğin, aşağıdaki görüntüde Data Lake Analytics için yerleşik tanılama ayarı ilke tanımları gösterilmektedir.

  ![Yerleşik ilke tanımları](media/deploy-scale/builtin-diagnostic-settings.png)

### <a name="custom-policy-definitions"></a>Özel ilke tanımları
Yerleşik bir ilkesi olmayan kaynak türleri için özel bir ilke tanımı oluşturmanız gerekir. Bunu, mevcut bir yerleşik ilkeyi kopyalayarak ve ardından kaynak türü için değiştirerek Azure portal el ile yapabilirsiniz. PowerShell Galerisi, ilke program aracılığıyla bir komut dosyası kullanarak oluşturmak daha etkilidir.

[Create-AzDiagPolicy](https://www.powershellgallery.com/packages/Create-AzDiagPolicy) betiği, POWERSHELL veya CLI kullanarak yükleyebileceğiniz belirli bir kaynak türü için ilke dosyaları oluşturur. Tanılama ayarları için özel bir ilke tanımı oluşturmak için aşağıdaki yordamı kullanın.


1. [Azure PowerShell](/powershell/azure/install-az-ps) yüklü olduğundan emin olun.
2. Aşağıdaki komutla betiği yüklersiniz:
  
    ```azurepowershell
    Install-Script -Name Create-AzDiagPolicy
    ```

3. Günlükleri nereye gönderileceğini belirtmek için parametreleri kullanarak betiği çalıştırın. Bir abonelik ve kaynak türü belirtmeniz istenecektir. Örneğin, Log Analytics çalışma alanına ve Olay Hub 'ına gönderen bir ilke tanımı oluşturmak için aşağıdaki komutu kullanın.

   ```azurepowershell
   Create-AzDiagPolicy.ps1 -ExportLA -ExportEH -ExportDir ".\PolicyFiles"  
   ```

4. Alternatif olarak, komutta bir abonelik ve kaynak türü de belirtebilirsiniz. Örneğin, Log Analytics çalışma alanına ve Azure SQL Server veritabanlarına yönelik Olay Hub 'ına gönderen bir ilke tanımı oluşturmak için aşağıdaki komutu kullanın.

   ```azurepowershell
   Create-AzDiagPolicy.ps1 -SubscriptionID xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -ResourceType Microsoft.Sql/servers/databases  -ExportLA -ExportEH -ExportDir ".\PolicyFiles"  
   ```

5. Betik her bir ilke tanımı için, her biri azurepolicy.js, azurepolicy.rules.jsüzerinde azurepolicy.parameters.jsolarak adlandırılan üç dosya içeren ayrı klasörler oluşturur. İlkeyi Azure portal el ile oluşturmak istiyorsanız, tüm ilke tanımını içerdiğinden azurepolicy.jsiçeriğini kopyalayabilir ve yapıştırabilirsiniz. İlke tanımını bir komut satırından oluşturmak için PowerShell veya CLı ile diğer iki dosyayı kullanın.

    Aşağıdaki örneklerde, ilke tanımının hem PowerShell hem de CLı 'dan nasıl yükleneceği gösterilmektedir. Her biri, yeni ilke tanımını yerleşik ilke tanımlarına göre gruplamak için bir **izleme** kategorisi belirleyen meta verileri içerir.

      ```azurepowershell
      New-AzPolicyDefinition -name "Deploy Diagnostic Settings for SQL Server database to Log Analytics workspace" -policy .\Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.rules.json -parameter .\Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.parameters.json -mode All -Metadata '{"category":"Monitoring"}'
      ```

      ```azurecli
      az policy definition create --name 'deploy-diag-setting-sql-database--workspace' --display-name 'Deploy Diagnostic Settings for SQL Server database to Log Analytics workspace'  --rules 'Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.rules.json' --params 'Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.parameters.json' --subscription 'AzureMonitor_Docs' --mode All
      ```

### <a name="initiative"></a>Girişim
Her bir ilke tanımı için bir atama oluşturmak yerine, her bir Azure hizmeti için Tanılama ayarları oluşturmak üzere ilke tanımlarını içeren bir girişim oluşturmak, yaygın bir stratejidir. Ortamınızı yönetme yönteminize bağlı olarak girişim ile yönetim grubu, abonelik veya kaynak grubu arasında bir atama oluşturun. Bu strateji aşağıdaki avantajları sunar:

- Her kaynak türü için birden çok atama yerine girişim için tek bir atama oluşturun. Birden çok izleme grubu, abonelik veya kaynak grubu için aynı girişimi kullanın.
- Yeni bir kaynak türü veya hedefi eklemeniz gerektiğinde girişimi değiştirin. Örneğin, ilk gereksinimleriniz verileri yalnızca bir Log Analytics çalışma alanına göndermek olabilir, ancak daha sonra Olay Hub 'ı eklemek istersiniz. Yeni atamalar oluşturmak yerine girişimi değiştirin.

Girişim oluşturma hakkında ayrıntılı bilgi için bkz. [girişim tanımı oluşturma ve atama](../governance/policy/tutorials/create-and-manage.md#create-and-assign-an-initiative-definition) . Aşağıdaki önerileri göz önünde bulundurun:

- **Kategoriyi** , ilgili yerleşik ve özel ilke tanımlarına göre gruplamak için **izleme** olarak ayarlayın.
- Log Analytics çalışma alanının ayrıntılarını ve girişim içindeki ilke tanımına ait Olay Hub 'ını belirtmek yerine ortak bir girişim parametresi kullanın. Bu, tüm ilke tanımları için ortak bir değeri kolayca belirtmenize ve gerekirse bu değeri değiştirmenize olanak sağlar.

![Girişim tanımı](media/deploy-scale/initiative-definition.png)

### <a name="assignment"></a>Atama 
Bir Azure yönetim grubuna, aboneliğine veya kaynak grubuna, izlenecek kaynaklarınızın kapsamına bağlı olarak girişim atayın. [Yönetim grubu](../governance/management-groups/overview.md) özellikle, kuruluşunuzda birden çok abonelik varsa, özellikle ilke kapsamı için yararlıdır.

![Girişim ataması](media/deploy-scale/initiative-assignment.png)

Girişim parametrelerini kullanarak, girişim içindeki tüm ilke tanımları için çalışma alanını veya başka herhangi bir ayrıntıyı bir kez belirtebilirsiniz. 

![Girişim parametreleri](media/deploy-scale/initiative-parameters.png)

### <a name="remediation"></a>Düzeltme
Girişim, oluşturulduğu gibi her bir sanal makine için de geçerlidir. Bir [Düzeltme görevi](../governance/policy/how-to/remediate-resources.md) , girişim içindeki ilke tanımlarını mevcut kaynaklara dağıtır, bu nedenle bu, önceden oluşturulmuş kaynaklar için Tanılama ayarları oluşturmanızı sağlar. Azure portal kullanarak atama oluşturduğunuzda, aynı anda bir düzeltme görevi oluşturma seçeneğiniz vardır. Düzeltme hakkındaki ayrıntılar için bkz. [Azure ilkesiyle uyumlu olmayan kaynakları](../governance/policy/how-to/remediate-resources.md) düzeltme.

![Girişim düzeltmesi](media/deploy-scale/initiative-remediation.png)


## <a name="azure-monitor-for-vms"></a>VM'ler için Azure İzleyici
[VM'ler için Azure izleyici](insights/vminsights-overview.md) , sanal makineleri Izlemek Için Azure izleyici 'deki birincil araçtır. VM'ler için Azure İzleyici etkinleştirildiğinde hem Log Analytics Aracısı hem de bağımlılık Aracısı yüklenir. Bu görevleri el ile gerçekleştirmek yerine, her bir sanal makinenin oluşturduğunuz şekilde yapılandırıldığından emin olmak için Azure Ilkesini kullanın.

> [!NOTE]
> VM'ler için Azure İzleyici, ortamınızda uyumsuz VM 'Leri keşfetmenize ve düzeltmenizi sağlayan **VM'ler için Azure izleyici Ilke kapsamı** adlı bir özellik içerir. Bu özelliği, Azure sanal makineleri için Azure Ilkesiyle ve Azure Arc ile bağlantılı karma sanal makinelerde doğrudan çalışmak yerine kullanabilirsiniz. Azure sanal makine ölçek kümeleri için, atamayı Azure Ilkesi kullanarak oluşturmanız gerekir.
 

VM'ler için Azure İzleyici, tam izlemeyi etkinleştirmek için her iki aracıyı de yükleyen aşağıdaki yerleşik girişimleri içerir. 

|Ad |Açıklama |
|:---|:---|
|VM'ler için Azure İzleyici etkinleştir | Azure sanal makinelerine ve Azure Arc ile bağlantılı karma VM 'Lere Log Analytics aracısını ve bağımlılık aracısını yükleme. |
|Sanal Makine Ölçek Kümeleri için Azure Izleyicisini etkinleştir | Azure sanal makine ölçek kümesine Log Analytics aracısını ve bağımlılık aracısını yükleme. |


### <a name="virtual-machines"></a>Sanal makineler
VM'ler için Azure İzleyici, Azure Ilke arabirimini kullanarak bu girişimler için Atamalar oluşturmak yerine, her kapsamdaki sanal makine sayısını incelemenizi sağlayan bir özellik içerir ve bu da girişim uygulanıp uygulanmadığı belirlenir. Daha sonra çalışma alanını yapılandırabilir ve bu arabirimi kullanarak gerekli atamaları oluşturabilirsiniz.

Bu işlemin ayrıntıları için bkz. [Azure ilkesi kullanarak VM'ler için Azure izleyici etkinleştirme](./insights/vminsights-enable-policy.md).

![VM'ler için Azure İzleyici ilkesi](media/deploy-scale/vminsights-policy.png)

### <a name="virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri
Sanal Makine Ölçek Kümeleri için izlemeyi etkinleştirmek üzere Azure Ilkesini kullanmak için, izlemek istediğiniz kaynaklarınızın kapsamına bağlı olarak **Sanal Makine Ölçek Kümeleri Için Azure Izleyicisini etkinleştirme** girişimi ' ni bir Azure yönetim grubuna, aboneliğine veya kaynak grubuna atayın. [Yönetim grubu](../governance/management-groups/overview.md) özellikle, kuruluşunuzda birden çok abonelik varsa, özellikle ilke kapsamı için yararlıdır.

![Girişim ataması](media/deploy-scale/virtual-machine-scale-set-assign-initiative.png)

Verilerin gönderileceği çalışma alanını seçin. Bu çalışma alanı, bölümünde açıklandığı gibi, *Vminsıghts* çözümünün yüklü olması gerekir []() .

![Çalışma alanını seçme](media/deploy-scale/virtual-machine-scale-set-workspace.png)

Bu ilkeye atanması gereken sanal makine ölçek kümesine sahipseniz bir düzeltme görevi oluşturun.

![Düzeltme görevi](media/deploy-scale/virtual-machine-scale-set-remediation.png)

### <a name="log-analytics-agent"></a>Log Analytics aracısı
Log Analytics aracısını yüklemek istediğiniz ancak bağımlılık aracısını değil senaryolar olabilir. Yalnızca aracı için yerleşik girişim yoktur, ancak VM'ler için Azure İzleyici tarafından sunulan yerleşik ilke tanımlarına göre kendi kendinize de oluşturabilirsiniz.

> [!NOTE]
> Log Analytics aracısının verileri Azure Izleyici 'ye teslim etmesini gerektirdiğinden, bağımlılık aracısının kendi kendine dağıtılması bir neden olmaz.


|Ad |Açıklama |
|-----|------------|
|Denetim Log Analytics aracı dağıtımı – VM görüntüsü (OS) listelenmemiş |VM görüntüsü (OS) listede tanımlanmamışsa ve aracı yüklü değilse, VM 'Leri uyumsuz olarak raporlar. |
|Linux sanal makineleri için Log Analytics aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Linux VM 'Leri için Log Analytics Aracısı dağıtın. |
|Windows VM 'Leri için Log Analytics aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Windows VM 'Leri için Log Analytics Aracısı dağıtın. |
| [Önizleme]: Log Analytics Aracı, Linux Azure yay makinelerinizde yüklü olmalıdır |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, karma Azure yay makinelerini Linux VM 'Ler için uyumsuz olarak raporlar. |
| [Önizleme]: aracı Log Analytics Windows Azure Arc makinelerinizde yüklü olmalıdır |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, karma Azure yay makinelerini Windows VM 'Ler için uyumsuz olarak raporlar. |
| [Önizleme]: Linux Azure yay makinelerine Log Analytics aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Linux karma Azure yay makineleri için Log Analytics Aracısı dağıtın. |
| [Önizleme]: Windows Azure yay makinelerine Log Analytics aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Windows karma Azure yay makineleri için Log Analytics Aracısı dağıtın. |
|Sanal makine ölçek kümelerinde denetim bağımlılığı Aracısı dağıtımı-VM görüntüsü (OS) listelenmemiş |VM görüntüsü (OS) listede tanımlanmamışsa ve aracı yüklü değilse, sanal makine ölçek kümesini uyumsuz olarak bildirir. |
|Sanal makine ölçek kümelerinde denetim Log Analytics Aracısı dağıtımı-VM görüntüsü (OS) listelenmemiş |VM görüntüsü (OS) listede tanımlanmamışsa ve aracı yüklü değilse, sanal makine ölçek kümesini uyumsuz olarak bildirir. |
|Linux sanal makine ölçek kümeleri için Log Analytics aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Linux sanal makine ölçek kümeleri için Log Analytics Aracısı dağıtın. |
|Windows sanal makine ölçek kümeleri için Log Analytics aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Windows sanal makine ölçek kümeleri için Log Analytics Aracısı dağıtın. |


## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilkesi](../governance/policy/overview.md)hakkında daha fazla bilgi edinin.
- [Tanılama ayarları](platform/diagnostic-settings.md)hakkında daha fazla bilgi edinin.