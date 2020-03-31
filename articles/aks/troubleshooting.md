---
title: Sık karşılaşılan Azure Kubernetes Hizmeti sorunlarını giderme
description: Azure Kubernetes Hizmetini (AKS) kullanırken sık karşılaşılan sorunları nasıl gidereceğinizi ve nasıl çözeceğinizi öğrenin
services: container-service
author: sauryadas
ms.topic: troubleshooting
ms.date: 12/13/2019
ms.author: saudas
ms.openlocfilehash: 7bdabf2ec109fe96c28185bd1a2a680ce19c2650
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79368341"
---
# <a name="aks-troubleshooting"></a>AKS sorunlarını giderme

Azure Kubernetes Hizmeti (AKS) kümeleri oluşturduğunuzda veya yönettiğinizde, zaman zaman sorunlarla karşılaşabilirsiniz. Bu makalede, bazı sık karşılaşılan sorunlar ve sorun giderme adımları ayrıntıları.

## <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-problems"></a>Genel olarak, Kubernetes hata ayıklama sorunları hakkında bilgi nereden bulabilirim?

[Kubernetes kümelerini sorun giderme için resmi kılavuzu](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/)deneyin.
Sorun giderme bölmeleri, düğümler, kümeler ve diğer özellikler için bir Microsoft mühendisi tarafından yayınlanan bir [sorun giderme kılavuzu](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md)da vardır.

## <a name="im-getting-a-quota-exceeded-error-during-creation-or-upgrade-what-should-i-do"></a>Oluşturma veya yükseltme sırasında bir "kota aşıldı" hatası alıyorum. Ne yapmalıyım? 

[Çekirdek leri talep](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request)etmelisin.

## <a name="what-is-the-maximum-pods-per-node-setting-for-aks"></a>AKS için düğüm başına maksimum bölme ayarı nedir?

