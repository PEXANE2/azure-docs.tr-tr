---
title: Azure HDInsight küme oluşturma-hata sözlüğü
description: Azure HDInsight kümeleri oluştururken oluşan hataların nasıl giderileceği hakkında bilgi edinin
author: karkrish
ms.author: v-todmc
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 08/24/2020
ms.openlocfilehash: cae8647d970020a22d59dc49b058d43fe28dd00c
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88816465"
---
# <a name="azure-hdinsight-cluster-creation-errors"></a>Azure HDInsight: küme oluşturma hataları

Bu makalede, küme oluştururken karşılaşabileceğiniz hatalara yönelik çözümler açıklanmaktadır.

> [!NOTE]
> Bu makalede açıklanan ilk üç hata doğrulama hatalardır. Bir Azure HDInsight ürünü **CsmDocument_2_0** sınıfını kullandığında bu durum ortaya çıkabilir.

## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>Hata kodu: ' CsmDocument_2_0 ' DeploymentDocument doğrulaması başarısız oldu

**Hata**: "betik eylemi konumuna erişilemiyor URI: \<SCRIPT ACTION URL\> "

### <a name="error-message-1"></a>Hata iletisi 1

"Uzak sunucu bir hata döndürdü: (404) bulunamadı."

#### <a name="cause"></a>Nedeni

HDInsight hizmeti, küme oluşturma isteği kapsamında verdiğiniz betik eylemi URL 'sine erişemiyor. Hizmet, betik eylemine erişmeye çalıştığında önceki hata iletisini alır.

#### <a name="resolution"></a>Çözüm

- HTTP veya HTTPS URL 'SI için, bu URL 'yi bir ınbilito tarayıcı penceresinden gitmeye çalışırken doğrulayın.
- Bir fgb URL 'SI için, betiğin istekte verdiğiniz depolama hesabında bulunduğundan emin olun. Ayrıca, bu depolama hesabı için depolama anahtarının doğru olduğundan emin olun.
- Bir ADLS URL 'SI için, betiğin depolama hesabında bulunduğundan emin olun.

---

### <a name="error-message-2"></a>Hata iletisi 2

"Verilen betik URI 'SI \<SCRIPT_URI\> ADLS 'de, ancak bu kümede Data Lake Storage sorumlusu yok"

#### <a name="cause"></a>Nedeni

HDInsight hizmeti, küme oluşturma isteği kapsamında verdiğiniz betik eylemi URL 'sine erişemiyor. Hizmet, betik eylemine erişmeye çalıştığında önceki hata iletisini alır.

#### <a name="resolution"></a>Çözüm

Karşılık gelen Azure Data Lake Storage Gen 1 hesabını kümeye ekleyin. Ayrıca, Data Lake Storage Gen 1 hesabına erişen hizmet sorumlusunu kümeye ekleyin.

---

### <a name="error-message-3"></a>Hata iletisi 3

İstekte belirtilen ' ' VM boyutu ' \<CUSTOMER_SPECIFIED_VM_SIZE\> ' geçersiz veya ' ' rolü için desteklenmiyor \<ROLE\> . Geçerli değerler şunlardır: \<VALID_VM_SIZE_FOR_ROLE\> . "

#### <a name="cause"></a>Nedeni

Belirttiğiniz sanal makine boyutuna rol için izin verilmiyor. Bu hata, VM boyut değerinin beklendiği gibi çalışmadığı veya bilgisayar rolü için uygun olmadığı için oluşabilir.

#### <a name="resolution"></a>Çözüm

Hata iletisi, VM boyutu için geçerli değerleri listeler. Bu değerlerden birini seçin ve küme oluştur isteğini yeniden deneyin.

---

## <a name="error-codeinvalidvirtualnetworkid"></a>Hata kodu: ınvalidvirtualnetworkıd  

### <a name="error"></a>Hata

"Virtualnetworkıd geçerli değil. Virtualnetworkıd ' \<USER_VIRTUALNETWORKID\> ' * "

### <a name="cause"></a>Nedeni

