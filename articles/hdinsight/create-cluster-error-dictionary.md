---
title: Azure HDInsight Küme oluşturma - hata sözlüğü
description: Azure HDInsight kümeleri oluştururken oluşan hataları nasıl gidereceklerini öğrenin
author: karkrish
ms.author: v-todmc
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/19/2019
ms.openlocfilehash: 803783eddfbffd5c3dbab7353ee00dd7f11a09e5
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618896"
---
# <a name="azure-hdinsight-cluster-creation-errors"></a>Azure HDInsight: Küme oluşturma hataları

Bu makalede, kümeler oluştururken karşılaşabileceğiniz hataların çözümleri açıklanmaktadır.

> [!NOTE]
> Bu makalede açıklanan ilk üç hata doğrulama hatalarıdır. Bunlar, bir Azure HDInsight ürünü **CsmDocument_2_0** sınıfı kullandığında oluşabilir.

## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>Hata kodu: DeploymentDocument 'CsmDocument_2_0' doğrulama başarısız

### <a name="error"></a>Hata

"Script Action konumuna URI:\<SCRIPT\>ACTION URL'si erişilemez "

#### <a name="error-message"></a>Hata iletisi

"Uzak sunucu bir hata döndürülür: (404) Bulunamadı."

### <a name="cause"></a>Nedeni

HDInsight hizmeti, Cluster Oluştur isteğinin bir parçası olarak sağladığınız komut dosyası eylem URL'sine erişemez. Hizmet, komut dosyası eylemine erişmeye çalıştığında önceki hata iletisini alır.

### <a name="resolution"></a>Çözüm

- Bir HTTP veya HTTPS URL'si için gizli bir tarayıcı penceresinden url'ye gitmeye çalışarak URL'yi doğrulayın.
- WASB URL'si için, komut dosyasının istekte verdiğiniz depolama hesabında bulunduğundan emin olun. Ayrıca, bu depolama hesabının depolama anahtarının doğru olduğundan emin olun.
- ADLS URL'si için komut dosyasının depolama hesabında bulunduğundan emin olun.

---

## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>Hata kodu: DeploymentDocument 'CsmDocument_2_0' doğrulama başarısız

### <a name="error"></a>Hata

"Script Action konumuURI \<erişilemez:\>SCRIPT_ACTION_URL "

#### <a name="error-message"></a>Hata iletisi

"Verilen komut \<uri\> SCRIPT_URI ADLS, ancak bu küme hiçbir veri gölü depolama ilkesi vardır"

### <a name="cause"></a>Nedeni

HDInsight hizmeti, Cluster Oluştur isteğinin bir parçası olarak sağladığınız komut dosyası eylem URL'sine erişemez. Hizmet, komut dosyası eylemine erişmeye çalıştığında önceki hata iletisini alır.

### <a name="resolution"></a>Çözüm

Kümeye ilgili Azure Veri Gölü Depolama Gen 1 hesabını ekleyin. Ayrıca, Veri Gölü Depolama Gen 1 hesabına erişen hizmet ilkesini kümeye ekleyin.

---

## <a name="error-code-deploymentdocument-csmdocument_2_0-failed-the-validation"></a>Hata kodu: DeploymentDocument 'CsmDocument_2_0' doğrulama başarısız

### <a name="error"></a>Hata

"VM boyutu\<\>' CUSTOMER_SPECIFIED_VM_SIZE ' isteği nde sağlanan geçersiz veya\<\>rol için desteklenmeyen ' ROL '. Geçerli değerler \<şunlardır: VALID_VM_SIZE_FOR_ROLE."\>

### <a name="cause"></a>Nedeni

Belirttiğiniz sanal makine boyutu, rol için izin verilmez. VM boyutu değeri beklendiği gibi çalışmadığından veya bilgisayar rolü için uygun olmadığından bu hata oluşabilir.

### <a name="resolution"></a>Çözüm

Hata iletisi VM boyutu için geçerli değerleri listeler. Bu değerlerden birini seçin ve Cluster oluştur isteğini yeniden deneyin.

---

## <a name="error-codeinvalidvirtualnetworkid"></a>Hata kodu: GeçersizVirtualNetworkId  

