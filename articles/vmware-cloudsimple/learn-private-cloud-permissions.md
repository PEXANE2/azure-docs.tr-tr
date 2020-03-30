---
title: CloudSimple tarafından Azure VMware Çözümü - Özel Bulut izin modeli
description: CloudSimple Private Cloud izin modelini, gruplarını ve kategorilerini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 28c4dc7831f97d66eb4d47f08e640344d5cca0d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77014955"
---
# <a name="cloudsimple-private-cloud-permission-model-of-vmware-vcenter"></a>VMware vCenter CloudSimple Özel Bulut izin modeli

CloudSimple, Özel Bulut ortamına tam yönetim erişimi sağlar. Her CloudSimple müşterisine, sanal makineleri kendi ortamlarında dağıtabilmek ve yönetebilmek için yeterli yönetim ayrıcalıkları verilir.  Gerekirse, yönetim işlevlerini gerçekleştirmek için ayrıcalıklarınızı geçici olarak artırabilirsiniz.

## <a name="cloud-owner"></a>Bulut Sahibi

Bir Özel Bulut oluşturduğunuzda, vCenter Tek Oturum Açma etki alanında bir **CloudOwner** kullanıcısı oluşturulur ve **Cloud-Owner-Role** erişimi özel buluttaki nesneleri yönetir. Bu kullanıcı ayrıca Özel Bulut vCenter için ek [vCenter Kimlik Kaynakları](set-vcenter-identity.md)ve diğer kullanıcılar ayarlayabilirsiniz.

> [!NOTE]
> CloudSimple Private Cloud vCenter'ınız cloudowner@cloudsimple.local için varsayılan kullanıcı, Bir Özel Bulut oluşturulduğunda olur.

## <a name="user-groups"></a>Kullanıcı Grupları

Özel Bulut'un dağıtımı sırasında **Bulut-Sahibi Grubu** adlı bir grup oluşturulur. Bu gruptaki kullanıcılar özel bulutta vSphere ortamının çeşitli bölümlerini yönetebilir. Bu gruba otomatik olarak **Bulut Sahibi-Rolü** ayrıcalıkları verilir ve **CloudOwner** kullanıcısı bu grubun bir üyesi olarak eklenir.  CloudSimple, yönetim kolaylığı için sınırlı ayrıcalıklara sahip ek gruplar oluşturur.  Bu önceden oluşturulmuş gruplara herhangi bir kullanıcı ekleyebilirsiniz ve aşağıda tanımlanan ayrıcalıklar gruplardaki kullanıcılara otomatik olarak atanır.

### <a name="pre-created-groups"></a>Önceden Oluşturulmuş Gruplar

