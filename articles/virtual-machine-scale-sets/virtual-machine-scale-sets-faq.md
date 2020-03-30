---
title: Azure sanal makine ölçek kümeleri hakkında SSS
description: Azure'da sanal makine ölçek kümeleri hakkında en sık sorulan soruların yanıtlarını alın.
author: mayanknayar
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: manayar
ms.openlocfilehash: 74195e83e17140b67ac060e1791c580e90e720f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79534448"
---
# <a name="azure-virtual-machine-scale-sets-faqs"></a>Azure sanal makine ölçek kümeleri hakkında SSS

Azure'da sanal makine ölçek kümeleri hakkında sık sorulan soruların yanıtlarını alın.

## <a name="top-frequently-asked-questions-for-scale-sets"></a>Ölçek kümeleri için sık sorulan sorular

### <a name="how-many-vms-can-i-have-in-a-scale-set"></a>Bir ölçek kümesinde kaç tane sanal makinem olabilir?

Ölçek kümesinde platform görüntülerine dayalı 0 ila 1.000 VM veya özel görüntülere dayalı 0 ila 600 VM olabilir.

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

Bir VM görüntüsü oluşturun ve yakalayın, ardından bunu ölçek kümenizin kaynağı olarak kullanın. Özel bir VM görüntüsünü niçin oluşturup kullanacağınız la ilgili bir öğretici için [Azure CLI](tutorial-use-custom-image-cli.md) veya [Azure PowerShell'i](tutorial-use-custom-image-powershell.md) kullanabilirsiniz

### <a name="if-i-reduce-my-scale-set-capacity-from-20-to-15-which-vms-are-removed"></a>Ölçek kümemin kapasitesini 20’den 15’e düşürürsem hangi VM’ler kaldırılır?

Sanal makineler, ölçek kümesinden güncelleştirme etki alanları ve hata etki alanları arasında eşit olacak şekilde kaldırılır. En yüksek kimlik numarasına sahip VM’ler ilk önce kaldırılır.

### <a name="what-if-i-then-increase-the-capacity-from-15-to-18"></a>Kapasiteyi 15’ten 18’e yükseltirsem ne olur?

Kapasiteyi 18’e artırırsanız 3 yeni VM oluşturulur. Her defasında VM örnek kimliği önceki en yüksek değerden artırılır (örneğin 20, 21, 22). VM’ler hata etki alanlarında ve güncelleştirme etki alanlarında dengelenir.

### <a name="when-im-using-multiple-extensions-in-a-scale-set-can-i-enforce-an-execution-sequence"></a>Bir ölçek kümesinde birden fazla uzantı kullanırken bir yürütme sırası uygulamayı zorunlu kılabilir miyim?

Evet, ölçek kümesi [uzantısı sıralamasını](virtual-machine-scale-sets-extension-sequencing.md)kullanabilirsiniz.

### <a name="do-scale-sets-work-with-azure-availability-sets"></a>Ölçek kümeleri Azure kullanılabilirlik kümeleri ile birlikte çalışır mı?

Bölgesel (non-zonal) ölçek kümesi, beş hata etki alanı ve beş güncelleştirme etki alanı yla örtülü kullanılabilirlik kümesi olarak hareket eden *yerleşim gruplarını*kullanır. 100'den fazla VM'den oluşan ölçek kümeleri birden çok yerleşim grubuna yayılır. Yerleştirme grupları hakkında daha fazla bilgi için bkz. [Büyük sanal makine ölçek kümeleri ile çalışma](virtual-machine-scale-sets-placement-groups.md). Bir sanal makine kullanılabilirlik kümesi, sanal makine ölçek kümesiyle aynı sanal ağda bulunabilir. Genellikle bir kullanılabilirlik kümesinde benzersiz yapılandırma gerektiren denetim düğümünü sanal makinelere, veri düğümlerini ise ölçek kümesine yerleştirmek, yaygın bir yapılandırmadır.

### <a name="do-scale-sets-work-with-azure-availability-zones"></a>Ölçek kümeleri Azure kullanılabilirlik bölgeleriyle çalışıyor mu?

Evet! Daha fazla bilgi için [ölçek ayarlı bölge dokümanına](./virtual-machine-scale-sets-use-availability-zones.md)bakın.


## <a name="autoscale"></a>Otomatik Ölçeklendirme

### <a name="what-are-best-practices-for-azure-autoscale"></a>Azure Otomatik Ölçeklendirme için en iyi uygulamalar nelerdir?

Otomatik ölçeklendirme için en iyi uygulamalar için [sanal makineleri otomatikleştirmek için en iyi uygulamalara](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices)bakın.

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a>Ana bilgisayar tabanlı ölçümleri kullanan otomatik ölçekleme için metrik adları nerede bulabilirim?

Ana bilgisayar tabanlı ölçümleri kullanan otomatik ölçekleme için metrik adlar için Azure [Monitor ile Desteklenen ölçümlere](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/)bakın.

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a>Azure Hizmet Veri Yolu konusuna ve sıra uzunluğuna dayalı olarak otomatik ölçekleme örnekleri var mı?

Evet. Azure Hizmet Veri Yolu konusuna ve sıra uzunluğuna dayalı otomatik ölçekleme örnekleri için, [Azure Monitor'un ortak ölçümleri otomatik olarak otomatikleştirin'e](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/)bakın.

Servis Veri Servisi kuyruğu için aşağıdaki JSON'u kullanın:

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

Depolama sırası için aşağıdaki JSON'u kullanın:

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

Örnek değerleri kaynağınız Tek düzen kaynak tanımlayıcılarınızla (URI) değiştirin.


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a>Ana bilgisayar tabanlı ölçümler i veya tanılama uzantısı kullanarak otomatik ölçeklendirme yapmalı mıyım?

Ana bilgisayar düzeyindeölçümleri veya konuk işletim sistemi tabanlı ölçümleri kullanmak için VM'de otomatik ölçeklendirme ayarı oluşturabilirsiniz.

Desteklenen ölçümlerin listesi için Azure [Monitor'un ortak ölçümleri otomatik olarak otomatik olarak hesaplamabölümüne](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics)bakın.

Sanal makine ölçek kümeleri için tam bir örnek için, [sanal makine ölçek kümeleri için Kaynak Yöneticisi şablonlarını kullanarak Gelişmiş otomatik ölçek yapılandırması'na](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets)bakın.

Örnek, ana bilgisayar düzeyindeCPU metrik ve ileti sayısı metrik kullanır.



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesinde uyarı kurallarını nasıl ayarlırım?

PowerShell veya Azure CLI aracılığıyla sanal makine ölçek kümeleri için ölçümlerde uyarılar oluşturabilirsiniz. Daha fazla bilgi için [Azure Monitor PowerShell hızlı başlatma örneklerine](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) ve [Azure Monitor çapraz platform CLI quickstart örneklerine](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts)bakın.

Sanal makine ölçeği kümesinin TargetResourceId'i aşağıdaki gibi görünür:

/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmssname

Herhangi bir VM performans sayacını, uyarı ayarlamak için metrik olarak seçebilirsiniz. Daha fazla bilgi için, [Azure Monitor otomatik ölçekleme ortak ölçümleri](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/) makalesinde Kaynak Yöneticisi tabanlı [Windows VM'leri](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) ve [Linux VM'leri için Konuk İşletim ölçümleri için Konuk İşletme](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) Sistemi ölçümlerine bakın.

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a>PowerShell'i kullanarak sanal makine ölçeğinde otomatik ölçek ayarlamayı nasıl ayarlıyorum?

PowerShell'i kullanarak sanal makine ölçeğinde otomatik ölçek ayarlamak için, [sanal makine ölçeği kümesini otomatik olarak ölçeklendirin.](tutorial-autoscale-powershell.md) [Azure CLI](tutorial-autoscale-cli.md) ve [Azure şablonlarıyla](tutorial-autoscale-template.md) otomatik ölçeklendirmeyi de yapılandırabilirsiniz


### <a name="if-i-have-stopped-deallocated-a-vm-is-that-vm-started-as-part-of-an-autoscale-operation"></a>Bir VM'yi durdurduysam (ayırdıysanız), bu VM otomatik ölçeklendirme işleminin bir parçası olarak mı başladı?

Hayır. Otomatik ölçek kuralları, ölçek kümesinin bir parçası olarak ek VM örnekleri gerektiriyorsa, yeni bir VM örneği oluşturulur. Durdurulan (ayrılan) VM örnekleri otomatik ölçeklendirme olayının bir parçası olarak başlatılamaz. Ancak, durdurulan (ayrılan) VM'ler, herhangi bir VM örneğinin VM örnek kimliği sırasına göre silinebileceği şekilde, örnek sayısını ölçekleyen bir otomatik ölçekolayının parçası olarak silinebilir.



## <a name="certificates"></a>Sertifikalar

### <a name="how-do-i-securely-ship-a-certificate-to-the-vm"></a>VM'ye nasıl güvenli bir sertifika gönderim?

Bir sertifikayı VM'ye güvenli bir şekilde sevk etmek için, müşterinin anahtar kasasından doğrudan bir Windows sertifika mağazasına müşteri sertifikası yükleyebilirsiniz.

Aşağıdaki JSON kullanın:

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

Kod, Windows ve Linux'u destekler.

Daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/mt589035.aspx)


### <a name="how-do-i-use-self-signed-certificates-provisioned-for-azure-service-fabric-clusters"></a>Azure Hizmet Kumaş Kümeleri için sağlanan kendi imzalı sertifikaları nasıl kullanırım?
En son örnek için azure kabuk içinde aşağıdaki azure CLI deyimini kullanın, stdout için yazdırılacak Hizmet Kumaşları CLI modülü Örnek belgeleri okuyun:

```azurecli
az sf cluster create -h
```

Kendi imzalı sertifikalar, Sertifika Yetkilisi tarafından sağlanan dağıtılmış güven için kullanılamaz ve kurumsal üretim çözümleri barındırmayı amaçlayan herhangi bir Hizmet Kumaş Kümesi için kullanılmamalıdır; ek Hizmet Kumaş Güvenliği kılavuzu için [Azure Hizmet Kumaş Güvenliği En İyi Uygulamaları](https://docs.microsoft.com/azure/security/fundamentals/service-fabric-best-practices) ve Hizmet Kumaş ı küme güvenlik senaryolarını gözden [geçirin.](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)

### <a name="can-i-specify-an-ssh-key-pair-to-use-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a>Kaynak Yöneticisi şablonundan ayarlanmış bir Linux sanal makine ölçeğiyle SSH kimlik doğrulaması için kullanılacak bir SSH anahtar çifti belirtebilir miyim?

Evet. **OSProfile** için REST API standart VM REST API benzer.

Şablonunuza **osProfile** ekleyin:

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

Bu JSON bloğu [bu Azure hızlı başlatma şablonunda](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json)kullanılır.

Daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration)

### <a name="how-do-i-remove-deprecated-certificates"></a>Amortismana uyacağım sertifikaları nasıl kaldıracağım?

Amortismana alınan sertifikaları kaldırmak için eski sertifikayı kasa sertifikaları listesinden çıkarın. Bilgisayarınızda kalmasını istediğiniz tüm sertifikaları listede bırakın. Bu, sertifikayı tüm VM'lerinizden kaldırmaz. Ayrıca, sanal makine ölçeği kümesinde oluşturulan yeni VM'lere sertifika eklemez.

Sertifikayı varolan VM'lerden kaldırmak için, sertifikaları sertifika deposunuzdan el ile kaldırmak için özel bir komut dosyası uzantısı kullanın.

### <a name="how-do-i-inject-an-existing-ssh-public-key-into-the-virtual-machine-scale-set-ssh-layer-during-provisioning"></a>Sağlama sırasında varolan bir SSH ortak anahtarısanal makine ölçeği kümesi SSH katmanına nasıl enjekte edebilirim?

VM'lere yalnızca ortak bir SSH anahtarı sağlıyorsanız, ortak anahtarları Anahtar Kasası'na koymanız gerekmez. Ortak anahtarlar gizli değildir.

Linux VM oluştururken SSH ortak anahtarlarını düz metin olarak sağlayabilirsiniz:

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

linuxConfiguration öğesi adı | Gerekli | Tür | Açıklama
--- | --- | --- | ---
Ssh | Hayır | Koleksiyon | Linux işletim sistemi için SSH anahtar yapılandırmasını belirtir
yol | Evet | Dize | SSH anahtarlarının veya sertifikasının bulunması gereken Linux dosya yolunu belirtir
Keydata | Evet | Dize | Base64 kodlanmış bir SSH ortak anahtarı belirtir

Örneğin, [101-vm-sshkey GitHub quickstart şablonuna](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json)bakın.

### <a name="when-i-run-update-azvmss-after-adding-more-than-one-certificate-from-the-same-key-vault-i-see-the-following-message"></a>Aynı anahtar `Update-AzVmss` kasasından birden fazla sertifika ekledikten sonra çalıştırdığımda aşağıdaki iletiyi görürsünüz:

>Güncelleme-AzVmss: Liste gizli /subscriptions/\<my-subscription-id>/resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, hangi izin verilmez tekrarlanan örnekleri içerir.

Bu, varolan kaynak kasası için yeni bir kasa sertifikası kullanmak yerine aynı kasayı yeniden eklemeye çalışırsanız gerçekleşebilir. Ek `Add-AzVmssSecret` sırlar ekliyorsanız komut düzgün çalışmaz.

Aynı anahtar kasasından daha fazla sır eklemek için $vmss.properties.osProfile.secrets[0].vaultCertificates listesini güncelleyin.

Beklenen giriş yapısı için [sanal makine kümesi oluştur veya güncelleştir'e](https://msdn.microsoft.com/library/azure/mt589035.aspx)bakın.

Anahtar kasasında ki sanal makine ölçeği kümesi nesnesinde sırrı bulun. Ardından, kasayla ilişkili listeye sertifika başvurunuzu (URL ve gizli mağaza adı) ekleyin.

> [!NOTE]
> Şu anda, sanal makine ölçeği kümesi API'yi kullanarak VM'lerden sertifikaları kaldıramazsınız.
>

Yeni VM'ler eski sertifikaya sahip olmaz. Ancak, sertifikaya sahip olan ve zaten dağıtılan VM'ler eski sertifikaya sahip olur.

### <a name="can-i-push-certificates-to-the-virtual-machine-scale-set-without-providing-the-password-when-the-certificate-is-in-the-secret-store"></a>Sertifika gizli mağazadayken, parola sağlamadan sertifikaları sanal makine ölçeğine itebilir miyim?

Komut dosyalarında parolaları sabit kodlamanız gerekmez. Dağıtım komut dosyasını çalıştırmak için kullandığınız izinlerle parolaları dinamik olarak alabilirsiniz. Gizli mağaza anahtar kasasından sertifika hareket ettiren bir komut `get certificate` dosyanız varsa, gizli mağaza komutu da .pfx dosyasının parolasını çıkar.

### <a name="how-does-the-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-the-sourcevault-value-when-i-have-to-specify-the-absolute-uri-for-a-certificate-by-using-the-certificateurl-property"></a>Sanal makine ölçeği için virtualMachineProfile.osProfile'ın Secrets özelliği nasıl çalışır? SertifikaUrl özelliğini kullanarak sertifika için mutlak URI'yi belirtmem gerektiğinde neden sourceVault değerine ihtiyacım var?

İşletim sistemi profilinin Sırlar özelliğinde bir Windows Uzaktan Yönetimi (WinRM) sertifika başvurusu bulunmalıdır.

Kaynak kasasını belirtmenin amacı, bir kullanıcının Azure Bulut Hizmeti modelinde bulunan erişim denetim listesi (ACL) ilkelerini zorlamaktır. Kaynak kasası belirtilmemişse, önemli bir kasaya bilgi verme veya sırlara erişme izni olmayan kullanıcılar, Bir Bilgi İşlem Kaynak Sağlayıcısı (CRP) aracılığıyla bunu yapabilir. ALA'lar var olmayan kaynaklar için bile vardır.

Yanlış kaynak kasa kimliği ancak geçerli bir anahtar kasa URL'si sağlarsanız, işlemi yoklarken bir hata bildirilir.

### <a name="if-i-add-secrets-to-an-existing-virtual-machine-scale-set-are-the-secrets-injected-into-existing-vms-or-only-into-new-ones"></a>Varolan sanal makine ölçeği kümesine sırlar eklersem, sırlar varolan VM'lere mi enjekte edilir, yoksa yalnızca yenilerine mi enjekte edilir?

Sertifikalar tüm VM'lerinize eklenir, hatta önceden var olanları. Sanal makine ölçeği ayaryükseltmeSiPolitika özelliği **manuel**olarak ayarlanmışsa, VM'de el ile güncelleştirme yaptığınızda sertifika VM'ye eklenir.

### <a name="where-do-i-put-certificates-for-linux-vms"></a>Linux VM'ler için sertifikaları nereye koyacağım?

Linux VM'leri için sertifikaları nasıl dağıtılayınız öğrenmek için, [müşteri tarafından yönetilen bir anahtar kasasından sertifikaları VM'lere dağıt'a](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/)bakın.

### <a name="how-do-i-add-a-new-vault-certificate-to-a-new-certificate-object"></a>Yeni bir sertifika nesnesine yeni bir kasa sertifikasını nasıl ekleyebilirim?

Varolan bir gizliye kasa sertifikası eklemek için aşağıdaki PowerShell örneğine bakın. Yalnızca bir gizli nesne kullanın.

```powershell
$newVaultCertificate = New-AzVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809

$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)

Update-AzVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```

### <a name="what-happens-to-certificates-if-you-reimage-a-vm"></a>Bir VM'yi yeniden görüntülerseniz sertifikalara ne olur?

Bir VM'yi yeniden görüntülerseniz, sertifikalar silinir. Reimaging tüm işletim sistemi diskini siler.

### <a name="what-happens-if-you-delete-a-certificate-from-the-key-vault"></a>Anahtarı kasasından bir sertifika yısa ne olur?

Eğer gizli anahtar kasasından silinir ve `stop deallocate` sonra tüm VM'leriniz için çalıştırır ve sonra yeniden başlatırsanız, bir başarısızlıkla karşılaşırsınız. CrP anahtar kasasından sırları almak gerekir, çünkü başarısızlık oluşur, ancak olamaz. Bu senaryoda, sanal makine ölçeği kümesi modelinden sertifikaları silebilirsiniz.

CRP bileşeni müşteri sırlarını devam etmez. Sanal makine `stop deallocate` ölçeği kümesindeki tüm VM'ler için çalıştırırsanız, önbellek silinir. Bu senaryoda, sırlar anahtar kasasından alınır.

Azure Hizmet Kumaşı'nda (tek kumaşlı kiracı modelinde) sırrın önbelleğe alınmış bir kopyası olduğundan, ölçekleme yaparken bu sorunla karşılaşmazsınız.

### <a name="why-do-i-have-to-specify-the-certificate-version-when-i-use-key-vault"></a>Key Vault'u kullanırken neden sertifika sürümünü belirtmem gerekiyor?

Sertifika sürümünü belirtmek için Anahtar Kasa gereksiniminin amacı, kullanıcıya VM'lerinde hangi sertifikanın dağıtılan sertifikaolduğunu açıkça belirtmektir.

Bir VM oluşturur ve sonra anahtar kasasında sırrınızı güncellerseniz, yeni sertifika VM'lerinize indirilmez. Ancak VM'leriniz referans olarak görünür ve yeni VM'ler yeni sırrı alır. Bunu önlemek için gizli bir sürüme başvurmanız gerekir.

### <a name="my-team-works-with-several-certificates-that-are-distributed-to-us-as-cer-public-keys-what-is-the-recommended-approach-for-deploying-these-certificates-to-a-virtual-machine-scale-set"></a>Ekibim bize .cer ortak anahtarları olarak dağıtılan çeşitli sertifikalarla çalışır. Bu sertifikaları sanal makine ölçeği kümesine dağıtmak için önerilen yaklaşım nedir?

.cer ortak anahtarlarını sanal makine ölçeği kümesine dağıtmak için yalnızca .cer dosyaları içeren bir .pfx dosyası oluşturabilirsiniz. Bunu yapmak için, kullanın. `X509ContentType = Pfx` Örneğin, .cer dosyasını C# veya PowerShell'de x509Certificate2 nesnesi olarak yükleyin ve ardından yöntemi arayın.

Daha fazla bilgi için bkz: [X509Certificate.Export Method (X509ContentType, String)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).

### <a name="how-do-i-pass-in-certificates-as-base64-strings"></a>Sertifikalarda base64 dizeleri olarak nasıl geçerim?

Bir sertifikada base64 dizesi olarak geçen taklit etmek için, Kaynak Yöneticisi şablonundaki en son sürümURL'yi ayıklayabilirsiniz. Kaynak Yöneticisi şablonunuzda aşağıdaki JSON özelliğini ekleyin:

```json
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```

### <a name="do-i-have-to-wrap-certificates-in-json-objects-in-key-vaults"></a>Anahtar kasalarında JSON nesnelerinde sertifika sarmak zorunda mıyım?

Sanal makine ölçek kümelerinde ve Sanal Ayarlar'da, sertifikalar JSON nesnelerine sarılmalıdır.

Ayrıca içerik türü uygulamasını/x-pkcs12'yi de destekliyoruz.

Şu anda .cer dosyalarını destekliyoruz. .cer dosyalarını kullanmak için bunları .pfx kapsayıcılarına aktarın.



## <a name="compliance-and-security"></a>Uyumluluk ve Güvenlik

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a>Sanal makine ölçek setleri PCI uyumlu mu?

Sanal makine ölçek kümeleri, CRP’nin üzerinde ince bir API katmanıdır. Her iki bileşen de, Azure hizmet ağacındaki işlem platformunun birer parçasıdır.

Uyumluluk açısından bakıldığında, sanal makine ölçek kümeleri Azure işlem platformunun temel parçalarından birini oluşturur. CRP’nin kendisiyle takımı, araçları, süreçleri, dağıtım yöntemini, güvenlik denetimlerini, tam zamanında (JIT) derlemeyi, izlemeyi, uyarıları, vb. paylaşır. Sanal makine ölçek kümeleri Ödeme Kartı Sektörü (PCI) ile uyumludur, çünkü CRP geçerli PCI Veri Güvenliği Standardı (DSS) kanıtının bir parçasıdır.

Daha fazla bilgi için [Microsoft Güven Merkezi'ne](https://www.microsoft.com/TrustCenter/Compliance/PCI)bakın.

### <a name="does-managed-identities-for-azure-resources-work-with-virtual-machine-scale-sets"></a>[Azure kaynakları için yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/msi-overview) sanal makine ölçeği kümeleriyle çalışır mı?

Evet. [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-msi) ve [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-msi)için Azure Quickstart şablonlarında bazı örnek MSI şablonlarını görebilirsiniz.

## <a name="deleting"></a>Silme 

### <a name="will-the-locks-i-set-in-place-on-virtual-machine-scale-set-instances-be-respected-when-deleting-instances"></a>Sanal makine ölçeğinde ayarlı kilitler örnekleri silerken saygı duyulacak mı?

Azure Portalı'nda, birden çok örnek seçerek tek bir örneği silme veya toplu silme özelliğine sahipsiniz. Kilit bulunan tek bir örneği silmeye çalışırsanız, kilit lere saygı duyulur ve örneği silemezsiniz. Ancak, birden çok örneği toplu olarak seçerseniz ve bu örneklerden herhangi birinde kilit varsa, kilit(ler) saygı duymaz ve seçili örneklerin tümü silinir. 
 
Azure CLI'de yalnızca tek bir örneği silme olanağınız var. Kilit bulunan tek bir örneği silmeye çalışırsanız, kilit lere saygı duyulur ve bu örneği silemezsiniz. 

## <a name="extensions"></a>Uzantılar

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a>Sanal makine ölçeği kümesi uzantısını nasıl silerim?

Sanal makine ölçeği kümesi uzantısını silmek için aşağıdaki PowerShell örneğini kullanın:

```powershell
$vmss = Get-AzVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName"

$vmss=Remove-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```

UzantıName değerini `$vmss`.

### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-azure-monitor-logs"></a>Azure Monitor günlükleriyle tümleşen sanal makine ölçeği şablon örneği var mı?

Azure Monitor günlükleriyle tümleşen sanal makine ölçeği şablonu örneği için, Azure Hizmet Dokusu kümesini dağıt'taki ikinci örneğe bakın [ve Azure Monitor günlüklerini kullanarak izlemeyi etkinleştirin.](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric)

### <a name="how-do-i-add-an-extension-to-all-vms-in-my-virtual-machine-scale-set"></a>Sanal makine ölçeği kümemdeki tüm VM'lere nasıl uzantı ekleyebilirim?

Güncelleştirme ilkesi **otomatik**olarak ayarlanmışsa, şablonu yeni uzantı özellikleriyle yeniden dağıtarak tüm VM'ler güncellenir.

Güncelleştirme ilkesi el **ile**ayarlanmışsa, önce uzantıyı güncelleştirin ve ardından VM'lerinizdeki tüm örnekleri el ile güncelleştirin.

### <a name="if-the-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected"></a>Varolan sanal makine ölçeği kümesiyle ilişkili uzantılar güncelleştirilirse, varolan VM'ler etkilenir mi?

Sanal makine ölçeği kümesi modelindeki uzantı tanımı güncelleştirilirse ve upgradePolicy özelliği **otomatik**olarak ayarlanırsa, VM'leri güncelleştirir. upgradePolicy özelliği **el ile**ayarlanmışsa, uzantıları modelle eşleşmeyen olarak işaretlenir.

### <a name="are-extensions-run-again-when-an-existing-machine-is-service-healed-or-reimaged"></a>Varolan bir makine servis ekidiğinde veya yeniden görüntülendiğinde uzantılar yeniden çalıştırılır mı?

Varolan bir VM hizmet iyileştirilmişse, yeniden başlatma olarak görünür ve uzantılar yeniden çalıştırılmaz. Bir VM yeniden görüntülenirse, işlem işletim sistemi sürücüsünün kaynak görüntüyle değiştirilmesine benzer. Uzantılar gibi en son modelden herhangi bir uzmanlık yeniden çalıştırılır.

### <a name="how-do-i-join-a-virtual-machine-scale-set-to-an-active-directory-domain"></a>Active Directory etki alanına ayarlanmış sanal makine ölçeğine nasıl katılabilirim?

Active Directory (AD) etki alanına ayarlanan sanal makine ölçeğine katılmak için bir uzantı tanımlayabilirsiniz.

Bir uzantı tanımlamak için JsonADDomainExtension özelliğini kullanın:

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

### <a name="my-virtual-machine-scale-set-extension-is-trying-to-install-something-that-requires-a-reboot"></a>Benim sanal makine ölçek kümesi uzantısı yeniden başlatma gerektiren bir şey yüklemek için çalışıyor.

Sanal makine ölçeği ayarlı uzantınız yeniden başlatma gerektiren bir şey yüklemeye çalışıyorsa, Azure Otomasyonu İstenen Durum Yapılandırması (Automation DSC) uzantısını kullanabilirsiniz. İşletim sistemi Windows Server 2012 R2 ise, Azure Windows Yönetim Çerçevesi (WMF) 5.0 kurulumu çeker, yeniden başlatır ve yapılandırmayla devam eder.

### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a>Sanal makine ölçeği kümemde kötü amaçlı yazılımdan korumayı nasıl açarım?

Sanal makine ölçeği setinizde kötü amaçlı yazılımdan korumayı açmak için aşağıdaki PowerShell örneğini kullanın:

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

### <a name="how-do-i-execute-a-custom-script-thats-hosted-in-a-private-storage-account"></a>Özel bir depolama hesabında barındırılan özel bir komut dosyasini nasıl çalıştırıyorum?

Özel bir depolama hesabında barındırılan özel bir komut dosyası yürütmek için, depolama hesabı anahtarı ve adı ile korumalı ayarlar ayarlayın. Daha fazla bilgi için [Bkz. Özel Komut Dosyası Uzantısı.](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings)

## <a name="passwords"></a>Parolalar

### <a name="how-do-i-reset-the-password-for-vms-in-my-virtual-machine-scale-set"></a>Sanal makine ölçeği kümemdeki VM'lerin parolasını nasıl sıfırlarım?

Ölçek kümelerinde VM'lerin parolasını değiştirmenin iki ana yolu vardır.

- Sanal makine ölçeği kümesi modelini doğrudan değiştirin. API 2017-12-01 ve sonrası ile kullanılabilir.

    Yönetici kimlik bilgilerini doğrudan ölçek kümesi modelinde (örneğin Azure Kaynak Gezgini, PowerShell veya CLI'yi kullanarak) güncelleştirin. Ölçek kümesi güncelleştirildikten sonra, tüm yeni VM'ler yeni kimlik bilgilerine sahip olabilir. Varolan VM'ler yalnızca yeniden görüntülenirlerse yeni kimlik bilgilerine sahiptir.

- VM erişim uzantılarını kullanarak parolayı sıfırla.

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

## <a name="networking"></a>Ağ Oluşturma

### <a name="is-it-possible-to-assign-a-network-security-group-nsg-to-a-scale-set-so-that-it-applies-to-all-the-vm-nics-in-the-set"></a>Bir Ağ Güvenlik Grubu'nu (NSG) kümedeki tüm VM NIC'leri için geçerli olacak şekilde bir ölçek kümesine atamak mümkün mü?

Evet. Ağ Güvenlik Grubu, ağ profilinin ağInterfaceConfigurations bölümünde atıfta bulunarak doğrudan bir ölçek kümesine uygulanabilir. Örnek:

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

### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-the-same-subscription-and-same-region"></a>Aynı abonelikte ve aynı bölgede sanal makine ölçek setleri için VIP takasını nasıl yaparım?

Azure Yük Bakiyesi ön uçlarına sahip iki sanal makine ölçek kümeniz varsa ve bunlar aynı abonelik ve bölgedeyse, herkese açık IP adreslerini her birinden bulabilir ve diğerine atayabilirsiniz. Örneğin [Azure Kaynak Yöneticisi'nde VIP Swap: Mavi-yeşil dağıtım.](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) Bu, kaynaklar ağ düzeyinde ayrılmış/tahsis edilmiş olsa da bir gecikme anlamına gelir. Daha hızlı bir seçenek, iki arka uç havuzu ve yönlendirme kuralı olan Azure Uygulama Ağ Geçidi'ni kullanmaktır. Alternatif olarak, uygulamanızı, evreleme ve üretim yuvaları arasında hızlı geçiş için destek sağlayan [Azure Uygulaması hizmetiyle](https://azure.microsoft.com/services/app-service/) barındırabilirsiniz.

### <a name="how-do-i-specify-a-range-of-private-ip-addresses-to-use-for-static-private-ip-address-allocation"></a>Statik özel IP adresi tahsisi için kullanılacak bir dizi özel IP adresini nasıl belirtebilirim?

IP adresleri belirttiğiniz bir alt ağdan seçilir.

Sanal makine ölçeği kümesi IP adreslerinin ayırma yöntemi her zaman "dinamik"tir, ancak bu, bu IP adreslerinin değişebileceği anlamına gelmez. Bu durumda, "dinamik" yalnızca PUT isteğinde IP adresini belirtmediğiniz anlamına gelir. Alt ağı kullanarak statik kümeyi belirtin.

### <a name="how-do-i-deploy-a-virtual-machine-scale-set-to-an-existing-azure-virtual-network"></a>Sanal makine ölçeği kümesini varolan bir Azure sanal ağına nasıl dağıtıyorum?

Varolan bir Azure sanal ağına ayarlı sanal makine ölçeği dağıtmak [için](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet)bkz.

### <a name="can-i-use-scale-sets-with-accelerated-networking"></a>Hızlandırılmış Ağ ile ölçek kümelerini kullanabilir miyim?

Evet. Hızlandırılmış ağ kullanımını kullanmak için, ölçek setinizin ağInterfaceConfigurations ayarlarında doğru şekilde HızlandırılmışAğ'ı ayarlayın. Örneğin:

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

### <a name="how-can-i-configure-the-dns-servers-used-by-a-scale-set"></a>Bir ölçek kümesi tarafından kullanılan DNS sunucularını nasıl yapılandırabilirim?

Özel bir DNS yapılandırması ile sanal makine ölçeği kümesi oluşturmak için, ölçek kümesi ağInterfaceConfigurations bölümüne bir dnsSettings JSON paketi ekleyin. Örnek:

```json
    "dnsSettings":{
        "dnsServers":["10.0.0.6", "10.0.0.5"]
    }
```

### <a name="how-can-i-configure-a-scale-set-to-assign-a-public-ip-address-to-each-vm"></a>Her VM'ye genel bir IP adresi atamak için bir ölçek kümesini nasıl yapılandırabilirim?

Her VM'ye genel bir IP adresi atayan sanal makine ölçeği kümesi oluşturmak için, Microsoft.Compute/virtualMachineScaleSets kaynağının API sürümünün 2017-03-30 olduğundan emin olun ve ölçek kümesi ipConfigurations bölümüne _bir publicipaddressconfiguration_ JSON paketi ekleyin. Örnek:

```json
    "publicipaddressconfiguration": {
        "name": "pub1",
        "properties": {
        "idleTimeoutInMinutes": 15
        }
    }
```

### <a name="can-i-configure-a-scale-set-to-work-with-multiple-application-gateways"></a>Birden çok Uygulama Ağ Geçidi ile çalışacak bir ölçek kümesini yapılandırabilir miyim?

Evet. Ölçek ayarlı ağ profilinizin _ipConfigurations_ _bölümündeuygulamaGatewayBackendAddressPools_ listesine birden çok Uygulama Ağ Geçidi arka uç adresi havuzları için kaynak disleri ekleyebilirsiniz.

## <a name="scale"></a>Ölçek

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a>Hangi durumda ikiden az VM'si olan sanal bir makine ölçeği kümesi oluştururum?

İkiden az VM'si olan sanal makine ölçeği kümesi oluşturmanın bir nedeni, sanal makine ölçeği kümesinin elastik özelliklerini kullanmaktır. Örneğin, VM işletme maliyetlerini ödemeden altyapınızı tanımlamak için sıfır VM'li sanal bir makine ölçeği dağıtabilirsiniz. Ardından, VM'leri dağıtmaya hazır olduğunuzda, üretim örneği sayısına ayarlanan sanal makine ölçeğinin "kapasitesini" artırın.

İkiden az VM'si olan sanal bir makine ölçeği kümesi oluşturmanın bir diğer nedeni de, ayrı vm'ler içeren bir kullanılabilirlik kümesi kullanmaktan daha az kullanılabilirlik ile ilgileniyorsanız. Sanal makine ölçek setleri, farklılaştırılabilen farklılaşmamış bilgi işlem birimleriyle çalışmanız için bir yol verir. Bu tekdüzelik, sanal makine ölçek kümeleri ile kullanılabilirlik kümeleri için önemli bir ayırt layıcıdır. Çoğu durum suz iş yükü tek tek birimleri izlemez. İş yükü düşerse, bir işlem birimine küçültebilir ve iş yükü arttığında çok sayıdasına ölçeklendirebilirsiniz.

### <a name="how-do-i-change-the-number-of-vms-in-a-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesindeki VM sayısını nasıl değiştirebilirim?

Azure portalında ayarlanan sanal makine ölçeğindeki Sanal Makine ölçeğindeki VM sayısını sanal makine ölçeği kümesi özellikleri bölümünden değiştirmek için "Ölçekleme" bıçağına tıklayın ve kaydırıcı çubuğunu kullanın.

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a>Belirli eşiklere ulaşıldığında özel uyarıları nasıl tanımlarım?

Belirtilen eşikler için uyarıları nasıl işleyeceğiniz konusunda biraz esnekliğiniz vardır. Örneğin, özelleştirilmiş web hooks tanımlayabilirsiniz. Aşağıdaki webhook örneği Kaynak Yöneticisi şablonundan dır:

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


## <a name="patching-and-operations"></a>Yama ve işlemler

### <a name="can-i-create-a-scale-set-in-an-existing-resource-group"></a>Varolan bir kaynak grubunda bir ölçek kümesi oluşturabilir miyim?

Evet, varolan bir kaynak grubunda bir ölçek kümesi oluşturabilirsiniz.

### <a name="can-i-move-a-scale-set-to-another-resource-group"></a>Bir ölçek kümesini başka bir kaynak grubuna taşıyabilir miyim?

Evet, ölçek ayarlı kaynakları yeni bir abonelik veya kaynak grubuna taşıyabilirsiniz.

### <a name="how-to-i-update-my-virtual-machine-scale-set-to-a-new-image-how-do-i-manage-patching"></a>Sanal makine ölçeğimi yeni bir görüntüye nasıl güncelleyebilirim? Yama yapmayı nasıl yönetirim?

Sanal makine ölçeğinizi yeni bir görüntüyle güncelleştirmek ve yama yı yönetmek için [bkz.](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set)

### <a name="can-i-use-the-reimage-operation-to-reset-a-vm-without-changing-the-image-that-is-i-want-reset-a-vm-to-factory-settings-rather-than-to-a-new-image"></a>Görüntüyü değiştirmeden bir VM'yi sıfırlamak için yeniden görüntü işlemini kullanabilir miyim? (Yani, ben yerine yeni bir görüntü için fabrika ayarları için bir VM sıfırlamak istiyorum.)

Evet, görüntüyü değiştirmeden vm'yi sıfırlamak için yeniden görüntü işlemini kullanabilirsiniz. Ancak, sanal makine ölçeğiniz bir platform `version = latest`görüntüsüne atıfta bulunuyorsa, VM'niz çağırdığınızda `reimage`daha sonraki bir işletim sistemi görüntüsüne güncellenebilir.

### <a name="is-it-possible-to-integrate-scale-sets-with-azure-monitor-logs"></a>Ölçek kümelerini Azure Monitor günlükleriyle tümleştirmek mümkün mü?

Evet, Azure Monitör uzantısını vm ölçeğine yükleyebilirsiniz. Aşağıda bir Azure CLI örneği verilmiştir:

```azurecli
az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group Team-03 --vmss-name nt01 --settings "{'workspaceId': '<your workspace ID here>'}" --protected-settings "{'workspaceKey': '<your workspace key here'}"
```

Azure portalının Günlük Analizi çalışma alanında gerekli çalışma alanıId ve çalışma alanıAnahtarını bulabilirsiniz. Genel Bakış sayfasında Ayarlar döşemesini tıklatın. Üstteki Bağlı Kaynaklar sekmesini tıklatın.

> [!NOTE]
> Ölçek _ayarlamanız yükseltme Politikanız_ El Kitabı olarak ayarlanmışsa, uzantıyı, yükseltmeyi çağırarak kümedeki tüm VM'lere uygulamanız gerekir. CLI bu _az vmss güncelleme örnekleri_olacaktır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="troubleshooting"></a>Sorun giderme

### <a name="how-do-i-turn-on-boot-diagnostics"></a>Önyükleme tanılamayı nasıl açarım?

Önyükleme tanılamasını açmak için önce bir depolama hesabı oluşturun. Sonra, sanal makine ölçeği **sanalMachineProfile**ayarlamak bu JSON blok koymak ve sanal makine ölçek kümesini güncelleyin:

```json
"diagnosticsProfile": {
    "bootDiagnostics": {
        "enabled": true,
        "storageUri": "http://yourstorageaccount.blob.core.windows.net"
    }
}
```

Yeni bir VM oluşturulduğunda, VM'nin InstanceView özelliği ekran görüntüsünün ayrıntılarını gösterir ve böyle devam edecektir. Bir örneği aşağıda verilmiştir:

```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
}
```

## <a name="virtual-machine-properties"></a>Sanal makine özellikleri

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-the-fault-domain-for-each-of-the-100-vms-in-my-virtual-machine-scale-set"></a>Birden fazla arama yapmadan her VM için özellik bilgilerini nasıl alabilirim? Örneğin, sanal makine ölçeğimdeki 100 VM'nin her biri için hata etki alanını nasıl alabilirim?

Birden çok arama yapmadan her VM için özellik `ListVMInstanceViews` bilgilerini almak için, aşağıdaki kaynak URI bir REST API `GET` yaparak arayabilirsiniz:

/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.Compute/virtualMachineScaleSets/<scaleset_name>/virtualMachines?$expand=instanceView&$select=instanceView

### <a name="can-i-pass-different-extension-arguments-to-different-vms-in-a-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesinde farklı genişletme bağımsız değişkenlerini farklı VM'lere geçirebilir miyim?

Hayır, sanal makine ölçeği kümesinde farklı VM'lere farklı uzantılı bağımsız değişkenler geçiremezsiniz. Ancak uzantılar, üzerinde çalıştıkları VM'nin makine adı gibi benzersiz özelliklerine göre hareket edebilir. Uzantılar, VM hakkında http://169.254.169.254 daha fazla bilgi almak için örnek meta verilerini de sorgulayabilir.

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a>Sanal makine ölçeğimde VM makine adları ve VM iDe'leri arasında neden boşluklar var? Örneğin: 0, 1, 3...

Sanal makine ölçeği küme VM makine adları ve VM iD'leri arasında boşluklar vardır, çünkü sanal makine ölçeğiniz **aşırı sağlama** özelliğini **ayarlamalıdır.** Aşırı sağlama **doğru**ayarlanmışsa, istenenden daha fazla VM oluşturulur. Daha sonra ekstra VM'ler silinir. Bu durumda, daha fazla dağıtım güvenilirliği kazanırsınız, ancak bitişik adlandırma ve bitişik Ağ Adresi Çevirisi (NAT) kuralları pahasına.

Bu özelliği **false**olarak ayarlayabilirsiniz. Küçük sanal makine ölçek kümeleri için bu, dağıtım güvenilirliğini önemli ölçüde etkilemez.

### <a name="what-is-the-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-the-vm-when-should-i-choose-one-over-the-other"></a>Sanal makine ölçeği kümesinde VM'yi silen ve VM'yi ayırmak arasındaki fark nedir? Birini diğerine ne zaman tercih etmeliyim?

Sanal makine ölçeği kümesinde VM'yi silmek ile VM'yi ayırmaarasındaki temel fark, sanal sabit diskleri (VHD'ler) `deallocate` silmemesidir. Çalıştırmayla `stop deallocate`ilişkili depolama maliyetleri vardır. Aşağıdaki nedenlerden biri veya diğerini kullanabilirsiniz:

- İşlem maliyetlerini ödemeyi durdurmak istiyorsunuz, ancak VM'lerin disk durumunu korumak istiyorsunuz.
- Sanal makine ölçeği kümesini ölçeklendirebileceğinden daha hızlı bir şekilde bir Sanal M kümesi başlatmak istiyorsunuz.
  - Bu senaryoyla ilgili olarak, kendi otomatik ölçeklendirme altyapınızı oluşturmuş ve daha hızlı bir uç-uç ölçek isteyebilirsiniz.
- Hata etki alanları arasında eşit olmayan dağıtılan veya etki alanlarını güncelleştiren sanal bir makine ölçeği kümeniz var. Bunun nedeni, Seçkili olarak VM'leri sildiğiniz veya VM'lerin aşırı sağlama dan sonra silinmiş olması olabilir. Sanal `stop deallocate` makine `start` ölçeğinde çalışan ve ardından, Sanal Makineler'i hata etki alanları arasında eşit olarak dağıtır veya etki alanlarını günceller.

### <a name="how-do-i-take-a-snapshot-of-a-virtual-machine-scale-set-instance"></a>Sanal makine ölçeği kümesi örneğinin anlık görüntüsünü nasıl alırım?
Sanal makine ölçeği kümesiörneğinden anlık görüntü oluşturun.

```azurepowershell-interactive
$rgname = "myResourceGroup"
$vmssname = "myVMScaleSet"
$Id = 0
$location = "East US"

$vmss1 = Get-AzVmssVM -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $Id     
$snapshotconfig = New-AzSnapshotConfig -Location $location -AccountType Standard_LRS -OsType Windows -CreateOption Copy -SourceUri $vmss1.StorageProfile.OsDisk.ManagedDisk.id
New-AzSnapshot -ResourceGroupName $rgname -SnapshotName 'mySnapshot' -Snapshot $snapshotconfig
```

Anlık görüntüden yönetilen bir disk oluşturun.

```azurepowershell-interactive
$snapshotName = "myShapshot"
$snapshot = Get-AzSnapshot -ResourceGroupName $rgname -SnapshotName $snapshotName  
$diskConfig = New-AzDiskConfig -AccountType Premium_LRS -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
$osDisk = New-AzDisk -Disk $diskConfig -ResourceGroupName $rgname -DiskName ($snapshotName + '_Disk')
```
