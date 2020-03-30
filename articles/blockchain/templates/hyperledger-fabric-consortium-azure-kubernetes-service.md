---
title: Azure Kubernetes Service (AKS) üzerinde Hyperledger Kumaş konsorsiyumu
description: Azure Kubernetes Hizmeti'nde Hyperledger Fabric konsorsiyum ağı nasıl dağıtılır ve yapılandırılır?
ms.date: 01/08/2020
ms.topic: article
ms.reviewer: v-umha
ms.openlocfilehash: 2312c002e5c2e0b813f8acbdc3e3bff597f204d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476449"
---
# <a name="hyperledger-fabric-consortium-on-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) üzerinde Hyperledger Kumaş konsorsiyumu

Azure Kubernetes Hizmetinde (AKS) Hyperledger Kumaşı (HLF) şablonundan yararlanıp Azure'da hyperledger Fabric konsorsiyum ağını dağıtabilirsiniz.

Bu makaleyi okuduktan sonra şunları yapabilir olacaksınız:

- Hyperledger Kumaş ve Hyperledger Fabric blockchain ağının yapı taşlarını oluşturan çeşitli bileşenler hakkında çalışma bilgisi edinin.
- Üretim senaryolarınız için Azure Kubernetes Hizmeti'nde bir Hyperledger Fabric konsorsiyumunu nasıl dağıttıve yapılandırın.

## <a name="hyperledger-fabric-consortium-architecture"></a>Hyperledger Kumaş Konsorsiyumu mimarisi

Azure'da Hyperledger Fabric ağı oluşturmak için, eş düğümleri olan Sipariş Hizmeti ve kuruluş dağıtmanız gerekir. Şablon dağıtımının bir parçası olarak oluşturulan farklı temel bileşenler şunlardır:

- **Siparişveren düğümleri**: Genel muhasebede işlem siparişinden sorumlu bir düğüm. Diğer düğümlerle birlikte, sipariş edilen düğümler Hyperledger Fabric ağının sipariş hizmetini oluşturur.

- **Eş düğümleri**: Öncelikle genel muhasebeleri ve akıllı sözleşmeleri barındıran, ağın bu temel öğelerini barındıran düğüm.

- **Kumaş CA**: Kumaş CA Hyperledger Kumaş için Sertifika Otoritesi (CA) olduğunu. Fabric CA, sertifika yetkilisini barındıran sunucu işlemini başlatmanızı ve başlatmanızı sağlar. Kimlikleri ve sertifikaları yönetmenize olanak tanır. Şablonun bir parçası olarak dağıtılan her AKS kümesinde varsayılan olarak bir Fabric CA bölmesi olacaktır.

- **CouchDB veya LevelDB**: Eş düğümleri için dünya devlet veritabanı LevelDB veya CouchDB'de saklanabilir. LevelDB, eş düğümüne katıştırılmış varsayılan durum veritabanıdır ve zincirkodu verilerini basit anahtar değeri çiftleri olarak saklar ve yalnızca anahtar, anahtar aralığı ve bileşik anahtar sorgularını destekler. CouchDB, zincirkodu veri değerleri JSON olarak modellendiğinde zengin sorguları destekleyen isteğe bağlı bir alternatif durum veritabanıdır.

Dağıtım şablonu, aboneliğinizdeki çeşitli Azure kaynaklarını döndürür. Dağıtılan farklı Azure kaynakları şunlardır:

- **AKS kümesi**: Müşteri tarafından sağlanan giriş parametrelerine göre yapılandırılan Azure Kubernetes kümesi. AKS kümesi, Hyperledger Fabric ağ bileşenlerini çalıştırmak için yapılandırılan çeşitli bölmelere sahiptir. Oluşturulan farklı bölmeler şunlardır:

  - **Kumaş aletleri**: Kumaş aleti Hyperledger Kumaş bileşenlerinin yapılandırılmasından sorumludur.
  - **Siparişçi/eş bölmeleri**: HLF ağının düğümleri.
  - **Proxy**: Istemci uygulamalarının AKS kümesiyle arayüz atabileceği bir NGNIX proxy pod'u.
  - **Kumaş CA**: Kumaş CA çalıştıran pod.
- **PostgreSQL**: Fabric CA kimliklerini korumak için PostgreSQL örneği dağıtılır.