| Grup Adı | Amaç | Rol |
| -------- | ------- | ------ |
| Bulut Sahibi-Grubu | Bu grubun üyeleri, Private Cloud vCenter için yönetim ayrıcalıklarına sahiptir | [Bulut Sahibi-Rolü](#cloud-owner-role) |
| Bulut-Genel Küme-Yönetici Grubu | Bu grubun üyeleri, Private Cloud vCenter Cluster'da yönetim ayrıcalıklarına sahiptir | [Bulut-Küme-Yönetici Rolü](#cloud-cluster-admin-role) |
| Bulut-Genel Depolama-Yönetici Grubu | Bu grubun üyeleri, Özel Bulut vCenter'daki depolamayı yönetebilir | [Bulut-Depolama-Yönetici Rolü](#cloud-storage-admin-role) |
| Bulut-Global-Ağ-Yönetici-Grup | Bu grubun üyeleri, Özel Bulut vCenter'da ağ ve dağıtılmış bağlantı noktası gruplarını yönetebilir | [Bulut-Ağ-Yönetici Rolü](#cloud-network-admin-role) |
| Bulut-Global-VM-Yönetici Grubu | Bu grubun üyeleri Özel Bulut vCenter'da sanal makineleri yönetebilir | [Cloud-VM-Yönetici Rolü](#cloud-vm-admin-role) |

Bireysel kullanıcılara Özel Bulut'u yönetme izni vermek için, uygun gruplara ek kullanıcı hesapları oluşturun.

> [!CAUTION]
> Yeni kullanıcılar yalnızca *Cloud-Owner-Group*, *Cloud-Global-Cluster-Admin-Group*, *Cloud-Global-Storage-Admin-Group,* *Cloud-Global-Network-Admin-Group* veya *Cloud-Global-VM-Admin-Group'a*eklenmelidir.  *Yöneticiler* grubuna eklenen kullanıcılar otomatik olarak kaldırılır.  *Yöneticiler* grubuna yalnızca hizmet hesapları eklenmelidir ve hizmet hesapları vSphere web UI'da oturum açmak için kullanılmamalıdır.

## <a name="list-of-vcenter-privileges-for-default-roles"></a>Varsayılan roller için vCenter ayrıcalıkları listesi

### <a name="cloud-owner-role"></a>Bulut Sahibi-Rolü

| **Kategori** | **Privilege** |
|----------|-----------|
| **Alarmlar** | Alarmı kabul et <br> Alarm oluşturma <br> Alarm eylemini devre dışı <br> Alarmı değiştirme <br> Alarmı kaldırma <br> Alarm durumunu ayarlama |
| **Izin** | İzni değiştirme |
| **İçerik Kitaplığı** | Kitaplık öğesi ekleme <br> Yerel kitaplık oluşturma <br> Abone kitaplığı oluşturma <br> Kitaplık öğesi silme <br> Yerel kitaplığı silme <br> Abone olan kitaplığı silme <br> Dosyaları indirme <br> Tahliye kitaplığı öğesi <br> Tahliye abone kitaplığı <br> İthalat depolama <br> Sonda abonelik bilgileri <br> Depolamayı okuma <br> Kitaplık öğeyi eşitle <br> Abone kitaplığını eşitle <br> Tür içgözlem <br> Yapılandırma ayarlarını güncelleştirme <br> Dosyaları güncelleştirme <br> Kitaplığı güncelleştir <br> Kitaplık öğeyi güncelleştirme <br> Yerel kitaplığı güncelleştirme <br> Abone kitaplığını güncelleştirme <br> Yapılandırma ayarlarını görüntüleme |
| **Şifreleme işlemleri** | Disk ekleme <br> Kopyalama <br> Şifre Çözme <br> Doğrudan Erişim <br> Şifreleme <br> Yeni şifreleme <br> KMS'yi yönet <br> Şifreleme ilkelerini yönetme <br> Anahtarları yönetme <br> Geçiş <br> Yeniden şifreleme <br> Kayıt VM <br> Kayıt ana bilgisayar |
| **dvPort grubu** | Oluşturma <br> Sil <br> Değiştir <br> İlke işlemi <br> Kapsam çalışması |
| **Datastore** | Alan ayırma <br> Datastore'a göz atın <br> Veri deposunun yapılandırılması <br> Düşük düzeyli dosya işlemleri <br> Veri deposunun taşınması <br> Veri deposunun kaldırma <br> Dosyayı kaldırma <br> Datastore'yi yeniden adlandır <br> Sanal makine dosyalarını güncelleştirme <br> Sanal makine meta verilerini güncelleştirme |
| **ESX Acente Müdürü** | Config <br> Değiştir <br> Görünüm |
| **Dahili numara** | Kayıt uzantısı <br> Kayıt dışı uzantısı <br> Uzantıyı güncelleştir |
| **Dış istatistik sağlayıcısı**| Kaydettir <br> Kaydı Kaldır <br> Güncelleştirme |
| **Klasör** | Klasör oluşturma <br> Klasörü silme <br> Klasörü taşıma <br> Klasörü yeniden adlandır |
| **Genel** | Görevi iptal etme <br> Kapasite planlaması <br> Tanılama <br> Yöntemleri devre dışı <br> Yöntemleri etkinleştirme <br> Genel etiket <br> Durum <br> Lisanslar <br> Günlük olayı <br> Özel öznitelikleri yönetme <br> Ara sunucu <br> Komut dosyası eylemi <br> Servis yöneticileri <br> Özel özniteliği ayarlama <br> Sistem etiketi |
| **Sistem durumu güncelleştirme sağlayıcısı** | Kaydettir <br> Kaydı Kaldır <br> Güncelleştirme |
| **Ana Bilgisayar > Yapılandırması** | Depolama bölümü yapılandırması |
| **Ev Sahibi > Envanteri** | Kümeyi değiştirme |
| **vSphere Etiketleme** | vSphere Etiketini Atama veya Ata <br> vSphere Etiketi Oluşturma <br> vSphere Etiket Kategorisi Oluştur <br> vSphere Etiketini Silme <br> vSphere Etiket Kategorisi Silme <br> vSphere Etiketini Edit <br> vSphere Etiket Kategorilerini Edit <br> Kategori için alana Göre Kullanılanı Değiştir <br> Etiket için alan tarafından kullanılanları değiştirin |
| **Ağ** | Ağ atama <br> Yapılandırma <br> Ağı taşıma <br> Kaldır |
| **Performans** | Aralıkları değiştirme |
| **Ev sahibi profili** | Görünüm |
| **Kaynak** | Öneri uygulayın <br> Kaynak havuzuna vApp atama <br> Kaynak havuzuna sanal makine atama <br> Kaynak havuzu oluşturma <br> Sanal makineden güç alan geçirin <br> Sanal makinede güçyle çalışan geçir <br> Kaynak havuzunda değişiklik <br> Kaynak havuzutaşıma <br> vMotion sorgula <br> Kaynak havuzunun kaldırılması <br> Kaynak havuzunun yeniden adlandırı |
| **Zamanlanmış görev** | Görev oluşturma <br> Görevi değiştirme <br> Görevi kaldırma <br> Görev çalıştırma |
| **Oturumlar** | Kullanıcının kimliğine bürünme <br> İleti <br> Oturumu doğrula <br> Oturumları görüntüleme ve durdurma |
| **Datastore kümesi** | Veri deposu kümesini yapılandırma |
| **Profil odaklı depolama** | Profil tabanlı depolama güncelleştirmesi <br> Profil odaklı depolama görünümü |
| **Depolama görünümleri** | Hizmeti yapılandırma <br> Görünüm |
| **Görevler** | Görev oluşturma <br> Görevi güncelleştirme |
| **Transfer hizmeti**| Yönetme <br> İzleme |
| **vApp** | Sanal makine ekleme <br> Kaynak havuzu atama <br> VApp'i Ata <br> Kopyalama <br> Oluşturma <br> Sil <br> Dışarı Aktarma <br> İçeri Aktarma <br> Taşı <br> Kapatma <br> Güç üzerinde <br> Yeniden Adlandır <br> Askıya Alma <br> Kaydı Kaldır <br> OVF ortamını görüntüleyin <br> vApp uygulama yapılandırması <br> vApp örnek yapılandırma <br> vApp managedBy yapılandırma <br> vApp kaynak yapılandırması |
| **VRMPolicy** | VRMPolicy sorgula <br> VRMPolicy'yi güncelleştir |
| **Sanal makine > Yapılandırma** | Varolan disk ekleme <br> Yeni disk ekleme <br> Aygıt ekleme veya kaldırma <br> Gelişmiş <br> CPU sayısını değiştirme <br> Kaynağı değiştirme <br> ManagedBy'yi yapılandır <br> Disk değiştirme izleme <br> Disk kiralama <br> Bağlantı ayarlarını görüntüleme <br> Sanal diski genişletme <br> Ana bilgisayar USB aygıtı <br> Bellek <br> Aygıt ayarlarını değiştirme <br> Sorgu Hata Toleransı uyumluluğu <br> Sahip olunmamış dosyaları sorgula <br> Ham cihaz <br> Yoldan yeniden yükleme <br> Diski kaldırma <br> Yeniden Adlandır <br> Konuk bilgilerini sıfırlama <br> Ek açıklama ayarlama <br> Ayarlar <br> Dosya yerleşimini değiştirme <br> Toggle fork üst <br> Sanal makinenin kilidini açın <br> Sanal makine uyumluluğunu yükseltme |
| **Sanal makine > Konuk işlemleri** | Konuk işlem takma adı değişikliği <br> Konuk işlem takma adı sorgusu <br> Konuk operasyon modifikasyonları <br> Konuk operasyon programı yürütme <br> Konuk işlem sorguları |
| **Sanal makine > Etkileşim** | Soruyu yanıtla <br> Sanal makinede yedekleme işlemi <br> CD ortamını yapılandırma <br> Disket ortamını yapılandırma <br> Konsol etkileşimi <br> Ekran görüntüsü oluşturma <br> Tüm diskleri parçala <br> Cihaz bağlantısı <br> Sürükle ve bırak <br> VIX API tarafından konuk işletim sistemi yönetimi <br> USB HID talimi kodlarını enjekte edin <br> Duraklatma veya Duraklatma <br> Silme veya küçültme işlemlerini gerçekleştirin <br> Kapatma <br> Güç üzerinde <br> Sanal makinede kayıt oturumu <br> Sanal makinede yeniden oynatma oturumu <br> Sıfırla <br> Özgeçmiş Hata Toleransı <br> Askıya Alma <br> Hata Toleransı Askıya Alma <br> Yük devretme testi <br> Test yeniden başlatma İkincil VM <br> Hata Toleransı kapat <br> Hata Toleransı'nı açın <br> VMware Araçları yüklemek |
| **Sanal makine > Envanter** | Varolan dan oluşturma <br> Yeni oluştur <br> Taşı <br> Kaydettir <br> Kaldır <br> Kaydı Kaldır |
| **Sanal makine > Sağlama** | Disk erişimine izin verme <br> Dosya erişimine izin verme <br> Salt okunur disk erişimine izin verme <br> Sanal makine indirmeye izin ver <br> Sanal makine dosyalarıyüklemeizin <br> Klon şablonu <br> Klon sanal makine <br> Sanal makineden şablon oluşturma <br> Özelleştirme <br> Şablon dağıtma <br> Şablon olarak işaretle <br> Sanal makine olarak işaretle <br> Özelleştirme belirtimini değiştirme <br> Diskleri tanıtın <br> Özelleştirme belirtimlerini okuyun |
| **Sanal makine > Servis yapılandırması** | Bildirimlere izin ver <br> Genel etkinlik bildirimlerinin yoklanmasına izin verme <br> Hizmet yapılandırmalarını yönetme <br> Hizmet yapılandırması değiştirme <br> Hizmet yapılandırmalarını sorgula <br> Hizmet yapılandırmasını okuma |
| **Sanal makine > Snapshot yönetimi** | Anlık görüntü oluşturma <br> Anlık görüntü kaldırma <br> Anlık görüntüyeniden adlandırma <br> Anlık görüntüye geri dön |
| **Sanal makine > vSphere Replication** | Çoğaltmayı yapılandırma <br> Çoğaltmayı yönetme <br> Çoğaltmayı izleme |
| **vService** | Bağımlılık oluşturma <br> Bağımlılığı yok et <br> Bağımlılık yapılandırması yeniden yapılandırma <br> Bağımlılığı güncelleştirme |

### <a name="cloud-cluster-admin-role"></a>Bulut-Küme-Yönetici Rolü

| **Kategori** | **Privilege** |
|----------|-----------|
| **Datastore** | Alan ayırma <br> Datastore'a göz atın <br> Veri deposunun yapılandırılması <br> Düşük düzeyli dosya işlemleri <br> Veri deposunun kaldırma <br> Datastore'yi yeniden adlandır <br> Sanal makine dosyalarını güncelleştirme <br> Sanal makine meta verilerini güncelleştirme |
| **Klasör** | Klasör oluşturma <br> Klasörü silme <br> Klasörü taşıma <br> Klasörü yeniden adlandır |
| **Ana Bilgisayar > Yapılandırması**  | Depolama bölümü yapılandırması |
| **vSphere Etiketleme** | vSphere Etiketini Atama veya Ata <br> vSphere Etiketi Oluşturma <br> vSphere Etiket Kategorisi Oluştur <br> vSphere Etiketini Silme <br> vSphere Etiket Kategorisi Silme <br> vSphere Etiketini Edit <br> vSphere Etiket Kategorilerini Edit <br> Kategori için alana Göre Kullanılanı Değiştir <br> Etiket için alan tarafından kullanılanları değiştirin |
| **Ağ** | Ağ atama |
| **Kaynak** | Öneri uygulayın <br> Kaynak havuzuna vApp atama <br> Kaynak havuzuna sanal makine atama <br> Kaynak havuzu oluşturma <br> Sanal makineden güç alan geçirin <br> Sanal makinede güçyle çalışan geçir <br> Kaynak havuzunda değişiklik <br> Kaynak havuzutaşıma <br> vMotion sorgula <br> Kaynak havuzunun kaldırılması <br> Kaynak havuzunun yeniden adlandırı |
| **vApp** | Sanal makine ekleme <br> Kaynak havuzu atama <br> VApp'i Ata <br> Kopyalama <br> Oluşturma <br> Sil <br> Dışarı Aktarma <br> İçeri Aktarma <br> Taşı <br> Kapatma <br> Güç üzerinde <br> Yeniden Adlandır <br> Askıya Alma <br> Kaydı Kaldır <br> OVF ortamını görüntüleyin <br> vApp uygulama yapılandırması <br> vApp örnek yapılandırma <br> vApp managedBy yapılandırma <br> vApp kaynak yapılandırması |
| **VRMPolicy** | VRMPolicy sorgula <br> VRMPolicy'yi güncelleştir |
| **Sanal makine > Yapılandırma** | Varolan disk ekleme <br> Yeni disk ekleme <br> Aygıt ekleme veya kaldırma <br> Gelişmiş <br> CPU sayısını değiştirme <br> Kaynağı değiştirme <br> ManagedBy'yi yapılandır <br> Disk değiştirme izleme <br> Disk kiralama <br> Bağlantı ayarlarını görüntüleme <br> Sanal diski genişletme <br> Ana bilgisayar USB aygıtı <br> Bellek <br> Aygıt ayarlarını değiştirme <br> Sorgu Hata Toleransı uyumluluğu <br> Sahip olunmamış dosyaları sorgula <br> Ham cihaz <br> Yoldan yeniden yükleme <br> Diski kaldırma <br> Yeniden Adlandır <br> Konuk bilgilerini sıfırlama <br> Ek açıklama ayarlama <br> Ayarlar <br> Dosya yerleşimini değiştirme <br> Toggle fork üst <br> Sanal makinenin kilidini açın <br> Sanal makine uyumluluğunu yükseltme |
| **Sanal makine > Konuk işlemleri** | Konuk işlem takma adı değişikliği <br> Konuk işlem takma adı sorgusu <br> Konuk operasyon modifikasyonları <br> Konuk operasyon programı yürütme <br> Konuk işlem sorguları |
| **Sanal makine > Etkileşim** | Soruyu yanıtla <br> Sanal makinede yedekleme işlemi <br> CD ortamını yapılandırma <br> Disket ortamını yapılandırma <br> Konsol etkileşimi <br> Ekran görüntüsü oluşturma <br> Tüm diskleri parçala <br> Cihaz bağlantısı <br> Sürükle ve bırak <br> VIX API tarafından konuk işletim sistemi yönetimi <br> USB HID talimi kodlarını enjekte edin <br> Duraklatma veya Duraklatma <br> Silme veya küçültme işlemlerini gerçekleştirin <br> Kapatma <br> Güç üzerinde <br> Sanal makinede kayıt oturumu <br> Sanal makinede yeniden oynatma oturumu <br> Sıfırla <br> Özgeçmiş Hata Toleransı <br> Askıya Alma <br> Hata Toleransı Askıya Alma <br> Yük devretme testi <br> Test yeniden başlatma İkincil VM <br> Hata Toleransı kapat <br> Hata Toleransı'nı açın <br> VMware Araçları yüklemek
| **Sanal makine > Envanter** | Varolan dan oluşturma <br> Yeni oluştur <br> Taşı <br> Kaydettir <br> Kaldır <br> Kaydı Kaldır |
| **Sanal makine > Sağlama** | Disk erişimine izin verme <br> Dosya erişimine izin verme <br> Salt okunur disk erişimine izin verme <br> Sanal makine indirmeye izin ver <br> Sanal makine dosyalarıyüklemeizin <br> Klon şablonu <br> Klon sanal makine <br> Sanal makineden şablon oluşturma <br> Özelleştirme <br> Şablon dağıtma <br> Şablon olarak işaretle <br> Sanal makine olarak işaretle <br> Özelleştirme belirtimini değiştirme <br> Diskleri tanıtın  <br> Özelleştirme belirtimlerini okuyun |
| **Sanal makine > Servis yapılandırması** | Bildirimlere izin ver <br> Genel etkinlik bildirimlerinin yoklanmasına izin verme <br> Hizmet yapılandırmalarını yönetme <br> Hizmet yapılandırması değiştirme <br> Hizmet yapılandırmalarını sorgula <br> Hizmet yapılandırmasını okuma
| **Sanal makine > Snapshot yönetimi** | Anlık görüntü oluşturma <br> Anlık görüntü kaldırma <br> Anlık görüntüyeniden adlandırma <br> Anlık görüntüye geri dön |
| **Sanal makine > vSphere Replication** | Çoğaltmayı yapılandırma <br> Çoğaltmayı yönetme <br> Çoğaltmayı izleme |
| **vService** | Bağımlılık oluşturma <br> Bağımlılığı yok et <br> Bağımlılık yapılandırması yeniden yapılandırma <br> Bağımlılığı güncelleştirme |

### <a name="cloud-storage-admin-role"></a>Bulut-Depolama-Yönetici Rolü

| **Kategori** | **Privilege** |
|----------|-----------|
| **Datastore** | Alan ayırma <br> Datastore'a göz atın <br> Veri deposunun yapılandırılması <br> Düşük düzeyli dosya işlemleri <br> Veri deposunun kaldırma <br> Datastore'yi yeniden adlandır <br> Sanal makine dosyalarını güncelleştirme <br> Sanal makine meta verilerini güncelleştirme |
| **Ana Bilgisayar > Yapılandırması** | Depolama bölümü yapılandırması |
| **Datastore kümesi** | Veri deposu kümesini yapılandırma |
| **Profil odaklı depolama** | Profil tabanlı depolama güncelleştirmesi <br> Profil odaklı depolama görünümü |
| **Depolama görünümleri** | Hizmeti yapılandırma <br> Görünüm |

### <a name="cloud-network-admin-role"></a>Bulut-Ağ-Yönetici Rolü

| **Kategori** | **Privilege** |
|----------|-----------|
| **dvPort grubu** | Oluşturma <br> Sil <br> Değiştir <br> İlke işlemi <br> Kapsam çalışması |
| **Ağ** | Ağ atama <br> Yapılandırma <br> Ağı taşıma <br> Kaldır |
| **Sanal makine > Yapılandırma** | Aygıt ayarlarını değiştirme |

### <a name="cloud-vm-admin-role"></a>Cloud-VM-Yönetici Rolü

| **Kategori** | **Privilege** |
|----------|-----------|
| **Datastore** | Alan ayırma <br> Datastore'a göz atın |
| **Ağ** | Ağ atama |
| **Kaynak** | Kaynak havuzuna sanal makine atama <br> Sanal makineden güç alan geçirin <br> Sanal makinede güçyle çalışan geçir
| **vApp** | Dışarı Aktarma <br> İçeri Aktarma |
| **Sanal makine > Yapılandırma** | Varolan disk ekleme <br> Yeni disk ekleme <br> Aygıt ekleme veya kaldırma <br> Gelişmiş <br> CPU sayısını değiştirme <br> Kaynağı değiştirme <br> ManagedBy'yi yapılandır <br> Disk değiştirme izleme <br> Disk kiralama <br> Bağlantı ayarlarını görüntüleme <br> Sanal diski genişletme <br> Ana bilgisayar USB aygıtı <br> Bellek <br> Aygıt ayarlarını değiştirme <br> Sorgu Hata Toleransı uyumluluğu <br> Sahip olunmamış dosyaları sorgula <br> Ham cihaz <br> Yoldan yeniden yükleme <br> Diski kaldırma <br> Yeniden Adlandır <br> Konuk bilgilerini sıfırlama <br> Ek açıklama ayarlama <br> Ayarlar <br> Dosya yerleşimini değiştirme <br> Toggle fork üst <br> Sanal makinenin kilidini açın <br> Sanal makine uyumluluğunu yükseltme |
| **Sanal makine >Konuk işlemleri** | Konuk işlem takma adı değişikliği <br> Konuk işlem takma adı sorgusu <br> Konuk operasyon modifikasyonları <br> Konuk operasyon programı yürütme <br> Konuk işlem sorguları    |
| **Sanal makine >Etkileşim** | Soruyu yanıtla <br> Sanal makinede yedekleme işlemi <br> CD ortamını yapılandırma <br> Disket ortamını yapılandırma <br> Konsol etkileşimi <br> Ekran görüntüsü oluşturma <br> Tüm diskleri parçala <br> Cihaz bağlantısı <br> Sürükle ve bırak <br> VIX API tarafından konuk işletim sistemi yönetimi <br> USB HID talimi kodlarını enjekte edin <br> Duraklatma veya Duraklatma <br> Silme veya küçültme işlemlerini gerçekleştirin <br> Kapatma <br> Güç üzerinde <br> Sanal makinede kayıt oturumu <br> Sanal makinede yeniden oynatma oturumu <br> Sıfırla <br> Özgeçmiş Hata Toleransı <br> Askıya Alma <br> Hata Toleransı Askıya Alma <br> Yük devretme testi <br> Test yeniden başlatma İkincil VM <br> Hata Toleransı kapat <br> Hata Toleransı'nı açın <br> VMware Araçları yüklemek |
| **Sanal makine >Envanteri** | Varolan dan oluşturma <br> Yeni oluştur <br> Taşı <br> Kaydettir <br> Kaldır <br> Kaydı Kaldır |
| **Sanal makine >Sağlama** | Disk erişimine izin verme <br> Dosya erişimine izin verme <br> Salt okunur disk erişimine izin verme <br> Sanal makine indirmeye izin ver <br> Sanal makine dosyalarıyüklemeizin <br> Klon şablonu <br> Klon sanal makine <br> Sanal makineden şablon oluşturma <br> Özelleştirme <br> Şablon dağıtma <br> Şablon olarak işaretle <br> Sanal makine olarak işaretle <br> Özelleştirme belirtimini değiştirme <br> Diskleri tanıtın <br> Özelleştirme belirtimlerini okuyun |
| **Sanal makine >Servis yapılandırması** | Bildirimlere izin ver <br> Genel etkinlik bildirimlerinin yoklanmasına izin verme <br> Hizmet yapılandırmalarını yönetme <br> Hizmet yapılandırması değiştirme <br> Hizmet yapılandırmalarını sorgula <br> Hizmet yapılandırmasını okuma
| **Sanal makine >Snapshot yönetimi** | Anlık görüntü oluşturma <br> Anlık görüntü kaldırma <br> Anlık görüntüyeniden adlandırma <br> Anlık görüntüye geri dön |
| **Sanal makine >vSphere Çoğaltma** | Çoğaltmayı yapılandırma <br> Çoğaltmayı yönetme <br> Çoğaltmayı izleme |
| **vService** | Bağımlılık oluşturma <br> Bağımlılığı yok et <br> Bağımlılık yapılandırması yeniden yapılandırma <br> Bağımlılığı güncelleştirme |
