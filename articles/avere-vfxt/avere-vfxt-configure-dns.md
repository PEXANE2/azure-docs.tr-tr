---
title: Avere vFXT DNS - Azure
description: Azure için Avere vFXT ile yuvarlak-robin yük dengeleme için bir DNS sunucusu yapılandırma
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: rohogue
ms.openlocfilehash: 81b53904f85e2ac936195b1e39d7586fd1d47524
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76153794"
---
# <a name="avere-cluster-dns-configuration"></a>Avere kümesi DNS yapılandırması

Bu bölümde, Avere vFXT kümenizi dengelemek için bir DNS sistemi yapılandırmatemelleri açıklanmaktadır.

Bu belge, Azure ortamında bir DNS sunucusu ayarlama ve yönetme *yönergelerini içermez.*

Azure'da bir vFXT kümesini yüklemek için round-robin DNS kullanmak yerine, monte edildiklerinde istemciler arasında IP adreslerini eşit olarak atamak için manuel yöntemler kullanmayı düşünün. [Avere Dağı kümesinde](avere-vfxt-mount-clients.md)çeşitli yöntemler tanımlanmıştır.

Bir DNS sunucusu kullanıp kullanmamaya karar verirken bunları aklınızda bulundurun:

* Sisteminize yalnızca NFS istemcileri tarafından erişilirse, DNS kullanmak gerekli değildir - sayısal IP adreslerini kullanarak tüm ağ adreslerini belirtmek mümkündür.

* Etkin Dizin sunucusu için bir DNS etki alanı belirtmeniz gerektiğinden, sisteminiz SMB (CIFS) erişimini destekliyorsa, DNS gereklidir.

* Kerberos kimlik doğrulamasını kullanmak istiyorsanız DNS gereklidir.

## <a name="load-balancing"></a>Yük dengeleme

Genel yükü dağıtmak için, DNS etki alanınızı istemciye bakan IP adresleri için round-robin yük dağılımını kullanacak şekilde yapılandırın.

## <a name="configuration-details"></a>Yapılandırma ayrıntıları

İstemciler kümeye eriştığında, RRDNS isteklerini kullanılabilir tüm arabirimler arasında otomatik olarak dengeler.

En iyi performans için, DNS sunucunuzu aşağıdaki diyagramda gösterildiği gibi istemciye bakan küme adreslerini işletecek şekilde yapılandırın.

Bir küme vserver solda gösterilir ve IP adresleri ortada ve sağda görünür. Her istemci erişim noktasını gösterildiği gibi A kayıtları ve işaretçileri ile yapılandırın.

![Avere küme round-robin DNS diyagramı](media/avere-vfxt-rrdns-diagram.png)
<!--- separate text description file provided  [diagram text description](avere-vfxt-rrdns-alt-text.md) -->

İstemcilere bakan her IP adresi, küme tarafından iç kullanım için benzersiz bir ada sahip olmalıdır. (Bu diyagramda, istemci IP'ler netlik için vs1-client-IP-* olarak adlandırılır, ancak üretimde muhtemelen istemci gibi daha kısa bir şey kullanmalısınız*.)

İstemciler, sunucu bağımsız değişkeni olarak vserver adını kullanarak kümeye monte edilir.

Vserver'ınızdaki sorgular ``named.conf`` için döngüsel sıra ayarlamak için DNS sunucunuzun dosyasını değiştirin. Bu seçenek, kullanılabilir değerlerin tümünün geçişini sağlar. Aşağıdaki gibi bir deyim ekleyin:

```
options {
    rrset-order {
        class IN A name "vserver1.example.com" order cyclic;
    };
};
```

Aşağıdaki ``nsupdate`` komutlar DNS'yi doğru yapılandırmaya bir örnek sağlar:

```
update add vserver1.example.com. 86400 A 10.0.0.10
update add vserver1.example.com. 86400 A 10.0.0.11
update add vserver1.example.com. 86400 A 10.0.0.12
update add vs1-client-IP-10.example.com. 86400 A 10.0.0.10
update add vs1-client-IP-11.example.com. 86400 A 10.0.0.11
update add vs1-client-IP-12.example.com. 86400 A 10.0.0.12
update add 10.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-10.example.com
update add 11.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-11.example.com
update add 12.0.0.10.in-addr.arpa. 86400 PTR vs1-client-IP-12.example.com
```

## <a name="cluster-dns-settings"></a>DNS ayarlarını kümele

vFXT kümesinin **Küme** > **Yönetim Ağı** ayarları sayfasında kullandığı DNS sunucusunu belirtin. Bu sayfadaki ayarlar şunlardır:

* DNS sunucu adresi
* DNS etki alanı adı
* DNS arama etki alanları

Bu sayfayı kullanma hakkında daha fazla bilgi için Avere Cluster Yapılandırma Kılavuzu'ndaki [DNS Ayarlarını](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_admin_network.html#gui-dns>) okuyun.
