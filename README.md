# Auto Mail

Membuat Pengiriman Email otomatis

## Note

- Buat file .env untuk menyimpan email dan password akun

  EMAIL_USER=akunPribadi@gmail.com

  EMAIL_PASS=xlrxxvugxmzxyxtx

- Password (xlrx xvug xmzx yxtx) ini di buat di google (https://myaccount.google.com/apppasswords), fitur ini akan terbuka kalau sudah melakukan verifikasi 2 langkah (2fa)

  Nanti akan disuruh buat nama aplikasi, kemudian dia akan kasih 16 digit kode tersebut.
  Ketika dimasukan ke variabel EMAIL_PASS jangan di kasih spasi / rapatkan semua karakter.

- 2fa ini ada 4 proses
  - Nomor Telepon
  - Authenticator, ini download google authenticator di playstore terus scan qr code yang di kasih.
  - Dialog Google, ini auto terisi kalau sudah lakukan authenticator
  - Kunci Sandi dan Kunci Keamanan, ini gak bisa dilakukan di laptop/pc, pakai hp aja.

## Source Code

```bash
    import smtplib
    from email.mime.text import MIMEText
    from email.mime.multipart import MIMEMultipart
    from dotenv import load_dotenv
    import os

    # Load variabel dari .env
    load_dotenv()

    EMAIL_USER = os.getenv("EMAIL_USER")
    EMAIL_PASS = os.getenv("EMAIL_PASS")

    # Daftar penerima
    recipients = [
        'akunPenerima1@gmail.com',
        'akunPenerima2@gmail.com',
        'akunPenerima3@gmail.com'
    ]

    # Isi email
    subject = 'Pesan Internal: Pengujian Sistem Email Otomatis'
    body = """
    Kepada Yth. Bapak/Ibu,

    Kami ingin menginformasikan bahwa sistem otomatisasi email internal saat ini telah berhasil dikonfigurasi.
    Email ini dikirim sebagai bagian dari pengujian sistem dan tidak memerlukan tindakan lebih lanjut dari Anda.

    Jika Anda menerima email ini dengan baik di kotak masuk utama, berarti sistem berjalan dengan semestinya.

    Apabila terdapat pertanyaan lebih lanjut atau kendala teknis, silakan hubungi tim IT kami.

    Hormat kami,
    Divisi IT Internal
    PT. Contoh Perusahaan
    """

    try:
        server = smtplib.SMTP('smtp.gmail.com', 587)
        server.starttls()
        server.login(EMAIL_USER, EMAIL_PASS)

        for recipient in recipients:
            # Buat email baru setiap loop
            msg = MIMEMultipart()
            msg['From'] = EMAIL_USER
            msg['To'] = recipient
            msg['Subject'] = subject
            msg.add_header('Reply-To', EMAIL_USER)
            msg.add_header('X-Mailer', 'Python SMTP')
            msg.attach(MIMEText(body, 'plain'))

            server.sendmail(EMAIL_USER, recipient, msg.as_string())
            print(f"✅ Email berhasil dikirim ke {recipient}")

        server.quit()

    except Exception as e:
        print("❌ Gagal mengirim email:", e)

```
