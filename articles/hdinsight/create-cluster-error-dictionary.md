---
title: Küme hata sözlüğü oluşturma
description: Küme hata sözlüğü oluşturmayı öğrenin.
ms.reviewer: hrasheed
author: karkrish
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: troubleshooting
ms.date: 11/19/2019
ms.author: v-todmc
ms.openlocfilehash: e537014f741196024c24dd6ee98af0d8af2e1b31
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75483069"
---
# <a name="hdinsight-cluster-creation-errors"></a>HDInsight: küme oluşturma hataları

Bu makalede, kümeler oluşturulurken karşılaşılan hatalara yönelik çözümler açıklanmaktadır. 

> [!NOTE]
> Aşağıdaki listedeki ilk üç hata, CsmDocument_2_0 sınıfı bir HDInsight ürünü tarafından kullanıldığında doğrulama hataları nedeniyle oluşur.



## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>Hata kodu: ' CsmDocument_2_0 ' DeploymentDocument doğrulaması başarısız oldu

### <a name="error-script-action-location-cannot-be-accessed-uriscript-action-url"></a>**Hata**: betik eylemi konumuna erişilemiyor URI:\<betik eylemi URL 'si\>

**Hata iletisi: "uzak sunucu bir hata döndürdü: (404) bulunamadı."**

### <a name="cause"></a>Nedeni
Küme oluşturma isteği kapsamında sunulan betik eylemi URL 'SI HDInsight hizmetinden erişilebilir değil. Betik eylemine erişmeye çalıştığınızda "ErrorMessage" gönderilir.

### <a name="resolution"></a>Çözünürlük 
  - Bir http/https URL 'SI için, URL 'YI bir tarayıcı penceresinden açmaya çalışırken doğrulayabilirsiniz. 
  - Bir fgb URL 'SI için, betiğin istekte verilen depolama hesabında bulunduğundan emin olun. Bu depolama hesabı için depolama anahtarının doğru olduğundan emin olun. 
  - Bir ADLS URL 'SI için, betiğin depolama hesabında bulunduğundan emin olun.

---

## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>Hata kodu: ' CsmDocument_2_0 ' DeploymentDocument doğrulaması başarısız oldu

### <a name="error-script-action-location-cannot-be-accessed-uri-script-action-url"></a>**Hata**: betik eylemi konumuna erişilemiyor URI: \<betik eylemi URL 'si\>

**Hata iletisi: verilen komut dosyası URI 'si \<BETIĞI URI 'SI\> ADLS içinde ve bu kümede Data Lake Storage sorumlusu yok**

### <a name="cause"></a>Nedeni
Küme oluştur isteği kapsamında sunulan betik eylemi URL 'SI HDInsight hizmetinden erişilebilir değil. Betik eylemine erişmeye çalıştığınızda "ErrorMessage" gönderilir. 

### <a name="resolution"></a>Çözünürlük

Karşılık gelen Azure Data Lake Store Gen 1 hesabının kümeye eklendiğinden emin olun. Azure Data Lake Store Gen 1 hesabına erişmek için kullanılan hizmet sorumlusu kümeye da eklenir. 

---

## <a name="error-code-deploymentdocument-csmdocument_2_0-failed-the-validation"></a>Hata kodu: ' CsmDocument_2_0 ' DeploymentDocument doğrulaması başarısız oldu

### <a name="error-vm-size-customer-specified-vm-size-provided-in-the-request-is-invalid-or-not-supported-for-role-role-valid-values-are-valid-vm-size-for-role"></a>**Hata**: istekte belirtilen '\<müşteri adı\>' VM boyutu geçersiz veya '\<role\>' rolü için desteklenmiyor. Geçerli değerler: rol\>IÇIN GEÇERLI VM boyutu \<.

### <a name="cause"></a>Nedeni
Rol için müşterinin belirttiği VM boyutlarına izin verilmiyor. VM boyut değeri beklendiği gibi çalışmadığından veya bilgisayar rolü için uygun olmadığından, bu doğru olabilir. 

