---
title: Kapsayıcı grubu için YAML başvurusu
description: Bir kapsayıcı grubunu yapılandırmak için Azure Container Instances tarafından desteklenen YAML dosyası başvurusu
ms.topic: article
ms.date: 08/12/2019
ms.openlocfilehash: be78c7d498187486a1502da17faa2b8faa5a0982
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84730535"
---
# <a name="yaml-reference-azure-container-instances"></a>YAML başvurusu: Azure Container Instances

Bu makalede bir [kapsayıcı grubunu](container-instances-container-groups.md)yapılandırmak için Azure Container Instances tarafından desteklenen YAML dosyasının sözdizimi ve özellikleri ele alınmaktadır. Azure CLı 'de [az Container Create][az-container-create] komutuna grup yapılandırmasını girmek IÇIN BIR YAML dosyası kullanın. 

YAML dosyası, tekrarlanabilir dağıtımlar için bir kapsayıcı grubu yapılandırmanın kolay bir yoludur. Bir kapsayıcı grubu oluşturmak veya güncelleştirmek için bir [Kaynak Yöneticisi şablonu](/azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups) veya Azure Container Instances SDK 'ları kullanmanın kısa bir alternatifidir.

> [!NOTE]
> Bu başvuru, Azure Container Instances REST API sürümüne yönelik YAML dosyaları için geçerlidir `2018-10-01` .

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

Aşağıdaki tablolarda, şemada ayarlamanız gereken değerler açıklanır.



