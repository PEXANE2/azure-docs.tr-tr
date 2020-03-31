---
title: Konteyner grubu için YAML başvurusu
description: Bir kapsayıcı grubunu yapılandırmak için Azure Kapsayıcı Örnekleri tarafından desteklenen YAML dosyası için başvuru
ms.topic: article
ms.date: 08/12/2019
ms.openlocfilehash: 8497330a327201c4c64e9f7ae57e6fc4225b52de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74896563"
---
# <a name="yaml-reference-azure-container-instances"></a>YAML başvurusu: Azure Kapsayıcı Örnekleri

Bu makalede, bir [kapsayıcı grubu](container-instances-container-groups.md)yapılandırmak için Azure Kapsayıcı Örnekleri tarafından desteklenen YAML dosyasının sözdizimi ve özellikleri kapsar. Grup yapılandırmasını Azure CLI'deki [az kapsayıcı oluşturma][az-container-create] komutuna sokmak için bir YAML dosyası kullanın. 

YAML dosyası, yeniden dönüştürülebilir dağıtımlar için bir kapsayıcı grubunu yapılandırmanın kullanışlı bir yoludur. Bir kapsayıcı grubu oluşturmak veya güncelleştirmek için [Kaynak Yöneticisi şablonu](/azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups) veya Azure Kapsayıcı Örnekleri SDK'larını kullanmaya kısa bir alternatiftir.

> [!NOTE]
> Bu başvuru, Azure Kapsayıcı Örnekleri REST API sürümü `2018-10-01`için YAML dosyaları için geçerlidir.

## <a name="schema"></a>Şema 