### <a name="resolution"></a>Çözünürlük
Hata iletisi, VM boyutu için geçerli değerleri listeler. Bu geçerli değerlerden birini seçin ve küme oluştur isteğini yeniden deneyin. 

---

## <a name="error-codeinvalidvirtualnetworkid"></a>Hata kodu: ınvalidvirtualnetworkıd  

### <a name="error-the-virtualnetworkid-is-not-valid-virtualnetworkid-user_virtualnetworkid"></a>**Hata**: Virtualnetworkıd geçerli değil. Virtualnetworkıd '\<USER_VIRTUALNETWORKID\>' *

### <a name="cause"></a>Nedeni
Küme oluşturma sırasında belirtilen **Virtualnetworkıd** değeri doğru biçimde değil. 

### <a name="resolution"></a>Çözünürlük
**Virtualnetworkıd** ve subnet 'in doğru biçimde olduğundan emin olun. **Virtualnetworkıd** değerini almak için Azure Portal gidin, Sanal ağınızı seçin ve sonra menüdeki **Özellikler** ' i seçin. **RESOURCEID** özelliği **virtualnetworkıd** değeridir. 

Sanal ağ KIMLIĞINE bir örnek: 

"/subscriptions/c15fd9b8-e2b8-1d4e-aa85-2e668040233b/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/myvnet" 

---

## <a name="error-code-customizationfailederrorcode"></a>Hata kodu: CustomizationFailedErrorCode

### <a name="error-cluster-deployment-failed-due-to-an-error-in-the-custom-script-action-failed-actions-script_name-please-go-to-ambari-ui-to-further-debug-the-failure"></a>**Hata**: özel betik eyleminde bir hata nedeniyle küme dağıtımı başarısız oldu. Başarısız eylemler: \<SCRIPT_NAME\>, hata ayıklamak için lütfen ambarı Kullanıcı arabirimine gidin.

### <a name="cause"></a>Nedeni
Küme oluşturma isteği sırasında girilen kullanıcının özel betiği, küme başarıyla dağıtıldıktan sonra yürütülür. Bu hata kodu, \<SCRIPT_NAME\>adında bu özel betik yürütülürken bir hata ile karşılaşıldığını gösterir.   

