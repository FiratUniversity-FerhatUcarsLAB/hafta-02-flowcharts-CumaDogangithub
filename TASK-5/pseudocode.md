1. BAŞLAT

  sistem açılır
  
  tarih–saat senkronize edilir
  
  kullanıcı profilleri, PIN/biometrik, acil kişi listesi yüklenir
  
  sensör listesi yüklenir:
  kapı/pencere manyetik, hareket (PIR), iç/dış kameralar, duman/CO, su kaçağı, cam kırılma, akıllı kilit, sabotaj (kutu açılma), enerji ve ağ izleme
  
  varsayılan mod belirlenir: “Evde”

2. MOD YÖNETİMİ

  mevcut mod ∈ {Evde, Uzakta (Dışarıda), Uyku, Tatil, Panik}
  
  moda göre aktif sensör setleri ve eylem eşikleri atanır:
  • Evde: iç kameralar kapalı/maskele, hareket sadece dış bölgede aktif
  • Uzakta/Tatil: tüm çevre + iç hareket aktif, giriş/çıkış gecikmesi açık
  • Uyku: alt kat–çevre aktif, üst kat hareket toleranslı
  • Panik: tüm alarmlar anında tetiklenir, gecikmeler yok

3. SÜREKLİ İZLEME DÖNGÜSÜ
  
  döngü başı: sistem sağlığı kontrolü (pil seviyesi, sensör bağlantısı, ağ durumu)
  
  her sensörden en son değerleri oku
  
  yeni olay var mı?
  • yoksa: otomasyonları ve zamanlayıcıları çalıştır, döngü başına dön
  • varsa: olayı işle

4. OLAY İŞLEME (GENEL AKIŞ)

  olay türünü belirle: güvenlik, güvenlik-dışı (konfor), arıza/sabotaj
  
  yanlış alarm azaltma filtresi uygula:
  • çift tetik mantığı (aynı bölge kısa sürede 2 farklı sensör)
  • minimum süre eşiği (ör. >1 sn hareket)
  • evcil hayvan filtresi (PIR alt bölge duyarlılığı)
  • kamera ile hızlı doğrulama (insan/araç nesne algı sinyali)
  
  olaya öncelik ata:
  • Kritik: duman/CO, sabotaj, cam kırılma, zorla giriş
  • Yüksek: kapı/pencere ihlali, ardışık hareket + kamera doğrulaması
  • Orta: tekil hareket, beklenmeyen kilit açma
  • Düşük: titreşim/tek kısa tetik, konfor olayları
  
  mod kurallarını uygula ve eylem kararı ver

5. EYLEM KARAR AĞACI

  eğer olay Kritik:
  • sireni anında çalıştır
  • tüm ışıkları acil durum senaryosuna göre yak-söndür (dışarı caydırıcı)
  • akıllı kilitleri kilitle, panjurları güvenli konuma çek
  • anında bildirim: push + SMS + acil kişi araması
  • duman/CO ise havalandırma aç, enerji kaynağını gerekli bölgede kes
  • kamera kaydını olay öncesi-sonrası tamponla birlikte sakla
  
  eğer olay Yüksek:
  • giriş/çıkış gecikmesi süresi doldu mu?
  – dolduysa siren + bildirim + ışık senaryosu
  – dolmadıysa geri sayım ve kullanıcı doğrulaması bekle
  • kamera canlı görüntüsünü kullanıcıya gönder
  
  eğer olay Orta:
  • sessiz bildirim gönder (kullanıcı onayı ister)
  • aynı bölgeden tekrar tetik olursa Yüksek’e yükselt
  
  eğer olay Düşük:
  • yalnızca logla; tekrar sayısı eşik aşarsa Orta’ya yükselt

6. KULLANICI ETKİLEŞİMİ VE DOĞRULAMA

  mobil bildirim: olay özeti + tek dokunuşla mod değiştirme
  
  iki aşamalı doğrulama: PIN/biometrik veya güvenli sözcük
  
  yanlış alarm bildirimi halinde: olay kapatılır, sensör o bölgede kısa süreliğine “izleme ama alarm yok” moduna alınır

7. KUR/ÇÖZ (ARM/DISARM) MANTIĞI

  kurma koşulları: tüm kritik kapı/pencere kapalı, açık kalan varsa kullanıcıya listele
  
  geofencing: tüm kullanıcılar evden ayrıldıysa otomatik “Uzakta” modu öner
  
  kurulduktan sonra giriş gecikmesi başlat: ana kapıdan girişte geri sayım sesli bildirilir
  
  çözme: PIN/biometrik, NFC etiketi veya güvenilir telefon yakınlık doğrulaması

8. OTOMASYON SENARYOLARI

  Uzakta moduna geçince: tüm ışıkları kapat, prizleri kes (kritik olmayan), güvenlik kameralarını tam aktif et
  
  Uyku modunda: dış çevre ışıkları düşük parlaklıkta açık, iç kameralar kapalı
  
  algılanan dış hareket + Uzakta: bahçe ışıkları kısa süre aç, caydırıcı anons oynat
  
  su kaçağı: ana su vanasını kapat, kullanıcıyı uyar

9. ARIZA VE SABOTAJ YÖNETİMİ

  sensör pil zayıf: düşük öncelik uyarı, 24 saat içinde tekrar hatırlat
  
  cihaz çevrimdışı: tekrar bağlanmayı dene; 5 dk içinde olmazsa uyar
  
  sabotaj (kutu açma, sinyal bozucu şüphesi): anında siren + yüksek öncelik uyarı + yerel kayıt
  
  elektrik kesintisi: UPS’e geç; ağ yoksa hücresel yedek üzerinden bildirim; minimum yaşam modu

10. KAYIT VE GİZLİLİK

  tüm olaylar zaman damgası ile loglanır (sensör, süre, eylem, kullanıcı müdahalesi)
  
  video/ses kayıtları yalnızca güvenlik modlarında ve olay penceresinde tutulur
  
  Evde modunda iç kameralar otomatik maskeleme veya kapalı; yalnızca manuel istekte açılır

11. PERİYODİK BAKIM VE ÖĞRENME

  günlük: sensör yoklama testi ve rapor
  
  haftalık: yanlış alarm istatistiği → eşik/duyarlılık otomatik ayarlama önerisi
  
  aylık: acil kişi listesi ve tahliye planı doğrulaması
  
  model öğrenmesi: tekrar eden yanlış tetikler için kural güncelleme öner

12. İLETİŞİM VE YEDEKLEME
  
  birincil kanal: ev interneti; yedek: hücresel
  
  bildirimler: push > SMS > otomatik arama sırası
  
  kritik olay kaydı, buluta ve yerel NVR’a aynı anda yazılır

13. BİTİR KOŞULLARI

  kullanıcı sistemi “Bakım” moduna alırsa alarmlar pasif, izleme ve log aktif
  
  sistem kapatılacaksa: son durumlar yedeklenir, güvenli şekilde kapanır