YAML dosyasının şeması, önemli özellikleri vurgulamak için yapılan açıklamaları da içeren aşağıdaki gibidir. Bu şemadaki özelliklerin açıklaması için [Özellik değerleri](#property-values) bölümüne bakın.

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
      ports: # External-facing ports exposed on the instance, must also be set in group ipAddress property 
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

Aşağıdaki tablolar şemada ayarlamanız gereken değerleri açıklar.

<a id="Microsoft.ContainerInstance/containerGroups" />

### <a name="microsoftcontainerinstancecontainergroups-object"></a>Microsoft.ContainerInstance/containerGroups nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  ad | string | Evet | Kapsayıcı grubunun adı. |
|  apiVersion | enum | Evet | 2018-10-01 |
|  location | string | Hayır | Kaynak konumu. |
|  etiketler | object | Hayır | Kaynak etiketleri. |
|  identity | object | Hayır | Yapılandırılırsa kapsayıcı grubunun kimliği. - [ContainerGroupIdentity nesnesi](#ContainerGroupIdentity) |
|  properties | object | Evet | [ContainerGroupProperties nesnesi](#ContainerGroupProperties) |


<a id="ContainerGroupIdentity" />

### <a name="containergroupidentity-object"></a>ContainerGroupIdentity nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  type | enum | Hayır | Kapsayıcı grubu için kullanılan kimlik türü. 'SystemAssigned, UserAssigned' türü, hem örtülü olarak oluşturulmuş bir kimlik hem de kullanıcı tarafından atanan kimlikler kümesini içerir. 'Yok' türü, kapsayıcı grubundaki kimlikleri kaldırır. - Sistem atanmış, userassigned, systemassigned, userassigned, hiçbiri |
|  kullanıcıAssignedIdentities | object | Hayır | Kapsayıcı grubuyla ilişkili kullanıcı kimlikleri listesi. Kullanıcı kimliği sözlüğü anahtar başvuruları, formdaki Azure Kaynak Yöneticisi kaynak kimlikleri olacaktır: '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}'. |


<a id="ContainerGroupProperties" />

### <a name="containergroupproperties-object"></a>ContainerGroupProperties nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  containers | array | Evet | Konteyner grubu içindeki konteynerler. - [Kapsayıcı nesnesi](#Container) |
|  imageRegistryCredentials | array | Hayır | Kapsayıcı grubunun oluşturulduğu görüntü kayıt defteri kimlik bilgileri. - [ImageRegistryCredential nesne](#ImageRegistryCredential) |
|  yeniden başlatmaPolitikası | enum | Hayır | Kapsayıcı grubundaki tüm kapsayıcılar için ilkeyi yeniden başlatın. - `Always`Her zaman `OnFailure` yeniden başlat- `Never` Hataüzerinde yeniden başlatın- Asla yeniden başlatın. - Her zaman, onfailure, asla |
|  ıpaddress | object | Hayır | Kapsayıcı grubunun IP adresi türü. - [IpAddress nesnesi](#IpAddress) |
|  osType | enum | Evet | Kapsayıcı grubundaki kapsayıcıların gerektirdiği işletim sistemi türü. - Windows veya Linux |
|  volumes | array | Hayır | Bu kapsayıcı grubundaki kapsayıcılar tarafından monte edilebilen birimlerin listesi. - [Birim nesnesi](#Volume) |
|  tanılama | object | Hayır | Bir kapsayıcı grubunun tanılama bilgileri. - [ContainerGroupDiagnostics nesnesi](#ContainerGroupDiagnostics) |
|  ağProfil | object | Hayır | Bir kapsayıcı grubunun ağ profili bilgileri. - [ContainerGroupNetworkProfile nesnesi](#ContainerGroupNetworkProfile) |
|  Dnsconfig | object | Hayır | Bir kapsayıcı grubu için DNS config bilgileri. - [DnsConfiguration nesnesi](#DnsConfiguration) |


<a id="Container" />

### <a name="container-object"></a>Kapsayıcı nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  ad | string | Evet | Kapsayıcı örneğinin kullanıcı tarafından sağlanan adı. |
|  properties | object | Evet | Kapsayıcı örneğinin özellikleri. - [ContainerProperties nesnesi](#ContainerProperties) |


<a id="ImageRegistryCredential" />

### <a name="imageregistrycredential-object"></a>ImageRegistryCredential nesne

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  sunucu | string | Evet | Docker görüntü kayıt sunucusu "http" ve "https" gibi bir protokol olmadan. |
|  kullanıcı adı | string | Evet | Özel kayıt defterinin kullanıcı adı. |
|  password | string | Hayır | Özel kayıt defterinin şifresi. |


<a id="IpAddress" />

### <a name="ipaddress-object"></a>IpAddress nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  ports | array | Evet | Kapsayıcı grubunda açığa çıkan bağlantı noktalarının listesi. - [Bağlantı noktası nesnesi](#Port) |
|  type | enum | Evet | IP'nin genel internete veya özel VNET'e maruz olup olmadığını belirtir. - Genel veya Özel |
|  ıp | string | Hayır | IP kamu internet maruz. |
|  dnsNameLabel | string | Hayır | IP için Dns ad etiketi. |


<a id="Volume" />

### <a name="volume-object"></a>Birim nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  ad | string | Evet | Birimin adı. |
|  azureDosya | object | Hayır | Azure Dosya birimi. - [AzureFileVolume nesnesi](#AzureFileVolume) |
|  emptyDir | object | Hayır | Boş dizin hacmi. |
|  gizli dizi | object | Hayır | Gizli ses. |
|  gitRepo | object | Hayır | Git repo hacmi. - [GitRepoVolume nesnesi](#GitRepoVolume) |


<a id="ContainerGroupDiagnostics" />

### <a name="containergroupdiagnostics-object"></a>ContainerGroupDiagnostics nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  logAnalytics | object | Hayır | Konteyner grubu günlük analiz bilgileri. - [LogAnalytics nesnesi](#LogAnalytics) |


<a id="ContainerGroupNetworkProfile" />

### <a name="containergroupnetworkprofile-object"></a>ContainerGroupNetworkProfile nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  id | string | Evet | Ağ profili tanımlayıcısı. |


<a id="DnsConfiguration" />

### <a name="dnsconfiguration-object"></a>DnsConfiguration nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  Nameservers | array | Evet | Kapsayıcı grubunun DNS sunucuları. - dize |
|  aramaEtki Alanları | string | Hayır | Kapsayıcı grubunda ana bilgisayar adı araması için DNS arama etki alanları. |
|  seçenekler | string | Hayır | Kapsayıcı grubu için DNS seçenekleri. |


<a id="ContainerProperties" />

### <a name="containerproperties-object"></a>ContainerProperties nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  image | string | Evet | Kapsayıcı örneğini oluşturmak için kullanılan görüntünün adı. |
|  command | array | Hayır | Kapsayıcı örneğinde exec formunda yürütülecek komutlar. - dize |
|  ports | array | Hayır | Kapsayıcı örneğindeki açıkbağlantı noktaları. - [ContainerPort nesnesi](#ContainerPort) |
|  çevreDeğişkenler | array | Hayır | Kapsayıcı örneğinde ayarlanan ortam değişkenleri. - [ÇevreDeğişken nesne](#EnvironmentVariable) |
|  kaynaklar | object | Evet | Kapsayıcı örneğinin kaynak gereksinimleri. - [Kaynak Gereksinimleri nesnesi](#ResourceRequirements) |
|  hacimMontajlar | array | Hayır | Birim, kapsayıcı örneğinde kullanılabilir bağlar. - [VolumeMount nesnesi](#VolumeMount) |
|  canlılıkProbe | object | Hayır | Canlılık sondası. - [ContainerProbe nesnesi](#ContainerProbe) |
|  hazırlıkProbe | object | Hayır | Hazırlık sondası. - [ContainerProbe nesnesi](#ContainerProbe) |


<a id="Port" />

### <a name="port-object"></a>Bağlantı noktası nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  protokol | enum | Hayır | Bağlantı noktasıyla ilişkili protokol. - TCP veya UDP |
|  port | integer | Evet | Bağlantı noktası numarası. |


<a id="AzureFileVolume" />

### <a name="azurefilevolume-object"></a>AzureFileVolume nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  Paylaşımadı | string | Evet | Ses düzeyi olarak monte edilecek Azure Dosyası paylaşımının adı. |
|  Readonly | boole | Hayır | Azure Dosyası'nın ses düzeyi olarak paylaşılıp paylaşılmadığını belirten bayrak salt okunur. |
|  storageAccountName | string | Evet | Azure Dosyası paylaşımını içeren depolama hesabının adı. |
|  depolamaAccountKey | string | Hayır | Azure Dosyası paylaşımına erişmek için kullanılan depolama hesabı erişim anahtarı. |


<a id="GitRepoVolume" />

### <a name="gitrepovolume-object"></a>GitRepoVolume nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  dizin | string | Hayır | Hedef dizin adı. '...' ile başlamamalı veya içermemelidir.  '.' sağlanırsa, ses dizini git deposu olacaktır.  Aksi takdirde, belirtilirse, birim verilen adı içeren alt dizinde git deposu nu içerecektir. |
|  depo | string | Evet | Depo URL'si |
|  revision | string | Hayır | Belirtilen düzeltme için karma işleyin. |


<a id="LogAnalytics" />

### <a name="loganalytics-object"></a>LogAnalytics nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  workspaceId | string | Evet | Günlük analitiği için çalışma alanı kimliği |
|  çalışma alanıAnahtar | string | Evet | Günlük analitiği için çalışma alanı anahtarı |
|  Logtype | enum | Hayır | Kullanılacak günlük türü. - ContainerInsights veya ContainerInstanceLogs |
|  meta veriler | object | Hayır | Günlük analitiği için meta veriler. |


<a id="ContainerPort" />

### <a name="containerport-object"></a>ContainerPort nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  protokol | enum | Hayır | Bağlantı noktasıyla ilişkili protokol. - TCP veya UDP |
|  port | integer | Evet | Konteyner grubu içinde açığa çıkan bağlantı noktası numarası. |


<a id="EnvironmentVariable" />

### <a name="environmentvariable-object"></a>ÇevreDeğişken nesne

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  ad | string | Evet | Çevre değişkeninin adı. |
|  value | string | Hayır | Ortam değişkeninin değeri. |
|  secureValue | string | Hayır | Güvenli ortam değişkeninin değeri. |


<a id="ResourceRequirements" />

### <a name="resourcerequirements-object"></a>Kaynak Gereksinimleri nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  Istek | object | Evet | Bu kapsayıcı örneğinin kaynak istekleri. - [ResourceRequests nesnesi](#ResourceRequests) |
|  Sınır -ları | object | Hayır | Bu kapsayıcı örneğinin kaynak sınırları. - [ResourceLimits nesnesi](#ResourceLimits) |


<a id="VolumeMount" />

### <a name="volumemount-object"></a>VolumeMount nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  ad | string | Evet | Ses montaj adı. |
|  mountPath | string | Evet | Birimin monte edilmesi gereken kapsayıcı içindeki yol. Kolon (:) içermemelidir. |
|  Readonly | boole | Hayır | Ses dağının salt okunur olup olmadığını belirten bayrak. |


<a id="ContainerProbe" />

### <a name="containerprobe-object"></a>ContainerProbe nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  Exec | object | Hayır | Sonda için yürütme komutu - [ContainerExec nesne](#ContainerExec) |
|  httpGet | object | Hayır | Http Sonda ayarları alın - [ContainerHttpGet nesne](#ContainerHttpGet) |
|  ilkDelaySeconds | integer | Hayır | İlk gecikme saniyesi. |
|  periodSeconds | integer | Hayır | Periyot saniyeleri. |
|  failureThreshold | integer | Hayır | Başarısızlık eşiği. |
|  başarıEşik | integer | Hayır | Başarı eşiği. |
|  zaman outSeconds | integer | Hayır | Zaman zaman ları saniyeler. |


<a id="ResourceRequests" />

### <a name="resourcerequests-object"></a>ResourceRequests nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | number | Evet | Bu kapsayıcı örneğinin GB bellek isteği. |
|  Cpu | number | Evet | Bu kapsayıcı örneğinin CPU isteği. |
|  Gpu | object | Hayır | Bu kapsayıcı örneğinin GPU isteği. - [GpuResource nesnesi](#GpuResource) |


<a id="ResourceLimits" />

### <a name="resourcelimits-object"></a>ResourceLimits nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | number | Hayır | Bu kapsayıcı örneğinIN GB bellek sınırı. |
|  Cpu | number | Hayır | Bu kapsayıcı örneğinin CPU sınırı. |
|  Gpu | object | Hayır | Bu kapsayıcı örneğinin GPU sınırı. - [GpuResource nesnesi](#GpuResource) |


<a id="ContainerExec" />

### <a name="containerexec-object"></a>ContainerExec nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  command | array | Hayır | Kapsayıcı içinde yürütülecek komutlar. - dize |


<a id="ContainerHttpGet" />

### <a name="containerhttpget-object"></a>KonteynerHttpNesne al

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  yol | string | Hayır | Sondaya giden yol. |
|  port | integer | Evet | Sonda için bağlantı noktası numarası. |
|  düzen | enum | Hayır | Plan. - http veya https |


<a id="GpuResource" />

### <a name="gpuresource-object"></a>GpuResource nesnesi

|  Adı | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  count | integer | Evet | GPU kaynağının sayısı. |
|  Sku | enum | Evet | GPU kaynağının SKU'su. - K80, P100, V100 |


## <a name="next-steps"></a>Sonraki adımlar

Öğreticiye bakın [YAML dosyasını kullanarak çok kapsayıcılı bir grubu dağıtın.](container-instances-multi-container-yaml.md)

Kapsayıcı grupları [sanal ağda](container-instances-vnet.md) dağıtmak için veya [dış birim monte](container-instances-volume-azure-files.md)etmek için BIR YAML dosyası kullanma örneklerine bakın.

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create

