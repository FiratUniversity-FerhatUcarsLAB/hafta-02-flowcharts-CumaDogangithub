1. Başla — ATM hazır mı diye kontrol et.

2. Ekrana “Kartınızı takınız / Takınız” mesajı göster.

3. Kart takılmasını bekle.
3.1. Eğer kart takılmazsa → zaman aşımı (ör. 30s) → işlem iptal, ana ekrana dön.

4. Kart algılanınca kart bilgilerini (chip/manyetik) oku.
4.1. Eğer kart okunamazsa → hata mesajı, kart iade etme veya tutma kararı (hata sayısı > N ise kartı tut).

5. Kullanıcıdan dil seçimini al (opsiyonel).

6. Kullanıcıdan işlem türünü sor → “Para Çekme” seçeneğini bekle.

7. Kullanıcı para çekme seçeneğini seçtiyse:
7.1. Ekrana “Çekmek istediğiniz tutarı girin” göster.
7.2. Kullanıcı tutar girişi yapar (veya ön tanımlı seçeneklerden birine basar).

8. Girilen tutarı doğrula:
8.1. Tutar pozitif mi ve ATM’nin verebileceği not kombinasyonuna uygun mu? (örn. 10/20/50 birim)
8.2. Günlük/hesap limitlerini kontrol et (yerel limit, banka limiti).
8.3. Eğer geçersiz ise → uygun hata mesajı → yeniden giriş iste veya iptal.

9. Ekrana “PIN giriniz” mesajı göster.

10. PIN girişini bekle (gizli giriş, maksimum deneme sayısı M).
10.1. Her bir başarısız denemede deneme sayacını arttır.
10.2. Eğer deneme sayısı > M → kartı bloke et veya tut → güvenlik mesajı göster → işlem iptal.

11. PIN alındıktan sonra:
11.1. Banka/işlem sunucusuna doğrulama isteği gönder: {kart_id, PAN, PIN, islem_tutar, terminal_id, zaman}
11.2. Sunucudan cevap bekle (timeout süresi).

12. Sunucu cevapları:
12.1. Onay (Auth OK) → ilerle.
12.2. Red (yetersiz bakiye, limit, çalıntı kart, vs.) → kullanıcıya uygun hata mesajı göster → işlem iptal veya alternatif (daha az tutar gir).
12.3. Ağ/Timeout/Hata → tekrar dene (k-adet) veya offline mod kontrolleri (eğer destekleniyorsa) → eğer başarısızsa işlem iptal.

13. Onay geldiyse:
13.1. Banka hesabından geçici rezervasyon/bloklama (hold) oluştur (tutar kadar).
13.2. ATM kasa bilgisinden nakit uygunluğunu kontrol et (her banknot türünden yeterli var mı).
13.3. Eğer kasa yetersizse → kullanıcıya “kasa yetersiz” mesajı → rezervasyonu iptal et → işlem iptal.

14. Not dağılımı hesapla (en az banknot sayısı / kullanılabilir stok).

15. Notları fiziksel olarak verme işlemini başlat:
15.1. Her bir banknotu verme döngüsü:
15.1.1. Mekanizma banknotu hazırla ve ilet.
15.1.2. Banknot doğru şekilde verildi mi sensörle doğrula.
15.1.3. Eğer sensör hata verirse → tekrar dene (k-adet) → başarısızsa işlemi durdur, iade planla, kasa hatası bildir.

16. Tüm banknotlar başarılı şekilde verildiyse:
16.1. Bankaya kesin onay gönder (settle/capture) — rezervasyonu tamamla.
16.2. Hesap bakiyesini güncelle (bankaca yapılır).

17. Makbuz/fiş seçeneği sun:
17.1. Kullanıcı isterse fişi yazdır ve/veya dijital seçenek (SMS/e-mail) sun.

18. Kartı iade et:
18.1. Kartı güvenli şekilde geri ver.
18.2. Eğer kart kullanıcı almazsa → belirli süre sonra kartı tut ve güvenlik prosedürünü çalıştır.

19. Ekrana başarı mesajı göster (tutar, bakiye özeti opsiyonel).

20. İşlem kayıtlarını sakla (lokal log ve merkez sunucuya gönder):
20.1. Kaydet: işlem_id, kart_maskesi, terminal_id, tutar, zaman, sonuç kodu, hata kodları (varsa).

21. Sonlandır — ana ekrana dön.

22. Hata ve istisna akışları (özete dair):
22.1. Yetersiz bakiye → kullanıcıya bildir → yeniden tutar girme şansı veya iptal.
22.2. Ağ hatası → tekrar deneme veya işlem iptal (kullanıcı bilgilendir).
22.3. Donanım hatası (kasa, sensör) → işlem iptal, logla, bakım bildir.
22.4. Kart şifresi yanlış üst üste M kez → kart bloke/tut, güvenlik bildirimi, işlem iptal.
22.5. Olağandışı işlem / fraud şüphesi → ek doğrulama ister veya işlemi reddeder.

23. Güvenlik önlemleri:
23.1. PIN asla düz metin kaydedilmez/loglanmaz.
23.2. Ağ trafiği şifreli kanaldan (TLS) gider.
23.3. Kritik veriler maskelenir (PAN sadece son 4 hane gösterilir).
23.4. Zaman aşımı ve otomatik temizleme (ekran gizleme).
23.5. İşlem sonrası bellek temizlenir (PIN vb).

24. Ek kurallar / limitler:
24.1. Günlük çekim limiti kontrolü.
24.2. Maksimum tek çekim limiti kontrolü.
24.3. Para çekme ücreti ve bildirim (varsa).

25. Kapanış: işlem tamamlandıysa veya iptal olduysa operasyona göre kullanıcıyı bilgilendir ve oturumu sonlandır.
