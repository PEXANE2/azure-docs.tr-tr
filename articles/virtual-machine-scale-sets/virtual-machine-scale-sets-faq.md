---
title: Azure sanal makine ölçek kümeleri hakkında SSS
description: Azure 'daki sanal makine ölçek kümeleri hakkında en sık sorulan soruların yanıtlarını alın.
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: faq
ms.date: 05/24/2019
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: a3074fdd10ef960a1c0b58b973d57da14d888af4
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83200157"
---
# <a name="azure-virtual-machine-scale-sets-faqs"></a>Azure sanal makine ölçek kümeleri hakkında SSS

Azure 'daki sanal makine ölçek kümeleri hakkında sık sorulan soruların yanıtlarını alın.

## <a name="top-frequently-asked-questions-for-scale-sets"></a>Ölçek Kümeleri için en sık sorulan sorular

### <a name="how-many-vms-can-i-have-in-a-scale-set"></a>Bir ölçek kümesinde kaç tane sanal makinem olabilir?

Ölçek kümesi, platform görüntülerini temel alan 0-1.000 VM veya özel görüntüleri temel alan 0-600 VM olabilir.

### <a name="are-data-disks-supported-within-scale-sets"></a>Ölçek kümelerinde veri diskleri destekleniyor mu?

Evet. Bir ölçek kümesi, kümedeki tüm sanal makineler için geçerli olan bağlı veri diski yapılandırmasını tanımlayabilir. Daha fazla bilgi için bkz. [Azure ölçek kümeleri ve bağlı veri diskleri](virtual-machine-scale-sets-attached-disks.md). Veri depolamayla ilgili diğer seçenekler şunlardır:

* Azure dosyaları (paylaşılan SMB sürücüleri)
* İşletim sistemi sürücüsü
* Geçici sürücü (yerel, Azure Depolama tarafından yedeklenmez)
* Azure veri hizmeti (örneğin Azure tabloları, Azure blobları)
* Dış veri hizmeti (örneğin, uzak veritabanı)

### <a name="which-azure-regions-support-scale-sets"></a>Hangi Azure bölgeleri ölçek kümelerini destekler?

Tüm bölgeler ölçek kümelerini destekler.

### <a name="how-do-i-create-a-scale-set-by-using-a-custom-image"></a>Özel bir görüntü kullanarak nasıl ölçek kümesi oluşturabilirim?

Bir VM görüntüsü oluşturup yakalayın, ardından bunu ölçek kümesi için kaynak olarak kullanın. Özel bir VM görüntüsü oluşturma ve kullanma hakkında öğreticide, [Azure CLI](tutorial-use-custom-image-cli.md) veya [Azure PowerShell](tutorial-use-custom-image-powershell.md) kullanabilirsiniz.

### <a name="if-i-reduce-my-scale-set-capacity-from-20-to-15-which-vms-are-removed"></a>Ölçek kümemin kapasitesini 20’den 15’e düşürürsem hangi VM’ler kaldırılır?

Varsayılan olarak, sanal makineler, kullanılabilirlik alanları genelinde (ölçek kümesi ZGen yapılandırmasında dağıtılmışsa) ve kullanılabilirliği en üst düzeye çıkarmak için hata etki alanları arasında eşit olarak ayarlanan ölçek kümesinden kaldırılır. En yüksek kimlik numarasına sahip VM’ler ilk önce kaldırılır.

Ölçek kümesi için bir [Ölçek genişletme İlkesi](virtual-machine-scale-sets-scale-in-policy.md) belirterek sanal makine kaldırma sırasını değiştirebilirsiniz.

### <a name="what-if-i-then-increase-the-capacity-from-15-to-18"></a>Kapasiteyi 15’ten 18’e yükseltirsem ne olur?

Kapasiteyi 18’e artırırsanız 3 yeni VM oluşturulur. Her defasında VM örnek kimliği önceki en yüksek değerden artırılır (örneğin 20, 21, 22). VM 'Ler hata etki alanları arasında dengelenir.

### <a name="when-im-using-multiple-extensions-in-a-scale-set-can-i-enforce-an-execution-sequence"></a>Bir ölçek kümesinde birden fazla uzantı kullanırken bir yürütme sırası uygulamayı zorunlu kılabilir miyim?

Evet, ölçek kümesi [uzantı sıralamasını](virtual-machine-scale-sets-extension-sequencing.md)kullanabilirsiniz.

### <a name="do-scale-sets-work-with-azure-availability-sets"></a>Ölçek kümeleri Azure kullanılabilirlik kümeleri ile birlikte çalışır mı?

Bölgesel (zikzak olmayan) ölçek kümesi, beş hata etki alanı ve beş güncelleştirme etki alanı ile örtük bir kullanılabilirlik kümesi görevi gören *yerleştirme gruplarını*kullanır. 100 ' den fazla VM 'nin ölçek kümesi birden çok yerleştirme grubuna yayılamaz. Yerleştirme grupları hakkında daha fazla bilgi için bkz. [Büyük sanal makine ölçek kümeleri ile çalışma](virtual-machine-scale-sets-placement-groups.md). Bir sanal makine kullanılabilirlik kümesi, sanal makine ölçek kümesiyle aynı sanal ağda bulunabilir. Genellikle bir kullanılabilirlik kümesinde benzersiz yapılandırma gerektiren denetim düğümünü sanal makinelere, veri düğümlerini ise ölçek kümesine yerleştirmek, yaygın bir yapılandırmadır.

### <a name="do-scale-sets-work-with-azure-availability-zones"></a>Ölçek Kümeleri Azure kullanılabilirlik bölgeleriyle birlikte çalışıyor mu?

Evet! Daha fazla bilgi için [Ölçek kümesi bölgesi belge](./virtual-machine-scale-sets-use-availability-zones.md)' ne bakın.


## <a name="autoscale"></a>Otomatik Ölçeklendirme

### <a name="what-are-best-practices-for-azure-autoscale"></a>Azure otomatik ölçeklendirme için en iyi uygulamalar nelerdir?

