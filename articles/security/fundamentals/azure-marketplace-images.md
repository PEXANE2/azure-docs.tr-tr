---
title: Azure Market görüntüleri için güvenlik önerileri | Microsoft Docs
description: Bu makale, Pazar yerinde bulunan görüntüler için öneriler sağlar
services: security
documentationcenter: na
author: barclayn
manager: barbkess
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.date: 01/11/2019
ms.author: barclayn
ms.openlocfilehash: 3d6b1ed2a3800058acba9fe5df2137811afed74a
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300783"
---
# <a name="security-recommendations-for-azure-marketplace-images"></a>Azure Market görüntüleri için güvenlik önerileri

Her bir çözümün aşağıdaki güvenlik yapılandırması önerilerini ile uyumlu olmasını öneririz. Bu, Azure Marketi 'nde iş ortağı çözümü görüntüleri için yüksek düzeyde güvenlik sağlanmasına yardımcı olur.

Bu öneriler Ayrıca, Azure Marketi 'nde görüntü bulunmayan kuruluşlar için de yararlı olabilir. Şirketinizin Windows ve Linux görüntü yapılandırmalarının aşağıdaki tablolarda bulunan yönergelere karşı kontrol etmek isteyebilirsiniz:

## <a name="open-source-based-images"></a>Açık kaynak tabanlı görüntüler

|||
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Kategori**                                                 | **Denetlemez**                                                                                                                                                                                                                                                                              |
| Güvenlik                                                     | Linux dağıtımına yönelik tüm en son güvenlik düzeltme eklerinin yüklü olması.                                                                                                                                                                                                              |
| Güvenlik                                                     | Belirli bir Linux dağıtımına yönelik VM görüntüsünü güvenli hale getirmeye yönelik sektör kuralları izlenir.                                                                                                                                                                                     |
| Güvenlik                                                     | Yalnızca gerekli Windows Server rolleri, özellikleri, hizmetleri ve ağ bağlantı noktalarıyla en az sayıda parmak izi tutarak saldırı yüzeyini sınırlayın.                                                                                                                                               |
| Güvenlik                                                     | Kötü amaçlı yazılım için kaynak kodu ve ortaya çıkan VM görüntüsünü tarayın.                                                                                                                                                                                                                                   |
| Güvenlik                                                     | VHD görüntüsü yalnızca, etkileşimli oturum açmaya izin verecek varsayılan parolalara sahip gerekli kilitli hesapları içerir; arka kapı yok.                                                                                                                                           |
| Güvenlik                                                     | Uygulama işlevsellik bir güvenlik duvarı gereci gibi bunlara dayanmadığı sürece güvenlik duvarı kuralları devre dışıdır.                                                                                                                                                                             |
| Güvenlik                                                     | Tüm hassas bilgiler, test SSH anahtarları, bilinen ana bilgisayar dosyası, günlük dosyaları ve gereksiz sertifikalar gibi VHD görüntüsünden kaldırılmıştır.                                                                                                                                       |
| Güvenlik                                                     | LVM 'nin kullanılması önerilir.                                                                                                                                                                                                                                            |
| Güvenlik                                                     | Gerekli kitaplıkların en son sürümleri eklenmelidir: </br> -OpenSSL v 1.0 veya üzeri </br> -Python 2,5 veya üzeri (Python 2.6 + kesinlikle önerilir) </br> -Zaten yüklenmemişse Python pyasn1 paketi </br> -d. OpenSSL v 1,0 veya üzeri                                                                |
| Güvenlik                                                     | Bash/Shell geçmiş girdileri temizlenmelidir                                                                                                                                                                                                                                             |
| Ağ                                                   | SSH sunucusu varsayılan olarak eklenmelidir. SSH 'yi aşağıdaki seçenekle, SSHD config 'e canlı tut olarak ayarlayın: ClientAliveInterval 180                                                                                                                                                        |
| Ağ                                                   | Görüntü özel ağ yapılandırması içermemelidir. Resolv. conf dosyasını silin:`rm /etc/resolv.conf`                                                                                                                                                                                |
| Dağıtım                                                   | En son Azure Linux aracısının yüklü olması gerekir </br> -Aracının RPM veya Deb paketi kullanılarak yüklenmesi gerekir.  </br> -El ile yükleme işlemini de kullanabilirsiniz, ancak yükleyici paketleri önerilir ve tercih edilir. </br> -Aracıyı GitHub deposundan el ile yüklüyorsanız, önce `waagent` `/usr/sbin` dosyayı kopyalayın ve (kök olarak) çalıştırın: </br>`# chmod 755 /usr/sbin/waagent` </br>`# /usr/sbin/waagent -install` </br>Aracı yapılandırma dosyası konumunda `/etc/waagent.conf`yer alır.    |
| Dağıtım                                                   | Azure desteğinin, ihtiyaç duyduğunda seri konsol çıkışıyla iş ortaklarımızı sağlayabilmesini ve bulut depolamadan işletim sistemi diski bağlama için yeterli zaman aşımı sağlamasına izin verir. Görüntü, çekirdek önyükleme satırına aşağıdaki parametreleri eklemiş olmalıdır:`console=ttyS0 earlyprintk=ttyS0 rootdelay=300` |
| Dağıtım                                                   | İşletim sistemi diski üzerinde takas bölümü yok. Linux Aracısı tarafından yerel kaynak diskinde oluşturma için değiştirme istenebilir.         |
| Dağıtım                                                   | İşletim sistemi diski için tek bir kök bölüm oluşturulması önerilir.      |
| Dağıtım                                                   | yalnızca 64 bitlik işletim sistemi.                                                                                                                                                                                                                                                          |

