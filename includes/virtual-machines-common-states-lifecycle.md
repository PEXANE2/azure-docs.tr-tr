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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188310"
---
Azure Sanal Makineler (VM'ler), *sağlama* ve *güç* durumlarına kategorize edilebilen farklı durumları geçer. Bu makalenin amacı, bu durumları açıklamak ve özellikle müşterilerin örneğin kullanımı için faturalandırıldığında vurgulamaktır. 

## <a name="power-states"></a>Güç durumları

Güç durumu VM'nin bilinen son durumunu temsil eder.

![VM güç durumu diyagramı](./media/virtual-machines-common-states-lifecycle/vm-power-states.png)

<br>
Aşağıdaki tablo her örnek durumunun açıklamasını sağlar ve örneğin kullanımı için faturalandırılıp faturalandırılmadığını gösterir.

<table>
<tr>
<th>
Durum
</th>
<th>
Açıklama
</th>
<th>
Örnek kullanım faturalandırma
</th>
</tr>
<tr>
<td>
<p><b>Başlatılıyor</b></p>
</td>
<td>
<p>VM başlıyor.</p>
<code>"statuses": [<br>
   {<br>
      "code": "PowerState/starting",<br>
       "level": "Info",<br>
        "displayStatus": "VM starting"<br>
    }<br>
    ]</code><br>
</td>
<td>
<p><b>Faturalandırılmama</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Çalışıyor</b></p>
</td>
<td>
<p>VM için normal çalışma durumu</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/running",<br>
 "level": "Info",<br>
 "displayStatus": "VM running"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Fatura</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Durduruluyor</b></p>
</td>
<td>
<p>Bu bir geçiş durumudur. Tamamlandığında, **Durduruldu**olarak gösterecektir.</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/stopping",<br>
 "level": "Info",<br>
 "displayStatus": "VM stopping"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Fatura</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Durduruldu</b></p>
</td>
<td>
<p>VM konuk işletim sistemi içinden veya PowerOff API'leri kullanılarak kapatıldı.</p>
<p>Donanım hala VM'ye tahsis edilir ve ana bilgisayarda kalır. </p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/stopped",<br>
 "level": "Info",<br>
 "displayStatus": "VM stopped"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Faturalı&#42;</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Serbest bırakılıyor</b></p>
</td>
<td>
<p>Geçiş durumu. Tamamlandığında, VM **Deallocated**olarak gösterecektir.</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/deallocating",<br>
 "level": "Info",<br>
 "displayStatus": "VM deallocating"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Faturalı değil&#42;</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Serbest bırakıldı</b></p>
</td>
<td>
<p>VM başarıyla durduruldu ve ana bilgisayardan kaldırıldı. </p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/deallocated",<br>
 "level": "Info",<br>
 "displayStatus": "VM deallocated"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Faturalandırılmama</b></p>
</td>
</tr>
</tbody>
</table>


&#42;Diskler ve Ağ gibi bazı Azure kaynakları ücrete tabidir. Örneğin yazılım lisansları ücrete tabi değildir.

## <a name="provisioning-states"></a>Hüküm veren devletler

Sağlama durumu, VM'de kullanıcı tarafından başlatılan, denetim düzlemi işleminin durumudur. Bu durumlar Bir VM'nin güç durumundan ayrıdır.

- **Oluşturma** – VM oluşturma.

- **Güncelleştirme** – varolan bir VM için modeli güncelleştirir. Başlat/Yeniden Başlatma gibi VM'de model olmayan bazı değişiklikler de güncelleştirme kapsamına girer.

- **Delete** – VM silme.

- **Deallocate** – VM'nin durdurulup ana bilgisayardan kaldırıldığı yerdir. VM'nin ayrılması güncelleştirme olarak kabul edilir, bu nedenle güncelleştirmeyle ilgili hüküm veren durumları görüntüler.



Platform kullanıcı tarafından başlatılan bir eylemi kabul ettikten sonra geçiş işlemi durumları şunlardır:

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
<p><b>Güncelleştirme</b></p>
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
<p><b>Silme</b></p>
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
<p>Özel bir görüntüyle değil de işletim sistemi görüntüsüyle bir VM oluşturulursa, aşağıdaki alt durumlar gözlemlenebilir:</p>
<p>1. <b>OSProvisioning</b> &ndash; Devam Ediyor VM çalışıyor ve konuk işletim sistemi kurulumu devam ediyor. <p /> 
<code> "statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating/OSProvisioningInprogress",<br>
 "level": "Info",<br>
 "displayStatus": "OS Provisioning In progress"<br>
 }<br>
]</code><br>
<p>2. <b>OSProvisioningKomple</b> &ndash; Kısa ömürlü durum. Herhangi bir uzantı yüklenmesi gerekmedikçe VM hızla **Başarı'ya** geçiş eder. Uzantıları yüklemek zaman alabilir. <br />
<code> "statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating/OSProvisioningComplete",<br>
 "level": "Info",<br>
 "displayStatus": "OS Provisioning Complete"<br>
 }<br>
]</code><br>
<p><b>Not</b>: İşletim sistemi hatası varsa veya işletim sistemi zamanında yüklenmezse Işletim Sistemi Sağlama **Başarısız'a** geçiş yapabilir. Müşteriler, altyapıda dağıtılan VM için faturalandırılır.</p>
</td>
</tr>
</table>


İşlem tamamlandıktan sonra, VM aşağıdaki durumlardan birine geçecektir:

- **Başarılı** – kullanıcı tarafından başlatılan eylemler tamamlandı.

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

 

- **Başarısız** – başarısız bir işlemi temsil eder. Daha fazla bilgi ve olası çözümler almak için hata kodlarına bakın.

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

Örnek görünümü API VM çalışma durumu bilgileri sağlar. Daha fazla bilgi için [Sanal Makineler - Örnek Görünüm](https://docs.microsoft.com/rest/api/compute/virtualmachines/instanceview) API belgelerine bakın.

Azure Kaynakları gezgini, VM çalışan durumunu görüntülemek için basit bir kullanıcı arabirimi sağlar: [Kaynak Gezgini.](https://resources.azure.com/)

Sağlama durumları VM özellikleri ve örnek görünümünde görülebilir. Güç durumları VM örneğingörünümünde kullanılabilir. 

