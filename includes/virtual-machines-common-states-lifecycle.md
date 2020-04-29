---
title: include dosyası
description: include dosyası
services: virtual-machines
author: shandilvarun
ms.service: virtual-machines
ms.topic: include
ms.date: 08/09/2018
ms.author: vashan, cynthn, rajsqr
ms.custom: include file
ms.openlocfilehash: 57f557a812ec5e4eea75b76ca1394ca360a85d30
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188310"
---
Azure sanal makineleri (VM 'Ler), *sağlama* ve *Güç* durumlarına kategorilere ayrılmamış farklı durumlara göre gider. Bu makalenin amacı, bu durumları açıklamanız ve müşterilerin örnek kullanım için faturalandırılırken özel olarak vurgulanmasını sağlamaktır. 

## <a name="power-states"></a>Güç durumları

Güç durumu, sanal makinenin bilinen son durumunu temsil eder.

![VM güç durumu diyagramı](./media/virtual-machines-common-states-lifecycle/vm-power-states.png)

<br>
Aşağıdaki tabloda her örnek durumunun açıklaması verilmiştir ve örnek kullanım için faturalandırıldığını gösterir.

<table>
<tr>
<th>
Durum
</th>
<th>
Açıklama
</th>
<th>
Örnek kullanım faturalandırması
</th>
</tr>
<tr>
<td>
<p><b>Başlatılıyor</b></p>
</td>
<td>
<p>VM başlatılıyor.</p>
<code>"statuses": [<br>
   {<br>
      "code": "PowerState/starting",<br>
       "level": "Info",<br>
        "displayStatus": "VM starting"<br>
    }<br>
    ]</code><br>
</td>
<td>
<p><b>Faturalandırılmamış</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Çalışıyor</b></p>
</td>
<td>
<p>Bir VM için normal çalışma durumu</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/running",<br>
 "level": "Info",<br>
 "displayStatus": "VM running"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>IP</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Durduruluyor</b></p>
</td>
<td>
<p>Bu, geçici bir durumdur. İşlem tamamlandığında, **durduruldu**olarak gösterilir.</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/stopping",<br>
 "level": "Info",<br>
 "displayStatus": "VM stopping"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>IP</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Durduruldu</b></p>
</td>
<td>
<p>VM, Konuk işletim sistemi içinden veya PowerOff API 'Leri kullanılarak kapatıldı.</p>
<p>Donanım hala VM 'ye ayrıldı ve konakta kalır. </p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/stopped",<br>
 "level": "Info",<br>
 "displayStatus": "VM stopped"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Faturalandırılan&#42;</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Serbest bırakılıyor</b></p>
</td>
<td>
<p>Geçiş durumu. İşlem tamamlandığında, VM **serbest bırakıldı**olarak gösterilir.</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/deallocating",<br>
 "level": "Info",<br>
 "displayStatus": "VM deallocating"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Faturalandırılmamış&#42;</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Serbest bırakıldı</b></p>
</td>
<td>
<p>VM başarıyla durdurulmuş ve konaktan kaldırılmış. </p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/deallocated",<br>
 "level": "Info",<br>
 "displayStatus": "VM deallocated"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Faturalandırılmamış</b></p>
</td>
</tr>
</tbody>
</table>


Diskler ve ağ gibi bazı Azure kaynaklarının ücretlendirdiği &#42;. Örnekteki yazılım lisansları ücret ödemez.

## <a name="provisioning-states"></a>Sağlama durumları

Sağlama durumu, sanal makinede Kullanıcı tarafından başlatılan bir denetim düzlemi işleminin durumudur. Bu durumlar, bir VM 'nin güç durumundan ayrıdır.

- **Oluşturma** – VM oluşturma.

- **Update** – mevcut bir VM için modeli güncelleştirir. VM 'de Başlat/yeniden başlatma gibi bazı model olmayan değişiklikler de güncelleştirme altına düşmektedir.

- **Sil** – VM silme.

- **Serbest bırakma** : bir VM 'nin durdurulduğu ve konaktan kaldırıldığı yerdir. Bir VM 'nin serbest çıkarılması bir güncelleştirme olarak kabul edilir, bu nedenle güncelleştirme ile ilgili sağlama durumlarının görüntülenmesini sağlar.



Platform Kullanıcı tarafından başlatılan bir eylemi kabul ettikten sonra, geçiş işlemi durumları aşağıda verilmiştir:

<br>

<table>
<tbody>
<tr>
<td width="162">
<p><b>Durumlar</b></p>
</td>
<td width="366">
<p>Açıklama</p>
</td>
</tr>
<tr>
<td width="162">
<p><b>Oluşturma</b></p>
</td>
<td width="366">
<code>"statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating",<br>
 "level": "Info",<br>
 "displayStatus": "Creating"<br>
 }</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>Bilen</b></p>
</td>
<td width="366">
<code>"statuses": [<br>
 {<br>
 "code": "ProvisioningState/updating",<br>
 "level": "Info",<br>
 "displayStatus": "Updating"<br>
 }<br>
 ]</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>Silinmesinden</b></p>
</td>
<td width="366">
<code>"statuses": [<br>
 {<br>
 "code": "ProvisioningState/deleting",<br>
 "level": "Info",<br>
 "displayStatus": "Deleting"<br>
 }<br>
 ]</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>İşletim sistemi sağlama durumları</b></p>
</td>
<td width="366">
<p>Bir VM, özelleştirilmiş bir görüntüyle değil bir işletim sistemi görüntüsüyle oluşturulduysa, aşağıdaki alt durumlar gözlemlenebilir:</p>
<p>1. <b>Osprovisioningınprogress</b> &ndash; sanal makinesi çalışıyor ve konuk işletim sistemi yüklemesi devam ediyor. <p /> 
<code> "statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating/OSProvisioningInprogress",<br>
 "level": "Info",<br>
 "displayStatus": "OS Provisioning In progress"<br>
 }<br>
]</code><br>
<p>2. <b>Osprovisioningtam</b> &ndash; kısa ömürlü durum. Herhangi bir uzantının yüklenmesi gerekmediği takdirde VM hızlı bir şekilde **başarılı** olarak geçiş yapar. Uzantıların yüklenmesi zaman alabilir. <br />
<code> "statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating/OSProvisioningComplete",<br>
 "level": "Info",<br>
 "displayStatus": "OS Provisioning Complete"<br>
 }<br>
]</code><br>
<p><b>Note</b>: işletim sistemi hatası varsa veya işletim sistemi zaman içinde yüklenemezse, Işletim sistemi sağlama **başarısız** durumuna geçirebilir. Müşteriler, altyapı üzerinde dağıtılan VM için faturalandırılır.</p>
</td>
</tr>
</table>


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

