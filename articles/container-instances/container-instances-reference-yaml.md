---
title: Kapsayıcı grubu için YAML başvurusu
description: Bir kapsayıcı grubunu yapılandırmak için Azure Container Instances tarafından desteklenen YAML dosyası başvurusu
ms.topic: article
ms.date: 08/12/2019
ms.openlocfilehash: 5603f2e0f63c4f83a6d3761feb540abb8b8b7d5c
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74533500"
---
# <a name="yaml-reference-azure-container-instances"></a>YAML başvurusu: Azure Container Instances

Bu makalede bir [kapsayıcı grubunu](container-instances-container-groups.md)yapılandırmak için Azure Container Instances tarafından desteklenen YAML dosyasının sözdizimi ve özellikleri ele alınmaktadır. Azure CLı 'de [az Container Create][az-container-create] komutuna grup yapılandırmasını girmek IÇIN BIR YAML dosyası kullanın. 

YAML dosyası, tekrarlanabilir dağıtımlar için bir kapsayıcı grubu yapılandırmanın kolay bir yoludur. Bir kapsayıcı grubu oluşturmak veya güncelleştirmek için bir [Kaynak Yöneticisi şablonu](/azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups) veya Azure Container Instances SDK 'ları kullanmanın kısa bir alternatifidir.

> [!NOTE]
> Bu başvuru, Azure Container Instances REST API sürümü `2018-10-01`için YAML dosyaları için geçerlidir.

## <a name="schema"></a>Şema 