### <a name="microsoftcontainerinstancecontainergroups-object"></a>Microsoft. Containerınstance/containerGroups nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  name | string | Evet | Kapsayıcı grubunun adı. |
|  apiVersion | enum | Evet | 2018-10-01 |
|  location | dize | No | Kaynak konumu. |
|  etiketler | nesne | Hayır | Kaynak etiketleri. |
|  identity | nesne | Hayır | Yapılandırılmışsa kapsayıcı grubunun kimliği. - [Containergroupıdentity nesnesi](#containergroupidentity-object) |
|  properties | nesne | Evet | [ContainerGroupProperties nesnesi](#containergroupproperties-object) |




### <a name="containergroupidentity-object"></a>Containergroupıdentity nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  tür | enum | Hayır | Kapsayıcı grubu için kullanılan kimlik türü. ' SystemAssigned, Useratandı ' türü hem örtük olarak oluşturulan bir kimliği hem de Kullanıcı tarafından atanan kimlikleri içerir. ' None ' türü, kapsayıcı grubundaki tüm kimlikleri kaldırır. -SystemAssigned, Useratandı, SystemAssigned, Useratandı, None |
|  Userassignedıdentities | nesne | Hayır | Kapsayıcı grubuyla ilişkili kullanıcı kimliklerinin listesi. Kullanıcı kimlik sözlüğü anahtar başvuruları Azure Resource Manager kaynak kimlikleri şu biçimde olacaktır: '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName} '. |




### <a name="containergroupproperties-object"></a>ContainerGroupProperties nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  containers | array | Evet | Kapsayıcı grubu içindeki kapsayıcılar. - [Kapsayıcı nesnesi](#container-object) |
|  imageRegistryCredentials | array | Hayır | Kapsayıcı grubunun oluşturulduğu görüntü kayıt defteri kimlik bilgileri. - [ImageRegistryCredential nesnesi](#imageregistrycredential-object) |
|  restartPolicy | enum | Hayır | Kapsayıcı grubundaki tüm kapsayıcılar için ilkeyi yeniden başlatın. - `Always`Her zaman yeniden Başlat-hata durumunda yeniden başlatma `OnFailure` `Never` . -Always, OnFailure, hiçbir zaman |
|  Belirlenemiyor | nesne | Hayır | Kapsayıcı grubunun IP adresi türü. - [IPAddress nesnesi](#ipaddress-object) |
|  osType | enum | Evet | Kapsayıcı grubundaki kapsayıcılar için gereken işletim sistemi türü. -Windows veya Linux |
|  volumes | array | Hayır | Bu kapsayıcı grubundaki kapsayıcılarla bağlankalebilecek birimlerin listesi. - [Birim nesnesi](#volume-object) |
|  tanılama | nesne | Hayır | Bir kapsayıcı grubu için tanılama bilgileri. - [ContainerGroupDiagnostics nesnesi](#containergroupdiagnostics-object) |
|  networkProfile | nesne | Hayır | Bir kapsayıcı grubu için ağ profili bilgileri. - [ContainerGroupNetworkProfile nesnesi](#containergroupnetworkprofile-object) |
|  dnsConfig | nesne | Hayır | Bir kapsayıcı grubu için DNS yapılandırma bilgileri. - [DnsConfiguration nesnesi](#dnsconfiguration-object) |




### <a name="container-object"></a>Kapsayıcı nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  name | string | Evet | Kapsayıcı örneğinin Kullanıcı tarafından sağlanmış adı. |
|  properties | nesne | Evet | Kapsayıcı örneğinin özellikleri. - [ContainerProperties nesnesi](#containerproperties-object) |




### <a name="imageregistrycredential-object"></a>ImageRegistryCredential nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  sunucu | string | Evet | "Http" ve "https" gibi bir protokol olmadan Docker görüntü kayıt defteri sunucusu. |
|  kullanıcı adı | string | Evet | Özel kayıt defteri için Kullanıcı adı. |
|  password | dize | No | Özel kayıt defteri için parola. |




### <a name="ipaddress-object"></a>IPAddress nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  ports | array | Evet | Kapsayıcı grubunda kullanıma sunulan bağlantı noktalarının listesi. - [Bağlantı noktası nesnesi](#port-object) |
|  tür | enum | Evet | IP 'nin genel İnternet 'e veya özel VNET 'e açık olup olmadığını belirtir. -Public veya Private |
|  IP | dize | No | Genel internet 'e sunulan IP. |
|  dnsNameLabel | dize | No | IP için DNS ad etiketi. |




### <a name="volume-object"></a>Birim nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  name | string | Evet | Birimin adı. |
|  azureFile | nesne | Hayır | Azure dosya birimi. - [AzureFileVolume nesnesi](#azurefilevolume-object) |
|  emptyDir | nesne | Hayır | Boş dizin birimi. |
|  gizli dizi | nesne | Hayır | Gizli birim. |
|  gitRepo | nesne | Hayır | Git depo birimi. - [GitRepoVolume nesnesi](#gitrepovolume-object) |




### <a name="containergroupdiagnostics-object"></a>ContainerGroupDiagnostics nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  Günlüğe kaydetme Analizi | nesne | Hayır | Kapsayıcı grubu Günlük Analizi bilgileri. - [LogAnalytics nesnesi](#loganalytics-object) |




### <a name="containergroupnetworkprofile-object"></a>ContainerGroupNetworkProfile nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  kimlik | string | Evet | Bir ağ profili için tanımlayıcı. |




### <a name="dnsconfiguration-object"></a>DnsConfiguration nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  Kullanır | array | Evet | Kapsayıcı grubu için DNS sunucuları. -dize |
|  searchDomains | dize | No | Kapsayıcı grubunda ana bilgisayar adı araması için DNS arama etki alanları. |
|  seçenekler | dize | No | Kapsayıcı grubu için DNS seçenekleri. |




### <a name="containerproperties-object"></a>ContainerProperties nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  image | string | Evet | Kapsayıcı örneğini oluşturmak için kullanılan görüntünün adı. |
|  command | array | Hayır | Exec form 'da kapsayıcı örneği içinde yürütülecek komutlar. -dize |
|  ports | array | Hayır | Kapsayıcı örneğindeki açığa çıkarılan bağlantı noktaları. - [ContainerPort nesnesi](#containerport-object) |
|  environmentVariables | array | Hayır | Kapsayıcı örneğinde ayarlanacak ortam değişkenleri. - [EnvironmentVariable nesnesi](#environmentvariable-object) |
|  kaynaklar | nesne | Evet | Kapsayıcı örneğinin kaynak gereksinimleri. - [ResourceRequirements nesnesi](#resourcerequirements-object) |
|  Birimsiz bağlama | array | Hayır | Kapsayıcı örneği için kullanılabilir birim bağlama. - [VolumeMount nesnesi](#volumemount-object) |
|  Livenessaraştırması | nesne | Hayır | Lizleştirme araştırması. - [Containeraraştırma nesnesi](#containerprobe-object) |
|  Readinessaraştırması | nesne | Hayır | Hazırlık araştırması. - [Containeraraştırma nesnesi](#containerprobe-object) |




### <a name="port-object"></a>Bağlantı noktası nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  protokol | enum | Hayır | Bağlantı noktasıyla ilişkili protokol. -TCP veya UDP |
|  port | integer | Evet | Bağlantı noktası numarası. |




### <a name="azurefilevolume-object"></a>AzureFileVolume nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  madı | string | Evet | Birim olarak takılacak Azure dosya paylaşımının adı. |
|  Özelliğinin | boole | Hayır | Bir birim olarak bağlanmış Azure dosyasının salt okunurdur olup olmadığını belirten bayrak. |
|  storageAccountName | string | Evet | Azure dosya paylaşımının bulunduğu depolama hesabının adı. |
|  storageAccountKey | dize | No | Azure dosya paylaşımınıza erişmek için kullanılan depolama hesabı erişim anahtarı. |




### <a name="gitrepovolume-object"></a>GitRepoVolume nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  dizin | dize | No | Hedef dizin adı. '.. ' İle içermemelidir veya başlamamalıdır.  '. ' Sağlanırsa, birim dizini git deposu olur.  Aksi halde, belirtilmişse, belirtilen ada sahip alt dizinde Git deposunu içerir. |
|  depo | string | Evet | Depo URL 'SI |
|  revision | dize | No | Belirtilen düzeltme için karmayı işleyin. |




### <a name="loganalytics-object"></a>LogAnalytics nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  workspaceId | string | Evet | Log Analytics için çalışma alanı kimliği |
|  workspaceKey | string | Evet | Log Analytics için çalışma alanı anahtarı |
|  Günlüğe kaydetme türü | enum | Hayır | Kullanılacak günlük türü. -Containerınsights veya Containerınstancelogs |
|  meta veriler | nesne | Hayır | Log Analytics için meta veriler. |




### <a name="containerport-object"></a>ContainerPort nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  protokol | enum | Hayır | Bağlantı noktasıyla ilişkili protokol. -TCP veya UDP |
|  port | integer | Evet | Kapsayıcı grubu içinde ortaya çıkarılan bağlantı noktası numarası. |




### <a name="environmentvariable-object"></a>EnvironmentVariable nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  name | string | Evet | Ortam değişkeninin adı. |
|  değer | dize | No | Ortam değişkeninin değeri. |
|  secureValue | dize | No | Güvenli ortam değişkeninin değeri. |




### <a name="resourcerequirements-object"></a>ResourceRequirements nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  istekleri | nesne | Evet | Bu kapsayıcı örneğinin kaynak istekleri. - [ResourceRequests nesnesi](#resourcerequests-object) |
|  değerleri | nesne | Hayır | Bu kapsayıcı örneğinin kaynak sınırları. - [Resourcelimit nesnesi](#resourcelimits-object) |




### <a name="volumemount-object"></a>VolumeMount nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  name | string | Evet | Birim bağlama adı. |
|  Bağlamayolu | string | Evet | Birimin bağlanması gereken kapsayıcı içindeki yol. İki nokta (:) içermemelidir. |
|  Özelliğinin | boole | Hayır | Birim bağlamamı salt okunurdur olduğunu belirten bayrak. |




### <a name="containerprobe-object"></a>Containeraraştırma nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  Exec | nesne | Hayır | Araştırma- [Containerexec nesnesine](#containerexec-object) yürütme komutu |
|  httpGet | nesne | Hayır | Http get ayarları araştırma- [Containerhttpget nesnesine](#containerhttpget-object) |
|  ınitialdelayseconds | integer | Hayır | İlk gecikme süresi. |
|  periodSeconds | integer | Hayır | Süre saniyesi. |
|  failureThreshold | integer | Hayır | Hata eşiği. |
|  Başarılı eşik | integer | Hayır | Başarı eşiği. |
|  timeoutSeconds | integer | Hayır | Zaman aşımı süresi. |




### <a name="resourcerequests-object"></a>ResourceRequests nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | sayı | Evet | Bu kapsayıcı örneğinin GB cinsinden bellek isteği. |
|  'suna | sayı | Evet | Bu kapsayıcı örneğinin CPU isteği. |
|  GPU | nesne | Hayır | Bu kapsayıcı örneğinin GPU isteği. - [GpuResource nesnesi](#gpuresource-object) |




### <a name="resourcelimits-object"></a>Resourcelimit nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | sayı | Hayır | Bu kapsayıcı örneğinin GB cinsinden bellek sınırı. |
|  'suna | sayı | Hayır | Bu kapsayıcı örneğinin CPU sınırı. |
|  GPU | nesne | Hayır | Bu kapsayıcı örneğinin GPU sınırı. - [GpuResource nesnesi](#gpuresource-object) |




### <a name="containerexec-object"></a>ContainerExec nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  command | array | Hayır | Kapsayıcı içinde yürütülecek komutlar. -dize |




### <a name="containerhttpget-object"></a>ContainerHttpGet nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  yol | dize | No | Araştırmanın yolu. |
|  port | integer | Evet | Araştırmanın bağlantı noktası numarası. |
|  düzen | enum | Hayır | Düzen. -http veya https |




### <a name="gpuresource-object"></a>GpuResource nesnesi

|  Name | Tür | Gerekli | Değer |
|  ---- | ---- | ---- | ---- |
|  count | integer | Evet | GPU kaynağının sayısı. |
|  isteyin | enum | Evet | GPU kaynağının SKU 'SU. -K80, P100, V100 |


## <a name="next-steps"></a>Sonraki adımlar

[YAML dosyası kullanarak çok kapsayıcılı bir grup dağıtma](container-instances-multi-container-yaml.md)öğreticisine bakın.

Bir [Sanal ağda](container-instances-vnet.md) kapsayıcı grupları dağıtmak veya [harici bir birimi bağlamak](container-instances-volume-azure-files.md)için bir YAML dosyası kullanma örneklerine bakın.

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create

