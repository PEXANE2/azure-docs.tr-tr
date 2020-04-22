---
title: Azure'da OpenShift Kapsayıcı Platformu 3.11 dağıtımında sorun giderme
description: Azure'da OpenShift Kapsayıcı Platformu 3.11 dağıtımında sorun giderme.
author: haroldwongms
manager: mdotson
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/14/2019
ms.author: haroldw
ms.openlocfilehash: 90fd3680cfdc4ecd1dcb0ce33b63f8d76dd8bfae
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759475"
---
# <a name="troubleshoot-openshift-container-platform-311-deployment-in-azure"></a>Azure'da OpenShift Kapsayıcı Platformu 3.11 dağıtımında sorun giderme

OpenShift kümesi başarıyla dağıtılamazsa, Azure portalı hata çıktısı sağlar. Çıktının okunması zor olabilir ve bu da sorunu tanımlamayı zorlaştırır. Çıkış kodu 3, 4 veya 5 için bu çıktıyı hızlıca tarar. Aşağıdaki üç çıkış kodu hakkında bilgi sağlar:

- Çıkış kodu 3: Red Hat Aboneliği Kullanıcı Adı / Şifre veya Kuruluş Kimliği / Etkinleştirme Anahtarı yanlış
- Çıkış kodu 4: Kırmızı Şapka Havuzu Kimliğiniz yanlış veya kullanılabilir hak yok
- Çıkış kodu 5: Docker İnce Havuz Hacmi sağlayamıyor

Diğer tüm çıkış kodları için, günlük dosyalarını görüntülemek için ssh üzerinden ana bilgisayara bağlanın.

**OpenShift Kapsayıcı Platformu 3.11**

SSH'den ansible playbook sunucusuna. Şablon veya Market teklifi için burç ana bilgisayarını kullanın. Burçtan, Kümedeki diğer tüm düğümlere (ana, infra, CNS, hesaplama) SSH yapabilirsiniz. Günlük dosyalarını görüntülemek için kök olmanız gerekir. Root varsayılan olarak SSH erişimi için devre dışı bırakılır, bu nedenle diğer düğümlere SSH kökünü kullanmayın.

**OKD**

SSH'den ansible playbook sunucusuna. OKD şablonu (sürüm 3.9 ve önceki sürüm) için ana-0 ana bilgisayar ını kullanın. OKD şablonu (sürüm 3.10 ve sonrası) için burç ana bilgisayarını kullanın. Ansible playbook ana bilgisayardan, SSH kümedeki diğer tüm düğümlere (ana, infra, CNS, işlem) yapabilirsiniz. Günlük dosyalarını görüntülemek için root (sudo su -) olmanız gerekir. Root varsayılan olarak SSH erişimi için devre dışı bırakılır, bu nedenle diğer düğümlere SSH kökünü kullanmayın.

## <a name="log-files"></a>Günlük dosyaları

Ana bilgisayar hazırlama komut dosyaları için günlük dosyaları (stderr ve `/var/lib/waagent/custom-script/download/0` stdout) tüm ana bilgisayarlarda bulunur. Ana bilgisayarın hazırlanması sırasında bir hata oluştuysa, hatayı belirlemek için bu günlük dosyalarını görüntüleyin.

Hazırlık komut dosyaları başarıyla çalıştırılırsa, ansible playbook ana bilgisayar `/var/lib/waagent/custom-script/download/1` dizinindeki günlük dosyalarının incelenmesi gerekir. Hata OpenShift gerçek yükleme sırasında oluştu, stdout dosyası hata görüntüler. Daha fazla yardım için Destek'e başvurmak için bu bilgileri kullanın.

Örnek çıktı