YAML dosyasının şeması, anahtar özelliklerini vurgulamak için yorumlar da dahil olmak üzere aşağıdaki gibidir. Bu şemadaki özelliklerin bir açıklaması için, bkz. [özellik değerleri](#property-values) bölümü.

```yml
name: string  # Name of the container group
apiVersion: '2018-10-01'
location: string
tags: {}
identity: 
  type: string
  userAssignedIdentities: {}
properties: # Properties of container group
  containers: # Array of container instances in the group
  - name: string # Name of an instance
    properties: # Properties of an instance
      image: string # Container image used to create the instance
      command:
      - string
      ports: # Exposed ports on the instance
      - protocol: string
        port: integer
      environmentVariables:
      - name: string
        value: string
        secureValue: string
      resources: # Resource requirements of the instance
        requests:
          memoryInGB: number
          cpu: number
          gpu:
            count: integer
            sku: string
        limits:
          memoryInGB: number
          cpu: number
          gpu:
            count: integer
            sku: string
      volumeMounts: # Array of volume mounts for the instance
      - name: string
        mountPath: string
        readOnly: boolean
      livenessProbe:
        exec:
          command:
          - string
        httpGet:
          path: string
          port: integer
          scheme: string
        initialDelaySeconds: integer
        periodSeconds: integer
        failureThreshold: integer
        successThreshold: integer
        timeoutSeconds: integer
      readinessProbe:
        exec:
          command:
          - string
        httpGet:
          path: string
          port: integer
          scheme: string
        initialDelaySeconds: integer
        periodSeconds: integer
        failureThreshold: integer
        successThreshold: integer
        timeoutSeconds: integer
  imageRegistryCredentials: # Credentials to pull a private image
  - server: string
    username: string
    password: string
  restartPolicy: string
  ipAddress: # IP address configuration of container group
    ports:
    - protocol: string
      port: integer
    type: string
    ip: string
    dnsNameLabel: string
  osType: string
  volumes: # Array of volumes available to the instances
  - name: string
    azureFile:
      shareName: string
      readOnly: boolean
      storageAccountName: string
      storageAccountKey: string
    emptyDir: {}
    secret: {}
    gitRepo:
      directory: string
      repository: string
      revision: string
  diagnostics:
    logAnalytics:
      workspaceId: string
      workspaceKey: string
      logType: string
      metadata: {}
  networkProfile: # Virtual network profile for container group
    id: string
  dnsConfig: # DNS configuration for container group
    nameServers:
    - string
    searchDomains: string
    options: string
```

## <a name="property-values"></a>Özellik değerleri

Aşağıdaki tablolarda, şemada ayarlamanız gereken değerler açıklanır.

<a id="Microsoft.ContainerInstance/containerGroups" />

### <a name="microsoftcontainerinstancecontainergroups-object"></a>Microsoft. Containerınstance/containerGroups nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  ad | string | Yes | Kapsayıcı grubunun adı. |
|  apiVersion | yardımının | Yes | 2018-10-01 |
|  location | string | Hayır | Kaynak konumu. |
|  etiketler | object | Hayır | Kaynak etiketleri. |
|  kimlik | object | Hayır | Yapılandırılmışsa kapsayıcı grubunun kimliği. - [Containergroupıdentity nesnesi](#ContainerGroupIdentity) |
|  properties | object | Yes | [ContainerGroupProperties nesnesi](#ContainerGroupProperties) |


<a id="ContainerGroupIdentity" />

### <a name="containergroupidentity-object"></a>Containergroupıdentity nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  type | yardımının | Hayır | Kapsayıcı grubu için kullanılan kimlik türü. ' SystemAssigned, Useratandı ' türü hem örtük olarak oluşturulan bir kimliği hem de Kullanıcı tarafından atanan kimlikleri içerir. ' None ' türü, kapsayıcı grubundaki tüm kimlikleri kaldırır. -SystemAssigned, Useratandı, SystemAssigned, Useratandı, None |
|  Userassignedıdentities | object | Hayır | Kapsayıcı grubuyla ilişkili kullanıcı kimliklerinin listesi. Kullanıcı kimlik sözlüğü anahtar başvuruları Azure Resource Manager kaynak kimlikleri şu biçimde olacaktır: '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName} '. |


<a id="ContainerGroupProperties" />

### <a name="containergroupproperties-object"></a>ContainerGroupProperties nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  kapsayıcılar | array | Yes | Kapsayıcı grubu içindeki kapsayıcılar. - [kapsayıcı nesnesi](#Container) |
|  imageRegistryCredentials | array | Hayır | Kapsayıcı grubunun oluşturulduğu görüntü kayıt defteri kimlik bilgileri. - [ImageRegistryCredential nesnesi](#ImageRegistryCredential) |
|  restartPolicy | yardımının | Hayır | Kapsayıcı grubundaki tüm kapsayıcılar için ilkeyi yeniden başlatın. - `Always` her zaman yeniden Başlat-`OnFailure` hata durumunda yeniden başlatma `Never`. -Always, OnFailure, hiçbir zaman |
|  Belirlenemiyor | object | Hayır | Kapsayıcı grubunun IP adresi türü. - [IPAddress nesnesi](#IpAddress) |
|  osType | yardımının | Yes | Kapsayıcı grubundaki kapsayıcılar için gereken işletim sistemi türü. -Windows veya Linux |
|  volumes | array | Hayır | Bu kapsayıcı grubundaki kapsayıcılarla bağlankalebilecek birimlerin listesi. - [Volume nesnesi](#Volume) |
|  tanılama | object | Hayır | Bir kapsayıcı grubu için tanılama bilgileri. - [Containergroupdiagnostics nesnesi](#ContainerGroupDiagnostics) |
|  networkProfile | object | Hayır | Bir kapsayıcı grubu için ağ profili bilgileri. - [Containergroupnetworkprofile nesnesi](#ContainerGroupNetworkProfile) |
|  dnsConfig | object | Hayır | Bir kapsayıcı grubu için DNS yapılandırma bilgileri. - [Dnsconfiguration nesnesi](#DnsConfiguration) |


<a id="Container" />

### <a name="container-object"></a>Kapsayıcı nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  ad | string | Yes | Kapsayıcı örneğinin Kullanıcı tarafından sağlanmış adı. |
|  properties | object | Yes | Kapsayıcı örneğinin özellikleri. - [Containerproperties nesnesi](#ContainerProperties) |


<a id="ImageRegistryCredential" />

### <a name="imageregistrycredential-object"></a>ImageRegistryCredential nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  sunucu | string | Yes | "Http" ve "https" gibi bir protokol olmadan Docker görüntü kayıt defteri sunucusu. |
|  kullanıcı adı | string | Yes | Özel kayıt defteri için Kullanıcı adı. |
|  password | string | Hayır | Özel kayıt defteri için parola. |


<a id="IpAddress" />

### <a name="ipaddress-object"></a>IPAddress nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  ports | array | Yes | Kapsayıcı grubunda kullanıma sunulan bağlantı noktalarının listesi. - [bağlantı noktası nesnesi](#Port) |
|  type | yardımının | Yes | IP 'nin genel İnternet 'e veya özel VNET 'e açık olup olmadığını belirtir. -Public veya Private |
|  IP | string | Hayır | Genel internet 'e sunulan IP. |
|  dnsNameLabel | string | Hayır | IP için DNS ad etiketi. |


<a id="Volume" />

### <a name="volume-object"></a>Birim nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  ad | string | Yes | Birimin adı. |
|  azureFile | object | Hayır | Azure dosya birimi. - [AzureFileVolume nesnesi](#AzureFileVolume) |
|  emptyDir | object | Hayır | Boş dizin birimi. |
|  gizli dizi | object | Hayır | Gizli birim. |
|  gitRepo | object | Hayır | Git depo birimi. - [GitRepoVolume nesnesi](#GitRepoVolume) |


<a id="ContainerGroupDiagnostics" />

### <a name="containergroupdiagnostics-object"></a>ContainerGroupDiagnostics nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  Günlüğe kaydetme Analizi | object | Hayır | Kapsayıcı grubu Günlük Analizi bilgileri. - [Loganalytics nesnesi](#LogAnalytics) |


<a id="ContainerGroupNetworkProfile" />

### <a name="containergroupnetworkprofile-object"></a>ContainerGroupNetworkProfile nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  id | string | Yes | Bir ağ profili için tanımlayıcı. |


<a id="DnsConfiguration" />

### <a name="dnsconfiguration-object"></a>DnsConfiguration nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  Kullanır | array | Yes | Kapsayıcı grubu için DNS sunucuları. -dize |
|  searchDomains | string | Hayır | Kapsayıcı grubunda ana bilgisayar adı araması için DNS arama etki alanları. |
|  Seçenekler | string | Hayır | Kapsayıcı grubu için DNS seçenekleri. |


<a id="ContainerProperties" />

### <a name="containerproperties-object"></a>ContainerProperties nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  image | string | Yes | Kapsayıcı örneğini oluşturmak için kullanılan görüntünün adı. |
|  command | array | Hayır | Exec form 'da kapsayıcı örneği içinde yürütülecek komutlar. -dize |
|  ports | array | Hayır | Kapsayıcı örneğindeki açığa çıkarılan bağlantı noktaları. - [Containerport nesnesi](#ContainerPort) |
|  environmentVariables | array | Hayır | Kapsayıcı örneğinde ayarlanacak ortam değişkenleri. - [Environmentvariable nesnesi](#EnvironmentVariable) |
|  kaynakların | object | Yes | Kapsayıcı örneğinin kaynak gereksinimleri. - [Resourcerequirements nesnesi](#ResourceRequirements) |
|  Birimsiz bağlama | array | Hayır | Kapsayıcı örneği için kullanılabilir birim bağlama. - [Volumemount nesnesi](#VolumeMount) |
|  Livenessaraştırması | object | Hayır | Lizleştirme araştırması. - [Containeraraştırma nesnesi](#ContainerProbe) |
|  Readinessaraştırması | object | Hayır | Hazırlık araştırması. - [Containeraraştırma nesnesi](#ContainerProbe) |


<a id="Port" />

### <a name="port-object"></a>Bağlantı noktası nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  Protocol | yardımının | Hayır | Bağlantı noktasıyla ilişkili protokol. -TCP veya UDP |
|  port | integer | Yes | Bağlantı noktası numarası. |


<a id="AzureFileVolume" />

### <a name="azurefilevolume-object"></a>AzureFileVolume nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  madı | string | Yes | Birim olarak takılacak Azure dosya paylaşımının adı. |
|  Özelliğinin | boole | Hayır | Bir birim olarak bağlanmış Azure dosyasının salt okunurdur olup olmadığını belirten bayrak. |
|  storageAccountName | string | Yes | Azure dosya paylaşımının bulunduğu depolama hesabının adı. |
|  storageAccountKey | string | Hayır | Azure dosya paylaşımınıza erişmek için kullanılan depolama hesabı erişim anahtarı. |


<a id="GitRepoVolume" />

### <a name="gitrepovolume-object"></a>GitRepoVolume nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  dizinden | string | Hayır | Hedef dizin adı. '.. ' İle içermemelidir veya başlamamalıdır.  '. ' Sağlanırsa, birim dizini git deposu olur.  Aksi halde, belirtilmişse, belirtilen ada sahip alt dizinde Git deposunu içerir. |
|  Depo | string | Yes | Depo URL 'SI |
|  uncaya | string | Hayır | Belirtilen düzeltme için karmayı işleyin. |


<a id="LogAnalytics" />

### <a name="loganalytics-object"></a>LogAnalytics nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  Workspaceıd | string | Yes | Log Analytics için çalışma alanı kimliği |
|  workspaceKey | string | Yes | Log Analytics için çalışma alanı anahtarı |
|  Günlüğe kaydetme türü | yardımının | Hayır | Kullanılacak günlük türü. -Containerınsights veya Containerınstancelogs |
|  meta veriler | object | Hayır | Log Analytics için meta veriler. |


<a id="ContainerPort" />

### <a name="containerport-object"></a>ContainerPort nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  Protocol | yardımının | Hayır | Bağlantı noktasıyla ilişkili protokol. -TCP veya UDP |
|  port | integer | Yes | Kapsayıcı grubu içinde ortaya çıkarılan bağlantı noktası numarası. |


<a id="EnvironmentVariable" />

### <a name="environmentvariable-object"></a>EnvironmentVariable nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  ad | string | Yes | Ortam değişkeninin adı. |
|  değer | string | Hayır | Ortam değişkeninin değeri. |
|  secureValue | string | Hayır | Güvenli ortam değişkeninin değeri. |


<a id="ResourceRequirements" />

### <a name="resourcerequirements-object"></a>ResourceRequirements nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  istekleri | object | Yes | Bu kapsayıcı örneğinin kaynak istekleri. - [Resourcerequests nesnesi](#ResourceRequests) |
|  değerleri | object | Hayır | Bu kapsayıcı örneğinin kaynak sınırları. - [Resourcelimit nesnesi](#ResourceLimits) |


<a id="VolumeMount" />

### <a name="volumemount-object"></a>VolumeMount nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  ad | string | Yes | Birim bağlama adı. |
|  Bağlamayolu | string | Yes | Birimin bağlanması gereken kapsayıcı içindeki yol. İki nokta (:) içermemelidir. |
|  Özelliğinin | boole | Hayır | Birim bağlamamı salt okunurdur olduğunu belirten bayrak. |


<a id="ContainerProbe" />

### <a name="containerprobe-object"></a>Containeraraştırma nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  Exec | object | Hayır | Araştırma- [Containerexec nesnesine](#ContainerExec) yürütme komutu |
|  httpGet | object | Hayır | Http get ayarları araştırma- [Containerhttpget nesnesine](#ContainerHttpGet) |
|  ınitialdelayseconds | integer | Hayır | İlk gecikme süresi. |
|  periodSeconds | integer | Hayır | Süre saniyesi. |
|  failureThreshold | integer | Hayır | Hata eşiği. |
|  Başarılı eşik | integer | Hayır | Başarı eşiği. |
|  timeoutSeconds | integer | Hayır | Zaman aşımı süresi. |


<a id="ResourceRequests" />

### <a name="resourcerequests-object"></a>ResourceRequests nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | number | Yes | Bu kapsayıcı örneğinin GB cinsinden bellek isteği. |
|  cpu | number | Yes | Bu kapsayıcı örneğinin CPU isteği. |
|  gpu | object | Hayır | Bu kapsayıcı örneğinin GPU isteği. - [Gpuresource nesnesi](#GpuResource) |


<a id="ResourceLimits" />

### <a name="resourcelimits-object"></a>Resourcelimit nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | number | Hayır | Bu kapsayıcı örneğinin GB cinsinden bellek sınırı. |
|  cpu | number | Hayır | Bu kapsayıcı örneğinin CPU sınırı. |
|  gpu | object | Hayır | Bu kapsayıcı örneğinin GPU sınırı. - [Gpuresource nesnesi](#GpuResource) |


<a id="ContainerExec" />

### <a name="containerexec-object"></a>ContainerExec nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  command | array | Hayır | Kapsayıcı içinde yürütülecek komutlar. -dize |


<a id="ContainerHttpGet" />

### <a name="containerhttpget-object"></a>ContainerHttpGet nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  Yolun | string | Hayır | Araştırmanın yolu. |
|  port | integer | Yes | Araştırmanın bağlantı noktası numarası. |
|  Şemadaki | yardımının | Hayır | Düzen. -http veya https |


<a id="GpuResource" />

### <a name="gpuresource-object"></a>GpuResource nesnesi

|  Adı | Tür | Gereklidir | Değer |
|  ---- | ---- | ---- | ---- |
|  count | integer | Yes | GPU kaynağının sayısı. |
|  isteyin | yardımının | Yes | GPU kaynağının SKU 'SU. -K80, P100, V100 |


## <a name="next-steps"></a>Sonraki adımlar

[YAML dosyası kullanarak çok kapsayıcılı bir grup dağıtma](container-instances-multi-container-yaml.md)öğreticisine bakın.

Bir [Sanal ağda](container-instances-vnet.md) kapsayıcı grupları dağıtmak veya [harici bir birimi bağlamak](container-instances-volume-azure-files.md)için bir YAML dosyası kullanma örneklerine bakın.

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create

