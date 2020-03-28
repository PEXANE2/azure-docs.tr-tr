---
title: Azure'daki RHEL sanal makinelerde SQL Server için kullanılabilirlik gruplarını yapılandırma - Linux Sanal Makineler | Microsoft Dokümanlar
description: RHEL küme ortamında Yüksek Kullanılabilirlik ayarlama ve STONITH'i kurma hakkında bilgi edinin
ms.service: virtual-machines-linux
ms.subservice: ''
ms.topic: tutorial
author: VanMSFT
ms.author: vanto
ms.reviewer: jroth
ms.date: 02/27/2020
ms.openlocfilehash: 40c91f67231fb6a9d01191ee5215eae8d4dc045b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79096689"
---
# <a name="tutorial-configure-availability-groups-for-sql-server-on-rhel-virtual-machines-in-azure"></a>Öğretici: Azure'daki RHEL sanal makinelerde SQL Server için kullanılabilirlik gruplarını yapılandırma 

> [!NOTE]
> Sunulan öğretici **genel önizlemededir.** 
>
> Bu eğitimde RHEL 7.6 ile SQL Server 2017'yi kullanıyoruz, ancak HA'yı yapılandırmak için RHEL 7 veya RHEL 8'de SQL Server 2019'u kullanmak mümkündür. Kullanılabilirlik grubu kaynaklarını yapılandırma komutları RHEL 8'de değişti ve makaleye bakmak, [kullanılabilirlik grubu kaynağı oluşturma](/sql/linux/sql-server-linux-availability-group-cluster-rhel#create-availability-group-resource) ve doğru komutlar hakkında daha fazla bilgi için RHEL 8 kaynakları oluşturmak isteyeceksiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Yeni bir kaynak grubu, Kullanılabilirlik Seti ve Azure Linux Sanal Makineleri (VM) oluşturun
> - Yüksek Kullanılabilirliği Etkinleştirme (HA)
> - Kalp Pili kümesi oluşturma
> - BIR STONITH aygıtı oluşturarak bir eskrim aracısı yapılandırma
> - RHEL'de SQL Server ve mssql araçlarını yükleme
> - SQL Server'ı Her Zaman Kullanılabilirlik Grubunda Yapılandır
> - Pacemaker kümesindeki kullanılabilirlik grubunu (AG) kaynakları yapılandırma
> - Bir başarısızlık ve eskrim aracısını test edin

Bu öğretici, Azure'da kaynakları dağıtmak için Azure komut satırı arabirimini (CLI) kullanır.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı tercih ederseniz, bu öğretici azure CLI sürüm 2.0.30 veya daha sonra gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak Grubu oluşturma

Birden fazla aboneliğiniz varsa, bu kaynakları dağıtmak istediğiniz [aboneliği ayarlayın.](/cli/azure/manage-azure-subscriptions-azure-cli)

