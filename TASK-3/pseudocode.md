Roller

  Hasta: Kayıt olur, giriş yapar, doktor arar, uygun saat seçer, randevu alır/günceller/iptal eder, check-in yapar.
  
  Doktor: Çalışma takvimini yönetir, randevu durumlarını güncelleyebilir.
  
  Sekreter: Randevuları onaylar/iptal eder, acil durum randevusu atar, bekleme listesini yönetir.
  
  Yönetici: Sistem ayarları, kullanıcı yetkileri ve raporlama.

Temel Veri Nesneleri

  Hasta: kimlik (TC), ad-soyad, iletişim, şifre özeti, sigorta bilgisi, durum.
  
  Doktor: ad-soyad, branş, poliklinik, günlük çalışma planı, durum.
  
  Poliklinik: ad, konum, durum.
  
  Randevu: hasta, doktor, poliklinik, tarih-saat, durum (rezerve, onaylı, iptal, tamamlandı, gecikti, no-show), oluşturulma zamanı, not.
  
  Bekleme Listesi: doktor, tarih-saat, hasta, öncelik skoru, kayıt zamanı.
  
  Bildirim: hedef kullanıcı, kanal (SMS/e-posta), mesaj, gönderim zamanı, durum.
  
  Oturum: token, kullanıcı, rol, geçerlilik.

İş Kuralları ve Sınırlar

  Randevu süre dilimi varsayılan 15 dakikadır.
  
  Randevu iptali, randevu saatinden 2 saat öncesine kadar hasta tarafından yapılabilir.
  
  Geç check-in eşiği randevu saatinden 10 dakika öncesidir (uyarı üretir).
  
  No-show, randevu saatinden 30 dakika sonra gelinmemişse işaretlenir.
  
  Hasta için günlük randevu limiti (örnek) 2 adettir.
  
  Çakışmaları engellemek için aynı doktor+tarih+saat üzerinde kilit/atomik işlem uygulanır.

Ana İşlevler (Özet Akışlar)

  1. Kayıt Olma: TC ve e-posta doğrulanır, şifre güvenli şekilde özetlenir, hasta kaydı oluşturulur.
  
  2. Giriş Yapma: Kimlik ve şifre kontrol edilir, rol bilgili oturum üretilir (token).
  
  3. Doktor Arama: Branş, poliklinik, tarih aralığı veya ad-soyad kriterlerine göre listeleme yapılır.
  
  4. Müsaitlik Sorgulama: Doktorun çalışma planından 15 dakikalık slotlar hesaplanır, dolu slotlar düşülür, boş slotlar sunulur.
  
  5. Randevu Oluşturma:

    Geçmiş zamana randevu engellenir.
    
    Hastanın günlük limitine bakılır.
    
    Slot uygun değilse hata verilir.
    
    Uygunsa “rezerv” durumunda randevu kaydedilir ve hastaya bildirim gönderilir.

6. Randevu Onaylama: Sekreter (veya kural bazlı otomasyon) randevuyu “onaylı” yapar; hastaya bildirilir.

7. Randevu İptali:
  
  Hasta iptali: süre kuralı uygunsa yapılır.
  
  Sekreter iptali: gerekçeyle yapılabilir.
  
  İptalde bekleme listesi tetiklenir (boşalan slota aday çağrısı).

8. Randevu Güncelleme (Tarih/Saat Değişimi):
  
  Eski slot boşaltılmadan önce yeni slot uygunluğu kontrol edilir.
  
  Uygunsa randevu taşınır ve eski slot için bekleme listesi tetiklenir.
  
9. Hasta Check-in: Hasta randevuya geldiğini bildirir. Geç check-in uyarısı üretilebilir. Durum muayeneye hazır olacak şekilde güncellenir.

10. No-Show Kontrolü: Zamanı geçen onaylı randevular otomatik “no-show” yapılır ve hastaya bilgilendirme gider.

11. Bekleme Listesi Yönetimi:

  Hasta uygun slot için bekleme listesine alınır, öncelik (aciliyet, engel, yaş vb.) ile sıralanır.
  
  Slot boşalınca en yüksek öncelikli adaya 15 dakikalık onay penceresi gönderilir; onay gelirse slot atanır.

12. Hatırlatıcılar:

  Randevudan 24 saat önce ve 2 saat önce otomatik SMS/e-posta hatırlatmaları gönderilir.

13. Doktor Takvim Yönetimi: Doktor gün/slot ekler veya kaldırır. Dolu slot varsa kaldırma engellenir (önce taşı/iptal).

14. Acil Durum Randevusu: Sekreter, kurallara göre mevcut hastayı uygun şekilde kaydırıp acil hastaya öncelik tanıyabilir.

15. (Opsiyonel) Ödeme: Ücretli randevularda tahsilat akışı çalıştırılır; başarılı ise kayıt altına alınır.

Güvenlik ve Denetim

  Her işlemde rol-bazlı yetki kontrolü uygulanır.
  
  Hassas veriler (şifre özeti, token) güvenli saklanır.
  
  Giriş denemeleri için oran sınırlama uygulanır.
  
  Tüm kritik olaylar denetim kaydına (log) yazılır.

Tipik Hasta Yolculuğu

  Kayıt ol veya giriş yap.
  
  Branş/poliklinik/tarih ile doktor ara.
  
  Müsait slotları gör ve saat seç.
  
  Randevu oluştur; onay ve hatırlatıcıları al.
  
  Randevu günü check-in yap ve muayene ol.
  
  Randevu “tamamlandı” durumuna çekilir.
  
  Gerekirse kurallara uygun şekilde iptal veya yeniden planlama yap.

Raporlama (Yönetici)
  
  Günlük/haftalık/aylık randevu sayıları (branş/poliklinik/doktor bazlı).
  
  No-show oranı ve dönemsel eğilimler.
  
  Bekleme listesinin kullanım ve dönüşüm oranı.
  
  Doluluk oranı ve ortalama bekleme süresi.
