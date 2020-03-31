---
title: Azure Marketi Görselleri için Güvenlik Önerileri | Microsoft Dokümanlar
description: Bu makalede, pazar yerinde yer alan görüntüler için öneriler sağlar
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
ms.openlocfilehash: b82cf957f4bd74cb2c63bfd5a7fe73899b395df6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73795821"
---
# <a name="security-recommendations-for-azure-marketplace-images"></a>Azure Marketi Görselleri için Güvenlik Önerileri

Resminiz bu güvenlik yapılandırma önerilerini karşılamalıdır. Bu, Azure Marketi'nde iş ortağı çözüm görüntüleri için yüksek düzeyde güvenlik sağlanmasına yardımcı olur.

Göndermeden önce her zaman resminizde bir güvenlik açığı algılaması çalıştırın. Kendi yayınlanan resminizde bir güvenlik açığı algılarsanız, müşterilerinizi hem güvenlik açığı hem de nasıl düzeltecekleriniz konusunda zamanında bilgilendirmeniz gerekir.

## <a name="open-source-based-images"></a>Kaynak Tabanlı Görselleri Aç

|||
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Kategori**                                                 | **İşaretli**                                                                                                                                                                                                                                                                              |
| Güvenlik                                                     | Linux dağıtımı için en son güvenlik yamaları yükleyin.                                                                                                                                                                                                              |
| Güvenlik                                                     | Belirli Linux dağıtımı için VM imajını güvence altına almak için endüstri yönergelerine uyun.                                                                                                                                                                                     |
| Güvenlik                                                     | Yalnızca gerekli Windows Server rolleri, özellikleri, hizmetleri ve ağ bağlantı noktalarıyla en az ayak izini koruyarak saldırı yüzeyini sınırlandırın.                                                                                                                                               |
| Güvenlik                                                     | Kaynak kodunu ve ortaya çıkan Kötü amaçlı yazılım vm görüntüsünü tarayın.                                                                                                                                                                                                                                   |
| Güvenlik                                                     | VHD görüntüsü yalnızca etkileşimli girişe izin verecek varsayılan parolaları olmayan gerekli kilitli hesapları içerir; Arka kapı yok.                                                                                                                                           |
| Güvenlik                                                     | Uygulama güvenlik duvarı cihazı gibi işlevsel olarak bunlara dayanmadığı sürece güvenlik duvarı kurallarını devre dışı bırakın.                                                                                                                                                                             |
| Güvenlik                                                     | Test SSH anahtarları, bilinen ana bilgisayar ları dosyası, günlük dosyaları ve gereksiz sertifikalar gibi VHD görüntüdeki tüm hassas bilgileri kaldırın.                                                                                                                                       |
| Güvenlik                                                     | LVM kullanmaktan kaçının.                                                                                                                                                                                                                                            |
| Güvenlik                                                     | Gerekli kitaplıkların en son sürümlerini ekleyin: </br> - OpenSSL v1.0 veya üzeri </br> - Python 2.5 veya üzeri (Python 2.6+ şiddetle tavsiye edilir) </br> - Python pyasn1 paketi zaten yüklü değilse </br> - d.OpenSSL v 1.0 veya üzeri                                                                |
| Güvenlik                                                     | Bash/Shell geçmiş girişlerini temizleyin.                                                                                                                                                                                                                                             |
| Ağ Oluşturma                                                   | Varsayılan olarak SSH sunucusunu ekleyin. Aşağıdaki seçenek: ClientAliveInterval 180 ile sshd config için SSH canlı tutmak ayarlayın.                                                                                                                                                        |
| Ağ Oluşturma                                                   | Görüntüden herhangi bir özel ağ yapılandırmasını kaldırın. resolv.conf silin: `rm /etc/resolv.conf`.                                                                                                                                                                                |
| Dağıtım                                                   | En son Azure Linux Aracısını yükleyin.</br> - RPM veya Deb paketini kullanarak yükleyin.  </br> - Manuel yükleme işlemini de kullanabilirsiniz, ancak yükleyici paketleri önerilir ve tercih edilir. </br> - Aracıyı GitHub deposundan el ile yüklüyorsanız, `waagent` önce `/usr/sbin` dosyayı kopyalayın ve çalıştırın (root olarak): </br>`# chmod 755 /usr/sbin/waagent` </br>`# /usr/sbin/waagent -install` </br>Aracı yapılandırma dosyası `/etc/waagent.conf`. |
| Dağıtım                                                   | Azure Desteğinin iş ortaklarımıza gerektiğinde seri konsol çıktısı sağlayabilmesini ve bulut depolamadan işletim sistemi disk montajı için yeterli zaman dilimi sağlamasını sağlayın. Görüntü Çekirdek Önyükleme Hattı için aşağıdaki `console=ttyS0 earlyprintk=ttyS0 rootdelay=300`parametreleri ekleyin: . |
| Dağıtım                                                   | Os diskinde takas bölümü yok. Takas Linux Agent tarafından yerel kaynak diskinde oluşturulması için istenebilir.         |
| Dağıtım                                                   | İşletim sistemi diski için tek bir kök bölümü oluşturun.      |
| Dağıtım                                                   | Yalnızca 64 bit işletim sistemi.                                                                                                                                                                                                                                                          |

