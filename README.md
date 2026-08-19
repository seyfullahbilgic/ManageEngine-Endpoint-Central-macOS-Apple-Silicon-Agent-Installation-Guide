ManageEngine Endpoint Central - macOS (Apple Silicon) Agent Installation Guide
Bu doküman, Apple Silicon (M1, M2, M3) işlemcili Mac cihazların, Intel tabanlı ajan üreten eski sürüm ManageEngine Endpoint Central sunucularına eklenmesi sırasında karşılaşılan "Bad CPU type in executable" hatasının çözümünü ve kurulum adımlarını içerir.

Sorun Analizi
Eski sürüm Endpoint Central sunucuları, sadece Intel (x86_64) mimarisine uygun ajan paketleri oluşturur. Yeni nesil Apple Silicon işlemciler (arm64), bu paketleri doğrudan çalıştıramaz ve aşağıdaki hataları verir:

Terminal'de: Bad CPU type in executable

Portalda: Ajan kurulmuş görünmesine rağmen cihazın listeye düşmemesi.

Çözüm: Rosetta 2 Kurulumu
macOS'in Intel uygulamalarını Apple Silicon üzerinde çalıştırmasını sağlayan çeviri katmanı olan Rosetta 2'nin kurulması gerekmektedir.

1. Adım: Rosetta 2'yi Manuel Kurun
Terminal'i açın ve aşağıdaki komutu çalıştırın:

/usr/sbin/softwareupdate --install-rosetta --agree-to-license
Not: Eğer "Not available" hatası alırsanız, cihazın internet bağlantısını ve Apple sunucularına erişimini kontrol edin.

2. Adım: Kurulumu Doğrulayın
Rosetta'nın başarıyla kurulup kurulmadığını test etmek için:

arch -x86_64 /usr/bin/true
Eğer komut hata vermeden boş satıra geçiyorsa Rosetta hazırdır.

Ajan Kurulum Adımları
1. Hazırlık
Endpoint Central portalından Mac ajan .zip paketini indirin.

Paketi klasöre çıkartın (İçerisinde UEMS_MacAgent.pkg ve serverinfo.plist dosyalarının yan yana olduğundan emin olun).

2. Yükleme
.pkg dosyasına sağ tıklayıp Aç (Open) diyerek kurulumu tamamlayın.

3. Servisi Başlatma ve Sunucuyla İletişim
Ajanın sunucuya hemen rapor vermesini sağlamak için aşağıdaki komutu kullanın:

sudo /Library/ManageEngine/UEMS_Agent/bin/dcagenttrayicon.app/Contents/MacOS/dcagenttrayicon contact
macOS Güvenlik İzinleri (Önemli)
Kurulum sonrası ajanın tam yetkiyle çalışabilmesi için şu izinlerin verilmesi şarttır:

Tam Disk Erişimi (Full Disk Access):

Sistem Ayarları > Gizlilik ve Güvenlik > Tam Disk Erişimi yoluna gidin.

listedeki UEMS Agent (veya Remote Control/Desktop Central) öğelerini aktif edin.

Arka Plan İzinleri:

Sistem Ayarları > Genel > Oturum Açma Öğeleri altında ManageEngine servislerine izin verildiğinden emin olun.

Önemli Notlar
Bu yöntem geçici bir çözümdür. En sağlıklı yöntem, Endpoint Central sunucusunu Build 11.2 veya üzerine güncelleyerek "Universal Agent" (Native ARM64 desteği) kullanmaktır.

Sunucu güncellendiğinde Rosetta kurulumuna gerek kalmadan ajanlar doğrudan çalışacaktır.
