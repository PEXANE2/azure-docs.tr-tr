---
title: Azure Kubernetes Service (AKS) üzerinde hiper muhasebe doku Consortium
description: Azure Kubernetes hizmetinde hiper muhasebe doku Consortium ağını dağıtma ve yapılandırma
ms.date: 01/08/2020
ms.topic: article
ms.reviewer: v-umha
ms.openlocfilehash: 93f78f2c8bc32a2012e5635e0daec10b8c51d167
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76901621"
---
# <a name="hyperledger-fabric-consortium-on-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) üzerinde hiper muhasebe doku Consortium

Azure Kubernetes Service (AKS) şablonunda hiper muhasebe dokusunu (HLF) Azure 'da bir hiper muhasebe doku Consortium ağını dağıtmak ve yapılandırmak için kullanabilirsiniz.

Bu makaleyi okuduktan sonra şunları yapabilir olacaksınız:

- Hiper muhasebe dokusuna ve hiper muhasebe doku blok zinciri ağının yapı taşlarını oluşturan çeşitli bileşenlere sahip çalışma hakkında bilgi edinin.
- Üretim senaryolarınız için Azure Kubernetes hizmetinde bir hiper muhasebe doku konsorsiyusunu dağıtmayı ve yapılandırmayı öğrenin.

## <a name="hyperledger-fabric-consortium-architecture"></a>Hiper muhasebe Fabric Consortium mimarisi

Azure 'da hiper muhasebe doku ağı oluşturmak için, iş düğümleri ile sıralama hizmetini ve organizasyonu dağıtmanız gerekir. Şablon dağıtımının bir parçası olarak oluşturulan farklı temel bileşenler şunlardır:

- **Orderer düğümleri**: muhasebedeki işlem sıralamaktan sorumlu bir düğüm. Diğer düğümlerle birlikte, sıralı düğümler hiper muhasebe doku ağının sıralama hizmetini oluşturur.

- **Eş düğümleri**: Öncelikle, ağın bu temel öğeleri ile ilgili temel elemanlar ve akıllı sözleşmeleri barındıran bir düğümdür.

- **Fabrıc CA**: doku CA 'Sı, hiper muhasebe dokusu Için sertifika yetkilisdir (CA). Doku CA 'sı, sertifika yetkilisini barındıran sunucu işlemini başlatıp başlatmanıza olanak tanır. Kimlikleri ve sertifikaları yönetmenizi sağlar. Şablonun bir parçası olarak dağıtılan her bir AKS kümesi, varsayılan olarak bir doku CA Pod 'ı olacaktır.

- **Couşdb veya leveldb**: Eş düğümleri için World durum veritabanı, Leveldb veya couşdb içinde depolanabilir. LevelDB, eş düğümüne eklenmiş olan varsayılan durum veritabanıdır ve chaincode verilerini basit anahtar-değer çiftleri olarak depolar ve yalnızca anahtar, anahtar aralığı ve bileşik anahtar sorgularını destekler. Couşdb, chaincode veri değerleri JSON olarak modellendiği zaman zengin sorguları destekleyen, isteğe bağlı alternatif bir durum veritabanıdır.

Dağıtım üzerindeki şablon, aboneliğinizdeki çeşitli Azure kaynaklarını alır. Dağıtılan farklı Azure kaynakları şunlardır:

- **Aks kümesi**: müşteri tarafından sunulan giriş parametrelerine göre yapılandırılan Azure Kubernetes kümesi. AKS kümesinde, hiper muhasebe doku ağı bileşenlerini çalıştırmak için yapılandırılmış çeşitli yığınlar vardır. Oluşturulan farklı pod 'ler şunlardır:

  - **Yapı Araçları**: doku Aracı, hiper muhasebe doku bileşenlerinin yapılandırılmasından sorumludur.
  - **Orderer/peer Pod**: HLF ağının düğümleri.
  - **Proxy**: istemci uygulamalarının aks kümesiyle arabirim içinde yer aldığı bir ngnx proxy Pod 'si.
  - **Fabrıc CA**: doku CA 'sını çalıştıran Pod.
- **PostgreSQL**: Fabric CA kimliklerini sürdürmek Için PostgreSQL örneği dağıtıldı.