```json
TASK [openshift_storage_glusterfs : Load heketi topology] **********************
fatal: [mycluster-master-0]: FAILED! => {"changed": true, "cmd": ["oc", "--config=/tmp/openshift-glusterfs-ansible-IbhnUM/admin.kubeconfig", "rsh", "--namespace=glusterfs", "deploy-heketi-storage-1-d9xl5", "heketi-cli", "-s", "http://localhost:8080", "--user", "admin", "--secret", "VuoJURT0/96E42Vv8+XHfsFpSS8R20rH1OiMs3OqARQ=", "topology", "load", "--json=/tmp/openshift-glusterfs-ansible-IbhnUM/topology.json", "2>&1"], "delta": "0:00:21.477831", "end": "2018-05-20 02:49:11.912899", "failed": true, "failed_when_result": true, "rc": 0, "start": "2018-05-20 02:48:50.435068", "stderr": "", "stderr_lines": [], "stdout": "Creating cluster ... ID: 794b285745b1c5d7089e1c5729ec7cd2\n\tAllowing file volumes on cluster.\n\tAllowing block volumes on cluster.\n\tCreating node mycluster-cns-0 ... ID: 45f1a3bfc20a4196e59ebb567e0e02b4\n\t\tAdding device /dev/sdd ... OK\n\t\tAdding device /dev/sde ... OK\n\t\tAdding device /dev/sdf ... OK\n\tCreating node mycluster-cns-1 ... ID: 596f80d7bbd78a1ea548930f23135131\n\t\tAdding device /dev/sdc ... Unable to add device: Unable to execute command on glusterfs-storage-4zc42:   Device /dev/sdc excluded by a filter.\n\t\tAdding device /dev/sde ... OK\n\t\tAdding device /dev/sdd ... OK\n\tCreating node mycluster-cns-2 ... ID: 42c0170aa2799559747622acceba2e3f\n\t\tAdding device /dev/sde ... OK\n\t\tAdding device /dev/sdf ... OK\n\t\tAdding device /dev/sdd ... OK", "stdout_lines": ["Creating cluster ... ID: 794b285745b1c5d7089e1c5729ec7cd2", "\tAllowing file volumes on cluster.", "\tAllowing block volumes on cluster.", "\tCreating node mycluster-cns-0 ... ID: 45f1a3bfc20a4196e59ebb567e0e02b4", "\t\tAdding device /dev/sdd ... OK", "\t\tAdding device /dev/sde ... OK", "\t\tAdding device /dev/sdf ... OK", "\tCreating node mycluster-cns-1 ... ID: 596f80d7bbd78a1ea548930f23135131", "\t\tAdding device /dev/sdc ... Unable to add device: Unable to execute command on glusterfs-storage-4zc42:   Device /dev/sdc excluded by a filter.", "\t\tAdding device /dev/sde ... OK", "\t\tAdding device /dev/sdd ... OK", "\tCreating node mycluster-cns-2 ... ID: 42c0170aa2799559747622acceba2e3f", "\t\tAdding device /dev/sde ... OK", "\t\tAdding device /dev/sdf ... OK", "\t\tAdding device /dev/sdd ... OK"]}

PLAY RECAP *********************************************************************
mycluster-cns-0       : ok=146  changed=57   unreachable=0    failed=0   
mycluster-cns-1       : ok=146  changed=57   unreachable=0    failed=0   
mycluster-cns-2       : ok=146  changed=57   unreachable=0    failed=0   
mycluster-infra-0     : ok=143  changed=55   unreachable=0    failed=0   
mycluster-infra-1     : ok=143  changed=55   unreachable=0    failed=0   
mycluster-infra-2     : ok=143  changed=55   unreachable=0    failed=0   
mycluster-master-0    : ok=502  changed=198  unreachable=0    failed=1   
mycluster-master-1    : ok=348  changed=140  unreachable=0    failed=0   
mycluster-master-2    : ok=348  changed=140  unreachable=0    failed=0   
mycluster-node-0      : ok=143  changed=55   unreachable=0    failed=0   
mycluster-node-1      : ok=143  changed=55   unreachable=0    failed=0   
localhost                  : ok=13   changed=0    unreachable=0    failed=0   

INSTALLER STATUS ***************************************************************
Initialization             : Complete (0:00:39)
Health Check               : Complete (0:00:24)
etcd Install               : Complete (0:01:24)
Master Install             : Complete (0:14:59)
Master Additional Install  : Complete (0:01:10)
Node Install               : Complete (0:10:58)
GlusterFS Install          : In Progress (0:03:33)
    This phase can be restarted by running: playbooks/openshift-glusterfs/config.yml

Failure summary:

  1. Hosts:    mycluster-master-0
     Play:     Configure GlusterFS
     Task:     Load heketi topology
     Message:  Failed without returning a message.
```

Yükleme sırasında en sık karşılaşılan hatalar şunlardır:

1. Özel anahtar parolası vardır
2. Özel anahtarlı anahtar kasası sırrı doğru oluşturulmadı
3. Hizmet temel kimlik bilgileri yanlış girildi
4. Hizmet sorumlusunun kaynak grubuna katkıda bulunan erişimi yoktur

### <a name="private-key-has-a-passphrase"></a>Private Key'in bir parolası vardır

SSH için izin reddedildi bir hata görürsünüz. özel anahtar üzerinde bir geçiş ifadesini kontrol etmek için ansible playbook ana bilgisayara ssh.

### <a name="key-vault-secret-with-private-key-wasnt-created-correctly"></a>Özel anahtarlı anahtar kasası sırrı doğru oluşturulmadı

Özel anahtar ansible playbook ana bilgisayara kopyalanır - ~/.ssh/id_rsa. Bu dosyanın doğru olduğunu onaylayın. Bir SSH oturumunu ansible playbook ana bilgisayardan küme düğümlerinden birine açarak test edin.

### <a name="service-principal-credentials-were-entered-incorrectly"></a>Hizmet temel kimlik bilgileri yanlış girildi

Şablonveya Market teklifine giriş sağlanırken, yanlış bilgiler sağlandı. Hizmet sorumlusu için doğru appId (clientId) ve parolayı (clientSecret) kullandığınızdan emin olun. Aşağıdaki azure cli komutunu vererek doğrulayın.

```azurecli
az login --service-principal -u <client id> -p <client secret> -t <tenant id>
```

### <a name="service-principal-doesnt-have-contributor-access-to-the-resource-group"></a>Hizmet sorumlusunun kaynak grubuna katkıda bulunan erişimi yoktur

Azure bulut sağlayıcısı etkinse, kullanılan hizmet sorumlusunun kaynak grubuna katkıda bulunan lara erişimi olmalıdır. Aşağıdaki azure cli komutunu vererek doğrulayın.

```azurecli
az group update -g <openshift resource group> --set tags.sptest=test
```

## <a name="additional-tools"></a>Ek araçlar

Bazı hatalar için, daha fazla bilgi almak için aşağıdaki komutları da kullanabilirsiniz:

1. systemctl \<durum hizmeti>
2. journalctl -xe