### <a name="error"></a>Hata

"VirtualNetworkId geçerli değildir. VirtualNetworkId\<'\>USER_VIRTUALNETWORKID '*"

### <a name="cause"></a>Nedeni

Küme oluşturma sırasında belirttiğiniz **VirtualNetworkId** değeri doğru biçimde değildir.

### <a name="resolution"></a>Çözüm

**VirtualNetworkId** ve alt ağ değerlerinin doğru biçimde olduğundan emin olun. **VirtualNetworkId** değerini almak için:

1. Azure portalına gidin.
1. Sanal ağınızı seçin.
1. **Özellikler** menü öğesini seçin. **ResourceID** özellik değeri **VirtualNetworkId** değeridir.

Aşağıda sanal ağ kimliği örneği verilmiştir:

"/abonelikler/c15fd9b8-e2b8-1d4e-aa85-2e668040233b/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/myvnet"

---

## <a name="error-code-customizationfailederrorcode"></a>Hata kodu: ÖzelleştirmeFailedErrorCode

### <a name="error"></a>Hata

"Küme dağıtımı, özel komut dosyası eylemindeki bir hata nedeniyle başarısız oldu. Başarısız \<Eylemler:\>SCRIPT_NAME , Lütfen ambari ui daha fazla hata ayıklamak için gidin."

### <a name="cause"></a>Nedeni

Küme Oluştur isteği sırasında sağladığınız özel komut dosyası, küme başarıyla dağıtıldıktan sonra yürütülür. Bu hata kodu, SCRIPT_NAME \<\>adlı özel komut dosyasının yürütülmesi sırasında bir hata ortaya çıktığını gösterir.

### <a name="resolution"></a>Çözüm