- **Azure Anahtar Kasası**: yapı CA kimlik bilgilerini ve müşteri tarafından sağlanmış kök sertifikaları kaydetmek için bir Anahtar Kasası örneği dağıtılır. Bu, şablon dağıtımı yeniden denenme durumunda kullanılan bu, şablonun mekanizması idare ediyor.
- **Azure yönetilen disk**: Azure yönetilen disk, muhasebe ve eş düğüm dünya durumu veritabanı için kalıcı mağazaya yöneliktir.
- **Genel IP**: küme ile arabirim oluşturma için dağıtılan aks KÜMESININ genel IP uç noktası.

## <a name="hyperledger-fabric-blockchain-network-setup"></a>Hiper muhasebe doku blok zinciri ağı kurulumu

Başlamak için birkaç sanal makine ve standart depolama hesabı dağıtımı destekleyebilen bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı oluşturabilirsiniz](https://azure.microsoft.com/free/).

Aşağıdaki adımları kullanarak hiper muhasebe doku blok zinciri ağını ayarlayın:

- [Sipariş/eş kuruluşu dağıtma](#deploy-the-ordererpeer-organization)
- [Consortium oluşturma](#build-the-consortium)
- [Yerel HLF işlemleri çalıştırma](#run-native-hlf-operations)

## <a name="deploy-the-ordererpeer-organization"></a>Sipariş/eş kuruluşu dağıtma

HLF ağ bileşenleri dağıtımına başlamak için [Azure Portal](https://portal.azure.com)gidin. **Azure Kubernetes hizmetinde hiper muhasebe dokusunu**aramak > **blok zinciri > kaynak oluştur** ' u seçin.

1. Şablon dağıtımını başlatmak için **Oluştur** ' u seçin. **Azure Kubernetes hizmetinde hiper defter oluşturma dokusu** görüntülenir.

    ![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-fabric-aks.png)

2. **Temel bilgiler** sayfasında proje ayrıntılarını girin.

    ![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-basics.png)

3. Şu ayrıntıları girin:
    - **Abonelik**: HLF ağ bileşenlerini dağıtmak istediğiniz abonelik adını seçin.
    - **Kaynak grubu**: yeni bir kaynak grubu oluşturun veya var olan boş bir kaynak grubunu seçin, kaynak grubu, şablonun bir parçası olarak dağıtılan tüm kaynakları tutar.
    - **Bölge**: HLF bileşenleri Için Azure Kubernetes kümesini dağıtmak istediğiniz Azure bölgesini seçin. Şablon, AKS 'nin kullanılabilir olduğu tüm bölgelerde kullanılabilir, aboneliğinizin sanal makine (VM) kota sınırına ulaşamadığı bir bölge seçtiğinizden emin olun.
    - **Kaynak öneki**: dağıtılan kaynakların adlandırılmasının ön eki. Kaynak ön eki altıdan az karakter uzunluğunda olmalı ve karakterlerin birleşimi hem sayı hem de harf içermelidir.
4. Dağıtılacak HLF ağ bileşenlerini tanımlamak için **doku ayarları** sekmesini seçin.

    ![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-settings.png)

5. Şu ayrıntıları girin:
    - **Kuruluş adı**: çeşitli veri düzlemi işlemleri için gerekli olan doku kuruluşunun adı.
    - **Fabric ağ bileşeni**: ayarlamak Istediğiniz blok zinciri ağ bileşenine göre hizmet veya eş düğümleri sıralama seçeneğini belirleyin.
    - **Düğüm sayısı** -aşağıdakiler iki düğüm türüdür:
        - Hizmeti sıralama-ağ için hataya dayanıklılık sağlanacak düğüm sayısını seçin. Yalnızca 3, 5 ve 7 desteklenen orderer düğüm sayısıdır.
        - Eş düğümleri-gereksiniminize göre 1-10 düğümleri seçebilirsiniz.
    - **Eş düğüm dünya durumu veritabanı**: leveldb Ile CoucbDB arasında seçim yapın. Bu alan, Kullanıcı doku ağ bileşeni açılan kutusunda eş düğüm seçtiğinde görüntülenir.
    - **Fabric Kullanıcı adı**: doku CA kimlik doğrulaması için kullanılan Kullanıcı adını girin.
    - **Fabrıc CA parolası**: doku CA kimlik doğrulaması için parolayı girin.
    - **Parolayı onaylayın**: doku CA parolasını onaylayın.
    - **Sertifikalar**: doku CA 'sını başlatmak için kendi kök sertifikalarınızı kullanmak istiyorsanız doku CA 'sı için kök sertifikayı karşıya yükle seçeneğini belirleyin, aksi durumda varsayılan doku CA 'sı otomatik olarak imzalanan sertifikalar oluşturur.
    - **Kök sertifika**: doku CA 'sının başlatılması gereken kök sertifikayı (ortak anahtar) karşıya yükleyin. . Pek biçimindeki sertifikalar desteklenir, sertifikaların UTC saat diliminde geçerli olması gerekir.
    - **Kök sertifika özel anahtarı**: kök sertifikanın özel anahtarını karşıya yükleyin. Hem ortak hem de özel anahtar birleştirilmiş bir. ped sertifikanız varsa, bu sertifikayı da buraya yükleyin.


6. Doku ağ bileşenlerinin oluşturulacağı temeldeki altyapı olan Azure Kubernetes kümesi yapılandırmasını tanımlamak için **aks küme ayarları** sekmesini seçin.

    ![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-aks-cluster-settings-1.png)

7. Şu ayrıntıları girin:
    - **Kubernetes küme adı**: oluşturulan aks kümesinin adı. Bu alan, belirtilen kaynak ön ekine göre önceden doldurulur, gerekirse değiştirebilirsiniz.
    - **Kubernetes sürümü**: kümede dağıtılacak Kubernetes sürümü. **Temel bilgiler** sekmesinde seçilen bölgeye bağlı olarak, kullanılabilir desteklenen sürümler değişebilir.
    - **DNS öneki**: aks kümesi için etki alanı adı SISTEMI (DNS) adı ön eki. Kümeyi oluşturduktan sonra kapsayıcıları yönetirken Kubernetes API 'sine bağlanmak için DNS kullanırsınız.
    - **Düğüm boyutu**: Kubernetes düğümünün boyutu, Azure 'DA bulunan VM stok tutma birimi (SKU 'lar) listesinden seçim yapabilirsiniz. En iyi performansı elde etmek için standart DS3 v2 önerilir.
    - **Düğüm sayısı**: kümede dağıtılacak Kubernetes düğümlerinin sayısı. Bu düğüm sayısının doku ayarlarında belirtilen HLF düğümlerinin en az birine eşit veya daha fazla tutulması önerilir.
    - **Hizmet sorumlusu ISTEMCI kimliği**: mevcut bir hizmet SORUMLUSUNUN istemci kimliğini girin veya aks kimlik doğrulaması için gerekli olan yeni bir oluştur. Bkz. [hizmet sorumlusu oluşturma](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-3.2.0#create-a-service-principal)adımları.
    - **Hizmet sorumlusu istemci parolası**: hizmet sorumlusu istemci kimliğinde belirtilen hizmet sorumlusunun istemci gizli anahtarını girin.
    - **İstemci parolasını onaylayın**: hizmet sorumlusu istemci gizli anahtarı 'nda belirtilen istemci gizliliğini onaylayın.
    - **Kapsayıcı Izlemeyi etkinleştir**: aks izlemesini etkinleştirmek için seçin. Bu, aks günlüklerinin belirtilen Log Analytics çalışma alanına gönderim yapmasını sağlar.
    - **Log Analytics çalışma alanı**: Log Analytics çalışma alanı, izleme etkinse oluşturulan varsayılan çalışma alanı ile doldurulur.

8. Yukarıdaki tüm ayrıntıları sağladıktan sonra **gözden geçir ve oluştur** sekmesini seçin. İnceleme ve oluşturma, verdiğiniz değerler için doğrulamayı tetikler.
9. Doğrulama başarılı olduktan sonra **Oluştur**' u seçebilirsiniz.
Dağıtım genellikle 10-12 dakika sürer, belirtilen AKS düğümlerinin boyutuna ve sayısına bağlı olarak değişebilir.
10. Başarılı dağıtımdan sonra, sağ üst köşedeki Azure bildirimleri aracılığıyla bilgilendirilirsiniz.
11. Şablon dağıtımının bir parçası olarak oluşturulan tüm kaynakları denetlemek için **kaynak grubuna git** ' i seçin. Tüm kaynak adları, **temel bilgiler** ayarında belirtilen önekle başlayacaktır.

## <a name="build-the-consortium"></a>Consortium oluşturma

Sıralama hizmetini ve eş düğümlerini dağıtan blok zinciri Konsorsiyumu gönderisini oluşturmak için aşağıdaki adımları sırayla gerçekleştirmeniz gerekir. Konsorsiyumu ayarlama, kanal oluşturma ve chaincode yükleme konusunda size yardımcı olan **ağ komut dosyanızı** (Byn.sh) oluşturun.

> [!NOTE]
> Sunulan ağ (BYN) komut dosyanızı oluşturma işlemi, demo/DevTest senaryoları için kesinlikle kullanılır. Üretim sınıfı kurulumu için yerel HLF API 'Lerini kullanmanızı öneririz.

Byn betiğini çalıştırmaya yönelik tüm komutlar Azure Bash komut satırı arabirimi (CLı) aracılığıyla yürütülebilir. Azure Shell web sürümünde oturum açabilirsiniz ![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/arrow.png) seçeneğinin sağ üst köşesinde bulunan seçeneğini Azure portal. Komut isteminde Bash CLı yazın.

Daha fazla bilgi için bkz. [Azure kabuğu](https://docs.microsoft.com/azure/cloud-shell/overview) .

![Azure Kubernetes hizmet şablonunda hiper muhasebe dokusu](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-powershell.png)


Byn.sh ve Fabric-admin. YAML dosyasını indirin.

```bash-interactive
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/consortiumScripts/byn.sh -o byn.sh; chmod 777 byn.sh
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/consortiumScripts/fabric-admin.yaml -o fabric-admin.yaml
```
**Azure CLI bash kabuğu 'nda ortam değişkenlerinin aşağıdaki şekilde ayarlayın**:

Kanal bilgilerini ve sipariş kuruluş bilgilerini ayarlama

```bash
SWITCH_TO_AKS_CLUSTER() { az aks get-credentials --resource-group $1 --name $2 --subscription $3; }
ORDERER_AKS_SUBSCRIPTION=<ordererAKSClusterSubscriptionID>
ORDERER_AKS_RESOURCE_GROUP=<ordererAKSClusterResourceGroup>
ORDERER_AKS_NAME=<ordererAKSClusterName>
ORDERER_DNS_ZONE=
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

Eş ve sipariş kuruluşları arasında çeşitli genel sertifikaları paylaşmak için bir Azure dosya paylaşımının oluşturulması.

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
**Kanal yönetimi komutları**

Yeni bir kanal oluşturmak için orderer kuruluş AKS kümesine git ve sorun komutu

```bash
SWITCH_TO_AKS_CLUSTER $ORDERER_AKS_RESOURCE_GROUP $ORDERER_AKS_NAME $ORDERER_AKS_SUBSCRIPTION
./byn.sh createChannel "$CHANNEL_NAME"
```

**Konsorsiyum yönetim komutları**

Bir kanala ve konsorsiyuma bir eşdüzey kuruluş eklemek için aşağıdaki komutları belirtilen sırada yürütün.

1. Eş kuruluş AKS kümesine gidin ve üye hizmeti 'nin bir Azure dosya depolama alanına (MSP) sağlamasını yükleyin.

    ```bash
    SWITCH_TO_AKS_CLUSTER $PEER_AKS_RESOURCE_GROUP $PEER_AKS_NAME $PEER_AKS_SUBSCRIPTION
    ./byn.sh uploadOrgMSP "$AZURE_FILE_CONNECTION_STRING"
    ```

2. Orderer kuruluş AKS kümesine gidin ve eş kuruluşu kanala ve konsorsiyuma ekleyin.

    ```bash
    SWITCH_TO_AKS_CLUSTER $ORDERER_AKS_RESOURCE_GROUP $ORDERER_AKS_NAME $ORDERER_AKS_SUBSCRIPTION
    #add peer in consortium
    ./byn.sh addPeerInConsortium "$PEER_ORG_NAME" "$AZURE_FILE_CONNECTION_STRING"
    #add peer in channel
    ./byn.sh addPeerInChannel "$PEER_ORG_NAME" "$CHANNEL_NAME" "$AZURE_FILE_CONNECTION_STRING"
    ```

3. Eş kuruluşa geri dönün ve kanaldaki eş düğümlerine katılması için komutu verin.

    ```bash
    SWITCH_TO_AKS_CLUSTER $PEER_AKS_RESOURCE_GROUP $PEER_AKS_NAME $PEER_AKS_SUBSCRIPTION
    ./byn.sh joinNodesInChannel "$CHANNEL_NAME" "$ORDERER_END_POINT" "$AZURE_FILE_CONNECTION_STRING"
    ```

Benzer şekilde, kanala daha fazla eş kurum eklemek için, eş AKS ortam değişkenlerini gerekli eş kuruluşa göre güncelleştirin ve 1 ile 3 arasındaki adımları yürütün.

**Chaincode yönetim komutları**

Chaincode ile ilgili işlemi gerçekleştirmek için aşağıdaki komutu yürütün. Bu komutlar bir demo chaincode üzerinde tüm işlemleri gerçekleştirir. Bu demo chaincode, "a" ve "b" iki değişkenine sahiptir. Chaincode 'un örneklenmesi sırasında, "a" 1000 ile başlatılır ve "b" 2000 ile başlatılır. Chaincode 'un her çağrılışında, 10 birim "a" dan "b" öğesine aktarılır. Chaincode üzerinde sorgu işlemi, "a" değişkeninin dünya durumunu gösterir.

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

## <a name="run-native-hlf-operations"></a>Yerel HLF işlemleri çalıştırma

Müşterilerin AKS 'de HLF ağı üzerinde hiper muhasebe yerel komutları yürütmeye başlama hakkında yardım almak için. Örnek uygulama, HLF işlemlerini gerçekleştirmek için Fabric NodeJS SDK 'sını kullanan sağlanır. Komutlar yeni kullanıcı kimliği oluşturmak ve kendi chaincode uygulamanızı yüklemek için sağlanır.

### <a name="before-you-begin"></a>Başlamadan önce

Uygulamanın ilk kurulumu için aşağıdaki komutları izleyin:

- Uygulama dosyalarını indirin
- Bağlantı profili ve yönetici profili oluştur
- Yönetici Kullanıcı kimliğini içeri aktar

İlk kurulumu tamamladıktan sonra, aşağıdaki işlemleri gerçekleştirmek için SDK 'Yı kullanabilirsiniz:

- Kullanıcı kimliği oluşturma
- Chaincode işlemleri

Yukarıda bahsedilen komutlar Azure Cloud Shell çalıştırılabilir.

### <a name="download-application-files"></a>Uygulama dosyalarını indirin

Uygulamayı çalıştırmaya yönelik ilk kurulum, tüm uygulama dosyalarını bir klasöre indirmenin bir klasörüdür.

**Uygulama klasörü oluşturun ve klasöre girin**:

```bash
mkdir app
cd app
```
Gerekli tüm dosya ve paketleri indirmek için aşağıdaki komutu yürütün:

```bash-interactive
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/application/setup.sh | bash
```
Bu komutun tüm paketleri yüklemesi zaman alır. Komutun başarılı bir şekilde yürütülmesi tamamlandıktan sonra, geçerli dizinde bir `node_modules` klasörü görebilirsiniz. Gerekli tüm paketler `node_modules` klasörüne yüklenir.

### <a name="generate-connection-profile-and-admin-profile"></a>Bağlantı profili ve yönetici profili oluştur

`app` klasörü içinde `profile` dizin oluştur

```bash
cd app
mkdir ./profile
```
Bu ortam değişkenlerini Azure Cloud Shell 'de ayarla

```bash
# Organization name whose connection profile is to be generated
ORGNAME=<orgname>
# Organization AKS cluster resource group
AKS_RESOURCE_GROUP=<resourceGroup>
```

Kuruluşun bağlantı profilini ve yönetici profilini oluşturmak için aşağıdaki komutu yürütün

```bash
./getConnector.sh $AKS_RESOURCE_GROUP | sed -e "s/{action}/gateway/g"| xargs curl > ./profile/$ORGNAME-ccp.json
./getConnector.sh $AKS_RESOURCE_GROUP | sed -e "s/{action}/admin/g"| xargs curl > ./profile/$ORGNAME-admin.json
```

Bu, sırasıyla `<orgname>-ccp.json` ve `<orgname>-admin.json` adlı profil klasöründe kuruluşun bağlantı profilini ve yönetici `profile` oluşturacaktır.

Benzer şekilde, her bir sipariş ve eş kuruluş için bağlantı profili ve yönetici profili oluşturun.


### <a name="import-admin-user-identity"></a>Yönetici Kullanıcı kimliğini içeri aktar

Son adım, kuruluşun yönetici kullanıcı kimliğini cüzdan içinde içeri aktarırsınız.

```bash
npm run importAdmin -- -o <orgName>

```
Yukarıdaki komut, yönetici kullanıcı kimliğini cüzdan 'e aktarmak için ımportadmin. js ' i yürütür. Komut dosyası yönetici profilinden yönetici kimliğini okur `<orgname>-admin.json` ve HLF işlemlerini yürütmek için bunu cüzdan 'e aktarır.

Betikler, kimlikleri depolamak için dosya sistemi cüzdan kullanır. Bağlantı profilindeki ". cüzdan" alanında belirtilen yola göre bir cüzdan oluşturur. Varsayılan olarak, ". cüzdan" alanı `<orgname>`ile başlatılır, bu da kimlikleri depolamak için geçerli dizinde `<orgname>` adlı bir klasör oluşturulur. Başka bir yolda cüzdan oluşturmak istiyorsanız, kayıt yönetici kullanıcısını ve diğer HLF işlemlerini çalıştırmadan önce bağlantı profilindeki ". cüzdan" alanını değiştirin.

Benzer şekilde, her kuruluş için yönetici kullanıcı kimliğini içeri aktarın.

Komutuna geçirilen bağımsız değişkenler hakkında daha fazla bilgi için komut yardımına bakın.

```bash
npm run importAdmin -- -h

```

### <a name="user-identity-generation"></a>Kullanıcı kimliği oluşturma

HLF kuruluşunun yeni kullanıcı kimliklerini oluşturmak için aşağıdaki komutları belirtilen sırada yürütün.

> [!NOTE]
> Kullanıcı kimliği oluşturma adımlarıyla başlatmadan önce, uygulamanın ilk kurulumunun tamamlandığından emin olun.

Azure Cloud Shell 'de ortam değişkenlerinin altına ayarla

```bash
# Organization name for which user identity is to be generated
ORGNAME=<orgname>
# Name of new user identity. Identity will be registered with the Fabric-CA using this name.
USER_IDENTITY=<username>

```

Yeni Kullanıcı Kaydet ve Kaydet

Yeni kullanıcıyı kaydetmek ve kaydetmek için, registerUser. js ' yi yürüten aşağıdaki komutu yürütün. Oluşturulan kullanıcı kimliğini cüzdan 'e kaydeder.

```bash
npm run registerUser -- -o $ORGNAME -u $USER_IDENTITY

```

> [!NOTE]
> Yönetici Kullanıcı kimliği, Yeni Kullanıcı için Register komutunu vermek üzere kullanılır. Bu nedenle, bu komutu yürütmeden önce, kullanıcının cüzdan içinde yönetici kullanıcı kimliğinin olması zorunludur. Aksi takdirde, bu komut başarısız olur.

Komutta geçirilen bağımsız değişkenler hakkında daha fazla bilgi için komut yardımına bakın

```bash
npm run registerUser -- -h

```

### <a name="chaincode-operations"></a>Chaincode işlemleri


> [!NOTE]
> Herhangi bir chaincode işlemine başlamadan önce, uygulamanın ilk kurulumunun tamamlandığından emin olun.

Azure Cloud Shell 'de chaincode 'a özgü ortam değişkenlerinin altına ayarlayın:

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

Aşağıdaki chaincode işlemleri gerçekleştirilebilir:

- Chaincode 'u yükler
- Chaincode örneği oluşturma
- Chaincode çağırma
- Sorgu chaincode

### <a name="install-chaincode"></a>Chaincode 'u yükler

Eş kuruluşa chaincode 'u yüklemek için aşağıdaki komutu yürütün.

```bash
npm run installCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -l $CC_LANG -v $CC_VERSION

```
`ORGNAME` ortam değişkeninde ayarlanan kuruluşun tüm eşdüzey düğümlerine chaincode yükler. Kanalınıza iki veya daha fazla eş kuruluş varsa ve bunların tümüne chaincode yüklemek istiyorsanız, her eş kuruluş için komutları ayrı olarak yürütün.

Adımları izleyin:

- `ORGNAME` `<peerOrg1Name>` ve `installCC` komutu vermek için ayarlayın.
- `ORGNAME` `<peerOrg2Name>` ve `installCC` komutu vermek için ayarlayın.

  Her eş kuruluş için yürütün.

Komutuna geçirilen bağımsız değişkenler hakkında daha fazla bilgi için yardım 'a bakın.

```bash
npm run installCC -- -h

```

### <a name="instantiate-chaincode"></a>Chaincode örneği oluşturma

Eşe chaincode örneğini oluşturmak için aşağıdaki komutu yürütün.

```bash
npm run instantiateCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -v $CC_VERSION -l $CC_LANG -c $CHANNEL -f <instantiateFunc> -a <instantiateFuncArgs>

```
`<instantiateFunc>` ve `<instantiateFuncArgs>` bağımsız değişken işlev adını ve virgülle ayrılmış listesini sırasıyla geçirin. Örneğin, [fabrcar chaincode](https://github.com/hyperledger/fabric-samples/blob/release/chaincode/fabcar/fabcar.go)'da, `<instantiateFunc>` chaincode kümesini `"Init"` ve boş dize `""``<instantiateFuncArgs>` örneğini oluşturmak için.

> [!NOTE]
> Kanalda herhangi bir eş kuruluştan bir kez komutunu yürütün.
> İşlem düzenli olarak sipariş 'e gönderildikten sonra, sipariş bu işlemi kanaldaki tüm eş kuruluşlara dağıtır. Bu nedenle, chaincode, kanaldaki tüm eş kuruluşlardaki tüm eşdüzey düğümlerde oluşturulur.

Komutta geçirilen bağımsız değişkenler hakkında daha fazla bilgi için komut yardımına bakın

```bash
npm run instantiateCC -- -h

```

### <a name="invoke-chaincode"></a>Chaincode çağırma

Chaincode işlevini çağırmak için aşağıdaki komutu yürütün:

```bash
npm run invokeCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL -f <invokeFunc> -a <invokeFuncArgs>

```
`<invokeFunction>` ve `<invokeFuncArgs>` bağımsız değişkenlerin çağırma işlev adını ve virgülle ayrılmış listesini sırasıyla geçirin. Fabcar chaincode örneğine devam ederek, ınitledger işlev kümesi `<invokeFunction>` `"initLedger"` ve `""``<invokeFuncArgs>`.

> [!NOTE]
> Kanalda herhangi bir eş kuruluştan bir kez komutunu yürütün.
> İşlem düzenli olarak sipariş 'e gönderildikten sonra, sipariş bu işlemi kanaldaki tüm eş kuruluşlara dağıtır. Bu nedenle, dünyanın durumu, kanaldaki tüm eş kuruluşların tüm eşdüzey düğümlerinde güncelleştirilir.

Komutta geçirilen bağımsız değişkenler hakkında daha fazla bilgi için komut yardımına bakın

```bash
npm run invokeCC -- -h

```

### <a name="query-chaincode"></a>Sorgu chaincode

Chaincode 'u sorgulamak için aşağıdaki komutu yürütün:

```bash
npm run queryCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL -f <queryFunction> -a <queryFuncArgs>

```

Sorgu işlev adı ve `<queryFunction>` bağımsız değişkenlerin virgülle ayrılmış listesini sırasıyla `<queryFuncArgs>` geçirin. Bu durumda `fabcar` chaincode 'u başvuru olarak alarak, dünyanın tüm arabasını `"queryAllCars"` `<queryFunction>` ve `""``<queryArgs>`.

Komutta geçirilen bağımsız değişkenler hakkında daha fazla bilgi için komut yardımına bakın

```bash
npm run queryCC -- -h

```
