---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 08/03/2020
ms.author: alkohli
ms.openlocfilehash: caa17a02305c9e9fcba30cf8c39c4288d83d3da9
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89419425"
---
VM oluşturma sırasında geçirilen özel IP 'yi kullanarak VM 'ye bağlanın.

IP adresiyle bağlantı kurmak için bir SSH oturumu açın.

`ssh -l <username> <ip address>`

İstendiğinde, VM oluştururken kullandığınız parolayı belirtin.

SSH anahtarını sağlamanız gerekiyorsa, bu komutu kullanın.

`ssh -i c:/users/Administrator/.ssh/id_rsa Administrator@5.5.41.236`

VM 'ye bağlanırken bir örnek çıktı aşağıda verilmiştir:

```powershell
PS C:\07-30-2020\linux> ssh -l Administrator 10.126.68.186
Administrator@10.126.68.186's password:
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 5.0.0-1027-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Jul 30 22:56:11 UTC 2020

  System load:  0.0               Processes:           105
  Usage of /:   5.6% of 28.90GB   Users logged in:     0
  Memory usage: 12%               IP address for eth0: 10.126.68.186
  Swap usage:   0%

 * Are you ready for Kubernetes 1.19? It's nearly here! Try RC3 with
   sudo snap install microk8s --channel=1.19/candidate --classic

   https://www.microk8s.io/ has docs and details.

68 packages can be updated.
0 updates are security updates.


*** System restart required ***

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

Administrator@mylinuxvm:
```

VM oluşturma sırasında genel bir IP adresi kullandıysanız, bu IP 'yi sanal makineye bağlanmak için kullanabilirsiniz. Genel IP 'yi almak için: 

```powershell
$publicIp = Get-AzureRmPublicIpAddress -Name <Public IP> -ResourceGroupName <Resource group name>
```
Bu durumda genel IP, sanal ağ arabirimi oluşturma sırasında geçirilen özel IP ile aynı olacaktır.