Otomatik ölçeklendirme için en iyi uygulamalar için bkz. [sanal makineleri otomatik ölçeklendirme Için en iyi uygulamalar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a>Ana bilgisayar tabanlı ölçümler kullanan otomatik ölçeklendirme için ölçüm adlarını nerede bulabilirim?

Ana bilgisayar tabanlı ölçümler kullanan otomatik ölçeklendirme için ölçüm adları için bkz. [Azure izleyici Ile desteklenen ölçümler](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a>Azure Service Bus konuya ve sıra uzunluğuna göre otomatik ölçeklendirme örnekleri var mı?

Evet. Azure Service Bus bir konuya ve sıra uzunluğuna göre otomatik ölçeklendirme örnekleri için bkz. [Azure izleyici otomatik ölçeklendirme ortak ölçümleri](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).

Service Bus kuyruğu için aşağıdaki JSON 'u kullanın:

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

Bir depolama kuyruğu için aşağıdaki JSON 'u kullanın:

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

Örnek değerleri, kaynak Tekdüzen Kaynak tanımlayıcılarıyla (URI) değiştirin.


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a>Ana bilgisayar tabanlı ölçümler veya bir tanılama uzantısı kullanarak otomatik ölçeklendirme yapmam gerekir mi?

Konak düzeyinde ölçümler veya Konuk işletim sistemi tabanlı ölçümler kullanmak için bir sanal makinede otomatik ölçeklendirme ayarı oluşturabilirsiniz.

Desteklenen ölçümlerin listesi için bkz. [Azure izleyici otomatik ölçeklendirme ortak ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics).

Sanal Makine Ölçek Kümeleri için tam bir örnek için bkz. [sanal makine ölçek kümeleri için Kaynak Yöneticisi şablonları kullanarak gelişmiş otomatik ölçeklendirme yapılandırması](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets).

Örnek, ana bilgisayar düzeyi CPU ölçümünü ve bir ileti sayısı ölçümünü kullanır.



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a>Nasıl yaparım?, bir sanal makine ölçek kümesinde uyarı kuralları mi ayarlanacak?

PowerShell veya Azure CLı ile sanal makine ölçek kümeleri için ölçümler üzerinde uyarılar oluşturabilirsiniz. Daha fazla bilgi için bkz. [Azure Izleyici PowerShell hızlı başlangıç örnekleri](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) ve [Azure IZLEYICI platformlar arası CLI hızlı başlangıç örnekleri](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).

Sanal makine ölçek kümesinin Targetresourceıd 'i şöyle görünür:

/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmssname

Bir uyarı ayarlanacak ölçüm olarak herhangi bir VM performans sayacını seçebilirsiniz. Daha fazla bilgi için bkz. [Azure izleyici otomatik ölçeklendirme genel ölçümler](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/) makalesindeki [Kaynak Yöneticisi tabanlı Windows VM 'LERI için konuk işletim sistemi ölçümleri](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) ve [Linux VM 'leri için konuk işletim sistemi](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) ölçümleri.

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a>PowerShell 'i kullanarak bir sanal makine ölçek kümesindeki otomatik ölçeklendirmeyi ayarlamak Nasıl yaparım?.

PowerShell kullanarak bir sanal makine ölçek kümesi üzerinde otomatik ölçeklendirmeyi ayarlamak için bkz. [sanal makine ölçek kümesini otomatik olarak ölçeklendirme](tutorial-autoscale-powershell.md). Otomatik ölçeklendirmeyi [Azure CLI](tutorial-autoscale-cli.md) ve [Azure şablonları](tutorial-autoscale-template.md) ile de yapılandırabilirsiniz


### <a name="if-i-have-stopped-deallocated-a-vm-is-that-vm-started-as-part-of-an-autoscale-operation"></a>Bir VM 'yi durdurdum (serbest bırakıldım), sanal makine otomatik ölçeklendirme işleminin bir parçası olarak başlatılır mi?

Hayır. Otomatik ölçeklendirme kuralları, ölçek kümesinin bir parçası olarak ek VM örnekleri gerektiriyorsa, yeni bir VM örneği oluşturulur. Durdurulan (serbest bırakıldı) VM örnekleri, otomatik ölçeklendirme olayının bir parçası olarak başlatılmaz. Ancak, bu durdurulan (serbest bırakılmış) VM 'Ler, örnek sayısında ölçeklendirilen bir otomatik ölçeklendirme olayının parçası olarak silinebilir, ancak VM örnek KIMLIĞI sırasına göre herhangi bir sanal makine örneğinin silinebileceği gibi.



## <a name="certificates"></a>Sertifikalar

### <a name="how-do-i-securely-ship-a-certificate-to-the-vm"></a>Nasıl yaparım? VM 'ye güvenli bir şekilde bir sertifika sevk edilsin mi?

SANAL makineye güvenli bir şekilde sertifika göndermek için müşterinin anahtar kasasından bir Windows sertifika deposuna doğrudan bir müşteri sertifikası yükleyebilirsiniz.

Aşağıdaki JSON 'ı kullanın:

```json
"secrets": [
    {
        "sourceVault": {
            "id": "/subscriptions/{subscriptionid}/resourceGroups/myrg1/providers/Microsoft.KeyVault/vaults/mykeyvault1"
        },
        "vaultCertificates": [
            {
                "certificateUrl": "https://mykeyvault1.vault.azure.net/secrets/{secretname}/{secret-version}",
                "certificateStore": "certificateStoreName"
            }
        ]
    }
]
```

Kod, Windows ve Linux 'u destekler.

Daha fazla bilgi için bkz. [sanal makine ölçek kümesi oluşturma veya güncelleştirme](https://msdn.microsoft.com/library/mt589035.aspx).


### <a name="how-do-i-use-self-signed-certificates-provisioned-for-azure-service-fabric-clusters"></a>Nasıl yaparım? Azure Service Fabric kümeleri için sağlanan otomatik olarak imzalanan sertifikaları kullanmak mı istiyorsunuz?
En son örnek için Azure kabuğu 'nda aşağıdaki Azure CLı ifadesini kullanın, Service dokuları CLı modülü örnek belgelerini okuyun ve STDOUT 'a yazdırılır:

```azurecli
az sf cluster create -h
```

Otomatik olarak imzalanan sertifikalar, bir sertifika yetkilisi tarafından sağlanmış olan dağıtılan güven için kullanılamaz ve kurumsal üretim çözümlerini barındırmak için tasarlanan tüm Service Fabric kümeleri için kullanılmamalıdır. Ek Service Fabric güvenlik kılavuzu için [Azure Service Fabric En Iyi güvenlik uygulamalarını](https://docs.microsoft.com/azure/security/fundamentals/service-fabric-best-practices) inceleyin ve [küme güvenliği senaryolarını Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).

### <a name="can-i-specify-an-ssh-key-pair-to-use-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a>Kaynak Yöneticisi şablonundan Linux sanal makine ölçek kümesi ile SSH kimlik doğrulaması için kullanılacak SSH anahtar çiftini belirtebilir miyim?

Evet. **Osprofile** için REST API standart VM REST API benzerdir.

Şablonunuza **Osprofile** ekleyin:

```json
"osProfile": {
    "computerName": "[variables('vmName')]",
    "adminUsername": "[parameters('adminUserName')]",
    "linuxConfiguration": {
        "disablePasswordAuthentication": "true",
        "ssh": {
            "publicKeys": [
                {
                    "path": "[variables('sshKeyPath')]",
                    "keyData": "[parameters('sshKeyData')]"
                }
            ]
        }
    }
}
```

Bu JSON bloğu [Bu Azure hızlı başlangıç şablonunda](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json)kullanılır.

Daha fazla bilgi için bkz. [sanal makine ölçek kümesi oluşturma veya güncelleştirme](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).

### <a name="how-do-i-remove-deprecated-certificates"></a>Kullanımdan kaldırılan sertifikaları kaldırmak Nasıl yaparım??

Kullanımdan kaldırılan sertifikaları kaldırmak için kasa sertifikaları listesinden eski sertifikayı kaldırın. Bilgisayarınızda kalmasını istediğiniz tüm sertifikaları listede bırakın. Bu, sertifikayı tüm sanal makinelerinizden kaldırmaz. Ayrıca sertifikayı, sanal makine ölçek kümesinde oluşturulan yeni VM 'lere eklemez.

Sertifikayı mevcut VM 'lerden kaldırmak için, sertifika deponuzdan sertifikaları el ile kaldırmak için özel bir betik uzantısı kullanın.

### <a name="how-do-i-inject-an-existing-ssh-public-key-into-the-virtual-machine-scale-set-ssh-layer-during-provisioning"></a>Nasıl yaparım? sağlama sırasında sanal makine ölçek kümesi SSH katmanına var olan bir SSH ortak anahtarı eklemek istiyor musunuz?

VM 'Leri yalnızca ortak bir SSH anahtarıyla sağladıysanız, ortak anahtarları Key Vault koymanız gerekmez. Ortak anahtarlar gizli değil.

Linux VM oluştururken düz metin olarak SSH ortak anahtarlarını sağlayabilirsiniz:

```json
"linuxConfiguration": {
    "ssh": {
        "publicKeys": [
            {
                "path": "path",
                "keyData": "publickey"
            }
        ]
    }
}
```

linuxConfiguration öğe adı | Gerekli | Tür | Açıklama
--- | --- | --- | ---
SSH | Hayır | Koleksiyon | Linux işletim sistemi için SSH anahtarı yapılandırmasını belirtir
yol | Yes | Dize | SSH anahtarlarının veya sertifikasının bulunması gereken Linux dosya yolunu belirtir
keyData | Yes | Dize | Base64 ile kodlanmış SSH ortak anahtarını belirtir

Bir örnek için bkz. [101-VM-sshkey GitHub hızlı başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).

### <a name="when-i-run-update-azvmss-after-adding-more-than-one-certificate-from-the-same-key-vault-i-see-the-following-message"></a>`Update-AzVmss`Aynı anahtar kasasından birden fazla sertifika ekledikten sonra çalıştırdığımda şu iletiyi görüyorum:

>Update-AzVmss: List Secret, izin verilmeyen/Subscriptions/ \< My-Subscription-id>/ResourceGroups/internal-RG-dev/Providers/Microsoft.keyvault/Vaults/internal-keyvault-dev yinelenen örneklerini içeriyor.

Bu durum, mevcut kaynak Kasası için yeni bir kasa sertifikası kullanmak yerine aynı kasayı yeniden eklemeye çalışırsanız meydana gelebilir. `Add-AzVmssSecret`Ek gizli dizileri ekliyorsanız Komut düzgün çalışmaz.

Aynı anahtar kasasından daha fazla gizli dizi eklemek için $vmss. Properties. osProfile. gizlilikler [0]. vaultCertificates listesini güncelleştirin.

Beklenen giriş yapısı için bkz. [sanal makine kümesi oluşturma veya güncelleştirme](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Anahtar kasasındaki sanal makine ölçek kümesi nesnesindeki gizli anahtarı bulun. Daha sonra, sertifika başvurunuz (URL ve gizli depo adı) kasayla ilişkili listeye ekleyin.

> [!NOTE]
> Şu anda sanal makine ölçek kümesi API 'sini kullanarak VM 'lerden sertifika kaldıramazsınız.
>

Yeni VM 'Ler eski sertifikaya sahip olmayacaktır. Ancak, sertifikaya sahip ve zaten dağıtılmış olan VM 'Ler eski sertifikaya sahip olur.

### <a name="can-i-push-certificates-to-the-virtual-machine-scale-set-without-providing-the-password-when-the-certificate-is-in-the-secret-store"></a>Sertifika, parola olmadan sanal makine ölçek kümesine, sertifika gizli depoda olduğunda sertifika gönderebilir miyim?

Betiklerdeki parolalara sabit kod eklemeniz gerekmez. Dağıtım betiğini çalıştırmak için kullandığınız izinlerle parolaları dinamik olarak alabilirsiniz. Gizli depolama anahtarı kasasından bir sertifikayı taşınan bir betiğe sahipseniz, gizli `get certificate` dizi komutu da. pfx dosyasının parolasını çıkarır.

### <a name="how-does-the-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-the-sourcevault-value-when-i-have-to-specify-the-absolute-uri-for-a-certificate-by-using-the-certificateurl-property"></a>VirtualMachineProfile. osProfile 'in bir sanal makine ölçek kümesi için gizli dizi özelliği nasıl çalışır? CertificateUrl özelliğini kullanarak bir sertifika için mutlak URI 'yi belirtmem gerektiğinde Sourcekasa değerine neden ihtiyacım var?

İşletim sistemi profilinin gizlilikler özelliğinde bir Windows Uzaktan Yönetimi (WinRM) Sertifika başvurusu bulunmalıdır.

Kaynak kasasının belirtilme amacı, kullanıcının Azure bulut hizmeti modelinde bulunan erişim denetim listesi (ACL) ilkelerini zorlayasağlamaktır. Kaynak Kasası belirtilmemişse, bir anahtar kasasında gizli dizi dağıtmak veya bu kaynaklara erişmek için izinleri olmayan kullanıcılar, bir Işlem kaynak sağlayıcısı (CRP) aracılığıyla yapılabilir. Mevcut olmayan kaynaklar için de ACL 'Ler mevcuttur.

Yanlış bir kaynak Kasası KIMLIĞI, ancak geçerli bir Anahtar Kasası URL 'SI sağlarsanız, işlemi yokladığınızda bir hata bildirilir.

### <a name="if-i-add-secrets-to-an-existing-virtual-machine-scale-set-are-the-secrets-injected-into-existing-vms-or-only-into-new-ones"></a>Var olan bir sanal makine ölçek kümesine gizli dizileri eksem, var olan VM 'Lere eklenen gizlilikler mı yoksa yalnızca yenilerini mi var?

Sertifikalar tüm sanal makinelerinize, hatta önceden mevcut olanlara eklenir. Sanal makine ölçek kümesi upgradePolicy özelliği **el ile**olarak AYARLANDıYSA, VM 'de el ile güncelleştirme GERÇEKLEŞTIRDIĞINIZDE sertifika VM 'ye eklenir.

### <a name="where-do-i-put-certificates-for-linux-vms"></a>Linux VM 'Leri için sertifika yerleştiririm?

Linux VM 'Ler için sertifika dağıtmayı öğrenmek için bkz. [müşteri tarafından yönetilen anahtar kasasından VM 'lere sertifika dağıtma](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).

### <a name="how-do-i-add-a-new-vault-certificate-to-a-new-certificate-object"></a>Yeni bir sertifika nesnesine yeni bir kasa sertifikası Nasıl yaparım? eklensin mi?

Var olan bir parolaya kasa sertifikası eklemek için aşağıdaki PowerShell örneğine bakın. Yalnızca bir gizli nesne kullanın.

```powershell
$newVaultCertificate = New-AzVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809

$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)

Update-AzVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```

### <a name="what-happens-to-certificates-if-you-reimage-a-vm"></a>Bir VM 'yi yeniden görüntütemediğinde sertifikalara ne olur?

Bir VM 'yi yeniden görüntüederseniz sertifikalar silinir. Yeniden görüntüleme, tüm işletim sistemi diskini siler.

### <a name="what-happens-if-you-delete-a-certificate-from-the-key-vault"></a>Anahtar kasasından bir sertifikayı silerseniz ne olur?

Gizli anahtar kasasından silinirse, `stop deallocate` tüm sanal makinelerinizden çalıştırın ve sonra yeniden başlatırsanız bir hatayla karşılaşırsınız. Bu hata, CRP 'nin anahtar kasasından gizli dizileri alması gerektiğinden, ancak başarısız olur. Bu senaryoda, sertifikaları sanal makine ölçek kümesi modelinden silebilirsiniz.

CRP bileşeni, müşteri gizli dizilerini kalıcı tutmaz. `stop deallocate`Sanal makine ölçek kümesindeki tüm VM 'ler için çalıştırırsanız, önbellek silinir. Bu senaryoda, gizli dizileri anahtar kasasından alınır.

Azure Service Fabric (tek doku kiracı modelinde) bir parolanın önbelleğe alınmış bir kopyası olduğundan, ölçeklendirilirken bu sorunla karşılaşmazsınız.

### <a name="why-do-i-have-to-specify-the-certificate-version-when-i-use-key-vault"></a>Key Vault kullanırken neden sertifika sürümünü belirtmem gerekir?

Sertifika sürümünü belirtmek için Key Vault gereksiniminin amacı, sanal makinelerinde hangi sertifikanın dağıtıldığı kullanıcıya açık hale getirmek olur.

Bir VM oluşturup gizli anahtarı anahtar kasasında güncelleştirirseniz yeni sertifika, sanal makinelerinize indirilmez. Ancak sanal makinelerinize başvuru olarak görünür ve yeni VM 'Ler yeni gizli dizi alır. Bunu önlemek için, bir gizli sürüme başvurmanız gerekir.

### <a name="my-team-works-with-several-certificates-that-are-distributed-to-us-as-cer-public-keys-what-is-the-recommended-approach-for-deploying-these-certificates-to-a-virtual-machine-scale-set"></a>Ekibim, bize. cer ortak anahtarları olarak dağıtılan çeşitli sertifikalarla birlikte çalışmaktadır. Bu sertifikaları bir sanal makine ölçek kümesine dağıtmak için önerilen yaklaşım nedir?

. Cer ortak anahtarlarını bir sanal makine ölçek kümesine dağıtmak için, yalnızca. cer dosyalarını içeren bir. pfx dosyası oluşturabilirsiniz. Bunu yapmak için kullanın `X509ContentType = Pfx` . Örneğin,. cer dosyasını C# veya PowerShell 'de bir x509Certificate2 nesnesi olarak yükleyin ve sonra yöntemi çağırın.

Daha fazla bilgi için bkz [. X509Certificate. Export yöntemi (X509ContentType, String)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).

### <a name="how-do-i-pass-in-certificates-as-base64-strings"></a>Sertifikalar Nasıl yaparım? Base64 dizeleri olarak mı?

Bir sertifikadaki bir Sertifikayı Base64 dizesi olarak öykünmek için Kaynak Yöneticisi şablonda en son sürümlü URL 'YI ayıklayabilirsiniz. Aşağıdaki JSON özelliğini Kaynak Yöneticisi şablonunuza ekleyin:

```json
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```

### <a name="do-i-have-to-wrap-certificates-in-json-objects-in-key-vaults"></a>Sertifikaları anahtar kasalarındaki JSON nesnelerinde sarması gerekiyor mu?

Sanal Makine Ölçek Kümeleri ve VM 'lerde, sertifikaların JSON nesnelerinde sarmalanması gerekir.

Ayrıca application/x-PKCS12 içerik türünü de destekliyoruz.

Şu anda. cer dosyalarını desteklemiyoruz. . Cer dosyalarını kullanmak için bunları. pfx kapsayıcılarına dışarı aktarın.



## <a name="compliance-and-security"></a>Uyumluluk ve güvenlik

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a>Sanal Makine Ölçek Kümeleri PCI uyumlu mı?

Sanal makine ölçek kümeleri, CRP’nin üzerinde ince bir API katmanıdır. Her iki bileşen de, Azure hizmet ağacındaki işlem platformunun birer parçasıdır.

Uyumluluk açısından bakıldığında, sanal makine ölçek kümeleri Azure işlem platformunun temel parçalarından birini oluşturur. CRP’nin kendisiyle takımı, araçları, süreçleri, dağıtım yöntemini, güvenlik denetimlerini, tam zamanında (JIT) derlemeyi, izlemeyi, uyarıları, vb. paylaşır. Sanal makine ölçek kümeleri Ödeme Kartı Sektörü (PCI) ile uyumludur, çünkü CRP geçerli PCI Veri Güvenliği Standardı (DSS) kanıtının bir parçasıdır.

Daha fazla bilgi için bkz. [Microsoft Güven Merkezi](https://www.microsoft.com/TrustCenter/Compliance/PCI).

### <a name="does-managed-identities-for-azure-resources-work-with-virtual-machine-scale-sets"></a>[Azure kaynakları için Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/msi-overview) sanal makine ölçek kümeleriyle çalışır mı?

Evet. [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-msi) ve [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-msi)için Azure hızlı başlangıç şablonlarında bazı örnek MSI şablonlarına bakabilirsiniz.

## <a name="deleting"></a>Silinmesinden

### <a name="will-the-locks-i-set-in-place-on-virtual-machine-scale-set-instances-be-respected-when-deleting-instances"></a>Örnek silinirken, sanal makine ölçek kümesi örneklerine göre ayarlanan kilitler dikkate alınır mi?

Azure portalında, tek bir örneği veya toplu silme özelliğini birden çok örnek seçerek silebilirsiniz. Bir kilidi olan tek bir örneği silmeye çalışırsanız kilit dikkate alınır ve örneği silemezsiniz demektir. Ancak, birden çok örneği toplu olarak seçerseniz ve bu örneklerden herhangi birinde bir kilit varsa, kilitler kullanılamaz ve seçilen örneklerin hepsi silinir.

Azure CLı 'de yalnızca tek bir örneği silebilirsiniz. Bir kilidi olan tek bir örneği silmeye çalışırsanız kilit dikkate alınır ve bu örneği silemezsiniz.

## <a name="extensions"></a>Uzantıları

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a>Nasıl yaparım? bir sanal makine ölçek kümesi uzantısı silinsin mi?

Bir sanal makine ölçek kümesi uzantısını silmek için aşağıdaki PowerShell örneğini kullanın:

```powershell
$vmss = Get-AzVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName"

$vmss=Remove-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```

İçinde extensionName değerini bulabilirsiniz `$vmss` .

### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-azure-monitor-logs"></a>Azure Izleyici günlükleri ile tümleşen bir sanal makine ölçek kümesi şablonu örneği var mı?

Azure Izleyici günlükleri ile tümleşen bir sanal makine ölçek kümesi şablonu örneği için Azure [Service Fabric kümesi dağıtma ve Azure izleyici günlüklerini kullanarak izlemeyi etkinleştirme](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric)bölümündeki ikinci örneğe bakın.

### <a name="how-do-i-add-an-extension-to-all-vms-in-my-virtual-machine-scale-set"></a>Nasıl yaparım? sanal makine ölçek kümesindeki tüm VM 'lere uzantı eklemek mi istiyorsunuz?

Güncelleştirme ilkesi **Otomatik**olarak ayarlandıysa, şablonu yeni uzantı özellikleriyle yeniden dağıtmak tüm VM 'leri günceller.

Güncelleştirme ilkesi **el ile**olarak ayarlandıysa, önce uzantıyı güncelleştirin ve ardından sanal makinelerinizdeki tüm örnekleri el ile güncelleştirin.

### <a name="if-the-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected"></a>Var olan bir sanal makine ölçek kümesiyle ilişkili uzantılar güncelleştirilirse, var olan VM 'Ler etkilenir mi?

Sanal makine ölçek kümesi modelindeki uzantı tanımı güncellendiyse ve upgradePolicy özelliği **Otomatik**olarak ayarlandıysa, VM 'leri güncelleştirir. UpgradePolicy özelliği **el ile**olarak ayarlandıysa, uzantılar modelle eşleşmiyor olarak işaretlenir.

### <a name="are-extensions-run-again-when-an-existing-machine-is-service-healed-or-reimaged"></a>Mevcut bir makine Service-cenled veya yeniden görüntüsü olduğunda uzantılar yeniden çalıştırılır mi?

Var olan bir VM Service-cenled ise, yeniden önyükleme olarak görünür ve uzantılar yeniden çalıştırılmaz. Bir VM yeniden yansıma içeriyorsa, işlem işletim sistemi sürücüsünü kaynak görüntüyle değiştirme benzerdir. Uzantılar gibi en son modelden herhangi bir özelleştirme yeniden çalıştırılır.

### <a name="how-do-i-join-a-virtual-machine-scale-set-to-an-active-directory-domain"></a>Nasıl yaparım? bir sanal makine ölçek kümesine Active Directory etki alanına katılması mı istiyorsunuz?

Bir sanal makine ölçek kümesini Active Directory (AD) etki alanına katmak için bir uzantı tanımlayabilirsiniz.

Bir uzantı tanımlamak için, JsonADDomainExtension özelliğini kullanın:

```json
"extensionProfile": {
    "extensions": [
        {
            "name": "joindomain",
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "JsonADDomainExtension",
                "typeHandlerVersion": "1.3",
                "settings": {
                    "Name": "[parameters('domainName')]",
                    "OUPath": "[variables('ouPath')]",
                    "User": "[variables('domainAndUsername')]",
                    "Restart": "true",
                    "Options": "[variables('domainJoinOptions')]"
                },
                "protectedsettings": {
                    "Password": "[parameters('domainJoinPassword')]"
                }
            }
        }
    ]
}
```

### <a name="my-virtual-machine-scale-set-extension-is-trying-to-install-something-that-requires-a-reboot"></a>Sanal makine ölçek kümesi uzantım, yeniden başlatma gerektiren bir şeyi yüklemeye çalışıyor.

Sanal makine ölçek kümesi uzantınız, yeniden başlatma gerektiren bir şeyi yüklemeye çalışıyorsa, Azure Otomasyonu Istenen durum yapılandırması (Automation DSC) uzantısını kullanabilirsiniz. İşletim sistemi Windows Server 2012 R2 ise, Azure Windows Management Framework (WMF) 5,0 Kurulum programını çeker ve yeniden başlatır ve yapılandırma ile devam eder.

### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi 'ndeki kötü amaçlı yazılımdan koruma Nasıl yaparım? açılsın mı?

Sanal makine ölçek kümesinde kötü amaçlı yazılımdan koruma açmak için aşağıdaki PowerShell örneğini kullanın:

```powershell
$rgname = 'autolap'
$vmssname = 'autolapbr'
$location = 'eastus'

# Retrieve the most recent version number of the extension.
$allVersions= (Get-AzVMExtensionImage -Location $location -PublisherName "Microsoft.Azure.Security" -Type "IaaSAntimalware").Version
$versionString = $allVersions[($allVersions.count)-1].Split(".")[0] + "." + $allVersions[($allVersions.count)-1].Split(".")[1]

$VMSS = Get-AzVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
echo $VMSS
Add-AzVmssExtension -VirtualMachineScaleSet $VMSS -Name "IaaSAntimalware" -Publisher "Microsoft.Azure.Security" -Type "IaaSAntimalware" -TypeHandlerVersion $versionString
Update-AzVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $VMSS
```

### <a name="how-do-i-execute-a-custom-script-thats-hosted-in-a-private-storage-account"></a>Özel bir depolama hesabında barındırılan özel bir betik Nasıl yaparım? mi çalışıyor?

Özel bir depolama hesabında barındırılan özel bir betiği yürütmek için, korumalı ayarları depolama hesabı anahtarı ve adıyla ayarlayın. Daha fazla bilgi için bkz. [Özel Betik uzantısı](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).

## <a name="passwords"></a>Parolalar

### <a name="how-do-i-reset-the-password-for-vms-in-my-virtual-machine-scale-set"></a>Sanal makine ölçek kümesindeki VM 'Lerin parolasını sıfırlamak Nasıl yaparım? mı?

Ölçek kümelerinde VM 'Lerin parolasını değiştirmek için iki ana yol vardır.

- Sanal makine ölçek kümesi modelini doğrudan değiştirin. API 2017-12-01 ve üzeri sürümlerde kullanılabilir.

    Yönetici kimlik bilgilerini doğrudan ölçek kümesi modelinde güncelleştirin (örneğin, Azure Kaynak Gezgini, PowerShell veya CLı kullanarak). Ölçek kümesi güncelleştirildikten sonra tüm yeni VM 'Ler yeni kimlik bilgilerine sahiptir. Mevcut VM 'Ler, yalnızca yeniden yansıma varsa yeni kimlik bilgilerine sahiptir.

- VM erişim uzantılarını kullanarak parolayı sıfırlayın. [Burada](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm)açıklandığı gibi parola gereksinimlerini izlediğinizden emin olun.

    Aşağıdaki PowerShell örneğini kullanın:

    ```powershell
    $vmssName = "myvmss"
    $vmssResourceGroup = "myvmssrg"
    $publicConfig = @{"UserName" = "newuser"}
    $privateConfig = @{"Password" = "********"}

    $extName = "VMAccessAgent"
    $publisher = "Microsoft.Compute"
    $vmss = Get-AzVmss -ResourceGroupName $vmssResourceGroup -VMScaleSetName $vmssName
    $vmss = Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion "2.0" -AutoUpgradeMinorVersion $true
    Update-AzVmss -ResourceGroupName $vmssResourceGroup -Name $vmssName -VirtualMachineScaleSet $vmss
    ```

## <a name="networking"></a>Ağ

### <a name="is-it-possible-to-assign-a-network-security-group-nsg-to-a-scale-set-so-that-it-applies-to-all-the-vm-nics-in-the-set"></a>Bir ölçek kümesine ağ güvenlik grubu (NSG) atamak mümkün olduğundan, küme içindeki tüm VM NIC 'Leri için geçerli olacak mı?

Evet. Ağ güvenlik grubu, ağ profilinin Networkınterfaceconfigurations bölümüne başvurarak bir ölçek kümesine doğrudan uygulanabilir. Örnek:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            },
                            "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-the-same-subscription-and-same-region"></a>Aynı abonelikte ve aynı bölgede yer alan sanal makine ölçek kümeleri için bir VIP takas Nasıl yaparım? mı?

Azure Load Balancer ön uçları olan iki sanal makine ölçek kümesine sahipseniz ve bunlar aynı abonelik ve bölgedeyse, genel IP adreslerini her birinden serbest bırakabilir ve diğerine atayabilirsiniz. Örneğin [Azure Resource Manager VIP takas: mavi-yeşil dağıtım '](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) a bakın. Bu, kaynakların ağ düzeyinde serbest bırakılmış/ayrılmış olması halinde bir gecikme anlamına gelmez. Daha hızlı bir seçenek, Azure Application Gateway iki arka uç havuzlarıyla ve bir yönlendirme kuralıyla kullanmaktır. Alternatif olarak, hazırlama ve üretim yuvaları arasında hızlı geçiş desteği sağlayan [Azure App Service](https://azure.microsoft.com/services/app-service/) ile uygulamanızı barındırabilirsiniz.

### <a name="how-do-i-specify-a-range-of-private-ip-addresses-to-use-for-static-private-ip-address-allocation"></a>Nasıl yaparım? statik özel IP adresi ayırması için kullanılacak özel IP adresi aralığını belirtin.

IP adresleri belirttiğiniz bir alt ağdan seçilir.

Sanal makine ölçek kümesi IP adreslerinin ayırma yöntemi her zaman "dinamik" dır, ancak bu IP adreslerinin değiştirebileceği anlamına gelmez. Bu durumda, "dinamik" yalnızca bir PUT isteğinde IP adresi belirtmeyeceğiniz anlamına gelir. Alt ağı kullanarak statik kümeyi belirtin.

### <a name="how-do-i-deploy-a-virtual-machine-scale-set-to-an-existing-azure-virtual-network"></a>Nasıl yaparım? bir sanal makine ölçek kümesi mevcut bir Azure sanal ağına dağıtılır mi?

Bir sanal makine ölçek kümesini mevcut bir Azure sanal ağına dağıtmak için, bkz. [sanal makine ölçek kümesini mevcut bir sanal ağa dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet).

### <a name="can-i-use-scale-sets-with-accelerated-networking"></a>Daha fazla hızlandırılmış ağ ile ölçek kümeleri kullanabilir miyim?

Evet. Hızlandırılmış ağ kullanmak için, ölçek kümesinin Networkınterfaceconfigurations ayarlarında Enableivmeağı ' nı doğru olarak ayarlayın. Örneğin:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "niconfig1",
            "properties": {
                "primary": true,
                "enableAcceleratedNetworking" : true,
                "ipConfigurations": [
                ]
            }
        }
    ]
}
```

### <a name="how-can-i-configure-the-dns-servers-used-by-a-scale-set"></a>Ölçek kümesi tarafından kullanılan DNS sunucularını nasıl yapılandırabilirim?

Özel bir DNS yapılandırması olan bir sanal makine ölçek kümesi oluşturmak için, ölçek kümesi Networkınterfaceconfigurations bölümüne bir dnsSettings JSON paketi ekleyin. Örnek:

```json
    "dnsSettings":{
        "dnsServers":["10.0.0.6", "10.0.0.5"]
    }
```

### <a name="how-can-i-configure-a-scale-set-to-assign-a-public-ip-address-to-each-vm"></a>Bir ölçek kümesini her VM 'ye genel bir IP adresi atamak için nasıl yapılandırabilirim?

Her VM 'ye genel bir IP adresi atayan bir sanal makine ölçek kümesi oluşturmak için, Microsoft. COMPUTE/virtualMachineScaleSets kaynağının API sürümünün 2017-03-30 olduğundan emin olun ve bir _publicıpaddressconfiguration_ JSON paketini ölçek kümesi ipconfigurations bölümüne ekleyin. Örnek:

```json
    "publicipaddressconfiguration": {
        "name": "pub1",
        "properties": {
        "idleTimeoutInMinutes": 15
        }
    }
```

### <a name="can-i-configure-a-scale-set-to-work-with-multiple-application-gateways"></a>Ölçek kümesini birden çok uygulama ağ geçidi ile çalışacak şekilde yapılandırabilir miyim?

Evet. Birden çok Application Gateway arka uç adres havuzu için kaynak kimliklerini, ölçek kümesi ağ profilinizin _ıpconfigurations_ bölümündeki _Applicationgatewaybackendadddresspoir_ listesine ekleyebilirsiniz.

## <a name="scale"></a>Ölçek

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a>Ne durumda ikiden az VM 'ye sahip bir sanal makine ölçek kümesi oluşturmalıyım?

İkiden az VM 'ye sahip bir sanal makine ölçek kümesi oluşturmanın bir nedeni, bir sanal makine ölçek kümesinin elastik özelliklerini kullanmaktır. Örneğin, sanal makine ölçek kümesini, sanal makine çalışan maliyetler ödemeksizin altyapınızı tanımlamak için sıfır VM ile dağıtabilirsiniz. Ardından, VM 'Leri dağıtmaya hazırsanız, sanal makine ölçek kümesinin "kapasitesini" üretim örneği sayısı olarak artırın.

Farklı VM 'lere sahip bir kullanılabilirlik kümesi kullanmaktan daha az önem taşıyan bir sanal makine ölçek kümesi oluşturabilirsiniz. Sanal Makine Ölçek Kümeleri, uygun olmayan işlem birimleriyle çalışmak için bir yol sağlar. Bu birlik, sanal makine ölçek kümeleri için kullanılabilirlik kümelerine göre önemli bir farklılığa sahiptir. Çok sayıda durum bilgisiz iş yükü ayrı birimleri izlemez. İş yükü düşerse, bir işlem birimine kadar ölçeklendirebilir ve sonra iş yükü arttıkça çok daha fazla ölçeği azaltabilirsiniz.

### <a name="how-do-i-change-the-number-of-vms-in-a-virtual-machine-scale-set"></a>Nasıl yaparım? bir sanal makine ölçek kümesindeki VM 'lerin sayısını değiştirmek mi istiyorsunuz?

Sanal Makine Ölçek Kümesi Özellikleri bölümünde bir sanal makine ölçek Azure portal kümesindeki VM sayısını değiştirmek için, "ölçeklendirme" dikey penceresine tıklayın ve kaydırıcı çubuğunu kullanın.

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a>Nasıl yaparım? belirli eşiklere ulaşıldığında özel uyarılar tanımlansın mı?

Belirtilen eşiklere yönelik uyarıları nasıl işleyeceğinizi gösteren bir esneklik vardır. Örneğin, özelleştirilmiş Web kancaları tanımlayabilirsiniz. Aşağıdaki Web kancası örneği bir Kaynak Yöneticisi şablonundan verilmiştir:

```json
{
    "type": "Microsoft.Insights/autoscaleSettings",
    "apiVersion": "[variables('insightsApi')]",
    "name": "autoscale",
    "location": "[parameters('resourceLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
    ],
    "properties": {
        "name": "autoscale",
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
        "enabled": true,
        "notifications": [
            {
                "operation": "Scale",
                "email": {
                    "sendToSubscriptionAdministrator": true,
                    "sendToSubscriptionCoAdministrators": true,
                    "customEmails": [
                        "youremail@address.com"
                    ]
                },
                "webhooks": [
                    {
                        "serviceUri": "<service uri>",
                        "properties": {
                            "key1": "custommetric",
                            "key2": "scalevmss"
                        }
                    }
                ]
            }
        ]
    }
}
```


## <a name="patching-and-operations"></a>Düzeltme eki uygulama ve işlemler

### <a name="can-i-create-a-scale-set-in-an-existing-resource-group"></a>Var olan bir kaynak grubunda ölçek kümesi oluşturabilir miyim?

Evet, var olan bir kaynak grubunda ölçek kümesi oluşturabilirsiniz.

### <a name="can-i-move-a-scale-set-to-another-resource-group"></a>Ölçek kümesini başka bir kaynak grubuna taşıyabilir miyim?

Evet, ölçek kümesi kaynaklarını yeni bir aboneliğe veya kaynak grubuna taşıyabilirsiniz.

### <a name="how-to-i-update-my-virtual-machine-scale-set-to-a-new-image-how-do-i-manage-patching"></a>Sanal makine ölçek kümesini yeni bir görüntüye nasıl güncelleştirebilirim? Düzeltme Eki Yönetimi Nasıl yaparım? mi?

Sanal makine ölçek kümesini yeni bir görüntüye güncelleştirmek ve düzeltme eki yönetmek için bkz. [sanal makine ölçek kümesini yükseltme](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).

### <a name="can-i-use-the-reimage-operation-to-reset-a-vm-without-changing-the-image-that-is-i-want-reset-a-vm-to-factory-settings-rather-than-to-a-new-image"></a>Görüntüyü değiştirmeden bir VM 'yi sıfırlamak için yeniden görüntü işlemini kullanabilir miyim? (Yani, bir VM 'yi yeni bir görüntü yerine fabrika ayarlarına sıfırlıyorum.)

Evet, görüntüyü değiştirmeden bir VM 'yi sıfırlamak için ReImage işlemini kullanabilirsiniz. Ancak, sanal makine ölçek kümesi bir platform görüntüsüne ile başvuruyorsa `version = latest` , ÇAĞıRDıĞıNıZDA VM 'niz daha sonraki bir işletim sistemi görüntüsüne güncelleştirebilir `reimage` .

### <a name="is-it-possible-to-integrate-scale-sets-with-azure-monitor-logs"></a>Ölçek Kümeleri Azure Izleyici günlükleriyle tümleştirilebilecek mi?

Evet, Azure Izleyici uzantısını ölçek kümesi VM 'lerine yükleyebilirsiniz. Azure CLı örneği aşağıda verilmiştir:

```azurecli
az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group Team-03 --vmss-name nt01 --settings "{'workspaceId': '<your workspace ID here>'}" --protected-settings "{'workspaceKey': '<your workspace key here'}"
```

Gerekli çalışma alanı kimliği ve workspaceKey ' nin Azure portal Log Analytics çalışma alanında bulabilirsiniz. Genel Bakış sayfasında, ayarlar kutucuğuna tıklayın. Üstteki bağlı kaynaklar sekmesine tıklayın.

> [!NOTE]
> Ölçek kümesi _Upgradepolicy_ , manuel olarak ayarlandıysa, uzantıyı üzerinde yükseltme çağırarak küme Içindeki tüm VM 'lere uygulamanız gerekir. CLı içinde bu, _az VMSS Update-Instances_olacaktır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="troubleshooting"></a>Sorun giderme

### <a name="how-do-i-turn-on-boot-diagnostics"></a>Önyükleme tanılaması Nasıl yaparım? açılsın mı?

Önyükleme tanılamayı açmak için, önce bir depolama hesabı oluşturun. Ardından, bu JSON bloğunu sanal makine ölçek kümesi **Virtualmachineprofile**öğesine yerleştirin ve sanal makine ölçek kümesini güncelleştirin:

```json
"diagnosticsProfile": {
    "bootDiagnostics": {
        "enabled": true,
        "storageUri": "http://yourstorageaccount.blob.core.windows.net"
    }
}
```

Yeni bir VM oluşturulduğunda, sanal makinenin InstanceView özelliği, ekran görüntüsünün ayrıntılarını gösterir ve bu şekilde devam eder. Bir örneği aşağıda verilmiştir:

```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
}
```

## <a name="virtual-machine-properties"></a>Sanal makine özellikleri

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-the-fault-domain-for-each-of-the-100-vms-in-my-virtual-machine-scale-set"></a>Nasıl yaparım? birden çok çağrı yapmadan her VM için özellik bilgilerini almak mı istiyorsunuz? Örneğin, sanal makine ölçek kümesindeki 100 VM 'lerinin her biri için hata etki alanını nasıl alabilirim?

Birden çok çağrı yapmadan her VM için özellik bilgilerini almak için `ListVMInstanceViews` `GET` AŞAĞıDAKI Kaynak URI 'sinde bir REST API yapabilirsiniz:

/Subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.Compute/virtualMachineScaleSets/<scaleset_name>/virtualMachines? $expand = InstanceView&$select = InstanceView

### <a name="can-i-pass-different-extension-arguments-to-different-vms-in-a-virtual-machine-scale-set"></a>Farklı uzantı bağımsız değişkenlerini bir sanal makine ölçek kümesindeki farklı VM 'lere geçirebilir miyim?

Hayır, bir sanal makine ölçek kümesindeki farklı VM 'lere farklı uzantı bağımsız değişkenleri geçiremezsiniz. Ancak, uzantılar, makine adı gibi, üzerinde çalıştığı VM 'nin benzersiz özelliklerine göre davranabilir. Uzantılar http://169.254.169.254 VM hakkında daha fazla bilgi almak için örnek meta verilerini de sorgulayabilir.

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a>Sanal makine ölçek kümesi VM makine adları ve VM kimlikleri arasında neden boşluklar var? Örneğin: 0, 1, 3...

Sanal makine ölçek kümesi **fazla sağlama** özelliği varsayılan değer olan **true**değerine ayarlandığından, sanal makine ölçek kümesi VM makine adları ve VM kimlikleri arasında boşluklar vardır. Aşırı sağlama değeri **true**olarak ayarlandıysa, istenenden daha fazla sanal makine oluşturulur. Daha sonra ek VM 'Ler silinir. Bu durumda, büyük bir dağıtım güvenilirliği elde edersiniz, ancak ardışık adlandırma ve bitişik ağ adresi çevirisi (NAT) kuralları masrafına sahip olursunuz.

Bu özelliği **false**olarak ayarlayabilirsiniz. Küçük sanal makine ölçek kümeleri için bu, dağıtım güvenilirliğini önemli ölçüde etkilemez.

### <a name="what-is-the-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-the-vm-when-should-i-choose-one-over-the-other"></a>Sanal makine ölçek kümesindeki bir VM 'yi silme ve VM 'nin ayırmayı kaldırma arasındaki fark nedir? Ne zaman birini seçmem gerekir?

Sanal makine ölçek kümesindeki bir VM 'yi silme ve VM 'nin ayırmayı kaldırma arasındaki temel fark, `deallocate` sanal sabit diskleri (VHD 'ler) silmez. Çalıştırmaya ilişkin depolama maliyetleri var `stop deallocate` . Aşağıdaki nedenlerden biri için bir veya diğerini kullanabilirsiniz:

- İşlem maliyetleri ödemekten vazgeçmek istiyorsunuz, ancak VM 'lerin disk durumunu tutmak istiyorsunuz.
- Sanal makine ölçek kümesini ölçeklendirebilmeniz için bir VM kümesini daha hızlı başlatmak istiyorsunuz.
  - Bu senaryoyla ilgili olarak, kendi otomatik ölçeklendirme motorinizi oluşturmuş ve uçtan uca daha hızlı bir ölçek istemeniz olabilir.
- Hata etki alanları veya güncelleştirme etki alanları arasında eşit olarak dağıtılan bir sanal makine ölçek kümesi vardır. Bunun nedeni, sanal makineleri seçmeli olarak silmiş veya aşırı sağlama sonrasında VM 'Lerin silindiğinden olabilir. `stop deallocate` `start` Sanal makine ölçek kümesi tarafından izlenen çalıştırmak, VM 'leri hata etki alanları veya güncelleştirme etki alanları arasında eşit olarak dağıtır.

### <a name="how-do-i-take-a-snapshot-of-a-virtual-machine-scale-set-instance"></a>Bir sanal makine ölçek kümesi örneğinin anlık görüntüsünü almak Nasıl yaparım? mı?
Bir sanal makine ölçek kümesi örneğinden bir anlık görüntü oluşturun.

```azurepowershell-interactive
$rgname = "myResourceGroup"
$vmssname = "myVMScaleSet"
$Id = 0
$location = "East US"

$vmss1 = Get-AzVmssVM -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $Id     
$snapshotconfig = New-AzSnapshotConfig -Location $location -AccountType Standard_LRS -OsType Windows -CreateOption Copy -SourceUri $vmss1.StorageProfile.OsDisk.ManagedDisk.id
New-AzSnapshot -ResourceGroupName $rgname -SnapshotName 'mySnapshot' -Snapshot $snapshotconfig
```

Anlık görüntüden yönetilen disk oluşturun.

```azurepowershell-interactive
$snapshotName = "mySnapshot"
$snapshot = Get-AzSnapshot -ResourceGroupName $rgname -SnapshotName $snapshotName  
$diskConfig = New-AzDiskConfig -AccountType Premium_LRS -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
$osDisk = New-AzDisk -Disk $diskConfig -ResourceGroupName $rgname -DiskName ($snapshotName + '_Disk')
```
