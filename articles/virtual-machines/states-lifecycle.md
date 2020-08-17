---
title: Azure 'da bir sanal makinenin yaşam döngüsü ve durumları
description: Azure 'daki bir VM 'nin yaşam döngüsüne genel bakış, bir VM 'nin herhangi bir zamanda içinde bulunabileceği çeşitli durumların açıklamalarını de içerebilir.
services: virtual-machines
author: shandilvarun
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 08/09/2018
ms.author: vashan
ms.openlocfilehash: cd104a31fdba932481889db198ae3cc4998a4466
ms.sourcegitcommit: 64ad2c8effa70506591b88abaa8836d64621e166
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88261897"
---
# <a name="virtual-machines-lifecycle-and-states"></a>Sanal makinelerin yaşam döngüsü ve durumları

Azure sanal makineleri (VM 'Ler), *sağlama* ve *Güç* durumlarına kategorilere ayrılmamış farklı durumlara göre gider. Bu makalenin amacı, bu durumları açıklamanız ve müşterilerin örnek kullanım için faturalandırılırken özel olarak vurgulanmasını sağlamaktır. 

## <a name="power-states"></a>Güç durumları

Güç durumu, sanal makinenin bilinen son durumunu temsil eder.

![VM güç durumu diyagramı](./media/virtual-machines-common-states-lifecycle/vm-power-states.png)

<br>
Aşağıdaki tabloda her örnek durumunun açıklaması verilmiştir ve örnek kullanım için faturalandırıldığını gösterir.

:::row:::
   :::column span="":::

   **Durumunda**
   
   :::column-end:::
   :::column span="":::

   **Açıklama**

   :::column-end:::
   :::column span="":::

   **Örnek kullanım faturalandırılıyor**

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **Başlatılıyor**

   :::column-end:::
   :::column span="":::

   VM başlatılıyor.

   ```json
   "statuses": [
    {
    "code": "PowerState/starting",
    "level": "Info",
    "displayStatus": "VM starting"
    }
   ]
   ```
   :::column-end:::
   :::column span="":::

   **Faturalandırılmamış**

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **Çalışma**

   :::column-end:::
   :::column span="":::

   Bir VM için normal çalışma durumu

   ```json
   "statuses": [
    {
    "code": "PowerState/running",
    "level": "Info",
    "displayStatus": "VM running"
    }
  ]
  ```
   :::column-end:::
   :::column span="":::

   **IP**

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **Durduruluyor**

   :::column-end:::
   :::column span="":::

   Bu, geçici bir durumdur. İşlem tamamlandığında, **durduruldu**olarak gösterilir.

   ```json
   "statuses": [
    {
    "code": "PowerState/stopping",
    "level": "Info",
    "displayStatus": "VM stopping"
    }
   ]
  ```
   :::column-end:::
   :::column span="":::

   **IP**

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **Durduruldu**

   :::column-end:::
   :::column span="":::

   VM, Konuk işletim sistemi içinden veya PowerOff API 'Leri kullanılarak kapatıldı.

   Donanım hala VM 'ye ayrıldı ve konakta kalır.

   ```json
   "statuses": [
    {
    "code": "PowerState/stopped",
    "level": "Info",
    "displayStatus": "VM stopped"
    }
   ]
  ```
   :::column-end:::
   :::column span="":::

   **IP***

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **Serbest bırakılıyor**

   :::column-end:::
   :::column span="":::

   Geçiş durumu. İşlem tamamlandığında, VM **serbest bırakıldı**olarak gösterilir.

   ```json
   "statuses": [
    {
    "code": "PowerState/deallocating",
    "level": "Info",
    "displayStatus": "VM deallocating"
    }
   ]
  ```
   :::column-end:::
   :::column span="":::

   **Faturalandırılmamış***

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **Serbest bırakıldı**

   :::column-end:::
   :::column span="":::

   VM başarıyla durdurulmuş ve konaktan kaldırılmış.

   ```json
   "statuses": [
    {
    "code": "PowerState/deallocated",
    "level": "Info",
    "displayStatus": "VM deallocated"
    }
   ]
  ```
   :::column-end:::
   :::column span="":::

   **Faturalandırılmamış**

   :::column-end:::
:::row-end:::


Diskler ve ağ gibi bazı Azure kaynaklarının ücretlendirdiği &#42;. Örnekteki yazılım lisansları ücret ödemez.

## <a name="provisioning-states"></a>Sağlama durumları

Sağlama durumu, sanal makinede Kullanıcı tarafından başlatılan bir denetim düzlemi işleminin durumudur. Bu durumlar, bir VM 'nin güç durumundan ayrıdır.

- **Oluşturma** – VM oluşturma.

- **Update** – mevcut bir VM için modeli güncelleştirir. VM 'de Başlat/yeniden başlatma gibi bazı model olmayan değişiklikler de güncelleştirme altına düşmektedir.

- **Sil** – VM silme.

- **Serbest bırakma** : bir VM 'nin durdurulduğu ve konaktan kaldırıldığı yerdir. Bir VM 'nin serbest çıkarılması bir güncelleştirme olarak kabul edilir, bu nedenle güncelleştirme ile ilgili sağlama durumlarının görüntülenmesini sağlar.



Platform Kullanıcı tarafından başlatılan bir eylemi kabul ettikten sonra, geçiş işlemi durumları aşağıda verilmiştir:

:::row:::
   :::column span="":::

   **Durumunda**
   
   :::column-end:::
   :::column span="2":::

   **Açıklama**

   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **Oluşturma**

   :::column-end:::
   :::column span="2":::

  ```json
   "statuses": [
    {
    "code": "ProvisioningState/creating",
    "level": "Info",
    "displayStatus": "Creating"
    }
   [
   ```
   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **Bilen**

   :::column-end:::
   :::column span="2":::

   ```json
   "statuses": [
    {
    "code": "ProvisioningState/updating",
    "level": "Info",
    "displayStatus": "Updating"
    }
   [
   ```
   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **Siliniyor**

   :::column-end:::
   :::column span="2":::

   ```json
   "statuses": [
    {
    "code": "ProvisioningState/deleting",
    "level": "Info",
    "displayStatus": "Deleting"
    }
   [
   ```
   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **İşletim sistemi sağlama durumları**
   
   :::column-end:::
   :::column span="2":::

   **Açıklama**

   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::



   :::column-end:::
   :::column span="2":::

   Bir VM, özelleştirilmiş bir görüntüyle değil bir işletim sistemi görüntüsüyle oluşturulduysa, aşağıdaki alt durumlar gözlemlenebilir:

   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **Osprovisioningınprogress**

   :::column-end:::
   :::column span="2":::

   VM çalışıyor ve konuk işletim sistemi yüklemesi devam ediyor.
 
   ```json
   "statuses": [
    {
    "code": "ProvisioningState/creating/OSProvisioningInprogress",
    "level": "Info",
    "displayStatus": "OS Provisioning In progress"
    }
   [
   ```
   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **Osprovisioningtamamlanmıştır**

   :::column-end:::
   :::column span="2":::
   
   Kısa ömürlü durum. Herhangi bir uzantının yüklenmesi gerekmediği takdirde VM hızlı bir şekilde **başarılı** olarak geçiş yapar. Uzantıların yüklenmesi zaman alabilir.
   
   ```json
   "statuses": [
    {
    "code": "ProvisioningState/creating/OSProvisioningComplete",
    "level": "Info",
    "displayStatus": "OS Provisioning Complete"
    }
   [
   ```
   
   **Note**: işletim sistemi hatası varsa veya işletim sistemi zaman içinde yüklenemezse, Işletim sistemi sağlama **başarısız** durumuna geçirebilir. Müşteriler, altyapı üzerinde dağıtılan VM için faturalandırılır.

   :::column-end:::

:::row-end:::

İşlem tamamlandıktan sonra, VM aşağıdaki durumlardan birine geçiş yapılır:

- **Başarılı** – Kullanıcı tarafından başlatılan eylemler tamamlandı.

    ```
  "statuses": [ 
  {
     "code": "ProvisioningState/succeeded",
     "level": "Info",
     "displayStatus": "Provisioning succeeded",
     "time": "time"
  }
  ]
    ```

 

- **Failed** : başarısız bir işlemi temsil eder. Daha fazla bilgi ve olası çözümler almak için hata kodlarına bakın.

    ```
  "statuses": [
    {
      "code": "ProvisioningState/failed/InternalOperationError",
      "level": "Error",
      "displayStatus": "Provisioning failed",
      "message": "Operation abandoned due to internal error. Please try again later.",
      "time": "time"
    }
    ]
    ```



## <a name="vm-instance-view"></a>VM örnek görünümü

Örnek görünümü API 'SI, VM çalışma durumu bilgilerini sağlar. Daha fazla bilgi için bkz. [sanal makineler-örnek görüntüleme](https://docs.microsoft.com/rest/api/compute/virtualmachines/instanceview) API 'si belgeleri.

Azure Kaynak Gezgini, VM çalıştırma durumunu görüntülemek için basit bir kullanıcı arabirimi sağlar: [Kaynak Gezgini](https://resources.azure.com/).

Sağlama durumları VM özellikleri ve örnek görünümünde görülebilir. Güç durumları VM 'nin örnek görünümünde kullanılabilir.

Aboneliğinizdeki tüm VM 'lerin güç durumunu almak için, [sanal makineler-tüm API](https://docs.microsoft.com/rest/api/compute/virtualmachines/listall) 'Leri parametresi **statusonly** *true*olarak ayarlanmış şekilde kullanın.

## <a name="next-steps"></a>Sonraki adımlar

VM 'nizi izleme hakkında daha fazla bilgi edinmek için bkz. [Azure 'da sanal makineleri izleme](../azure-monitor/insights/monitor-vm-azure.md).