Küme oluşturma sırasında belirttiğiniz **Virtualnetworkıd** değeri doğru biçimde değil.

### <a name="resolution"></a>Çözüm

**Virtualnetworkıd** ve subnet değerlerinin doğru biçimde olduğundan emin olun. **Virtualnetworkıd** değerini almak için:

1. Azure portala gidin.
1. Sanal ağınızı seçin.
1. **Özellikler** menü öğesini seçin. **RESOURCEID** özelliği değeri **virtualnetworkıd** değeridir.

Bir sanal ağ KIMLIĞI örneği aşağıda verilmiştir:

"/subscriptions/c15fd9b8-e2b8-1d4e-aa85-2e668040233b/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/myvnet"

---

## <a name="error-code-customizationfailederrorcode"></a>Hata kodu: CustomizationFailedErrorCode

### <a name="error"></a>Hata

"Özel betik eyleminde bir hata nedeniyle küme dağıtımı başarısız oldu. Başarısız eylemler: \<SCRIPT_NAME\> , hata ayıklamak için lütfen ambarı Kullanıcı arabirimine gidin. "

### <a name="cause"></a>Nedeni

Küme oluşturma isteği sırasında verdiğiniz özel betik, küme başarıyla dağıtıldıktan sonra yürütülür. Bu hata kodu, adlı özel betiğin yürütülmesi sırasında bir hata olduğunu gösterir \<SCRIPT_NAME\> .

### <a name="resolution"></a>Çözüm

