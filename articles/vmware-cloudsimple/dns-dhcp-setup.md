---
title: CloudSimple tarafından Azure VMware Çözümü - Özel Bulut için iş yükü DNS ve DHCP'yi ayarlama
description: CloudSimple Private Cloud ortamınızda çalışan uygulamalar ve iş yükleri için DNS ve DHCP'nin nasıl ayarlanır olduğunu açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ef1266b783034cf18dc2b3ea4be5ebc01bc12c70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024696"
---
# <a name="set-up-dns-and-dhcp-applications-and-workloads-in-your-cloudsimple-private-cloud"></a>CloudSimple Private Cloud'unuzda DNS ve DHCP uygulamalarını ve iş yüklerini ayarlama

Özel Bulut ortamında çalışan uygulamalar ve iş yükleri, arama ve IP adresi ataması için ad çözümlemesi ve DHCP hizmetleri gerektirir.  Bu hizmetleri sağlamak için uygun bir DHCP ve DNS altyapısı gereklidir.  Bu hizmetleri Özel Bulut ortamınızda sağlamak için sanal bir makine yapılandırabilirsiniz.  

## <a name="prerequisites"></a>Ön koşullar

* VLAN yapılandırılan dağıtılmış bir bağlantı noktası grubu
* Kurulumu şirket içi veya Internet tabanlı DNS sunucularına yönlendirme
* Sanal makine şablonu veya ISO sanal bir makine oluşturmak için

## <a name="linux-based-dns-server-setup"></a>Linux tabanlı DNS sunucu kurulumu

Linux, DNS sunucuları kurmak için çeşitli paketler sunar.  Burada bir açık kaynak BIND DNS sunucusu kurmak için talimatlar ile DigitalOcean bir [örnek kurulum.](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-ubuntu-18-04)

## <a name="windows-based-setup"></a>Windows tabanlı kurulum

Bu Microsoft konuları, Bir Windows sunucusunun DNS sunucusu ve DHCP sunucusu olarak nasıl ayarlanır olduğunu açıklar.

* [DNS Server olarak Windows Server](https://docs.microsoft.com/windows-server/networking/dns/dns-top)
* [DHCP Server olarak Windows Server](https://docs.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-top)