## <a name="windows-server-based-images"></a>Windows Server tabanlı görüntüler

|||
|-------------| -------------------------|
| **Kategori**                                                     | **İşaretli**                                                                                                                                                                |
| Güvenlik                                                         | Güvenli bir işletim sistemi temel görüntüsü kullanın. Windows Server'ı temel alan herhangi bir görüntünün kaynağı için kullanılan VHD, Microsoft Azure aracılığıyla sağlanan Windows Server Işletim Sistemi görüntülerinden olmalıdır. |
| Güvenlik                                                         | En son güvenlik güncelleştirmelerini yükleyin.                                                                                                                                     |
| Güvenlik                                                         | Uygulamalar yönetici, kök veya yönetici gibi kısıtlanmış kullanıcı adlarına bağlı olmamalıdır.                                                                |
| Güvenlik                                                         | Hem işletim sistemi sabit diskleri hem de veri sabit diskleri için BitLocker Sürücü Şifrelemesini etkinleştirin.                                                             |
| Güvenlik                                                         | Yalnızca gerekli Windows Server rolleri, özellikleri, hizmetleri ve ağ bağlantı noktalarını etkin bir şekilde en az ayak izine sahip tutarak saldırı yüzeyini sınırlandırın.                         |
| Güvenlik                                                         | Kaynak kodunu ve ortaya çıkan Kötü amaçlı yazılım vm görüntüsünü tarayın.                                                                                                                     |
| Güvenlik                                                         | Windows Server görüntüleri güvenlik güncelleştirmesini otomatik güncelleştirmeye ayarlayın.                                                                                                                |
| Güvenlik                                                         | VHD görüntüsü yalnızca etkileşimli girişe izin verecek varsayılan parolaları olmayan gerekli kilitli hesapları içerir; Arka kapı yok.                             |
| Güvenlik                                                         | Uygulama güvenlik duvarı cihazı gibi işlevsel olarak bunlara dayanmadığı sürece güvenlik duvarı kurallarını devre dışı bırakın.                                                               |
| Güvenlik                                                         | HOSTS dosyaları, günlük dosyaları ve gereksiz sertifikalar da dahil olmak üzere VHD görüntüdeki tüm hassas bilgileri kaldırın.                                              |
| Dağıtım                                                       | Yalnızca 64 bit işletim sistemi.                            |

Kuruluşunuzun Azure pazarda görüntüleri olmasa bile, Windows ve Linux görüntü yapılandırmalarınızı bu önerilere karşı kontrol etmeyi düşünün.

## <a name="contacting-customers"></a>Müşterilerle iletişim kurma

Müşterileri ve iletişim e-postalarını belirlemek için:

1.  Bulut İş Ortağı Portalı'nda, sol **rayda, Insights'ı**seçin.
2.  Siparişler **ve Kullanım** sekmesinde, gerekli tarih aralığındaki kullanımı sorgulamak için **Başlangıç Tarihi** ve **Bitiş Tarihi** alanlarını kullanın. Bu, teklif için günlük olarak hangi Azure aboneliklerinin kullanıldığını gösterir. Bu verileri dışa aktarın. 
3.  Benzer şekilde, **Müşteri** sekmesinde, müşteri tabanınızı sorgula ve dışa aktarın.
4.  Gerekli müşteri bilgilerini bulmak için Abonelik Kimliğini adım 2'den 3.