### <a name="resolution"></a>Çözünürlük
Bu kullanıcının özel komut dosyası olduğundan, kullanıcılar sorunu gidermeniz ve gerekirse betiği yeniden çalıştırabilmelidir. Betik başarısızlığının sorunlarını gidermek için,/var/lib/Ambari-Agent/* klasöründeki günlükleri inceleyin. Ya da, işlem sayfasını, ambarı Kullanıcı arabiriminde açın ve ardından hata ayrıntılarını görüntülemek için **run_customscriptionaction** işlemini seçin. 

---

## <a name="error-codeinvaliddocumenterrorcode"></a>Hata kodu: ınvalidbelgeenterrorcode

### <a name="error-the-meta_store_type-metastore-schema-version-metastore_major_version-in-database-database_name-is-incompatible-with-cluster-version-cluster_version"></a>**Hata**: \<META_STORE_TYPE\> meta veri deposu şema sürümü \<METASTORE_MAJOR_VERSION\> \<database_name, küme sürümü\> \<uyumlu değil CLUSTER_VERSION

### <a name="cause"></a>Nedeni
Özel meta veri deposu, seçili HDInsight kümesi sürümüyle uyumlu değil. Şu anda, HDInsight 4,0 kümesi yalnızca meta veri deposu 3 ' ü destekler. *x*ve HDInsight 3,6, meta veri deposu sürüm 3 ' ü desteklemez. *x* veya üzeri. 

### <a name="resolution"></a>Çözünürlük
Her HDInsight küme sürümü tarafından desteklenen yalnızca meta veri deposu sürümlerini kullandığınızdan emin olun. Özel bir meta veri deposu belirtilmemişse, HDInsight 'ın dahili olarak bir meta veri deposu oluşturduğunu unutmayın. Ancak, bu meta veri kümesi silme işlemi sonrasında otomatik olarak silinir. 

---

## <a name="error-code-failedtoconnectwithclustererrorcode"></a>Hata kodu: FailedToConnectWithClusterErrorCode 

### <a name="error-unable-to-connect-to-cluster-management-endpoint-to-perform-scaling-operation-verify-that-network-security-rules-are-not-blocking-external-access-to-the-cluster-and-that-the-cluster-manager-ambari-ui-can-be-successfully-accessed"></a>**Hata**: ölçeklendirme işlemi gerçekleştirmek için küme yönetimi uç noktasına bağlanılamadı. Ağ güvenlik kurallarının kümeye dış erişimi engellemediğini ve Küme Yöneticisi (ambarı) Kullanıcı arabirimine başarıyla erişildiğini doğrulayın.

### <a name="cause"></a>Nedeni
Ağ güvenlik grubunuzda (NSG), kritik Azure sistem durumu ve yönetim hizmetleriyle küme iletişimini engelleyen bir güvenlik duvarı kuralınız vardır. 

### <a name="resolution"></a>Çözünürlük
Ağ trafiğini denetlemek için  **ağ güvenlik grupları** kullanmayı planlıyorsanız, HDInsight 'ı yüklemeden önce aşağıdaki işlemleri gerçekleştirin: 
  - HDInsight için kullanmayı planladığınız Azure bölgesini belirler. 
  - HDInsight için gereken IP adreslerini belirler. Daha fazla bilgi için bkz. [HDInsight yönetimi IP adresleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses). 
    - HDInsight 'ı yüklemeyi planladığınız alt ağ için ağ güvenlik grupları oluşturun veya değiştirin. 
    - **Ağ güvenlik grupları:**   **443** numaralı bağlantı noktasında IP adreslerinden **gelen** trafiğe izin verin. Bu, HDInsight Yönetim Hizmetleri 'nin kümeye sanal ağ dışından ulaşmasını sağlar. 

---

## <a name="error-code-storagepermissionsblockedformsi"></a>Hata kodu: Storagepermissionsblockedformsı  

### <a name="error-the-managed-identity-does-not-have-permissions-on-the-storage-account-please-verify-that-storage-blob-data-owner-role-is-assigned-to-the-managed-identity-for-the-storage-account-storage-subscriptions-subscription-id-resourcegroups-resource-group-name-providersmicrosoftstoragestorageaccounts-storage-account-name-managed-identity-subscriptions-subscription-id-resourcegroups--resource-group-name-providersmicrosoftmanagedidentityuserassignedidentities-user-managed-identity-name"></a>**Hata**: yönetilen kimliğin depolama hesabı üzerinde izni yok. Lütfen depolama hesabı için yönetilen kimliğe ' Depolama Blobu veri sahibi ' rolünün atandığını doğrulayın. Depolama:/Subscriptions/\<abonelik KIMLIĞI\>/resourceGroups/\< kaynak grubu adı\>/providers/Microsoft.Storage/storageAccounts/\<depolama hesabı adı\>, yönetilen kimlik:/Subscriptions/\<abonelik KIMLIĞI\>/resourceGroups//\< kaynak grubu adı\>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/\<Kullanıcı tarafından yönetilen kimlik adı\>

### <a name="cause"></a>Nedeni
**Kimliği yönetmek** için gerekli izinler sağlanmadı. **Kullanıcı tarafından atanan yönetilen kimliğin** ADLS 2. depolama hesabında BLOB depolama katılımcısı rolü yoktu. 

### <a name="resolution"></a>Çözünürlük

Azure portal açın, depolama hesabınıza gidin, **Access Control (IAM)** bölümüne bakın ve Depolama Blobu veri katılımcısı veya Depolama Blobu veri sahibi rolünün, abonelik için **Kullanıcı tarafından atanan yönetilen kimliğe** "" atanmış "erişime sahip olduğundan emin olun. Daha fazla bilgi için bkz. [Data Lake Storage 2. hesabındaki yönetilen kimlik için Izinleri ayarlama](hdinsight-hadoop-use-data-lake-storage-gen2.md). 

---

## <a name="error-code-invalidnetworksecuritygroupsecurityrules"></a>Hata kodu: ınvalidnetworksecuritygroupsecurityrules  

### <a name="error-the-security-rules-in-the-network-security-group-subscriptionssubscriptionidresourcegroupsresource-group-name-defaultprovidersmicrosoftnetworknetworksecuritygroupsnetwork-security-group-name-configured-with-subnet-subscriptionssubscriptionidresourcegroupsresource-group-name-rg-westeurope-vnet-tomtom-defaultprovidersmicrosoftnetworkvirtualnetworksvirtual-network-namesubnetssubnet-name"></a>**Hata**: ağ güvenlik grubu/Subscriptions/\<SubscriptionID >, resourcegroups/<Resource Group name> varsayılan/Providers/Microsoft\/güvenlik kuralları. Network/networkSecurityGroups/\<ağ güvenlik grubu adı\> alt ağ/Subscriptions/\<subscriptionıd >\/ResourceGroups/\<kaynak grubu adı > RG-westeurope-VNET-TomTom-default\/Providers/Microsoft. Network/virtualnetworks/\<Virtual Ağ adı >\/alt ağları/\<alt ağ adı\> 
gerekli gelen ve/veya giden bağlantıya izin vermez. Daha fazla bilgi için lütfen [Azure HDInsight için bir sanal ağ planlayın](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment)veya desteğe başvurun. * *

### <a name="cause"></a>Nedeni
HDInsight kümenize gelen trafiği denetlemek için ağ güvenlik grupları (NSG 'ler) veya Kullanıcı tanımlı yollar (UDRs) kullanıyorsanız, kümenizin kritik Azure sistem durumu ve yönetim hizmetleriyle iletişim kurabildiğinden emin olmanız gerekir.

### <a name="resolution"></a>Çözünürlük
Ağ trafiğini denetlemek için  **ağ güvenlik grupları** kullanmayı planlıyorsanız, HDInsight 'ı yüklemeden önce aşağıdaki işlemleri gerçekleştirin: 
  - HDInsight için kullanmayı planladığınız Azure bölgesini belirleyip bölgeniz için IP adreslerinin güvenli bir listesini oluşturun: [sistem durumu ve Yönetim Hizmetleri: belirli bölgeler](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses#health-and-management-services-specific-regions).
  - HDInsight için gereken IP adreslerini belirler. Daha fazla bilgi için bkz. [HDInsight YÖNETIM IP adresleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses). 
  - HDInsight 'ı yüklemeyi planladığınız alt ağ için ağ güvenlik grupları oluşturun veya değiştirin. **Ağ güvenlik grupları**: bağlantı noktası **443** IP adreslerinden gelen trafiğe izin verin. Bu, HDInsight Yönetim Hizmetleri 'nin kümeye sanal ağ dışından ulaşmasını güvence altına alabilir.
  
---

## <a name="error-code-cluster-setup-failed-to-install-components-on-one-or-more-hosts"></a>Hata kodu: küme kurulumu bir veya daha fazla konağa bileşen yükleyemedi

###  <a name="errorcluster-setup-failed-to-install-components-on-one-or-more-hosts-please-retry-your-request"></a>**Hata**: küme kurulumu bir veya daha fazla konağa bileşen yükleyemedi. Lütfen isteğinizi yeniden deneyin. 

### <a name="cause"></a>Nedeni 
Genellikle bu hata, geçici bir sorun olduğunda veya bir Azure kesintisi olduğunda üretilir 

### <a name="resolution"></a>Çözünürlük

Küme dağıtımını etkileyebilecek olası Azure kesintileri için [Azure durum](https://status.azure.com/status) sayfasına bakın. Herhangi bir kesinti yoksa, küme dağıtımını yeniden deneyin. 

## <a name="next-steps"></a>Sonraki Adımlar

Küme oluşturma ile ilgili sorun giderme sorunları hakkında daha fazla bilgi için bkz. [Azure HDInsight ile küme oluşturma hatalarında sorun giderme](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-troubleshoot-cluster-creation-fails).