Bir bölgede Kaynak Grubu `<resourceGroupName>` oluşturmak için aşağıdaki komutu kullanın. Seçtiğiniz `<resourceGroupName>` bir ad ile değiştirin. Bu öğretici `East US 2` için kullanıyoruz. Daha fazla bilgi için aşağıdaki [Quickstart'a](../quick-create-cli.md)bakın.

```azurecli-interactive
az group create --name <resourceGroupName> --location eastus2
```

## <a name="create-an-availability-set"></a>Kullanılabilirlik Kümesi Oluşturma

Bir sonraki adım, bir Kullanılabilirlik Kümesi oluşturmaktır. Azure Bulut Su Şur'da `<resourceGroupName>` aşağıdaki komutu çalıştırın ve Kaynak Grubu adınız ile değiştirin. Için `<availabilitySetName>`bir ad seçin.

```azurecli-interactive
az vm availability-set create \
    --resource-group <resourceGroupName> \
    --name <availabilitySetName> \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Komut tamamlandıktan sonra aşağıdaki sonuçları almalısınız:

```output
{
  "id": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Compute/availabilitySets/<availabilitySetName>",
  "location": "eastus2",
  "name": "<availabilitySetName>",
  "platformFaultDomainCount": 2,
  "platformUpdateDomainCount": 2,
  "proximityPlacementGroup": null,
  "resourceGroup": "<resourceGroupName>",
  "sku": {
    "capacity": null,
    "name": "Aligned",
    "tier": null
  },
  "statuses": null,
  "tags": {},
  "type": "Microsoft.Compute/availabilitySets",
  "virtualMachines": []
}
```

## <a name="create-rhel-vms-inside-the-availability-set"></a>Kullanılabilirlik Kümesi içinde RHEL VM'ler oluşturun

> [!WARNING]
> Bir You-As-You-Go (PAYG) RHEL resmi seçer seniz ve Yüksek Kullanılabilirlik (HA) yapılandırmak, aboneliğinizi kaydetmek için gerekli olabilir. Bu, VM için Microsoft Azure RHEL aboneliği ve Red Hat aboneliği için ücretlendirileceksiniz gibi abonelik için iki kez ödeme yapabilirsiniz. Daha fazla bilgi için bkz. https://access.redhat.com/solutions/2458541.
>
> "Çift faturalı" olmamak için Azure VM'yi oluştururken bir RHEL HA görüntüsü kullanın. RHEL-HA görüntüleri olarak sunulan görüntüler, HA repo önceden etkin leştirilmiş PAYG görüntüleridir.

1. HA ile RHEL sunan Sanal Makine (VM) görüntülerinin bir listesini alın:

    ```azurecli-interactive
    az vm image list --all --offer "RHEL-HA"
    ```

    Aşağıdaki sonuçları görmeniz gerekir:

    ```output
    [
            {
              "offer": "RHEL-HA",
              "publisher": "RedHat",
              "sku": "7.4",
              "urn": "RedHat:RHEL-HA:7.4:7.4.2019062021",
              "version": "7.4.2019062021"
            },
            {
              "offer": "RHEL-HA",
              "publisher": "RedHat",
              "sku": "7.5",
              "urn": "RedHat:RHEL-HA:7.5:7.5.2019062021",
              "version": "7.5.2019062021"
            },
            {
              "offer": "RHEL-HA",
              "publisher": "RedHat",
              "sku": "7.6",
              "urn": "RedHat:RHEL-HA:7.6:7.6.2019062019",
              "version": "7.6.2019062019"
            }
    ]
    ```

    Bu öğretici için, görüntüyü `RedHat:RHEL-HA:7.6:7.6.2019062019`seçiyoruz.

    > [!IMPORTANT]
    > Kullanılabilirlik Grubu'nun ayarlanabilmesi için makine adlarının 15 karakterden az olması gerekir. Kullanıcı adı büyük harf karakterleri içeremez ve parolalar 12'den fazla karaktere sahip olmalıdır.

1. Kullanılabilirlik Kümesi'nde 3 VM oluşturmak istiyoruz. Aşağıdaki komutta aşağıdakileri değiştirin:

    - `<resourceGroupName>`
    - `<VM-basename>`
    - `<availabilitySetName>`
    - `<VM-Size>`- Bir örnek "Standard_D16_v3" olacaktır
    - `<username>`
    - `<adminPassword>`

    ```azurecli-interactive
    for i in `seq 1 3`; do
           az vm create \
             --resource-group <resourceGroupName> \
             --name <VM-basename>$i \
             --availability-set <availabilitySetName> \
             --size "<VM-Size>"  \
             --image "RedHat:RHEL-HA:7.6:7.6.2019062019" \
             --admin-username "<username>" \
             --admin-password "<adminPassword>" \
             --authentication-type all \
             --generate-ssh-keys
    done
    ```

Yukarıdaki komut VM'leri oluşturur ve bu VM'ler için varsayılan bir VNet oluşturur. Farklı yapılandırmalar hakkında daha fazla bilgi için [az vm create](https://docs.microsoft.com/cli/azure/vm) makalesine bakın.

Her VM için komut tamamlandıktan sonra aşağıdakilere benzer sonuçlar almalısınız:

```output
{
  "fqdns": "",
  "id": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Compute/virtualMachines/<VM1>",
  "location": "eastus2",
  "macAddress": "<Some MAC address>",
  "powerState": "VM running",
  "privateIpAddress": "<IP1>",
  "publicIpAddress": "",
  "resourceGroup": "<resourceGroupName>",
  "zones": ""
}
```

> [!IMPORTANT]
> Yukarıdaki komutla oluşturulan varsayılan görüntü varsayılan olarak 32GB işletim sistemi diski oluşturur. Bu varsayılan yüklemeyle yer inme potansiyeliniz olabilir. Örnek olarak 128 GB'lık `az vm create` bir işletim sistemi diski oluşturmak için yukarıdaki `--os-disk-size-gb 128`komuta eklenen aşağıdaki parametreyi kullanabilirsiniz: .
>
> Daha sonra yüklemenize uyum sağlamak için uygun klasör birimlerini genişletmeniz gerekiyorsa [Mantıksal Birim Yöneticisi'ni (LVM) yapılandırabilirsiniz.](../../../virtual-machines/linux/configure-lvm.md)

### <a name="test-connection-to-the-created-vms"></a>Oluşturulan VM'lere test bağlantısı

Azure Bulut Uyruğu'nda aşağıdaki komutu kullanarak VM1'e veya diğer VMM'lere bağlanın. VM IP'lerinizi bulamıyorsanız, Azure [Bulut BulutU'nda bu Quickstart'ı](../../../cloud-shell/quickstart.md#ssh-into-your-linux-vm)izleyin.

```azurecli-interactive
ssh <username>@publicipaddress
```

Bağlantı başarılı olursa, Linux terminalini temsil eden aşağıdaki çıktıyı görmeniz gerekir:

```output
[<username>@<VM1> ~]$
```

SSH oturumundan ayrılmak için yazın. `exit`

## <a name="enable-high-availability"></a>Yüksek Kullanılabilirlik Etkinleştirme

> [!IMPORTANT]
> Öğreticinin bu bölümünü tamamlamak için RHEL ve Yüksek Kullanılabilirlik Eklentisi için bir aboneliğiniz olması gerekir. Önceki bölümde önerilen bir resim kullanıyorsanız, başka bir abonelik kaydetmeniz gerekmez.
 
Her VM düğümüne bağlanın ve HA'yı etkinleştirmek için aşağıdaki kılavuzu izleyin. Daha fazla bilgi [için RHEL için yüksek kullanılabilirlik aboneliğini etkinleştirme bilgisine](/sql/linux/sql-server-linux-availability-group-cluster-rhel#enable-the-high-availability-subscription-for-rhel)bakın.

> [!TIP]
> Makale boyunca her VM'de aynı komutların çalıştırılması gerektiğinden, vm'lerin her birine aynı anda bir SSH oturumu açarsanız daha kolay olacaktır.
>
> Birden çok `sudo` komutkopyalayıp yapıştırıyorsanız ve parola istenirse, ek komutlar çalışmaz. Her komutu ayrı ayrı çalıştırın.


1. Pacemaker güvenlik duvarı bağlantı noktalarını açmak için her VM'de aşağıdaki komutları çalıştırın:

    ```bash
    sudo firewall-cmd --permanent --add-service=high-availability
    sudo firewall-cmd --reload
    ```

1. Aşağıdaki komutları kullanarak Tüm düğümlerde Pacemaker paketlerini güncelleştirin ve yükleyin:

    > [!NOTE]
    > **nmap,** abunuzdaki kullanılabilir IP adreslerini bulmak için bir araç olarak bu komut bloğunun bir parçası olarak yüklenir. **Nmap**yüklemek zorunda değil, ama daha sonra bu öğretici yararlı olacaktır.

    ```bash
    sudo yum update -y
    sudo yum install -y pacemaker pcs fence-agents-all resource-agents fence-agents-azure-arm nmap
    sudo reboot
    ```

1. Pacemaker paketlerini yüklerken oluşturulan varsayılan kullanıcının parolasını ayarlayın. Tüm düğümlerde aynı parolayı kullanın.

    ```bash
    sudo passwd hacluster
    ```

1. Ana bilgisayar dosyasını açmak ve ana bilgisayar adı çözümlemesi ayarlamak için aşağıdaki komutu kullanın. Daha fazla bilgi için, ana bilgisayar dosyasını yapılandırma konusunda [AG'yi Yapılandır'](/sql/linux/sql-server-linux-availability-group-configure-ha#prerequisites) a bakın.

    ```
    sudo vi /etc/hosts
    ```

    **VI** düzenleyicisinde, `i` metin eklemek için girin ve boş bir satıra ilgili VM'nin **Özel IP'sini** ekleyin. Ardından IP'nin yanındaki boşluktan sonra VM adını ekleyin. Her satırın ayrı bir girişi olmalıdır.

    ```output
    <IP1> <VM1>
    <IP2> <VM2>
    <IP3> <VM3>
    ```

    > [!IMPORTANT]
    > Yukarıdaki **Özel IP** adresinizi kullanmanızı öneririz. Bu yapılandırmada Genel IP adresinin kullanılması kurulumun başarısız lığa neden olur ve VM'nizi harici ağlara maruz bırakmanızı önermiyoruz.

    **Vi** düzenleyiciden çıkmak için önce **Esc** tuşuna basın `:wq` ve sonra dosyayı yazmak için komutu girin ve çıkın.

## <a name="create-the-pacemaker-cluster"></a>Kalp Pili kümesini oluşturma

Bu bölümde, biz etkinleştirmek ve pcsd hizmeti başlatmak ve daha sonra küme yapılandırmak. Linux'taki SQL Server için küme kaynakları otomatik olarak oluşturulmaz. Kalp pili kaynaklarını el ile etkinleştirmemiz ve oluşturmamız gerekecek. Daha fazla bilgi için, [RHEL için bir failover küme örneği yapılandırma](/sql/linux/sql-server-linux-shared-disk-cluster-red-hat-7-configure#install-and-configure-pacemaker-on-each-cluster-node) makaleye bakın

### <a name="enable-and-start-pcsd-service-and-pacemaker"></a>Pcsd hizmetini ve Kalp Pili'ni etkinleştirin ve başlatın

1. Tüm düğümlerde komutları çalıştırın. Bu komutlar düğümlerin yeniden başlatıldıktan sonra kümeye yeniden katılmasını sağlar.

    ```bash
    sudo systemctl enable pcsd
    sudo systemctl start pcsd
    sudo systemctl enable pacemaker
    ``` 

1. Varolan küme yapılandırmalarını tüm düğümlerden kaldırın. Şu komutu çalıştırın:

    ```bash
    sudo pcs cluster destroy 
    sudo systemctl enable pacemaker 
    ```

1. Birincil düğümde, kümeyi ayarlamak için aşağıdaki komutları çalıştırın.

    - Küme düğümlerinin kimliğini doğrulamak için `pcs cluster auth` komutu çalıştırırken, bir parola istenir. Daha önce oluşturulan **hacluster** kullanıcısının parolasını girin.

    ```bash
    sudo pcs cluster auth <VM1> <VM2> <VM3> -u hacluster
    sudo pcs cluster setup --name az-hacluster <VM1> <VM2> <VM3> --token 30000
    sudo pcs cluster start --all
    sudo pcs cluster enable --all
    ```

1. Tüm düğümlerin çevrimiçi olup olmadığını kontrol etmek için aşağıdaki komutu çalıştırın.

    ```bash
    sudo pcs status
    ```

    Tüm düğümler çevrimiçiyse, aşağıdakilere benzer bir çıktı görürsünüz:

    ```output
    Cluster name: az-hacluster
     
    WARNINGS:
    No stonith devices and stonith-enabled is not false
     
    Stack: corosync
    Current DC: <VM2> (version 1.1.19-8.el7_6.5-c3c624ea3d) - partition with quorum
    Last updated: Fri Aug 23 18:27:57 2019
    Last change: Fri Aug 23 18:27:56 2019 by hacluster via crmd on <VM2>
     
    3 nodes configured
    0 resources configured
     
    Online: [ <VM1> <VM2> <VM3> ]
     
    No resources
     
     
    Daemon Status:
          corosync: active/enabled
          pacemaker: active/enabled
          pcsd: active/enabled
    ```

1. Canlı kümede beklenen oyları 3'e ayarlayın. Bu komut yalnızca canlı kümeyi etkiler ve yapılandırma dosyalarını değiştirmez.

    Tüm düğümlerde, aşağıdaki komutla beklenen oyları ayarlayın:

    ```bash
    sudo pcs quorum expected-votes 3
    ```

## <a name="configure-the-fencing-agent"></a>Eskrim aracısını yapılandırın

Bir STONITH cihazı bir eskrim ajanı sağlar. Aşağıdaki talimatlar bu öğretici için değiştirilmiştir. Daha fazla bilgi için [bir STONITH aygıtı oluşturmaya](../../../virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker.md#create-stonith-device)bakın.
 
Azure Çit Aracısı sürümünü denetler ve [güncelleştirdiğinden emin olun.](../../../virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker.md#cluster-installation) Aşağıdaki komutu kullanın:

```bash
sudo yum info fence-agents-azure-arm
```

Aşağıdaki örneğe benzer bir çıktı görmeniz gerekir.

```output
Loaded plugins: langpacks, product-id, search-disabled-repos, subscription-manager
Installed Packages
Name        : fence-agents-azure-arm
Arch        : x86_64
Version     : 4.2.1
Release     : 11.el7_6.8
Size        : 28 k
Repo        : installed
From repo   : rhel-ha-for-rhel-7-server-eus-rhui-rpms
Summary     : Fence agent for Azure Resource Manager
URL         : https://github.com/ClusterLabs/fence-agents
License     : GPLv2+ and LGPLv2+
Description : The fence-agents-azure-arm package contains a fence agent for Azure instances.
```

### <a name="register-a-new-application-in-azure-active-directory"></a>Azure Etkin Dizini'nde yeni bir uygulama kaydetme
 
 1. Şuraya gidin: https://portal.azure.com
 2. Azure [Active Directory bıçağını](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties)açın. Özellikler'e gidin ve Dizin Kimliğini yazın. Bu,`tenant ID`
 3. [ **Uygulama kayıtlarını** tıklatın](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)
 4. **Yeni kayıt'ı** tıklatın
 5. **Yalnızca** bu `<resourceGroupName>-app`kuruluş **dizininde Hesaplar'ı** seçin gibi bir Ad girin
 6. Uygulama Türü **Web'i**seçin, oturum açma http://localhost) URL'sini girin (örneğin ve Ekle'yi tıklatın. Oturum açma URL'si kullanılmaz ve geçerli bir URL olabilir. Bir kez bittikten sonra, **Kayıt Tıklayın**
 7. Yeni Uygulama kaydınız için **Sertifikalar ve sırlar** seçin ve **ardından Yeni müşteri sırrını** tıklatın
 8. Yeni bir anahtar için açıklama girin (istemci sırrı), **asla süresi dolmaz'ı** seçin ve **Ekle'yi** tıklatın
 9. Sırrın değerini yaz. Hizmet Sorumlusunun şifresi olarak kullanılır
10. **Genel Bakış**’ı seçin. Başvuru Kimliğini yazın. Hizmet Sorumlusunun kullanıcı adı (aşağıdaki adımlardaki giriş kimliği) olarak kullanılır
 
### <a name="create-a-custom-role-for-the-fence-agent"></a>Çit aracısı için özel bir rol oluşturma

[Azure CLI'yi kullanarak Azure kaynakları için özel bir rol oluşturmak için](../../../role-based-access-control/tutorial-custom-role-cli.md#create-a-custom-role)öğreticiyi izleyin.

Json dosyanız aşağıdakilere benzer olmalıdır:

- Seçtiğiniz `<username>` bir ad ile değiştirin. Bu, bu rol tanımıoluştururken herhangi bir yineleme önlemek içindir.
- Azure `<subscriptionId>` Abonelik Kimliğinizle değiştirin.

```json
{
  "Name": "Linux Fence Agent Role-<username>",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows to power-off and start virtual machines",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/powerOff/action",
    "Microsoft.Compute/virtualMachines/start/action"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<subscriptionId>"
  ]
}
```

Rolü eklemek için aşağıdaki komutu çalıştırın:

- Dosyanın adı ile değiştirin. `<filename>`
- Komutu dosyanın kaydettiği klasörün dışındaki bir yoldan yürütüyorsanız, dosyanın klasör yolunu komuta ekleyin.

```bash
az role definition create --role-definition "<filename>.json"
```

Aşağıdaki çıktıyı görmeniz gerekir:

```output
{
  "assignableScopes": [
    "/subscriptions/<subscriptionId>"
  ],
  "description": "Allows to power-off and start virtual machines",
  "id": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/roleDefinitions/<roleNameId>",
  "name": "<roleNameId>",
  "permissions": [
    {
      "actions": [
        "Microsoft.Compute/*/read",
        "Microsoft.Compute/virtualMachines/powerOff/action",
        "Microsoft.Compute/virtualMachines/start/action"
      ],
      "dataActions": [],
      "notActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Linux Fence Agent Role-<username>",
  "roleType": "CustomRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="assign-the-custom-role-to-the-service-principal"></a>Özel rolü Hizmet Sorumlusuna atama

Son adımda `Linux Fence Agent Role-<username>` oluşturulan özel rolü Hizmet Sorumlusuna atayın. Artık Sahibi rolünü kullanmayın!
 
1. Şuraya gidin: https://portal.azure.com
2. Tüm [kaynaklar bıçağını](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAll) açın
3. İlk küme düğümünün sanal makinesini seçin
4. **Erişim denetimine (IAM)** tıklayın
5. **Rol ataması ekle'yi** tıklatın
6. `Linux Fence Agent Role-<username>` **Rol** listesinden rolü seçme
7. **Seç** listesine, yukarıda oluşturduğunuz uygulamanın adını girin,`<resourceGroupName>-app`
8. **Kaydet'i** tıklatın
9. Tüm küme düğümü için yukarıdaki adımları yineleyin.

### <a name="create-the-stonith-devices"></a>STONITH aygıtlarını oluşturun

Düğüm 1'de aşağıdaki komutları çalıştırın:

- `<ApplicationID>` Başvuru kaydınızdaki kimlik değerini değiştirin.
- İstemci `<servicePrincipalPassword>` sırrının değeriyle değiştirin.
- `<resourceGroupName>` Bu öğretici için kullanılan aboneliğinizden Kaynak Grubu'nu değiştirin.
- Azure `<tenantID>` Aboneliğinizden `<subscriptionId>` ve azure aboneliğinizden ve bu abonelikten değiştirin.

```bash
sudo pcs property set stonith-timeout=900
sudo pcs stonith create rsc_st_azure fence_azure_arm login="<ApplicationID>" passwd="<servicePrincipalPassword>" resourceGroup="<resourceGroupName>" tenantId="<tenantID>" subscriptionId="<subscriptionId>" power_timeout=240 pcmk_reboot_timeout=900
```

HA hizmetine izin vermek için güvenlik duvarımıza`--add-service=high-availability`zaten bir kural eklediğimiziçin, tüm düğümlerde aşağıdaki güvenlik duvarı bağlantı noktalarını açmaya gerek yoktur: 2224, 3121, 21064, 5405. Ancak, HA ile herhangi bir bağlantı sorunu yaşıyorsanız, HA ile ilişkili bu bağlantı noktalarını açmak için aşağıdaki komutu kullanın.

> [!TIP]
> İsteğe bağlı olarak biraz zaman kazanmak için aynı anda bu öğretici tüm bağlantı noktalarını ekleyebilirsiniz. Açılması gereken bağlantı noktaları aşağıdaki göreli bölümlerinde açıklanmıştır. Şimdi tüm bağlantı noktalarını eklemek isterseniz, ek bağlantı noktalarını ekleyin: 1433 ve 5022.

```bash
sudo firewall-cmd --zone=public --add-port=2224/tcp --add-port=3121/tcp --add-port=21064/tcp --add-port=5405/tcp --permanent
sudo firewall-cmd --reload
```

## <a name="install-sql-server-and-mssql-tools"></a>SQL Server ve mssql araçlarını yükleme
 
VM'lere SQL Server ve mssql araçlarını yüklemek için aşağıdaki bölümü kullanın. Tüm düğümlerde bu eylemlerin her birini gerçekleştirin. Daha fazla bilgi için [SQL Server'a Red Hat VM yükle](/sql/linux/quickstart-install-connect-red-hat)'ye bakın.

### <a name="installing-sql-server-on-the-vms"></a>VM'lere SQL Server Yükleme

SQL Server'ı yüklemek için aşağıdaki komutlar kullanılır:

```bash
sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
sudo yum install -y mssql-server
sudo /opt/mssql/bin/mssql-conf setup
sudo yum install mssql-server-ha
```

### <a name="open-firewall-port-1433-for-remote-connections"></a>Uzak bağlantılar için güvenlik duvarı bağlantı noktası 1433'ü açın

Uzaktan bağlanmak için VM'de 1433 bağlantı noktasını açmanız gerekir. Her VM'nin güvenlik duvarında bağlantı noktası 1433'ü açmak için aşağıdaki komutları kullanın:

```bash
sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
sudo firewall-cmd --reload
```

### <a name="installing-sql-server-command-line-tools"></a>SQL Server komut satırı araçlarını yükleme

SQL Server komut satırı araçlarını yüklemek için aşağıdaki komutlar kullanılır. Daha fazla bilgi için [SQL Server komut satırı araçlarını yükleyin.](/sql/linux/quickstart-install-connect-red-hat#tools)

```bash
sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo
sudo yum install -y mssql-tools unixODBC-devel
```
 
> [!NOTE] 
> Kolaylık sağlamak için PATH ortamı değişkeninize /opt/mssql-tools/bin/ ekleyin. Bu, tam yolu belirtmeden araçları çalıştırmanızı sağlar. PATH ortam değişkenini hem oturum açma bilgileriyle başlatılan oturumları hem de etkileşimli/oturum açma bilgisi olmadan başlatılan oturumları için değiştirmek üzere aşağıdaki komutları çalıştırın:</br></br>
`echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile`</br>
`echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc`</br>
`source ~/.bashrc`


### <a name="check-the-status-of-the-sql-server"></a>SQL Server'ın durumunu kontrol edin

Yapılandırmayı bitirdikten sonra, SQL Server'ın durumunu kontrol edebilir ve çalıştığını doğrulayabilirsiniz:

```bash
systemctl status mssql-server --no-pager
```

Aşağıdaki çıktıyı görmeniz gerekir:

```output
● mssql-server.service - Microsoft SQL Server Database Engine
   Loaded: loaded (/usr/lib/systemd/system/mssql-server.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2019-12-05 17:30:55 UTC; 20min ago
     Docs: https://docs.microsoft.com/en-us/sql/linux
 Main PID: 11612 (sqlservr)
   CGroup: /system.slice/mssql-server.service
           ├─11612 /opt/mssql/bin/sqlservr
           └─11640 /opt/mssql/bin/sqlservr
```

## <a name="configure-sql-server-always-on-availability-group"></a>SQL Server'ı Her Zaman Kullanılabilirlik Grubunda Yapılandır

VM'leriniz için SQL Server Always On Availability Group'u yapılandırmak için aşağıdaki adımları kullanın. Daha fazla bilgi için, [Linux'ta yüksek kullanılabilirlik için SQL Server Always On Availability Group'u yapılandırın](/sql/linux/sql-server-linux-availability-group-configure-ha)

### <a name="enable-alwayson-availability-groups-and-restart-mssql-server"></a>AlwaysOn kullanılabilirlik gruplarını etkinleştirin ve mssql-server'ı yeniden başlatın

SQL Server örneğini barındıran her düğümde AlwaysOn kullanılabilirlik gruplarını etkinleştirin. Sonra mssql-server'ı yeniden başlatın. Şu betiği çalıştırın:

```
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1
sudo systemctl restart mssql-server
```

### <a name="create-a-certificate"></a>Sertifika oluşturma

Şu anda AG bitiş noktasına AD kimlik doğrulamasını destekliyoruz. Bu nedenle, AG uç nokta şifreleme için bir sertifika kullanmamız gerekir.

1. SQL Server Management Studio (SSMS) veya SQL CMD kullanarak **tüm düğümlere** bağlanın. AlwaysOn_health oturumunu etkinleştirmek ve bir ana anahtar oluşturmak için aşağıdaki komutları çalıştırın:

    > [!IMPORTANT]
    > SQL Server örneğinize uzaktan bağlanıyorsanız, güvenlik duvarınızda 1433 bağlantı noktasının açık olması gerekir. Ayrıca, her VM için NSG'nizde 1433 bağlantı noktasına gelen bağlantılara izin vermeniz gerekir. Daha fazla bilgi için [bkz.](../../../virtual-network/manage-network-security-group.md#create-a-security-rule)

    - Kendi `<Master_Key_Password>` parolanızla değiştirin.


    ```sql
    ALTER EVENT SESSION  AlwaysOn_health ON SERVER WITH (STARTUP_STATE=ON);
    GO
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Master_Key_Password>';
    ```

 
1. SSMS veya SQL CMD kullanarak birincil yinelemeye bağlanın. Aşağıdaki komutlar, birincil SQL `/var/opt/mssql/data/dbm_certificate.cer` Server yinelemenizde `var/opt/mssql/data/dbm_certificate.pvk` bir sertifika ve özel bir anahtar oluşturur:

    - Kendi `<Private_Key_Password>` parolanızla değiştirin.

```sql
CREATE CERTIFICATE dbm_certificate WITH SUBJECT = 'dbm';
GO

BACKUP CERTIFICATE dbm_certificate
   TO FILE = '/var/opt/mssql/data/dbm_certificate.cer'
   WITH PRIVATE KEY (
           FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
           ENCRYPTION BY PASSWORD = '<Private_Key_Password>'
       );
GO
```

Komutu çalıştırarak SQL CMD oturumundan `exit` çıkın ve SSH oturumunuza geri dönün.
 
### <a name="copy-the-certificate-to-the-secondary-replicas-and-create-the-certificates-on-the-server"></a>Sertifikayı ikincil yinelemelere kopyalayın ve sunucuda sertifikaları oluşturun

1. Kullanılabilirlik yinelemelerini barındıracak tüm sunucularda oluşturulan iki dosyayı aynı konuma kopyalayın.
 
    Birincil sunucuda, sertifikayı `scp` hedef sunuculara kopyalamak için aşağıdaki komutu çalıştırın:

    - Kullandığınız `<username>` `<VM2>` kullanıcı adı ve hedef VM adı ile değiştirin.
    - Tüm ikincil yinelemeler için bu komutu çalıştırın.

    > [!NOTE]
    > Koşmak `sudo -i`zorunda değilsiniz, bu da size kök ortamı nı verir. Daha önce bu `sudo` öğreticide yaptığımız gibi her komutun önünde komutu çalıştırabilirsiniz.

    ```bash
    # The below command allows you to run commands in the root environment
    sudo -i
    ```

    ```bash
    scp /var/opt/mssql/data/dbm_certificate.* <username>@<VM2>:/home/<username>
    ```

1. Hedef Sunucu'da aşağıdaki komutu çalıştırın:

    - Kullanıcı `<username>` adınız ile değiştirin.
    - Komut `mv` dosyaları veya dizini bir yerden diğerine taşır.
    - Komut, `chown` dosya, dizin veya bağlantı sahibini ve grubunu değiştirmek için kullanılır.
    - Tüm ikincil yinelemeler için bu komutları çalıştırın.

    ```bash
    sudo -i
    mv /home/<username>/dbm_certificate.* /var/opt/mssql/data/
    cd /var/opt/mssql/data
    chown mssql:mssql dbm_certificate.*
    ```

1. Aşağıdaki Transact-SQL komut dosyası, birincil SQL Server yinelemesi üzerinde oluşturduğunuz yedeklemeden bir sertifika oluşturur. Komut dosyasını güçlü parolalarla güncelleştirin. Şifre çözme parolası, önceki adımda .pvk dosyasını oluşturmak için kullandığınız parolayla aynıdır. Sertifikayı oluşturmak için, tüm ikincil sunucularda SQL CMD veya SSMS kullanarak aşağıdaki komut dosyasını çalıştırın:

    ```sql
    CREATE CERTIFICATE dbm_certificate
        FROM FILE = '/var/opt/mssql/data/dbm_certificate.cer'
        WITH PRIVATE KEY (
        FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
        DECRYPTION BY PASSWORD = '<Private_Key_Password>'
                );
    GO
    ```

### <a name="create-the-database-mirroring-endpoints-on-all-replicas"></a>Tüm yinelemelerde uç noktaları yansıtan veritabanını oluşturma

SQL CMD veya SSMS kullanarak tüm SQL örneklerinde aşağıdaki komut dosyasını çalıştırın:

```sql
CREATE ENDPOINT [Hadr_endpoint]
    AS TCP (LISTENER_PORT = 5022)
    FOR DATABASE_MIRRORING (
    ROLE = ALL,
    AUTHENTICATION = CERTIFICATE dbm_certificate,
ENCRYPTION = REQUIRED ALGORITHM AES
);
GO

ALTER ENDPOINT [Hadr_endpoint] STATE = STARTED;
GO
```

### <a name="create-the-availability-group"></a>Kullanılabilirlik Grubu Oluşturma

SQL CMD veya SSMS kullanarak birincil yinelemeyi barındıran SQL Server örneğine bağlanın. Kullanılabilirlik Grubu oluşturmak için aşağıdaki komutu çalıştırın:

- İstediğiniz Kullanılabilirlik Grubu adı ile değiştirin. `ag1`
- `<VM1>`,, `<VM2>`ve `<VM3>` değerleri yinelemeleri barındıran SQL Server örneklerinin adlarıyla değiştirin.

```sql
CREATE AVAILABILITY GROUP [ag1]
     WITH (DB_FAILOVER = ON, CLUSTER_TYPE = EXTERNAL)
     FOR REPLICA ON
         N'<VM1>'
          WITH (
             ENDPOINT_URL = N'tcp://<VM1>:5022',
             AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
             FAILOVER_MODE = EXTERNAL,
             SEEDING_MODE = AUTOMATIC
             ),
         N'<VM2>'
          WITH (
             ENDPOINT_URL = N'tcp://<VM2>:5022',
             AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
             FAILOVER_MODE = EXTERNAL,
             SEEDING_MODE = AUTOMATIC
             ),
         N'<VM3>'
         WITH(
            ENDPOINT_URL = N'tcp://<VM3>:5022',
            AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
            FAILOVER_MODE = EXTERNAL,
            SEEDING_MODE = AUTOMATIC
            );
GO

ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
GO
```

### <a name="create-a-sql-server-login-for-pacemaker"></a>Pacemaker için BIR SQL Server girişi oluşturma

Tüm SQL Sunucularında Pacemaker için bir SQL girişi oluşturun. Aşağıdaki Transact-SQL bir giriş oluşturur.

- Kendi `<password>` karmaşık parolanızla değiştirin.

```sql
USE [master]
GO

CREATE LOGIN [pacemakerLogin] with PASSWORD= N'<password>';
GO

ALTER SERVER ROLE [sysadmin] ADD MEMBER [pacemakerLogin];
GO
```

Tüm SQL Sunucularında, SQL Server girişi için kullanılan kimlik bilgilerini kaydedin. 

1. Dosyayı oluşturun:

    ```bash
    sudo vi /var/opt/mssql/secrets/passwd
    ```

1. Dosyaya aşağıdaki 2 satırı ekleyin:

    ```bash
    pacemakerLogin
    <password>
    ```

    **Vi** düzenleyiciden çıkmak için önce **Esc** tuşuna basın `:wq` ve sonra dosyayı yazmak için komutu girin ve çıkın.

1. Dosyayı yalnızca köke göre okunabilir hale getirin:

    ```bash
    sudo chown root:root /var/opt/mssql/secrets/passwd
    sudo chmod 400 /var/opt/mssql/secrets/passwd
    ```

### <a name="join-secondary-replicas-to-the-availability-group"></a>Kullanılabilirlik grubuna ikincil yinelemelere katılma

1. AG'ye ikincil yinelemelere katılmak için, tüm sunucular için güvenlik duvarında 5022 bağlantı noktasını açmanız gerekir. SSH oturumunuzda aşağıdaki komutu çalıştırın:

    ```bash
    sudo firewall-cmd --zone=public --add-port=5022/tcp --permanent
    sudo firewall-cmd --reload
    ```

1. İkincil yinelemelerinizde, AG'ye katılmak için aşağıdaki komutları çalıştırın:

    ```sql
    ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    GO

    ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
    GO
    ```

1. Birincil yineleme ve her ikincil yinelemede aşağıdaki İşlem-SQL komut dosyasını çalıştırın:

    ```sql
    GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::ag1 TO pacemakerLogin;
    GO
    
    GRANT VIEW SERVER STATE TO pacemakerLogin;
    GO
    ```

1. İkincil yinelemeler birleştikten sonra, **Her Zaman Yüksek Kullanılabilirlik** düğümlerini genişleterek bunları SSMS Object Explorer'da görebilirsiniz:

    ![availability-grup-joined.png](media/sql-server-linux-rhel-ha-stonith-tutorial/availability-group-joined.png)

### <a name="add-a-database-to-the-availability-group"></a>Kullanılabilirlik grubuna veritabanı ekleme

Veritabanı ekleme [yle ilgili yapılandırma kullanılabilirlik grubu makalesini](/sql/linux/sql-server-linux-availability-group-configure-ha#add-a-database-to-the-availability-group)izleyeceğiz.

Bu adımda aşağıdaki Transact-SQL komutları kullanılır. Birincil yinelemede bu komutları çalıştırın:

```sql
CREATE DATABASE [db1]; -- creates a database named db1
GO

ALTER DATABASE [db1] SET RECOVERY FULL; -- set the database in full recovery mode
GO

BACKUP DATABASE [db1] -- backs up the database to disk
   TO DISK = N'/var/opt/mssql/data/db1.bak';
GO

ALTER AVAILABILITY GROUP [ag1] ADD DATABASE [db1]; -- adds the database db1 to the AG
GO
```

### <a name="verify-that-the-database-is-created-on-the-secondary-servers"></a>Veritabanının ikincil sunucularda oluşturulduğunu doğrulama

Her ikincil SQL Server yinelemesinde, db1 veritabanının oluşturulup oluşturulmadı ve SYNCHRONIZED durumunda olup olmadığını görmek için aşağıdaki sorguyu çalıştırın:

```
SELECT * FROM sys.databases WHERE name = 'db1';
GO
SELECT DB_NAME(database_id) AS 'database', synchronization_state_desc FROM sys.dm_hadr_database_replica_states;
```

`synchronization_state_desc` Liste IÇIN SENKRONIZe `db1`ise, bu yinelemeler eşitlenmiş olduğu anlamına gelir. İkinciler birincil `db1` çoğaltma da gösteriliyor.

## <a name="create-availability-group-resources-in-the-pacemaker-cluster"></a>Pacemaker kümesinde kullanılabilirlik grubu kaynakları oluşturma

[Pacemaker kümesindeki kullanılabilirlik grubu kaynaklarını oluşturmak](/sql/linux/sql-server-linux-create-availability-group#create-the-availability-group-resources-in-the-pacemaker-cluster-external-only)için kılavuzu takip edeceğiz.

### <a name="create-the-ag-cluster-resource"></a>AG küme kaynağını oluşturma

1. Kullanılabilirlik grubunda `ag_cluster` `ag1`kaynak oluşturmak için aşağıdaki komutu kullanın.

    ```bash
    sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=30s master notify=true
    ```

1. Aşağıdaki komutu kullanarak devam etmeden önce kaynağınızı kontrol edin ve çevrimiçi olduğundan emin olun:

    ```bash
    sudo pcs resource
    ```

    Aşağıdaki çıktıyı görmeniz gerekir:

    ```output
    [<username>@VM1 ~]$ sudo pcs resource
    Master/Slave Set: ag_cluster-master [ag_cluster]
    Masters: [ <VM1> ]
    Slaves: [ <VM2> <VM3> ]
    ```

### <a name="create-a-virtual-ip-resource"></a>Sanal BIR IP kaynağı oluşturma

1. Sanal bir IP kaynağı oluşturmak için ağınızdaki kullanılabilir statik IP adresini kullanın. Komut aracını `nmap`kullanarak bir tane bulabilirsiniz.

    ```bash
    nmap -sP <IPRange>
    # For example: nmap -sP 10.0.0.*
    # The above will scan for all IP addresses that are already occupied in the 10.0.0.x space.
    ```

1. **Stonith özellikli** özelliği false olarak ayarlayın

    ```bash
    sudo pcs property set stonith-enabled=false
    ```

1. Aşağıdaki komutu kullanarak sanal IP kaynağı oluşturun:

    - Aşağıdaki `<availableIP>` değeri kullanılmayan bir IP adresiyle değiştirin.

    ```bash
    sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<availableIP>
    ```

### <a name="add-constraints"></a>Kısıtlamalar Ekle

1. IP adresi ve AG kaynağının aynı düğümüzerinde çalıştığından emin olmak için bir birlikte konum kısıtlaması yapılandırılması gerekir. Şu komutu çalıştırın:

    ```bash
    sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
    ```

1. AG kaynağının IP adresinden önce çalışır durumda olduğundan emin olmak için bir sipariş kısıtlaması oluşturun. Birlikte konum kısıtlaması bir sıralama kısıtlaması anlamına gelirken, bu onu zorlar.

    ```bash
    sudo pcs constraint order promote ag_cluster-master then start virtualip
    ```

1. Kısıtlamaları doğrulamak için aşağıdaki komutu çalıştırın:

    ```bash
    sudo pcs constraint list --full
    ```

    Aşağıdaki çıktıyı görmeniz gerekir:

    ```
    Location Constraints:
    Ordering Constraints:
          promote ag_cluster-master then start virtualip (kind:Mandatory) (id:order-ag_cluster-master-virtualip-mandatory)
    Colocation Constraints:
          virtualip with ag_cluster-master (score:INFINITY) (with-rsc-role:Master) (id:colocation-virtualip-ag_cluster-master-INFINITY)
    Ticket Constraints:
    ```

### <a name="re-enable-stonith"></a>Yeniden stonith etkinleştirmek

Test için hazırız. Düğüm 1'de aşağıdaki komutu çalıştırarak kümedeki taşitleri yeniden etkinleştirin:

```bash
sudo pcs property set stonith-enabled=true
```

### <a name="check-cluster-status"></a>Küme durumunu denetleme

Küme kaynaklarınızın durumunu aşağıdaki komutu kullanarak denetleyebilirsiniz:

```output
[<username>@VM1 ~]$ sudo pcs status
Cluster name: az-hacluster
Stack: corosync
Current DC: <VM3> (version 1.1.19-8.el7_6.5-c3c624ea3d) - partition with quorum
Last updated: Sat Dec  7 00:18:38 2019
Last change: Sat Dec  7 00:18:02 2019 by root via cibadmin on VM1

3 nodes configured
5 resources configured

Online: [ <VM1> <VM2> <VM3> ]

Full list of resources:

 Master/Slave Set: ag_cluster-master [ag_cluster]
     Masters: [ <VM2> ]
     Slaves: [ <VM1> <VM3> ]
 virtualip      (ocf::heartbeat:IPaddr2):       Started <VM2>
 rsc_st_azure   (stonith:fence_azure_arm):      Started <VM1>

Daemon Status:
  corosync: active/enabled
  pacemaker: active/enabled
  pcsd: active/enabled
```

## <a name="test-failover"></a>Yük devretme testi

Yapılandırmanın şimdiye kadar başarılı olduğundan emin olmak için, bir başarısızlık test edeceğiz. Daha fazla bilgi için, [Linux'ta Her Zaman Kullanılabilirlik Grubu'nun başarısız lığına](/sql/linux/sql-server-linux-availability-group-failover-ha)bakın.

1. Birincil yinelemeüzerinde el ile başarısız olmak `<VM2>`için aşağıdaki komutu çalıştırın. Sunucu `<VM2>` adınızın değeriyle değiştirin.

    ```bash
    sudo pcs resource move ag_cluster-master <VM2> --master
    ```

1. Kısıtlamalarınızı yeniden denetlerseniz, el ile başarısız olması nedeniyle başka bir kısıtlamanın eklandığını görürsünüz:

    ```output
    [<username>@VM1 ~]$ sudo pcs constraint list --full
    Location Constraints:
          Resource: ag_cluster-master
            Enabled on: VM2 (score:INFINITY) (role: Master) (id:cli-prefer-ag_cluster-master)
    Ordering Constraints:
            promote ag_cluster-master then start virtualip (kind:Mandatory) (id:order-ag_cluster-master-virtualip-mandatory)
    Colocation Constraints:
            virtualip with ag_cluster-master (score:INFINITY) (with-rsc-role:Master) (id:colocation-virtualip-ag_cluster-master-INFINITY)
    Ticket Constraints:
    ```

1. Aşağıdaki komutu `cli-prefer-ag_cluster-master` kullanarak KIMLIĞI ile kısıtlamayı kaldırın:

    ```bash
    sudo pcs constraint remove cli-prefer-ag_cluster-master
    ```

1. Küme kaynaklarınızı komutu `sudo pcs resource`kullanarak denetleyin ve birincil örneğin `<VM2>`şimdi olduğunu görmeniz gerekir.

    ```output
    [<username>@<VM1> ~]$ sudo pcs resource
    Master/Slave Set: ag_cluster-master [ag_cluster]
         ag_cluster (ocf::mssql:ag):        FAILED <VM1> (Monitoring)
         Masters: [ <VM2> ]
         Slaves: [ <VM3> ]
    virtualip      (ocf::heartbeat:IPaddr2):       Started <VM2>
    [<username>@<VM1> ~]$ sudo pcs resource
    Master/Slave Set: ag_cluster-master [ag_cluster]
         Masters: [ <VM2> ]
         Slaves: [ <VM1> <VM3> ]
    virtualip      (ocf::heartbeat:IPaddr2):       Started <VM2>
    ```

## <a name="test-fencing"></a>Test Eskrim

Aşağıdaki komutu çalıştırarak STONITH'i test edebilirsiniz. `<VM1>` için `<VM3>`aşağıdaki komutu çalıştırmayı deneyin.

```bash
sudo pcs stonith fence <VM3> --debug
```

> [!NOTE]
> Varsayılan olarak, çit eylemi düğümü kapatır ve sonra çıkar. Düğümü yalnızca çevrimdışı getirmek istiyorsanız, komuttaki seçeneği `--off` kullanın.

Aşağıdaki çıktıyı almalısınız:

```output
[<username>@<VM1> ~]$ sudo pcs stonith fence <VM3> --debug
Running: stonith_admin -B <VM3>
Return Value: 0
--Debug Output Start--
--Debug Output End--
 
Node: <VM3> fenced
```
Bir çit cihazını test etme hakkında daha fazla bilgi için aşağıdaki [Red Hat](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/s1-stonithtest-haar) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

SQL Sunucularınız için kullanılabilirlik grubu dinleyicisi kullanmak için bir yük dengeleyicisi oluşturmanız ve yapılandırmanız gerekir.

> [!div class="nextstepaction"]
> [Öğretici: Azure'daki RHEL sanal makinelerde SQL Server için kullanılabilirlik grubu dinleyicisini yapılandırma](sql-server-linux-rhel-ha-listener-tutorial.md)
