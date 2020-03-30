---
title: Çalışan kapsayıcı örneğinde komutları yürütme
description: Şu anda Azure Kapsayıcı Örnekleri'nde çalışan bir kapsayıcıda komutu nasıl yürütteceklerini öğrenin
ms.topic: article
ms.date: 03/30/2018
ms.openlocfilehash: de48e6ac246e2b0751561b4c60bb63d88b599bdf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247208"
---
# <a name="execute-a-command-in-a-running-azure-container-instance"></a>Çalışan bir Azure kapsayıcı örneğinde komut uygulama

Azure Container Instances, çalışmakta olan bir kapsayıcıda komut yürütülmesini destekler. Zaten başlattığınız bir kapsayıcıda komut yürütmek, özellikle de uygulama geliştirme ve sorun giderme sırasında kullanışlıdır. Bu özelliğin en yaygın kullanım örneği, çalışan bir kapsayıcıdaki sorunlarla ilgili hata ayıklama gerçekleştirmek amacıyla etkileşimli bir kabuk başlatmaktır.

## <a name="run-a-command-with-azure-cli"></a>Azure CLI ile komut çalıştırma

[Azure CLI'de][azure-cli] [az kapsayıcı yöneticisi][az-container-exec] olan çalışan bir kapsayıcıda komut uygulayın:

```azurecli
az container exec --resource-group <group-name> --name <container-group-name> --exec-command "<command>"
```

Örneğin, bir Nginx konteyner bir Bash kabuk başlatmak için:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
```

Aşağıdaki örnek çıktıda, Bash kabuğu çalışan bir Linux konteynerinde başlatılır `ls` ve bu da yürütülen bir terminal sağlar:

```output
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# exit
exit
Bye.
```

Bu örnekte, Komut İstemi çalışan bir Nanoserver kapsayıcısında başlatılır:

```azurecli
az container exec --resource-group myResourceGroup --name myiis --exec-command "cmd.exe"
```

```output
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\>dir
 Volume in drive C has no label.
 Volume Serial Number is 76E0-C852

 Directory of C:\

03/23/2018  09:13 PM    <DIR>          inetpub
11/20/2016  11:32 AM             1,894 License.txt
03/23/2018  09:13 PM    <DIR>          Program Files
07/16/2016  12:09 PM    <DIR>          Program Files (x86)
03/13/2018  08:50 PM           171,616 ServiceMonitor.exe
03/23/2018  09:13 PM    <DIR>          Users
03/23/2018  09:12 PM    <DIR>          var
03/23/2018  09:22 PM    <DIR>          Windows
               2 File(s)        173,510 bytes
               6 Dir(s)  21,171,609,600 bytes free

C:\>exit
Bye.
```

## <a name="multi-container-groups"></a>Birden çok kapsayıcılı gruplar

Kapsayıcı [grubunuzun](container-instances-container-groups.md) uygulama kapsayıcısı ve günlük kenar arabası gibi birden çok kapsayıcısı varsa, komutu `--container-name`çalıştıracak kapsayıcının adını belirtin.

Örneğin, konteyner grubunda *mynginx* iki kap, *nginx-app* ve *logger*vardır. *Nginx-app* konteyner üzerinde bir kabuk başlatmak için:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --container-name nginx-app --exec-command "/bin/bash"
```

## <a name="restrictions"></a>Kısıtlamalar

Azure Kapsayıcı Örnekleri şu anda [az kapsayıcı yöneticisiyle][az-container-exec]tek bir işlem başlatmayı destekler ve komut bağımsız değişkenlerini geçiremezsiniz. Örneğin, 'deki `sh -c "echo FOO && echo BAR"`gibi komutları zincirleyemez `echo FOO`siniz veya yürütemezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Kapsayıcı Örnekleri'ndeki Sorun Giderme kapsayıcısı ve dağıtım sorunlarındaki](container-instances-troubleshooting.md)diğer sorun giderme araçları ve yaygın dağıtım sorunları hakkında bilgi edinin.

<!-- LINKS - internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[azure-cli]: /cli/azure