## <a name="windows-server-based-images"></a>Windows Server tabanlı görüntüler

|||
|-------------| -------------------------|
| **Kategori**                                                     | **Denetlemez**                                                                                                                                                                |
| Güvenlik                                                         | Güvenli bir işletim sistemi temel görüntüsü kullanın. Windows Server tabanlı herhangi bir görüntünün kaynağı için kullanılan VHD, Microsoft Azure ile sunulan Windows Server işletim sistemi görüntülerinden olmalıdır. |
| Güvenlik                                                         | Tüm en son güvenlik güncelleştirmelerini yükler.                                                                                                                                     |
| Güvenlik                                                         | Uygulamalar, yönetici, kök ve yönetici gibi kısıtlı kullanıcı adlarına bağımlılığı içermemelidir.                                                                |
| Güvenlik                                                         | BitLocker Sürücü Şifrelemesi hem işletim sistemi sabit sürücüler hem de veri sabit sürücüleri için etkinleştirilmiştir.                                                             |
| Güvenlik                                                         | Yalnızca gerekli Windows Server rolleri, özellikleri, hizmetleri ve ağ bağlantı noktaları etkin olan en az sayıda kaplama tutarak saldırı yüzeyini sınırlayın.                         |
| Güvenlik                                                         | Kötü amaçlı yazılım için kaynak kodu ve ortaya çıkan VM görüntüsünü tarayın.                                                                                                                     |
| Güvenlik                                                         | Windows Server görüntülerini güvenlik güncelleştirmesini otomatik güncelleştirme olarak ayarlayın.                                                                                                                |
| Güvenlik                                                         | VHD görüntüsü yalnızca, etkileşimli oturum açmaya izin verecek varsayılan parolalara sahip gerekli kilitli hesapları içerir; arka kapı yok.                             |
| Güvenlik                                                         | Uygulama işlevsellik bir güvenlik duvarı gereci gibi bunlara dayanmadığı sürece güvenlik duvarı kuralları devre dışıdır.                                                               |
| Güvenlik                                                         | Tüm hassas bilgiler, VHD görüntüsünden kaldırılmıştır. Örneğin, HOSTS dosyası, günlük dosyaları ve gereksiz sertifikaların kaldırılması gerekir.                                              |
| Dağıtım                                                       | yalnızca 64 bitlik işletim sistemi.                            |