Azure portalında bir AKS kümesi dağıtırsanız, düğüm başına maksimum bölme ayarı varsayılan olarak 30'dur.
Azure CLI'de bir AKS kümesi dağıtırsanız, düğüm başına maksimum bölme ayarı varsayılan olarak 110'dur. (Azure CLI'nin en son sürümünü kullandığınızdan emin olun). Bu varsayılan ayar `–-max-pods` `az aks create` komuttaki bayrak kullanılarak değiştirilebilir.

## <a name="im-getting-an-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Gelişmiş ağ ile bir AKS küme dağıtırken bir yetersizSubnetSize hata alıyorum. Ne yapmalıyım?

Azure CNI (gelişmiş ağ) kullanılırsa, AKS IP adreslerini yapılandırılan düğüm başına "max-pods"a göre ayırır. Düğüm başına yapılandırılan maksimum bölmelere bağlı olarak, alt ağ boyutu düğüm sayısının çarpımından ve düğüm başına maksimum bölme ayarından daha büyük olmalıdır. Aşağıdaki denklem de şu şekilde dir:

Kümedeki alt ağ boyutu > sayı (gelecekteki ölçekleme gereksinimleri dikkate alınarak) * düğüm kümesi başına maksimum bölme.

Daha fazla bilgi için [kümeniz için IP Planı adresine](configure-azure-cni.md#plan-ip-addressing-for-your-cluster)bakın.

## <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>Benim pod CrashLoopBackOff modunda sıkışmış. Ne yapmalıyım?

Kapsülün bu modda sıkışıp kalması için çeşitli nedenler olabilir. Şuna bakabilirsiniz:

* Pod kendisi, kullanarak `kubectl describe pod <pod-name>`.
* Günlükleri `kubectl logs <pod-name>`kullanarak.

Bölme sorunlarını nasıl gidereceklerine ilişkin daha fazla bilgi için [hata ayıklama uygulamalarına](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#debugging-pods)bakın.

## <a name="im-trying-to-enable-rbac-on-an-existing-cluster-how-can-i-do-that"></a>RBAC'ı varolan bir kümede etkinleştirmeye çalışıyorum. Bunu nasıl yapabilirim?

Ne yazık ki, varolan kümelerde rol tabanlı erişim denetimini (RBAC) etkinleştirmek şu anda desteklenmez. Açıkça yeni kümeler oluşturmanız gerekir. CLI kullanıyorsanız, RBAC varsayılan olarak etkinleştirilir. AKS portalını kullanıyorsanız, oluşturma iş akışında RBAC'ı etkinleştirmek için bir geçiş düğmesi kullanılabilir.

## <a name="i-created-a-cluster-with-rbac-enabled-by-using-either-the-azure-cli-with-defaults-or-the-azure-portal-and-now-i-see-many-warnings-on-the-kubernetes-dashboard-the-dashboard-used-to-work-without-any-warnings-what-should-i-do"></a>Varsayılanlı Azure CLI'yi veya Azure portalını kullanarak RBAC özellikli bir küme oluşturdum ve şimdi Kubernetes panosunda birçok uyarı görüyorum. Pano herhangi bir uyarı olmadan çalışmak için kullanılır. Ne yapmalıyım?

Panodaki uyarıların nedeni, kümenin artık RBAC ile etkinleştirilmiş olması ve bu kümeye erişimin varsayılan olarak devre dışı bırakılmış olmasıdır. Genel olarak, bu yaklaşım iyi bir uygulamadır, çünkü panokümenin tüm kullanıcılarına varsayılan olarak maruz kalma güvenlik tehditlerine yol açabilir. Panoyu etkinleştirmek istiyorsanız, bu blog [gönderisindeki](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/)adımları izleyin.

## <a name="i-cant-connect-to-the-dashboard-what-should-i-do"></a>Panoya bağlanamıyorum. Ne yapmalıyım?

Hizmetiniz küme dışında erişmenin en kolay `kubectl proxy`yolu, yerel ana bilgisayar bağlantı noktasına gönderilen istekleri Kubernetes API sunucusuna gönderen vekillerin isteklerini çalıştırmaktır. Buradan, API sunucusu hizmetinize proxy `http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/node?namespace=default`olabilir: .

Kubernetes panosunu görmüyorsanız, bölmenin `kube-proxy` ad alanında çalışıp `kube-system` çalışmadığını kontrol edin. Çalışan bir durumda değilse, bölmeyi silin ve yeniden başlatılacaktır.

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Kubectl günlüklerini kullanarak günlükleri alamıyorum veya API sunucusuna bağlanamıyorum. "Sunucudan hata: hata arama arka uç: arama tcp..." alıyorum. Ne yapmalıyım?

Varsayılan ağ güvenlik grubunun değiştirilmediğinden ve her iki bağlantı noktası 22 ve 9000'in API sunucusuna bağlantı için açık olduğundan emin olun. Bölmenin `tunnelfront` *kube-sistem* ad alanında komutu `kubectl get pods --namespace kube-system` kullanarak çalışıp çalışmadığını denetleyin. Değilse, bölmenin zorla silinmesini ve yeniden başlatılacaktır.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error-how-do-i-fix-this-problem"></a>Yükseltmeye veya ölçeklendirmeye çalışıyorum ve bir "ileti alıyorum: Özellik 'imageReference' değiştirmeye izin verilmez" hatası. Bu sorunu nasıl giderebilirim?

AKS kümesiiçindeki aracı düğümlerinde etiketleri değiştirdiğiniz için bu hatayı alıyor olabilirsiniz. MC_* kaynak grubundaki kaynakların etiketlerini ve diğer özelliklerini değiştirme ve silme beklenmeyen sonuçlara yol açabilir. AKS kümesindeki MC_* grubu altındaki kaynakların değiştirilmesi hizmet düzeyi hedefini (SLO) bozar.

## <a name="im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed"></a>Kümemin başarısız durumda olduğu hatalar alıyorum ve yükseltme veya ölçekleme düzeltilene kadar çalışmayacak

*Bu sorun giderme yardımı,https://aka.ms/aks-cluster-failed*

Bu hata, kümeler birden çok nedenden dolayı başarısız bir durum girdiğinde oluşur. Daha önce başarısız olan işlemi yeniden denemeden önce küme başarısız durumunuzu gidermek için aşağıdaki adımları izleyin:

1. Küme `failed` eyalet dışına çıkana `upgrade` `scale` ve işlemler başarılı olamayacak olana kadar. Ortak kök sorunları ve çözümleri şunlardır:
    * Yetersiz bilgi **işlem (CRP) kotası**ile ölçekleme. Çözmek için, önce kümenizi kota içinde kararlı bir hedef durumuna geri ölçeklendirin. Ardından, ilk kota sınırlarının ötesine yeniden ölçeklendirmeye çalışmadan önce [bir işlem kotası artışı istemek](../azure-portal/supportability/resource-manager-core-quotas-request.md) için aşağıdaki adımları izleyin.
    * Gelişmiş ağ ve yetersiz alt **ağ (ağ) kaynakları**ile bir küme ölçekleme. Çözmek için, önce kümenizi kota içinde kararlı bir hedef durumuna geri ölçeklendirin. Ardından, ilk kota sınırlarının ötesine yeniden ölçeklendirmeye çalışmadan önce [kaynak kotası artışı istemek](../azure-resource-manager/templates/error-resource-quota.md#solution) için aşağıdaki adımları izleyin.
2. Yükseltme hatasının altında yatan neden çözüldükten sonra, kümeniz inbaşarılı bir durumda olmalıdır. Başarılı bir durum doğrulandıktan sonra, özgün işlemi yeniden deneyin.

## <a name="im-receiving-errors-when-trying-to-upgrade-or-scale-that-state-my-cluster-is-being-currently-being-upgraded-or-has-failed-upgrade"></a>Kümemin şu anda yükseltilmekte olduğunu veya yükseltmede başarısız olduğunu yükseltmeye veya ölçeklendirmeye çalışırken hatalar alıyorum

*Bu sorun giderme yardımı,https://aka.ms/aks-pending-upgrade*

Tek bir düğüm havuzu veya [birden çok düğüm havuzu](use-multiple-node-pools.md) olan bir kümedeki yükseltme ve ölçeklendirme işlemleri birbirini dışlar. Aynı anda yükseltme ve ölçeklendirme küme veya düğüm havuzu olamaz. Bunun yerine, her işlem türü, aynı kaynaktaki bir sonraki istekten önce hedef kaynakta tamamlanmalıdır. Sonuç olarak, etkin yükseltme veya ölçek işlemleri gerçekleştiğinde veya denendiğinde ve daha sonra başarısız olduğunda işlemler sınırlıdır. 

Kümenizde ayrıntılı durum `az aks show -g myResourceGroup -n myAKSCluster -o table` almak için sorun çalıştırın tanıyardımcı olmak için. Sonuca göre:

* Küme etkin olarak yükseltiyorsa, işlem sona erene kadar bekleyin. Başarılı olduysa, daha önce başarısız olan işlemi yeniden deneyin.
* Küme yükseltmede başarısız olduysa, önceki bölümde özetlenen adımları izleyin.

## <a name="can-i-move-my-cluster-to-a-different-subscription-or-my-subscription-with-my-cluster-to-a-new-tenant"></a>Kümemi farklı bir aboneye veya kümemle aboneliğimle birlikte yeni bir kiracıya taşıyabilir miyim?

AKS kümenizi farklı bir aboneye veya küme sahibi aboneliğinizi yeni bir kiracıya taşıdıysanız, küme rol atamaları ve hizmet ilkeleri haklarını kaybetme nedeniyle işlevselliğini kaybeder. AKS, bu kısıtlama nedeniyle **kümelerin abonelikler veya kiracılar arasında taşınmasını desteklemez.**

## <a name="im-receiving-errors-trying-to-use-features-that-require-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri gerektiren özellikleri kullanmaya çalışırken hatalar alıyorum

*Bu sorun giderme yardımı, aka.ms/aks-vmss-enablement*

AKS kümenizin sanal makine ölçeğinde olmadığını gösteren hatalar alabilirsiniz( örneğin:

**AgentPool 'agentpool' otomatik ölçeklendirmeyi etkin olarak ayarladı ancak Sanal Makine Ölçeği Kümeleri'nde değil**

Küme otomatik ölçekleyici veya birden çok düğüm havuzları gibi özellikleri kullanmak için, sanal makine ölçek kümeleri kullanan AKS kümeleri oluşturulmalıdır. Sanal makine ölçek kümelerine bağlı özellikleri kullanmaya çalışırsanız ve normal, sanal olmayan makine ölçeği kümesi AKS kümesini hedefliyorsanız hatalar döndürülür.

AkS kümesini doğru oluşturmak için uygun dokümandaki *adımları başlatmadan* izleyin:

* [Küme otomatik ölçekleyicisini kullanma](cluster-autoscaler.md)
* [Birden çok düğüm havuzu oluşturma ve kullanma](use-multiple-node-pools.md)
 
## <a name="what-naming-restrictions-are-enforced-for-aks-resources-and-parameters"></a>AKS kaynakları ve parametreleri için hangi adlandırma kısıtlamaları uygulanır?

*Bu sorun giderme yardımı, aka.ms/aks-naming-rules*

Adlandırma kısıtlamaları hem Azure platformu hem de AKS tarafından uygulanır. Bir kaynak adı veya parametre bu kısıtlamalardan birini kırarsa, farklı bir giriş sağlamanızı isteyen bir hata döndürülür. Aşağıdaki yaygın adlandırma yönergeleri geçerlidir:

* Küme adları 1-63 karakter olmalıdır. İzin verilen tek karakterler harfler, sayılar, tireler ve alt çizerlerdir. İlk ve son karakter bir harf veya sayı olmalıdır.
* AKS *MC_* kaynak grubu adı kaynak grubu adı ve kaynak adı birleştirir. Otomatik olarak oluşturulan sözdizimi `MC_resourceGroupName_resourceName_AzureRegion` 80 char'dan büyük olmamalıdır. Gerekirse, kaynak grup adınızın veya AKS küme adının uzunluğunu azaltın.
* *dnsPrefix* alfasayısal değerlerle başlayıp bitmeli ve 1-54 karakter arasında olmalıdır. Geçerli karakterler alfasayısal değerleri ve tireleri (-) içerir. *dnsPrefix* dönem (.) gibi özel karakterler içeremez.

## <a name="im-receiving-errors-when-trying-to-create-update-scale-delete-or-upgrade-cluster-that-operation-is-not-allowed-as-another-operation-is-in-progress"></a>Küme oluşturmaya, güncelleştirmeye, ölçeklendirmeye, silmeye veya yükseltmeye çalışırken hatalar alıyorum, bu işlem başka bir işlem devam ederken izin verilmiyor.

*Bu sorun giderme yardımı, aka.ms/aks-pending-operation*

Önceki bir işlem devam ederken küme işlemleri sınırlıdır. Kümenizin ayrıntılı bir durumunu almak `az aks show -g myResourceGroup -n myAKSCluster -o table` için komutu kullanın. Gerektiğinde kendi kaynak grubunuzu ve AKS küme adınızı kullanın.

Küme durumunun çıktısını temel alır:

* Küme *Başarılı* veya *Başarısız*dışında herhangi bir sağlama durumundaysa,*(Yükseltme / Güncelleme / Oluşturma / Ölçekleme / Silme / Geçiş*) işlemi sona erene kadar bekleyin. Önceki işlem tamamlandığında, en son küme işleminizi yeniden deneyin.

* Kümenin başarısız bir yükseltmesi varsa, [kümemin başarısız durumda olduğu hataları alıyorum ve yükseltme veya ölçekleme düzeltilene kadar çalışmaz](#im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed)özetlenen adımları izleyin.

## <a name="im-receiving-errors-that-my-service-principal-was-not-found-when-i-try-to-create-a-new-cluster-without-passing-in-an-existing-one"></a>Varolan bir kümeyi geçirmeden yeni bir küme oluşturmaya çalıştığımda hizmet müdürümün bulunmadığı hatalar alıyorum.

Bir AKS kümesi oluştururken, sizin adınıza kaynak oluşturmak için bir hizmet sorumlusu gerektirir. AKS, küme oluşturma zamanında yeni bir tane oluşturma olanağı sunar, ancak bu, kümenin oluşturmada başarılı olması için Azure Active Directory'nin yeni hizmet ilkesini makul bir süre içinde tam olarak yaymasını gerektirir. Bu yayılma çok uzun sürdüğünde, küme bunu yapmak için kullanılabilir bir hizmet ilkesi bulamadığı için doğrulamayı oluşturamaz. 

Bunun için aşağıdaki geçici geçici çözümlerini kullanın:
1. Zaten bölgeler arasında yayılan ve küme oluşturma zamanında AKS'ye geçmek için var olan bir hizmet ilkesi kullanın.
2. Otomasyon komut dosyaları kullanıyorsanız, hizmet temel oluşturma ve AKS küme oluşturma arasında zaman gecikmeleri ekleyin.
3. Azure portalı kullanıyorsanız, oluşturma sırasında küme ayarlarına dönün ve birkaç dakika sonra doğrulama sayfasını yeniden deneyin.

## <a name="im-receiving-errors-after-restricting-my-egress-traffic"></a>Çıkış trafiğimi kısıtladıktan sonra hatalar alıyorum

Bir AKS kümesinden çıkış trafiğini kısıtlarken, AKS için [gerekli ve isteğe bağlı önerilen](limit-egress-traffic.md) giden bağlantı noktaları / ağ kuralları ve FQDN / uygulama kuralları vardır. Ayarlarınız bu kurallardan herhangi biriyle çakışıyorsa, belirli `kubectl` komutları çalıştıramayabilirsiniz. Aks kümesi oluştururken de hatalar görebilirsiniz.

Ayarlarınızın gerekli veya isteğe bağlı önerilen giden bağlantı noktaları / ağ kuralları ve FQDN / uygulama kuralları ile çelişmediğini doğrulayın.

## <a name="azure-storage-and-aks-troubleshooting"></a>Azure Depolama ve AKS Sorun Giderme

### <a name="what-are-the-recommended-stable-versions-of-kubernetes-for-azure-disk"></a>Azure disk için Kubernetes' in önerilen kararlı sürümleri nelerdir? 

| Kubernetes sürümü | Önerilen sürüm |
| -- | :--: |
| 1.12 | 1.12.9 veya sonrası |
| 1.13 | 1.13.6 veya sonrası |
| 1.14 | 1.14.2 veya sonrası |


### <a name="what-versions-of-kubernetes-have-azure-disk-support-on-the-sovereign-cloud"></a>Egemen Bulut'ta Kubernetes'in hangi sürümlerinde Azure Disk desteği vardır?

| Kubernetes sürümü | Önerilen sürüm |
| -- | :--: |
| 1.12 | 1.12.0 veya sonrası |
| 1.13 | 1.13.0 veya sonrası |
| 1.14 | 1.14.0 veya sonrası |


### <a name="waitforattach-failed-for-azure-disk-parsing-devdiskazurescsi1lun1-invalid-syntax"></a>Azure Disk için WaitForAttach başarısız oldu: "/dev/disk/azure/scsi1/lun1" ayrıştırma: geçersiz sözdizimi

Kubernetes sürüm 1.10'da MountVolume.WaitForAttach Azure Disk yeniden montajıyla başarısız olabilir.

Linux'ta yanlış devicepath biçimi hatası görebilirsiniz. Örnek:

```console
MountVolume.WaitForAttach failed for volume "pvc-f1562ecb-3e5f-11e8-ab6b-000d3af9f967" : azureDisk - Wait for attach expect device path as a lun number, instead got: /dev/disk/azure/scsi1/lun1 (strconv.Atoi: parsing "/dev/disk/azure/scsi1/lun1": invalid syntax)
  Warning  FailedMount             1m (x10 over 21m)   kubelet, k8s-agentpool-66825246-0  Unable to mount volumes for pod
```

Windows'da yanlış bir DevicePath(LUN) numarası hatası görebilirsiniz. Örnek:

```console
Warning  FailedMount             1m    kubelet, 15282k8s9010    MountVolume.WaitForAttach failed for volume "disk01" : azureDisk - WaitForAttach failed within timeout node (15282k8s9010) diskId:(andy-mghyb
1102-dynamic-pvc-6c526c51-4a18-11e8-ab5c-000d3af7b38e) lun:(4)
```

Bu sorun Kubernetes aşağıdaki sürümlerinde giderilmiştir:

| Kubernetes sürümü | Sabit sürüm |
| -- | :--: |
| 1.10 | 1.10.2 veya sonrası |
| 1.11 | 1.11.0 veya sonrası |
| 1.12 ve sonrası | Yok |

### <a name="failure-when-setting-uid-and-gid-in-mountoptions-for-azure-disk"></a>Azure Disk için mountOptions'ta uid ve gid ayarı yaparken hata

Azure Disk varsayılan olarak ext4,xfs dosya sistemini kullanır ve uid=x,gid=x gibi montaj seçenekleri montaj zamanında ayarlanamaz. Örneğin, mountOptions uid=999,gid=999'u ayarlamaya çalıştıysanız, şu gibi bir hata görürsünüz:

```console
Warning  FailedMount             63s                  kubelet, aks-nodepool1-29460110-0  MountVolume.MountDevice failed for volume "pvc-d783d0e4-85a1-11e9-8a90-369885447933" : azureDisk - mountDevice:FormatAndMount failed with mount failed: exit status 32
Mounting command: systemd-run
Mounting arguments: --description=Kubernetes transient mount for /var/lib/kubelet/plugins/kubernetes.io/azure-disk/mounts/m436970985 --scope -- mount -t xfs -o dir_mode=0777,file_mode=0777,uid=1000,gid=1000,defaults /dev/disk/azure/scsi1/lun2 /var/lib/kubelet/plugins/kubernetes.io/azure-disk/mounts/m436970985
Output: Running scope as unit run-rb21966413ab449b3a242ae9b0fbc9398.scope.
mount: wrong fs type, bad option, bad superblock on /dev/sde,
       missing codepage or helper program, or other error
```

Aşağıdakilerden birini yaparak sorunu azaltabilirsiniz:

* fsGroup'ta runAsUser ve gid'de uid ayarlayarak [bir bölme için güvenlik bağlamını yapılandırın.](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) Örneğin, aşağıdaki ayar pod run'ı kök olarak ayarlar ve herhangi bir dosya için erişilebilir hale getirir:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  securityContext:
    runAsUser: 0
    fsGroup: 0
```

  >[!NOTE]
  > Gid ve uid varsayılan olarak kök veya 0 olarak monte olduğundan. Gid veya uid kök süzme olarak ayarlanırsa, örneğin 1000, Kubernetes bu diskin altındaki tüm dizinleri ve dosyaları değiştirmek için kullanır. `chown` Bu işlem zaman alıcı olabilir ve diskin montajının çok yavaş olmasını sağlayabilir.

* Gid `chown` ve uid ayarlamak için initContainers kullanın. Örnek:

```yaml
initContainers:
- name: volume-mount
  image: busybox
  command: ["sh", "-c", "chown -R 100:100 /data"]
  volumeMounts:
  - name: <your data volume>
    mountPath: /data
```

### <a name="error-when-deleting-azure-disk-persistentvolumeclaim-in-use-by-a-pod"></a>Azure Disk KalıcıSını Bir bölme tarafından kullanılan İşlem Silme hatası

Bir bölme tarafından kullanılan bir Azure Disk KalıcıSesiAlma'yı silmeye çalışırsanız, bir hata görebilirsiniz. Örnek:

```console
$ kubectl describe pv pvc-d8eebc1d-74d3-11e8-902b-e22b71bb1c06
...
Message:         disk.DisksClient#Delete: Failure responding to request: StatusCode=409 -- Original Error: autorest/azure: Service returned an error. Status=409 Code="OperationNotAllowed" Message="Disk kubernetes-dynamic-pvc-d8eebc1d-74d3-11e8-902b-e22b71bb1c06 is attached to VM /subscriptions/{subs-id}/resourceGroups/MC_markito-aks-pvc_markito-aks-pvc_westus/providers/Microsoft.Compute/virtualMachines/aks-agentpool-25259074-0."
```

Kubernetes sürüm 1.10 ve sonraki sürümlerde, bu hatayı önlemek için varsayılan olarak etkinleştirilen bir PersistentVolumeClaim koruma özelliği vardır. Bu sorun için düzeltme olmayan Kubernetes sürümünü kullanıyorsanız, PersistentVolumeClaim'i silmeden önce PersistentVolumeClaim'i kullanarak bölmeyi silerek bu sorunu azaltabilirsiniz.


### <a name="error-cannot-find-lun-for-disk-when-attaching-a-disk-to-a-node"></a>Bir düğüme disk takarken "Disk için Lun bulamıyor" hatası

Bir düğüme disk eklediğinizde aşağıdaki hatayı görebilirsiniz:

```console
MountVolume.WaitForAttach failed for volume "pvc-12b458f4-c23f-11e8-8d27-46799c22b7c6" : Cannot find Lun for disk kubernetes-dynamic-pvc-12b458f4-c23f-11e8-8d27-46799c22b7c6
```

Bu sorun Kubernetes aşağıdaki sürümlerinde giderilmiştir:

| Kubernetes sürümü | Sabit sürüm |
| -- | :--: |
| 1.10 | 1.10.10 veya sonrası |
| 1.11 | 1.11.5 veya sonrası |
| 1.12 | 1.12.3 veya sonrası |
| 1.13 | 1.13.0 veya sonrası |
| 1.14 ve sonrası | Yok |

Bu sorun için düzeltme olmayan Kubernetes bir sürümünü kullanıyorsanız, birkaç dakika bekleyip yeniden deneyerek sorunu azaltabilirsiniz.

### <a name="azure-disk-attachdetach-failure-mount-issues-or-io-errors-during-multiple-attachdetach-operations"></a>Birden çok ekleme/ayırma işlemi sırasında Azure Disk ekleme/ayırma hatası, montaj sorunları veya G/Ç hataları

Kubernetes sürüm 1.9.2'den başlayarak, birden fazla ekleme/ayırma işlemi paralel olarak çalıştırırken, kirli VM önbelleği nedeniyle aşağıdaki disk sorunlarını görebilirsiniz:

* Disk ekleme/ayırma hataları
* Disk G/Ç hataları
* VM'den beklenmeyen disk kopması
* VM, var olmayan diskin takılması nedeniyle başarısız duruma giriyor

Bu sorun Kubernetes aşağıdaki sürümlerinde giderilmiştir:

| Kubernetes sürümü | Sabit sürüm |
| -- | :--: |
| 1.10 | 1.10.12 veya sonrası |
| 1.11 | 1.11.6 veya sonrası |
| 1.12 | 1.12.4 veya sonrası |
| 1.13 | 1.13.0 veya sonrası |
| 1.14 ve sonrası | Yok |

Bu sorun için düzeltme olmayan Kubernetes bir sürümünü kullanıyorsanız, aşağıdakileri deneyerek sorunu azaltabilirsiniz:

* Bir disk uzun süre ayırmak için bekliyorsa, diski el ile ayırmayı deneyin

### <a name="azure-disk-waiting-to-detach-indefinitely"></a>Azure Disk süresiz olarak ayırmayı bekliyor

Bazı durumlarda, bir Azure Disk ayırma işlemi ilk denemede başarısız olursa, ayırma işlemini yeniden denemez ve özgün düğüm VM'ye bağlı kalır. Bu hata, bir diski bir düğümden diğerine taşınırken oluşabilir. Örnek:

```console
[Warning] AttachVolume.Attach failed for volume "pvc-7b7976d7-3a46-11e9-93d5-dee1946e6ce9" : Attach volume "kubernetes-dynamic-pvc-7b7976d7-3a46-11e9-93d5-dee1946e6ce9" to instance "/subscriptions/XXX/resourceGroups/XXX/providers/Microsoft.Compute/virtualMachines/aks-agentpool-57634498-0" failed with compute.VirtualMachinesClient#CreateOrUpdate: Failure sending request: StatusCode=0 -- Original Error: autorest/azure: Service returned an error. Status= Code="ConflictingUserInput" Message="Disk '/subscriptions/XXX/resourceGroups/XXX/providers/Microsoft.Compute/disks/kubernetes-dynamic-pvc-7b7976d7-3a46-11e9-93d5-dee1946e6ce9' cannot be attached as the disk is already owned by VM '/subscriptions/XXX/resourceGroups/XXX/providers/Microsoft.Compute/virtualMachines/aks-agentpool-57634498-1'."
```

Bu sorun Kubernetes aşağıdaki sürümlerinde giderilmiştir:

| Kubernetes sürümü | Sabit sürüm |
| -- | :--: |
| 1.11 | 1.11.9 veya sonrası |
| 1.12 | 1.12.7 veya sonrası |
| 1.13 | 1.13.4 veya sonrası |
| 1.14 ve sonrası | Yok |

Bu sorun için düzeltme olmayan Kubernetes bir sürümünü kullanıyorsanız, diski el ile ayırarak sorunu azaltabilirsiniz.

### <a name="azure-disk-detach-failure-leading-to-potential-race-condition-issue-and-invalid-data-disk-list"></a>Azure Disk ayırma hatası, olası yarış koşulu sorununa ve geçersiz veri diski listesine yol açıyor

Bir Azure Diski kopmazsa, üstel geri çekiyi kullanarak diski ayırmak için altı kata kadar yeniden dener. Ayrıca, veri diski listesinde yaklaşık 3 dakika boyunca düğüm düzeyinde bir kilit tutar. Disk listesi, el ile ekleme veya ayırma işlemi gibi bu süre içinde el ile güncelleştirilirse, düğüm düzeyindeki kilit tarafından tutulan disk listesinin geçersiz olması ve VM düğümünde kararsızlığa neden olur.

Bu sorun Kubernetes aşağıdaki sürümlerinde giderilmiştir:

| Kubernetes sürümü | Sabit sürüm |
| -- | :--: |
| 1.12 | 1.12.9 veya sonrası |
| 1.13 | 1.13.6 veya sonrası |
| 1.14 | 1.14.2 veya sonrası |
| 1.15 ve sonrası | Yok |

Bu sorun için düzeltme olmayan Kubernetes bir sürümünü kullanıyorsanız ve düğüm VM eski bir disk listesi varsa, tek bir toplu işlem olarak VM tüm varolmayan diskler ayırarak sorunu azaltabilirsiniz. **Varolmayan diskleri tek tek ayırmak başarısız olabilir.**


### <a name="large-number-of-azure-disks-causes-slow-attachdetach"></a>Çok sayıda Azure Diski yavaş ekleme/ayırmaya neden olur

Bir düğüm VM'sine eklenen Azure Disklerinin sayısı 10'dan büyükse, ekleme ve ayırma işlemleri yavaş olabilir. Bu sorun bilinen bir sorundur ve şu anda geçici çözüm yoktur.

### <a name="azure-disk-detach-failure-leading-to-potential-node-vm-in-failed-state"></a>Azure Disk iatüre hatası başarısız durumda potansiyel düğüm VM'ye yol açıyor

Bazı kenar durumlarında, Bir Azure Disk ayırması kısmen başarısız olabilir ve düğüm VM'yi başarısız bir durumda bırakabilir.

Bu sorun Kubernetes aşağıdaki sürümlerinde giderilmiştir:

| Kubernetes sürümü | Sabit sürüm |
| -- | :--: |
| 1.12 | 1.12.10 veya sonrası |
| 1.13 | 1.13.8 veya sonrası |
| 1.14 | 1.14.4 veya sonrası |
| 1.15 ve sonrası | Yok |

Bu sorun için düzeltme olmayan ve düğüm VM başarısız durumda olan Kubernetes bir sürümünü kullanıyorsanız, aşağıdakilerden birini kullanarak VM durumunu el ile güncelleyerek sorunu azaltabilirsiniz:

* Kullanılabilirlik kümesi tabanlı küme için:
    ```azurecli
    az vm update -n <VM_NAME> -g <RESOURCE_GROUP_NAME>
    ```

* VMSS tabanlı küme için:
    ```azurecli
    az vmss update-instances -g <RESOURCE_GROUP_NAME> --name <VMSS_NAME> --instance-id <ID>
    ```

## <a name="azure-files-and-aks-troubleshooting"></a>Azure Dosyaları ve AKS Sorun Giderme

### <a name="what-are-the-recommended-stable-versions-of-kubernetes-for-azure-files"></a>Azure dosyaları için Kubernetes'in önerilen kararlı sürümleri nelerdir?
 
| Kubernetes sürümü | Önerilen sürüm |
| -- | :--: |
| 1.12 | 1.12.6 veya sonrası |
| 1.13 | 1.13.4 veya sonrası |
| 1.14 | 1.14.0 veya sonrası |

### <a name="what-versions-of-kubernetes-have-azure-files-support-on-the-sovereign-cloud"></a>Egemen Bulut'ta Kubernetes'in hangi sürümleriazure files desteğine sahiptir?

| Kubernetes sürümü | Önerilen sürüm |
| -- | :--: |
| 1.12 | 1.12.0 veya sonrası |
| 1.13 | 1.13.0 veya sonrası |
| 1.14 | 1.14.0 veya sonrası |

### <a name="what-are-the-default-mountoptions-when-using-azure-files"></a>Azure Dosyaları kullanırken varsayılan montaj Seçenekleri nelerdir?

Önerilen ayarlar:

| Kubernetes sürümü | fileMode ve dirMode değeri|
| -- | :--: |
| 1.12.0 - 1.12.1 | 0755 |
| 1.12.2 ve sonrası | 0777 |

Kubernetes sürüm 1.8.5 veya daha büyük ve dinamik bir depolama sınıfı ile kalıcı birim oluşturma ile bir küme kullanıyorsanız, depolama sınıfı nesnesi üzerinde montaj seçenekleri belirtilebilir. Aşağıdaki örnek *0777*ayarlar:

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
  - mfsymlinks
  - nobrl
  - cache=none
parameters:
  skuName: Standard_LRS
```

Bazı ek kullanışlı *mountOptions* ayarları:

* *mfsymlinks* Azure Files montaj (cifs) sembolik bağlantıları destek yapacak
* *nobrl* sunucuya byte aralığı kilit istekleri göndermesini engeller. Bu ayar, cifs stili zorunlu bayt aralığı kilitleri ile kırmak bazı uygulamalar için gereklidir. Çoğu cifs sunucusu henüz danışma byte aralığı kilitleri isteyen destek yok. *Nobrl*kullanmıyorsanız, cifs stili zorunlu bayt aralığı kilitleri ile kırılabilen uygulamalar benzer hata iletilerine neden olabilir:
    ```console
    Error: SQLITE_BUSY: database is locked
    ```

### <a name="error-could-not-change-permissions-when-using-azure-files"></a>Azure Dosyaları kullanırken "izinleri değiştiremedi" hatası

Azure Dosyaları eklentisinde PostgreSQL çalıştırırken aşağıdakilere benzer bir hata görebilirsiniz:

```console
initdb: could not change permissions of directory "/var/lib/postgresql/data": Operation not permitted
fixing permissions on existing directory /var/lib/postgresql/data
```

Bu hata, cifs/SMB protokolünü kullanarak Azure Files eklentisi tarafından kaynaklanır. CIFS/SMB protokolü kullanırken, dosya ve dizin izinleri montajdan sonra değiştirilemezdi.

Bu sorunu gidermek için Azure Disk eklentisi ile birlikte *subPath'i* kullanın. 

> [!NOTE] 
> Ext3/4 disk türü için, disk biçimlendirindikten sonra kayıp+bulunan dizin vardır.

### <a name="azure-files-has-high-latency-compared-to-azure-disk-when-handling-many-small-files"></a>Azure Dosyaları, birçok küçük dosyayı işlerken Azure Disk'e kıyasla yüksek gecikme gecikmesine sahiptir

Birçok küçük dosyayı işlemek gibi bazı durumlarda, Azure Disk'le karşılaştırıldığında Azure Dosyalarını kullanırken yüksek gecikme cezası yla karşılaşabilirsiniz.

### <a name="error-when-enabling-allow-access-allow-access-from-selected-network-setting-on-storage-account"></a>Depolama hesabında "Seçili ağdan erişime izin ver" ayarını etkinleştirirken hata

AKS'de dinamik sağlama için kullanılan bir depolama hesabında *seçili ağdan erişime izin* verirseniz, AKS bir dosya paylaşımı oluşturduğunda bir hata alırsınız:

```console
persistentvolume-controller (combined from similar events): Failed to provision volume with StorageClass "azurefile": failed to create share kubernetes-dynamic-pvc-xxx in account xxx: failed to create file share, err: storage: service returned error: StatusCode=403, ErrorCode=AuthorizationFailure, ErrorMessage=This request is not authorized to perform this operation.
```

Bu hata, *Seçili ağdan erişime izin verirken*Seçilen ağda kubernetes *kalıcı ses denetleyicisinin* olmamasından kaynaklanmaktadır.

[Azure Dosyaları ile statik sağlama](azure-files-volume.md)yı kullanarak sorunu azaltabilirsiniz.

### <a name="azure-files-fails-to-remount-in-windows-pod"></a>Azure Dosyaları Windows bölmesinde yeniden monte edilemede başarısız

Azure Dosyaları yuvasına sahip bir Windows bölmesi silinir ve aynı düğümde yeniden oluşturulacak şekilde zamanlanırsa, montaj başarısız olur. Bu hata, Azure `New-SmbGlobalMapping` Dosyaları bineği düğüme zaten monte edilmiş olduğundan komutun başarısız olmasıdır.

Örneğin, benzer bir hata görebilirsiniz:

```console
E0118 08:15:52.041014    2112 nestedpendingoperations.go:267] Operation for "\"kubernetes.io/azure-file/42c0ea39-1af9-11e9-8941-000d3af95268-pvc-d7e1b5f9-1af3-11e9-8941-000d3af95268\" (\"42c0ea39-1af9-11e9-8941-000d3af95268\")" failed. No retries permitted until 2019-01-18 08:15:53.0410149 +0000 GMT m=+732.446642701 (durationBeforeRetry 1s). Error: "MountVolume.SetUp failed for volume \"pvc-d7e1b5f9-1af3-11e9-8941-000d3af95268\" (UniqueName: \"kubernetes.io/azure-file/42c0ea39-1af9-11e9-8941-000d3af95268-pvc-d7e1b5f9-1af3-11e9-8941-000d3af95268\") pod \"deployment-azurefile-697f98d559-6zrlf\" (UID: \"42c0ea39-1af9-11e9-8941-000d3af95268\") : azureMount: SmbGlobalMapping failed: exit status 1, only SMB mount is supported now, output: \"New-SmbGlobalMapping : Generic failure \\r\\nAt line:1 char:190\\r\\n+ ... ser, $PWord;New-SmbGlobalMapping -RemotePath $Env:smbremotepath -Cred ...\\r\\n+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~\\r\\n    + CategoryInfo          : NotSpecified: (MSFT_SmbGlobalMapping:ROOT/Microsoft/...mbGlobalMapping) [New-SmbGlobalMa \\r\\n   pping], CimException\\r\\n    + FullyQualifiedErrorId : HRESULT 0x80041001,New-SmbGlobalMapping\\r\\n \\r\\n\""
```

Bu sorun Kubernetes aşağıdaki sürümlerinde giderilmiştir:

| Kubernetes sürümü | Sabit sürüm |
| -- | :--: |
| 1.12 | 1.12.6 veya sonrası |
| 1.13 | 1.13.4 veya sonrası |
| 1.14 ve sonrası | Yok |

### <a name="azure-files-mount-fails-due-to-storage-account-key-changed"></a>Depolama hesabı anahtarının değişmesi nedeniyle Azure Files montajı başarısız oldu

Depolama hesabı anahtarınız değiştiyse, Azure Dosyaları montaj hataları görebilirsiniz.

Temel 64 kodlanmış depolama hesabı anahtarınızla Azure dosyasında *azurestorageaccountkey* alanını el ile güncelleştirerek sorunu azaltabilirsiniz.

Base64'teki depolama hesabı anahtarınızı kodlamak `base64`için . Örnek:

```console
echo X+ALAAUgMhWHL7QmQ87E1kSfIqLKfgC03Guy7/xk9MyIg2w4Jzqeu60CVw2r/dm6v6E0DWHTnJUEJGVQAoPaBc== | base64
```

Azure gizli dosyanızı güncelleştirmek için `kubectl edit secret`. Örnek:

```console
kubectl edit secret azure-storage-account-{storage-account-name}-secret
```

Birkaç dakika sonra aracı düğümü, güncelleştirilmiş depolama anahtarıyla azure dosya montajını yeniden dener.

### <a name="cluster-autoscaler-fails-to-scale-with-error-failed-to-fix-node-group-sizes"></a>Küme otomatik ölçeklendirici düğüm grubu boyutlarını düzeltmek için başarısız hata ile ölçeklendirmek için başarısız

Küme otomatik ölçeklendirici yukarı / aşağı ölçekleme değilse ve küme [otomatik ölçeklendirici günlükleri][view-master-logs]aşağıdaki gibi bir hata görürseniz.

```console
E1114 09:58:55.367731 1 static_autoscaler.go:239] Failed to fix node group sizes: failed to decrease aks-default-35246781-vmss: attempt to delete existing nodes
```

Bu hata, küme otomatik ölçekleyicisinin kümedeki nden farklı bir değerle sona erdiği yukarı akışlı küme otomatik ölçeklendirici yarış koşulundan kaynaklanır. Bu durumdan çıkmak için [küme otomatik ölçekleyicisini][cluster-autoscaler]devre dışı bırakıp yeniden etkinleştirmenizi yeterlidir.

### <a name="slow-disk-attachment-getazuredisklun-takes-10-to-15-minutes-and-you-receive-an-error"></a>Yavaş disk eki, GetAzureDiskLun 10 ila 15 dakika sürer ve bir hata alırsınız

**1.15.0'dan büyük** Kubernetes sürümlerinde Hata **WaitForAttach disk için Lun bulamıyor**gibi bir hata alabilirsiniz.  Bunun geçici çözüm yaklaşık 15 dakika beklemek ve yeniden denemektir.

<!-- LINKS - internal -->
[view-master-logs]: view-master-logs.md
[cluster-autoscaler]: cluster-autoscaler.md