- **Azure Anahtar kasası**: Kumaş CA kimlik bilgilerini ve müşteri tarafından sağlanan kök sertifikalarını kaydetmek için anahtar kasa örneği dağıtılır, şablon dağıtımının yeniden denenmesinde kullanılır, bu şablonun mekaniğini işlemek içindir.
- **Azure Yönetilen disk**: Azure Yönetilen disk, genel muhasebe ve eş düğümü dünya durumu veritabanı için kalıcı depolama alanı içindir.
- **Public IP**: Aks kümesinin kümeyle iç içe geçmiş için dağıtılan genel IP bitiş noktası.

## <a name="hyperledger-fabric-blockchain-network-setup"></a>Hyperledger Kumaş Blockchain ağ kurulumu

Başlangıç olarak, birden çok sanal makine ve standart depolama hesabıdağıtmayı desteklenebilen bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabı oluşturabilirsiniz.](https://azure.microsoft.com/free/)

Kurulum Hyperledger Kumaş Blockchain ağ aşağıdaki adımları kullanarak:

- [Siparişçi/eş organizasyonunu dağıtma](#deploy-the-ordererpeer-organization)
- [Konsorsiyumu oluşturun](#build-the-consortium)
- [Yerel HLF işlemlerini çalıştırın](#run-native-hlf-operations)

## <a name="deploy-the-ordererpeer-organization"></a>Siparişçi/eş organizasyonunu dağıtma

HLF ağ bileşenleri dağıtımına başlamak için [Azure portalına](https://portal.azure.com)gidin. **Azure Kubernetes Hizmetinde Hyperledger Fabric**için > search'> **blockchain kaynak oluştur'u** seçin.

1. Şablon dağıtımını başlatmak için **oluştur'u** seçin. **Azure Kubernetes Hizmeti'nde Hyperledger Kumaşı Oluştur** ekranları.

    ![Azure Kubernetes Hizmet Şablonunda Hyperledger Kumaşı](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-fabric-aks.png)

2. **Temel bilgiler** sayfasına proje ayrıntılarını girin.

    ![Azure Kubernetes Hizmet Şablonunda Hyperledger Kumaşı](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-basics.png)

3. Şu ayrıntıları girin:
    - **Abonelik**: HLF ağ bileşenlerini dağıtmak istediğiniz abonelik adını seçin.
    - **Kaynak grubu**: Yeni bir kaynak grubu oluşturun veya varolan boş bir kaynak grubu seçin, kaynak grubu şablonun bir parçası olarak dağıtılan tüm kaynakları tutar.
    - **Bölge**: HLF bileşenleri için Azure Kubernetes kümesini dağıtmak istediğiniz Azure bölgesini seçin. Şablon, AKS'nin kullanılabildiği tüm bölgelerde kullanılabilir Aboneliğinizin Sanal Makine (VM) kota sınırına ulaşamadığı bir bölge seçmeyi sağlayın.
    - Kaynak öneki : Dağıtılan kaynakların adlandırılması için **önek.** Kaynak öneki nin uzunluğu altı karakterden az olmalı ve karakterlerin birleşimi hem sayı hem de harf içermelidir.
4. Dağıtılacak HLF ağ bileşenlerini tanımlamak için **Kumaş Ayarları** sekmesini seçin.

    ![Azure Kubernetes Hizmet Şablonunda Hyperledger Kumaşı](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-settings.png)

5. Şu ayrıntıları girin:
    - **Kuruluş adı**: Çeşitli veri düzlemi işlemleri için gerekli olan Kumaş organizasyonunun adıdır. Kuruluş adının dağıtım başına benzersiz olması gerekir. 
    - **Kumaş ağ bileşeni**: Kurulum yapmak istediğiniz Blockchain ağ bileşenine göre Sipariş Hizmeti veya Eş düğümlerini seçin.
    - **Düğüm sayısı** - Aşağıdaki iki tür düğüm vardır:
        - Sipariş hizmeti - ağa sağlanan hata toleransı için düğüm sayısını seçin. Desteklenen sipariş düğümü sayısı yalnızca 3,5 ve 7'dir.
        - Eş düğümleri - gereksiniminize göre 1-10 düğüm seçebilirsiniz.
    - **Eş düğümü dünya durumu veritabanı**: LevelDB ve CoucbDB arasında seçim yapın. Bu alan, kullanıcı Kumaş ağ bileşeni açılır durumda eş düğümünü seçtiğinde görüntülenir.
    - **Kumaş kullanıcı adı**: Fabric CA kimlik doğrulaması için kullanılan kullanıcı adını girin.
    - **Fabric CA şifresi**: Fabric CA kimlik doğrulaması için parolayı girin.
    - **Şifreyi onayla**: Fabric CA şifresini onaylayın.
    - **Sertifikalar**: Kumaş CA'yı başlatmak için kendi kök sertifikalarınızı kullanmak istiyorsanız, Fabric CA seçeneği için Yükle kök sertifikasını seçin, aksi takdirde varsayılan olarak Fabric CA kendi imzalı sertifikalar oluşturur.
    - **Kök Sertifikası**: Fabric CA'nın başlatılması gereken kök sertifikasını (ortak anahtar) yükleyin. .pem formatında sertifikalar desteklenir, sertifikalar UTC saat diliminde geçerli olmalıdır.
    - **Kök Sertifikası özel anahtar**: Kök sertifikasının özel anahtarını yükleyin. Hem ortak hem de özel anahtarı bir arada bulunan .pem sertifikanız varsa, bunu buraya da yükleyin.


6. Kumaş ağ bileşenlerinin ayarlanacağı temel altyapı olan Azure Kubernetes küme yapılandırmasını tanımlamak için **AKS küme ayarları** sekmesini seçin.

    ![Azure Kubernetes Hizmet Şablonunda Hyperledger Kumaşı](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-aks-cluster-settings-1.png)

7. Şu ayrıntıları girin:
    - **Kubernetes küme adı**: Oluşturulan AKS kümesinin adı. Bu alan, sağlanan kaynak önekine göre önceden doldurulur, gerekirse değiştirebilirsiniz.
    - **Kubernetes versiyonu**: Kümede dağıtılacak Olan Kubernetes sürümü. Temel bilgiler sekmesinde seçilen bölgeye bağlı olarak, desteklenen **sürümler** değişebilir.
    - **DNS öneki**: AKS kümesi için alan adı sistemi (DNS) adı öneki. Kümeyi oluşturduktan sonra kapsayıcıları yönetirken Kubernetes API'sine bağlanmak için DNS'yi kullanırsınız.
    - **Düğüm boyutu**: Kubernetes düğümünün boyutu, Azure'da bulunan VM Stok tutma birimi (SKS) listesinden seçim yapabilirsiniz. Optimum performans için Standart DS3 v2'yi öneriyoruz.
    - **Düğüm sayısı**: Kümede dağıtılacak Kubernetes düğümlerinin sayısı. Bu düğüm sayısını Kumaş ayarlarında belirtilen HLF düğümlerinin sayısından en az eşit veya daha fazla tutmanızı öneririz.
    - **Hizmet ana istemci kimliği**: Varolan bir hizmet sorumlusunun istemci kimliğini girin veya AKS kimlik doğrulaması için gerekli olan yeni bir kimlik oluşturun. Bkz. [hizmet ilkesi oluşturmak](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-3.2.0#create-a-service-principal)için adımlar.
    - **Servis asıl istemci sırrı**: Servis asıl müşteri kimliğinde verilen hizmet sorumlusunun istemci sırrını girin.
    - **İstemci sırrını onaylayın**: Servis inde verilen istemci sırrını onaylayın.
    - **Kapsayıcı izlemeyi etkinleştir**: AKS günlüklerinin belirtilen Log Analytics çalışma alanına itmesini sağlayan AKS izlemeyi etkinleştirmeyi seçin.
    - **Günlük Analitiği çalışma alanı**: Log analytics çalışma alanı, izleme etkinse oluşturulan varsayılan çalışma alanıyla doldurulur.

8. Yukarıdaki tüm ayrıntıları sağladıktan sonra **Gözden Geçir'i ve oluşturma** sekmesini seçin. İnceleme ve oluşturma sağladığınız değerlerin doğrulanmasını tetikler.
9. Doğrulama geçtikten sonra **oluştur**seçeneğini belirleyebilirsiniz.
Dağıtım genellikle 10-12 dakika sürer, belirtilen AKS düğümlerinin boyutuna ve sayısına bağlı olarak değişebilir.
10. Başarılı dağıtımdan sonra, sağ üst köşedeki Azure bildirimleri aracılığıyla bilgilendirilirsiniz.
11. Şablon dağıtımının bir parçası olarak oluşturulan tüm kaynakları denetlemek için **kaynak grubuna git'i** seçin. Tüm kaynak adları **Temeller** ayarında sağlanan önek ile başlar.

## <a name="build-the-consortium"></a>Konsorsiyumu oluşturun

Sipariş hizmetini ve eş düğümlerini dağıtan blockchain konsorsiyum upostu oluşturmak için aşağıdaki adımları sırayla gerçekleştirmeniz gerekir. Konsorsiyumu kurmanıza, kanal oluşturmanıza ve zincir kodu yüklemenize yardımcı olan Ağ komut dosyanızı (byn.sh) **oluşturun.**

> [!NOTE]
> Sağlanan Ağınızı Oluştur (byn) komut dosyası kesinlikle demo/dist senaryolar için kullanılacaktır. Üretim sınıfı kurulumu için yerel HLF API'lerini kullanmanızı öneririz.

Byn komutunu çalıştırmak için tüm komutlar Azure Bash Komut Satırı Arabirimi (CLI) aracılığıyla yürütülebilir. Azure kabuk web sürümüne giriş yapabilirsiniz ![Azure Kubernetes Hizmet Şablonunda Hyperledger Kumaşı](./media/hyperledger-fabric-consortium-azure-kubernetes-service/arrow.png) Azure portalının sağ üst köşesindeki seçenek. Komut isteminde, bash yazın ve bash CLI'ye geçmek için girin.

Daha fazla bilgi için [Azure kabuğuna](https://docs.microsoft.com/azure/cloud-shell/overview) bakın.

![Azure Kubernetes Hizmet Şablonunda Hyperledger Kumaşı](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-powershell.png)


byn.sh ve fabric-admin.yaml dosyasını indirin.

```bash-interactive
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/consortiumScripts/byn.sh -o byn.sh; chmod 777 byn.sh
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/consortiumScripts/fabric-admin.yaml -o fabric-admin.yaml
```
**Azure CLI Bash kabuğunda ortam değişkenlerinin altında ayarlayın:**

Kanal bilgilerini ve siparişçi kuruluş bilgilerini ayarlama

```bash
SWITCH_TO_AKS_CLUSTER() { az aks get-credentials --resource-group $1 --name $2 --subscription $3; }
ORDERER_AKS_SUBSCRIPTION=<ordererAKSClusterSubscriptionID>
ORDERER_AKS_RESOURCE_GROUP=<ordererAKSClusterResourceGroup>
ORDERER_AKS_NAME=<ordererAKSClusterName>
ORDERER_DNS_ZONE=$(az aks show --resource-group $ORDERER_AKS_RESOURCE_GROUP --name $ORDERER_AKS_NAME --subscription $ORDERER_AKS_SUBSCRIPTION -o json | jq .addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName | tr -d '"')
ORDERER_END_POINT="orderer1.$ORDERER_DNS_ZONE:443"
CHANNEL_NAME=<channelName>
```
Eş kuruluş bilgilerini ayarlama

```bash
PEER_AKS_RESOURCE_GROUP=<peerAKSClusterResourceGroup>
PEER_AKS_NAME=<peerAKSClusterName>
PEER_AKS_SUBSCRIPTION=<peerAKSClusterSubscriptionID>
#Peer organization name is case-sensitive. Specify exactly the same name, which was provided while creating the Peer AKS Cluster.
PEER_ORG_NAME=<peerOrganizationName>
```

Eş ve sipariş veren kuruluşlar arasında çeşitli genel sertifikaları paylaşmak için bir Azure Dosyası paylaşımı oluşturun.

```bash
STORAGE_SUBSCRIPTION=<subscriptionId>
STORAGE_RESOURCE_GROUP=<azureFileShareResourceGroup>
STORAGE_ACCOUNT=<azureStorageAccountName>
STORAGE_LOCATION=<azureStorageAccountLocation>
STORAGE_FILE_SHARE=<azureFileShareName>

az account set --subscription $STORAGE_SUBSCRIPTION
az group create -l $STORAGE_LOCATION -n $STORAGE_RESOURCE_GROUP
az storage account create -n $STORAGE_ACCOUNT -g  $STORAGE_RESOURCE_GROUP -l $STORAGE_LOCATION --sku Standard_LRS
STORAGE_KEY=$(az storage account keys list --resource-group $STORAGE_RESOURCE_GROUP  --account-name $STORAGE_ACCOUNT --query "[0].value" | tr -d '"')
az storage share create  --account-name $STORAGE_ACCOUNT  --account-key $STORAGE_KEY  --name $STORAGE_FILE_SHARE
SAS_TOKEN=$(az storage account generate-sas --account-key $STORAGE_KEY --account-name $STORAGE_ACCOUNT --expiry `date -u -d "1 day" '+%Y-%m-%dT%H:%MZ'` --https-only --permissions lruwd --resource-types sco --services f | tr -d '"')
AZURE_FILE_CONNECTION_STRING="https://$STORAGE_ACCOUNT.file.core.windows.net/$STORAGE_FILE_SHARE?$SAS_TOKEN"
```
**Kanal Yönetim Komutları**

Yeni bir kanal oluşturmak için sipariş kuruluşu AKS küme ve sorun komutuna gidin

```bash
SWITCH_TO_AKS_CLUSTER $ORDERER_AKS_RESOURCE_GROUP $ORDERER_AKS_NAME $ORDERER_AKS_SUBSCRIPTION
./byn.sh createChannel "$CHANNEL_NAME"
```

**Konsorsiyum Yönetim Komutanlıkları**

Bir kanal ve konsorsiyumda bir eş organizasyonu eklemek için verilen sırada komutları aşağıda yürütün.

1. Peer Organization AKS Cluster'a gidin ve Üye Hizmet Sağlama'sını (MSP) Azure Dosya Depolama'ya yükleyin.

    ```bash
    SWITCH_TO_AKS_CLUSTER $PEER_AKS_RESOURCE_GROUP $PEER_AKS_NAME $PEER_AKS_SUBSCRIPTION
    ./byn.sh uploadOrgMSP "$AZURE_FILE_CONNECTION_STRING"
    ```

2. Sipariş eden Organizasyon AKS küme gidin ve kanal ve konsorsiyum akran organizasyonu ekleyin.

    ```bash
    SWITCH_TO_AKS_CLUSTER $ORDERER_AKS_RESOURCE_GROUP $ORDERER_AKS_NAME $ORDERER_AKS_SUBSCRIPTION
    #add peer in consortium
    ./byn.sh addPeerInConsortium "$PEER_ORG_NAME" "$AZURE_FILE_CONNECTION_STRING"
    #add peer in channel
    ./byn.sh addPeerInChannel "$PEER_ORG_NAME" "$CHANNEL_NAME" "$AZURE_FILE_CONNECTION_STRING"
    ```

3. Eş kuruluşuna geri dön ve kanaldaki eş düğümlerine katılmak için komut verme.

    ```bash
    SWITCH_TO_AKS_CLUSTER $PEER_AKS_RESOURCE_GROUP $PEER_AKS_NAME $PEER_AKS_SUBSCRIPTION
    ./byn.sh joinNodesInChannel "$CHANNEL_NAME" "$ORDERER_END_POINT" "$AZURE_FILE_CONNECTION_STRING"
    ```

Benzer şekilde, kanala daha fazla eş kuruluş eklemek için, eş AKS ortam değişkenlerini gerekli eş organizasyonuna göre güncelleştirin ve 1 ile 3 arasındaki adımları uygulayın.

**Zincirkodu yönetimi komutları**

Zincirleme kodla ilgili işlemi gerçekleştirmek için aşağıdaki komutu uygulayın. Bu komutlar tüm işlemleri bir demo chaincode üzerinde gerçekleştirir. Bu demo chaincode iki değişken "a" ve "b" vardır. Zincir kodun anında "a" harfi 1000 ile, "b" ise 2000 ile başharfe işlenir. Zincir kodun her çağrıda 10 birim "a"dan "b"ye aktarılır. Chaincode'daki sorgu işlemi "a" değişkeninin dünya durumunu gösterir.

Eş kuruluş AKS kümesinde yürütülen aşağıdaki komutları yürütün.

```bash
# switch to peer organization AKS cluster. Skip this command if already connected to the required Peer AKS Cluster
SWITCH_TO_AKS_CLUSTER $PEER_AKS_RESOURCE_GROUP $PEER_AKS_NAME $PEER_AKS_SUBSCRIPTION
```
**Chaincode işlem komutları**

```bash
PEER_NODE_NAME="peer<peer#>"
./byn.sh installDemoChaincode "$PEER_NODE_NAME"
./byn.sh instantiateDemoChaincode "$PEER_NODE_NAME" "$CHANNEL_NAME" "$ORDERER_END_POINT" "$AZURE_FILE_CONNECTION_STRING"
./byn.sh invokeDemoChaincode "$PEER_NODE_NAME" "$CHANNEL_NAME" "$ORDERER_END_POINT" "$AZURE_FILE_CONNECTION_STRING"
./byn.sh queryDemoChaincode "$PEER_NODE_NAME" "$CHANNEL_NAME"
```

## <a name="run-native-hlf-operations"></a>Yerel HLF işlemlerini çalıştırın

Müşterilerin AKS'deki HLF ağında Hyperledger yerel komutlarını yürütmeye başlamalarına yardımcı olmak için. Örnek uygulama HLF işlemleri gerçekleştirmek için kumaş NodeJS SDK kullanır sağlanır. Komutlar, yeni kullanıcı kimliği oluşturmak ve kendi zincir kodunuzu yüklemek için sağlanır.

### <a name="before-you-begin"></a>Başlamadan önce

Uygulamanın ilk kurulumu için aşağıdaki komutları izleyin:

- Uygulama dosyalarını indirin
- Bağlantı profili ve yönetici profili oluşturma
- Yönetici kullanıcı kimliğini alma

İlk kurulumu tamamladıktan sonra, aşağıdaki işlemleri gerçekleştirmek için SDK'yı kullanabilirsiniz:

- Kullanıcı kimliği oluşturma
- Zincirkodu işlemleri

Yukarıda belirtilen komutlar Azure Cloud Shell'den yürütülebilir.

### <a name="download-application-files"></a>Uygulama dosyalarını indirin

Uygulama çalıştırmak için ilk kurulum bir klasörde tüm uygulama dosyalarını indirmektir.

**Uygulama klasörü oluşturun ve klasöre girin:**

```bash
mkdir app
cd app
```
Gerekli tüm dosya ve paketleri indirmek için aşağıdaki komutu uygulayın:

```bash-interactive
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/application/setup.sh | bash
```
Bu komutun tüm paketleri yüklemesi zaman alır. Komutun başarılı bir şekilde yürütülmesinden sonra, geçerli dizinde bir `node_modules` klasör görebilirsiniz. Gerekli tüm paketler klasöre `node_modules` yüklenir.

### <a name="generate-connection-profile-and-admin-profile"></a>Bağlantı profili ve yönetici profili oluşturma

Klasör `profile` içinde `app` dizin oluşturma

```bash
cd app
mkdir ./profile
```
Azure bulut bulutu üzerinde bu ortam değişkenlerini ayarlama

```bash
# Organization name whose connection profile is to be generated
ORGNAME=<orgname>
# Organization AKS cluster resource group
AKS_RESOURCE_GROUP=<resourceGroup>
```

Kuruluşun bağlantı profilini ve yönetici profilini oluşturmak için aşağıdaki komutu çalıştırın

```bash
./getConnector.sh $AKS_RESOURCE_GROUP | sed -e "s/{action}/gateway/g"| xargs curl > ./profile/$ORGNAME-ccp.json
./getConnector.sh $AKS_RESOURCE_GROUP | sed -e "s/{action}/admin/g"| xargs curl > ./profile/$ORGNAME-admin.json
```

Profil klasöründe ad `profile` `<orgname>-ccp.json` ve ad ile birlikte kuruluşun `<orgname>-admin.json` bağlantı profilini ve yöneticisini oluşturur.

Benzer şekilde, her siparişçi ve eş kuruluş için bağlantı profili ve yönetici profili oluşturun.


### <a name="import-admin-user-identity"></a>Yönetici kullanıcı kimliğini alma

Son adım, kuruluşun yönetici kullanıcı kimliğini cüzdanda almaktır.

```bash
npm run importAdmin -- -o <orgName>

```
Yukarıdaki komut, yönetici kullanıcı kimliğini cüzdana almak için importAdmin.js'yi yürütür. Komut dosyası yönetici profilinden `<orgname>-admin.json` yönetici kimliğini okur ve HLF işlemlerini yürütmek için cüzdana içeri daraltıyor.

Komut dosyaları, kimlikleri depolamak için dosya sistemi cüzdanını kullanır. Bağlantı profilinde ".cüzdan" alanında belirtilen yola göre bir cüzdan oluşturur. Varsayılan olarak, ".wallet" alanı `<orgname>`ile başharfe `<orgname>` çevrilir, bu da kimlikleri depolamak için geçerli dizinde adı geçen bir klasörün oluşturulduğu anlamına gelir. Başka bir yolda cüzdan oluşturmak istiyorsanız, kayıt yöneticisi kullanıcısını ve diğer HLF işlemlerini çalıştırmadan önce bağlantı profilindeki ".cüzdan" alanını değiştirin.

Benzer şekilde, her kuruluş için yönetici kullanıcı kimliğini içe aktarın.

Komutta geçirilen bağımsız değişkenler hakkında daha fazla ayrıntı için komut yardımına bakın.

```bash
npm run importAdmin -- -h

```

### <a name="user-identity-generation"></a>Kullanıcı kimliği oluşturma

HLF kuruluşu için yeni kullanıcı kimlikleri oluşturmak için verilen sırada aşağıdaki komutları uygulayın.

> [!NOTE]
> Kullanıcı kimliği oluşturma adımlarıyla başlamadan önce, uygulamanın ilk kurulumunun yapıldığından emin olun.

Azure bulut kabuğunda ortam değişkenlerinin altında ayarlama

```bash
# Organization name for which user identity is to be generated
ORGNAME=<orgname>
# Name of new user identity. Identity will be registered with the Fabric-CA using this name.
USER_IDENTITY=<username>

```

Yeni kullanıcıyı kaydedin ve kaydedin

Yeni kullanıcıyı kaydettirmek ve kaydetmek için registerUser.js'yi yürüten aşağıdaki komutu uygulayın. Oluşturulan kullanıcı kimliğini cüzdana kaydeder.

```bash
npm run registerUser -- -o $ORGNAME -u $USER_IDENTITY

```

> [!NOTE]
> Yönetici kullanıcı kimliği, yeni kullanıcı için kayıt komutu vermek için kullanılır. Bu nedenle, bu komutu yürütmeden önce cüzdanda yönetici kullanıcı kimliğinin olması zorunludur. Aksi takdirde, bu komut başarısız olur.

Komutta geçirilen bağımsız değişkenler hakkında daha fazla bilgi için komut yardımına bakın

```bash
npm run registerUser -- -h

```

### <a name="chaincode-operations"></a>Zincirkodu işlemleri


> [!NOTE]
> Herhangi bir chaincode işlemiyle başlamadan önce, uygulamanın ilk kurulumunun yapıldığından emin olun.

Azure Bulutu kabuğunda zincirkoduna özgü ortam değişkenlerinin altında ayarlayın:

```bash
# peer organization name where chaincode is to be installed
ORGNAME=<orgName>
USER_IDENTITY="admin.$ORGNAME"
CC_NAME=<chaincodeName>
CC_VERSION=<chaincodeVersion>
# Language in which chaincode is written. Supported languages are 'node', 'golang' and 'java'
# Default value is 'golang'
CC_LANG=<chaincodeLanguage>
# CC_PATH contains the path where your chaincode is place. In case of go chaincode, this path is relative to 'GOPATH'.
# For example, if your chaincode is present at path '/opt/gopath/src/chaincode/chaincode.go'.
# Then, set GOPATH to '/opt/gopath' and CC_PATH to 'chaincode'
CC_PATH=<chaincodePath>
# 'GOPATH' environment variable. This needs to be set in case of go chaincode only.
export GOPATH=<goPath>
# Channel on which chaincode is to be instantiated/invoked/queried
CHANNEL=<channelName>

````

Aşağıdaki zincirleme kod işlemleri yapılabilir:

- Zincir kodu yükleme
- Anlık zincir kodu
- Zincir kodu çağırma
- Zincir kodu sorgula

### <a name="install-chaincode"></a>Zincir kodu yükleme

Eş kuruluşuna zincir kodu yüklemek için aşağıdaki komutu çalıştırın.

```bash
npm run installCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -l $CC_LANG -v $CC_VERSION

```
Ortam değişkeninde `ORGNAME` ayarlanan kuruluşun tüm eş düğümlerine zincir kodu yükler. Kanalınızda iki veya daha fazla eş kuruluş varsa ve bunların tümüne zincir kodu yüklemek istiyorsanız, her eş kuruluş için komutları ayrı ayrı çalıştırın.

Aşağıdaki adımları izleyin:

- Komutu ayarla `ORGNAME` `<peerOrg1Name>` ve ve vere. `installCC`
- Komutu ayarla `ORGNAME` `<peerOrg2Name>` ve ve vere. `installCC`

  Her eş kuruluş için çalıştırın.

Komutta geçirilen bağımsız değişkenler hakkında daha fazla bilgi için komut yardımına bakın.

```bash
npm run installCC -- -h

```

### <a name="instantiate-chaincode"></a>Anlık zincir kodu

Eş üzerinde chaincode anlık için aşağıdaki komutu çalıştırın.

```bash
npm run instantiateCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -v $CC_VERSION -l $CC_LANG -c $CHANNEL -f <instantiateFunc> -a <instantiateFuncArgs>

```
Instantiation işlev adı ve virgül ayrılmış `<instantiateFunc>` `<instantiateFuncArgs>` bağımsız değişkenler listesini geçirin ve sırasıyla. Örneğin, [fabrcar chaincode](https://github.com/hyperledger/fabric-samples/blob/release/chaincode/fabcar/fabcar.go), zincir kodu için ayarlanmış `<instantiateFunc>` `"Init"` anında `<instantiateFuncArgs>` ve `""`boş dize .

> [!NOTE]
> Komutu kanaldaki herhangi bir eş kuruluşundan bir kez daha yürütün.
> Hareket siparişçiye başarıyla gönderildikten sonra, siparişveren bu hareketi kanaldaki tüm eş kuruluşlara dağıtır. Bu nedenle, zincir kodu kanaldaki tüm eş kuruluşlarında tüm eş düğümleri üzerinde anında.

Komutta geçirilen bağımsız değişkenler hakkında daha fazla bilgi için komut yardımına bakın

```bash
npm run instantiateCC -- -h

```

### <a name="invoke-chaincode"></a>Zincir kodu çağırma

Zincirkodu işlevini çağırmak için aşağıdaki komutu uygulayın:

```bash
npm run invokeCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL -f <invokeFunc> -a <invokeFuncArgs>

```
Geçen işlev adı ve virgül ayrılmış `<invokeFunction>` `<invokeFuncArgs>` bağımsız değişkenler listesini çağırın ve sırasıyla. Fabcar chaincode örneği ile devam, initLedger `<invokeFunction>` işlevini `"initLedger"` `<invokeFuncArgs>` çağırmak `""`için ayarlanmış ve .

> [!NOTE]
> Komutu kanaldaki herhangi bir eş kuruluşundan bir kez daha yürütün.
> Hareket siparişçiye başarıyla gönderildikten sonra, siparişveren bu hareketi kanaldaki tüm eş kuruluşlara dağıtır. Bu nedenle, dünya durumu kanaldaki tüm eş kuruluşlarının tüm eş düğümleri üzerinde güncellenir.

Komutta geçirilen bağımsız değişkenler hakkında daha fazla bilgi için komut yardımına bakın

```bash
npm run invokeCC -- -h

```

### <a name="query-chaincode"></a>Zincir kodu sorgula

Chaincode sorgulamak için aşağıdaki komutu yürüt:

```bash
npm run queryCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL -f <queryFunction> -a <queryFuncArgs>

```

Sorgu işlev adı ve virgül ayrılmış `<queryFunction>` `<queryFuncArgs>` bağımsız değişkenler listesini geçirin ve sırasıyla. Yine, `fabcar` referans olarak chaincode alarak, dünya durumunda ki `<queryFunction>` `"queryAllCars"` tüm `<queryArgs>` `""`araçları sorgulamak için ayarlanmış ve .

Komutta geçirilen bağımsız değişkenler hakkında daha fazla bilgi için komut yardımına bakın

```bash
npm run queryCC -- -h

```