Komut dosyası özel komut dosyanız olduğundan, sorunu gidermenizi ve gerekirse komut dosyasını yeniden çalıştırmanızı öneririz. Komut dosyası hatasını gidermek için /var/lib/ambari-agent/* klasöründeki günlükleri inceleyin. Veya Ambari UI'deki **İşlemler** sayfasını açın ve ardından hata ayrıntılarını görüntülemek için **run_customscriptaction** işlemini seçin.

---

## <a name="error-codeinvaliddocumenterrorcode"></a>Hata kodu: GeçersizDocumentErrorCode

### <a name="error"></a>Hata

"META_STORE_TYPE \<\> Metastore şema \<sürümü\> \<veritabanında\> METASTORE_MAJOR_VERSION DATABASE_NAME küme \<\>sürümü ile uyumsuz CLUSTER_VERSION "

### <a name="cause"></a>Nedeni

Özel metastore, seçili HDInsight küme sürümüyle uyumsuzdur. Şu anda, HDInsight 4.0 kümeleri yalnızca Metastore sürüm 3.0 ve sonraki sürümlerini desteklerken, HDInsight 3.6 kümeleri Metastore sürüm 3.0 ve sonraki sürümlerini desteklemez.

### <a name="resolution"></a>Çözüm

Yalnızca HDInsight küme sürümünüzün desteklediği Metastore sürümlerini kullanın. Özel bir metastore belirtmezseniz, HDInsight dahili olarak bir metastore oluşturur ve küme silme işleminden sonra siler.

---

## <a name="error-code-failedtoconnectwithclustererrorcode"></a>Hata kodu: FailedToConnectWithClusterErrorCode 

### <a name="error"></a>Hata

"Ölçekleme işlemi gerçekleştirmek için küme yönetimi bitiş noktasına bağlanılamıyor. Ağ güvenliği kurallarının kümeye dış erişimi engellemediğini ve küme yöneticisi (Ambari) UI'ye başarıyla erişilebileni doğrulayın."

### <a name="cause"></a>Nedeni

Ağ güvenlik grubunuzdaki (NSG) bir güvenlik duvarı kuralı, kritik Azure sağlık ve yönetim hizmetleriyle küme iletişimini engelliyor.

### <a name="resolution"></a>Çözüm

Ağ trafiğini denetlemek için ağ güvenlik gruplarını kullanmayı planlıyorsanız, HDInsight'ı yüklemeden önce aşağıdaki işlemleri yapmayı planlayın:

- HDInsight için kullanmayı planladığınız Azure bölgesini tanımlayın.
- HDInsight tarafından gerekli IP adreslerini tanımlayın. Daha fazla bilgi için bkz. [HDInsight yönetimi IP adresleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses).
  - HDInsight'ı yüklemeyi planladığınız alt ağ için ağ güvenlik gruplarını oluşturun veya değiştirin.
  - Ağ güvenlik grupları için IP adreslerinden bağlantı noktası 443'te gelen trafiğe izin verin. Bu yapılandırma, HDInsight yönetim hizmetlerinin kümeye sanal ağ dışından ulaşmasını sağlar.

---

## <a name="error-code-storagepermissionsblockedformsi"></a>Hata kodu: StoragePermissionsBlockedForMsi

### <a name="error"></a>Hata

"Yönetilen Kimlik depolama hesabında izinleri yok. Lütfen depolama hesabı için Yönetilen Kimlik'e 'Depolama Blob Veri Sahibi' rolünün atandığını doğrulayın. Depolama: \</abonelikler/\> Abonelik Kimliği\< /kaynakGruplar/ Kaynak Grup Adı\> /sağlayıcıları/Microsoft.Storage/storageAccounts/ \<Storage Hesap Adı\>, Yönetilen Kimlik: /abonelikler/ \<Abonelik Kimliği\> /kaynakGrupları/ / Kaynak\< Grup Adı\> /sağlayıcılar/Microsoft.ManagedIdentity/userAssignedIdentities/ \<User Managed Identity Name\>"

### <a name="cause"></a>Nedeni

Kimliği yönetmek için gereken izinleri sağlamadın. Kullanıcı tarafından atanan yönetilen kimlik, Azure Veri Gölü Depolama Gen2 depolama hesabında Blob Depolama Katılımcısı rolüne sahip değildir.

### <a name="resolution"></a>Çözüm

1. Azure portalı açın.
1. Depolama hesabınıza gidin.
1. Erişim **Denetimi (IAM)** altına bakın.
1. Kullanıcının Depolama Blob Veri Katılımcısı rolüne veya kendilerine atanan Depolama Blob Veri Sahibi rolüne sahip olduğundan emin olun.

Daha fazla bilgi için [bkz.](hdinsight-hadoop-use-data-lake-storage-gen2.md)

---

## <a name="error-code-invalidnetworksecuritygroupsecurityrules"></a>Hata kodu: GeçersizAğSecurityGroupSecurityRules

### <a name="error"></a>Hata

"Ağ Güvenlik\<Grubu/abonelikler/ SubscriptionID\>/resourceGroups/<Kaynak Grubu\> adı varsayılan/sağlayıcılar/Microsoft.Network/networkSecurityGroups/\<Ağ Güvenliği Grup Adı\> \<alt\>ağ /abonelikler/ SubscriptionID\</resourceGroups/ Kaynak Grup adı\> RG-westeurope-vnet-tomtom-default/providers/Microsoft.Network/virtualNetworks/\<Virtual Network Name/subnets/\>\<Subnet Name\> gerekli giriş ve/veya dış bağlantıya izin vermez. Daha fazla bilgi için lütfen [Azure HDInsight için sanal ağ planlayın](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment)veya desteğe başvurun."

### <a name="cause"></a>Nedeni

Ağ güvenlik grupları veya kullanıcı tanımlı rotalar (ÜDR) HDInsight kümenize gelen trafiği kontrol ediyorsa, kümenizin kritik Azure sağlık ve yönetim hizmetleriyle iletişim kurabileceğinden emin olun.

### <a name="resolution"></a>Çözüm

Ağ trafiğini denetlemek için ağ güvenlik gruplarını kullanmayı planlıyorsanız, HDInsight'ı yüklemeden önce aşağıdaki işlemleri yapmayı planlayın:

- HDInsight için kullanmayı planladığınız Azure bölgesini belirleyin ve bölgeniz için IP adreslerinin güvenli bir listesini oluşturun. Daha fazla bilgi için Bkz. [Sağlık ve yönetim hizmetleri: Belirli bölgeler.](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses#health-and-management-services-specific-regions)
- HDInsight'ın gerektirdiği IP adreslerini tanımlayın. Daha fazla bilgi için [HDInsight yönetimi IP adreslerine](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)bakın.
- HDInsight'ı yüklemeyi planladığınız alt ağ için ağ güvenlik gruplarını oluşturun veya değiştirin. Ağ güvenlik grupları için IP adreslerinden bağlantı noktası 443'te gelen trafiğe izin verin. Bu yapılandırma, HDInsight yönetim hizmetlerinin kümeye sanal ağ dışından ulaşmasını sağlar.

---

## <a name="error-code-cluster-setup-failed-to-install-components-on-one-or-more-hosts"></a>Hata kodu: Küme kurulumu bileşenleri bir veya daha fazla ana bilgisayara yüklemeyi başaramadı

### <a name="error"></a>Hata

"Küme kurulumu bileşenleri bir veya daha fazla ana bilgisayara yüklemeyi başaramadı. Lütfen isteğinizi yeniden deneyin."

### <a name="cause"></a>Nedeni 

Genellikle, geçici bir sorun veya Azure kesintisi olduğunda bu hata oluşturulur.

### <a name="resolution"></a>Çözüm

Küme dağıtımını etkileyebilecek azure kesintileri için [Azure durum](https://status.azure.com) sayfasını denetleyin. Kesinti yoksa küme dağıtımını yeniden deneyin.

---

## <a name="error-code-failedtoconnectwithclustererrorcode"></a>Hata Kodu: FailedtoConnectWithclusterErrorcode

### <a name="error"></a>Hata

Küme yönetimi bitiş noktasına bağlanamıyor. Lütfen daha sonra tekrar deneyin.

### <a name="cause"></a>Nedeni

HDInsight Hizmeti küme oluşturmaya çalışırken kümenize bağlanamaz

### <a name="resolution"></a>Çözüm

Özel VNet ağ güvenlik grubu (NSG' ler) ve kullanıcı tanımlı rotalar (ÜDS) kullanıyorsanız, kümenizin HDInsight yönetim hizmetleriyle iletişim kurabilmesini sağlayın. Daha fazla bilgi için [HDInsight yönetimi IP adreslerine](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)bakın.

---

## <a name="error-code-deployments-failed-due-to-policy-violation-resource-resource-uri-was-disallowed-by-policy-policy-identifiers-policyassignmentnamepolicy-name-idprovidersmicrosoftmanagementmanagementgroupsmanagement-group-name-providersmicrosoftauthorizationpolicyassignmentspolicy-namepolicydefinition-policy-definition"></a>Hata Kodu: İlke ihlali nedeniyle dağıtımlar<Resource URI>başarısız oldu: 'Kaynak' ilkesi tarafından izin verilmedi. İlke tanımlayıcıları: '[{"policyAssignment":{"name":"<Policy Name> ","id":"/providers/Microsoft.Management/managementGroups/<Management Group Name> providers/Microsoft.Authorization/policyAssignments/<Policy Name>"},"policyDefinition":<Policy Definition>

### <a name="cause"></a>Nedeni

Abonelik tabanlı Azure ilkeleri, genel IP adreslerinin oluşturulmasını reddedebilir. HDInsight kümesi oluşturmak için iki genel IP gerekir.

Aşağıdaki ilkeler genellikle küme oluşturmayı etkiler:

* Abonelik içinde IP Adresleri veya Yük bakiyeleri oluşturulmasını engelleyen ilkeler.
* Depolama hesapları oluşturmayı engelleyen ilke.
* IP Adresleri veya Yük Dengeleyicileri gibi ağ kaynaklarının silen ilkesi.

### <a name="resolution"></a>Çözüm

HDInsight Cluster oluştururken abonelik tabanlı Azure ilkesini silin veya devre dışı kaltın.

---

## <a name="next-steps"></a>Sonraki adımlar

Küme oluşturmadaki sorun giderme hataları hakkında daha fazla bilgi için [Azure HDInsight ile Sorun Giderme küme oluşturma hatalarına](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-troubleshoot-cluster-creation-fails)bakın.
