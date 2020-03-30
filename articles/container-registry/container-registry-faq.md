---
title: Sık sorulan sorular
description: Azure Konteyner Kayıt Defteri hizmetiyle ilgili sık sorulan soruların yanıtları
author: sajayantony
ms.topic: article
ms.date: 07/02/2019
ms.author: sajaya
ms.openlocfilehash: c0d51c9c31e4e6859eaedce371efeafaa5fd4f46
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78403216"
---
# <a name="frequently-asked-questions-about-azure-container-registry"></a>Azure Konteyner Kayıt Defteri hakkında sık sorulan sorular

Bu makalede, Azure Kapsayıcı Kayıt Defteri ile ilgili sık sorulan sorular ve bilinen sorunlar giderir.

## <a name="resource-management"></a>Kaynak yönetimi

- [Kaynak Yöneticisi şablonu kullanarak bir Azure kapsayıcı kayıt defteri oluşturabilir miyim?](#can-i-create-an-azure-container-registry-using-a-resource-manager-template)
- [ACR'deki görüntüler için güvenlik açığı var mı?](#is-there-security-vulnerability-scanning-for-images-in-acr)
- [Azure Konteyner Kayıt Defteri ile Kubernetes'i nasıl yapılandırırım?](#how-do-i-configure-kubernetes-with-azure-container-registry)
- [Konteyner kayıt defteri için yönetici kimlik bilgilerini nasıl alabilirim?](#how-do-i-get-admin-credentials-for-a-container-registry)
- [Kaynak Yöneticisi şablonunda yönetici kimlik bilgilerini nasıl alabilirim?](#how-do-i-get-admin-credentials-in-a-resource-manager-template)
- [Çoğaltma, Azure CLI veya Azure PowerShell kullanılarak silinse de Yasak durumunda başarısız olur](#delete-of-replication-fails-with-forbidden-status-although-the-replication-gets-deleted-using-the-azure-cli-or-azure-powershell)
- [Güvenlik duvarı kuralları başarıyla güncelleştirilir, ancak bunlar etkili olmaz](#firewall-rules-are-updated-successfully-but-they-do-not-take-effect)

### <a name="can-i-create-an-azure-container-registry-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak bir Azure Kapsayıcı Kayıt Defteri oluşturabilir miyim?

Evet. Burada, bir kayıt defteri oluşturmak için kullanabileceğiniz bir [şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-container-registry) verilmiştir.

### <a name="is-there-security-vulnerability-scanning-for-images-in-acr"></a>ACR'deki görüntüler için güvenlik açığı var mı?

Evet. Azure Güvenlik [Merkezi,](https://docs.microsoft.com/azure/security-center/azure-container-registry-integration) [Twistlock](https://www.twistlock.com/2016/11/07/twistlock-supports-azure-container-registry/) ve [Aqua'daki](https://blog.aquasec.com/image-vulnerability-scanning-in-azure-container-registry)belgelere bakın.

### <a name="how-do-i-configure-kubernetes-with-azure-container-registry"></a>Azure Konteyner Kayıt Defteri ile Kubernetes'i nasıl yapılandırırım?

[Azure Kubernetes Hizmeti](../aks/cluster-container-registry-integration.md)için [Kubernetes](https://kubernetes.io/docs/user-guide/images/#using-azure-container-registry-acr) için belgelere ve adımlara bakın.

### <a name="how-do-i-get-admin-credentials-for-a-container-registry"></a>Konteyner kayıt defteri için yönetici kimlik bilgilerini nasıl alabilirim?

> [!IMPORTANT]
> Yönetici kullanıcı hesabı, tek bir kullanıcının kayıt defterine erişebilmek için tasarlanmıştır, özellikle sınama amacıyla. Yönetici hesabı kimlik bilgilerini birden çok kullanıcıyla paylaşmanızı önermiyoruz. Başsız senaryolar için kullanıcılar ve hizmet ilkeleri için bireysel kimlik önerilir. Bkz. [Kimlik Doğrulama genel bakışı.](container-registry-authentication.md)

Yönetici kimlik bilgilerini almadan önce, kayıt defterinin yönetici kullanıcısının etkin olduğundan emin olun.

Azure CLI'yi kullanarak kimlik bilgileri almak için:

```azurecli
az acr credential show -n myRegistry
```

Azure Powershell'i kullanma:

```powershell
Invoke-AzureRmResourceAction -Action listCredentials -ResourceType Microsoft.ContainerRegistry/registries -ResourceGroupName myResourceGroup -ResourceName myRegistry
```

### <a name="how-do-i-get-admin-credentials-in-a-resource-manager-template"></a>Kaynak Yöneticisi şablonunda yönetici kimlik bilgilerini nasıl alabilirim?

> [!IMPORTANT]
> Yönetici kullanıcı hesabı, tek bir kullanıcının kayıt defterine erişebilmek için tasarlanmıştır, özellikle sınama amacıyla. Yönetici hesabı kimlik bilgilerini birden çok kullanıcıyla paylaşmanızı önermiyoruz. Başsız senaryolar için kullanıcılar ve hizmet ilkeleri için bireysel kimlik önerilir. Bkz. [Kimlik Doğrulama genel bakışı.](container-registry-authentication.md)

Yönetici kimlik bilgilerini almadan önce, kayıt defterinin yönetici kullanıcısının etkin olduğundan emin olun.

İlk parolayı almak için:

```json
{
    "password": "[listCredentials(resourceId('Microsoft.ContainerRegistry/registries', 'myRegistry'), '2017-10-01').passwords[0].value]"
}
```

İkinci parolayı almak için:

```json
{
    "password": "[listCredentials(resourceId('Microsoft.ContainerRegistry/registries', 'myRegistry'), '2017-10-01').passwords[1].value]"
}
```

### <a name="delete-of-replication-fails-with-forbidden-status-although-the-replication-gets-deleted-using-the-azure-cli-or-azure-powershell"></a>Çoğaltma, Azure CLI veya Azure PowerShell kullanılarak silinse de Yasak durumunda başarısız olur

Hata, kullanıcının bir kayıt defterinde izinleri olduğunda ancak abonelikte Okuyucu düzeyinde izinleri olmadığında görülür. Bu sorunu gidermek için, kullanıcıya abonelik le ilgili Okuyucu izinleri atayın:


```azurecli  
az role assignment create --role "Reader" --assignee user@contoso.com --scope /subscriptions/<subscription_id> 
```

### <a name="firewall-rules-are-updated-successfully-but-they-do-not-take-effect"></a>Güvenlik duvarı kuralları başarıyla güncelleştirilir, ancak bunlar etkili olmaz

Güvenlik duvarı kuralı değişikliklerini yaymak biraz zaman alır. Güvenlik duvarı ayarlarını değiştirdikten sonra, lütfen bu değişikliği doğrulamadan önce birkaç dakika bekleyin.


## <a name="registry-operations"></a>Kayıt defteri işlemleri

- [Docker Registry HTTP API V2'ye nasıl erişebilirim?](#how-do-i-access-docker-registry-http-api-v2)
- [Bir depodaki herhangi bir etikettarafından başvurulmayan tüm bildirimleri nasıl silerim?](#how-do-i-delete-all-manifests-that-are-not-referenced-by-any-tag-in-a-repository)
- [Resimleri silen kayıt defteri kotası kullanımı neden azaltmıyor?](#why-does-the-registry-quota-usage-not-reduce-after-deleting-images)
- [Depolama kotası değişikliklerini nasıl doğrularım?](#how-do-i-validate-storage-quota-changes)
- [CLI'yi bir kapta çalıştırırken kayıt defterimle nasıl kimlik doğrularım?](#how-do-i-authenticate-with-my-registry-when-running-the-cli-in-a-container)
- [TLS 1.2 nasıl etkinleştirilir?](#how-to-enable-tls-12)
- [Azure Kapsayıcı Kayıt Defteri İçerik Güvenini destekliyor mu?](#does-azure-container-registry-support-content-trust)
- [Kayıt defteri kaynağını yönetmek için izin almadan görüntüleri çekme veya itme iznine nasıl erişebilirim?](#how-do-i-grant-access-to-pull-or-push-images-without-permission-to-manage-the-registry-resource)
- [Bir kayıt defteri için otomatik görüntü karantinasını nasıl etkinleştirebilirim?](#how-do-i-enable-automatic-image-quarantine-for-a-registry)

### <a name="how-do-i-access-docker-registry-http-api-v2"></a>Docker Registry HTTP API V2'ye nasıl erişebilirim?

ACR Docker Registry HTTP API V2 destekler. API'lara ' dan `https://<your registry login server>/v2/`erişilebilir. Örnek: `https://mycontainerregistry.azurecr.io/v2/`

### <a name="how-do-i-delete-all-manifests-that-are-not-referenced-by-any-tag-in-a-repository"></a>Bir depodaki herhangi bir etikettarafından başvurulmayan tüm bildirimleri nasıl silerim?

Eğer bash iseniz:

```azurecli
az acr repository show-manifests -n myRegistry --repository myRepository --query "[?tags[0]==null].digest" -o tsv  | xargs -I% az acr repository delete -n myRegistry -t myRepository@%
```

Powershell için:

```azurecli
az acr repository show-manifests -n myRegistry --repository myRepository --query "[?tags[0]==null].digest" -o tsv | %{ az acr repository delete -n myRegistry -t myRepository@$_ }
```

Not: Onay `-y` atlamak için silme komutunu ekleyebilirsiniz.

Daha fazla bilgi için Azure [Kapsayıcı Kayıt Defteri'ndeki kapsayıcı görüntülerini sil'e](container-registry-delete.md)bakın.

### <a name="why-does-the-registry-quota-usage-not-reduce-after-deleting-images"></a>Resimleri silen kayıt defteri kotası kullanımı neden azaltmıyor?

Bu durum, altta yatan katmanlar hala diğer kapsayıcı görüntüleri tarafından başvurulan ediliyorsa gerçekleşebilir. Referansı olmayan bir resmi silerseniz, kayıt defteri kullanımı birkaç dakika içinde güncellenir.

### <a name="how-do-i-validate-storage-quota-changes"></a>Depolama kotası değişikliklerini nasıl doğrularım?

Aşağıdaki docker dosyasını kullanarak 1GB katmanı olan bir resim oluşturun. Bu, görüntünün kayıt defterindeki başka bir görüntü tarafından paylaşılmayan bir katmana sahip olmasını sağlar.

```dockerfile
FROM alpine
RUN dd if=/dev/urandom of=1GB.bin  bs=32M  count=32
RUN ls -lh 1GB.bin
```

Docker CLI'yi kullanarak görüntüyü oluşturun ve kayıt defterinize itin.

```bash
docker build -t myregistry.azurecr.io/1gb:latest .
docker push myregistry.azurecr.io/1gb:latest
```

Azure portalında depolama alanı kullanımının arttığını görebilmelidir veya CLI'yi kullanarak kullanımı sorgulayabilirsiniz.

```azurecli
az acr show-usage -n myregistry
```

Azure CLI veya portalını kullanarak görüntüyü silin ve birkaç dakika içinde güncelleştirilmiş kullanımı kontrol edin.

```azurecli
az acr repository delete -n myregistry --image 1gb
```

### <a name="how-do-i-authenticate-with-my-registry-when-running-the-cli-in-a-container"></a>CLI'yi bir kapta çalıştırırken kayıt defterimle nasıl kimlik doğrularım?

Docker soketini monte ederek Azure CLI kapsayıcısını çalıştırmanız gerekir:

```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock azuresdk/azure-cli-python:dev
```

Konteynerde, yükleyin: `docker`

```bash
apk --update add docker
```

Ardından kayıt defterinizle kimlik doğrulaması:

```azurecli
az acr login -n MyRegistry
```

### <a name="how-to-enable-tls-12"></a>TLS 1.2 nasıl etkinleştirilir?

TLS 1.2'yi yeni bir docker istemcisi (sürüm 18.03.0 ve üzeri) kullanarak etkinleştirin. 

> [!IMPORTANT]
> Azure Konteyner Kayıt Defteri, 13 Ocak 2020'den itibaren sunuculardan ve uygulamalardan tls 1.2 kullanmak için tüm güvenli bağlantıları gerektirir. TLS 1.0 ve 1.1 desteği emekli yedirilecektir.

### <a name="does-azure-container-registry-support-content-trust"></a>Azure Kapsayıcı Kayıt Defteri İçerik Güvenini destekliyor mu?

Evet, [Docker Notary](https://docs.docker.com/notary/getting_started/) tümleşik olduğundan ve etkinleştirilebildiği için Azure Konteyner Kayıt Defteri'nde güvenilir görüntüleri kullanabilirsiniz. Ayrıntılar için [Azure Kapsayıcı Kayıt Defteri'nde İçerik Güveni'ne](container-registry-content-trust.md)bakın.


####  <a name="where-is-the-file-for-the-thumbprint-located"></a>Parmak izinin dosyası nerede?

Altında `~/.docker/trust/tuf/myregistry.azurecr.io/myrepository/metadata`:

* Tüm rollerin ortak anahtarları ve sertifikaları (temsilci rolleri `root.json`hariç) .
* Temsilci rolünün ortak anahtarları ve sertifikaları, ana rolünün JSON dosyasında `targets.json` depolanır (örneğin `targets/releases` rol için).

Docker ve Noter istemcisi tarafından yapılan genel TUF doğrulamasonra bu ortak anahtarları ve sertifikaları doğrulamak için önerilir.

### <a name="how-do-i-grant-access-to-pull-or-push-images-without-permission-to-manage-the-registry-resource"></a>Kayıt defteri kaynağını yönetmek için izin almadan görüntüleri çekme veya itme iznine nasıl erişebilirim?

ACR, farklı izin düzeyleri sağlayan [özel rolleri](container-registry-roles.md) destekler. Özellikle `AcrPull` ve `AcrPush` roller, kullanıcıların Azure'daki kayıt defteri kaynağını yönetme izni olmadan görüntüleri çekmelerine ve/veya itmelerine olanak tanır.

* Azure portalı: Kayıt > Erişim Denetimi (IAM) -> Ekle (Rolü seçin `AcrPull` veya `AcrPush` için seçin).
* Azure CLI: Aşağıdaki komutu çalıştırarak kayıt defterinin kaynak kimliğini bulun:

  ```azurecli
  az acr show -n myRegistry
  ```
  
  Daha sonra bir `AcrPull` kullanıcıya veya `AcrPush` rolü atayabilirsiniz `AcrPull`(aşağıdaki örnek kullanır):

  ```azurecli
  az role assignment create --scope resource_id --role AcrPull --assignee user@example.com
  ```

  Veya, rolü uygulama kimliğiyle tanımlanan bir hizmet ilkesine atamak:

  ```azurecli
  az role assignment create --scope resource_id --role AcrPull --assignee 00000000-0000-0000-0000-000000000000
  ```

Atanın kimliği doğrulanabilir ve kayıt defterindeki görüntülere erişebilir.

* Bir kayıt defterine kimlik doğrulamak için:
    
  ```azurecli
  az acr login -n myRegistry 
  ```

* Depoları listelemek için:

  ```azurecli
  az acr repository list -n myRegistry
  ```

* Görüntü çekmek için:

  ```bash
  docker pull myregistry.azurecr.io/hello-world
  ```

Yalnızca rolün `AcrPull` veya `AcrPush` rolün kullanımıyla, devralan kişi Azure'daki kayıt defteri kaynağını yönetme iznine sahip değildir. Örneğin, `az acr list` veya `az acr show -n myRegistry` kayıt defterini göstermez.

### <a name="how-do-i-enable-automatic-image-quarantine-for-a-registry"></a>Bir kayıt defteri için otomatik görüntü karantinasını nasıl etkinleştirebilirim?

Görüntü karantinası şu anda ACR bir önizleme özelliğidir. Yalnızca güvenlik tadına başarıyla geçmiş görüntülerin normal kullanıcılar tarafından görülebilmesi için bir kayıt defterinin karantina modunu etkinleştirebilirsiniz. Ayrıntılar için [ACR GitHub repo'ya](https://github.com/Azure/acr/tree/master/docs/preview/quarantine)bakın.

## <a name="diagnostics-and-health-checks"></a>Teşhis ve sağlık kontrolleri

- [Durumu kontrol edin`az acr check-health`](#check-health-with-az-acr-check-health)
- [docker pull hatası ile başarısız olur: net/http: bağlantı beklerken istek iptal edildi (Client.Timeout üstbilgi beklerken aşıldı)](#docker-pull-fails-with-error-nethttp-request-canceled-while-waiting-for-connection-clienttimeout-exceeded-while-awaiting-headers)
- [docker itme başarılı olur ancak docker çekme hatası ile başarısız olur: yetkisiz: kimlik doğrulama gerekli](#docker-push-succeeds-but-docker-pull-fails-with-error-unauthorized-authentication-required)
- [`az acr login`başarılı olur, ancak docker komutları hata ile başarısız olur: yetkisiz: kimlik doğrulama gerekli](#az-acr-login-succeeds-but-docker-fails-with-error-unauthorized-authentication-required)
- [Docker daemon'un hata ayıklama günlüklerini etkinleştirin ve alın](#enable-and-get-the-debug-logs-of-the-docker-daemon) 
- [Yeni kullanıcı izinleri güncelleştirilmeden hemen sonra etkili olmayabilir](#new-user-permissions-may-not-be-effective-immediately-after-updating)
- [Kimlik doğrulama bilgileri doğrudan REST API aramalarında doğru biçimde verilmez](#authentication-information-is-not-given-in-the-correct-format-on-direct-rest-api-calls)
- [Azure portalı neden tüm depolarımı veya etiketlerimi listelemiyor?](#why-does-the-azure-portal-not-list-all-my-repositories-or-tags)
- [Azure portalı neden depo veya etiket getirmiyor?](#why-does-the-azure-portal-fail-to-fetch-repositories-or-tags)
- [Çekme veya itme isteğim neden izin verilmeyen işlemle başarısız oluyor?](#why-does-my-pull-or-push-request-fail-with-disallowed-operation)
- [Windows'daki http izlerini nasıl toplarım?](#how-do-i-collect-http-traces-on-windows)

### <a name="check-health-with-az-acr-check-health"></a>Durumu kontrol edin`az acr check-health`

Sık karşılaşılan ortam ve kayıt defteri sorunlarını gidermek için [bkz.](container-registry-check-health.md)

### <a name="docker-pull-fails-with-error-nethttp-request-canceled-while-waiting-for-connection-clienttimeout-exceeded-while-awaiting-headers"></a>docker pull hatası ile başarısız olur: net/http: bağlantı beklerken istek iptal edildi (Client.Timeout üstbilgi beklerken aşıldı)

 - Bu hata geçici bir sorunsa, yeniden deneme başarılı olacaktır.
 - Sürekli `docker pull` başarısız olursa, docker daemon ile ilgili bir sorun olabilir. Sorun genellikle Docker daemon yeniden başlatılarak azaltılabilir. 
 - Docker daemon'u yeniden başladıktan sonra bu sorunu görmeye devam ederseniz, sorun makineyle ilgili bazı ağ bağlantısı sorunları olabilir. Makinedeki genel ağın sağlıklı olup olmadığını kontrol etmek için, uç nokta bağlantısını test etmek için aşağıdaki komutu çalıştırın. Bu `az acr` bağlantı denetimi komutunu içeren minimum sürüm 2.2.9'dur. Eski bir sürüm kullanıyorsanız Azure CLI'nizi yükseltin.
 
  ```azurecli
  az acr check-health -n myRegistry
  ```

 - Her zaman tüm Docker istemci işlemlerinde bir yeniden deneme mekanizması olmalıdır.

### <a name="docker-pull-is-slow"></a>Docker çekme yavaş
Makine ağı indirme hızınızı test etmek için [bu](http://www.azurespeed.com/Azure/Download) aracı kullanın. Makine ağı yavaşsa, kayıt defterinizle aynı bölgede Azure VM kullanmayı düşünün. Bu genellikle daha hızlı ağ hızı sağlar.

### <a name="docker-push-is-slow"></a>Docker itme yavaş
Makine ağı yükleme hızınızı test etmek için [bu](http://www.azurespeed.com/Azure/Upload) aracı kullanın. Makine ağı yavaşsa, kayıt defterinizle aynı bölgede Azure VM kullanmayı düşünün. Bu genellikle daha hızlı ağ hızı sağlar.

### <a name="docker-push-succeeds-but-docker-pull-fails-with-error-unauthorized-authentication-required"></a>Docker itme başarılı olur ancak docker çekme hatası ile başarısız olur: yetkisiz: kimlik doğrulama gerekli

Bu hata, varsayılan olarak `--signature-verification` etkinleştirilen Docker daemon'un Red Hat sürümünde olabilir. Red Hat Enterprise Linux (RHEL) veya Fedora için Docker daemon seçeneklerini aşağıdaki komutu çalıştırarak kontrol edebilirsiniz:

```bash
grep OPTIONS /etc/sysconfig/docker
```

Örneğin, Fedora 28 Server aşağıdaki docker daemon seçenekleri vardır:

`OPTIONS='--selinux-enabled --log-driver=journald --live-restore'`

Eksik, `--signature-verification=false` `docker pull` benzer bir hata ile başarısız olur:

```output
Trying to pull repository myregistry.azurecr.io/myimage ...
unauthorized: authentication required
```

Hatayı gidermek için:
1. Seçeneği `--signature-verification=false` Docker daemon yapılandırma dosyasına `/etc/sysconfig/docker`ekleyin. Örnek:
   
   `OPTIONS='--selinux-enabled --log-driver=journald --live-restore --signature-verification=false'`
   
2. Docker daemon hizmetini aşağıdaki komutu çalıştırarak yeniden başlatın:
   
   ```bash
   sudo systemctl restart docker.service
   ```

Ayrıntıları `--signature-verification` çalıştırarak `man dockerd`bulunabilir.

### <a name="az-acr-login-succeeds-but-docker-fails-with-error-unauthorized-authentication-required"></a>az acr giriş başarılı olur ancak docker hata ile başarısız olur: yetkisiz: kimlik doğrulama gerekli

Örneğin, `docker push myregistry.azurecr.io/myimage:latest`kayıt defteri kaynak adı büyük harf veya karışık durumda olsa bile, tüm küçük `myRegistry`bir sunucu URL'sini kullandığınızdan emin olun.

### <a name="enable-and-get-the-debug-logs-of-the-docker-daemon"></a>Docker daemon'un hata ayıklama günlüklerini etkinleştirin ve alın  

`debug` Seçenekle başla. `dockerd` İlk olarak, Docker daemon`/etc/docker/daemon.json`yapılandırma dosyası ( ) yoksa oluşturun `debug` ve seçeneği ekleyin:

```json
{   
    "debug": true   
}
```

Sonra daemon'u yeniden başlat. Örneğin, Ubuntu 14.04 ile:

```bash
sudo service docker restart
```

Ayrıntılar [Docker belgelerinde](https://docs.docker.com/engine/admin/#enable-debugging)bulunabilir. 

 * Günlükler sisteminize bağlı olarak farklı konumlarda oluşturulabilir. Örneğin, Ubuntu 14.04 için. `/var/log/upstart/docker.log`   
Ayrıntılar için [Docker belgelerine](https://docs.docker.com/engine/admin/#read-the-logs) bakın.    

 * Windows için Docker için günlükler %LOCALAPPDATA%/docker/ altında oluşturulur. Ancak henüz tüm hata ayıklama bilgilerini içermeyebilir.   

   Tam daemon günlüğüne erişmek için bazı ek adımlar gerekebilir:

    ```console
    docker run --privileged -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v /usr/local/bin/docker:/usr/local/bin/docker alpine sh

    docker run --net=host --ipc=host --uts=host --pid=host -it --security-opt=seccomp=unconfined --privileged --rm -v /:/host alpine /bin/sh
    chroot /host
    ```
    Şimdi vm çalışan `dockerd`tüm dosyalara erişim var. Günlük şu `/var/log/docker.log`anda.

### <a name="new-user-permissions-may-not-be-effective-immediately-after-updating"></a>Yeni kullanıcı izinleri güncelleştirilmeden hemen sonra etkili olmayabilir

Bir hizmet sorumlusuna yeni izinler (yeni roller) verdiyseniz, değişiklik hemen etkili olmayabilir. İki olası nedeni vardır:

* Azure Active Directory rol ataması gecikmesi. Normalde hızlıdır, ama yayılma gecikmesi nedeniyle dakikalar sürebilir.
* ACR belirteç sunucusunda izin gecikmesi. Bu işlem 10 dakika kadar sürebilir. Azaltmak için, 1 dakika `docker logout` sonra aynı kullanıcıyla yeniden kimlik doğrulaması yapabilirsiniz:

  ```bash
  docker logout myregistry.azurecr.io
  docker login myregistry.azurecr.io
  ```

Şu anda ACR, kullanıcılar tarafından ev çoğaltma silme işlemini desteklemez. Geçici çözüm, şablona ev çoğaltma oluşturmak eklemek için, ancak `"condition": false` aşağıda gösterildiği gibi ekleyerek oluşturma atlamak:

```json
{
    "name": "[concat(parameters('acrName'), '/', parameters('location'))]",
    "condition": false,
    "type": "Microsoft.ContainerRegistry/registries/replications",
    "apiVersion": "2017-10-01",
    "location": "[parameters('location')]",
    "properties": {},
    "dependsOn": [
        "[concat('Microsoft.ContainerRegistry/registries/', parameters('acrName'))]"
     ]
},
```

### <a name="authentication-information-is-not-given-in-the-correct-format-on-direct-rest-api-calls"></a>Kimlik doğrulama bilgileri doğrudan REST API aramalarında doğru biçimde verilmez

`InvalidAuthenticationInfo` Özellikle `curl` aracı kullanarak `-L`bir hatayla karşılaşabilirsiniz `--location` , (yönlendirmeleri takip etmek).
Örneğin, seçenek ve temel `curl` kimlik `-L` doğrulaması kullanarak blob alma:

```bash
curl -L -H "Authorization: basic $credential" https://$registry.azurecr.io/v2/$repository/blobs/$digest
```

aşağıdaki yanıta neden olabilir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Error><Code>InvalidAuthenticationInfo</Code><Message>Authentication information is not given in the correct format. Check the value of Authorization header.
RequestId:00000000-0000-0000-0000-000000000000
Time:2019-01-01T00:00:00.0000000Z</Message></Error>
```

Temel nedeni, bazı `curl` uygulamaların özgün istek üstbilgi ile yönlendirmeleri izlemesidir.

Sorunu çözmek için, üstbilgi olmadan yeniden yönlendirmeleri el ile izlemeniz gerekir. Yanıt üstbilgisini `-D -` seçeneğiyle `curl` yazdırın ve `Location` sonra ayıklayın: üstbilgi:

```bash
redirect_url=$(curl -s -D - -H "Authorization: basic $credential" https://$registry.azurecr.io/v2/$repository/blobs/$digest | grep "^Location: " | cut -d " " -f2 | tr -d '\r')
curl $redirect_url
```

### <a name="why-does-the-azure-portal-not-list-all-my-repositories-or-tags"></a>Azure portalı neden tüm depolarımı veya etiketlerimi listelemiyor? 

Microsoft Edge/IE tarayıcısını kullanıyorsanız, en fazla 100 depo veya etiket görebilirsiniz. Kayıt defterinizde 100'den fazla depo veya etiket varsa, bunların hepsini listelemek için Firefox veya Chrome tarayıcısını kullanmanızı öneririz.

### <a name="why-does-the-azure-portal-fail-to-fetch-repositories-or-tags"></a>Azure portalı neden depo veya etiket getirmiyor?

Tarayıcı, depo veya etiket getirme isteğini sunucuya gönderemeyebilir. Bunun gibi çeşitli nedenleri olabilir:

* Ağ bağlantısı nın olmaması
* Güvenlik duvarı
* Reklam engelleyiciler
* DNS hataları

Lütfen ağ yöneticinize başvurun veya ağ yapılandırmanızı ve bağlantınızı kontrol edin. Ortamınızın `az acr check-health -n yourRegistry` Konteyner Kayıt Defteri'ne bağlanıp bağlanabildiğini kontrol etmek için Azure CLI'nizi çalıştırmayı deneyin. Buna ek olarak, herhangi bir eski tarayıcı önbelleği veya çerezleri önlemek için tarayıcınızda gizli veya özel bir oturum deneyebilirsiniz.

### <a name="why-does-my-pull-or-push-request-fail-with-disallowed-operation"></a>Çekme veya itme isteğim neden izin verilmeyen işlemle başarısız oluyor?

İşlemlerin izin verilmeyen bazı senaryoları aşağıda verilmiştir:
* Klasik kayıt defterleri artık desteklenmez. Lütfen [az acr güncellemesini](https://docs.microsoft.com/cli/azure/acr?view=azure-cli-latest#az-acr-update) veya Azure portalını kullanarak desteklenen bir [SNU'ya](https://aka.ms/acr/skus) yükseltin.
* Görüntü veya depo, silinmeyecek veya güncelleştirilemeyecek şekilde kilitlenebilir. Geçerli öznitelikleri görüntülemek için [az acr show deposu](https://docs.microsoft.com/azure/container-registry/container-registry-image-lock) komutunu kullanabilirsiniz.
* Görüntü karantinadaysa bazı işlemlere izin verilmez. [Karantina](https://github.com/Azure/acr/tree/master/docs/preview/quarantine)hakkında daha fazla bilgi edinin.

### <a name="how-do-i-collect-http-traces-on-windows"></a>Windows'daki http izlerini nasıl toplarım?

#### <a name="prerequisites"></a>Ön koşullar

- Kemancıda https'nin şifresini çözmeyi etkinleştirin:<https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS>
- Docker ui ile proxy kullanmasını etkinleştirin:<https://docs.docker.com/docker-for-windows/#proxies>
- Tamamlandığında geri dönmek için emin olun.  Docker bu etkin ve kemancı çalışmıyor çalışmaz çalışmaz.

#### <a name="windows-containers"></a>Windows kapsayıcıları

Docker proxy'sini 127.0.0.1:8888 olarak yapılandır

#### <a name="linux-containers"></a>Linux kapsayıcıları

Docker vm sanal anahtarının ip'ini bulun:

```powershell
(Get-NetIPAddress -InterfaceAlias "*Docker*" -AddressFamily IPv4).IPAddress
```

Docker proxy'sini önceki komutun ve 8888 bağlantı noktasının çıktısına göre yapılandırın (örneğin 10.0.75.1:8888)

## <a name="tasks"></a>Görevler

- [Toplu işlemi nasıl iptal ederim?](#how-do-i-batch-cancel-runs)
- [Az acr build komutuna .git klasörünü nasıl dahil edebilirim?](#how-do-i-include-the-git-folder-in-az-acr-build-command)
- [Görevler Kaynak tetikleyicileri için GitLab'ı destekliyor mu?](#does-tasks-support-gitlab-for-source-triggers)
- [Görevler hangi git deposu yönetim hizmetini destekler?](#what-git-repository-management-service-does-tasks-support)

### <a name="how-do-i-batch-cancel-runs"></a>Toplu işlemi nasıl iptal ederim?

Aşağıdaki komutlar belirtilen kayıt defterinde çalışan tüm görevleri iptal eder.

```azurecli
az acr task list-runs -r $myregistry --run-status Running --query '[].runId' -o tsv \
| xargs -I% az acr task cancel-run -r $myregistry --run-id %
```

### <a name="how-do-i-include-the-git-folder-in-az-acr-build-command"></a>Az acr build komutuna .git klasörünü nasıl dahil edebilirim?

Yerel bir kaynak klasörünü `az acr build` komuta `.git` geçirirseniz, klasör varsayılan olarak yüklenen paketin dışında tutulur. Aşağıdaki ayarı `.dockerignore` içeren bir dosya oluşturabilirsiniz. Yüklenen `.git` paketteki tüm dosyaları geri yüklemekomutunu söyler. 

`!.git/**`

Bu ayar `az acr run` komut için de geçerlidir.

### <a name="does-tasks-support-gitlab-for-source-triggers"></a>Görevler Kaynak tetikleyicileri için GitLab'ı destekliyor mu?

Şu anda Kaynak tetikleyiciler için GitLab destekliyoruz.

### <a name="what-git-repository-management-service-does-tasks-support"></a>Görevler hangi git deposu yönetim hizmetini destekler?

| Git hizmeti | Kaynak bağlam | Manuel yapı | Commit trigger ile otomatik yapı |
|---|---|---|---|
| GitHub | https://github.com/user/myapp-repo.git#mybranch:myfolder | Evet | Evet |
| Azure Repos | https://dev.azure.com/user/myproject/_git/myapp-repo#mybranch:myfolder | Evet | Evet |
| GitLab | https://gitlab.com/user/myapp-repo.git#mybranch:myfolder | Evet | Hayır |
| BitBucket | https://user@bitbucket.org/user/mayapp-repo.git#mybranch:myfolder | Evet | Hayır |

## <a name="run-error-message-troubleshooting"></a>Hata İletisi Sorun Giderme Çalıştır

| Hata iletisi | Sorun giderme kılavuzu |
|---|---|
|VM için erişim yapılandırıldı, bu nedenle abonelik bulunamadı|Bu, ACR Görevinizde kullanıyorsanız `az login --identity` gerçekleşebilir. Bu geçici bir hatadır ve Yönetilen Kimliğinizin rol ataması yayılmadığında oluşur. Yeniden denemeden önce birkaç saniye beklemek işe yarıyor.|

## <a name="cicd-integration"></a>CI/CD entegrasyonu

- [CircleCI](https://github.com/Azure/acr/blob/master/docs/integration/CircleCI.md)
- [GitHub Eylemleri](https://github.com/Azure/acr/blob/master/docs/integration/github-actions/github-actions.md)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Konteyner Kayıt Defteri hakkında [daha fazla bilgi edinin.](container-registry-intro.md)