Betik özel betiğinizin bulunduğu için, sorunu gidermeniz ve gerekirse betiği yeniden çalıştırmanız önerilir. Betik başarısızlığının sorunlarını gidermek için,/var/lib/Ambari-Agent/* klasöründeki günlükleri inceleyin. Ya da, **işlem sayfasını ambarı** Kullanıcı arabiriminde açın ve ardından hata ayrıntılarını görüntülemek için **run_customscriptaction** işlemini seçin.

---

## <a name="error-codeinvaliddocumenterrorcode"></a>Hata kodu: ınvalidbelgeenterrorcode

### <a name="error"></a>Hata

" \<META_STORE_TYPE\> Veritabanında meta veri deposu şema sürümü, \<METASTORE_MAJOR_VERSION\> \<DATABASE_NAME\> küme sürümü ile uyumsuz \<CLUSTER_VERSION\> "

### <a name="cause"></a>Nedeni

Özel meta veri deposu, seçili HDInsight kümesi sürümüyle uyumlu değil. Şu anda, HDInsight 4,0 kümeleri yalnızca meta veri sürümü 3,0 ve üstünü destekler, ancak HDInsight 3,6 kümeleri, meta Tasileri sürümü 3,0 ve üstünü desteklemez.

### <a name="resolution"></a>Çözüm

HDInsight kümesi sürümünüzün desteklediği yalnızca meta veri deposu sürümlerini kullanın. Özel bir meta veri deposu belirtmezseniz, HDInsight dahili olarak bir meta veri deposu oluşturur ve ardından küme silme sonrasında onu siler.

---

## <a name="error-code-failedtoconnectwithclustererrorcode"></a>Hata kodu: FailedToConnectWithClusterErrorCode 

### <a name="error"></a>Hata

"Ölçeklendirme işlemi gerçekleştirmek için küme yönetimi uç noktasına bağlanılamadı. Ağ güvenlik kurallarının kümeye dış erişimi engellemediğini ve Küme Yöneticisi (ambarı) Kullanıcı arabirimine başarıyla erişildiğini doğrulayın. "

### <a name="cause"></a>Nedeni

Ağ güvenlik grubunuzda (NSG) bulunan bir güvenlik duvarı kuralı, kritik Azure sistem durumu ve yönetim hizmetleriyle küme iletişimini engelliyor.

### <a name="resolution"></a>Çözüm

Ağ trafiğini denetlemek için ağ güvenlik grupları kullanmayı planlıyorsanız, HDInsight 'ı yüklemeden önce aşağıdaki işlemleri gerçekleştirin:

- HDInsight için kullanmayı planladığınız Azure bölgesini belirler.
- HDInsight için gereken IP adreslerini belirler. Daha fazla bilgi için bkz. [HDInsight yönetimi IP adresleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses).
  - HDInsight 'ı yüklemeyi planladığınız alt ağ için ağ güvenlik grupları oluşturun veya değiştirin.
  - Ağ güvenlik grupları için IP adreslerinden 443 numaralı bağlantı noktasında gelen trafiğe izin verin. Bu yapılandırma, HDInsight Yönetim Hizmetleri 'nin kümeye sanal ağ dışından erişmesini sağlar.

---

## <a name="error-code-storagepermissionsblockedformsi"></a>Hata kodu: Storagepermissionsblockedformsı

### <a name="error"></a>Hata

"Yönetilen kimliğin depolama hesabında izinleri yok. Lütfen depolama hesabı için yönetilen kimliğe ' Depolama Blobu veri sahibi ' rolünün atandığını doğrulayın. Depolama:/Subscriptions/ \<Subscription ID\> /ResourceGroups/ \< Resource Group Name\> /providers/Microsoft.Storage/storageAccounts/ \<Storage Account Name\> , Managed Identity:/Subscriptions/ \<Subscription ID\> /ResourceGroups// \< Resource Group Name\> /providers/Microsoft.ManagedIdentity/userAssignedIdentities/ \<User Managed Identity Name\> "

### <a name="cause"></a>Nedeni

Kimliği yönetmek için gerekli izinleri sağlamamadınız. Kullanıcı tarafından atanan yönetilen kimliğin Azure Data Lake Storage 2. depolama hesabında BLOB depolama katılımcısı rolü yok.

### <a name="resolution"></a>Çözüm

1. Azure portalını açın.
1. Depolama hesabınıza gidin.
1. **Access Control (IAM)** bölümüne bakın.
1. Kullanıcının Depolama Blobu veri katılımcısı rolüne veya bunlara atanmış Depolama Blobu veri sahibi rolüne sahip olduğundan emin olun.

Daha fazla bilgi için bkz. [Data Lake Storage 2. hesabındaki yönetilen kimlik için Izinleri ayarlama](hdinsight-hadoop-use-data-lake-storage-gen2.md).

---

## <a name="error-code-invalidnetworksecuritygroupsecurityrules"></a>Hata kodu: ınvalidnetworksecuritygroupsecurityrules

### <a name="error"></a>Hata

"Ağ güvenlik grubundaki/Subscriptions/ \<SubscriptionID\> /resourcegroups/<kaynak grubu adı \> varsayılan/sağlayıcıları/Microsoft. Network/networkSecurityGroups/ \<Network Security Group Name\> alt ağ/Subscriptions/ \<SubscriptionID\> /ResourceGroups/ \<Resource Group name\> RG-westeurope-VNET-TomTom-default/Providers/Microsoft. Network/virtualnetworks//Subnets/ile yapılandırılan güvenlik kuralları, \<Virtual Network Name\> \<Subnet Name\> gerekli gelen ve/veya giden bağlantılara izin vermiyor. Daha fazla bilgi için lütfen [Azure HDInsight için bir sanal ağ planlayın](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment)veya desteğe başvurun. "

### <a name="cause"></a>Nedeni

Ağ güvenlik grupları veya Kullanıcı tanımlı yollar (UDRs) HDInsight kümenize gelen trafiği denetyorsa, kümenizin kritik Azure sistem durumu ve yönetim hizmetleriyle iletişim kurabildiğinden emin olun.

### <a name="resolution"></a>Çözüm

Ağ trafiğini denetlemek için ağ güvenlik grupları kullanmayı planlıyorsanız, HDInsight 'ı yüklemeden önce aşağıdaki işlemleri gerçekleştirin:

- HDInsight için kullanmayı planladığınız Azure bölgesini tanımlayabilir ve bölgeniz için IP adreslerinin güvenli bir listesini oluşturun. Daha fazla bilgi için bkz. [sistem durumu ve Yönetim Hizmetleri: belirli bölgeler](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses#health-and-management-services-specific-regions).
- HDInsight 'ın gerektirdiği IP adreslerini belirler. Daha fazla bilgi için bkz. [HDInsight YÖNETIM IP adresleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses).
- HDInsight 'ı yüklemeyi planladığınız alt ağ için ağ güvenlik grupları oluşturun veya değiştirin. Ağ güvenlik grupları için IP adreslerinden 443 numaralı bağlantı noktasında gelen trafiğe izin verin. Bu yapılandırma, HDInsight Yönetim Hizmetleri 'nin kümeye sanal ağ dışından erişmesini sağlar.

---

## <a name="error-code-cluster-setup-failed-to-install-components-on-one-or-more-hosts"></a>Hata kodu: küme kurulumu bir veya daha fazla konağa bileşen yükleyemedi

### <a name="error"></a>Hata

"Küme kurulumu bir veya daha fazla konağa bileşen yükleyemedi. Lütfen isteğinizi yeniden deneyin. "

### <a name="cause"></a>Nedeni 

Genellikle bu hata, geçici bir sorun veya bir Azure kesintisi olduğunda oluşturulur.

### <a name="resolution"></a>Çözüm

Küme dağıtımını etkileyebilecek tüm Azure kesintileri için [Azure durum](https://status.azure.com) sayfasına bakın. Kesintiler yoksa, küme dağıtımını yeniden deneyin.

---

## <a name="error-code-failedtoconnectwithclustererrorcode"></a>Hata kodu: FailedToConnectWithClusterErrorCode

### <a name="error"></a>Hata

Küme yönetimi uç noktasına bağlanılamadı. Lütfen daha sonra yeniden deneyin.

### <a name="cause"></a>Nedeni

HDInsight hizmeti, kümeyi oluşturmaya çalışırken kümenize bağlanamaz

### <a name="resolution"></a>Çözüm

Özel VNet ağ güvenlik grubu (NSG) ve Kullanıcı tanımlı yollar (UDRs) kullanıyorsanız, kümenizin HDInsight yönetim hizmetleriyle iletişim kurabildiğinden emin olun. Daha fazla bilgi için bkz. [HDInsight YÖNETIM IP adresleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses).

---

## <a name="error-code-deployments-failed-due-to-policy-violation-resource-resource-uri-was-disallowed-by-policy-policy-identifiers-policyassignmentnamepolicy-name-idprovidersmicrosoftmanagementmanagementgroupsmanagement-group-name-providersmicrosoftauthorizationpolicyassignmentspolicy-namepolicydefinition-policy-definition"></a>Hata kodu: bir ilke ihlali nedeniyle dağıtımlar başarısız oldu: ' kaynak ' <Resource URI> ' ilke tarafından izin verilmedi. İlke tanımlayıcıları: ' [{"policyAssignment": {"Name": " <Policy Name> ", "ID": "/Providers/Microsoft.Management/managementGroups/ <Management Group Name> providers/Microsoft. Authorization/policyasers/ <Policy Name> "}, "policydefinition": <Policy Definition>

### <a name="cause"></a>Nedeni

Abonelik tabanlı Azure ilkeleri, genel IP adreslerinin oluşturulmasını reddedebilir. HDInsight kümesi oluşturmak için iki genel IP gerekir.

Aşağıdaki ilkeler genellikle küme oluşturmayı etkiler:

* Abonelik içinde IP adresleri veya yük dengeleyiciler oluşturulmasını önleyen ilkeler.
* Depolama hesaplarının oluşturulmasını önleyen ilke.
* IP adresleri veya yük dengeleyiciler gibi ağ kaynaklarının silinmesini önleyen ilke.

### <a name="resolution"></a>Çözüm

HDInsight kümesi oluştururken abonelik tabanlı Azure Ilke atamasını silin veya devre dışı bırakın.

---

## <a name="next-steps"></a>Sonraki adımlar

Küme oluşturma hatalarında sorun giderme hakkında daha fazla bilgi için bkz. [Azure HDInsight ile küme oluşturma hatalarında sorun giderme](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-troubleshoot-cluster-creation-fails).
